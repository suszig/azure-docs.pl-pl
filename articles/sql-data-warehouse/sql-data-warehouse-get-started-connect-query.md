<properties
   pageTitle="作業開始: Azure SQL Data Warehouse への接続 | Microsoft Azure"
   description="SQL Data Warehouse に接続して、クエリを実行する"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager=""
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/20/2015"
   ms.author="twounder"/>


# Visual Studio を使用した接続とクエリ

> [AZURE.SELECTOR]
- [Visual Studio](sql-data-warehouse-get-started-connect.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md)


このチュートリアルでは、Visual Studio を使用して、Azure SQL Data Warehouse データベースへの接続とクエリを数分で実行する方法について説明します。 このチュートリアルでは次を行います。

+ 前提条件のソフトウェアをインストールする
+ AdventureWorksDW サンプル データベースを含んだデータベースに接続する
+ サンプル データベースに対してクエリを実行する

## 前提条件

+ ダウンロードと Visual Studio 2015 または SSDT のインストール、を参照してください visual Studio 2013/2015 - [Visual Studio のインストールや SSDT](sql-data-warehouse-install-visual-studio.md)。

## 完全修飾 Azure SQL サーバー名を取得します。

データベースに接続するには、サーバーの完全な名前を必要 (*** servername**. **.database.windows.net*) に接続するデータベースが格納されています。

1. 移動して、 [Azure ポータル](https://portal.azure.com)します。
2. 接続先のデータベースを探します。
3. サーバーの完全名を見つけます (これを次の手順で使用します)。

![][1]

## SQL Database に接続する

1. Visual Studio を開きます。
2. [表示] メニューから **SQL Server オブジェクト エクスプローラー**を開きます。

![][2]

3. **[SQL Server の追加]** をクリックします。

![][3]

4. 前の手順で取得した*サーバー名*を入力します。
5. **[認証]** リストで、**[SQL Server 認証]** を選択します。
6. SQL Database サーバーを作成したときに指定した**ログイン情報**と**パスワード**を入力し、**[接続]** をクリックします。

## サンプル クエリの実行

サーバーの登録が完了したので、次はクエリを記述してみましょう。

1. SSDT でユーザー データベースをクリックします。

2. **[新しいクエリ]** をクリックします。 新しいウィンドウが開きます。

![][4]

3. クエリ ウィンドウに次のコードを入力します。

    ```
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. クエリを実行します。

    クエリをクリックして、緑色の矢印を実行または、ショートカット キーを使用します。 `CTRL`+`shift キーを押し`+`E`します。

## 次のステップ

これで接続して、クエリを実行できる、 [PowerBI で接続する][]します。




[connecting with powerbi]: ./sql-data-warehouse-integrate-power-bi.md 
[1]: ./media/sql-data-warehouse-get-started-connect-query/get-server-name.png 
[2]: ./media/sql-data-warehouse-get-started-connect-query/open-ssdt.png 
[3]: ./media/sql-data-warehouse-get-started-connect-query/connection-dialog.png 
[4]: ./media/sql-data-warehouse-get-started-connect-query/new-query.png 

