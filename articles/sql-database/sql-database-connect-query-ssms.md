<properties
    pageTitle="SSMS を使用した SQL データベースへの接続 | Microsoft Azure"
    description="SQL Server Management Studio (SSMS) を使用した Azure SQL データベースへの接続方法について説明します。 次に、TRANSACT-SQL (T-SQL) を使用して、サンプル クエリを実行します。"
    metaCanonical=""
    keywords="connect to sql database,sql server management studio"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jeffreyg"
    editor="" />

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/09/2015"
    ms.author="sstein" />

# SQL Server Management Studio を使用して SQL Database に接続し、T-SQL サンプル クエリを実行する

> [AZURE.SELECTOR]
- [C#](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

この記事では、SQL Server Management Studio (SSMS) を使用して Azure SQL データベースに接続し、TRANSACT-SQL (T-SQL) ステートメントを使用して簡単なクエリを実行する方法を示します。

まず、Azure に SQL データベースが必要です。 1 つの手順に簡単に作成できます [Microsoft Azure SQL Database の概要](sql-database-get-started.md)します。 ここでの例は、その記事で作った AdventureWorks のサンプル データベースに基づいていますが、クエリの実行までの同じ手順を他の SQL データベースに対して適用できます。

## SQL Server Management Studio (SSMS) のインストールと起動

SQL Database で作業を行う場合、最新のバージョンの SSMS を使用する必要があります。 参照してください [SQL Server Management Studio のダウンロード](https://msdn.microsoft.com/library/mt238290.aspx) それを取得します。 最新のバージョンであれば、最新の更新が利用可能な場合に SSMS が自動的に通知します。

## SSMS を起動し、SQL データベース サーバーに接続する

1. Windows 検索ボックスで、「Microsoft SQL Server Management Studio」と入力し、デスクトップ アプリをクリックして SSMS を起動します。
2.  **サーバーへの接続** ダイアログ ボックスで、 **サーバー名** ボックス形式で、SQL データベースをホストするサーバーの名前を入力 *& lt; サーバー名 >*.**.database.windows.net を付けて**します。
3. 選択 **SQL Server 認証** から、 **認証** ] ボックスの一覧です。
4. 型、 **ログイン** と **パスワード** 、サーバーを作成したときに設定して、順にクリックして **接続** SQL データベースに接続します。

    ![SQL Server Management Studio: SQL Database サーバーへの接続](./media/sql-database-connect-query-ssms/1-connect.png)

### SQL Database への接続が失敗した場合

接続の失敗の最も一般的な原因は、サーバー名、ユーザー名、パスワードの間違いや、サーバーがセキュリティ上の理由で接続を許可しないといったものです。 サーバーのファイアウォールの設定が、ローカルのコンピューターの IP アドレスからの接続や SSMS クライアントが使用する IP アドレスからの接続を許可するようになっているか確認してください。 この 2 つの IP アドレスが異なることがあります。

ファイアウォール設定が原因で接続が失敗した場合、最新バージョンの SSMS では、ユーザーへの確認の後、ファイアウォール規則が作成されます。 これを取得するには、次を参照してください。 [ダウンロード SSMS](https://msdn.microsoft.com/library/mt238290.aspx)します。 以前のバージョンを使用している場合は、エラー メッセージで IP アドレスが報告されます。この IP アドレスをサーバーのファイアウォール規則に追加する必要があります。 詳細については、次を参照してください。 [方法: ファイアウォールの設定を構成する (Azure SQL データベース)](sql-database-configure-firewall-settings.md)します。

## サンプル クエリの実行

SQL Database に接続すると、サンプル クエリを実行できるようになります。 AdventureWorks のサンプルを使用してデータベースを作成していないかどうかは [Microsoft Azure SQL Database を使ってみる](sql-database-get-started.md), 、このクエリは機能しません。 学習を進めるには、飛ばして次のステップに進んでください。

1.  **オブジェクト エクスプ ローラー**, に移動し、 **AdventureWorks** データベースです。
2. データベースを右クリックし、[ **新しいクエリ**します。

    ![新しいクエリ](./media/sql-database-connect-query-ssms/4-run-query.png)

3. クエリ ウィンドウに、次のコードをコピーして貼り付けます。

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. クリックして、 **Execute** ] ボタンをクリックします。  次のスクリーン ショットに、成功したクエリを示します。

    ![成功](./media/sql-database-connect-query-ssms/5-success.png)

## 次のステップ

SQL Server で可能な方法とほぼ同じように、T-SQL ステートメントを使用して Azure にデータベースを作成して管理することもできます。 SQL Server で T-SQL の使用に詳しい方ならを参照してください。 [Azure SQL Database TRANSACT-SQL 情報)](sql-database-transact-sql-information.md) の相違点の概要についてです。

T-SQL の初めての場合は、次を参照してください。 [チュートリアル: TRANSACT-SQL ステートメントの作成](https://msdn.microsoft.com/library/ms365303.aspx) と [TRANSACT-SQL リファレンス (データベース エンジン)](https://msdn.microsoft.com/library/bb510741.aspx)します。

