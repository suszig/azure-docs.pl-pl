<properties
   pageTitle="Visual Studio で SQL Data Warehouse に接続する | Microsoft Azure"
   description="SQL Data Warehouse に接続して、クエリを実行する"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/22/2015"
   ms.author="twounder;barbkess"/>

# Visual Studio で SQL Data Warehouse に接続する

> [AZURE.SELECTOR]
- [Visual Studio](sql-data-warehouse-get-started-connect.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md)

このチュートリアルでは、Visual Studio の SQL Server Data Tools を使用して、Azure SQL Data Warehouse データベースにわずか数分で接続する方法について説明します。  接続後に、簡単なクエリを実行します。

## 前提条件

+ SQL Data Warehouse に含まれる AdventureWorksDW サンプル データベース。 これを作成するを参照してください。 [SQL Data Warehouse データベースを作成する](sql-data-warehouse-get-started-create.md)です。 
+ Visual Studio の SQL Server Data Tools。 インストール手順とオプションでは、次を参照してください [Visual Studio のインストールや SSDT。](sql-data-warehouse-install-visual-studio.md)

## 手順 1. Azure SQL サーバーの完全修飾名を検索する

データベースは、Azure SQL サーバーに関連付けられます。 データベースに接続するには、サーバーの完全修飾名を必要 (**servername**. **.database.windows.net*)。

完全修飾サーバー名を検索するには

1. 移動して、 [Azure ポータル](https://portal.azure.com)します。
2. クリックして **SQL データベース** に接続するデータベース] をクリックします。 この例では、AdventureWorksDW サンプル データベースを使用します。
3. サーバーの完全名を見つけます。

    ![Full server name][1]

## 手順 2. SQL Database に接続する

1. Visual Studio を開きます。
2. SQL Server オブジェクト エクスプローラーを開きます。 これを行うには、次のように選択します。 **ビュー** > **SQL Server オブジェクト エクスプ ローラー**します。
 
    ![SQL Server Object Explorer][2]

3. クリックして、 **SQL Server の追加** アイコン。

    ![Add SQL Server][3]

1. [サーバーへの接続] ウィンドウのフィールドに入力します。

    ![サーバーへの接続][4]

    - **サーバー名**します。 入力、 *サーバー名* 以前であることです。
    - **認証**します。 [SQL Server 認証] を選択します。
    - **ログイン** と **パスワード**します。 Azure SQL Server のログインとパスワードを入力します。
    - クリックして **接続**します。

1. 確認のために、Azure SQL Server を展開します。 サーバーに関連付けられているデータベースが表示されます。 AdventureWorksDW を展開すると、サンプル データベース内のテーブルが表示されます。

    ![Explore AdventureWorksDW][5]


## 手順 3. サンプル クエリを実行する

サーバーへの接続が完了したので、次はクエリを記述してみましょう。 

1. SQL Server オブジェクト エクスプローラーでデータベースを右クリックします。 

2. 選択 **新しいクエリ**します。 新しいクエリ ウィンドウが開きます。

    ![新しいクエリ][6]

3. 次の TSQL クエリをクエリ ウィンドウにコピーします。

    ```
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. クエリを実行します。 そのためには、緑色の矢印をクリックするか、`CTRL`+`SHIFT`+`E` のショートカット キーを使用します。

    ![Run query][7]

1. クエリ結果を確認します。 この例では、FactInternetSales テーブルに 60,398 行が含まれています。

    ![Query results][8]

## 次のステップ

これで接続して、クエリを実行できる、 [PowerBI でデータを視覚化する][]です。

[visualizing the data with PowerBI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md  


<!--Image references-->

[1]: ./media/sql-data-warehouse-get-started-connect/get-server-name.png
[2]: ./media/sql-data-warehouse-get-started-connect/open-ssdt.png
[3]: ./media/sql-data-warehouse-get-started-connect/add-server.png
[4]: ./media/sql-data-warehouse-get-started-connect/connection-dialog.png
[5]: ./media/sql-data-warehouse-get-started-connect/explore-sample.png
[6]: ./media/sql-data-warehouse-get-started-connect/new-query2.png
[7]: ./media/sql-data-warehouse-get-started-connect/run-query.png
[8]: ./media/sql-data-warehouse-get-started-connect/query-results.png
