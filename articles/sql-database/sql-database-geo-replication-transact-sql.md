<properties
    pageTitle="Transact-SQL を使用して Azure SQL Database の geo レプリケーションを構成する | Microsoft Azure"
    description="Transact-SQL を使用した Azure SQL Database の geo レプリケーションの構成"
    services="sql-database"
    documentationCenter=""
    authors="carlrabeler"
    manager="jeffreyg"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management"
    ms.date="11/10/2015"
    ms.author="carlrab"/>

# Transact-SQL を使用して Azure SQL Database の geo レプリケーションを構成する



> [AZURE.SELECTOR]
- [Azure ポータル](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [Transact-SQL](sql-database-geo-replication-transact-sql.md)


この記事では、Transact-SQL を使用して Azure SQL Database の geo レプリケーションを構成する方法について説明します。


geo レプリケーションでは、さまざまなデータ センターの場所 (リージョン) に最大 4 つのレプリカ (セカンダリ) データベースを作成することができます。 セカンダリ データベースは、データ センターで障害が発生した場合やプライマリ データベースに接続できない場合に使用できます。

geo レプリケーションは、Standard データベースと Premium データベースにのみ使用できます。 

Standard データベースでは、読み取り不可のセカンダリ データベースを 1 つ置くことができますが、その場合は推奨されたリージョンを使用する必要があります。 Premium データベースでは、利用可能な任意のリージョンに、最大 4 つの読み取り可能なセカンダリ データベースを置くことができます。


geo レプリケーションを構成するには、次のものが必要です。

- -クリックするだけで Azure サブスクリプションがない場合は、Azure のサブスクリプション **無料評価版** これの上部にあるページをこの記事を完了する通します。
- Azure SQL Database 論理サーバー <MyLocalServer> SQL データベース <MyDB> 別の地理的リージョンにレプリケートするプライマリ データベース-にします。
- 複数の Azure SQL Database 論理サーバー <MySecondaryServer(n)> - geo レプリケーション セカンダリ データベースの作成先であるパートナー サーバーとなる論理サーバーです。
- プライマリ上の DBManager であるログイン。geo レプリケートするローカル データベースの db_ownership を所有し、geo レプリケーションを構成するパートナー サーバー上の DBManager になります。
- SQL Server Management Studio (SSMS) の最新版を入手する - SQL Server Management Studio の最新バージョンは、[ダウンロード SQL Server Management Studio] (https://msdn.microsoft.com/library/mt238290.aspx) に移動します。 詳細については、Azure SQL データベースを管理する SQL Server Management Studio を使用して論理サーバーとデータベースを参照してください [Azure SQL Database の管理 SQL Server Management Studio を使用します。](sql-database-manage-azure-ssms.md)

## セカンダリ データベースの追加

使用することができます、 **ALTER DATABASE** ステートメント パートナー サーバーに geo レプリケートのセカンダリ データベースを作成します。 このステートメントは、レプリケートされるデータベースが含まれているサーバーの master データベースに対して実行します。 geo レプリケートされたデータベース ("プライマリ データベース") は、レプリケートされているデータベースと同じ名前になります。また、既定では、サービス レベルがプライマリ データベースと同じになります。 セカンダリ データベースは、読み取り可能または読み取り不可とすることができるほか、単一データベースまたはエラスティック データベースとすることができます。 詳細については、次を参照してください。 [ALTER DATABASE (TRANSACT-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) と [サービス階層](https://azure.microsoft.com/documentation/articles/sql-database-service-tiers/)します。
セカンダリ データベースが作成され、シード処理が行われると、データはプライマリ データベースから非同期にレプリケートを開始します。 以下の手順では、Management Studio を使用して geo レプリケーションを構成する方法について説明します。 単一データベースまたはエラスティック データベースとして、読み取り不可のセカンダリと読み取り可能なセカンダリを作成する手順について説明します。

> [AZURE.NOTE] (たとえば、ため geo レプリケーション関係が現在存在するか、既に存在する場合、コマンドは失敗します指定したパートナー サーバーでセカンダリ データベースが存在する場合。


### 読み取り不可のセカンダリ (単一データベース) の追加

読み取り不可のセカンダリを単一データベースとして作成するには、次の手順に従います。

1. バージョン 13.0.600.65 以降の SQL Server Management Studio を使用します。

     > [AZURE.IMPORTANT] ダウンロード、 [最新](https://msdn.microsoft.com/library/mt238290.aspx) SQL Server Management Studio のバージョン。 常に最新バージョンの Management Studio を使用して、Azure ポータルの更新プログラムとの同期を維持することをお勧めします。


2. データベース フォルダーを開き、展開、 **システム データベース** フォルダーを右クリックし **マスター**, 、クリックして **新しいクエリ**します。

3. 次を使用して **ALTER DATABASE** geo レプリケーションをローカル データベースを読み取り不可のセカンダリ データベース上でプライマリにするのにはステートメント <MySecondaryServer1>.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer1> WITH (ALLOW_CONNECTIONS = NO);

4. クリックして **Execute** クエリを実行します。


### 読み取り可能なセカンダリ (単一データベース) の追加
読み取り可能なセカンダリを単一データベースとして作成するには、次の手順に従います。

1. Management Studio で Azure SQL Database 論理サーバーに接続します。

2. データベース フォルダーを開き、展開、 **システム データベース** フォルダーを右クリックし **マスター**, 、クリックして **新しいクエリ**します。

3. 次を使用して **ALTER DATABASE** geo レプリケーションをローカル データベースをセカンダリ サーバー上の読み取り可能なセカンダリ データベースをプライマリにするのにはステートメントです。

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);

4. クリックして **Execute** クエリを実行します。



### 読み取り不可のセカンダリ (エラスティック データベース) の追加
読み取り不可のセカンダリをエラスティック データベースとして作成するには、次の手順に従います。

1. Management Studio で Azure SQL Database 論理サーバーに接続します。

2. データベース フォルダーを開き、展開、 **システム データベース** フォルダーを右クリックし **マスター**, 、クリックして **新しいクエリ**します。

3. 次を使用して **ALTER DATABASE** geo レプリケーションをローカル データベースをエラスティック プール内のセカンダリ サーバー上の読み取り不可のセカンダリ データベースをプライマリにするのにはステートメントです。

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer3> WITH (ALLOW_CONNECTIONS = NO)
           , ELASTIC_POOL (name = MyElasticPool1);

4. クリックして **Execute** クエリを実行します。



### 読み取り可能なセカンダリ (エラスティック データベース) の追加
読み取り可能なセカンダリをエラスティック データベースとして作成するには、次の手順に従います。

1. Management Studio で Azure SQL Database 論理サーバーに接続します。

2. データベース フォルダーを開き、展開、 **システム データベース** フォルダーを右クリックし **マスター**, 、クリックして **新しいクエリ**します。

3. 次を使用して **ALTER DATABASE** geo レプリケーションをローカル データベースをエラスティック プール内のセカンダリ サーバー上の読み取り可能なセカンダリ データベースをプライマリにするのにはステートメントです。

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = NO)
           , ELASTIC_POOL (name = MyElasticPool2);

4. クリックして **Execute** クエリを実行します。



## セカンダリ データベースを削除する

使用することができます、 **ALTER DATABASE** ステートメントを完全にセカンダリ データベースとそのプライマリ間のレプリケーション パートナーシップを終了します。 このステートメントは、プライマリ データベースが存在する master データベースに対して実行されます。 リレーションシップの終了後、セカンダリ データベースは通常の読み取り/書き込みデータベースになります。 セカンダリ データベースへの接続が切断された場合、コマンドは成功します。ただし、接続が復元されると、セカンダリ データベースは読み取り/書き込みデータベースになります。 詳細については、次を参照してください。 [ALTER DATABASE (TRANSACT-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) と [サービス階層](https://azure.microsoft.com/documentation/articles/sql-database-service-tiers/)します。

geo レプリケートされたセカンダリを geo レプリケーション パートナーシップから削除するには、次の手順に従います。

1. Management Studio で Azure SQL Database 論理サーバーに接続します。

2. データベース フォルダーを開き、展開、 **システム データベース** フォルダーを右クリックし **マスター**, 、クリックして **新しいクエリ**します。

3. 次を使用して **ALTER DATABASE** geo レプリケートのセカンダリを削除するステートメントです。

        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;

4. クリックして **Execute** クエリを実行します。


## セカンダリ データベースを昇格させて新しいプライマリにする、計画されたフェールオーバーを開始する

使用することができます、 **ALTER DATABASE** 計画的に、セカンダリが既存のプライマリに降格する場合は、新しいプライマリ データベースがセカンダリ データベースを昇格するステートメントです。 このステートメントは、昇格の対象となる geo レプリケートされたセカンダリ データベースが存在する Azure SQL Database 論理サーバー上の master データベースに対して実行されます。 この機能は、DR ドリル時など、計画されたフェールオーバー用に設計されており、プライマリ データベースが使用できることが必要条件となります。

コマンドによって、次のワークフローが実行されます。

1. 一時的にレプリケーションを同期モードに切り替えます。未処理のトランザクションはすべてセカンダリにフラッシュされ、新しいトランザクションはすべてブロックされます。

2. geo レプリケーション パートナーシップにおける 2 つのデータベースのロールを切り替えます。  

このシーケンスによってデータが失われることはありません。 ロールの切り替え中に、わずかですが両方のデータベースが使用できなくなる期間 (0 ～ 25 秒程度) が生じます。 通常の状況では、操作全体が完了するのに 1 分かかりません。 詳細については、次を参照してください。 [ALTER DATABASE (TRANSACT-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) と [サービス階層](https://azure.microsoft.com/documentation/articles/sql-database-service-tiers/)します。


> [AZURE.NOTE] コマンドが発行されたときに、プライマリ データベースが使用できない場合、プライマリ サーバーが使用できないことを示すエラー メッセージとコマンドは失敗します。 まれに、操作が完了しないで、スタックしたような状態になる場合があります。 この場合、ユーザーは強制フェールオーバー コマンドを実行できますが、データが失われることを容認する必要があります。

計画されたフェールオーバーを開始するには、次の手順に従います。

1. Management Studio で、geo レプリケートされたセカンダリ データベースが存在する Azure SQL Database 論理サーバーに接続します。

2. データベース フォルダーを開き、展開、 **システム データベース** フォルダーを右クリックし **マスター**, 、クリックして **新しいクエリ**します。

3. 次を使用して **ALTER DATABASE** 主に読み取り可能なセカンダリ データベースで geo レプリケーションに geo レプリケートされたデータベースを作成するステートメント <MySecondaryServer4> という名前で、 <ElasticPool2>.

        ALTER DATABASE <MyDB> FAILOVER;

4. クリックして **Execute** クエリを実行します。



## プライマリ データベースからセカンダリ データベースへの計画されていないフェールオーバーを開始します。

使用することができます、 **ALTER DATABASE** を強制的にセカンダリをプライマリ データベースが不要になった利用可能な時に既存のプライマリの降格、計画されていない方法で新しいプライマリ データベースがセカンダリ データベースを昇格させるステートメントです。 このステートメントは、昇格の対象となる geo レプリケートされたセカンダリ データベースが存在する Azure SQL Database 論理サーバー上の master データベースに対して実行されます。

この機能は、データベースの可用性を復元することが重要で、多少のデータ損失は許容されるような障害復旧を目的として設計されています。 強制フェールオーバーが呼び出されると、指定したセカンダリ データベースはすぐにプライマリ データベースになり、書き込みトランザクションの受け入れを開始します。 元のプライマリ データベースがこの新しいプライマリ データベースと再接続できるようになるとすぐに、元のプライマリ データベース上で増分バックアップが行われます。古いプライマリ データベースは新しいプライマリ データベースのセカンダリ データベースとなり、それ以降、新しいプライマリの単なる同期用レプリカになります。

ただし、セカンダリ データベースではポイントインタイム リストアがサポートされていません。そのため、古いプライマリ データベースにコミットされたデータのうち、強制フェールオーバーが発生する前に新しいプライマリ データベースにレプリケートされなかったデータを復元しようとする場合、ユーザーは、この失われたデータを復元するようサポートに問い合わせる必要があります。

> [AZURE.NOTE] 場合は、コマンドを発行して、プライマリとセカンダリの両方がオンラインになって、古いプライマリは、新しいセカンダリになりますが、データの同期は行われません。 そのため、一部のデータは失われる可能性があります。


プライマリ データベースに複数のセカンダリ データベースがある場合、コマンドは、そのコマンドが実行されたセカンダリ サーバーでのみ成功します。 ただし、その他のセカンダリ データベースは、強制フェールオーバーが発生したことを認識しません。 ユーザーは、"セカンダリを削除する" API を使用してこの構成を手動で修復した後、これら残りのセカンダリ データベースで geo レプリケーションを再構成する必要があります。

geo レプリケートされたセカンダリを geo レプリケーション パートナーシップから強制的に削除するには、次の手順に従います。

1. Management Studio で、geo レプリケートされたセカンダリ データベースが存在する Azure SQL Database 論理サーバーに接続します。

2. データベース フォルダーを開き、展開、 **システム データベース** フォルダーを右クリックし **マスター**, 、クリックして **新しいクエリ**します。

3. 次を使用して **ALTER DATABASE** ステートメントを <MyLocalDB> 読み取り可能なセカンダリ データベースで geo レプリケーションのプライマリに <MySecondaryServer4> という名前で、 <ElasticPool2>.

        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;

4. クリックして **Execute** クエリを実行します。

## geo レプリケーションの構成と正常性を監視する

監視タスクには、geo レプリケーションの構成に関する監視と、データ レプリケーションの正常性に関する監視が含まれます。  使用することができます、 **sys.dm_geo_replication_links** 動的管理ビューで、master データベースの Azure SQL Database 論理サーバーの各データベースに対するすべての既存のレプリケーション リンクに関する情報を返します。 このビューでは、プライマリ データベースとセカンダリ データベースの間の各レプリケーション リンクについて 1 行表示されます。 使用することができます、 **sys.dm_replication_status** 動的管理ビューを現在進行中のレプリケーションのレプリケーション リンクでの Azure SQL データベースごとに行を返します。 これには、プライマリ データベースとセカンダリ データベースの両方が含まれます。 特定のプライマリ データベースについて複数の連続レプリケーション リンクが存在する場合、このテーブルには、各リレーションシップについて 1 行が含まれます。 このビューは、すべてのデータベース (論理 master データベースを含む) で作成されます。 ただし、論理 master データベースでこのビューにクエリを実行しても、空のセットが返されます。 使用することができます、 **sys.dm_operation_status** 動的管理ビューをすべてのデータベース レプリケーション リンクのステータスなどの操作のステータスが表示されます。 詳細については、次を参照してください。 [sys.geo_replication_links (Azure SQL データベース)](https://msdn.microsoft.com/library/mt575501.aspx), 、[sys.dm_geo_replication_link_status (Azure SQL データベース)](https://msdn.microsoft.com/library/mt575504.aspx), 、および [sys.dm_operation_status (Azure SQL データベース)](https://msdn.microsoft.com/library/dn270022.aspx)します。

geo レプリケーション パートナーシップを監視するには、次の手順に従います。

1. Management Studio で Azure SQL Database 論理サーバーに接続します。

2. データベース フォルダーを開き、展開、 **システム データベース** フォルダーを右クリックし **マスター**, 、クリックして **新しいクエリ**します。

3. 次のステートメントを使用して、geo レプリケーション リンクと共にすべてのデータベースを表示します。

        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM [sys].geo_replication_links;

4. クリックして **Execute** クエリを実行します。
5. データベース フォルダーを開き、展開、 **システム データベース** フォルダーを右クリックし **MyDB**, 、クリックして **新しいクエリ**します。
6. 次のステートメントを使用して、MyDB のセカンダリ データベースのレプリケーション ラグと前回のレプリケーション時刻を表示します。

        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status

7. クリックして **Execute** クエリを実行します。
8. 次のステートメントを使用して、MyDB データベースに関連付けられた最近の geo レプリケーション操作を表示します。

        SELECT * FROM sys.dm_operation_status where major_resource_is = 'MyDB'
        ORDER BY start_time DESC

9. クリックして **Execute** クエリを実行します。


## 次のステップ

- [障害復旧訓練](sql-database-disaster-recovery-drills.md)


## その他のリソース

- [新しい geo レプリケーション機能に関するスポットライト](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication)
- [geo レプリケーションを使用したビジネス継続性を実現するクラウド アプリケーションの設計](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [ビジネス継続性の概要](sql-database-business-continuity.md)
- [SQL Database のドキュメント](https://azure.microsoft.com/documentation/services/sql-database/)


