---
title: Tools für Apache Hive mit Data Lake (Apache Hadoop) für Visual Studio – Azure HDInsight
description: Erfahren Sie, wie Sie die Data Lake Tools für Visual Studio verwenden, um Apache Hive-Abfragen mit Apache Hadoop in Azure HDInsight auszuführen.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: ae2b06f266ef19d9558511284ba94c77cdca1955
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409682"
---
# <a name="run-apache-hive-queries-using-the-data-lake-tools-for-visual-studio"></a>Ausführen von Apache Hive-Abfragen mit Data Lake-Tools für Visual Studio

Erfahren Sie, wie Sie die Data Lake Tools für Visual Studio für Abfragen in Apache Hive verwenden. Mithilfe der Data Lake Tools können Sie Hive-Abfragen an Apache Hadoop in Azure HDInsight ganz einfach erstellen, übermitteln und überwachen.

## <a id="prereq"></a>Voraussetzungen

* Ein Azure HDInsight-Cluster (Apache Hadoop in HDInsight)

  > [!IMPORTANT]  
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Visual Studio (eine der folgenden Versionen):

    * Visual Studio 2013 Community/Professional/Premium/Ultimate mit Update 4

    * Visual Studio 2015 (beliebige Edition)

    * Visual Studio 2017 (beliebige Edition)

* HDInsight-Tools für Visual Studio oder Azure Data Lake-Tools für Visual Studio. Informationen zum Installieren und Konfigurieren der Tools finden Sie unter [Erste Schritte bei der Verwendung von Hadoop-Tools für Visual Studio für HDInsight](apache-hadoop-visual-studio-tools-get-started.md) .

## <a id="run"></a> Ausführen von Apache Hive-Abfragen mit Visual Studio

1. Öffnen Sie **Visual Studio**, wählen Sie **Neu** > **Projekt** > **Azure Data Lake** > **HIVE** > **Hive-Anwendung**. Geben Sie einen Namen für dieses Projekt an.

2. Öffnen Sie die Datei **Script.hql** , die mit diesem Projekt erstellt wird, und fügen Sie dann die folgenden HiveQL-Anweisungen ein:

   ```hiveql
   set hive.execution.engine=tez;
   DROP TABLE log4jLogs;
   CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
   STORED AS TEXTFILE LOCATION '/example/data/';
   SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   ```

    Diese Anweisungen führen die folgenden Aktionen aus:

   * `DROP TABLE`: Wenn die Tabelle vorhanden ist, wird sie von dieser Anweisung gelöscht.

   * `CREATE EXTERNAL TABLE`: Erstellt eine neue „externe“ Tabelle in Hive. Externe Tabellen speichern nur die Tabellendefinition in Hive. Die Daten verbleiben am ursprünglichen Speicherort.

     > [!NOTE]  
     > Externe Tabellen sollten Sie verwenden, wenn Sie erwarten, dass die zugrunde liegenden Daten aus einer externen Quelle aktualisiert werden. Beispielsweise einen MapReduce-Auftrag oder Azure-Dienst.
     >
     > Durch das Löschen einer externen Tabelle werden **nicht** die Daten, sondern nur die Tabellendefinitionen gelöscht.

   * `ROW FORMAT`: Teilt Hive mit, wie die Daten formatiert werden. In diesem Fall werden die Felder in den einzelnen Protokollen durch Leerzeichen getrennt.

   * `STORED AS TEXTFILE LOCATION`: Teilt Hive mit, dass die Daten im Verzeichnis „example/data“ als Text gespeichert sind.

   * `SELECT`: Wählt die Anzahl aller Zeilen aus, bei denen die Spalte `t4` den Wert `[ERROR]` enthält. Mit dieser Anweisung sollte der Wert `3` zurückgegeben werden, da dieser Wert in drei Zeilen enthalten ist.

   * `INPUT__FILE__NAME LIKE '%.log'`: Teilt Hive mit, dass nur Daten aus Dateien mit der Erweiterung „.log“ zurückgeben werden sollen. Diese Klausel schränkt die Suche auf die Datei „sample.log“ ein, die die Daten enthält.

3. Wählen Sie auf der Symbolleiste den **HDInsight-Cluster**, den Sie für diese Abfrage verwenden möchten. Wählen Sie **Übermitteln**, um die Anweisungen als Hive-Auftrag auszuführen.

   ![Leiste „Übermitteln“](./media/apache-hadoop-use-hive-visual-studio/toolbar.png)

4. Die **Hive-Auftragszusammenfassung** wird mit Informationen zum aktiven Auftrag angezeigt. Verwenden Sie den Link **Aktualisieren**, um die Auftragsinformationen zu aktualisieren, bis der **Auftragsstatus** zu **Abgeschlossen** wechselt.

   ![Auftragszusammenfassung, die einen abgeschlossenen Auftrag anzeigt](./media/apache-hadoop-use-hive-visual-studio/jobsummary.png)

5. Verwenden Sie den Link **Auftragsausgabe** , um die Ausgabe dieses Auftrags anzuzeigen. `[ERROR] 3` wird angezeigt – dies ist der Wert, der von dieser Abfrage zurückgegeben wird.

6. Sie können Hive-Abfragen auch ohne Erstellung eines Projekts ausführen. Erweitern Sie im **Server-Explorer** die Option **Azure** > **HDInsight**, klicken Sie dann mit der rechten Maustaste auf den HDInsight-Server, und wählen Sie anschließend **Hive-Abfrage schreiben** aus.

7. Fügen Sie im sich daraufhin öffnenden Dokument **temp.hql** die folgenden HiveQL-Anweisungen hinzu:

   ```hiveql
   set hive.execution.engine=tez;
   CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
   INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   ```

    Diese Anweisungen führen die folgenden Aktionen aus:

   * `CREATE TABLE IF NOT EXISTS`: Erstellt eine Tabelle, sofern diese noch nicht vorhanden ist. Da das `EXTERNAL`-Schlüsselwort nicht verwendet wird, erstellt diese Anweisung eine interne Tabelle. Interne Tabellen werden im Hive-Data Warehouse gespeichert und von Hive verwaltet.

     > [!NOTE]  
     > Anders als bei `EXTERNAL` Tabellen werden beim Löschen von internen Tabellen auch die zugrunde liegenden Daten gelöscht.

   * `STORED AS ORC`: Speichert die Daten im ORC-Format (Optimized Row Columnar). ORC ist ein stark optimiertes und effizientes Format zum Speichern von Hive-Daten.

   * `INSERT OVERWRITE ... SELECT`: Wählt die Zeilen aus der `log4jLogs` Tabelle, die `[ERROR]` enthalten, und fügt dann die Daten in die `errorLogs` Tabelle ein.

8. Wählen Sie auf der Symbolleiste **Übermitteln** aus, um den Auftrag auszuführen. Verwenden Sie **Auftragsstatus** , um zu ermitteln, ob der Auftrag erfolgreich abgeschlossen wurde.

9. Verwenden Sie den **Server-Explorer**, und erweitern Sie **Azure** > **HDInsight** > Ihren HDInsight-Cluster > **Hive-Datenbanken** > **Standard**, um zu überprüfen, ob der Auftrag die Tabelle erstellt hat. Die Tabellen **errorLogs** und **log4jLogs** werden aufgelistet.

## <a id="nextsteps"></a>Nächste Schritte

Wie Sie sehen, können Sie mit den HDInsight-Tools für Visual Studio mühelos mit Hive-Abfragen in HDInsight arbeiten.

Allgemeine Informationen zu Hive in HDInsight:

* [Verwenden von Apache Hive mit Apache Hadoop in HDInsight](hdinsight-use-hive.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Apache Pig mit Apache Hadoop in HDInsight](hdinsight-use-pig.md)

* [Verwenden von MapReduce mit Apache Hadoop in HDInsight](hdinsight-use-mapreduce.md)

Weitere Informationen zu den HDInsight Tools für Visual Studio:

* [Erste Schritte mit HDInsight Tools für Visual Studio](apache-hadoop-visual-studio-tools-get-started.md)

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[apache-tez]: https://tez.apache.org
[apache-hive]: https://hive.apache.org/
[apache-log4j]: https://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: https://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: https://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
