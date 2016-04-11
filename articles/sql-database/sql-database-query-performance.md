<properties 
   pageTitle="Azure SQL Database Query Performance Insight" 
   description="クエリのパフォーマンスを監視して、Azure SQL Database の CPU 消費量の多いクエリを明らかにします。" 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jeffreyg" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="12/02/2015"
   ms.author="sstein"/>

# Azure SQL Database Query Performance Insight


リレーショナル データベースのパフォーマンスの管理とチューニングは、かなりの専門知識と時間の投入を必要とする困難な作業です。 Query Performance Insight を使用すると、短時間でデータベース パフォーマンスのトラブルシューティングを行うことができます。以下の情報が提供されます。

- データベース リソース (DTU) の消費量の詳細な情報。 
- CPU 消費の多いクエリ。パフォーマンス向上のためのチューニングの対象となる可能性があります。 
- クエリの詳細にドリル ダウンできます。
​

## 前提条件

- Query Performance Insight は、Azure SQL Database V12 でのみ使用できます。
- クエリのパフォーマンスに関する考察である必要があります [クエリ ストア](https://msdn.microsoft.com/library/dn817826.aspx) 、データベースで実行されています。 まだ実行されていない場合は、ポータルからクエリ ストアをオンにするように求められます。

 
## アクセス許可

次 [ロールベースのアクセス制御](role-based-access-control-configure.md) Query Performance Insight を使用するのに必要なアクセス許可。 

- **リーダー**, 、**所有者**, 、**の共同作業者**, 、**SQL DB の共同作業者** または **SQL Server の共同作業者** 上位のリソースの使用のクエリ、およびグラフを表示する権限が必要です。 
- **所有者**, 、**の共同作業者**, 、**SQL DB の共同作業者** または **SQL Server の共同作業者** クエリ テキストを表示する権限が必要です。



## Query Performance Insight の使用

Query Performance Insight は簡単に使用できます。

- リソース消費量上位のクエリの一覧を確認します。 
- 個別のクエリを選択して詳細を表示します。
- クリックして **設定** データの表示方法をカスタマイズするか、別の期間を表示します。



> [AZURE.NOTE] いくつかの時間分のデータは、クエリのパフォーマンスに関する考察を提供する SQL データベースのクエリ ストアでキャプチャされる必要があります。 一定の期間、データベースでアクティビティが発生しない場合、またはクエリ ストアがアクティブではない場合は、その時間帯を表示してもグラフは空になります。 クエリ ストアが実行されていない場合はいつでも有効にできます。   



## CPU 消費量上位クエリを確認する

 [ポータル](https://portal.azure.com) 、次の操作します。

1. SQL データベースを参照してクリックして **Query Performance Insight**します。 

    ![Query Performance Insight][1]

    上位クエリ ビューが開き、CPU 消費量上位クエリの一覧が表示されます。

1. 詳細については、グラフの周囲をクリックします。<br>バーは、選択したクエリから呼び出される CPU % を示して、一番上の行は、データベースの全体的な DTU 割合を示します。 グラフに表示する個別のクエリを選別するには、選択または選択解除します。

    ![上位クエリ][2]

1. 必要に応じてクリックして **によるグラフの編集** CPU 消費量のデータの表示方法をカスタマイズするか、別の期間を表示します。

## 個々のクエリの詳細表示

クエリの詳細を表示するには:

1. 上位のクエリの一覧でクエリをクリックします。<br>詳細ビューが開き、クエリの CPU 消費量が時間ごとに分割されます。
3. 詳細については、グラフの周囲をクリックします。<br>一番上の行は、全体的な DTU 割合と、バーは選択したクエリによって使用される CPU % です。
4. クエリ実行の間隔ごとに、期間、実行回数、リソース使用率などの、詳細なメトリックが表示されます。
    
    ![クエリの詳細][3]

1. 必要に応じてクリックして **設定** CPU 消費量のデータの表示方法をカスタマイズするか、別の期間を表示します。




## 概要

Query Performance Insight を使うと、クエリ ワークロードの影響や、データベース リソース消費量との関係を理解できます。 この機能では、消費量上位クエリがわかり、問題になる前に簡単に識別して修正できます。 クリックして、 **Query Performance Insight** タイル上のリソース (CPU) コンシューマー クエリを表示するデータベースのブレードをします。 




## 次のステップ

データベースのワークロードは動的であり、継続的に変化します。 クエリを監視し、パフォーマンスの改善のために調整を続けます。 

チェック アウト、 [Index Advisor](sql-database-index-advisor.md) SQL データベースのパフォーマンスを向上させるための推奨事項の詳細。

<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png





