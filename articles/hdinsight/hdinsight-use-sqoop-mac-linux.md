<properties
    pageTitle="Linux ベースの HDInsight での Hadoop Sqoop の使用 | Microsoft Azure"
    description="Sqoop を使用して、HDInsight クラスター上の Linux ベースの Hadoop と Azure SQL データベース間でインポートおよびエクスポートする方法について説明します。"
    editor="cgronlun"
    manager="paulettm"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/04/2015"
    ms.author="larryfr"/>


# HDInsight の Hadoop での Sqoop の使用 (SSH)

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Linux ベースの HDInsight クラスターと Azure SQL Database または SQL Server データベースの間のインポートとエクスポートに Sqoop を使用する方法について説明します。
> [AZURE.NOTE] この記事の手順では、Linux ベースの HDInsight クラスターへの接続に SSH を使用します。 Windows クライアントに記載されている Linux ベースのクラスターで、Sqoop を使用する Azure PowerShell を使用できますも [(PowerShell) を HDInsight での Hadoop での Sqoop の使用](hdinsight-use-sqoop.md)します。

## Sqoop とは

Hadoop はログやファイルなどの非構造化データおよび半構造化データを処理する場合に自然な選択ですが、リレーショナル データベースに格納された構造化データを処理する必要が生じることもあります。

[Sqoop ][sqoop-user-guide-1.4.4] を Hadoop クラスターとリレーショナル データベース間でデータを転送するためのツールです。 このツールを使用して、SQL Server、MySQL、Oracle などのリレーショナル データベース管理システム (RDBMS) から Hadoop 分散ファイル システム (HDFS) へデータをインポートしたり、MapReduce または Hive を使用して Hadoop のデータを変換してから、そのデータを RDBMS にエクスポートして戻したりできます。 このチュートリアルでは、リレーショナル データベースとして SQL Server データベースを使用します。

HDInsight クラスターでサポートされている Sqoop のバージョン、を参照してください [HDInsight で提供されるクラスター バージョンの新機能ですか? ][hdinsight-versions]します。


## 前提条件

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **ワークステーション**: SSH クライアントを使用しているコンピューター。

- **AZURE-CLI**: 詳細については、を参照してください [のインストールと Azure CLI の構成](../xplat-cli-install.md)。

- **Linux ベースの HDInsight クラスター**: クラスターのプロビジョニング方法については、「 [HDInsight の概要](hdinsight-hadoop-linux-tutorial-get-started.md) または [HDInsight クラスターのプロビジョニング ][hdinsight-provision]します。

- **Azure SQL データベース**: このドキュメントでは、SQL データベースの例を作成するための手順を説明します。 SQL データベースの詳細については、次を参照してください。 [[sql database--はじめに] Azure SQL データベースの使用開始][sqldatabase-get-started]します。

* **SQL Server**: このドキュメントの手順は、多少変更するだけで SQL Server でも使用できます。ただし、HDInsight クラスターと SQL Server の両方が同じ Azure Virtual Network 上に存在する必要があります。 SQL Server でのこの記事の使用に固有の要件の詳細については、次を参照してください。、 [を使用して SQL Server](#using-sql-server) セクションです。

## シナリオの理解

HDInsight クラスターにはサンプル データがいくつか付属しています。 という名前の Hive テーブルを使用して、 **hivesampletable**, にあるデータ ファイルを参照しています **wasb:////hive/ウェアハウス/hivesampletable**します。 テーブルはモバイル デバイスのデータを含んでいます。 Hive テーブルのスキーマは次のとおりです。

| フィールド| データ型|
| ----- | --------- |
| clientid| 文字列|
| querytime| 文字列|
| market| 文字列|
| deviceplatform| 文字列|
| devicemake| 文字列|
| devicemodel| 文字列|
| state| 文字列|
| country| 文字列|
| querydwelltime| double|
| sessionid| bigint|
| sessionpagevieworder| bigint|

まずエクスポート **hivesampletable** を Azure SQL database または SQL server という名前のテーブルに **mobiledata**, 、hdinsight にテーブルをインポート **wasb:///チュートリアル/usesqoop/importeddata**します。

## データベースの作成

1. ターミナルまたはコマンド プロンプトを開き、次のコマンドを使用して、新しい Azure SQL Database サーバーを作成します。

        azure sql server create <adminLogin> <adminPassword> <region>

    たとえば、 `azure の sql server 管理者パスワード「米国西部」を作成する`します。

    コマンドが完了したら、次のような応答が返されます。

        info:    Executing command sql server create
        + Creating SQL Server
        data:    Server Name i1qwc540ts
        info:    sql server create command OK

    > [AZURE.IMPORTANT] このコマンドによって返されるサーバーの名前に注意してください。 これは、作成された SQL Database サーバーの短い名前です。 完全修飾ドメイン名 (FQDN) は **< shortname >..database.windows.net を付けて**します。

2. 次のコマンドを使用して **sqooptest** という名前のデータベースを SQL Database サーバーに作成します。

        sql db create [options] <serverName> sqooptest <adminLogin> <adminPassword>

    コマンドが完了すると、"OK" というメッセージが返されます。
    > [AZURE.NOTE] アクセスできないことを示すエラーが返される場合は、次のコマンドを使用して、SQL Database ファイアウォールに、クライアント ワークステーションの IP アドレスの追加が必要になる場合があります。
    >
    > `sql firewallrule create [options] &lt;serverName&gt; &lt;ruleName&gt; &lt;startIPAddress&gt; &lt;endIPAddress&gt;`

## テーブルの作成

> [AZURE.NOTE] SQL Database に接続してテーブルを作成するには、多くの方法があります。 次の手順を使用して [FreeTDS](http://www.freetds.org/) 、HDInsight クラスターからです。

1. SSH を使用して、Linux ベースの HDInsight クラスターに接続します。 接続時に使用するアドレスは `CLUSTERNAME ssh.azurehdinsight.net` ポートは `22`します。

    SSH を使用して HDInsight に接続する方法の詳細については、次のドキュメントを参照してください。

    * **Linux、Unix または OS X クライアント**:を参照してください [Linux、OS X または Unix から Linux ベースの HDInsight クラスターへの接続](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)

    * **Windows クライアント**:を参照してください [Windows から Linux ベースの HDInsight クラスターへの接続](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)

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

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
        GO

    ときに、 `移動` ステートメントに入ると、前のステートメントが評価されます。 最初に、**mobiledata** テーブルが作成され、次にクラスター化インデックスがそのテーブルに追加されます (SQL Database が必要)。

    次を使用して、テーブルが作成されたことを確認します。

        SELECT * FROM information_schema.tables
        GO

    次のような出力が表示されます。

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

8. 入力 `終了` で、 `1 >` プロンプト、tsql ユーティリティを終了します。

## Sqoop のエクスポート

2. 次のコマンドを使用して、Sqoop lib ディレクトリから、SQL Server JDBC ドライバーへのリンクを作成します。 これにより、Sqoop がこのドライバーを使用して SQL Database と対話できます。

        sudo ln /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /usr/hdp/current/sqoop-client/lib/sqljdbc41.jar

3. 次のコマンドを使用して、Sqoop が SQL Database を認識できることを確認します。

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    これは、先ほど作成した **sqooptest** データベースを含むデータベースの一覧を返します。

4. 次のコマンドを使用して、**hivesampletable** から **mobiledata** テーブルにデータをエクスポートします。

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasb:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1

    これは Sqoop の SQL データベースに接続するよう指示、 **sqooptest** データベース、およびデータのエクスポート、 **wasb:///hive/ウェアハウス/hivesampletable** (物理ファイル、 *hivesampletable*,、) に、 **mobiledata** テーブルです。

5. コマンドが完了したら、次を使用して、TSQL によってデータベースに接続します。

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    接続されたら、次のステートメントを使用して、データが **mobiledata** テーブルにエクスポートされたことを確認します。

        SELECT * FROM mobiledata
        GO

    テーブル内のデータの一覧が表示されます。 型 `終了` 、tsql ユーティリティを終了します。

## Sqoop のインポート

1. 次の使用からデータをインポートして、 **mobiledata** に SQL データベースのテーブル、 **wasb:///チュートリアル/usesqoop/importeddata** ディレクトリに HDInsight:

        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

    インポートされたデータには、タブ文字で区切られたフィールドと、改行文字で終了する行が含まれます。

2. インポートが完了したら、次のコマンドを使用して、新しいディレクトリのデータを列挙します。

        hadoop fs -text wasb:///tutorials/usesqoop/importeddata/part-m-00000


## SQL Server の使用

Sqoop を使用すると、Azure でホストされているデータ センターまたは仮想マシンで、SQL Server からデータをインポートしたり、エクスポートしたりすることもできます。 SQL Database と SQL Server の使用方法には、次の違いがあります。

* HDInsight と SQL Server の両方が、同じ Azure Virtual Network に存在する必要があります。
    > [AZURE.NOTE] HDInsight は場所ベースの仮想ネットワークのみをサポートし、アフィニティ グループ ベースの仮想ネットワークは現在扱っていません。

    SQL Server をデータセンター内で使用している場合は、仮想ネットワークを*サイト間*または*ポイント対サイト*として構成する必要があります。
    > [AZURE.NOTE] **ポイント対サイト**仮想ネットワークの場合、SQL Server が VPN クライアント構成アプリケーションを実行している必要があります。このアプリケーションは、Azure 仮想ネットワーク構成の**ダッシュボード**から入手できます。

    作成と仮想ネットワークの構成の詳細については、次を参照してください。 [仮想ネットワークの構成タスク](../services/virtual-machines/)します。

* SQL 認証を許可するよう、SQL Server を構成する必要があります。 詳細については、を参照してください [認証モードを選択する](https://msdn.microsoft.com/ms144284.aspx)。

* リモート接続を許可するよう、SQL Server を構成する必要がある場合があります。 参照してください [SQL Server データベース エンジンへの接続のトラブルシューティング方法](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) の詳細

* **SQL Server Management Studio** または **tsql** などのユーティリティを使用して SQL Server データベースに **sqooptest** テーブルを作成する必要があります。Azure CLI を使用する手順は、Azure SQL Database でのみ機能します。

    **mobiledata** テーブルを作成する TSQL ステートメントは、SQL Database に使用する TSQL ステートメントに似ています。ただし、クラスター化インデックスの作成は例外です。SQL Server の場合、これは必要ありません。

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])

* Azure Virtual Network で名前を解決するためにドメイン ネーム システム (DNS) を構成していないと、HDInsight から SQL Server に接続するときに、SQL Server の IP アドレスを使用する必要がある場合があります。 次に例を示します。

        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1


## 次のステップ

ここでは Sqoop の使用方法を説明しました。 詳細については、次を参照してください。

- [を使用して HDInsight での Oozie ][hdinsight-use-oozie]: Oozie ワークフローで Sqoop を使用して操作します。
- [HDInsight ][hdinsight-analyze-flight-data]: 航空券の分析に Hive を使用して遅延データとし、Sqoop を使用して Azure SQL database にデータをエクスポートします。
- [[Hdinsight でのデータのアップロード] を HDInsight にデータをアップロード][hdinsight-upload-data]: Hdinsight/azure Blob ストレージにデータをアップロードするためには、その他のメソッドを検索します。




[hdinsight-versions]: hdinsight-component-versioning.md 
[hdinsight-provision]: hdinsight-provision-clusters.md 
[hdinsight-get-started]: ../hdinsight-get-started.md 
[hdinsight-storage]: ../hdinsight-use-blob-storage.md 
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md 
[hdinsight-use-oozie]: hdinsight-use-oozie.md 
[hdinsight-upload-data]: hdinsight-upload-data.md 
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md 
[sqldatabase-get-started]: ../sql-database-get-started.md 
[sqldatabase-create-configue]: ../sql-database-create-configure.md 
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx 
[powershell-install]: ../install-configure-powershell.md 
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx 
[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html 

