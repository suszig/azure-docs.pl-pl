<properties
    pageTitle="BACPAC ファイルをインポートして新しい Azure SQL Database を作成する"
    description="既存の BACPAC ファイルをインポートして新しい Azure SQL Database を作成する"
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


# BACPAC ファイルをインポートして新しい Azure SQL Database を作成する


**1 つのデータベース**

> [AZURE.SELECTOR]
- [Azure ポータル](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)

この記事で使用して、BACPAC ファイルから新しい Azure SQL データベースを作成する方法については、 [Azure ポータル](https://portal.azure.com)します。

BACPAC は、データベース スキーマとデータを含む .bacpac ファイルです。 詳細については、バックアップ パッケージ (.bacpac) を参照してください [データ層アプリケーション](https://msdn.microsoft.com/library/ee210546.aspx)します。

Azure Storage BLOB コンテナーからインポートされた BACPAC からデータベースが作成されます。 Azure ストレージに .bacpac ファイルがあるない場合は、次の手順に従って、1 つを作成 [作成と Azure SQL Database の BACPAC のエクスポート](sql-database-export.md)します。


> [AZURE.NOTE] Azure SQL データベースは自動的に作成され、すべてのユーザー データベースを復元するバックアップを保持します。 詳細については、「 [ビジネス継続性の概要](sql-database-business-continuity.md)します。


.bacpac から SQL Database をインポートするには、以下が必要です。

- Azure サブスクリプション。 をクリックする必要がある場合、Azure サブスクリプションだけで **無料評価版** これの上部にあるページをこの記事を完了する通します。
- Azure SQL Database V12 サーバー。 V12 サーバーがない、1 つ作成この記事の手順: [最初の Azure SQL Database を作成する](sql-database-get-started.md)です。
- インポートするデータベースの .bacpac ファイル、 [Azure ストレージ アカウント (クラシック)](storage-create-storage-account.md) blob コンテナーです。


## データベースを含めるサーバーの選択

次のように、インポートするデータベースの [SQL Server] ブレードを開きます。

1.  移動して、 [Azure ポータル](https://portal.azure.com)します。
2.  クリックして **[すべて参照**します。
3.  クリックして **の SQL server**します。
2.  データベースを復元するサーバーをクリックします。
3.  SQL Server] ブレードで [ **インポート データベース** を開くには、 **データベースのインポート** ブレード。

    ![データベースのインポート][1]

1.  をクリックして **ストレージ** ストレージ アカウント、blob コンテナー、および .bacpac ファイルを選択し、クリックして **OK**します。

    ![記憶域のオプションを構成する][2]

1.  新しいデータベースの価格レベルを選択し、クリックして **選択**します。

    ![価格レベルを選択][3]

1.  入力、 **データベース名**します。
2.  入力、 **サーバー管理ログイン** と **パスワード** Azure SQL サーバーのデータベースをインポートします。
1.  クリックして **作成** BACPAC からデータベースを作成します。

    ![データベースの作成][4]

クリックすると **作成** インポート データベース要求をサービスに送信します。 データベースのサイズに応じて、インポート操作の完了に時間がかかる場合があります。

## インポート操作の進行状況の監視

2.  クリックして **[すべて参照**します。
3.  クリックして **の SQL server**します。
2.  復元先のサーバーをクリックします。
3.  SQL server] ブレードで [ **インポート/エクスポート履歴**:

    ![インポートのエクスポートの履歴][5]
    ![インポートのエクスポートの履歴][6]





## サーバーにデータベースが存在することの確認

2.  クリックして **[すべて参照**します。
3.  クリックして **SQL データベース** し、新しいデータベースを確認して **オンライン**します。



## 次のステップ

- [SQL Server Management Studio (SSMS) での接続](sql-database-connect-to-database.md)



## その他のリソース

- [SQL Database のドキュメント](https://azure.microsoft.com/documentation/services/sql-database/)


<!--Image references-->
[1]: ./media/sql-database-import/import-database.png
[2]: ./media/sql-database-import/storage-options.png
[3]: ./media/sql-database-import/pricing-tier.png
[4]: ./media/sql-database-import/create.png
[5]: ./media/sql-database-import/import-history.png
[6]: ./media/sql-database-import/import-status.png

