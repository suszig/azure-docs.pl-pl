<properties
    pageTitle="Azure SQL Database エラスティック データベース クエリの概要 | Microsoft Azure"
    description="エラスティック クエリ機能の概要"    
    services="sql-database"
    documentationCenter=""  
    manager="jeffreyg"
    authors="sidneyh"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/15/2015"
    ms.author="torsteng" />


# Azure SQL Database エラスティック データベース クエリの概要 (プレビュー)

エラスティック データベース クエリ機能 (プレビュー) を使用すると、Azure SQL Database (SQLDB) の複数のデータベースにまたがる Transact-SQL クエリを実行することができます。 リモート テーブルにアクセスし、Microsoft とサード パーティ製のツール (Excel、PowerBI、Tableau など) を接続するデータベースにまたがるクエリを実行することができます。 複数のデータベース層のデータに対してクエリします。 この機能により、クエリを SQL Database 内の大規模なデータ層にスケールアウトし、結果をビジネス インテリジェンス (BI) レポートで視覚化することができます。
エラスティック データベース クエリ アプリケーションを構築するには、次を参照してください。 [エラスティック データベース クエリの概要](sql-database-elastic-query-getting-started.md)します。

## エラスティック データベース クエリの新機能

* 1 つのリモート データベースを使ったデータベース間クエリ シナリオを T-SQL で完全に定義できるようになりました。 これにより、リモート データベースの読み取り専用クエリを実行できます。 これにより、現在のオンプレミス SQL Server のお客様が 3 部および 4 部構成名または SQL DB へのリンク サーバーを使用してアプリケーションを移行できるようになります。
* エラスティック クエリは、Premium パフォーマンス レベルだけでなく、Standard パフォーマンス レベルでもサポートされるようになりました。 下位のパフォーマンス レベルのパフォーマンスに関する制限については、以下の「プレビューの制限事項」セクションを参照してください。
* エラスティック クエリでは、SQL パラメーターを実行用にリモート データベースにプッシュ送信できるようになりました。
* リモート ストアド プロシージャ呼び出しまたは sp_execute_fanout を使用してリモート関数呼び出しのようなパラメーターを使用できます [sp_executesql](https://msdn.microsoft.com/library/ms188001.aspx)します。
* リモート データベースから大きな結果セットを取得する際のパフォーマンスが向上しました。
* エラスティック クエリを使用する外部テーブルで、スキーマまたはテーブル名が異なるリモート テーブルを参照できるようになりました。

## エラスティック データベース クエリのシナリオ

目的は、複数のデータベースが 1 つの全体的な結果に行を提供するクエリ シナリオを容易に実現することです。 クエリは、ユーザーまたはアプリケーションによって直接、またはデータベースに接続されているツールを使って間接的に構成できます。 これは特に、商用の BI またはデータ統合ツール (または、変更することができないアプリケーション) を使用してレポートを作成する場合に便利です。 エラスティック クエリを使用すると、Excel、PowerBI、Tableau、Cognos などのツールの使い慣れた SQL Server 接続機能を使用して、複数のデータベースにまたがるクエリを実行できます。
また、SQL Server Management Studio や Visual Studio によって発行されるクエリ経由でデータベースのコレクション全体に簡単にアクセスでき、Entity Framework やその他の ORM 環境から複数のデータベースにまたがるクエリを容易に実行できます。 図 1 は、既存のクラウド アプリケーション シナリオを示しています (が使用される、 [elastic database クライアント ライブラリ](sql-database-elastic-database-client-library.md) 層のスケール アウトされたデータに基づいて構築され、およびエラスティック クエリがデータベースにまたがるレポートに使用されます。

**図 1** スケールアウトされたデータ層で使用されるエラスティック データベース クエリ

![スケール アウトされたデータ層で使用されるエラスティック データベース クエリ][1]

エラスティック クエリの顧客シナリオは、次のトポロジによって特徴付けられます。

* **列方向のパーティション分割 – データベース間クエリ** (トポロジ 1): データは、データ層内の複数のデータベースの間で列方向にパーティション分割されます。 通常は、データベースごとに異なるテーブルのセットが存在します。 これは、異なるデータベースではスキーマが異なることを意味します。 たとえば、あるデータベースに在庫に関するすべてのテーブルが含まれていて、別のデータベースには会計に関連するすべてのテーブルが含まれているケースが該当します。 このトポロジを使用する一般的なユース ケースでは、複数のデータベースの複数のテーブルを対象にクエリを実行したりレポートを作成したりする必要があります。
* **行方向のパーティション分割 – シャーディング** (トポロジ 2): データは行方向にパーティション分割され、スケールアウトされたデータ層にわたって行が分散されます。 この方法では、参加しているすべてのデータベースでスキーマが同じになります。 この方法は、"シャーディング" とも呼ばれます。 シャーディングは、(1) エラスティック データベース ツール ライブラリまたは (2) 自己シャーディングを使って実行、管理できます。 エラスティック クエリは、複数のシャードを対象にクエリを実行したりレポートを作成するために使用します。

> [AZURE.NOTE] エラスティック データベース クエリは、大部分の処理をデータ層で実行できるレポート シナリオの場合に最適です。 負荷の高いレポート ワークロードまたはデータ ウェアハウスのより複雑なクエリを使用するシナリオの場合も使用を検討して [Azure SQL Data Warehouse](http://azure.microsoft.com/services/sql-data-warehouse/)します。


## エラスティック データベース クエリのトポロジ

### トポロジ 1: 列方向のパーティション分割 – データベース間クエリ

コーディングを開始するには、「 [データベースにまたがるクエリ (垂直的パーティション分割) の概要](sql-database-elastic-query-getting-started-vertical.md)します。

エラスティック クエリを使用すると、SQLDB データベースにあるデータを他の SQLDB データベースで利用できます。 これにより、あるデータベースに対するクエリで他のリモート SQLDB データベース内のテーブルを参照することができます。 最初の手順として、各リモート データベースの外部データ ソースを定義します。 外部データ ソースは、リモート データベース上にあるテーブルにアクセスするローカル データベースに定義します。 リモート データベースに変更を加える必要はありません。 スキーマがデータベースごとに異なる一般的な列方向のパーティション分割シナリオでは、エラスティック クエリを使用して、参照データへのアクセスや、データベース間クエリなどの一般的なユース ケースを実装できます。

**参照データ**: 参照データの管理のためにトポロジ 1 が使用されます。 次の図では、参照データを含む 2 つのテーブル (T1 と T2) は、専用のデータベースに保持されています。 エラスティック クエリを使用すると、図に示すように、他のデータベースからリモートでテーブル T1 と T2 にアクセスできるようになります。 参照テーブルのサイズが小さい場合や参照テーブルへのリモート クエリに選択的述語が含まれる場合は、トポロジ 1 を使用します。

**図 2** 列方向のパーティション分割 - エラスティック クエリを使用して参照データを照会する

![Vertical partitioning - Using elastic query to query reference data][3]

**データベース間クエリ**: エラスティック クエリを使用すると、複数の SQLDB データベースにまたがるクエリを必要とするユース ケースに対応できます。 図 3 には、CRM、Inventory、HR、Products の 4 つの異なるデータベースが示されています。 これらのデータベースのいずれかで実行されるクエリでは、他のデータベースにもアクセスする必要があります。 エラスティック クエリを使用すると、4 つのデータベースごとにいくつかの単純な DDL ステートメントを実行することで、このケースに対応するデータベースを構成できます。 この 1 回限りの構成を行った後は、T-SQL クエリまたは BI ツールからローカル テーブルを参照するだけでリモート テーブルにアクセスできます。 この方法は、リモート クエリからサイズの大きな結果が返されない場合にお勧めします。

**図 3** 列方向のパーティション分割 - エラスティック クエリを使用して複数のデータベースにまたがって照会する

![Vertical partitioning - Using elastic query to query across various databases][4]

### トポロジ 2: 行方向のパーティション分割 - シャーディング

コーディングを開始するには、を参照してください [水平方向のパーティション分割 (シャーディング) エラスティック データベース クエリの概要](sql-database-elastic-query-getting-started.md)。

データ層のエラスティック クエリを使用して、つまり、水平方向にパーティション分割されて、シャード上のレポート タスクを実行する必要があります、 [elastic database シャード マップ](sql-database-elastic-scale-shard-map-management.md) データ層のデータベースを示すためです。 一般に、このシナリオではシャード マップが 1 つだけ使用され、エラスティック クエリ機能を備えた専用のデータベースがレポート クエリのエントリ ポイントとして機能します。 シャード マップにアクセスする必要があるのは、この専用のデータベースのみです。 図 2 に、このトポロジと、エラスティック クエリ データベースおよびシャード マップを使用した構成を示します。 エラスティック クエリ データベースのみが Azure SQL Database v12 データベースである必要があることに注意してください。 データ層では、Azure SQL Database の任意のバージョンまたはエディションのデータベースを使用できます。 エラスティック データベース クライアント ライブラリおよびシャード マップの作成の詳細については、次を参照してください。 [シャード マップの管理](sql-database-elastic-scale-shard-map-management.md)します。

**図 4** 行方向のパーティション分割: シャーディングされたデータ層に対してエラスティック クエリを使用する

![Horizontal partitioning - Using elastic query for reporting over sharded data tiers][5]
> [AZURE.NOTE] 専用のエラスティック データベース クエリ データベースは SQL DB v12 データベースである必要があります。 シャード自体に対する制限はありません。


## エラスティック データベース クエリの実装

以降では、列および行方向のパーティション分割シナリオ用にエラスティック クエリを実装する手順について説明します。 さらに、さまざまなパーティション分割シナリオに対するより詳細なドキュメントも紹介しています。

### 列方向のパーティション分割 – データベース間クエリ

次の手順では、リモート SQLDB データベース上にあるテーブルへのアクセスを必要とする列方向のパーティション分割シナリオ向けに、エラスティック データベース クエリを構成します。

*    [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey 

*    [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential

*    [CREATE/DROP EXTERNAL DATA SOURCE](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource of type **RDBMS**

*    [CREATE/DROP EXTERNAL TABLE](https://msdn.microsoft.com/library/dn935021.aspx) mytable


DDL ステートメントを実行すると、ローカル テーブルであるかのようにリモート テーブル "mytable" にアクセスできます。 Azure SQL Database により、リモート データベースへの接続が自動的に開かれてリモート データベースで要求が処理され、その結果が返されます。
垂直方向のパーティション分割のシナリオを参照して必要な手順の詳細について [垂直的パーティション分割のエラスティック クエリ](sql-database-elastic-query-vertical-partitioning.md)します。

### 行方向のパーティション分割 - シャーディング

次の手順では、(通常は) いくつかのリモート SQLDB データベース上にある一連のテーブルへのアクセスを必要とする行方向のパーティション分割シナリオ向けに、エラスティック データベース クエリを構成します。

*    [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey

*    [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential 

*    Create a [shard map](sql-database-elastic-scale-shard-map-management.md) representing your data tier using the elastic database client library.   

*    [CREATE/DROP EXTERNAL DATA SOURCE](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource of type **SHARD_MAP_MANAGER**

*    [CREATE/DROP EXTERNAL TABLE](https://msdn.microsoft.com/library/dn935021.aspx) mytable


これらの手順を実行すると、ローカル テーブルであるかのように、行方向にパーティション分割されたテーブル "mytable" にアクセスできます。 Azure SQL Database により、テーブルが物理的に格納されているリモート データベースへの複数の並列接続が自動的に開かれます。さらに、リモート データベースで要求が処理され、その結果が返されます。
水平方向のパーティション分割のシナリオを参照して必要な手順の詳細について [水平的パーティション分割のエラスティック クエリ](sql-database-elastic-query-horizontal-partitioning.md)します。

## T-SQL クエリの実行

外部データ ソースと外部テーブルを定義すると、通常の SQL Server 接続文字列を使用して、外部テーブルが定義されているデータベースに接続できます。 次に、その接続で外部テーブルに対して T-SQL ステートメントを実行できます。この場合、次に示す制限事項があります。 ドキュメントのトピックで詳細の情報と T-SQL クエリの例を確認できます [水平的パーティション分割](sql-database-elastic-query-horizontal-partitioning.md) と [垂直的パーティション分割](sql-database-elastic-query-vertical-partitioning.md)します。

## ツールの接続性

通常の SQL Server 接続文字列を使用して、アプリケーション、BI、またはデータ統合ツールを、外部テーブルを持つデータベースに接続できます。 ご使用のツールのデータ ソースとして SQL Server がサポートされていることを確認してください。 接続されたら、ツールを使用して接続する他の SQL Server データベースと同様に、エラスティック クエリ データベースと外部テーブルを参照します。

## コスト

エラスティック クエリは、Azure SQL Database データベースのコストに含まれます。 注リモート データベースは弾力性のクエリのエンドポイントよりも、異なるデータ センターにあるトポロジがサポートされますが、リモート データベースからのデータ送信は標準料金が発生する [Azure レート](https://azure.microsoft.com/pricing/details/data-transfers/)します。

## プレビューの制限事項

* Standard パフォーマンス レベルでは、初回のエラスティック クエリの実行に数分かかる場合があります。 これは、エラスティック クエリ機能の読み込みに要する時間です。パフォーマンス レベルが上位になるほど、読み込みのパフォーマンスが高くなります。
* SSMS または SSDT の外部データ ソースまたは外部テーブルからのスクリプト処理はまだサポートされていません。
* SQL DB の Import/Export では、外部データ ソースと外部テーブルはまだサポートされていません。 Import/Export を使用する必要がある場合は、エクスポートの前にこれらのオブジェクトを削除し、インポート後にこれらのオブジェクトを再作成します。
* 現在、エラスティック データベース クエリでは、外部テーブルへの読み取り専用アクセスだけがサポートされています。ただし、外部テーブルが定義されているデータベースでは、完全な T-SQL 機能を使用できます。例: など SELECT を使用して一時的な結果を保持するには役立ちます。 <column_list> INTO <local_table>, 、または外部テーブルを参照するエラスティック クエリ データベースでストアド プロシージャを定義します。
* 外部テーブル定義では、nvarchar(max) 以外の LOB 型はサポートされていません。 この制限を回避するために、リモート データベースで LOB 型を nvarchar(max) にキャストするビューを作成し、ベース テーブルではなくそのビューで外部テーブルを定義し、クエリを使ってこれを元の LOB 型にキャストするという方法を利用できます。
* 外部テーブルに対する列の統計情報は、現在サポートされていません。 テーブルの統計情報はサポートされていますが、手動で作成する必要があります。

## フィードバック

以下に示す Disqus、MSDN フォーラム、または Stackoverflow で、エラスティック クエリに関するノウハウを共有してください。 サービスに関して何でもご意見とご感想をお寄せください (障害、機能差、悪口など)。

## 詳細情報

データベース間クエリと列方向のパーティション分割シナリオの詳細については、次のドキュメントを参照してください。

* [データベースにまたがるクエリを実行して垂直方向のパーティション分割の概要](sql-database-elastic-query-vertical-partitioning.md)
* 完全に、ステップ バイ ステップ チュートリアルを実行してください (分) を実行している実際の例: [データベースにまたがるクエリ (垂直的パーティション分割) の概要](sql-database-elastic-query-getting-started-vertical.md)します。


行方向のパーティション分割とシャーディングのシナリオの詳細については、次のドキュメントを参照してください。

* [水平方向のパーティション分割とシャーディングの概要](sql-database-elastic-query-horizontal-partitioning.md)
* 完全に、ステップ バイ ステップ チュートリアルを実行してください (分) を実行している実際の例: [水平方向のパーティション分割 (シャーディング) エラスティック データベース クエリの概要](sql-database-elastic-query-getting-started.md)します。


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]







[1]: ./media/sql-database-elastic-query-overview/overview.png 
[2]: ./media/sql-database-elastic-query-overview/topology1.png 
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png 
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png 
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png 

