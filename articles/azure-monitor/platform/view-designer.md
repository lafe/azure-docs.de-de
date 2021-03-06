---
title: Erstellen von Ansichten zum Analysieren von Daten in Azure Log Analytics | Microsoft-Dokumentation
description: Mit dem Ansicht-Designer in Log Analytics können Sie benutzerdefinierte Ansichten erstellen, die im Azure-Portal angezeigt werden und verschiedene Datenvisualisierungen im Log Analytics-Arbeitsbereich enthalten. Dieser Artikel enthält eine Übersicht über den Ansicht-Designer und die Verfahren zum Erstellen und Bearbeiten von benutzerdefinierten Ansichten.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: bwren
ms.openlocfilehash: ec56e21a989fb0e8db7b8bafb1357c6ed64eae75
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53192249"
---
# <a name="create-custom-views-by-using-view-designer-in-log-analytics"></a>Erstellen benutzerdefinierter Ansichten mithilfe des Ansicht-Designers in Log Analytics
Mithilfe des Ansicht-Designers in [Azure Log Analytics](../../azure-monitor/log-query/log-query-overview.md) können Sie verschiedene benutzerdefinierten Ansichten im Azure-Portal erstellen, in denen Sie Daten in Ihrem Log Analytics-Arbeitsbereich visualisieren können. Dieser Artikel bietet eine Übersicht über den Ansicht-Designer und die Verfahren zum Erstellen und Bearbeiten von benutzerdefinierten Ansichten.

Weitere Informationen zum Ansicht-Designer finden Sie in folgenden Artikeln:

* [Kachelreferenz](view-designer-tiles.md): Referenzleitfaden zu den Einstellungen für die einzelnen verfügbaren Kacheln in den benutzerdefinierten Ansichten.
* [Referenz zur Visualisierungskomponente](view-designer-parts.md): Bietet einen Referenzleitfaden zu den Einstellungen für die in Ihren benutzerdefinierten Ansichten verfügbaren Visualisierungkomponenten.


## <a name="concepts"></a>Konzepte
Ansichten werden im Azure-Portal auf der Seite **Übersicht** Ihres Log Analytics-Arbeitsbereichs angezeigt. Die Kacheln in den einzelnen benutzerdefinierten Ansichten werden in alphabetischer Reihenfolge angezeigt, und die Kacheln für die Lösungen werden im gleichen Arbeitsbereich installiert.

![Seite „Übersicht“](media/view-designer/overview-page.png)

Die mit dem Ansicht-Designer erstellten Ansichten enthalten die in der folgenden Tabelle beschriebenen Elemente:

| Teil | BESCHREIBUNG |
|:--- |:--- |
| Kacheln | Werden auf der Seite **Übersicht** Ihres Log Analytics-Arbeitsbereichs angezeigt. In jeder Kachel wird eine visuelle Zusammenfassung der jeweils dargestellten benutzerdefinierten Ansicht angezeigt. Jeder Kacheltyp enthält eine unterschiedliche Visualisierung Ihrer Datensätze. Zum Anzeigen einer benutzerdefinierten Ansicht wählen Sie die entsprechende Kachel aus. |
| Benutzerdefinierte Ansicht | Wird angezeigt, wenn Sie eine Kachel auswählen. Jede Ansicht enthält eine oder mehrere Visualisierungskomponenten. |
| Visualisierungskomponenten | Stellen eine Visualisierung von Daten im Log Analytics-Arbeitsbereich basierend auf einem oder mehreren [Protokollsuchvorgängen](../../azure-monitor/log-query/log-query-overview.md) dar. Die meisten Komponenten weisen eine Kopfzeile mit einer allgemeinen Visualisierung und eine Liste mit den wichtigsten Ergebnissen auf. Die einzelnen Komponententypen enthalten unterschiedliche Visualisierungen der Datensätze im Log Analytics-Arbeitsbereich. Sie wählen Elemente in der Komponente aus, um einen Protokollsuchvorgang auszuführen, der detaillierte Datensätze bereitstellt. |


## <a name="work-with-an-existing-view"></a>Verwenden einer vorhandenen Ansicht
In Ansichten, die mit dem Ansicht-Designer erstellt wurden, werden die folgenden Optionen angezeigt:

![Menü „Übersicht“](media/view-designer/overview-menu.png)

Die Optionen sind in der folgenden Tabelle beschrieben:

| Option | BESCHREIBUNG |
|:--|:--|
| Aktualisieren   | Aktualisiert die Ansicht mit den neuesten Daten. | 
| Analytics | Öffnet das [Advanced Analytics-Portal](../../azure-monitor/log-query/portals.md) zum Analysieren von Daten mit Protokollabfragen. |
| Edit (Bearbeiten)       | Öffnet die Ansicht im Ansicht-Designer zum Bearbeiten der zugehörigen Inhalte und der Konfiguration.  |
| Klonen      | Erstellt eine neue Ansicht und öffnet sie im Ansicht-Designer. Der Name der neuen Ansicht entspricht dem ursprünglichen Namen, jedoch ist der Zusatz *Kopie* angefügt. |
| Datumsbereich | Legt einen Datums- und Uhrzeitfilterbereich für die in der Ansicht enthaltenen Daten fest. Dieser Datumsbereich wird vor allen Datumsbereichen angewendet, die in Abfragen in der Ansicht festgelegt werden.  |
| +          | Definieren Sie einen benutzerdefinierten, für die Ansicht definierten Filter. |


## <a name="create-a-new-view"></a>Erstellen einer neuen Ansicht
Sie können eine neue Ansicht im Ansicht-Designer erstellen, indem Sie im Menü des Log Analytics-Arbeitsbereichs **Ansicht-Designer** auswählen.

![Kachel für den Ansicht-Designer](media/view-designer/view-designer-tile.png)


## <a name="work-with-view-designer"></a>Verwenden des Ansicht-Designers
Mithilfe des Ansicht-Designers können Sie neue Ansichten erstellen oder vorhandene Ansichten bearbeiten. 

Der Ansicht-Designer verfügt über drei Bereiche: 
* **Entwurf**: Enthält die benutzerdefinierte Ansicht, die Sie erstellen oder bearbeiten. 
* **Steuerung**: Enthält die Kacheln und Komponenten, die Sie im Bereich **Entwurf** hinzufügen. 
* **Eigenschaften**: Zeigt die Eigenschaften der Kacheln oder ausgewählten Komponenten an.

![Ansicht-Designer](media/view-designer/view-designer-screenshot.png)

### <a name="configure-the-view-tile"></a>Konfigurieren der Kachel für die Ansicht
Eine benutzerdefinierte Ansicht kann nur über eine einzige Kachel verfügen. Wählen Sie die Registerkarte **Kachel** im Bereich **Steuerung** aus, um die aktuelle Kachel anzuzeigen oder eine andere auszuwählen. Im Bereich **Eigenschaften** werden die Eigenschaften der aktuellen Kachel angezeigt. 

Sie können die Kacheleigenschaften entsprechend den Informationen in der [Kachelreferenz](view-designer-tiles.md) konfigurieren und dann auf **Übernehmen** klicken, um die Änderungen zu speichern.

### <a name="configure-the-visualization-parts"></a>Konfigurieren der Visualisierungskomponenten
Eine Ansicht kann eine beliebige Anzahl von Visualisierungskomponenten enthalten. Um einer Ansicht Komponenten hinzuzufügen, wählen Sie die Registerkarte **Ansicht** und anschließend eine Visualisierungskomponente aus. Im Bereich **Eigenschaften** werden die Eigenschaften der ausgewählten Komponente angezeigt. 

Sie können die Ansichtseigenschaften entsprechend den Informationen in der [Referenz der Visualisierungskomponenten](view-designer-parts.md) konfigurieren und dann auf **Übernehmen** klicken, um die Änderungen zu speichern.

Ansichten weisen nur eine Zeile mit Visualisierungskomponenten auf. Sie können die vorhandenen Komponenten neu anordnen, indem Sie sie an eine neue Position ziehen.

Sie können eine Visualisierungskomponente aus der Ansicht entfernen, indem Sie das Symbol **X** rechts oben in der Komponente auswählen.


### <a name="menu-options"></a>Menüoptionen
Die Optionen zur Verwendung von Ansichten im Bearbeitungsmodus sind in der folgenden Tabelle beschrieben.

![Bearbeitungsmenü](media/view-designer/edit-menu.png)

| Option | BESCHREIBUNG |
|:--|:--|
| Speichern        | Speichert die Änderungen und schließt die Ansicht. |
| Abbrechen      | Verwirft die Änderungen und schließt die Ansicht. |
| Ansicht löschen | Löscht die Ansicht. |
| Export      | Exportiert die Ansicht in eine [Azure Resource Manager-Vorlage](../../azure-resource-manager/resource-group-authoring-templates.md), die Sie in einen anderen Arbeitsbereich importieren können. Der Name der Datei entspricht dem Namen der Ansicht mit der Erweiterung *omsview*. |
| Importieren      | Importiert die *omsview*-Datei, die Sie aus einem anderen Arbeitsbereich exportiert haben. Mit dieser Aktion wird die Konfiguration der vorhandenen Ansicht überschrieben. |
| Klonen       | Erstellt eine neue Ansicht und öffnet sie im Ansicht-Designer. Der Name der neuen Ansicht entspricht dem ursprünglichen Namen, jedoch ist der Zusatz *Kopie* angefügt. |

## <a name="next-steps"></a>Nächste Schritte
* Fügen Sie [Kacheln](view-designer-tiles.md) zu Ihrer benutzerdefinierten Ansicht hinzu.
* Fügen Sie Ihrer benutzerdefinierten Ansicht [Visualisierungskomponenten](view-designer-parts.md) hinzu.
