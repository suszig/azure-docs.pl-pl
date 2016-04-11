<properties
   pageTitle="Beeline による HDInsight (Hadoop) での Hive の使用 | Microsoft Azure"
   description="SSH を使用して HDInsight で Hadoop クラスターに接続し、Beeline を使用して Hive クエリを対話的に実行する方法について説明します。 Beeline は、JDBC を介して HiveServer2 を使用するためのユーティリティです。"
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

#Beeline による HDInsight での Hive と Hadoop の使用

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

この記事では、Secure Shell (SSH) を使用して Linux ベースの HDInsight クラスターに接続しを使用して Hive クエリを対話的に実行する方法を説明します、 [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) コマンド ライン ツールです。

> [AZURE.NOTE] Beeline は、Hive に接続する JDBC を使用します。 Hive で JDBC の使用に関する詳細については、次を参照してください。 [Hive JDBC ドライバーを使用して Azure HDInsight で Hive への接続](hdinsight-connect-hive-jdbc-driver.md)します。

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

##<a id="beeline"></a>Beeline コマンドを使用する

1. 接続したら、次を使用して、ヘッド ノードのホスト名を取得します。

        hostname -f
    
    Beeline から HiveServer2 に接続する際に使用できるよう、返されたホスト名を保存します。
    
2. 次のコマンドを使用して Hive CLI を起動します。

        beeline

2. `beeline>` プロンプトから、次を使用して HiveServer2 サービスに接続します。 置換 __ホスト名__ ヘッド ノードの前に、ホスト名に返されます。

        !connect jdbc:hive2://HOSTNAME:10001/;transportMode=http admin

    メッセージが表示されたら、HDInsight クラスターの管理者 (admin) アカウントのパスワードを入力します。 接続が確立されると、プロンプトが次に変わります。
    
        jdbc:hive2://HOSTNAME:10001/>

3. Beeline コマンドは、通常、`!` 文字で始まります。たとえば、「`!help`」でヘルプが表示されます。 ただし、`!` は、多くの場合、省略できます。 たとえば、「`help`」も機能します。

    ヘルプを表示すると、「`!sql`」が表示されます。これは HiveQL ステートメントの実行に使用されます。 ただし、HiveQL は一般的に使用されるため、先行する「`!sql`」を省略できます。 次の 2 つのステートメントはまったく同じ結果となります。Hive で現在利用可能なテーブルが表示されます。
    
        !sql show tables;
        show tables;
    
    新しいクラスターで 1 つのテーブルを表示する必要があります: __hivesampletable__します。

4. hivesampletable のスキーマを表示するには、次を使用します。

        describe hivesampletable;
        
    次の情報が返されます。
    
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

    テーブルの列が表示されます。 このデータにクエリを実行できますが、代わりに、まったく新しいテーブルを作成し、Hive にデータを読み込み、スキーマを適用する方法について確認しましょう。
    
5. という名前の新しいテーブルを作成する次のステートメントを入力する **log4jLogs** 、HDInsight クラスターと共に提供されたサンプル データを使用しています。

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
    * **INPUT__FILE__NAME のように '%.log'** -Hive にだけで終わるファイルからデータを返すする必要があります。 ログです。 通常、Hive でクエリを実行するとき、同じフォルダー内ではデータのスキーマが同じになりますが、このサンプル ログ ファイルは他のデータ形式で格納されます。

    > [AZURE.NOTE] 基になるデータや別の MapReduce 操作をデータの自動アップロード処理などの外部ソースによって更新されますが、常に最新のデータを使用するための Hive クエリを対象に予測される場合、外部テーブルを使用する必要があります。
    >
    > 外部テーブルを削除しても **いない** データ、テーブル定義のみを削除します。
    
    このコマンドの出力は次のように表示されます。
    
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

4. Beeline を終了するには、`!quit` を使用します。

##<a id="file"></a>HiveQL ファイルを実行する

Beeline を使用し、HiveQL ステートメントを含むファイルを実行することもできます。 次の手順でファイルを作成し、Beeline を使用してそれを実行します。

1. という名前の新しいファイルを作成する次のコマンドを使用して __query.hql__:

        nano query.hql
        
2. エディターが開いたら、ファイルの内容として次を使用します。 このクエリはという名前の新しい「内部」テーブルを作成 **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    これらのステートメントは次のアクションを実行します。

    * **テーブル IF NOT EXISTS の作成** -が既に存在しない場合は、テーブルを作成します。 以降、 **外部** キーワードが使用されていない、これは内部テーブルであり、Hive データ ウェアハウスに格納され、Hive によって完全に管理されます。
    * **STORED AS ORC** -Optimized Row Columnar (ORC) 形式でデータを格納します。 この形式は、Hive にデータを格納するための、非常に効率的で適切な形式です。
    * **INSERT OVERWRITE ...選択** の行を選択、 **log4jLogs** がテーブル **[ERROR]**, へのデータを挿入、 **errorLogs** テーブルです。
    
    > [AZURE.NOTE] 外部テーブルとは異なり、内部デーブルを削除する基底のデータが削除されます。
    
3. ファイルを保存するには使用 __Ctrl__+ ___X__, を入力し、 __Y__, 、最後に __Enter__します。

4. 次を使用し、Beeline でファイルを実行します。 Replease __ホスト名__ ヘッド ノードの前に収集した名前を持つと __パスワード__ 管理者アカウントのパスワード。

        beeline -u 'jdbc:hive2://HOSTNAME:10001/;transportMode=http' -n admin -p PASSWORD -f query.hql

5. 確認する、 **errorLogs** テーブルが作成された、Beeline を起動して、HiveServer2 に接続し、次のステートメントを使用して、すべての行を返します **errorLogs**:

        SELECT * from errorLogs;

    3 行のデータを返す、すべてを含む **[ERROR]** 各行の t4 列にします。
    
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

##<a id="summary"></a>概要

このように、Beeline コマンドを使用すると、HDInsight クラスターで簡単に対話的に Hive クエリを実行できます。

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



