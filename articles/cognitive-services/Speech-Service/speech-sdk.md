---
title: Informationen zum Speech-Dienst-SDK – Speech Services
titleSuffix: Azure Cognitive Services
description: Das Speech-Dienst-SDK (Software Development Kit) gewährt Ihren Anwendungen nativen Zugriff auf die Funktionen des Speech-Diensts, sodass die Entwicklung von Software erleichtert wird. Dieser Artikel enthält weitere Informationen zum SDK für Windows, Linux und Android.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: 5d98ae7fe4062ce88ee2a3df5f9bb4d992d4023c
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984680"
---
# <a name="about-the-speech-service-sdk"></a>Informationen zum Speech-Dienst-SDK

Das Speech-Dienst-SDK (Software Development Kit) gewährt Ihren Anwendungen nativen Zugriff auf die Funktionen des Speech-Diensts, sodass die Entwicklung von Software erleichtert wird. Derzeit stellt das SDK den Zugriff auf die **Spracherkennung**, die **Sprachübersetzung** und die **Absichtserkennung** bereit.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>Abrufen des SDK

### <a name="windows"></a>Windows

Für Windows werden die folgenden Sprachen unterstützt:

* C# (UWP und .NET), C++: Sie können die neueste Version des Speech SDK-NuGet-Pakets referenzieren und verwenden. Das Paket enthält 32-Bit- und 64-Bit-Clientbibliotheken sowie verwaltete (.NET-)Bibliotheken. Das SDK kann in Visual Studio mithilfe von NuGet installiert werden. Suchen Sie nach **Microsoft.CognitiveServices.Speech**.

* Java: Sie können die neueste Version des Speech-SDK-Maven-Pakets, das nur Windows x64 unterstützt, referenzieren und verwenden. Fügen Sie in Ihrem Maven-Projekt `https://csspeechstorage.blob.core.windows.net/maven/` als zusätzliches Repository hinzu, und verweisen Sie auf `com.microsoft.cognitiveservices.speech:client-sdk:1.2.0` als Abhängigkeit.

### <a name="linux"></a>Linux

> [!NOTE]
> Derzeit unterstützen wir nur Ubuntu 16.04 und 18.04 auf einem PC (x86 oder x64 für die C++-Entwicklung und x64 für .NET Core und Java).

Stellen Sie sicher, dass der erforderliche Compiler und die erforderlichen Bibliotheken installiert sind, indem Sie folgende Shellbefehle ausführen:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

* C#: Sie können die neueste Version des Speech SDK-NuGet-Pakets referenzieren und verwenden. Um auf das SDK zu verweisen, fügen Sie Ihrem Projekt den folgenden Paketverweis hinzu:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="1.2.0" />
  ```

* Java: Sie können die neueste Version des Speech SDK-Maven-Pakets referenzieren und verwenden. Fügen Sie in Ihrem Maven-Projekt `https://csspeechstorage.blob.core.windows.net/maven/` als zusätzliches Repository hinzu, und verweisen Sie auf `com.microsoft.cognitiveservices.speech:client-sdk:1.2.0` als Abhängigkeit.

* C++: Laden Sie das SDK als [TAR-Paket](https://aka.ms/csspeech/linuxbinary) herunter, und entpacken Sie die Dateien in einem Verzeichnis Ihrer Wahl. In der folgenden Tabelle wird die Ordnerstruktur des SDK gezeigt:

  |path|BESCHREIBUNG|
  |-|-|
  |`license.md`|Lizenz|
  |`ThirdPartyNotices.md`|Hinweise zu Drittanbietern|
  |`include`|Headerdateien für C und C++|
  |`lib/x64`|Native x64-Bibliothek für die Verknüpfung mit Ihrer Anwendung|
  |`lib/x86`|Native x86-Bibliothek für die Verknüpfung mit Ihrer Anwendung|

  Wenn Sie eine Anwendung erstellen möchten, kopieren oder verschieben Sie die erforderlichen Binärdateien (und Bibliotheken) in Ihre Entwicklungsumgebung. Fügen Sie sie nach Bedarf in Ihren Buildprozess ein.

### <a name="android"></a>Android

Das Java SDK für Android ist als [AAR (Android-Bibliothek)](https://developer.android.com/studio/projects/android-library) verpackt und enthält die erforderlichen Bibliotheken sowie die erforderlichen Android-Berechtigungen. Es wird in einem Maven-Repository unter `https://csspeechstorage.blob.core.windows.net/maven/` als Paket `com.microsoft.cognitiveservices.speech:client-sdk:1.2.0` gehostet.

Um das Paket im Android Studio-Projekt zu nutzen, nehmen Sie die folgenden Änderungen vor:

* Fügen Sie der Datei „build.gradle“ auf Projektebene Folgendes im Abschnitt `repository` hinzu:

  ```gradle
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* Fügen Sie der Datei „build.gradle“ auf Modulebene Folgendes im Abschnitt `dependencies` hinzu:

  ```gradle
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.2.0'
  ```

Das Java SDK ist auch Teil des [Speech-Geräte-SDK](speech-devices-sdk.md).

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)
