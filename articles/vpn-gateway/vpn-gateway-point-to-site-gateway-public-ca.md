---
title: Umstellung von selbstsignierten Zertifikaten auf Zertifikate von öffentlichen Zertifizierungsstellen für P2S-Gateways | Azure-VPN-Gateway | Microsoft-Dokumentation
description: In diesem Artikel wird die Umstellung auf die neuen Zertifikate von öffentlichen Zertifizierungsstellen für P2S-Gateways erläutert.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: cherylmc
ms.openlocfilehash: ac1ae4125418a9c0b3e9587cd03a44e752ac8f82
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56236956"
---
# <a name="transition-from-self-signed-to-public-ca-certificates-for-p2s-gateways"></a>Umstellung von selbstsignierten Zertifikaten auf Zertifikate von öffentlichen Zertifizierungsstellen für P2S-Gateways

Azure-VPN-Gateway stellt für P2S-Verbindungen keine selbstsignierten Zertifikate mehr für Gateways aus. Ausgestellte Zertifikate werden nun von einer öffentlichen Zertifizierungsstelle signiert. Allerdings werden von älteren Gateways ggf. weiterhin selbstsignierte Zertifikate verwendet. Diese selbstsignierten Zertifikate stehen kurz vor ihrem Ablaufdatum und müssen auf Zertifikate von öffentlichen Zertifizierungsstellen umgestellt werden.

Bisher musste das selbstsignierte Zertifikat für das Gateway alle 18 Monate aktualisiert werden. VPN-Clientkonfigurationsdateien mussten anschließend generiert und für alle P2S-Clients erneut bereitgestellt werden. Durch die Umstellung auf Zertifikate von öffentlichen Zertifizierungsstellen entfällt diese Einschränkung. Neben der Umstellung bei Zertifikaten bietet diese Änderung auch Plattformverbesserungen, bessere Metriken und mehr Stabilität.

Von dieser Änderung sind nur ältere Gateways betroffen. Wenn Ihr Gatewayzertifikat umgestellt werden muss, erhalten Sie im Azure-Portal eine (Popup-) Benachrichtigung. Sie können anhand der in diesem Artikel beschriebenen Schritte überprüfen, ob Ihr Gateway betroffen ist.

>[!IMPORTANT]
>Die Umstellung ist für den 12.03.2019 um 18:00 Uhr (UTC) geplant. Wenn Sie ein anderes Zeitfenster bevorzugen, können Sie eine Supportanfrage stellen. Stellen Sie Ihre Supportanfrage mindestens 24 Stunden im Voraus.  Sie können eines der folgenden Zeitfenster anfordern:
>
>* 06:00 Uhr (UTC) am 25. Feb.
>* 18:00 Uhr (UTC) am 25. Feb.
>* 06:00 Uhr (UTC) am 01. Mär.
>* 18:00 Uhr (UTC) am 01. Mär.
>
>**Alle verbleibenden Gateways werden am 12 März 2019 ab 18:00 Uhr (UTC) umgestellt**.
>

## <a name="1-verify-your-certificate"></a>1. Überprüfen Ihres Zertifikats

1. Prüfen Sie, ob Sie von diesem Update betroffen sind. Laden Sie Ihre aktuelle VPN-Client-Konfiguration unter Befolgung der Anweisungen in [diesem Artikel](point-to-site-vpn-client-configuration-azure-cert.md) herunter.

2. Öffnen oder extrahieren Sie die ZIP-Datei, und wechseln Sie zum Ordner „Generic“. Im Ordner „Generic“ finden Sie zwei Dateien, von denen eine *VPNSettings.xml* ist.
3. Öffnen Sie Datei *VPNSettings.xml* in einem beliebigen XML-Viewer/-Editor. Suchen Sie in der XML-Datei nach den folgenden Feldern:

  * `<ServerCertRootCn>DigiCert Global Root CA</ServerCertRootCn>`
  * `<ServerCertIssuerCn>DigiCert Global Root CA</ServerCertIssuerCn>`
4. Wenn für *ServerCertRotCn* und *ServerCertIssuerCn* „DigiCert Global Root CA“ angezeigt wird, sind Sie von dieser Aktualisierung nicht betroffen, weshalb Sie nicht mit den Schritten in diesem Artikel fortfahren müssen. Wenn jedoch etwas anderes angezeigt wird, unterliegt Ihr Gatewayzertifikat der Aktualisierung und wird umgestellt.

## <a name="2-check-certificate-transition-schedule"></a>2. Überprüfen des Zeitplans für die Zertifikatsumstellung

Wenn Ihr Zertifikat der Aktualisierung unterliegt, wird Ihr Gatewayzertifikat umgestellt. Unter dem Hinweis **Wichtig** finden Sie Umstellungszeiten. Nach der Aktualisierung können P2S-Clients mit ihrem alten Profil keine Verbindung mehr herstellen. Sie müssen neue VPN-Clientprofile generieren und diese auf den Clients installieren.

## <a name="3-generate-vpn-client-configuration-profile"></a>3. Generieren von VPN-Clientkonfigurationsprofilen

Sobald das Zertifikat umgestellt wurde, laden Sie das neue VPN-Profil (mit den VPN-Clientkonfigurationsdateien) aus dem Azure-Portal herunter. Anweisungen finden Sie unter [Erstellen und Installieren von Konfigurationsdateien für VPN-Clients](point-to-site-vpn-client-configuration-azure-cert.md). Sie müssen keine neuen Clientzertifikate generieren.

## <a name="4-deploy-vpn-client-profile"></a>4. Bereitstellen des VPN-Clientprofils

Stellen Sie das neue Profil für alle Punkt-zu-Standort-VPN-Clients bereit. VPN-Clients verlieren die Konnektivität, bis das neue VPN-Profil für Punkt-zu-Standort-Verbindungen heruntergeladen und auf Clientgeräten bereitgestellt wurde. Die Clientzertifikate, die bereits auf VPN-Clientcomputern installiert sind, müssen nicht erneut ausgestellt werden.

## <a name="5-connect-the-vpn-client"></a>5. Herstellen der Verbindung zwischen VPN-Client und Azure

Stellen Sie wie gewohnt eine Verbindung zwischen dem VPN-Client und Azure her.
