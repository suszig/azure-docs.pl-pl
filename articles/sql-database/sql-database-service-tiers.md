<properties
    pageTitle="SQL Database のパフォーマンスとオプション: サービス階層 | Microsoft Azure"
    description="サービス階層の SQL Database のパフォーマンスとビジネス継続性機能を比較して、実際の規模に合わせてコストと互換性のバランスを取ります。"
    keywords="database options,database performance,eDTU"
    services="sql-database"
    documentationCenter=""
    authors="rothja"
    manager="jeffreyg"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="11/10/2015"
    ms.author="jroth"/>

# SQL Database のオプションとパフォーマンス: 各サービス階層で使用できる内容について理解します
 

[Azure SQL Database](sql-database-technical-overview.md) さまざまな種類のワークロードを処理する複数のサービス層を提供します。 実行できます [1 つのデータベースを作成する](sql-database-get-started.md) 特性と価格を定義します。 によって複数のデータベースを管理する、または [弾力性データベース プールを作成する](sql-database-elastic-pool-portal.md)です。 どちらの場合も、層が含まれて **基本的な**, 、**標準**, 、および **Premium**します。 ただし、これらの階層のデータベース オプションは、個別のデータベースを作成するのか、それともエラスティック データベース プール内にデータベースを作成するのかで、異なります。 この記事では、両方のケースでのサービス階層について概要を示します。

## サービス階層とデータベース オプション
Basic、Standard、および Premium のサービス階層は、いずれも稼働時間の SLA が 99.99% で、予測可能なパフォーマンス、柔軟なビジネス継続性のオプション、セキュリティ機能、1 時間単位の課金体系などが提供されます。 次の表では、各種のアプリケーション ワークロードに最適なサービス階層の例を示します。

| サービス階層 | 対象のワークロード |
|---|---|
| **基本** | 小規模なデータベースに最適です。一般的に、同時に 1 つのアクティブな操作のみをサポートします。 たとえば、開発、テスト、小規模なあまり使用されないアプリケーションに使用するデータベースなどです。 |
| **Standard** | ほとんどのクラウド アプリケーションに適しています。同時に複数のクエリをサポートしています。 たとえば、ワークグループや Web アプリケーションなどです。 |
| **Premium** | トランザクション量が多く、多数の同時ユーザーをサポートし、高度なビジネス継続性機能を必要とする場合に合わせて設計されています。 たとえば、ミッション クリティカルなアプリケーションをサポートするデータベースなどです。 |

>[AZURE.NOTE] Web および Business エディションは廃止されています。 方法を見つけて [Web および Business エディションのアップグレード](sql-database-upgrade-new-service-tiers.md)します。 お読みください、 [の終了に関する FAQ](http://azure.microsoft.com/pricing/details/sql-database/web-business/) Web と Business Edition の使用を続行する場合。

### Single Database サービス階層とパフォーマンス レベル
単一のデータベースの場合、各サービス階層内に複数のパフォーマンス レベルがあるため、ワークロードのニーズに最も合ったレベルを柔軟に選択できます。 スケールアップまたはスケールダウンする必要がある場合は、Azure クラシック ポータルでデータベースの階層を簡単に変更することができます。このときアプリケーションのダウンタイムはありません。 参照してください [を変更するデータベースのサービス階層とパフォーマンス レベル](sql-database-scale-up.md) 詳細です。

使用して作成されたデータベースには、ここに示すパフォーマンス特性を適用 [SQL Database V12](sql-database-v12-whats-new.md)します。 Azure 内の基になるハードウェアに複数の SQL データベースをホストする場合も、データベースは、所定のリソースを取得します。個々のデータベースの期待されるパフォーマンス特性には影響しません。

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]


について理解を深める dtu を参照してください、 [DTU セクション](#understanding-dtus) 」を参照します。 

>[AZURE.NOTE] このサービスの階層テーブルでその他のすべての行の詳細については、次を参照してください。 [サービス層の機能と制限](sql-database-performance-guidance.md#service-tier-capabilities-and-limits)します。

### エラスティック データベース プール サービス階層と eDTU のパフォーマンス
内の複数のデータベースを管理するオプションのあるもを作成し、1 つのデータベースをスケーリングするだけでなく、 [弾力性データベース プール](sql-database-elastic-pool.md)します。 エラスティック データベース プール内のデータベースはすべて、共通のリソース セットを共有します。 パフォーマンス特性が表されます *弾力性データベースのトランザクション単位* (Edtu)。 エラスティック データベース プールは 1 つのデータベースでは、次の 3 つのサービス階層で、: **基本的な**, 、**標準**, 、および **Premium**します。 エラスティック データベースの場合もやはり、この 3 つのサービス階層によってパフォーマンス全体に関する制限事項といくつかの機能が定義されます。  

エラスティック データベース プールの場合、これらのデータベースは DTU リソースを共有して使用することができ、プール内のデータベースに特定のパフォーマンス レベルを割り当てる必要はありません。 たとえば、Standard プール内の単一のデータベースで使用できるのは 0 eDTU から最大データベース eDTU (サービス階層によって定義された 100 eDTU またはユーザーが定義するカスタム値) までとなります。　 これにより、プール全体に提供された eDTU リソースを、多様なワークロードを持つ複数のデータベースが効率的に使用することができます。 

次の表では、エラスティック データベース プールのサービス階層の特性について説明します。

[AZURE.INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

プール内の各データベースはまた、該当する層における単一のデータベースの特性に従います。 たとえば、Basic プールでは、プールあたりの最大セッションが 2400 ～ 28800 の範囲に制限されますが、このプール内の個々のデータベースの場合、データベースあたりセッションが 300 に制限されます (前のセクションで指定した単一の Basic データベースに対する制限)。

### DTU について

[AZURE.INCLUDE [SQL DB DTU description](../../includes/sql-database-understanding-dtus.md)]

## データベース パフォーマンスの監視
SQL Database のパフォーマンスの監視は、選択したデータベース パフォーマンスのレベルに対するリソース使用率を監視することから始めます。 この関連データは、次の方法で表示されます。

1.  Microsoft Azure クラシック ポータル。

2.  ユーザー データベースおよびユーザー データベースを含むサーバーのマスター データベースの動的管理ビュー。

 [Azure ポータル](https://portal.azure.com/), 、1 つのデータベースの使用率を監視するには、データベースを選択し、クリックすると、 **監視** グラフ。 すると、 **メトリック** をクリックして変更できるウィンドウ、 **によるグラフの編集** ] ボタンをクリックします。 次のメトリックを追加します。

- CPU の割合
- DTU の割合
- データ IO の割合
- Storage の割合

それらを表示を行うこれらのメトリックを追加すると、 **監視** の詳細なグラフ、 **メトリック** ウィンドウです。 次の 4 つすべてのメトリックに対して相対的な平均使用率の割合を表示する、 **DTU** 、データベースのです。

![データベース パフォーマンスのサービス階層の監視](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

また、パフォーマンス メトリックに対してアラートを構成することができます。 クリックして、 **アラートの追加** ボタンをクリックして、 **メトリック** ウィンドウです。 ウィザードの指示に従ってアラートを構成します。 メトリックが特定のしきい値を超えた場合、またはメトリックが特定のしきい値を下回った場合に警告するオプションがあります。

たとえば、データベースのワークロードが増加する見込みの場合、データベースのいずれかのパフォーマンス メトリックが 80% に達すると電子メールのアラートを送信するように構成できます。 このアラートは、1 つ上位のパフォーマンス レベルに切り替えるタイミングを示す早期の警告として使用できます。

下位のパフォーマンス レベルにダウングレードできるかどうかを判断するために、パフォーマンス メトリックを利用することもできます。 たとえば、Standard S2 データベースを使用していて、すべてのパフォーマンス メトリックは、どの時点でもデータベースの平均的な使用率が 10% を超えないとします。 この場合、データベースは Standard S1 で快適に動作します。 ただし、下位のパフォーマンス レベルへの移行を決定する前に、急上昇や変動するワークロードに注意してください。 

システム ビューから入手できますが、ポータルで公開されているものと同じメトリックも: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) 論理 **マスター** 、サーバーのデータベースと [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) ユーザー データベース (**sys.dm_db_resource_stats** 各 Basic、Standard、および Premium のユーザー データベースで作成されます。 Web および Business Edition のデータベースは空の結果セットを返します)。 使用 **sys.resource_stats** 細分性の低いデータを長期間で監視する必要がある場合。 使用 **sys.dm_db_resource_stats** 小規模な期間内のより詳細なデータを監視する必要がある場合。 詳細については、次を参照してください。 [Azure SQL データベース パフォーマンス ガイダンス](sql-database-performance-guidance.md#monitoring-resource-use-with-sysresourcestats)します。

エラスティック データベース プールでは、このセクションで説明した手法を使用して、プール内の個々のデータベースを監視することができます。 ただし、プールを全体として監視することもできます。 詳細については、次を参照してください。 [モニター弾力性データベース プールの管理と](sql-database-elastic-pool-portal.md#monitor-and-manage-an-elastic-database-pool)です。

## 次のステップ
料金については、これらの階層を詳しく調べる [SQL Database の料金](http://azure.microsoft.com/pricing/details/sql-database/)します。

グループとして複数のデータベースを管理する場合は、 [エラスティック データベース プール](sql-database-elastic-pool-guidance.md) と共に [弾力性データベース プールの価格およびパフォーマンスの考慮事項](sql-database-elastic-pool-guidance.md)します。

SQL データベース階層についてわかったところで実際に試してみましょうで、 [無料評価版](http://azure.microsoft.com/pricing/free-trial/) 学習と [最初の SQL database を作成する方法](sql-database-get-started.md)!
 

