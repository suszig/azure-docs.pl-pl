<properties
   pageTitle="SQL Data Warehouse へのサンプル データのロード | Microsoft Azure"
   description="SQL Data Warehouse へのサンプル データのロード"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="11/02/2015"
   ms.author="lodipalm;barbkess"/>

#SQL Data Warehouse へのサンプル データのロード

[作成した SQL Data Warehouse データベース インスタンス] した後 [SQL Data Warehouse データベース インスタンスの作成]、次の手順を作成し、いくつかのテーブルを読み込みます。  SQL Data Warehouse 用に作成されている Adventure Works サンプル スクリプトを使用して、架空の会社 Adventure Works のテーブルを作成してロードできます。  これらのスクリプトは、sqlcmd を使用して SQL を実行し、bcp を使用してデータをロードします。  これらのツールがインストールされていない場合は、次のリンク [install bcp] をと [sqlcmd のインストール] を。

次の簡単な手順で Adventure Works サンプル データベースを SQL DW にロードします。

1. [SQL Data Warehouse の Adventure Works サンプル スクリプト] をダウンロードします。

2. ダウンロードした zip からローカル コンピューターのディレクトリにファイルを抽出します。

3. 抽出したファイル aw_create.bat を編集し、ファイルの先頭にある以下の変数を設定します。  "=" とパラメーターの間にスペースを入れないようにします。  編集内容の例を次に示します。

        server=mylogicalserver.database.windows.net
        user=mydwuser
        password=Mydwpassw0rd
        database=mydwdatabase

4. Windows コマンド プロンプトから編集した aw_create.bat を実行します。  編集バージョンの aw_create.bat を保存したディレクトリにいることを確認します。
このスクリプトでは、次のことが行われます。
    * データベースに既に存在する Adventure Works のテーブルまたはビューを削除します
    * Adventure Works のテーブルとビューを作成します
    * bcp を使用して Adventure Works の各テーブルをロードします
    * Adventure Works の各テーブルの行数を検証します
    * Adventure Works の各テーブルのすべての列の統計情報を収集します


##サンプル データのクエリ

SQL Data Warehouse にサンプル データをロードしたら、いくつかのクエリをすぐに実行できます。  クエリを実行するには、データベースへの接続、新しく作成された Adventure Works Visual Studio と SSDT を使用して Azure SQL DW の」の説明に従って、[connect] ドキュメントです。

従業員のすべての情報を取得する簡単な SELECT ステートメントの例:

    SELECT * FROM DimEmployee;

GROUP BY などのコンストラクトを使用する、より複雑なクエリを実行して、日ごとの総売上金額の合計を参照する例:

    SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
    FROM FactInternetSales
    GROUP BY OrderDateKey
    ORDER BY OrderDateKey;

SELECT と WHERE 句を使用して、ある日付以前の注文をフィルター処理する例:

    SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
    FROM FactInternetSales
    WHERE OrderDateKey > '20020801'
    GROUP BY OrderDateKey
    ORDER BY OrderDateKey;

SQL Data Warehouse は、SQL Server がサポートするほぼすべての T-SQL 構造をサポートします。  違いが記載されている、[コードの移行] ドキュメントです。

## 次のステップ
これでサンプル データを含むいくつかのクエリを試用する機会がある得られたら、[読み込み] を [] を開発する方法を確認するか SQL Data Warehouse への [移行]。

<!--Image references-->

<!--Article references-->
[migrate]: ./sql-data-warehouse-overview-migrate.md
[develop]: ./sql-data-warehouse-overview-develop.md
[load]: ./sql-data-warehouse-overview-load.md
[connect]: ./sql-data-warehouse-get-started-connect.md
[migrate code]: ./sql-data-warehouse-migrate-code.md
[create a SQL Data Warehouse database instance]: ./sql-data-warehouse-get-started-provision.md
[install bcp]: ./sql-data-warehouse-load-with-bcp.md
[install sqlcmd]: ./sql-data-warehouse-get-started-connect-query-sqlcmd.md

<!--Other Web references-->
[Adventure Works Sample Scripts for SQL Data Warehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip

