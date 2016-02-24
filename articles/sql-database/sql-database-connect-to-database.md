<properties
   pageTitle="SSMS を使用して Azure SQL Database に接続する方法 | Microsoft Azure"
   description="SSMS を使用して Azure SQL Database に接続する方法について説明します。"
   services="sql-database"
   documentationCenter=""
   authors="sidneyh"
   manager="jeffreyg"
   editor=""
   tags=""/>
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/14/2015"
   ms.author="sidneyh"/>

# SQL Server Management Studio (SSMS) での接続

次の手順に従って、SQL Server Management Studio (SSMS) を使用して SQL Database に接続し、クエリを実行します。

## 前提条件

* SQL Server Management Studio SSMS での最新バージョンをダウンロードする (SSMS) - 参照 [SQL Server Management Studio のダウンロード](https://msdn.microsoft.com/library/mt238290.aspx)します。
* AdventureWorks サンプル データベース」の説明に従って [Microsoft Azure SQL Database の概要](sql-database-get-started.md)します。


## 完全修飾 Azure SQL サーバー名を取得します。

データベースに接続するには、サーバーの完全な名前を必要 (***servername**..database.windows.net を付けて*) に接続するデータベースが格納されています。

1. 移動して、 [Azure ポータル](https://portal.azure.com)します。
2. 接続先のデータベースを探します。
3. サーバーの完全名を見つけます。

    ![完全修飾サーバー名][6]

    下の手順 3 でサーバーの完全修飾名を使用します。



## SQL Database に接続する

1. SSMS を開きます。
2. クリックして **接続** > **データベース エンジンをしています.**

    ![[接続] > [データベース エンジン]][7]

2.  **サーバーへの接続** ダイアログ ボックスで、 **サーバー名** ボックスで、サーバー名の形式で入力 *& lt; サーバー名 >*.**.database.windows.net を付けて**します。
3.  **認証** 一覧で、[ **SQL Server 認証**します。
4. 入力、 **ログイン** と **パスワード** ] をクリックして、SQL データベース サーバーの作成時に指定した **接続**します。

    ![[サーバーに接続] ダイアログ][2]



### 接続が失敗した場合
作成した論理サーバーのファイアウォールによって、ローカル コンピューターからの接続が許可されていることを確認します。 詳細については、次を参照してください。 [方法: SQL データベースのファイアウォール設定を構成する](sql-database-configure-firewall-settings.md)です。

## サンプル クエリの実行

1.  **オブジェクト エクスプ ローラー**, に移動し、 **AdventureWorks** データベースです。
2. データベースを右クリックし、[ **新しいクエリ**します。

    ![新しいクエリ][4]

3. クエリ ウィンドウに、次のコードをコピーして貼り付けます。

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. クリックして、 **Execute** ] ボタンをクリックします。  次のスクリーン ショットに、成功したクエリを示します。

    ![成功][5]




## 次のステップ
Transact-SQL ステートメントを使用してデータベースを作成、管理することができます。 詳細については、次を参照してください。 [CREATE DATABASE (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) と [Azure SQL Database の管理 SQL Server Management Studio を使用して](sql-database-manage-azure-ssms.md)します。 イベントを Azure ストレージに記録することもできます。 参照してください [SQL database の監査を使ってみる](sql-database-auditing-get-started.md) の詳細。

<!--Image references-->

[1]:./media/sql-database-connect-to-database/1-download.png
[2]:./media/sql-database-connect-to-database/2-connect.png
[3]:./media/sql-database-connect-to-database/3-connect-to-database.png
[4]:./media/sql-database-connect-to-database/4-run-query.png
[5]:./media/sql-database-connect-to-database/5-success.png
[6]:./media/sql-database-connect-to-database/server-name.png
[7]:./media/sql-database-connect-to-database/connect-dbengine.png

