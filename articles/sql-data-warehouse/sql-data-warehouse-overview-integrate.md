<properties
   pageTitle="SQL Data Warehouse を使用した統合ソリューションの作成 | Microsoft Azure"
   description="SQL Data Warehouse と統合されたソリューションを持つツールとパートナー。 "
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
   ms.date="09/22/2015"
   ms.author="lodipalm"/>

#SQL Data Warehouse と他のサービスの併用
SQL Data Warehouse では、コア機能に加えて Azure 内の他のサービスの多くを並行して活用できます。  具体的には、現在のところ、次のサービスと密接に統合するための措置を講じています。

+ Power BI
+ Azure Data Factory
+ Azure Machine Learning
+ Azure Stream Analytics

今後、Azure エコシステムでさらに多くのサービスと接続できるようになる予定です。

##Power BI
Power BI との統合によって、SQL Data Warehouse のコンピューティング パワーを、Power BI の動的なレポート作成や視覚化と併せて活用することができます。 現在、Power BI との統合には次のものが含まれています。 

+ **Direct Connect**: SQL Data Warehouse に対する論理的プッシュ ダウンと接続する高度なです。  これにより、分析の速度と規模を高めることができます。
+ **Power BI で開く**: [Power BI で開く] ボタンよりシームレスな接続の Power BI にインスタンス情報を渡します。 

参照してください [Power BI との統合](../sql-data-warehouse-integrate-power-bi.md) または [Power BI ドキュメント](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) の詳細。

##Azure Data Factory
Azure Data Factory は、抽出 - 読み込みの複雑なパイプラインを作成するための管理されたプラットフォームを提供します。  SQL Data Warehouse と Azure Data Factory の統合には、次が含まれます。

+ **ストアド プロシージャ**: SQL Data Warehouse でストアド プロシージャの実行を調整します。

参照してください [統合して、Azure Data Factory を使って](../sql-data-warehouse-integrate-azure-data-factory.md) または [Azure Data Factory のドキュメント](https://azure.microsoft.com/documentation/services/data-factory/) の詳細。

##Azure Machine Learning
Azure Machine Learning は、完全に管理されたを分析サービスで、大規模な予測ツール セットを活用する複雑なモデルを作成できます。  SQL Data Warehouse は、次の機能により、これらのモデルのソースと変換先の両方としてサポートされています。

+ **データの読み取り:** T-SQL を使用して、SQL Data Warehouse に対して大規模にモデルを駆動します。 
+ **データの書き込み:** コミットが任意のモデルから SQL Data Warehouse に変更します。

参照してください [Azure Machine Learning との統合](../sql-data-warehouse-integrate-azure-machine-learning.md) または [Azure Machine Learning ドキュメント](https://azure.microsoft.com/services/machine-learning/) の詳細。

##Azure Stream Analytics
Azure Stream Analytics は、Azure Event Hub で生成されたイベント データの処理と使用のための、複雑で完全に管理されたインフラストラクチャです。  SQL Data Warehouse との統合により、ストリーミング データを効率的に処理でき、さらに、関連するデータと並行して格納できることにより、より深く高度な分析が可能になります。  

+ **ジョブの出力:** Stream Analytics ジョブから SQL Data Warehouse に直接出力を送信します。

参照してください [Azure Stream Analytics との統合](../sql-data-warehouse-integrate-azure-stream-analytics.md) または [Azure Stream Analytics のドキュメント](https://azure.microsoft.com/documentation/services/stream-analytics/) の詳細。

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-integrate-solution-partners.md

<!--MSDN references-->

<!--Other Web references-->

