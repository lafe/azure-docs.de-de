---
title: 'Verwenden von Apache Beeline mit Apache Hive: Azure HDInsight'
description: Erfahren Sie, wie Sie den Beeline-Client verwenden, um Hive-Abfragen mit Hadoop in HDInsight auszuführen. Beeline ist ein Dienstprogramm zum Arbeiten mit HiveServer2 über JDBC.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
keywords: Beeline-Hive, Hive, Beeline
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: hrasheed
ms.openlocfilehash: 23fa146b7bdaef0451984d0fbc638c57691cf259
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56201719"
---
# <a name="use-the-apache-beeline-client-with-apache-hive"></a>Verwenden des Apache Beeline-Clients mit Apache Hive

Hier erfahren Sie, wie Sie [Apache Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) verwenden, um Hive-Abfragen unter HDInsight auszuführen.

Beeline ist ein Hive-Client, der auf den Hauptknoten des HDInsight-Clusters enthalten ist. Beeline verwendet JDBC, um eine Verbindung mit HiveServer2 herzustellen, einem Dienst, der in Ihrem HDInsight-Cluster gehostet wird. Mit Beeline können Sie auch remote über das Internet auf Hive unter HDInsight zugreifen. Die folgenden Beispiele zeigen die am häufigsten verwendeten Verbindungszeichenfolgen zum Herstellen einer Verbindung mit HDInsight aus Beeline:

* __Verwenden von Beeline aus einer SSH-Verbindung mit einem Hauptknoten oder Edgeknoten__: `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'`
* __Verwenden von Beeline auf einem Client, Herstellen der Verbindung mit HDInsight über ein virtuelles Azure-Netzwerk__:`-u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'`
* __Verwenden von Beeline auf einem Client, Herstellen der Verbindung mit einem HDInsight ESP-Cluster (Enterprise-Sicherheitspaket) über ein virtuelles Azure-Netzwerk__: `-u 'jdbc:hive2://<headnode-FQDN>:10001/default;principal=hive/_HOST@<AAD-Domain>;auth-kerberos;transportMode=http' -n <username>`
* __Verwenden von Beeline auf einem Client, Herstellen der Verbindung mit HDInsight über das öffentliche Internet__:`-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password`

> [!NOTE]  
> Ersetzen Sie `admin` durch das Anmeldekonto für Ihren Cluster.
>
> Ersetzen Sie `password` durch das Kennwort des Anmeldekontos für den Cluster.
>
> Ersetzen Sie `clustername` durch den Namen Ihres HDInsight-Clusters.
>
> Ersetzen Sie bei der Verbindung mit dem Cluster über ein virtuelles Netzwerk `<headnode-FQDN>` durch den vollqualifizierten Domänennamen eines Clusterhauptknotens.
>
> Ersetzen Sie bei der Verbindung mit einem ESP-Cluster (Enterprise-Sicherheitspaket) `<AAD-Domain>` durch den Namen der Azure Active Directory-Instanz (AAD), mit der der Cluster verknüpft ist. Ersetzen Sie `<username>` durch den Namen eines Kontos in der Domäne mit der Berechtigung für den Zugriff auf den Cluster.

## <a id="prereq"></a>Voraussetzungen

* Ein Linux-basierter Hadoop-Cluster in HDInsight, Version 3.4 oder höher.

  > [!IMPORTANT]  
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Ein SSH-Client oder ein lokaler Beeline-Client. In den meisten Schritten in diesem Dokument wird davon ausgegangen, dass Sie Beeline aus einer SSH-Sitzung für den Cluster verwenden. Informationen zum Ausführen von Beeline von außerhalb des Clusters finden Sie im Abschnitt zur [Remoteverwendung von Beeline](#remote).

    Weitere Informationen zur Verwendung von SSH finden Sie unter [Verwenden von SSH mit HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="beeline"></a>Ausführen einer Hive-Abfrage

1. Wenn Sie Beeline starten, müssen Sie eine Verbindungszeichenfolge für HiveServer2 in Ihrem HDInsight-Cluster bereitstellen:

    * Wenn Sie eine Verbindung über das öffentliche Internet herstellen möchten, müssen Sie den Kontonamen für die Clusteranmeldung (Standard ist `admin`) und das Kennwort angeben. Beispiel: Verwenden von Beeline von einem Clientsystem zum Herstellen einer Verbindung mit der `<clustername>.azurehdinsight.net`-Adresse. Diese Verbindung erfolgt über Port `443` und wird mithilfe von SSL verschlüsselt:

        ```bash
        beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password
        ```

    * Beim Herstellen einer Verbindung aus einer SSH-Sitzung mit einem Clusterhauptknoten können Sie eine Verbindung mit der `headnodehost`-Adresse an Port `10001` herstellen:

        ```bash
        beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
        ```

    * Beim Herstellen einer Verbindung über ein virtuelles Azure-Netzwerk müssen Sie den vollqualifizierten Domänennamen (FQDN) des Clusterhauptknotens angeben. Da diese Verbindung direkt mit den Clusterknoten hergestellt wird, wird für die Verbindung Port `10001` verwendet:

        ```bash
        beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
        ```
    * Beim Herstellen einer Verbindung mit einem mit Azure Active Directory (AAD) verknüpften ESP-Cluster (Enterprise-Sicherheitspaket) müssen Sie auch den Domänennamen `<AAD-Domain>` und den Namen eines Domänenbenutzerkontos mit der Berechtigung für den Zugriff auf den Cluster `<username>` angeben:
        
        ```bash
        kinit <username>
        beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/default;principal=hive/_HOST@<AAD-Domain>;auth-kerberos;transportMode=http' -n <username>
        ```

2. Beeline-Befehle beginnen mit dem Zeichen `!`, z.B. `!help` zum Anzeigen der Hilfe. Jedoch kann `!` bei einigen Befehlen ausgelassen werden. `help` funktioniert beispielsweise auch.

    Es gibt ein `!sql`, das zum Ausführen von HiveQL-Anweisungen verwendet wird. HiveQL wird aber so häufig genutzt, dass Sie das vorangestellte `!sql` weglassen können. Die folgenden beiden Anweisungen sind gleichwertig:

    ```hiveql
    !sql show tables;
    show tables;
    ```

    In einem neuen Cluster wird nur eine Tabelle aufgelistet: **hivesampletable**.

3. Verwenden Sie folgenden Befehl, um das Schema für „hivesampletable“ anzuzeigen:

    ```hiveql
    describe hivesampletable;
    ```

    Dieser Befehl gibt folgende Information zurück:

        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Diese Information beschreibt die Spalten in der Tabelle.

4. Geben Sie die folgenden Anweisungen ein, um eine Tabelle mit dem Namen **log4jLogs** zu erstellen, indem Sie die über das HDInsight-Cluster bereitgestellten Beispieldaten verwenden:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (
        t1 string,
        t2 string,
        t3 string,
        t4 string,
        t5 string,
        t6 string,
        t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs 
        WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' 
        GROUP BY t4;
    ```

    Diese Anweisungen führen die folgenden Aktionen aus:

    * `DROP TABLE`: Wenn die Tabelle vorhanden ist, wird sie gelöscht.

    * `CREATE EXTERNAL TABLE`: Erstellt eine **externe** Tabelle in Hive. Externe Tabellen speichern nur die Tabellendefinition in Hive. Die Daten verbleiben an ihrem ursprünglichen Speicherort.

    * `ROW FORMAT`: Gibt an, wie die Daten formatiert werden. In diesem Fall werden die Felder in den einzelnen Protokollen durch Leerzeichen getrennt.

    * `STORED AS TEXTFILE LOCATION`: Wo die Daten gespeichert werden und in welchem Dateiformat.

    * `SELECT`: Wählt die Anzahl aller Zeilen aus, bei denen die Spalte **t4** den Wert **[ERROR]** enthält. Diese Abfrage gibt den Wert **3** zurück, da dieser Wert in drei Zeilen enthalten ist.

    * `INPUT__FILE__NAME LIKE '%.log'`: Hive versucht, das Schema auf alle Dateien im Verzeichnis anzuwenden. In diesem Fall enthält das Verzeichnis Dateien, die dem Schema nicht entsprechen. Um überflüssige Daten in den Ergebnissen zu vermeiden, weist diese Anweisung Hive an, nur Daten aus Dateien zurückzugeben, die auf „.log“ enden.

  > [!NOTE]  
  > Externe Tabellen sollten Sie verwenden, wenn Sie erwarten, dass die zugrunde liegenden Daten aus einer externen Quelle aktualisiert werden. Das könnte z.B. ein automatisierter Datenupload oder ein MapReduce-Vorgang sein.
  >
  > Durch das Löschen einer externen Tabelle werden **nicht** die Daten, sondern nur die Tabellendefinitionen gelöscht.

    Die Ausgabe dieses Befehls ähnelt dem folgenden Text:

        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :

        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)

        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

5. Verwenden Sie `!exit`, um Beeline zu beenden.

### <a id="file"></a>Ausführen einer HiveQL-Datei mithilfe von Beeline

Verwenden Sie die folgenden Schritte, um eine Datei zu erstellen und sie dann mit Beeline auszuführen.

1. Verwenden Sie den folgenden Befehl, um eine Datei mit dem Namen **query.hql**zu erstellen:

    ```bash
    nano query.hql
    ```

2. Verwenden Sie als Inhalt der Datei den folgenden Text. Diese Abfrage erstellt eine neue „interne“ Tabelle mit dem Namen **errorLogs**:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Diese Anweisungen führen die folgenden Aktionen aus:

    * **CREATE TABLE IF NOT EXISTS**: Wenn die Tabelle noch nicht vorhanden ist, wird sie erstellt. Da das **EXTERNAL**-Schlüsselwort nicht verwendet wird, erstellt diese Anweisung eine interne Tabelle. Interne Tabellen werden im Hive-Data Warehouse gespeichert und vollständig von Hive verwaltet.
    * **ALS ORC GESPEICHERT** – Speichert die Daten im ORC-Format (Optimized Row Columnar). ORC ist ein stark optimiertes und effizientes Format zum Speichern von Hive-Daten.
    * **ÜBERSCHREIBEN EINFÜGEN ... SELECT**: Wählt die Zeilen aus der Tabelle **log4jLogs** aus, die den Wert **[ERROR]** enthalten. Dann werden die Daten in die Tabelle **errorLogs** eingefügt.

    > [!NOTE]  
    > Anders als bei externen Tabellen werden beim Löschen von internen Tabellen auch die zugrunde liegenden Daten gelöscht.

3. Verwenden Sie **STRG**+**_X**, um die Datei zu speichern. Geben Sie dann **Y** ein, und drücken Sie die **EINGABETASTE**.

4. Verwenden Sie Folgendes, um die Datei mit Beeline auszuführen:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]  
    > Der `-i`-Parameter startet Beeline und führt die Anweisungen in der Datei `query.hql` aus. Nach Abschluss der Abfrage wird die Eingabeaufforderung `jdbc:hive2://headnodehost:10001/>` angezeigt. Sie können eine Datei auch mit dem `-f`-Parameter ausführen, der Beeline beendet, nachdem die Abfrage abgeschlossen ist.

5. Um zu überprüfen, ob die Tabelle **errorLogs** erstellt wurde, verwenden Sie die folgende Anweisung, um alle Zeilen aus **errorLogs** zurückzugeben:

    ```hiveql
    SELECT * from errorLogs;
    ```

    Es sollten drei Datenzeilen zurückgegeben werden, die alle in Spalte „t4“ den Wert **[FEHLER]** enthalten:

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a id="remote"></a>Remoteverwendung von Beeline

Wenn Sie Beeline lokal installiert haben und eine Verbindung über das öffentliche Internet herstellen, verwenden Sie die folgenden Parameter:

* __Verbindungszeichenfolge__: `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`

* __Clusteranmeldename__: `-n admin`

* __Clusteranmeldekennwort__: `-p 'password'`

Ersetzen Sie `clustername` in der Verbindungszeichenfolge durch den Namen des HDInsight-Clusters.

Ersetzen Sie `admin` durch Ihren Clusteranmeldenamen und `password` durch das zugehörige Kennwort.

Wenn Sie Beeline lokal installiert haben und eine Verbindung über ein virtuelles Azure-Netzwerk herstellen, verwenden Sie die folgenden Parameter:

* __Verbindungszeichenfolge__: `-u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'`

Verwenden Sie die Informationen im Dokument [Verwalten von HDInsight mithilfe der Apache Ambari-REST-API](../hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes), um den vollqualifizierten Domänennamen eines Hauptknotens zu ermitteln.

## <a id="sparksql"></a>Verwenden von Beeline mit Apache Spark

Apache Spark stellt eine eigene Implementierung von HiveServer2 bereit, die manchmal als Spark Thrift-Server bezeichnet wird. Bei diesem Dienst wird Spark SQL anstelle von Hive zum Auflösen von Abfragen verwendet und ermöglicht je nach Abfrage ggf. eine bessere Leistung.

Die __Verbindungszeichenfolge__, die beim Herstellen einer Verbindung über das Internet verwendet wird, weicht geringfügig ab. Sie enthält `httpPath/sparkhive2` anstelle von `httpPath=/hive2`. Nachfolgend sehen Sie ein Beispiel zum Herstellen einer Verbindung über das Internet:

```bash 
beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/sparkhive2' -n admin -p password
```

Wenn Sie direkt vom Clusterhauptknoten oder von einer Ressource, die sich in der gleichen Azure Virtual Network-Instanz wie der HDInsight-Cluster befindet, eine Verbindung herstellen, muss für den Spark Thrift-Server Port `10002` anstelle von Port `10001` verwendet werden. Nachfolgend sehen Sie ein Beispiel zum direkten Herstellen einer Verbindung mit dem Hauptknoten:

```bash
beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'
```

## <a id="summary"></a><a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu Hive in HDInsight finden Sie im folgenden Artikel:

* [Verwenden von Apache Hive mit Apache Hadoop in HDInsight](hdinsight-use-hive.md)

Weitere Informationen zu anderen Methoden zur Verwendung von Hadoop in HDInsight finden Sie in den folgenden Artikeln:

* [Verwenden von Apache Pig mit Apache Hadoop in HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit Apache Hadoop in HDInsight](hdinsight-use-mapreduce.md)

Wenn Sie Tez mit Hive verwenden, lesen Sie folgendes Dokument: [Verwenden der Apache Ambari-Tez-Ansicht in Linux-basiertem HDInsight](../hdinsight-debug-ambari-tez-view.md)

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

[putty]: https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: https://technet.microsoft.com/library/ee692792.aspx
