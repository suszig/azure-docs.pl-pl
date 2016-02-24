<properties
   pageTitle="HDInsight (Hadoop) での Hive シェルの使用 | Microsoft Azure"
   description="Linux ベースの HDInsight クラスターで Hive シェルを使用する方法について説明します。 SSh を使用して HDInsight クラスターに接続してから、Hive シェルを使用して対話的にクエリを実行する方法について説明します。"
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

#SSH による HDInsight での Hive と Hadoop の使用

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

この記事では、Secure Shell (SSH) を使用して Azure HDInsight クラスターで Hadoop に接続してから、Hive コマンド ライン インターフェイス (CLI) を使用して Hive クエリを対話的に実行する方法について説明します。

> [AZURE.IMPORTANT] Hive コマンドは、Linux ベースの HDInsight クラスターで利用可能な Beeline の使用を検討してください。 Beeline は Hive を使用するための最新クライアントであり、HDInsight クラスターに付属します。 これを使用する方法については、次を参照してください。 [Beeline を使用した HDInsight での Hadoop での Hive の使用](hdinsight-hadoop-use-hive-beeline.md)します。

##<a id="prereq"></a>前提条件

この記事の手順を完了するには、次のものが必要です。

* HDInsight クラスターでの Linux ベースの Hadoop

* SSH クライアント SSH クライアントを備えた Linux、Unix、および Mac OS Windows ユーザーは [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) などのクライアントをダウンロードする必要があります。

##<a id="ssh"></a>SSH を使用した接続

SSH コマンドを使用して、HDInsight クラスターの完全修飾ドメイン名 (FQDN) に接続します。 FQDN が、クラスターを指定した名前になります **. >.azurehdinsight.net**します。 次の例、という名前のクラスターに接続 **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**SSH 認証に証明書キーを指定した場合** HDInsight クラスターを作成したときに、クライアント システム上の秘密キーの場所を指定する必要があります。

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**SSH 認証のパスワードを指定した場合** HDInsight クラスターを作成したときに、入力を求められたらパスワードを指定する必要があります。

HDInsight で SSH の使用に関する詳細については、次を参照してください。 [SSH Linux、Unix、OS X から HDInsight 上の Linux ベースの Hadoop を使用する](hdinsight-hadoop-linux-use-ssh-unix.md)です。

###PuTTY (Windows ベースのクライアント)

Windows ではビルトイン SSH クライアントは提供されません。 使用することをお勧め **PuTTY**, からダウンロードできる [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)します。

PuTTY の使用の詳細については、次を参照してください。 [SSH Windows から Linux ベースの Hadoop を使用する ](hdinsight-hadoop-linux-use-ssh-windows.md)です。

##<a id="hive"></a>Hive コマンドの使用

2. 接続したら、次のコマンドを使用して Hive CLI を起動します。

        hive

3. という名前の新しいテーブルを作成するには、次の文を入力して、CLI を使用して **log4jLogs** サンプル データを使用します。

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    これらのステートメントは次のアクションを実行します。

    * **DROP TABLE** -テーブルが既に存在する場合は、テーブルとデータ ファイルを削除します。
    * **CREATE EXTERNAL TABLE** -Hive に新しく '外部' テーブルを作成します。 外部テーブルは Hive にテーブル定義のみを格納します。 データは元の場所に残されます。
    * **ROW FORMAT** -Hive にデータの形式です。 ここでは、各ログのフィールドは、スペースで区切られています。
    * **STORED AS TEXTFILE LOCATION** - Hive にデータを格納格納先 (example/data ディレクトリ) といるテキストとして格納されています。
    * **選択** -すべての行の数を選択、列 **t4** 値が含まれています **[ERROR]**します。 これには、値が返さ **3** がこの値を含む 3 つの行あるためです。
    * **INPUT__FILE__NAME のように '%.log'** -Hive にだけで終わるファイルからデータを返すする必要があります。 ログです。 これにより、検索はデータを含む sample.log ファイルに制限され、定義したスキーマに一致しない他のサンプル データ ファイルのデータを返すことができなくなります。

    > [AZURE.NOTE] 基になるデータや別の MapReduce 操作をデータの自動アップロード処理などの外部ソースによって更新されますが、常に最新のデータを使用するための Hive クエリを対象に予測される場合、外部テーブルを使用する必要があります。
    >
    > 外部テーブルを削除しても **いない** データ、テーブル定義のみを削除します。

4. という名前の新しい「内部」テーブルを作成する次のステートメントを使用して **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    これらのステートメントは次のアクションを実行します。

    * **テーブル IF NOT EXISTS の作成** -が既に存在しない場合は、テーブルを作成します。 以降、 **外部** キーワードが使用されていない、これは内部テーブルであり、Hive データ ウェアハウスに格納され、Hive によって完全に管理されます。
    * **STORED AS ORC** -Optimized Row Columnar (ORC) 形式でデータを格納します。 この形式は、Hive にデータを格納するための、非常に効率的で適切な形式です。
    * **INSERT OVERWRITE ...選択** の行を選択、 **log4jLogs** がテーブル **[ERROR]**, へのデータを挿入、 **errorLogs** テーブルです。

    列のみを確認するを含む **[ERROR]** 列に格納された、t4 に、 **errorLogs** テーブルで、次のステートメントを使用して、すべての行を返します **errorLogs**:

        SELECT * from errorLogs;

    3 行のデータを返す、すべてを含む **[ERROR]** 各行の t4 列にします。

    > [AZURE.NOTE] 外部テーブルとは異なり、内部デーブルを削除する基底のデータが削除されます。

##<a id="summary"></a>概要

このように、Hive コマンドを使用すると、HDInsight クラスターで簡単に対話的に Hive クエリを実行し、ジョブ ステータスを監視し、出力を取得できます。

##<a id="nextsteps"></a>次のステップ

HDInsight での Hive に関する全般的な情報

* [HDInsight での Hive と Hadoop の使用](hdinsight-use-hive.md)

HDInsight での Hadoop のその他の使用方法に関する情報

* [HDInsight での Pig と Hadoop の使用](hdinsight-use-pig.md)

* [HDInsight での MapReduce と Hadoop の使用](hdinsight-use-mapreduce.md)

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

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md



[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png


