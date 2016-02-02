<properties
   pageTitle="SQL Data Warehouse へのサンプル データのロード | Microsoft Azure"
   description="SQL Data Warehouse にサンプル データをロードする方法を説明します"
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
   ms.date="10/21/2015"
   ms.author="lodipalm;barbkess"/>


# SQL Data Warehouse へのサンプル データのロード

SQL Data Warehouse インスタンスを作成するときに、何らかのサンプル データを簡単に読み込むことができます。 プロビジョニング中にこの手順をされなかった場合にすることもできます [サンプル データを手動で読み込む:operator[]][]します。

ここでは、AdventureWorksDW をデータベースに読み込む方法の概要を説明します。 このデータセットは、AdventureWorks という架空の企業のサンプル データ ウェアハウス構造を元に作られています。データは主にこの会社の売上と顧客に関するものです。

## 作成時にサンプル データを追加する

次の手順で、デプロイメント中にサンプル データを SQL Data Warehouse に読み込むことができます。

1. 作成プロセスを開始するには、SQL Data Warehouse での検索、 [Azure 旧ポータルの][] をクリックして [+ 新規] と then 'のデータとストレージ' または ' SQL Data Warehouse' を検索して、Marketplace にします。

2. プロセスが開始されたら、[ソースの選択] をクリックして、[サンプル] に設定します。 新しいサーバーを作成しない場合は、作成に使用しているサーバーのログイン情報の入力も求められます。


> [AZURE.NOTE] インスタンスにサンプル データを読み込むには、Azure サービスでサーバーへのアクセスを有効にする必要があります (新しいサーバーを作成する場合の既定では有効です)。 これを行わない場合、読み込みは失敗すると、ことはできます [サンプル データを手動で読み込む:operator[]][]します。


## PowerBI を使用して Adventureworks を分析する

初めて PowerBI を使用するときは、サンプル データ セットを使用することをお勧めします。 サンプル データを読み込んだ後、Azure クラシック ポータルで Power BI で開く] ボタンをクリックするかをいずれかの SQL Data Warehouse への接続を開くことができます [Power BI の][]  と [SQL Data Warehouse に接続する][]します。 接続後は、データ ウェアハウスと同じ名前で新しいデータセットが作成されます。 分析を簡単にするために、ここでは、会社の売上分析に重要ないくつかのメトリックを含む 'AggregateSales' というビューを作成しました。 このビューの名前をクリックすると、ビューが展開されて含まれる列が表示されます。次の手順で、いくつかの簡単な視覚エフェクトを作成できます。

1. まず、[PostalCode] 列と [SalesAmount] 列をクリックして、全売上のマップを簡単に作成できます。 Power BI では、このデータを地理データとして自動的に認識し、マップに配置することができます。

2. 売上の棒グラフを作成するには、[SalesAmount] 列をクリックすると、自動的に作成されます。 [CustomerIncome] グラフを [AggregateSales] の左にある [Axis] フィールドにドラッグすると、次元が追加され、顧客の収入ブラケット別の売上が表示されます。

3. 最後に、売上のタイムラインを作成する場合、必要な操作は、[SalesAmount]、[OrderDate]、および [折れ線グラフ] ([視覚エフェクト] の 2 行目の最初のアイコン) のクリックのみです。

左上にある [保存] をクリックし、視覚エフェクトをレポートとして保存することで、進行状況をいつでも保存できます。

## サンプルへの接続とクエリ

また、従来の方法を使用してサンプル データを分析することもできます。 」の説明に従って、 [接続し、クエリの][] ドキュメントを Visual Studio での SQL Server Data Tools を使用してこのデータベースに接続することができます。 SQL Data Warehouse にサンプル データをロードしたら、いくつかのクエリの実行をすぐに開始できます。

簡単な SELECT ステートメントを実行して、従業員のすべての情報を取得できます。

    SELECT * FROM DimEmployee;

また、GROUP BY などのコンストラクトを使用する、より複雑なクエリを実行して、日ごとの総売上金額の合計を参照することもできます。

    SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
    FROM FactInternetSales
    GROUP BY OrderDateKey
    ORDER BY OrderDateKey;

WHERE 句を使用して、ある日付以前の注文をフィルター処理できます。

    SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
    FROM FactInternetSales
    WHERE OrderDateKey > '20020801'
    GROUP BY OrderDateKey
    ORDER BY OrderDateKey;

実際には、SQL Data Warehouse はほぼすべての SQL Server は、T-SQL コンストラクトをサポートしているし、の違いの一部を見つけることができます、 [コードを移行][] ドキュメントです。



## 次のステップ

これでしばらくながらウォーミング アップしてサンプル データのチェック アウトする方法を紹介しました [を開発][], 、[を読み込む][], 、または [を移行][]します。







[migrate]: ./sql-data-warehouse-overview-migrate.md 
[develop]: ./sql-data-warehouse-overview-develop.md 
[load]: ./sql-data-warehouse-overview-load.md 
[connect and query]: ./sql-data-warehouse-get-started-connect.md 
[migrate code]: ./sql-data-warehouse-migrate-code.md 
[load sample data manually]: ./sql-data-warehouse-get-started-manually-load-samples.md 
[azure classic portal]: https://portal.azure.com/ 
[power bi]: http://www.powerbi.com/ 
[connecting to sql data warehouse]: ./sql-data-warehouse-integrate-power-bi.md 
[microsoft command line utilities for sql server]: http://www.microsoft.com/download/details.aspx?id=36433/ 
[sample data scripts]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksPDW2012.zip/ 

