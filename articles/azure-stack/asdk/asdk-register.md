---
title: Registrieren des ASDK bei Azure | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Azure Stack bei Azure registriert wird, um die Azure Marketplace-Syndikation und die Berichterstellung zur Verwendung zu aktivieren.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: 5daf60669d6e647fc1060bb5ac172b777c9559c5
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55512294"
---
# <a name="azure-stack-registration"></a>Azure Stack-Registrierung
Sie können Ihre ASDK-Installation (Azure Stack Development Kit) bei Azure registrieren, um Marketplace-Elemente von Azure herunterzuladen und die Berichterstellung zu E-Commerce-Daten für Microsoft einzurichten. Die Registrierung ist für die uneingeschränkte Unterstützung aller Azure Stack-Funktionen (einschließlich Marketplace-Syndikation) erforderlich. Eine Registrierung ist erforderlich, da Sie hierüber wichtige Azure Stack-Funktionen wie die Marketplace-Syndikation und Berichterstellung zur Verwendung testen können. Nachdem Sie Azure Stack registrieren, wird Ihre Nutzung an den Azure-Commerce übermittelt. Dies wird im Abonnement angezeigt, das Sie zur Registrierung verwendet haben. Die übermittelte Nutzung durch Benutzer des ASDK wird diesen jedoch nicht in Rechnung gestellt.

Wenn Sie Ihr ASDK nicht registrieren, wird Ihnen die Warnung **Aktivierung erforderlich** angezeigt, die Ihnen dazu rät, Ihr Azure Stack Development Kit zu registrieren. Dies ist das erwartete Verhalten.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie die folgenden Anweisungen zur Registrierung des ASDK bei Azure befolgen, installieren Sie die mit Azure Stack kompatiblen PowerShell-Module, und laden Sie die Azure Stack-Tools herunter. Dies wird im Artikel [Konfiguration nach der Bereitstellung](asdk-post-deploy.md) beschrieben.

Außerdem muss der PowerShell-Sprachmodus auf dem Computer, der zur Registrierung des ASDK bei Azure verwendet wird, auf **FullLanguageMode** festgelegt werden. Um zu überprüfen, ob der aktuelle Sprachmodus auf vollständig festgelegt ist, öffnen Sie ein PowerShell-Fenster mit erhöhten Rechten, und führen Sie die folgenden PowerShell-Befehle aus:

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Vergewissern Sie sich, dass in der Ausgabe **FullLanguageMode** zurückgegeben wird. Wenn ein anderer Sprachmodus zurückgegeben wird, muss die Registrierung auf einem anderen Computer ausgeführt oder der Sprachmodus auf **FullLanguageMode** festgelegt werden, damit Sie fortfahren können.

Das für die Registrierung verwendete Azure AD-Konto muss Zugriff auf das Azure-Abonnement besitzen und berechtigt sein, Identitätsanwendungen und Dienstprinzipale in dem Verzeichnis zu erstellen, das diesem Abonnement zugeordnet ist. Wir empfehlen, dass Sie Azure Stack mithilfe einer Verwaltung mit geringstmöglichen Berechtigungen bei Azure registrieren, indem Sie [ein für die Registrierung zu verwendendes Dienstkonto erstellen](..\azure-stack-registration-role.md), statt Anmeldeinformationen eine globalen Administrators zu verwenden.

## <a name="register-azure-stack-with-azure"></a>Registrieren von Azure Stack in Azure
Führen Sie diese Schritte aus, um das ASDK bei Azure zu registrieren.

> [!NOTE]
> Alle diese Schritte müssen von einem Computer ausgeführt werden, der Zugriff auf den privilegierten Endpunkt hat. Beim ASDK ist das der Development Kit-Hostcomputer.

1. Öffnen Sie eine PowerShell-Konsole als Administrator.  

2. Führen Sie die folgenden PowerShell-Befehle zum Registrieren Ihrer ASDK-Installation bei Azure aus. Sie müssen sich sowohl bei Ihrem Azure-Abonnement als auch bei der lokalen ASDK-Installation anmelden. Wenn Sie noch nicht über ein Azure-Abonnement verfügen, [können Sie hier ein kostenloses Azure-Konto erstellen](https://azure.microsoft.com/free/?b=17.06). Durch das Registrieren von Azure Stack fallen keine zusätzlichen Kosten in Ihrem Azure-Abonnement an.<br><br>Legen Sie einen eindeutigen Namen für die Registrierung fest, wenn Sie das Cmdlet **Set-AzsRegistration** ausführen. Der **RegistrationName**-Parameter hat den Standardwert **AzureStackRegistration**. Wenn Sie den gleichen Namen jedoch in mehreren Instanzen von Azure Stack verwenden, tritt beim Ausführen des Skripts ein Fehler auf.

    ```PowerShell  
    # Add the Azure cloud subscription environment name. 
    # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
    Add-AzureRmAccount -EnvironmentName "<environment name>"

    # Register the Azure Stack resource provider in your Azure subscription
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

    # Import the registration module that was downloaded with the GitHub tools
    Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

    # If you have multiple subscriptions, run the following command to select the one you want to use:
    # Get-AzureRmSubscription -SubscriptionID "<subscription ID>" | Select-AzureRmSubscription
    
    # Register Azure Stack
    $AzureContext = Get-AzureRmContext
    $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
    $RegistrationName = "<unique-registration-name>"
    Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint AzS-ERCS01 `
    -BillingModel Development `
    -RegistrationName $RegistrationName `
    -UsageReportingEnabled:$true
    ```
3. Nach Abschluss des Skripts sollte diese Meldung angezeigt werden: **Ihre Umgebung ist nun registriert und mit den angegebenen Parametern aktiviert.**

    ![Ihre Umgebung ist nun registriert.](media/asdk-register/1.PNG)


## <a name="register-in-disconnected-environments"></a>Registrieren in nicht verbundenen Umgebungen
Wenn Sie Azure Stack in einer nicht mit Azure verbundenen Umgebung (ohne Internetverbindung) registrieren, müssen Sie ein Registrierungstoken aus der Azure Stack-Umgebung abrufen und dieses Token dann auf einem Computer verwenden, der eine Verbindung mit Azure herstellen kann. Es dient zum Registrieren und Erstellen einer Aktivierungsressource für Ihre ASDK-Umgebung.
 
 > [!IMPORTANT]
 > Stellen Sie vor der Verwendung dieser Anleitung zum Registrieren von Azure Stack sicher, dass Sie PowerShell für Azure Stack installiert und die Azure Stack-Tools wie unter [Konfiguration nach der Bereitstellung](asdk-post-deploy.md) beschrieben heruntergeladen haben. Dies gilt sowohl für den ASDK-Hostcomputer als auch für den Computer mit Internetzugriff, der zum Herstellen der Verbindung mit Azure und zum Durchführen der Registrierung verwendet wird.

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Abrufen eines Registrierungstokens aus der Azure Stack-Umgebung
Starten Sie auf dem ASDK-Hostcomputer PowerShell als Administrator, und navigieren Sie zum Ordner **Registration** im Verzeichnis **AzureStack-Tools-master**, das beim Herunterladen der Azure Stack-Tools erstellt wurde. Verwenden Sie die folgenden PowerShell-Befehle, um das Modul **RegisterWithAzure.psm1** zu importieren, und verwenden Sie anschließend das Cmdlet **Get-AzsRegistrationToken**, um das Registrierungstoken abzurufen:  

   ```PowerShell  
   # Import the registration module that was downloaded with the GitHub tools
   Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

   # Create registration token
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
   # File path to save the token. This example saves the file as C:\RegistrationToken.txt.
   $FilePathForRegistrationToken = "$env:SystemDrive\RegistrationToken.txt"
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $CloudAdminCred `
   -UsageReportingEnabled:$false `
   -PrivilegedEndpoint AzS-ERCS01 `
   -BillingModel Development `
   -MarketplaceSyndicationEnabled:$false `
   -TokenOutputFilePath $FilePathForRegistrationToken
   ```

Speichern Sie dieses Registrierungstoken für die Verwendung auf dem Computer mit der Internetverbindung. Sie können die Datei oder den Text aus der Datei kopieren, die durch den Parameter „$FilePathForRegistrationToken“ erstellt wurde.

### <a name="connect-to-azure-and-register"></a>Verbinden mit Azure und Registrieren
Verwenden Sie auf dem Computer mit Internetverbindung die folgenden PowerShell-Befehle, um das Modul **RegisterWithAzure.psm1** zu importieren. Verwenden Sie anschließend das Cmdlet **Register-AzsEnvironment** für die Registrierung bei Azure, indem Sie das gerade erstellte Registrierungstoken und einen eindeutigen Registrierungsnamen angeben:  

  ```PowerShell  
  # Add the Azure cloud subscription environment name. 
  # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
  Add-AzureRmAccount -EnvironmentName "<environment name>"

  # If you have multiple subscriptions, run the following command to select the one you want to use:
  # Get-AzureRmSubscription -SubscriptionID "<subscription ID>" | Select-AzureRmSubscription

  # Register the Azure Stack resource provider in your Azure subscription
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  # Register with Azure
  # This example uses the C:\RegistrationToken.txt file.
  $registrationToken = Get-Content -Path "$env:SystemDrive\RegistrationToken.txt"
  $RegistrationName = "<unique-registration-name>"
  Register-AzsEnvironment -RegistrationToken $registrationToken `
  -RegistrationName $RegistrationName
  ```

Alternativ können Sie mit dem Cmdlet **Get-Content** auf eine Datei verweisen, die Ihr Registrierungstoken enthält:

  ```PowerShell  
  # Add the Azure cloud subscription environment name. 
  # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
  Add-AzureRmAccount -EnvironmentName "<environment name>"

  # If you have multiple subscriptions, run the following command to select the one you want to use:
  # Get-AzureRmSubscription -SubscriptionID "<subscription ID>" | Select-AzureRmSubscription

  # Register the Azure Stack resource provider in your Azure subscription
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  # Register with Azure 
  # This example uses the C:\RegistrationToken.txt file.
  $registrationToken = Get-Content -Path "$env:SystemDrive\RegistrationToken.txt"
  Register-AzsEnvironment -RegistrationToken $registrationToken `
  -RegistrationName $RegistrationName
  ```

Nach Abschluss der Registrierung sollte eine ähnliche Meldung wie **Ihre Azure Stack-Umgebung ist jetzt bei Azure registriert.** angezeigt werden.

> [!IMPORTANT]
> Lassen Sie das PowerShell-Fenster geöffnet. 

Speichern Sie das Registrierungstoken und den Namen der Registrierungsressource zur späteren Verwendung.

### <a name="retrieve-an-activation-key-from-the-azure-registration-resource"></a>Abrufen eines Aktivierungsschlüssels aus der Azure-Registrierungsressource

Verwenden Sie weiterhin den Computer mit Internetverbindung **und dasselbe PowerShell-Konsolenfenster**, und rufen Sie einen Aktivierungsschlüssel von der Registrierungsressource ab, die bei der Registrierung für Azure erstellt wurde.

Führen Sie zum Abrufen des Aktivierungsschlüssels die folgenden PowerShell-Befehle aus, und verwenden Sie den gleichen eindeutigen Registrierungsnamenswert, den Sie bei der Registrierung für Azure im vorherigen Schritt angegeben haben:  

  ```Powershell
  $RegistrationResourceName = "<unique-registration-name>"
  # File path to save the activation key. This example saves the file as C:\ActivationKey.txt.
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName `
  -KeyOutputFilePath $KeyOutputFilePath
  ```
### <a name="create-an-activation-resource-in-azure-stack"></a>Erstellen einer Aktivierungsressource in Azure Stack

Kehren Sie mit der Datei oder dem Text aus dem Aktivierungsschlüssel, der von **Get-AzsActivationKey** erstellt wurde, zur Azure Stack-Umgebung zurück. Führen Sie die folgenden PowerShell-Befehle aus, um mit diesem Aktivierungsschlüssel eine Aktivierungsressource in Azure Stack zu erstellen:   

  ```Powershell
  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1
  
  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
  $ActivationKey = "<activation key>"
  New-AzsActivationResource -PrivilegedEndpointCredential $CloudAdminCred `
  -PrivilegedEndpoint AzS-ERCS01 `
  -ActivationKey $ActivationKey
  ```

Alternativ können Sie mit dem Cmdlet **Get-Content** auf eine Datei verweisen, die Ihr Registrierungstoken enthält:

  ```Powershell
  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
  # This example uses the C:\ActivationKey.txt file.
  $ActivationKey = Get-Content -Path "$env:SystemDrive\Activationkey.txt"
  New-AzsActivationResource -PrivilegedEndpointCredential $CloudAdminCred `
  -PrivilegedEndpoint AzS-ERCS01 `
  -ActivationKey $ActivationKey
  ```

Nach Abschluss der Registrierung sollte eine ähnliche Meldung wie **Der Registrierungs- und Aktivierungsvorgang für Ihre Umgebung wurde beendet.** angezeigt werden.

## <a name="verify-the-registration-was-successful"></a>Überprüfen der erfolgreichen Ausführung der Registrierung

Sie können die Kachel **Regionsverwaltung** verwenden, um zu überprüfen, ob die Azure Stack-Registrierung erfolgreich war. Diese Kachel steht im Standarddashboard im Administratorportal zur Verfügung.

1. Melden Sie sich beim [Azure Stack-Verwaltungsportal](https://adminportal.local.azurestack.external) an.

2. Wählen Sie im Dashboard **Regionsverwaltung** aus.

    [ ![Kachel „Regionsverwaltung“](media/asdk-register/admin1sm.png "Kachel „Regionsverwaltung“")](media/asdk-register/admin1.png#lightbox)

3. Wählen Sie **Eigenschaften** aus. Auf diesem Blatt werden der Status und Details Ihrer Umgebung angezeigt. Der Status kann **registriert** oder **nicht registriert** lauten. Wenn er „Registriert“ ist, wird außerdem die ID des Azure-Abonnements angezeigt, mit dem Sie Ihren Azure Stack registriert haben, zusammen mit der Registrierungsressourcengruppe und dem Namen.

## <a name="move-a-registration-resource"></a>Verschieben einer Registrierungsressource
Das Verschieben einer Registrierungsressource zwischen Ressourcengruppen im gleichen Abonnement **wird unterstützt**. Weitere Informationen zum Verschieben von Ressourcen in eine neue Ressourcengruppe finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).


## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen eines Azure Stack-Marketplace-Elements](../azure-stack-marketplace.md)
