---
title: Verwenden von Azure Event Grid zum Automatisieren der Größenänderung hochgeladener Bilder | Microsoft Docs
description: Azure Event Grid kann bei Blob-Uploads in Azure Storage ausgelöst werden. Sie können diese Funktion verwenden, um in Azure Storage hochgeladene Bilddateien an andere Dienste (z. B. Azure Functions) für die Größenänderung und andere Optimierungen zu senden.
services: event-grid, functions
author: spelluru
manager: jpconnoc
editor: ''
ms.service: event-grid
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/29/2019
ms.author: spelluru
ms.custom: mvc
ms.openlocfilehash: b3ddaf7667baf98d9d5daa93a3106e457d0aeacb
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55756868"
---
# <a name="tutorial-automate-resizing-uploaded-images-using-event-grid"></a>Tutorial: Automatisieren der Größenänderung von hochgeladenen Bildern mit Event Grid

[Azure Event Grid](overview.md) ist ein Ereignisdienst für die Cloud. Event Grid ermöglicht Ihnen das Erstellen von Abonnements für Ereignisse, die von Azure-Diensten oder Drittanbieterressourcen ausgelöst werden.  

Dieses Tutorial stellt Teil zwei einer Reihe von Tutorials zu Storage dar. Es erweitert das [vorherige Tutorial zu Storage][previous-tutorial], um serverlose automatische Miniaturansichtengenerierung mithilfe von Azure Event Grid und Azure Functions hinzuzufügen. Event Grid ermöglicht, dass [Azure Functions](../azure-functions/functions-overview.md) auf Ereignisse von [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md) reagieren kann und Miniaturansichten hochgeladener Bilder generiert. Ein Ereignisabonnement wird für das Blob Storage-Erstellungsereignis erstellt. Wenn ein Blob einem bestimmten Blob Storage-Container hinzugefügt wird, wird ein Funktionsendpunkt aufgerufen. Aus Event Grid an die Funktionsbindung übergebene Daten werden für den Zugriff auf das Blob und zum Generieren der Miniaturansicht verwendet.

Sie verwenden die Azure-CLI und das Azure-Portal, um die Größenänderungsfunktionalität einer vorhandenen Bildupload-App hinzuzufügen.

![Veröffentlichte Web-App im Microsoft Edge-Browser](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines neuen Azure Storage-Kontos
> * Bereitstellen von serverlosem Code mit Azure Functions
> * Erstellen eines Blob Storage-Ereignisabonnements in Event Grid

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

Sie müssen das vorherige Blob Storage-Tutorial abgeschlossen haben: [Hochladen von Bilddaten in die Cloud mit Azure Storage][previous-tutorial].

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Falls Sie den Event Grid-Ressourcenanbieter noch nicht in Ihrem Abonnement registriert haben, stellen Sie sicher, dass er registriert ist.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.EventGrid
```

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, erfordert dieses Tutorial die Azure CLI-Version 2.0.14 oder höher. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli). 

Falls Sie nicht Cloud Shell verwenden, müssen Sie sich erst mithilfe von `az login` anmelden.

## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

Für Azure Functions ist ein allgemeines Speicherkonto erforderlich. Erstellen Sie ein separates allgemeines Speicherkonto in der Ressourcengruppe, indem Sie den Befehl [az storage account create](/cli/azure/storage/account#az-storage-account-create) verwenden.

Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. 

Ersetzen Sie im folgenden Befehl den Platzhalter `<general_storage_account>` durch Ihren eigenen global eindeutigen Namen für das allgemeine Speicherkonto. 

1. Legen Sie eine Variable fest, die den Namen der Ressourcengruppe enthält, die Sie im vorherigen Tutorial erstellt haben. 

    ```azurecli-interactive
    resourceGroupName=<Name of the resource group that you created in the previous tutorial>
    ```
2. Legen Sie eine Variable für den Namen des Speicherkontos fest, das für die Azure-Funktion erforderlich ist. 

    ```azurecli-interactive
    functionstorage=<name of the storage account to be used by function>
    ```
3. Erstellen Sie das Speicherkonto für die Azure-Funktion. Dies unterscheidet sich von dem Speicher, der die Bilder enthält. 

    ```azurecli-interactive
    az storage account create --name $functionstorage --location eastus --resource-group $resourceGroupName --sku Standard_LRS --kind storage
    ```

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App  

Sie müssen über eine Funktions-App verfügen, die die Ausführung Ihrer Funktion in Azure hostet. Die Funktionen-App bietet eine Umgebung für die serverlose Ausführung des Funktionscodes. Erstellen Sie eine Funktionen-App mithilfe des Befehls [az functionapp create](/cli/azure/functionapp#az-functionapp-create). 

Ersetzen Sie im folgenden Befehl den Platzhalter `<function_app>` durch den eigenen eindeutigen Namen Ihrer Funktions-App. Da der Name der Funktions-App als DNS-Standarddomäne für die Funktions-App verwendet wird, muss der Name für alle Apps in Azure eindeutig sein. Ersetzen Sie `<general_storage_account>` durch den Namen des allgemeinen Speicherkontos, das Sie erstellt haben.

1. Geben Sie einen Namen für die Funktions-App an, die erstellt werden soll. 

    ```azurecli-interactive
    functionapp=<name of the function app>
    ```
2. Erstellen Sie die Azure-Funktion. 

    ```azurecli-interactive
    az functionapp create --name $functionapp --storage-account  $functionstorage --resource-group $resourceGroupName --consumption-plan-location eastus
    ```

Jetzt müssen Sie die Funktions-App für die Verbindung mit dem Blob Storage-Konto konfigurieren,das Sie im [vorhergehenden Tutorial][previous-tutorial] erstellt haben.

## <a name="configure-the-function-app"></a>Konfigurieren der Funktions-App

Die Funktion benötigt die Verbindungszeichenfolge zum Herstellen der Verbindung mit dem Blob Storage-Konto. Der Funktionscode, den Sie im folgenden Schritt in Azure bereitstellen, sucht in der App-Einstellung „myblobstorage_STORAGE“ nach der Verbindungszeichenfolge und in der App-Einstellung „myContainerName“ nach dem Namen des Miniaturansichtcontainers. Rufen Sie die Verbindungszeichenfolge mit dem Befehl [az storage account show-connection-string](/cli/azure/storage/account) ab. Legen Sie Anwendungseinstellungen mit dem Befehl [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) fest.

In den folgenden CLI-Befehlen ist `<blob_storage_account>` der Name des Blob Storage-Kontos, das Sie im vorherigen Tutorial erstellt haben.

1. Rufen Sie die Verbindungszeichenfolge für das Speicherkonto ab, das die Bilder enthält. 

    ```azurecli-interactive
    storageConnectionString=$(az storage account show-connection-string --resource-group $resourceGroupName --name $blobStorageAccount --query connectionString --output tsv)
    ```
2. Konfigurieren Sie die Funktions-App. 

    ```azurecli-interactive
    az functionapp config appsettings set --name $functionapp --resource-group $resourceGroupName --settings AzureWebJobsStorage=$storageConnectionString THUMBNAIL_CONTAINER_NAME=thumbnails THUMBNAIL_WIDTH=100 FUNCTIONS_EXTENSION_VERSION=~2
    ```

    Die Einstellung `FUNCTIONS_EXTENSION_VERSION=~2` legt fest, dass die Funktions-App unter der Version 2.x der Azure Functions-Runtime ausgeführt wird.

Sie können nun ein Funktionscodeprojekt für diese Funktions-App bereitstellen.

## <a name="deploy-the-function-code"></a>Bereitstellen des Funktionscodes 

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)

Das C#-Beispielskript (.csx) für die Größenänderung ist auf [GitHub](https://github.com/Azure-Samples/function-image-upload-resize) verfügbar. Stellen Sie dieses Funktionscodeprojekt für die Funktions-App mithilfe des Befehls [az functionapp deployment source config](/cli/azure/functionapp/deployment/source) bereit. 

Im folgenden Befehl ist `<function_app>` der Name der zuvor erstellten Funktions-App.

```azurecli-interactive
az functionapp deployment source config --name $functionapp --resource-group $resourceGroupName --branch master --manual-integration --repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)
Die Node.js-Beispielfunktion für die Größenänderung ist auf [GitHub](https://github.com/Azure-Samples/storage-blob-resize-function-node) verfügbar. Stellen Sie dieses Funktionscodeprojekt für die Funktions-App mithilfe des Befehls [az functionapp deployment source config](/cli/azure/functionapp/deployment/source) bereit.

Im folgenden Befehl ist `<function_app>` der Name der zuvor erstellten Funktions-App.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-resize-function-node
```
---

Die Funktion für die Bildgrößenänderung wird durch HTTP-Anforderungen ausgelöst, die sie vom Event Grid-Dienst empfängt. Erstellen Sie ein Ereignisabonnement, um Event Grid zu informieren, dass Sie diese Benachrichtigungen unter der URL Ihrer Funktion erhalten möchten. Für dieses Tutorial abonnieren Sie durch Blobs erstellte Ereignisse.

Die von der Event Grid-Benachrichtigung an die Funktion übergebenen Daten enthalten die URL des Blobs. Diese URL wird wiederum an die Eingabebindung übergeben, um das hochgeladene Bild aus dem Blob-Speicher abzurufen. Die Funktion generiert eine Miniaturansicht und schreibt den sich ergebenden Datenstrom in einen separaten Container in Blob Storage. 

In diesem Projekt wird `EventGridTrigger` als Triggertyp verwendet. Es wird empfohlen, anstelle generischer HTTP-Trigger den Event Grid-Trigger zu verwenden. Event Grid überprüft automatisch Event Grid-Funktionstrigger. Bei generischen HTTP-Triggern müssen Sie die [Überprüfungsantwort](security-authentication.md#webhook-event-delivery) implementieren.

Weitere Informationen zu dieser Funktion finden Sie unter in den [Dateien „function.json“ und „run.csx“](https://github.com/Azure-Samples/function-image-upload-resize/tree/master/imageresizerfunc).
 
Der Funktionsprojektcode wird direkt aus dem öffentlichen Beispielrepository bereitgestellt. Weitere Informationen zu den Bereitstellungsoptionen für Azure Functions finden Sie unter [Continuous Deployment für Azure Functions](../azure-functions/functions-continuous-deployment.md).

## <a name="create-an-event-subscription"></a>Erstellen eines Ereignisabonnements

Ein Ereignisabonnement gibt an, welche vom Anbieter generierten Ereignisse an einen bestimmten Endpunkt gesendet werden sollen. In diesem Fall wird der Endpunkt durch Ihre Funktion zur Verfügung gestellt. Führen Sie die folgenden Schritte durch, um ein Ereignisabonnement zu erstellen, dass Benachrichtigungen an Ihre Funktion im Azure-Portal sendet: 

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Menü **Alle Dienste** und anschließend **Funktions-Apps** aus. 

    ![Navigieren zu Funktions-Apps im Azure-Portal](./media/resize-images-on-storage-blob-upload-event/portal-find-functions.png)

2. Erweitern Sie Ihre Funktions-App, wählen Sie die Funktion **Vorschauminiatur** aus, und wählen Sie dann **Event Grid-Abonnement hinzufügen** aus.

    ![Navigieren zu Funktions-Apps im Azure-Portal](./media/resize-images-on-storage-blob-upload-event/add-event-subscription.png)

3. Verwenden Sie die in der Tabelle angegebenen Einstellungen für das Ereignisabonnement.
    
    ![Erstellen eines Ereignisabonnements aus der Funktion im Azure-Portal](./media/resize-images-on-storage-blob-upload-event/event-subscription-create.png)

    | Einstellung      | Empfohlener Wert  | Beschreibung                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name** | imageresizersub | Der Name, der Ihr neues Ereignisabonnement angibt. | 
    | **Thementyp** |  Speicherkonten | Wählen Sie den Speicherkonto-Ereignisanbieter aus. | 
    | **Abonnement** | Ihr Azure-Abonnement | Ihr aktuelles Azure-Abonnement ist standardmäßig ausgewählt.   |
    | **Ressourcengruppe** | myResourceGroup | Wählen Sie **Vorhandene verwenden** und anschließend die Ressourcengruppe aus, die Sie in diesem Tutorial verwendet haben.  |
    | **Ressource** |  Ihr Blob Storage-Konto |  Wählen Sie das Blob Storage-Konto aus, das Sie erstellt haben. |
    | **Ereignistypen** | Blob erstellt | Deaktivieren Sie alle Typen außer **Blob erstellt**. Nur Ereignistypen mit `Microsoft.Storage.BlobCreated` werden an die Funktion übergeben.| 
    | **Abonnententyp** |  Automatisch generiert |  Als Webhook vordefiniert |
    | **Abonnentenendpunkt** | Automatisch generiert | Verwenden Sie die Endpunkt-URL, die für Sie generiert wird. | 
4. Wechseln Sie zur Registerkarte **Filter**, und führen Sie die folgenden Aktionen durch:     
    1. Wählen Sie die Option **Betrefffilter aktivieren** aus.
    2. Geben Sie für **Betreff beginnt mit** den folgenden Wert ein: **/blobServices/default/containers/images/blobs/**.

        ![Festlegen eines Filters für das Ereignisabonnement](./media/resize-images-on-storage-blob-upload-event/event-subscription-filter.png) 
2. Wählen Sie **Erstellen** aus, um das Ereignisabonnement hinzuzufügen. Dadurch wird ein Ereignisabonnement erstellt, das die Funktion `Thumbnail` auslöst, wenn ein Blob dem Container `images` hinzugefügt wird. Die Funktion ändert die Größe der Bilder und fügt sie dem Container `thumbnails` hinzu.

Nachdem die Back-End-Dienste konfiguriert wurden, testen Sie die Funktion für die Größenänderung von Bildern in der Beispiel-Web-App. 

## <a name="test-the-sample-app"></a>Testen der Beispiel-App

Navigieren Sie zum Testen der Größenänderung von Bildern in der Web-App zur URL Ihrer veröffentlichten App. Die Standard-URL der Web-App lautet `https://<web_app>.azurewebsites.net`.

Klicken Sie auf den Bereich **Fotos hochladen**, um eine Datei auszuwählen und hochzuladen. Sie können auch ein Foto in diese Region ziehen. 

Beachten Sie, dass eine Kopie des hochgeladenen Bilds im Karussell **Generierte Miniaturansichten** angezeigt wird, nachdem das hochgeladene Bild nicht mehr angezeigt wird. Die Größe dieses Bilds wurde von der Funktion angepasst, und das Bild wurde dem Container *thumbnails* hinzugefügt und vom Webclient heruntergeladen.

![Veröffentlichte Web-App im Microsoft Edge-Browser](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png) 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines neuen Azure Storage-Kontos
> * Bereitstellen von serverlosem Code mit Azure Functions
> * Erstellen eines Blob Storage-Ereignisabonnements in Event Grid

Fahren Sie mit Teil 3 der Tutorialreihe zu Storage fort, um zu erfahren, wie der Zugriff auf das Speicherkonto gesichert wird.

> [!div class="nextstepaction"]
> [Sichern des Zugriffs auf die Daten einer Anwendung in der Cloud](../storage/blobs/storage-secure-access-application.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

+ Weitere Informationen zu Event Grid finden Sie unter [Einführung in Azure Event Grid](overview.md). 
+ Ein weiteres Tutorial zu Azure Functions finden Sie unter [Erstellen einer Funktion, die in Azure Logic Apps integriert ist](../azure-functions/functions-twitter-email.md). 

[previous-tutorial]: ../storage/blobs/storage-upload-process-images.md
