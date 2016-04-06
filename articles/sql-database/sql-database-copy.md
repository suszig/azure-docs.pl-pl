<properties
    pageTitle="Azure SQL Database のコピー"
    description="Azure SQL Database のコピーの作成"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jeffreyg"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/16/2015"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# Azure SQL Database のコピー

**1 つのデータベース**

> [AZURE.SELECTOR]
- [Azure ポータル](sql-database-copy.md)
- [PowerShell](sql-database-copy-powershell.md)
- [SQL](sql-database-copy-transact-sql.md)

次の手順は、使用した SQL データベースをコピーする方法を示して、 [Azure ポータル](https://portal.azure.com)します。 データベース コピー操作により新しい SQL データベースが作成されます。 コピーは、同じサーバーまたは別のサーバーに作成したデータベースのスナップショット バックアップです。

> [AZURE.NOTE] Azure SQL データベースは自動的に作成され、すべてのユーザー データベースを復元するバックアップを保持します。 詳細については、「 [ビジネス継続性の概要](sql-database-business-continuity.md)します。

コピー プロセスが完了すると、新しいデータベースは、コピー元のデータベースに依存せずに完全に機能するデータベースになります。 コピーの完了時点で、新しいデータベースのトランザクションはコピー元のデータベースと同じになります。 データベース コピーのサービス レベルとパフォーマンス レベル (価格レベル) はコピー元のデータベースと同じになります。 コピーの完了後、コピーは完全に機能する独立したデータベースになります。 ログイン、ユーザー、アクセス許可は非依存で管理できます。


データベースを同じ論理サーバーにコピーすると、両方のデータベースで同じログインを利用できます。 データベースをコピーするために使用するセキュリティ プリンシパルが、新しいデータベースのデータベース所有者 (DBO) になります。 すべてのデータベース ユーザー、アクセス許可、セキュリティ識別子 (SID) がデータベースのコピーにコピーされます。


SQL Database をコピーするには、以下が必要です。

- Azure サブスクリプション。 をクリックする必要がある場合、Azure サブスクリプションだけで **無料評価版** これの上部にあるページをこの記事を完了する通します。
- コピーする SQL データベース。 SQL データベースがない、1 つ作成この記事の手順: [最初の Azure SQL Database を作成する](sql-database-get-started.md)です。



## SQL データベースのコピー

コピーするデータベースの SQL データベース ブレードを開きます。

1.  移動して、 [Azure ポータル](https://portal.azure.com)します。
2.  [参照] > [SQL データベース] の順に選択し、コピーするデータベースにアクセスします。
3.  SQL データベース] ブレードで [ **コピー** を開くには、 **コピー** ブレード。

    ![データベースのコピー][1]

1.  データベース コピーの名前を入力します。 既定の名前が提示されますが、任意で変更できます。
2.  選択、 **ターゲット サーバー**します。 対象サーバーとは、データベース コピーが作成される場所です。 新しいサーバーを作成したり、一覧から既存のサーバーを選択したりできます。
3.  をクリックして **OK** コピー プロセスを開始します。

    ![データベースの名前とサーバー][2]




## コピー操作の進行状況の監視

2.  コピーの開始後、ポータル通知をクリックすると、詳細が表示されます。


    ![notification][3]

 
    ![monitor][4]





## サーバーにデータベースが存在することの確認

- クリックして **参照** > **SQL データベース** し、新しいデータベースを確認して **オンライン**します。



## 次のステップ

- [SQL Server Management Studio (SSMS) での接続](sql-database-connect-to-database.md)
- [データベースを BACPAC にエクスポートする](sql-database-export.md)



## その他のリソース

- [ビジネス継続性の概要](sql-database-business-continuity.md)
- [SQL Database のドキュメント](https://azure.microsoft.com/documentation/services/sql-database/)


<!--Image references-->
[1]: ./media/sql-database-copy/copy.png
[2]: ./media/sql-database-copy/copy-ok.png
[3]: ./media/sql-database-copy/copy-notification.png
[4]: ./media/sql-database-copy/monitor-copy.png



