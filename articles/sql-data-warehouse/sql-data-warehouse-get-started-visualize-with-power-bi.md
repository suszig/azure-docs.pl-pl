<properties
    pageTitle="SQL Data Warehouse のデータを Power BI で視覚化する | Microsoft Azure"
    description="SQL Data Warehouse のデータを Power BI で視覚化します。"
    services="sql-data-warehouse"
    documentationCenter="NA"
    authors="lodipalm"
    manager="barbkess"
    editor=""/>

<tags
    ms.service="sql-data-warehouse"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-services"
    ms.date="10/21/2015"
    ms.author="lodipalm"/>

# Power BI でデータを視覚化する


> [AZURE.NOTE] このチュートリアルを完了するには、AdventureWorksDW サンプル データベースを事前に読み込まれている SQL Data Warehouse データベースが必要です。 [SQL Data Warehouse を作成する](sql-data-warehouse-get-started-provision.md) 1 つを作成する方法を示します。 
> 
> 場合は、既に SQL Data Warehouse データベースがあるが、サンプル データがない、 [サンプル データを手動で読み込む][]です。

このチュートリアルでは、Power BI を使用して、SQL Data Warehouse に接続し、いくつかの基本的な視覚化を作成する方法を示します。

> [AZURE.VIDEO azure-sql-data-warehouse-sample-data-and-powerbi]

## AdventureWorksDW に接続する

Power BI を開き、AdventureWorksDW データベースに接続するには:

1. サインイン、 [Azure ポータル][]します。
2. クリックして **SQL データベース** し、AdventureWorks SQL Data Warehouse データベースを選択します。 

    ![データベースの検索][1]

3. [Power BI で開く] ボタンをクリックします。

    ![Power BI ボタン][2]

4. SQL Data Warehouse の接続ページにデータベースの Web アドレスが表示されます。 [次へ] をクリックします。

    ![Power BI connection][3]

6. Azure SQL Server のユーザー名とパスワードを入力すると、SQL Data Warehouse データベースに完全に接続されます。 

    ![Power BI sign in][4]

1. Power BI にサインインしたら、左側のブレードの AdventureWorksDW データセットをクリックします。 これでデータベースが開きます。 

    ![Power BI open AdventureWorksDW][5]



## Power BI レポートを作成してサンプル データを分析する

Power BI を使用して AdventureWorksDW サンプル データを分析する準備が整いました。 分析を実行するため、AdventureWorksDW には AggregateSales という名前のビューがあります。 このビューには、会社の売上高を分析するための重要なメトリックがいくつか含まれています。 

1. 郵便番号に基づく売上高の地図を作成するには、右側のフィールド ペインにある [AggregateSales] ビューをクリックして展開します。 [PostalCode] 列と [SalesAmount] 列をクリックして選択します。 

    ![Power BI select AggregateSales][6] 

    Power BI はこのデータを地理データとして自動的に認識し、マップに配置します。

    ![Power BI マップ][7]

2. この手順では、顧客の所得ごとの売上高を表示する棒グラフを作成します。 これを作成するには、展開した AggregateSales ビューにアクセスします。 [SalesAmount] フィールドをクリックします。 [Customer Income] フィールドを左方向へドラッグし、[軸] にドロップします。
    
    ![Power BI select axis][8]

    棒グラフは左側に移動しました。

    ![Power BI バー][9]

3. この手順では、発注日ごとの売上高を表示する折れ線グラフを作成します。 これを作成するには、展開した AggregateSales ビューにアクセスします。 [SalesAmount] と [OrderDate] をクリックします。 [視覚化] 列で、折れ線グラフのアイコンをクリックします。"視覚化" の下の 2 行目先頭にあるアイコンです。

    ![Power BI select line chart][10]

    これで、同じデータを 3 とおりの方法で視覚化したレポートが完成しました。

    ![Power BI ライン][11]

クリックして、いつでも、進行状況を保存できる **ファイル** を選択すると、 **保存**します。

## 次のステップ
これでサンプル データを含む [ウォーム アップをしばらく与えたを参照してくださいハウツー [開発][], 、[ロード][], 、または [移行][]します。

<!--Image references-->
[1]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migrate]: ./sql-data-warehouse-overview-migrate.md
[develop]: ./sql-data-warehouse-overview-develop.md
[load]: ./sql-data-warehouse-overview-load.md
[load sample data manually]: ./sql-data-warehouse-get-started-manually-load-samples.md
[Azure Portal]: https://portal.azure.com/
[Power BI]: http://www.powerbi.com/
[connecting to SQL Data Warehouse]: ./sql-data-warehouse-integrate-power-bi.md
[Create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md


