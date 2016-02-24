<properties
   pageTitle="HDInsight のクエリ コンソールでの Hadoop Hive の使用 | Microsoft Azure"
   description="Web ベースのクエリ コンソールを使用して、お使いのブラウザーから HDInsight Hadoop クラスターで Hive クエリを実行する方法について説明します。"
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

# クエリ コンソールを使用して Hive クエリを実行

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

この記事では、HDInsight クエリ コンソールを使用して、お使いのブラウザーから HDInsight Hadoop クラスターで Hive クエリを実行する方法について説明します。

> [AZURE.NOTE] クエリ コンソールでは、Windows ベースの HDInsight クラスターで利用可能なだけです。


##<a id="prereq"></a>前提条件

この記事の手順を完了するには、次のものが必要です。

* Windows ベースの HDInsight Hadoop クラスター

* 最新の Web ブラウザー

##<a id="run"></a> クエリ コンソールを使用して Hive クエリを実行

1. Web ブラウザーを開きに移動 __https://CLUSTERNAME.azurehdinsight.net__, ここで、 __CLUSTERNAME__ 、HDInsight クラスターの名前を指定します。 プロンプトが表示されたら、クラスターの作成時に使用したユーザー名とパスワードを入力します。


2. ページの上部にあるリンクから選択 **Hive エディター**します。 HDInsight クラスターで実行する HiveQL ステートメントの入力に使用できるフォームが表示されます。

    ![Hive エディター](./media/hdinsight-hadoop-use-hive-query-console/queryconsole.png)

    テキスト `Select * from hivesampletable` を次の HiveQL ステートメントに置き換えます。

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

2. クリックして **送信**します。  **ジョブ セッション** 、ページの下部でジョブの詳細を表示する必要があります。

3. ときに、 **ステータス** ] フィールドに変更 **完了**, [ **の詳細を表示** ジョブのです。 [詳細] ページ、 **ジョブの出力** を含む `[ERROR]   3`します。 使用することができます、 **ダウンロード** ジョブの出力を含むファイルをダウンロードするには、このフィールドの下のボタンをクリックします。


##<a id="summary"></a>概要

このように、クエリ コンソールを使用すると、HDInsight クラスターで簡単に Hive クエリを実行し、ジョブ ステータスを監視し、出力を取得できます。

Hive クエリ コンソールを使用して Hive ジョブを実行する方法の詳細については、次のように選択します。 **作業の開始** クエリ コンソールの上部に用意されているサンプルを使用しています。 各サンプルでは順を追って Hive を使用してデータを分析します。また、サンプルで使用されている HiveQL ステートメントについての説明も含まれています。

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



[hdinsight-storage]: hdinsight-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-get-started.md

[Powershell-install-configure]: install-configure-powershell.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png

