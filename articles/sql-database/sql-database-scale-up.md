<properties
    pageTitle="Azure SQL Database のサービス階層とパフォーマンス レベルを変更する"
    description="「Azure SQL Database のサービス階層とパフォーマンス レベルを変更する」では、SQL Databaseのスケール アップとスケール ダウンの方法について説明しています。Azure SQL Database の価格レベルを変更します。"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jeffreyg"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="12/01/2015"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# SQL Database のサービス階層とパフォーマンス レベル (価格レベル) を変更する

**1 つのデータベース**

> [AZURE.SELECTOR]
- [Azure Portal](sql-database-scale-up.md)
- [PowerShell](sql-database-scale-up-powershell.md)


この記事を使用して SQL database のサービス階層とパフォーマンス レベルを変更する方法を示しています、 [Azure ポータル](https://portal.azure.com)します。

内の情報を使用して [を新しいサービス階層 SQL Database Web/ビジネス データベースをアップグレード](sql-database-upgrade-new-service-tiers.md) と [Azure SQL Database サービス階層とパフォーマンス レベル](sql-database-service-tiers.md) Azure SQL Database の適切なサービス階層とパフォーマンス レベルを決定します。
> [AZURE.IMPORTANT] SQL Database のサービス階層とパフォーマンス レベルの変更はオンライン操作です。 そのため、操作中はダウンタイムがなく、データベースを使用できます。

- データベースをダウングレードするには、データベースがダウングレード後のサービス階層で許可されている最大サイズより小さい必要があります。
- データベースをアップグレードするときに [標準 Geo レプリケーション](https://msdn.microsoft.com/library/azure/dn758204.aspx) または [アクティブ Geo レプリケーション](https://msdn.microsoft.com/library/azure/dn741339.aspx) が有効なまずアップグレードそのセカンダリ データベース目的のパフォーマンス階層にプライマリ データベースをアップグレードする前にします。
- Premium サービス階層からダウングレードするときは、最初に geo レプリケーション リレーションシップをすべて終了する必要があります。 説明する手順を行うことができる、 [連続コピー リレーションシップの終了](https://msdn.microsoft.com/library/azure/dn741323.aspx) トピックは、プライマリとアクティブ セカンダリ データベース間のレプリケーション プロセスを停止します。
- サービス階層によって、提供されている復元サービスは異なります。 ダウングレードすると、特定の時点に復元する機能を使えなくなったり、バックアップの保存期間が短くなったりする可能性があります。 詳細については、次を参照してください。 [Azure SQL データベースのバックアップと復元](https://msdn.microsoft.com/library/azure/jj650016.aspx)します。
- 24 時間の期間内に 4 つまでの個別のデータベースの変更 (サービス階層またはパフォーマンス レベル) を行うことができます。
- データベースに対する新しいプロパティは、変更が完了するまで適用されません。


**この記事を完了するには、以下が必要です。**

- Azure サブスクリプション。 Azure サブスクリプションをお持ちでない場合、このページの上部の**無料試用版**をクリックしてからこの記事に戻り、最後まで完了してください。
- Azure SQL Database。 SQL データベースがない、1 つ作成この記事の手順: [最初の Azure SQL Database を作成する](sql-database-get-started.md)します。


## データベースのサービス階層とパフォーマンス レベルを変更する

スケールアップまたはスケールダウンするデータベースの [SQL Database] ブレードを開きます。

1.  移動して、 [Azure ポータル](https://portal.azure.com)します。
2.  **[すべて参照]** をクリックします。
3.  **[SQL Database]** をクリックします。
2.  変更するデータベースをクリックします。
3.  [SQL Database] ブレードで **[価格レベル]** をクリックします。

    ![価格タイル][1]

1.  新しい階層を選択し、**[選択]** をクリックします。

    **[選択]** をクリックすると、データベース階層を変更するスケール要求が送信されます。 データベースのサイズに応じて、スケール操作の完了に時間がかかる場合があります。 通知をクリックすると、スケール操作の詳細と状態が表示されます。

    ![価格レベルの選択][2]

3.  左側のリボンにある **[通知]** をクリックします。

    ![通知][3]

## データベースが選択した価格レベルであることを確認する

   スケール操作が完了したら、データベースが目的の階層になっていることを確認します。

2.  **[すべて参照]** をクリックします。
3.  **[SQL Database]** をクリックします。
2.  更新したデータベースをクリックします。
3.  **[価格レベル]** タイルで、正しい階層に設定されていることを確認します。

    ![新しい価格][4]


## 次のステップ

- [スケール アウトとスケールイン](sql-database-elastic-scale-get-started.md)
- [接続し、SSMS での SQL データベースの照会](sql-database-connect-query-ssms.md)
- [Azure SQL データベースをエクスポートします。](sql-database-export.md)

## その他のリソース

- [ビジネス継続性の概要](sql-database-business-continuity.md)
- [SQL Database のドキュメント](https://azure.microsoft.com/documentation/services/sql-database/)




[1]: ./media/sql-database-scale-up/pricing-tile.png 
[2]: ./media/sql-database-scale-up/choose-tier.png 
[3]: ./media/sql-database-scale-up/scale-notification.png 
[4]: ./media/sql-database-scale-up/new-tier.png 

