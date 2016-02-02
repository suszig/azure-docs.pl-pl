<properties
   pageTitle="Beeline による HDInsight (Hadoop) での Hive の使用 | Microsoft Azure"
   description="SSH を使用して HDInsight で Hadoop クラスターに接続し、Beeline を使用して Hive クエリを対話的に実行する方法について説明します。Beeline は、JDBC を介して HiveServer2 を使用するためのユーティリティです。"
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


# Beeline による HDInsight での Hive と Hadoop の使用

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

この記事では、Secure Shell (SSH) を使用して Linux ベースの HDInsight クラスターに接続しを使用して Hive クエリを対話的に実行する方法を説明します、 [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) コマンド ライン ツールです。
> [AZURE.NOTE] Beeline は Hive への接続に JDBC を使用しました。 Hive で JDBC の使用に関する詳細については、次を参照してください。 [Hive JDBC ドライバーを使用して Azure HDInsight で Hive への接続](hdinsight-connect-hive-jdbc-driver.md)します。

## <a id="prereq"></a>前提条件

この記事の手順を完了するには、次のものが必要です。

* HDInsight クラスターでの Linux ベースの Hadoop

* SSH クライアント SSH クライアントを備えた Linux、Unix、および Mac OS Windows ユーザーのなど、クライアントをダウンロードする必要があります [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)します。

## <a id="ssh"></a>SSH による接続します。

SSH コマンドを使用して、HDInsight クラスターの完全修飾ドメイン名 (FQDN) に接続します。 FQDN はクラスターに指定した名前で、その後、**.azurehdinsight.net** が続きます。 以下の例では、**myhdinsight** という名前のクラスターに接続します。

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**HDInsight クラスターの作成時に SSH 認証に証明書キーを指定した場合は**、クライアント システムの秘密キーの場所を指定する必要があることがあります。

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**HDInsight クラスターの作成時に SSH 認証のパスワードを指定した場合は**、パスワードの入力を求められます。

HDInsight で SSH の使用に関する詳細については、次を参照してください。 [SSH Linux、Unix、OS X から HDInsight 上の Linux ベースの Hadoop を使用する](hdinsight-hadoop-linux-use-ssh-unix.md)します。

### PuTTY (Windows ベースのクライアント)

Windows ではビルトイン SSH クライアントは提供されません。 使用することをお勧め **PuTTY**, からダウンロードできる [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)します。

PuTTY の使用の詳細については、次を参照してください。 [SSH Windows から Linux ベースの Hadoop を使用する ](hdinsight-hadoop-linux-use-ssh-windows.md)します。

## <a id="beeline"></a>Beeline コマンドを使用してください。

1. 接続したら、次を使用して、ヘッド ノードのホスト名を取得します。

        hostname -f

    Beeline から HiveServer2 に接続する際に使用できるよう、返されたホスト名を保存します。

2. 次のコマンドを使用して Hive CLI を起動します。

        beeline

2. `Beeline >` プロンプトで、次を使用して、HiveServer2 サービスに接続します。 前にヘッド ノードで返されたホスト名で __HOSTNAME__ を置き換えます。

        !connect jdbc:hive2://HOSTNAME:10001/;transportMode=http admin

    メッセージが表示されたら、HDInsight クラスターの管理者 (admin) アカウントのパスワードを入力します。 接続が確立されると、プロンプトが次に変わります。

        jdbc:hive2://HOSTNAME:10001/>

3. Beeline コマンドは、通常で始まり、 `!` 文字、たとえば `! ヘルプ` ヘルプを表示します。 ただし、 `!` 省略されたことがよくあります。 たとえば、 `ヘルプ` はでも機能します。

    ヘルプを表示するかどうか、わかります `! sql`, を使用して、HiveQL ステートメントを実行します。 ただし、HiveQL はので一般的に使用できる ommit 示したを使用して `! sql`します。 次の 2 つのステートメントはまったく同じ結果となります。Hive で現在利用可能なテーブルが表示されます。

        !sql show tables;
        show tables;

    新しいクラスターでは、1 つのテーブルだけが表示されます。__hivesampletable__ です。

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

5. HDInsight クラスター付属のサンプル データを使用し、次のステートメントを入力して「**log4jLogs**」という名前の新しいテーブルを作成します。

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    これらのステートメントは次のアクションを実行します。

    * **DROP TABLE** - テーブルが既存の場合にテーブルとデータ ファイルを削除します。
    * **CREATE EXTERNAL TABLE**: Hive に新しく '外部' テーブルを作成します。 外部テーブルは Hive にテーブル定義のみを格納します。 データは元の場所に残されます。
    * **ROW FORMAT** - Hive にデータの形式を示します。 ここでは、各ログのフィールドは、スペースで区切られています。
    * **STORED AS TEXTFILE LOCATION** - Hive に、データの格納先 (example/data ディレクトリ) と、データはテキストとして格納されていることを示します。
    * **SELECT** - **t4** 列の値が **[ERROR]** であるすべての行の数を指定します。 ここでは、この値を含む行が 3 行あるため、**3** という値が返されています。
    * **INPUT__FILE__NAME LIKE '%.log'** - Hive に .log で終わるファイルのデータのみを返す必要があることを示します。 通常、Hive でクエリを実行するとき、同じフォルダー内ではデータのスキーマが同じになりますが、このサンプル ログ ファイルは他のデータ形式で格納されます。
    > [AZURE.NOTE] 基盤となるデータを外部ソースによって更新する (データの自動アップロード処理など) 場合や別の MapReduce 操作によって更新する場合に、Hive クエリで最新のデータを使用する場合は、外部テーブルを使用する必要があります。
    >
    > 外部テーブルを削除しても、データは削除**されません**。テーブル定義のみが削除されます。

    このコマンドの出力は次のように表示されます。

        INFO: Tez セッションまだ作成されていません。 セッションを開くとき
        情報:

        INFO: 状態: 実行中 (アプリ id application_1443698635933_0001 でクラスターに YARN Executing)

        INFO: 1 のマッピング:-/- レジューサ 2: 0/1
        INFO: マップ 1: 0/1 レジューサ 2: 0/1
        INFO: マップ 1: 0/1 レジューサ 2: 0/1
        INFO: マップ 1: 0/1 レジューサ 2: 0/1
        INFO: マップ 1: 0/1 レジューサ 2: 0/1
        INFO: マップ 1: 0 (+ 1)/1 レジューサ 2: 0/1
        INFO: マップ 1: 0 (+ 1)/1 レジューサ 2: 0/1
        INFO: マップ 1: 1/1 レジューサ 2: 0/1
        INFO: マップ 1: 1/1 レジューサ 2: 0 (+ 1)/1
        INFO: マップ 1: 1/1 レジューサ 2: 1/1
        +----------+--------+--+
        |  度 |数 |
        +----------+--------+--+
        |[ERROR] |3      |
        +----------+--------+--+
        1 行には、(47.351 秒) が選択されています。

4. Beeline を終了するには使用 `! 終了`します。

## <a id="file"></a>HiveQL ファイルを実行します。

Beeline を使用し、HiveQL ステートメントを含むファイルを実行することもできます。 次の手順でファイルを作成し、Beeline を使用してそれを実行します。

1. 次のコマンドを使用し、「__query.hql__」という名前の新しいファイルを作成します。

        nano query.hql

2. エディターが開いたら、ファイルの内容として次を使用します。 このクエリにより「**errorLogs**」という名前の新しい「内部」テーブルが作成されます。

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    これらのステートメントは次のアクションを実行します。

    * **CREATE TABLE IF NOT EXISTS** - 既存のテーブルがない場合、テーブルを作成します。 **EXTERNAL** キーワードが使用されていないため、これは内部テーブルであり、Hive のデータ保管先に格納され、完全に Hive によって管理されます。
    * **STORED AS ORC** - Optimized Row Columnar (ORC) 形式でデータを格納します。 この形式は、Hive にデータを格納するための、非常に効率的で適切な形式です。
    * **INSERT OVERWRITE ...選択** の行を選択、 **log4jLogs** がテーブル **[ERROR]**, へのデータを挿入、 **errorLogs** テーブルです。
    > [AZURE.NOTE] 外部テーブルとは異なり、内部テーブルを削除すると、基盤となるデータも削除されます。

3. ファイルを保存するには、__Ctrl__ キーを押しながら __X__ キーを押し、__Y__ キー、__Enter__ キーの順に押します。

4. 次を使用し、Beeline でファイルを実行します。 前にヘッド ノードで取得した名前で __HOSTNAME__ を置き換え、管理者アカウントのパスワードで __PASSWORD__ を置き換えます。

        beeline -u 'jdbc:hive2://HOSTNAME:10001/;transportMode=http' -n admin -p PASSWORD -f query.hql

5. **errorLogs** テーブルが作成されたことを確認するには、Beeline を起動し、HiveServer2 に接続し、**errorLogs** からすべての行を返す次のステートメントを使用します。

        SELECT * from errorLogs;

    3 つのデータ行が返され、各行の t4 列には **[ERROR]** が含まれます。

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)


## <a id="summary"></a>概要

このように、Beeline コマンドを使用すると、HDInsight クラスターで簡単に対話的に Hive クエリを実行できます。

## <a id="nextsteps"></a>次のステップ

HDInsight での Hive に関する全般的な情報

* [HDInsight での Hadoop の Hive を使用します。](hdinsight-use-hive.md)

HDInsight での Hadoop のその他の使用方法に関する情報

* [HDInsight での Hadoop での Pig の使用](hdinsight-use-pig.md)

* [HDInsight での Hadoop の MapReduce を使用します。](hdinsight-use-mapreduce.md)


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

