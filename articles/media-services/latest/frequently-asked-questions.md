---
title: Häufig gestellte Fragen zu Azure Media Services v3 | Microsoft-Dokumentation
description: Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/05/2019
ms.author: juliako
ms.openlocfilehash: a447c359c38c2173ea42b6d717067fc8b3a88f9a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55875490"
---
# <a name="azure-media-services-v3-frequently-asked-questions"></a>Häufig gestellte Fragen zu Azure Media Services v3

Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu Azure Media Services (AMS) v3.

## <a name="v3-apis"></a>v3-APIs

### <a name="how-do-i-configure-media-reserved-units"></a>Wie lassen sich reservierte Einheiten für Medien konfigurieren?

Für die Audioanalyse- und Videoanalyseaufträge, die von Media Services v3 oder Video Indexer ausgelöst werden, wird dringend empfohlen, Ihr Konto mit zehn S3-MRUs bereitzustellen. Erstellen Sie im [Azure-Portal](https://portal.azure.com/) ein Supportticket, falls Sie mehr als zehn S3-MRUs benötigen.

Details dazu finden Sie unter [Skalieren der Medienverarbeitung an der Befehlszeilenschnittstelle](media-reserved-units-cli-how-to.md).

### <a name="what-is-the-recommended-method-to-process-videos"></a>Was ist die empfohlene Methode zum Verarbeiten von Videos?

Es empfiehlt sich, Aufträge mithilfe einer HTTP(s)-URL zu übermitteln, die auf das Video verweist. Weitere Informationen finden Sie unter [HTTP(s)-Erfassung](job-input-from-http-how-to.md). Sie brauchen vor der Verarbeitung kein Medienobjekt mit dem Eingabevideo zu erstellen.

### <a name="how-does-pagination-work"></a>Wie funktioniert die Paginierung?

Bei Verwendung der Paginierung sollten Sie immer den Link „Weiter“ verwenden, um die Sammlung zu enumerieren, und sich nicht auf eine bestimmte Seitengröße verlassen. Weitere Informationen und Beispiele finden Sie unter [Filterung, Sortierung, Paginierung](entities-overview.md).

## <a name="live-streaming"></a>Livestreaming 

###  <a name="how-to-insert-breaksvideos-and-image-slates-during-live-stream"></a>Wie werden Unterbrechungen/Videos und Bildtafeln während des Livedatenstroms eingefügt?

Die Livecodierung von Media Services v3 unterstützt aktuell das Einfügen von Video oder Bildtafeln in Livedatenströme noch nicht. 

Sie können einen [lokalen Liveencoder](recommended-on-premises-live-encoders.md) verwenden, um das Quellvideo zu wechseln. Viele Apps bieten die Möglichkeit zum Wechseln der Quelle, darunter Telestream Wirecast, Switcher Studio (unter iOS), OBS Studio (kostenlose App) und viele weitere.

## <a name="media-services-v2-vs-v3"></a>Media Services v2 i. Vgl. mit v3 

### <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>Kann ich v3-Ressourcen im Azure-Portal verwalten?

Bisher nicht. Sie können eines der unterstützten SDKs verwenden. Informationen finden Sie in den Lernprogrammen und Beispielen in dieser Dokumentation.

### <a name="is-there-an-assetfile-concept-in-v3"></a>Gibt es in v3 ein AssetFile-Konzept?

„AssetFile“ wurde aus der AMS-API entfernt. So ist Media Services nicht länger vom Storage SDK abhängig. Ab jetzt sind die Informationen, die zu Azure Storage gehören, in Storage enthalten und nicht mehr in Media Services. 

Weitere Informationen finden Sie unter [Migration zu Media Services v3](migrate-from-v2-to-v3.md).

### <a name="where-did-client-side-storage-encryption-go"></a>Wo ist die clientseitige Speicherverschlüsselung geblieben?

Aktuell wird empfohlen, die serverseitige Speicherverschlüsselung (die standardmäßig aktiviert ist) zu verwenden. Weitere Informationen finden Sie unter [Azure Storage Service Encryption für ruhende Daten](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Media Services v3 – Übersicht](media-services-overview.md)
