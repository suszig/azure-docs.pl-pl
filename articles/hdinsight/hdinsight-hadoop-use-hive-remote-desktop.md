<properties
   pageTitle="HDInsight での Hadoop Hive と Remote Desktop の使用 | Microsoft Azure"
   description="リモート デスクトップを使用して HDInsight で Hadoop クラスターに接続し、Hive コマンド ライン インターフェイスを使用して Hive クエリを実行する方法について説明します。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="12/04/2015"
   ms.author="larryfr"/>

# リモート デスクトップによる HDInsight での Hive と Hadoop の使用

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

この記事では、リモート デスクトップを使用して HDInsight クラスターに接続し、Hive コマンド ライン インターフェイス (CLI) を使用して Hive クエリを実行する方法について説明します。

> [AZURE.NOTE] このドキュメントには、例で使用される HiveQL ステートメントの実行の詳細な説明は行いません。 この例で使用される HiveQL については「[HDInsight での Hive と Hadoop の使用](hdinsight-use-hive.md)」をご覧ください。

##<a id="prereq"></a>前提条件

この記事の手順を完了するには、次のものが必要です。

* Windows ベースの HDInsight (HDInsight で Hadoop を使用) クラスター

* Windows 10、Window 8、Windows 7 を実行するクライアント コンピューター

##<a id="connect"></a>リモート デスクトップへの接続

「[RDP を使用した HDInsight クラスターへの接続](hdinsight-administer-use-management-portal.md#rdp)」の手順に従って、HDInsight クラスターのリモート デスクトップを有効にしてからデスクトップに接続します。

##<a id="hive"></a>Hive コマンドの使用

HDInsight クラスターのデスクトップに接続したら、次の手順に従って Hive を使用します。

1. HDInsight デスクトップから開始、 **Hadoop コマンド ライン**します。

2. 次のコマンドを入力して、Hive CLI を起動します。

        %hive_home%\bin\hive

    CLI が起動すると、Hive CLI プロンプトの`hive>`  が表示されます。

3. という名前の新しいテーブルを作成するには、次の文を入力して、CLI を使用して **log4jLogs** サンプル データを使用します。

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    これらのステートメントは次のアクションを実行します。

    * **DROP TABLE**: テーブルが既に存在する場合は、テーブルとデータ ファイルを削除します。

    * **CREATE EXTERNAL TABLE**: Hive に新しく '外部' テーブルを作成します。 外部テーブルは、Hive にテーブル定義のみを格納し、データは、元の場所に残します。

        > [AZURE.NOTE] (データの自動アップロード処理など)、外部ソースから、または別の MapReduce 操作によって更新する基になるデータが予想される場合、外部テーブルを使用する必要がありますが、常に最新のデータを使用するための Hive クエリをします。
        >
        > 外部テーブルを削除しても **いない** データ、テーブル定義のみを削除します。

    * **ROW FORMAT**: Hive にデータの形式です。 ここでは、各ログのフィールドは、スペースで区切られています。

    * **STORED AS TEXTFILE LOCATION**: (example/data ディレクトリ) が格納されている Hive にデータを格納し、テキストとして格納されています。

    * **選択**: すべての行の数を選択、列 **t4** 値を含む **[ERROR]**します。 これには、値が返さ **3** がこの値を含む 3 行あるためです。

    * **INPUT__FILE__NAME のように '%.log'** -Hive にだけで終わるファイルからデータを返すする必要があります。 ログです。 これにより、検索はデータを含む sample.log ファイルに制限され、定義したスキーマに一致しない他のサンプル データ ファイルのデータを返すことができなくなります。


4. という名前の新しい「内部」テーブルを作成する次のステートメントを使用して **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    これらのステートメントは次のアクションを実行します。

    * **テーブル IF NOT EXISTS の作成**: が既に存在しない場合は、テーブルを作成します。  **外部** キーワードが使用されていない、これは内部テーブルであり、Hive データ ウェアハウスに格納され、Hive によって完全に管理されます。

        > [AZURE.NOTE] 異なり **外部** も内部テーブルを削除してテーブルを基になるデータを削除します。

    * **STORED AS ORC**: optimized row columnar (ORC) 形式でデータを格納します。 この形式は、Hive にデータを格納するための、非常に効率的で適切な形式です。

    * **INSERT OVERWRITE ...選択**: 行を選択、 **log4jLogs** がテーブル **[ERROR]**, へのデータを挿入、 **errorLogs** テーブルです。

    のみを含む列を確認する **[ERROR]** 列に格納された、t4 に、 **errorLogs** テーブルで、次のステートメントを使用して、すべての行を返します **errorLogs**:

        SELECT * from errorLogs;

    3 行のデータを返す、すべてを含む **[ERROR]** 各行の t4 列にします。

##<a id="summary"></a>概要

このように、Hive コマンドを使用すると、HDInsight クラスターで簡単に対話的に Hive クエリを実行し、ジョブ ステータスを監視し、出力を取得できます。

##<a id="nextsteps"></a>次のステップ

HDInsight での Hive に関する全般的な情報

* [HDInsight での Hive と Hadoop の使用](hdinsight-use-hive.md)

HDInsight での Hadoop のその他の使用方法に関する情報

* [HDInsight での Pig と Hadoop の使用](hdinsight-use-pig.md)

* [HDInsight での MapReduce と Hadoop の使用](hdinsight-use-mapreduce.md)


[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx



