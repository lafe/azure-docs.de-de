---
title: 'Azure IoT Hub-Gerätestreams: Node.js-Schnellstartanleitung (Vorschauversion) | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung führen Sie eine dienstseitige Node.js-Anwendung aus, die über einen Gerätestream mit einem IoT-Gerät kommuniziert.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/15/2019
ms.author: rezas
ms.openlocfilehash: 590faaf727345dcfe8ab61a1860ca46d78256b22
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55219004"
---
# <a name="quickstart-communicate-to-a-device-application-in-nodejs-via-iot-hub-device-streams-preview"></a>Schnellstart: Kommunizieren mit einer Geräteanwendung in Node.js über IoT Hub-Gerätestreams (Vorschauversion)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Über [IoT Hub-Gerätestreams](./iot-hub-device-streams-overview.md) können Dienst- und Geräteanwendungen sicher und firewallfreundlich kommunizieren. Während der Public Preview-Phase unterstützt das Node.js SDK nur dienstseitige Gerätestreams. Aus diesem Grund wird in dieser Schnellstartanleitung nur das Ausführen der dienstseitigen Anwendung erläutert. Es empfiehlt sich, auch eine entsprechende geräteseitige Anwendung auszuführen (verfügbar in der Schnellstartanleitung für [C](./quickstart-device-streams-echo-c.md) oder [C#](./quickstart-device-streams-echo-csharp.md)).

Die dienstseitige Node.js-Anwendung in dieser Schnellstartanleitung hat folgende Funktionen:

* Erstellen eines Gerätestreams mit einem IoT-Gerät

* Lesen von Befehlszeileneingaben und senden dieser Eingaben an die Geräteanwendung, die sie anschließend zurückgibt

Der Code veranschaulicht den Initiierungsprozess eines Gerätestreams sowie das Senden und Empfangen von Daten.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


## <a name="prerequisites"></a>Voraussetzungen

Auf Ihrem Entwicklungscomputer muss sich mindestens Node.js v4.x.x befinden, um die dienstseitige Anwendung in dieser Schnellstartanleitung ausführen zu können.

Sie können Node.js für mehrere Plattformen von [Node.js.org](https://nodejs.org) herunterladen.

Mit dem folgenden Befehl können Sie die aktuelle Node.js-Version auf Ihrem Entwicklungscomputer überprüfen:

```
node --version
```

Laden Sie das Node.js-Beispielprojekt von https://github.com/Azure-Samples/azure-iot-samples-node/archive/streams-preview.zip herunter, und extrahieren Sie das ZIP-Archiv (falls Sie dies nicht bereits getan haben).


## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Wenn Sie das vorherige Tutorial [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz und Lesen der Telemetriedaten aus der IoT Hub-Instanz mit einer Back-End-Anwendung (Node.js)](quickstart-send-telemetry-node.md) absolviert haben, können Sie diesen Schritt überspringen.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub-device-streams.md)]


## <a name="register-a-device"></a>Registrieren eines Geräts

Wenn Sie das vorherige Tutorial [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz und Lesen der Telemetriedaten aus der IoT Hub-Instanz mit einer Back-End-Anwendung (Node.js)](quickstart-send-telemetry-node.md) absolviert haben, können Sie diesen Schritt überspringen.

Ein Gerät muss bei Ihrer IoT Hub-Instanz registriert sein, um eine Verbindung herstellen zu können. In dieser Schnellstartanleitung verwenden Sie Azure Cloud Shell, um ein simuliertes Gerät zu registrieren.

1. Führen Sie die folgenden Befehle in Azure Cloud Shell aus, um die IoT Hub-CLI-Erweiterung hinzuzufügen und die Geräteidentität zu erstellen. 

   **YourIoTHubName**: Ersetzen Sie diesen Platzhalter unten durch den Namen, den Sie für Ihren IoT-Hub ausgewählt haben.

   **MyDevice**: Der für das registrierte Gerät angegebene Name. Verwenden Sie „MyDevice“ wie gezeigt. Wenn Sie für Ihr Gerät einen anderen Namen wählen, müssen Sie diesen innerhalb des gesamten Artikels verwenden und den Gerätenamen in den Beispielanwendungen aktualisieren, bevor Sie sie ausführen.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Darüber hinaus benötigen Sie eine _Dienstverbindungszeichenfolge_, damit die Back-End-Anwendung eine Verbindung mit Ihrer IoT Hub-Instanz herstellen und die Nachrichten abrufen kann. Der folgende Befehl ruft die Dienstverbindungszeichenfolge für Ihre IoT Hub-Instanz ab:

    **YourIoTHubName**: Ersetzen Sie diesen Platzhalter unten durch den Namen, den Sie für Ihren IoT-Hub ausgewählt haben.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --hub-name YourIoTHubName
    ```

    Notieren Sie sich den zurückgegebenen Wert, der in etwa wie folgt aussieht:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`


## <a name="communicate-between-device-and-service-via-device-streams"></a>Kommunizieren zwischen Gerät und Dienst über Gerätestreams

### <a name="run-the-device-side-application"></a>Ausführen der geräteseitigen Anwendung

Wie bereits erwähnt, unterstützt das IoT Hub Node.js SDK während der Public Preview-Phase nur dienstseitige Gerätestreams. Verwenden Sie als geräteseitige Anwendung die entsprechenden Geräteprogramme aus der Anleitung für [C](./quickstart-device-streams-echo-c.md) oder [C#](./quickstart-device-streams-echo-csharp.md). Vergewissern Sie sich, dass die geräteseitige Anwendung ausgeführt wird, bevor Sie mit dem nächsten Schritt fortfahren.


### <a name="run-the-service-side-application"></a>Ausführen der dienstseitigen Anwendung

Nachdem Sie sich vergewissert haben, dass die geräteseitige Anwendung ausgeführt wird, gehen Sie wie folgt vor, um die dienstseitige Anwendung in Node.js auszuführen:

- Geben Sie Ihre Dienstanmeldeinformationen und die Geräte-ID als Umgebungsvariablen an.
```
  # In Linux
  export IOTHUB_CONNECTION_STRING="<provide_your_service_connection_string>"
  export STREAMING_TARGET_DEVICE="MyDevice"

  # In Windows
  SET IOTHUB_CONNECTION_STRING=<provide_your_service_connection_string>
  SET STREAMING_TARGET_DEVICE=MyDevice
```
Ändern Sie `MyDevice` in die Geräte-ID, die Sie für Ihr Gerät gewählt haben.

- Navigieren Sie in Ihrem entzippten Projektordner zu `Quickstarts/device-streams-service`, und führen Sie das Beispiel mit dem Knoten aus.
```
  cd azure-iot-samples-node-streams-preview/iot-hub/Quickstarts/device-streams-service
  
  # Install the preview service SDK, and other dependencies
  npm install azure-iothub@streams-preview
  npm install

  node echo.js
```

Am Ende des letzten Schritts initiiert das dienstseitige Programm einen Stream an Ihr Gerät und sendet anschließend einen Zeichenfolgenpuffer über den Stream an den Dienst. In diesem Beispiel liest das dienstseitige Programm einfach stdin im Terminal und sendet die Daten an das Gerät, das sie daraufhin zurückgibt. Dies veranschaulicht eine erfolgreiche bidirektionale Kommunikation zwischen den beiden Anwendungen.

Konsolenausgabe auf der Dienstseite: ![Alternativer Text](./media/quickstart-device-streams-echo-nodejs/service-console-output.PNG "Konsolenausgabe auf der Dienstseite")


Anschließend können Sie das Programm durch erneutes Drücken der EINGABETASTE beenden.


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]


## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen IoT-Hub eingerichtet, ein Gerät registriert, einen Gerätestream zwischen geräte- und dienstseitigen Anwendungen eingerichtet und mithilfe des Streams Daten zwischen den Anwendungen hin und her gesendet.

Weitere Informationen zu Gerätestreams finden Sie hier:

> [!div class="nextstepaction"]
> [IoT Hub-Gerätestreams (Vorschau)](./iot-hub-device-streams-overview.md)
