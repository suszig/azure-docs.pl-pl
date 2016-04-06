<properties 
    pageTitle="Azure ポータルを使用して Azure SQL Database の geo レプリケーションを構成する | Microsoft Azure" 
    description="Azure ポータルを使用して Azure SQL Database の geo レプリケーションを構成する" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jeffreyg" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="12/01/2015"
    ms.author="sstein"/>

# Azure ポータルを使用して Azure SQL Database の geo レプリケーションを構成する


> [AZURE.SELECTOR]
- [Azure ポータル](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [Transact-SQL](sql-database-geo-replication-transact-sql.md)


この記事には、SQL データベースの geo レプリケーションを構成する方法がでは、 [Azure ポータル](https://portal.azure.com)します。

geo レプリケーションでは、さまざまなデータ センターの場所 (リージョン) に最大 4 つのレプリカ (セカンダリ) データベースを作成することができます。 セカンダリ データベースは、データ センターで障害が発生した場合やプライマリ データベースに接続できない場合に使用できます。

geo レプリケーションは、Standard データベースと Premium データベースにのみ使用できます。 

Standard データベースでは、読み取り不可のセカンダリ データベースを 1 つ置くことができますが、その場合は推奨されたリージョンを使用する必要があります。 Premium データベースでは、利用可能な任意のリージョンに、最大 4 つの読み取り可能なセカンダリ データベースを置くことができます。


geo レプリケーションを構成するには、次のものが必要です。

- Azure サブスクリプション。 をクリックする必要がある場合、Azure サブスクリプションだけで **無料評価版** これの上部にあるページをこの記事を完了する通します。
- Azure SQL Database データベース。別の地理的リージョンにレプリケートするプライマリ データベースです。



## セカンダリ データベースの追加

次の手順では、geo レプリケーション パートナーシップに新しいセカンダリ データベースを作成します。  

セカンダリ データベースを追加するには、サブスクリプションの所有者または共同所有者でなければなりません。 

セカンダリ データベースは、プライマリ データベースと同じ名前となります。また、既定でのサービス レベルはプライマリ データベースと同じになります。 セカンダリ データベースは、読み取り可能 (Premium レベルのみ) または読み取り不可とすることができるほか、単一データベースまたはエラスティック データベースとすることができます。 詳細については、次を参照してください。 [サービス階層](sql-database-service-tiers.md)します。
セカンダリ データベースを作成しシード処理を行うと、プライマリ データベースから新しいセカンダリ データベースへのデータのレプリケートが開始されます。 

> [AZURE.NOTE] (たとえば、結果として以前の geo レプリケーション リレーションシップを終了するには)、パートナーのデータベースが既に存在する場合は、コマンドは失敗します。




### セカンダリ データベースの追加

1.  [Azure ポータル](https://portal.azure.com) geo レプリケーションをセットアップ データベースを参照します。
2. [SQL Database ブレードで、次のように選択します。 **設定をすべて** > **Geo レプリケーション**します。
3. セカンダリ データベースを作成するリージョンを選択します。 セカンダリ データベース用のリージョンとして、Premium データベースでは任意のリージョンを使用できますが、Standard データベースでは推奨リージョンを使用する必要があります。


    ![Add secondary][1]


4. 構成、 **セカンダリ タイプ** (**Readable**, 、または **non-readable**)、のみ Premium データベースが読み取り可能なセカンダリを持つ、標準的なデータベース セカンダリにしか設定できません **non-readable**します。
5. セカンダリ データベース用のサーバーを選択または構成します。

    ![セカンダリ データベースの作成][3]

5. 必要に応じて、以下のようにセカンダリ データベースをエラスティック データベース プールに追加できます。

       - Click **Elastic database pool** and select a pool on the target server to create the secondary database in. A pool must already exist on the target server as this workflow does not create a new pool.

6. クリックして **作成** セカンダリを追加します。
 
6. セカンダリ データベースが作成され、シード処理が始まります。 
 
    ![シード処理][6]

7. シード処理が完了すると、セカンダリ データベースは自身の状態 (読み取り不可) を表示します。

    ![セカンダリ データベース準備完了][9]



## セカンダリ データベースを削除する

この操作では、セカンダリ データベースへのレプリケーションを完全に終了し、セカンダリ データベースのロールを通常の読み取り/書き込みデータベースに変更します。 セカンダリ データベースへの接続が切断された場合、コマンドは成功しますが、接続が復元するまでセカンダリ データベースは読み取り/書き込み状態になりません。  

1.  [Azure ポータル](https://portal.azure.com) のプライマリ データベースで geo レプリケーション パートナーシップを参照します。
2. [SQL Database ブレードで、次のように選択します。 **設定をすべて** > **Geo レプリケーション**します。
3.  **セカンダリ** ボックスの一覧、geo レプリケーション パートナーシップからを削除するデータベースを選択します。
4. クリックして **レプリケーションを停止する**です。

    ![セカンダリ データベースの削除][7]


5. クリックすると **レプリケーションの停止** が開きますので、確認ウィンドウをクリックして **はい** geo レプリケーション パートナーシップからデータベースを削除する (設定を読み取り/書き込みデータベースのレプリケーションの一部ではない)。


    ![confirm removal][8]



## フェールオーバーの開始

セカンダリ データベースは、プライマリ データベースとして使用するように切り替えることができます。  

1.  [Azure ポータル](https://portal.azure.com) のプライマリ データベースで geo レプリケーション パートナーシップを参照します。
2. [SQL Database ブレードで、次のように選択します。 **設定をすべて** > **Geo レプリケーション**します。
3.  **セカンダリ** するのには、新しいプライマリ データベースを選択します。
4. クリックして **フェールオーバー**します。

    ![フェールオーバー][10]

コマンドによって、次のワークフローが実行されます。 

1. 一時的にレプリケーションを同期モードに切り替えます。 これにより、すべての未完了のトランザクションがセカンダリ データベースにフラッシュされます。 

2. geo レプリケーション パートナーシップで 2 つのデータベースのプライマリ ロールとセカンダリ ロールを切り替えます。  

計画されたフェールオーバーの場合、このシーケンスによってデータが失われることはありません。 ロールの切り替え中に、わずかですが両方のデータベースが使用できなくなる期間 (0 ～ 25 秒程度) が生じます。 通常の状況では、操作全体が完了するのに 1 分かかりません。 

   

## 次のステップ

- [geo レプリケーションを使用したビジネス継続性を実現するクラウド アプリケーションの設計](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [障害復旧訓練](sql-database-disaster-recovery-drills.md)


## その他のリソース

- [新しい geo レプリケーション機能に関するスポットライト](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication)
- [geo レプリケーションを使用したビジネス継続性を実現するクラウド アプリケーションの設計](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [ビジネス継続性の概要](sql-database-business-continuity.md)
- [SQL Database のドキュメント](https://azure.microsoft.com/documentation/services/sql-database/)


<!--Image references-->
[1]: ./media/sql-database-geo-replication-portal/configure-geo-replication.png
[2]: ./media/sql-database-geo-replication-portal/add-secondary.png
[3]: ./media/sql-database-geo-replication-portal/create-secondary.png
[4]: ./media/sql-database-geo-replication-portal/secondary-type.png
[5]: ./media/sql-database-geo-replication-portal/create.png
[6]: ./media/sql-database-geo-replication-portal/seeding0.png
[7]: ./media/sql-database-geo-replication-portal/remove-secondary.png
[8]: ./media/sql-database-geo-replication-portal/stop-confirm.png
[9]: ./media/sql-database-geo-replication-portal/seeding-complete.png
[10]: ./media/sql-database-geo-replication-portal/failover.png



