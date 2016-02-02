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
- [Tutorial Overview](data-factory-get-started.md)
- [Using Data Factory Editor](data-factory-get-started-using-editor.md)
- [Using PowerShell](data-factory-monitor-manage-using-powershell.md)
- [Using Visual Studio](data-factory-get-started-using-vs.md)


この記事のチュートリアルは、Azure Data Factory サービスの使用をスムーズに開始するのに役立ちます。 このチュートリアルでは、Azure Data Factory を作成し、その Data Factory 内にパイプラインを作成して、Azure Blob Storage から Azure SQL Database にデータをコピーする方法について説明します。
> [AZURE.NOTE] Data Factory サービスの詳細については、次を参照してください。、 [Azure Data Factory の ][data-factory-introduction] 記事です。

## このチュートリアルの前提条件

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**。 サブスクリプションがない場合は、無料試用版のアカウントを数分で作成することができます。 参照してください、 [無料評価版 ][azure-free-trial] の詳細については、資料です。
- **Azure ストレージ アカウント**。 として blob ストレージを使用する、 **ソース** このチュートリアルではデータを格納します。 Azure ストレージ アカウントを取得していない場合を参照してください、 [[データ ファクトリの作成-記憶域] ストレージ アカウントを作成][data-factory-create-storage] 1 つを作成する手順についての記事です。
- **Azure SQL データベース**。 このチュートリアルでは、Azure SQL Database を**ターゲット** データ ストアとして使用します。 チュートリアルでは、「」を参照に使用できる Azure SQL database がない場合 [を作成して Azure SQL Database ][data-factory-create-sql-database] を作成します。
- **SQL Server 2012/2014 または Visual Studio 2013**。 サンプル データベースを作成して、結果データをデータベースに表示するには、SQL Server Management Studio または Visual Studio を使用します。

### Azure ストレージ アカウントのアカウント名とアカウント キーを収集する

このチュートリアルの内容を実行するには、Azure ストレージ アカウントのアカウント名とアカウント キーが必要です。 メモしておきます、 **アカウント名** と **アカウント キー** の以下の手順に従って、Azure ストレージ アカウント。

1. ログイン、 [Azure ポータルの ][azure-portal]します。
2. 左側にある **[参照]** ハブをクリックし、**[ストレージ アカウント]** を選択します。
3. **[ストレージ アカウント]** ブレードで、このチュートリアルで使用する **Azure ストレージ アカウント**を選択します。
4. **[ストレージ]** ブレードで **[キー]** タイルをクリックします。
5. **[キーの管理]** ブレードで、**[ストレージ アカウント名]** テキスト ボックスの隣にある (イメージの) **[コピー]** ボタンをクリックし、任意の場所 (たとえばテキスト ファイル) に貼り付けて保存します。
6. **プライマリ アクセス キー**についても、前のコピー手順を繰り返すか、またはメモしておきます。
7. **[X]** をクリックしてすべてのブレードを閉じます。

### Azure SQL Database のサーバー名、データベース名、ユーザー アカウントを収集する

このチュートリアルの内容を実行するには、Azure SQL のサーバー名、データベース名、ユーザー名が必要です。 次の手順に従って、Azure SQL Database の**サーバー**、**データベース**、**ユーザー**の名前をメモしておきます。

1. **Azure ポータル**の左側にある **[参照]** をクリックし、**[SQL データベース]** を選択します。
2. **[SQL データベース]** ブレードで、このチュートリアルで使用する**データベース**を選択します。 **データベース名**をメモしておきます。
3. **[SQL Database]** ブレードで **[プロパティ]** タイルをクリックします。
4. **[サーバー名]** と **[サーバー管理ログイン]** の値をメモしておきます。
5. **[X]** をクリックしてすべてのブレードを閉じます。

### Azure のサービスから Azure SQL サーバーへのアクセスを許可します。

Data Factory サービスから Azure SQL サーバーにアクセスできるように、Azure SQL サーバーで **[Azure サービスへのアクセスを許可する]** の設定が**オン**になっていることを確認します。 この設定を確認してオンにするには、次の手順を実行します。

1. 左側にある **[参照]** ハブをクリックし、**[SQL サーバー]** をクリックします。
2. **お使いのサーバー**を選択し、**[SQL サーバー]** ブレードで **[設定]** をクリックします。
3. **[設定]** ブレードで **[ファイアウォール]** をクリックします。
4. **[ファイアウォールの設定]** ブレードの **[Azure サービスへのアクセスを許可する]** で **[オン]** をクリックします。
5. **[X]** をクリックしてすべてのブレードを閉じます。

### Azure Blob Storage と Azure SQL Database をチュートリアル用に準備する

ここからは、次の手順を実行して、チュートリアルで使用する Azure Blob Storage と Azure SQL Database を準備します。

1. メモ帳を起動し、次のテキストを貼り付け、**emp.txt** という名前でハード ドライブの **C:\ADFGetStarted** フォルダーに保存します。

        John, Doe
        Jane, Doe

2. ツールを使用して [Azure ストレージ エクスプ ローラー](https://azurestorageexplorer.codeplex.com/) を作成する、 **adftutorial** コンテナーにアップロードして、 **emp.txt** コンテナーにファイルです。

    ![Azure Storage Explorer](./media/data-factory-get-started/getstarted-storage-explorer.png)
3. 次の SQL スクリプトを使用して、**emp** テーブルを Azure SQL Database に作成します。

     CREATE TABLE dbo.emp
     (
         ID int IDENTITY(1,1) NOT NULL,
         FirstName varchar(50),
         LastName varchar(50),
     )
     GO
    
     CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

 **SQL Server 2012/2014 がコンピューターにインストールする必要があるかどうか:** からの指示に従って [手順 2: の Azure SQL Database の管理 SQL Server Management Studio ][sql-management-studio] 資料、Azure SQL サーバーに接続し、SQL スクリプトを実行します。 この記事ではメモ、 [Azure Classic Portal](http://manage.windowsazure.com), ではなく、 [Azure ポータル](http://portal.azure.com), 、Azure SQL サーバーのファイアウォールを構成します。

 **お使いのコンピューターにインストールされている Visual Studio 2013 があるかどうか:** で、 [Azure ポータル](http://portal.azure.com), 、] をクリックして **参照** ハブで、左クリック **の SQL server**, 、データベースを選択し、クリックして **Visual Studio で開く** 、Azure SQL サーバーに接続し、スクリプトを実行するにはツールバーにボタンをクリックします。 クライアントから Azure SQL サーバーへのアクセスが許可されていない場合は、コンピューター (IP アドレス) からのアクセスを許可するように、Azure SQL Server のファイアウォールを構成する必要があります。 Azure SQL サーバーのファイアウォールを構成する手順については、上の記事を参照してください。


以下の手順を実行します。

- クリックして [Data Factory エディターを使用して](data-factory-get-started-using-editor.md) Azure Classic Portal の一部である Data Factory エディターを使用して、チュートリアルを実行する上部にあるリンクです。
- クリックして [PowerShell を使用した](data-factory-monitor-manage-using-powershell.md) Azure PowerShell を使用して、チュートリアルを実施する上部にあるリンクです。
- クリックして [Visual Studio を使って](data-factory-get-started-using-vs.md) Visual Studio 2013 を使用して、チュートリアルを実行する上部にあるリンクです。





[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/ 
[azure-portal]: https://portal.azure.com/ 
[sql-management-studio]: http://azure.microsoft.com/documentation/articles/sql-database-manage-azure-ssms/#Step2 
[monitor-manage-using-powershell]: data-factory-monitor-manage-using-powershell.md 
[data-factory-introduction]: data-factory-introduction.md 
[data-factory-create-storage]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account 
[data-factory-create-sql-database]: ../sql-database-get-started.md 

