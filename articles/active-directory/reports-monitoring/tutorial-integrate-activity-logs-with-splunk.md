---
title: Streamen von Azure Active Directory-Protokollen in Splunk mit Azure Monitor (Vorschauversion) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure Active Directory-Protokolle in Splunk mit Azure Monitor (Vorschauversion) integrieren.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: c4b605b6-6fc0-40dc-bd49-101d03f34665
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 31a4f5028cc6711ec92a495b19a17e8a0fbf11aa
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56170457"
---
# <a name="integrate-azure-ad-logs-with-splunk-using-azure-monitor-preview"></a>Integrieren von Azure AD-Protokollen in Splunk mit Azure Monitor (Vorschauversion)

In diesem Artikel erfahren Sie, wie Sie Azure Active Directory-Protokolle (Azure AD) in Splunk mit Azure Monitor integrieren. Zunächst leiten Sie die Protokolle an einen Azure Event Hub weiter und integrieren dann den Event Hub in Splunk.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um dieses Feature verwenden zu können:
* Ein Azure Event Hub, der Azure AD-Aktivitätsprotokolle enthält. Erfahren Sie, [wie Sie Aktivitätsprotokolle an einen Event Hub streamen](quickstart-azure-monitor-stream-logs-to-event-hub.md). 
* Das Azure Monitor-Add-On für Splunk. [Laden Sie Ihre Splunk-Instanz herunter, und konfigurieren Sie sie](https://github.com/Microsoft/AzureMonitorAddonForSplunk/blob/master/README.md).

## <a name="tutorial"></a>Tutorial 

1. Öffnen Sie Ihre Splunk-Instanz, und wählen Sie **Data Summary** (Datenzusammenfassung) aus.

    ![Die Schaltfläche für die Datenzusammenfassung](./media/tutorial-integrate-activity-logs-with-splunk/DataSummary.png)

2. Wählen Sie die Registerkarte **Sourcetypes** (Quelltypen) und dann **amal: aadal:audit** aus.

    ![Die Registerkarte mit Quelltypen der Datenzusammenfassung](./media/tutorial-integrate-activity-logs-with-splunk/sourcetypeaadal.png)

    Die Azure AD-Aktivitätsprotokolle werden wie in der folgenden Abbildung angezeigt:

    ![Aktivitätsprotokolle](./media/tutorial-integrate-activity-logs-with-splunk/activitylogs.png)

> [!NOTE]
> Falls Sie kein Add-On in Ihrer Splunk-Instanz installieren können, z.B. bei Verwendung eines Proxys oder bei Ausführung in Splunk Cloud, können Sie diese Ereignisse an die HTTP-Ereignissammlung von Splunk weiterleiten. Verwenden Sie dazu diese [Azure-Funktion](https://github.com/Microsoft/AzureFunctionforSplunkVS), die durch neue Nachrichten in Event Hub ausgelöst wird. 
>

## <a name="next-steps"></a>Nächste Schritte

* [Interpretieren des Überwachungsprotokollschemas in Azure Monitor](reference-azure-monitor-audit-log-schema.md)
* [Interpret sign-in logs schema in Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md) (Interpretieren des Anmeldeprotokollschemas in Azure Monitor)
* [Häufig gestellte Fragen und bekannte Probleme](concept-activity-logs-azure-monitor.md#frequently-asked-questions)
