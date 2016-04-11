<properties
   pageTitle="Hadoop tools for Visual Studio での Hive クエリ | Microsoft Azure"
   description="Visual Studio の Hadoop ツールを使用して HDInsight の Hadoop で Hive を使用する方法について説明します。"
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

#Visual Studio の HDInsight ツールを使用して Hive クエリを実行

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

この記事では、Visual Studio の HDInsight ツールを使用して、Hive クエリを HDInsight クラスターに送信する方法を説明します。

> [AZURE.NOTE] このドキュメントには、例で使用される HiveQL ステートメントの実行の詳細な説明は行いません。 この例で使用される HiveQL については「[HDInsight での Hive と Hadoop の使用](hdinsight-use-hive.md)」をご覧ください。

##<a id="prereq"></a>前提条件

この記事の手順を完了するには、次のものが必要です。

* Azure HDInsight (HDInsight での Hadoop) クラスター (Linux または Windows ベース)

* Visual Studio 2012 [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)、Visual Studio 2013 [Update 3](http://go.microsoft.com/fwlink/?LinkId=390465)、[Visual Studio Express 2013](http://www.microsoft.com/download/details.aspx?id=40769)

##<a id="run"></a> Visual Studio の HDInsight ツールを使用して Hive クエリを実行

1. 開いている **Visual Studio** 選択 **新規** > **プロジェクト** > **HDInsight** > **Hive アプリケーション**します。 プロジェクトの名前を指定します。

2. 開いている、 **Script.hql** このプロジェクト、および次の HiveQL ステートメントでの貼り付けで作成されるファイル。

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
    * **選択**: すべての行の数を選択して、列 **t4** 値が含まれて **[ERROR]**します。 これには、値が返さ **3** がこの値を含む 3 行あるためです。
    * **INPUT__FILE__NAME のように '%.log'** -Hive にだけで終わるファイルからデータを返すする必要があります。 ログです。 これにより、検索はデータを含む sample.log ファイルに制限され、定義したスキーマに一致しない他のサンプル データ ファイルのデータを返すことができなくなります。

3. ツールバーで、選択、 **HDInsight クラスター** このクエリに使用し、選択する **送信** 、Hive ジョブとしてステートメントを実行します。  **Hive ジョブの概要** が表示され、実行中のジョブに関する情報を表示します。 使用して、 **更新** まで、ジョブの情報を更新へのリンク、 **ジョブの状態** に変更 **完了**します。

4. 使用して、 **ジョブの出力** このジョブの出力を表示するリンク。 SELECT ステートメントによって返された値「`[ERROR] 3`」が表示されます。

5. プロジェクトを作成せずに Hive クエリを実行することもできます。 使用して **サーバー エクスプ ローラー**, 、展開 **Azure** > **HDInsight**, 、HDInsight サーバーを右クリックし、[ **Write a Hive Query**します。

6.  **Temp.hql** ドキュメントが表示されたらには、次の HiveQL ステートメントを追加します。

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    これらのステートメントは次のアクションを実行します。

    * **テーブル IF NOT EXISTS の作成**: が既に存在しない場合は、テーブルを作成します。  **外部** キーワードが使用されていない、これは内部テーブルであり、Hive データ ウェアハウスに格納され、Hive によって完全に管理されます。

        > [AZURE.NOTE] 異なり **外部** も内部テーブルを削除してテーブルを基になるデータを削除します。

    * **STORED AS ORC**: optimized row columnar (ORC) 形式でデータを格納します。 この形式は、Hive にデータを格納するための、非常に効率的で適切な形式です。
    * **INSERT OVERWRITE ...選択**: 行を選択、 **log4jLogs** がテーブル **[ERROR]**, へのデータを挿入、 **errorLogs** テーブルです。

7. ツールバーで、次のように選択します。 **送信** ジョブを実行します。 使用して、 **ジョブの状態** をジョブが正常に完了したことを確認します。

8. ジョブが完了し、新しいテーブルが作成されたことを確認するため **サーバー エクスプ ローラー** 展開 **Azure** > **HDInsight** >、HDInsight クラスター > **Hive データベース** > と **既定**します。 確認する必要があります、 **errorLogs** テーブルおよび **log4jLogs** テーブルです。

##<a id="summary"></a>概要

このように、Visual Studio の HDInsight ツールを使用すると、HDInsight クラスターで簡単に Hive クエリを実行し、ジョブ ステータスを監視し、出力を取得できます。

##<a id="nextsteps"></a>次のステップ

HDInsight での Hive に関する全般的な情報

* [HDInsight での Hive と Hadoop の使用](hdinsight-use-hive.md)

HDInsight での Hadoop のその他の使用方法に関する情報

* [HDInsight での Pig と Hadoop の使用](hdinsight-use-pig.md)

* [HDInsight での MapReduce と Hadoop の使用](hdinsight-use-mapreduce.md)

Visual Studio の HDInsight ツールに関する詳細情報:

* [Visual Studio の HDInsight ツールの概要](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)


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



[hdinsight-storage]: hdinsight-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png

