<properties
    pageTitle="Azure SQL Database の BACPAC の作成およびエクスポート"
    description="Azure SQL Database の BACPAC の作成およびエクスポート"
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


# Azure SQL Database の BACPAC の作成およびエクスポート

**1 つのデータベース**

> [AZURE.SELECTOR]
- [Azure ポータル](sql-database-export.md)
- [PowerShell](sql-database-export-powershell.md)

この記事で使用して Azure SQL データベースの BACPAC をエクスポートする方法については、 [Azure ポータル](https://portal.azure.com)します。

A [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) .bacpac ファイルで、データベース スキーマとデータが含まれています。 基本的なユース ケース BACPAC は、データベースを移動する 1 つのサーバー間には [ローカル データベースをクラウドに移行](sql-database-cloud-migrate.md), 、および既存のデータベースをオープン フォーマットでアーカイブされます。

> [AZURE.NOTE] Bacpac は、バックアップに使用すると、復元操作はしていません。 Azure SQL Database では、すべてのユーザー データベースのバックアップが自動的に作成されます。 詳細については、「 [ビジネス継続性の概要](sql-database-business-continuity.md)します。


BACPAC は Azure Storage BLOB コンテナーにエクスポートされます。操作が正常に完了した後にダウンロードできます。

この記事を完了するには、以下が必要です。

- Azure サブスクリプション。 をクリックする必要がある場合、Azure サブスクリプションだけで **無料評価版** これの上部にあるページをこの記事を完了する通します。
- Azure SQL Database。 SQL データベースがない、1 つ作成この記事の手順: [最初の Azure SQL Database を作成する](sql-database-get-started.md)です。
-  [Azure ストレージ アカウント](storage-create-storage-account.md) に BACPAC を格納する blob コンテナーとします。 ストレージ アカウントが、従来の配置モデルを使用する必要があります現在ように選択 **クラシック** ストレージ アカウントを作成するときにします。 


## データベースのエクスポート

エクスポートするデータベースの [SQL Database] ブレードを開きます。

> [AZURE.IMPORTANT] BACPAC ファイルをトランザクション上の一貫性を保証するためにはまず [、データベースのコピーを作成](sql-database-copy.md) し、そのデータベースのコピーをエクスポートします。 

1.  移動して、 [Azure ポータル](https://portal.azure.com)します。
2.  クリックして **[すべて参照**します。
3.  クリックして **SQL データベース**します。
2.  BACPAC としてエクスポートするデータベースをクリックします。
3.  SQL データベース] ブレードで [ **エクスポート** を開くには、 **データベースのエクスポート** ブレード。

    ![[エクスポート] ボタン][1]

1.  をクリックして **ストレージ** し、BACPAC を保存するストレージ アカウントおよび blob コンテナーを選択します。

    ![データベースのエクスポート][2]

1.  入力、 **サーバー管理ログイン** と **パスワード** をエクスポートするデータベースを含む Azure SQL サーバーにします。
1.  をクリックして **作成** データベースをエクスポートします。

クリックすると **作成** のデータベースのエクスポート要求を作成およびサービスに送信します。 データベースのサイズに応じて、エクスポート操作の完了に時間がかかる場合があります。

## エクスポート操作の進行状況の監視

2.  クリックして **[すべて参照**します。
3.  クリックして **の SQL server**します。
2.  エクスポートした元 (ソース) のデータベースを含むサーバーをクリックします。
3.  SQL server] ブレードで [ **インポート/エクスポート履歴**:

    ![インポート/エクスポート履歴][3]
    ![インポート/エクスポート履歴][4]

## BACPAC がストレージ コンテナーにあることの確認

2.  クリックして **[すべて参照**します。
3.  クリックして **ストレージ アカウント (クラシック)**します。
2.  BACPAC を格納したストレージ アカウントをクリックします。
3.  クリックして **コンテナー** 詳細については (ダウンロードしてここから BACPAC を保存) にデータベースをエクスポートしたコンテナーを選択します。

    ![.bacpac ファイルの詳細][5]  


## 次のステップ

- [Azure SQL Database のインポート](sql-database-import.md)



## その他のリソース

- [ビジネス継続性の概要](sql-database-business-continuity.md)
- [障害復旧訓練](sql-database-disaster-recovery-drills.md)
- [SQL Database のドキュメント](https://azure.microsoft.com/documentation/services/sql-database/)


<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-history.png
[4]: ./media/sql-database-export/export-status.png
[5]: ./media/sql-database-export/bacpac-details.png


