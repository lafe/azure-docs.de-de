---
title: Zuordnungsdatenfluss in Azure Data Factory – Schemaabweichung
description: Erstellen robuster Datenflüsse in Azure Data Factory mit Schemaabweichung
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: 5799386b3ed725c610569caae8dfc72796c654b9
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56212712"
---
# <a name="azure-data-factory-mapping-data-flow-schema-drift"></a>Zuordnungsdatenfluss in Azure Data Factory – Schemaabweichung

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Das Konzept der Schemaabweichung ist der Fall, wenn Ihre Quellen häufig Metadaten ändern. Felder, Spalten, Typen usw. können schnell hinzugefügt, entfernt oder geändert werden. Ohne Behandlung der Schemaabweichung wird Ihr Datenfluss anfällig für Änderungen in Upstream-Datenquellen. Wenn sich Eingangsspalten und -felder ändern, treten bei typischen ETL-Mustern Fehler auf, da sie dazu neigen, an diesen Quellennamen gebunden zu werden.

Zum Schutz gegen Schemaabweichung müssen in einem Datenfluss-Tool Funktionen enthalten sein, die Ihnen als Data Engineer Folgendes ermöglichen:

* Definieren von Quellen mit änderbaren Feldnamen, Datentypen, Werte und Größen
* Definieren von Transformationsparametern, die anstelle von hartcodierten Feldern und Werten mit Datenmustern arbeiten können
* Definieren von Ausdrücken, die Muster verstehen, um sie mit Eingangsfeldern abzugleichen, anstatt benannte Felder zu verwenden

Im Datenfluss von Azure Data Factory werden diese Funktionen über diesen Workflow ausgeführt:

* Wählen Sie „Allow Schema Drift (Schemaabweichung zulassen)“ in Ihrer Quellentransformation aus.

<img src="media/data-flow/schemadrift001.png" width="400">

* Wenn Sie diese Option ausgewählt haben, werden alle Eingangsfelder bei jeder Datenflussausführung aus der Quelle gelesen und über den gesamten Fluss der Senke übergeben.

* Stellen Sie sicher, dass Sie „Auto-Map (Automatische Zuordnung)“ verwenden, um alle neuen Felder in der Senkentransformation zuzuordnen, sodass alle neuen Felder abgerufen werden und in Ihr Ziel gelangen:

<img src="media/data-flow/automap.png" width="400">

* Alles funktioniert, wenn neue Felder in diesem Szenario mit einer einfachen Zuordnung von Quelle -> Senke (auch bekannt als „Kopieren“) eingeführt werden.

* Um in diesem Workflow Transformationen hinzuzufügen, die die Schemaabweichung behandeln, können Sie den Musterabgleich zum Abgleich mit Spalten nach Name, Typ und Wert durchführen.

* Klicken Sie in der „Derived Column (Abgeleitete Spalte)“ oder der Transformation „Aggregate (Aggregieren)“ auf „Add Column Pattern (Spaltenmuster hinzufügen)“, wenn Sie eine Transformation erstellen möchten, die „Schema Drift (Schemaabweichung)“ versteht.

<img src="media/data-flow/columnpattern.png" width="400">

> [!NOTE]
> Sie müssen eine architektonische Entscheidung in Ihrem Datenfluss treffen, um die Schemaabweichung in Ihrem Fluss zu akzeptieren. Wenn Sie dies tun, können Sie Schutz gegen Schemaänderungen aus den Quellen bieten. Allerdings verlieren Sie die frühe Bindung Ihrer Spalten und Typen im gesamten Datenfluss. Azure Data Factory behandelt Schemaabweichungsflüsse als Flüsse mit später Bindung, sodass die Spaltennamen bei der Erstellung Ihrer Transformationen Ihnen nicht im gesamten Fluss in den Schemaansichten zur Verfügung stehen werden.

<img src="media/data-flow/taxidrift1.png" width="400">

Im Beispieldatenfluss des Taxi Demos ist eine Beispielschemaabweichung im unteren Datenfluss mit der TripFare-Quelle enthalten. Beachten Sie bei der Transformation „Aggregate (Aggregieren)“, dass wir den „Spaltenmuster“-Entwurf für die Aggregationsfelder verwenden. Anstatt bestimmte Spalten zu benennen oder anhand der Position nach Spalten zu suchen, gehen wir davon aus, dass die Daten sich ändern können und möglicherweise zwischen den Ausführungen nicht in der gleichen Reihenfolge angezeigt werden.

In diesem Beispiel der Behandlung der Schemaabweichung des Azure Data Factory-Datenflusses haben wir eine Aggregation erstellt, die nach Spalten vom Typ „double“ sucht, wissend, dass die Datendomäne Preise für jede Fahrt enthält. Wir können dann eine alle double-Felder in der Quelle umfassende aggregierte mathematische Berechnung ausführen, unabhängig davon, wo die Spalte landet, und unabhängig von ihrer Benennung.

Die Datenflusssyntax von Azure Data Factory stellt mit „$$“ die einzelnen mit Ihrem Abgleichsmuster übereinstimmenden Spalten dar. Sie können auch mit komplexen Funktionen für Zeichenfolgensuche und reguläre Ausdrücke einen Abgleich auf Spaltennamen durchführen. In diesem Fall erstellen wir einen neuen aggregierten Feldnamen basierend auf jeder Übereinstimmung eines „double“-Typs einer Spalte und fügen den Text ```_total``` jedem dieser übereinstimmenden Namen an: 

```concat($$, '_total')```

Dann runden und summieren wir die Werte für jede dieser übereinstimmenden Spalten:

```round(sum ($$))```

Sie können dies mit dem Azure Data Factory-Datenflussbeispiel „Taxi Demo“ testen. Schalten Sie die Debug-Sitzung mithilfe des Debug-Schalters am oberen Rand der Datenfluss-Entwurfsoberfläche ein, sodass Sie interaktiv die Ergebnisse sehen können:

<img src="media/data-flow/taxidrift2.png" width="800">

