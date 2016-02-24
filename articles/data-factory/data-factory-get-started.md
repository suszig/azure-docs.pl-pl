<properties
    pageTitle="チュートリアル: Azure Data Factory パイプラインでコピー アクティビティを使用する"
    description="このチュートリアルでは、Azure Data Factory パイプラインでコピー アクティビティを使用して、Azure BLOB から Azure SQL Database にデータをコピーする方法を示します。"
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="11/02/2015"
    ms.author="spelluru"/>

# チュートリアル: Azure BLOB から Azure SQL にデータをコピーする
> [AZURE.SELECTOR]
- [チュートリアルの概要](data-factory-get-started.md)
- [Data Factory エディターを使用します。](data-factory-get-started-using-editor.md)
- [PowerShell の使用](data-factory-monitor-manage-using-powershell.md)
- [Visual Studio を使用](data-factory-get-started-using-vs.md)

この記事のチュートリアルは、Azure Data Factory サービスの使用をスムーズに開始するのに役立ちます。 このチュートリアルでは、Azure Data Factory を作成し、その Data Factory 内にパイプラインを作成して、Azure Blob Storage から Azure SQL Database にデータをコピーする方法について説明します。

> [AZURE.NOTE] Data Factory サービスの詳細については、次を参照してください。、 [Azure Data Factory の概要][data-factory-introduction] 記事です。

##このチュートリアルの前提条件
このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**します。  サブスクリプションがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[無料評価版] [azure の無料評価版] の資料を参照してください。
- **Azure ストレージ アカウント**します。 として blob ストレージを使用する、 **ソース** このチュートリアルではデータを格納します。 Azure ストレージ アカウントを取得していない場合は、次を参照してください。、 [ストレージ アカウントの作成][data-factory-create-storage] 1 つを作成する手順についての記事です。
- **Azure SQL Database**します。 Azure SQL データベースとして使用する、 **宛先** このチュートリアルではデータを格納します。 チュートリアルでは、「」を参照に使用できる Azure SQL database がない場合 [を作成して、Azure SQL データベースを構成する方法][data-factory-create-sql-database] を作成します。
- **SQL Server 2012/2014年または Visual Studio 2013**します。 サンプル データベースを作成して、結果データをデータベースに表示するには、SQL Server Management Studio または Visual Studio を使用します。  

### Azure ストレージ アカウントのアカウント名とアカウント キーを収集する
このチュートリアルの内容を実行するには、Azure ストレージ アカウントのアカウント名とアカウント キーが必要です。 メモしておきます、 **アカウント名** と **アカウント キー** の以下の手順に従って、Azure ストレージ アカウント。

1. [Azure のポータル] [azure ポータル] にログインします。
2. クリックして **参照** ハブ左側に **ストレージ アカウント**します。
3.  **ストレージ アカウント** ブレードで、 **Azure ストレージ アカウント** このチュートリアルで使用します。
4.  **ストレージ** ブレードで、をクリックして **キー** を並べて表示します。
5.  **キーの管理** ブレードで、] をクリックして **コピー** ] の隣に (イメージ) ボタン **ストレージ アカウント名** テキスト ボックスおよび貼り付けて保存場所に (例: テキスト ファイル内)。  
6. 前の手順をコピーまたはメモしておきますを繰り返して、 **プライマリ アクセス キー**します。
7. クリックしてすべてのブレードを閉じます **X**です。

### Azure SQL Database のサーバー名、データベース名、ユーザー アカウントを収集する
このチュートリアルの内容を実行するには、Azure SQL のサーバー名、データベース名、ユーザー名が必要です。 名前を書き留めて **server**, 、**データベース**, 、および **ユーザー** 以下の手順に従って、Azure SQL database の。

1.  **Azure ポータル**, 、] をクリックして **参照** 左側に **SQL データベース**します。
2.  **SQL データベース] ブレード**, を選択、 **データベース** このチュートリアルで使用します。 メモしておきます、 **データベース名**します。  
3.  **SQL DATABASE** ブレードで、をクリックして **プロパティ** を並べて表示します。
4. 値を書き留めて **サーバー名** と **サーバー管理ログイン**します。
5. クリックしてすべてのブレードを閉じます **X**です。

### Azure のサービスから Azure SQL サーバーへのアクセスを許可します。
いることを確認 **Azure サービスへのアクセスを許可する** 設定になっている **ON** 、Azure SQL サーバーの Data Factory サービスは、Azure SQL サーバーでアクセスできるようにします。 この設定を確認してオンにするには、次の手順を実行します。

1. クリックして **参照** ハブをクリックして左 **の SQL server**します。
2. 選択 **サーバー**, 、] をクリック **設定** 上、 **SQL SERVER** ブレードです。
3.  **設定** ブレードで、をクリックして **ファイアウォール**します。
4.  **ファイアウォールの設定** ブレードで、をクリックして **ON** の **Azure サービスへのアクセスを許可する**です。
5. クリックしてすべてのブレードを閉じます **X**です。

### Azure BLOB ストレージと Azure SQL Database をチュートリアル用に準備する
ここからは、次の手順を実行して、チュートリアルで使用する Azure Blob Storage と Azure SQL Database を準備します。  

1. メモ帳を起動し、次のテキストを貼り付け、として保存 **emp.txt** に **C:\ADFGetStarted** 、ハード ドライブ上のフォルダーです。

        John, Doe
        Jane, Doe

2. ツールを使用して [Azure ストレージ エクスプ ローラー](https://azurestorageexplorer.codeplex.com/) を作成する、 **adftutorial** コンテナーにアップロードして、 **emp.txt** コンテナーにファイルです。

    ![Azure ストレージ エクスプローラー](./media/data-factory-get-started/getstarted-storage-explorer.png)
3. 作成する次の SQL スクリプトを使用して、 **emp** Azure SQL データベース内のテーブルです。  


        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

    **If you have SQL Server 2012/2014 installed on your computer:** follow instructions from [Step 2: Connect to SQL Database of the Managing Azure SQL Database using SQL Server Management Studio][sql-management-studio] article to connect to your Azure SQL server and run the SQL script. Note that this article uses the [Azure Classic Portal](http://manage.windowsazure.com), not the [Azure Portal](http://portal.azure.com), to configure firewall for an Azure SQL server.

    **If you have Visual Studio 2013 installed on your computer:** in the [Azure Portal](http://portal.azure.com), click **BROWSE** hub on the left, click **SQL servers**, select your database, and click **Open in Visual Studio** button on toolbar to connect to your Azure SQL server and run the script. If your client is not allowed to access the Azure SQL server, you will need to configure firewall for your Azure SQL server to allow access from your machine (IP Address). See the article above for steps to configure the firewall for your Azure SQL server.


以下の手順を実行します。

- クリックして [Data Factory エディターを使用して](data-factory-get-started-using-editor.md) Azure Classic Portal の一部である Data Factory エディターを使用して、チュートリアルを実行する上部にあるリンクです。
- クリックして [PowerShell を使用した](data-factory-monitor-manage-using-powershell.md) Azure PowerShell を使用して、チュートリアルを実施する上部にあるリンクです。
- クリックして [Visual Studio を使って](data-factory-get-started-using-vs.md) Visual Studio 2013 を使用して、チュートリアルを実行する上部にあるリンクです。
 


<!--Link references-->
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[sql-management-studio]: http://azure.microsoft.com/documentation/articles/sql-database-manage-azure-ssms/#Step2

[monitor-manage-using-powershell]: data-factory-monitor-manage-using-powershell.md
[data-factory-introduction]: data-factory-introduction.md
[data-factory-create-storage]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account
[data-factory-create-sql-database]: ../sql-database-get-started.md 

