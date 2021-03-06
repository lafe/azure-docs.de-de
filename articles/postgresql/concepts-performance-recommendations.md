---
title: Leistungsempfehlungen in Azure Database for PostgreSQL
description: Dieser Artikel beschreibt die Empfehlungen zur Leistung, die in Azure Database for PostgreSQL erzielt werden kann.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/26/2018
ms.openlocfilehash: 1dedc08f27d1a483290dc61cce879290ca66592d
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548091"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql"></a>Leistungsempfehlungen in Azure Database for PostgreSQL

**Anwendungsbereich:** Azure Database for PostgreSQL 9.6 und 10

> [!IMPORTANT]
> Die Leistungsempfehlungen befinden sich in der Public Preview.

Das Leistungsempfehlungen-Feature gibt die obersten Indizes an, die Sie in Ihrer Azure Database for PostgreSQL-Serverinstanz zur Verbesserung der Leistung erstellen können. Um Indexempfehlungen zu erzeugen, berücksichtigt das Feature verschiedene Datenbankmerkmale einschließlich des Schemas und der Workload laut Abfragespeicher. Nach der Implementierung von Leistungsempfehlungen sollten Kunden die Leistung testen, um die Auswirkungen dieser Änderungen auszuwerten. 

## <a name="permissions"></a>Berechtigungen
Zum Ausführen einer Analyse mit dem Leistungsempfehlungen-Feature sind die Berechtigungen **Besitzer** oder **Mitwirkender** erforderlich.

## <a name="performance-recommendations"></a>Empfehlungen zur Leistung
Das Feature [Leistungsempfehlungen](concepts-performance-recommendations.md) analysiert Workloads auf Ihrem Server zum Identifizieren von Indizes mit der Möglichkeit zur Verbesserung der Leistung.

Öffnen Sie im Bereich **Support und Problembehandlung** auf der Menüleiste der Azure-Portalseite für Ihren PostgreSQL-Server die Option **Leistungsempfehlungen**.

![Leistungsempfehlungen-Startseite](./media/concepts-performance-recommendations/performance-recommendations-landing-page.png)

Wählen Sie **Analysieren** und eine Datenbank aus. Dadurch wird die Analyse gestartet. Je nach Workload kann dies einige Minuten dauern. Wenn die Analyse abgeschlossen ist, wird im Portal eine Benachrichtigung angezeigt.

Das Fenster **Leistungsempfehlungen** zeigt eine Liste mit den gefundenen Empfehlungen an. Eine Empfehlung zeigt Informationen zur relevanten **Datenbank**, **Tabelle**, **Spalte** und **Indexgröße** an.

![Neue Seite „Leistungsempfehlungen“](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Kopieren Sie zum Implementieren der Empfehlung den Abfragetext, und führen Sie ihn auf dem gewünschten Client aus.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Überwachung und Optimierung](concepts-monitoring.md) in Azure Database for PostgreSQL.

