<properties 
    pageTitle="Azure 仮想マシン上の SQL Server にデータを移動する| Azure" 
    description="フラット ファイルまたはオンプレミスの SQL Server から Azure VM 上の SQL Server にデータを移動します。" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="paulettm" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/12/2015" 
    ms.author="fashah;mohabib;bradsev" /> 


# Azure 仮想マシン上の SQL Server にデータを移動する

この**メニュー**は、Cortana Analytics Process (CAP) でデータを保存および処理できるターゲット環境にデータを取り込む方法について説明するトピックにリンクしています。

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## はじめに

**このドキュメント**では、フラット ファイル (CSV 形式または TSV 形式) またはオンプレミスの SQL Server から、Azure 仮想マシン上の SQL Server にデータを移動するためのオプションについて説明します。 クラウドにデータを移動するためのこれらのタスクは、Azure で提供される Cortana Analytics Process の一部です。

Machine Learning の Azure SQL Database にデータを移動するオプションについて説明するトピックを参照してください。 [Azure Machine Learning for Azure SQL Database にデータを移動](machine-learning-data-science-move-sql-azure.md)します。

次の表は、Azure 仮想マシン上の SQL Server にデータを移動するためのオプションをまとめたものです。

 <b>ソース</b>| <b>Azure VM 上の SQL Server の移行先:</b>|
------------------ |-------------------- |
 <b>フラット ファイル</b>| 1.<a href="#insert-tables-bcp">コマンドライン一括コピー ユーティリティ (BCP) </a><br> 2 です。<a href="#insert-tables-bulkquery">一括挿入 SQL クエリ </a><br> 3 です。<a href="#sql-builtin-utilities">SQL Server のグラフィカル組み込みユーティリティ</a>
 <b>内部設置型 SQL Server</b>| 1.<a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Microsoft Azure VM のウィザードに SQL Server データベースの配置</a><br> 2 です。<a href="#export-flat-file">フラット ファイルへのエクスポート </a><br> 3 です。<a href="#sql-migration">SQL Database 移行ウィザード </a> <br> 4 です。<a href="#sql-backup">データベースのバックアップと復元 </a><br>

このドキュメントでは、SQL Server Management Studio または Visual Studio のデータベース エクスプローラーから SQL コマンドが実行されることを想定していることに注意してください。
> [AZURE.TIP] 代わりに、使用することができます [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) を作成してデータを SQL Server VM を Azure に移動するパイプラインをスケジュールします。 詳細については、次を参照してください。 [Azure Data Factory (コピー アクティビティ) を使ってデータをコピー](../data-factory/data-factory-copy-activity.md)します。


## <a name="prereqs"></a>前提条件

このチュートリアルでは、以下があることを前提としています。

* **Azure サブスクリプション**。 サインアップすることができます、サブスクリプションを持っていない場合、 [無料評価版](https://azure.microsoft.com/pricing/free-trial/)します。
* **Azure ストレージ アカウント**。 このチュートリアルのデータを格納するには、Azure ストレージ アカウントを使用します。 Azure ストレージ アカウントを取得していない場合、 [ストレージ アカウントの作成](storage-create-storage-account.md#create-a-storage-account) 記事です。 ストレージ アカウントを作成した後は、ストレージにアクセスするために使用するアカウント キーを取得する必要があります。 参照してください [表示、コピーおよび再生成するストレージ アクセス キー](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)します。
* **Azure VM 上に SQL Server** がプロビジョニングされていること。 手順については、次を参照してください。 [Azure SQL Server 仮想マシンを高度な分析用の IPython Notebook サーバーとして設定](machine-learning-data-science-setup-sql-server-virtual-machine.md)します。
* **Azure PowerShell** がローカルにインストールされ構成されていること。 手順については、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](powershell-install-configure.md)します。


## <a name="filesource_to_sqlonazurevm"></a> Azure VM 上の SQL Server にフラット ファイル ソースからデータを移動

データがフラット ファイル (行と列の形式で配置されている) に存在する場合は、次の方法を使用して Azure の SQL Server VM にデータを移動できます。

1. [コマンドライン一括コピー ユーティリティ (BCP)](#insert-tables-bcp)
2. [一括挿入 SQL クエリ ](#insert-tables-bulkquery)
3. [(インポート/エクスポート、SSIS) の SQL Server のグラフィカル組み込みユーティリティ](#sql-builtin-utilities)


### <a name="insert-tables-bcp"></a>コマンドライン一括コピー ユーティリティ (BCP)

BCP は、SQL Server と一緒にインストールされるコマンド ライン ユーティリティであり、データを移動する最も簡単な方法の 1 つです。 これは、3 つの異なる SQL Server (オンプレミスの SQL Server、SQL Azure、および Azure での SQL Server VM) すべて機能します。
> [AZURE.NOTE] **BCP 用のデータの場所**  
> 必須ではありませんが、ターゲットの SQL Server と同じマシン上にソース データを含むファイルがある場合、高速転送 (ネットワークの速度とローカル ディスク IO の速度の差) を使用できます。 マシンにデータを含むフラット ファイルを移動することができますを使用して SQL Server がインストールされているさまざまなファイル コピー ツールなど、 [AZCopy](../storage-use-azcopy.md), 、[Azure ストレージ エクスプ ローラー](https://azurestorageexplorer.codeplex.com/) または windows コピー/貼り付けを使用してリモート デスクトップ プロトコル (RDP)。

1. データベースとテーブルがターゲットの SQL Server データベースで作成されていることを確認します。 使用して実行する方法の例をここでは、 `Create Database` と `Create Table` コマンド。

     CREATE DATABASE <database_name>
    
     CREATE TABLE <tablename>
     (
         <columnname1> <datatype> <constraint>,
         <columnname2> <datatype> <constraint>,
         <columnname3> <datatype> <constraint>
     ) 

2. bcp がインストールされているマシンのコマンド ラインから次のコマンドを発行し、テーブルのスキーマを記述するフォーマット ファイルを生成します。

    `bcp dbname.テーブル名が nul c を書式設定には、-x-f exportformatfilename.xml-s servername\sqlinstance-t-t \t-r \n`

3. 次のように bcp コマンドを使用して、データをデータベースに挿入します。 SQL Server が同じマシン上にインストールされていると想定した場合、これはコマンド ラインから動作するはずです。

    `bcp dbname.datafilename.tsv-f exportformatfilename.xml-s servername\sqlinstancename-u ユーザー名の「-p パスワード-b block_size_to_move_in_single_attemp-t \t-r \n でテーブル名`

> **BCP 挿入の最適化** 、次の記事を参照してください [一括インポートを最適化するためのガイドライン'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) このような挿入を最適化します。

### <a name="insert-tables-bulkquery-parallel"></a>高速データ移動の挿入の並列処理

移動するデータのサイズが大きい場合は、PowerShell スクリプトで複数の BCP コマンドを並行して同時に実行することによって、高速化できます。
> [AZURE.NOTE] **ビッグ データの取り込み** 
> 大きなデータセットや非常に大きなデータセットのデータ読み込みを最適化するには、複数のファイル グループおよびパーティション テーブルを使用して、論理的および物理的なデータベース テーブルをパーティション分割します。 作成してパーティション テーブルへのデータを読み込む方法の詳細については、次を参照してください。  [SQL パーティション テーブルの並列読み込み](machine-learning-data-science-parallel-load-sql-partitioned-tables.md)します。


次のサンプル PowerShell スクリプトは、bcp を使用した並列挿入を示しています。

    $NO_OF_PARALLEL_JOBS=2
    
     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)
    
           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt
    
            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n 
      }
    
    
    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }
    
    
    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }
    
    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy

### <a name="insert-tables-bulkquery"></a>一括挿入 SQL クエリ

[一括挿入 SQL クエリ](https://msdn.microsoft.com/library/ms188365) 行/列ベースのファイルからデータベースにデータをインポートするために使用できます (サポートされる型は、「、[データを一括エクスポートまたはインポート (SQL Server) を準備する](https://msdn.microsoft.com/library/ms188609)) トピックです。

一括挿入用のいくつかのサンプル コマンドを以下に示します。

1. インポートする前に、データを分析して任意のカスタム オプションを設定し、SQL Server データベースの日付などのすべての特別なフィールドが同じ形式になるようにします。 次に、日付形式を年-月-日 (データに年-月-日形式で日付が含まれている場合) に設定する方法の例を示します。

        SET DATEFORMAT ymd; 

2. 一括インポート ステートメントを使用してデータをインポートする:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH 
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )



### <a name="sql-builtin-utilities"></a>SQL Server の組み込みユーティリティ

SQL Server 統合サービス (SSIS) を使用して、フラット ファイルから Azure の SQL Server VM にデータをインポートすることができます。 
SSIS は 2 つの Studio 環境で使用できます。 詳細については、「 [Integration Services (SSIS) と Studio 環境](https://technet.microsoft.com/library/ms140028.aspx):

- SQL Server Data Tools の詳細については、「 [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)
- インポート/エクスポート ウィザードの詳細については、「 [SQL Server インポートおよびエクスポート ウィザード](https://msdn.microsoft.com/library/ms141209.aspx)

## <a name="sqlonprem_to_sqlonazurevm"></a>内部設置型 SQL Server から Azure VM で SQL Server へのデータの移動

次の移行方法を使用することもできます。

1. [Microsoft Azure VM のウィザードに SQL Server データベースの配置します。](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [フラット ファイルにエクスポートします。](#export-flat-file)
3. [SQL Database 移行ウィザード](#sql-migration)
4. [データベースのバックアップと復元](#sql-backup)

それぞれの方法について以下で説明します。

### Microsoft Azure VM への SQL Server データベースのデプロイ ウィザード

**Microsoft Azure VM への SQL Server データベースのデプロイ ウィザード**は、オンプレミスの SQL Server インスタンスから Azure VM の SQL Server にデータを簡単に移行するためのお勧めの方法です。 詳細な手順と、他の方法の詳細については、次を参照してください。 [Azure VM 上の SQL Server にデータベースを移行](../virtual-machines/virtual-machines-migrate-onpremises-database.md)します。

### <a name="export-flat-file"></a>フラット ファイルにエクスポートします。

データを一括エクスポートから内部設置型 SQL Server に記載されているさまざまな方法を使用できる、 [データのエクスポート (SQL Server) の一括インポートおよび](https://msdn.microsoft.com/library/ms175937.aspx) トピックです。 このドキュメントでは、一例として一括コピー プログラム (BCP) について説明します。 データをフラット ファイルにエクスポートした後は、一括インポートを使用して別の SQL Server にそのデータをインポートできます。

1. 次のように bcp ユーティリティを使用して、オンプレミスの SQL Server からファイルにデータをエクスポートします。

    `bcp dbname.tablename datafile.tsv-s servername\sqlinstancename-t-t \t-t \n-c アウト`

2. SQL Server vm を使用して Azure にデータベースとテーブルを作成、 `データベースを作成` と `テーブルを作成する` テーブルのスキーマには、手順 1 でエクスポートします。

3. エクスポート/インポートされているデータのテーブルのスキーマを記述するためのフォーマット ファイルを作成します。 フォーマット ファイルの詳細については、「 [フォーマット ファイル (SQL Server) を作成](https://msdn.microsoft.com/library/ms191516.aspx)します。

    SQL Server マシンから BCP を実行する場合のフォーマット ファイルの生成

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n 

    SQL Server に対して BCP をリモート実行する場合のフォーマット ファイルの生成

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n

4. セクションで説明した方法のいずれかを使用して [ファイル ソースからのデータの移動](#filesource_to_sqlonazurevm) SQL Server にフラット ファイルのデータを移動します。

### <a name="sql-migration"></a>SQL Database 移行ウィザード

[SQL Server データベース移行ウィザード](http://sqlazuremw.codeplex.com/) は 2 つの SQL server インスタンス間でデータを移動するわかりやすい方法を提供します。 これにより、ユーザーは、ソースと移動先テーブルの間のデータ スキーマをマップし、列のタイプおよびその他のさまざまな機能を選択できます。 これは、内部で一括コピー (BCP) を使用します。 次に、SQL データベースの移行ウィザードのようこそ画面のスクリーン ショットを示します。

![SQL Server 移行ウィザード][2]

### <a name="sql-backup"></a>データベースのバックアップと復元

SQL Server は以下のものをサポートします。

1. [データベースのバックアップと復元機能](https://msdn.microsoft.com/library/ms187048.aspx) (両方をローカル ファイルまたは bacpac を blob にエクスポート) と [データ層アプリケーション](https://msdn.microsoft.com/library/ee210546.aspx) (bacpac を使用)。
2. コピーされたデータベースを使用して Azure で SQL Server VM を直接作成する機能、または既存の SQL Azure データベースにコピーする機能。 詳細については、次を参照してください。 [データベース コピー ウィザードを使用して](https://msdn.microsoft.com/library/ms188664.aspx)します。

以下に、SQL Server Management Studio からのデータベースのバックアップ/復元オプションのスクリーン ショットを示します。

![SQL Server インポート ツール][1]

## リソース

[Azure VM の SQL Server にデータベースを移行します。](../virtual-machines/virtual-machines-migrate-onpremises-database.md)

[Azure の仮想マシンの概要 [SQL Server](../virtual-machines/virtual-machines-sql-server-infrastructure-services.md)


[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png 
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png 

