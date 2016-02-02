<properties 
    pageTitle="Linux ベースの HDInsight での Hive を使用したフライトの遅延データの分析 | Microsoft Azure" 
    description="Linux ベースの HDInsight で Hive を使用してフライト データを分析し、Sqoop を使用して SQL Database にデータをエクスポートする方法について説明します。" 
    services="hdinsight" 
    documentationCenter="" 
    authors="Blackmist" 
    manager="paulettm" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/04/2015" 
    ms.author="larryfr"/>


# HDInsight での Hive を使用したフライト遅延データの分析

Linux ベースの HDInsight で Hive を使用してフライト遅延データを分析し、Sqoop を使用して Azure SQL Database にデータをエクスポートする方法について説明します。
> [AZURE.NOTE] このドキュメントの各部分は Windows ベースの HDInsight クラスター (Python と Hive など) で使用できますが、多くの手順は Linux ベースのクラスターに固有のものです。 Windows ベースのクラスターで手順については、「 [HDInsight で Hive を使用したフライト遅延データの分析](hdinsight-analyze-flight-delay-data.md)

### 前提条件

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。

- __HDInsight クラスター__。 参照してください [Linux での HDInsight で Hive と Hadoop の使用を開始](hdinsight-hadoop-linux-tutorial-get-started.md) 新しい Linux ベースの HDInsight クラスターを作成する手順についてです。

- __Azure SQL データベース__。 保存先データ ストアとして Azure SQL Database を使用します。 SQL データベースが既にがあるない場合は、を参照してください [Azure SQL データベースを作成する方法](../sql-database/sql-database-create.md)

- __Azure CLI__。 Azure CLI をインストールしていない場合は、次を参照してください。 [のインストールと Azure CLI の構成](../xplat-cli-install.md) 他手順を実行します。


## フライト データのダウンロード

1. 参照 [研究・革新技術庁/運輸統計局 ][rita-website]します。
2. このページで、次の値を選択します。

    |名前 |値 |
    |Filter Year |2013 |
    |Filter Period |年 1 月 |
    |フィールド |Year、FlightDate、UniqueCarrier、通信事業者、FlightNum、OriginAirportID、Origin、OriginCityName、OriginState、DestAirportID、Dest、DestCityName、DestState、DepDelayMinutes、ArrDelay、ArrDelayMinutes、CarrierDelay、WeatherDelay、NASDelay、SecurityDelay、LateAircraftDelay。 その他のフィールドはすべてクリア |

3. **[Download]** をクリックします。

## データのアップロード

1. 次のコマンドを使用して、HDInsight クラスター ヘッドノードに zip ファイルをアップロードします。

        scp FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    __FILENAME__ を zip ファイルの名前に置き換えます。 __USERNAME__ を HDInsight クラスターの SSH ログインに置き換えます。 CLUSTERNAME を HDInsight クラスターの名前に置き換えます。
    > [AZURE.NOTE] パスワードを使用して SSH ログインを認証する場合、パスワードの入力を求められます。 公開キーを使用している場合は、使用する必要があります、 `-i` パラメーターと一致する秘密キーへのパスを指定します。 たとえば `scp-i ~/.ssh/id_rsa FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`します。

2. アップロードが完了したら、SSH を使用してクラスターに接続します。

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Linux ベースの HDInsight での SSH の使用方法の詳細については、次の記事をご覧ください。

    * [Linux、Unix、OS X から HDInsight 上の Linux ベースの Hadoop で SSH を使用します。](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Windows から Linux ベースの Hadoop で SSH を使用します。](hdinsight-hadoop-linux-use-ssh-windows.md)

3. 接続したら、次のコマンドを使用して .zip ファイルを解凍します。

        unzip FILENAME.zip

    サイズが約 60 MB の .csv ファイルが抽出されます。

4. 次のコマンドを使用して、WASB (HDInsight で使用される分散データ ストア) に新しいディレクトリを作成し、ファイルをコピーします。

    hadoop fs/wordcount/apps-p/tutorials/flightdelays/data
    hadoop fs copyFromLocal FILENAME.csv/tutorials/flightdelays/data/FILENAME.csv

## HiveQL の作成と実行

次の手順に従って、CSV ファイルから __Delays__ という名前の Hive テーブルにデータをインポートします。

1. 次のコマンドを使用して、__flightdelays.hql__ という名前の新しいファイルを作成し、編集します。

     nano flightdelays.hql

 このファイルの内容として以下を使用します。

     DROP TABLE delays_raw;
     -- Creates an external table over the csv file
     CREATE EXTERNAL TABLE delays_raw (
         YEAR string,
         FL_DATE string,
         UNIQUE_CARRIER string,
         CARRIER string,
         FL_NUM string,
         ORIGIN_AIRPORT_ID string,
         ORIGIN string,
         ORIGIN_CITY_NAME string,
         ORIGIN_CITY_NAME_TEMP string,
         ORIGIN_STATE_ABR string,
         DEST_AIRPORT_ID string,
         DEST string,
         DEST_CITY_NAME string,
         DEST_CITY_NAME_TEMP string,
         DEST_STATE_ABR string,
         DEP_DELAY_NEW float,
         ARR_DELAY_NEW float,
         CARRIER_DELAY float,
         WEATHER_DELAY float,
         NAS_DELAY float,
         SECURITY_DELAY float,
         LATE_AIRCRAFT_DELAY float)
     -- The following lines describe the format and location of the file
     ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
     LINES TERMINATED BY '\n'
     STORED AS TEXTFILE
     LOCATION '/tutorials/flightdelays/data';
    
     -- Drop the delays table if it exists
     DROP TABLE delays;
     -- Create the delays table and populate it with data
     -- pulled in from the CSV file (via the external table defined previously)
     CREATE TABLE delays AS
     SELECT YEAR AS year,
         FL_DATE AS flight_date,
         substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
         substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
         substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
         ORIGIN_AIRPORT_ID AS origin_airport_id,
         substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
         substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
         substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
         DEST_AIRPORT_ID AS dest_airport_id,
         substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
         substring(DEST_CITY_NAME,2) AS dest_city_name,
         substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
         DEP_DELAY_NEW AS dep_delay_new,
         ARR_DELAY_NEW AS arr_delay_new,
         CARRIER_DELAY AS carrier_delay,
         WEATHER_DELAY AS weather_delay,
         NAS_DELAY AS nas_delay,
         SECURITY_DELAY AS security_delay,
         LATE_AIRCRAFT_DELAY AS late_aircraft_delay
     FROM delays_raw;

2. __Ctrl + X__ キーを押した後、__Y__ キーを押してファイルを保存します。

3. 次のコマンドを使用して Hive を起動し、__flightdelays.hql__ ファイルを実行します。

        hive -i flightdelays.hql

    これは、ファイルを実行し、返す、 `hive >` プロンプトです。

4. 受信すると、 `hive >` プロンプトで、次を使用して、インポートされたフライト遅延データからデータを取得します。

        INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
        SELECT regexp_replace(origin_city_name, '''', ''),
            avg(weather_delay)
        FROM delays
        WHERE weather_delay IS NOT NULL
        GROUP BY origin_city_name;

    これは平均遅延時間と共に、悪天候による遅延が発生した都市の一覧を取得し、保存して `/tutorials/flightdelays/output`します。 その後、Sqoop がこの場所からデータを読み取り、Azure SQL Database にエクスポートします。

## SQL Database の作成

Azure SQL Database を作成するには、次の手順に従います。 これは、Sqoop を使用して HDInsight からエクスポートされたデータを保持するために使用されます。

1. Azure CLI がインストールされている開発環境から、次のコマンドを使用して新しい Azure SQL Database を作成します。

        azure sql server create <adminLogin> <adminPassword> <region>

    たとえば、 `azure の sql server 管理者パスワード「米国西部」を作成する`します。

    コマンドが完了したら、次のような応答が返されます。

        info:    Executing command sql server create
        + Creating SQL Server
        data:    Server Name i1qwc540ts
        info:    sql server create command OK


> [AZURE.IMPORTANT] このコマンドによって返されるサーバーの名前に注意してください。これは、作成された SQL Database サーバーの短い名前です。完全修飾ドメイン名 (FQDN) は `< shortname >..database.windows.net を付けて`します。

2. 次のコマンドを使用して **sqooptest** という名前のデータベースを SQL Database サーバーに作成します。

        azure sql db create [options] <serverName> sqooptest <adminLogin> <adminPassword>

    コマンドが完了すると、"OK" というメッセージが返されます。
    > [AZURE.NOTE] アクセスできないことを示すエラーが返される場合は、次のコマンドを使用して、SQL Database ファイアウォールに、クライアント ワークステーションの IP アドレスの追加が必要になる場合があります。
    >
    > `sql firewallrule create [options] &lt;serverName&gt; &lt;ruleName&gt; &lt;startIPAddress&gt; &lt;endIPAddress&gt;`

## SQL Database テーブルの作成

> [AZURE.NOTE] SQL Database に接続してテーブルを作成するには、多くの方法があります。 次の手順を使用して [FreeTDS](http://www.freetds.org/) 、HDInsight クラスターからです。

1. SSH を使用して Linux ベースの HDInsight クラスターに接続し、SSH セッションから次の手順を実行します。

3. 次のコマンドを使用して FreeTDS をインストールします。

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. FreeTDS がインストールされると、次のコマンドを使用して、以前に作成した SQL Database サーバーに接続できます。

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    次のような出力が返されます。

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. `1 >` プロンプトで次の行を入力します。

        CREATE TABLE [dbo].[delays](
        [origin_city_name] [nvarchar](50) NOT NULL,
        [weather_delay] float,
        CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
        ([origin_city_name] ASC))
        GO

    ときに、 `移動` ステートメントに入ると、前のステートメントが評価されます。 これにより、クラスター化インデックス (SQL Database で必要) 付きの、__delays__ という名前の新しいテーブルが作成されます。

    次を使用して、テーブルが作成されたことを確認します。

        SELECT * FROM information_schema.tables
        GO

    次のような出力が表示されます。

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     delays      BASE TABLE

8. 入力 `終了` で、 `1 >` プロンプト、tsql ユーティリティを終了します。

## Sqoop を使用したデータのエクスポート

1. 次のコマンドを使用して、Sqoop lib ディレクトリから、SQL Server JDBC ドライバーへのリンクを作成します。 これにより、Sqoop がこのドライバーを使用して SQL Database と対話できます。

        sudo ln /usr/share/java/sqljdbc_4.1/enu/sqljdbc4.jar /usr/hdp/current/sqoop-client/lib/sqljdbc4.jar

2. 次のコマンドを使用して、Sqoop が SQL Database を認識できることを確認します。

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    これにより、先ほど作成した sqooptest データベースを含むデータベースのリストが返されます。

3. 次のコマンドを使用して、hivesampletable から mobiledata テーブルにデータをエクスポートします。

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir 'wasb:///tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1

    これは Sqoop sqooptest データベースに SQL データベースに接続し、wasb からデータをエクスポートするよう指示します。///チュートリアル/flightdelays/出力 (以前に hive クエリの出力を保存した場所) delays テーブルにします。

4. コマンドが完了したら、次を使用して、TSQL によってデータベースに接続します。

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    接続されたら、次のステートメントを使用して、データが mobiledata テーブルにエクスポートされたことを確認します。

        SELECT * FROM delays
        GO

    テーブル内のデータの一覧が表示されます。 型 `終了` 、tsql ユーティリティを終了します。

## <a id="nextsteps"></a> 次のステップ

ここでは、ファイルを Azure BLOB ストレージにアップロードする方法、Azure BLOB ストレージのデータを Hive テーブルに取り込む方法、Hive クエリの実行方法、Sqoop を使用して HDFS から Azure SQL Database にデータをエクスポートする方法を学習しました。 詳細については、次の記事を参照してください。

* [[Hdinsight の入門] HDInsight の概要][hdinsight-get-started]
* [[Hdinsight を使用して hive] HDInsight での Hive を使用します。][hdinsight-use-hive]
* [[Hdinsight を使用して oozie] HDInsight での Oozie を使用します。][hdinsight-use-oozie]
* [[Hdinsight での sqoop の使用] を HDInsight での Sqoop を使用します。][hdinsight-use-sqoop]
* [[Hdinsight での pig の使用] を HDInsight での Pig を使用します。][hdinsight-use-pig]
* [[Hdinsight での開発-mapreduce] HDInsight 用 Java MapReduce プログラムを開発します。][hdinsight-develop-mapreduce]
* [[Hdinsight の開発-ストリーミング] HDInsight 用 Python Hadoop ストリーミング プログラムを開発します。][hdinsight-develop-streaming]




[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/ 
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/ 
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/ 
[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time 
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx 
[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md 
[hdinsight-use-hive]: hdinsight-use-hive.md 
[hdinsight-provision]: hdinsight-provision-clusters.md 
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md 
[hdinsight-upload-data]: hdinsight-upload-data.md 
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md 
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md 
[hdinsight-use-pig]: hdinsight-use-pig.md 
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md 
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md 
[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL 
[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx 

