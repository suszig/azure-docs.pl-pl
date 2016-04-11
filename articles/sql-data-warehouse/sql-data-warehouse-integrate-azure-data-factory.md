<properties
   pageTitle="SQL Data Warehouse での Azure Data Factory の使用 | Microsoft Azure"
   description="ソリューション開発のための、Azure SQL Data Warehouse での Azure Data Factory (ADF) の使用に関するヒント。"
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
   ms.date="09/23/2015"
   ms.author="lodipalm"/>

# SQL Data Warehouse での Azure Data Factory の使用

Azure Data Factory では、SQL Data Warehouse におけるデータ転送とストアド プロシージャの実行を、完全に管理された方法で調整できます。  これにより、複雑な抽出、変換、および読み込み (ETL) プロシージャを、SQL Data Warehouse を使用して簡単に設定およびスケジュールできます。 Azure Data Factory の概要については、次を参照してください。、 [Azure Data Factory のドキュメント][]します。

## データの移動 

Azure Data Factory により、オンプレミス ソースとさまざまな Azure サービスの両方の間でデータを移動できます。  Azure Data Factory との現在の包括的統合では、次の場所との間でのデータ移動がサポートされています。

+ Azure BLOB ストレージ
+ Azure SQL データベース
+ 内部設置型 SQL Server
+ IaaS 上の SQL Server

コピー アクティビティ データを設定する方法の詳細については「 [Azure Data Factory を使ってデータをコピー](../data-factory/data-factory-data-movement-activities.md)します。

## ストアド プロシージャ
 Azure Data Factory を使用すると、データ転送のスケジュールを設定できますが、それと同様に、ストアド プロシージャの実行を調整する際に使用することもできます。  これにより、さらに複雑なパイプラインを作成でき、SQL Data Warehouse の演算性能を利用する Azure Data Factory の機能も拡張されます。

## 次のステップ
統合の概要については、次を参照してください。 [SQL Data Warehouse の統合の概要](sql-data-warehouse-overview-integrate.md)します。
他の開発のヒントを参照してください。 [SQL Data Warehouse の開発の概要](sql-data-warehouse-overview-develop.md)します。

<!--Image references-->

<!--Article references-->

[Copy data with Azure Data Factory]:https://azure.microsoft.com/en-us/documentation/articles/data-factory-azure-sql-connector/ 
[SQL Data Warehouse development overview]:https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-overview-develop/ 
[SQL Data Warehouse integration overview]:https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-overview-integrate/

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory documentation]:https://azure.microsoft.com/documentation/services/data-factory/
[Copy data with Azure Data Factory]:https://azure.microsoft.com/en-us/documentation/articles/data-factory-data-movement-activities/


