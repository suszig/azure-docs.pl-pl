<properties
    pageTitle="Azure ポータルでの SQL Data Warehouse データベースの作成 | Microsoft Azure"
    description="Azure ポータルで Azure SQL Data Warehouse を作成する方法を説明します。"
    services="sql-data-warehouse"
    documentationCenter="NA"
    authors="barbkess"
    manager="jhubbard"
    editor=""
    tags="azure-sql-data-warehouse"/>
<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="12/15/2015"
   ms.author="lodipalm;barbkess"/>

# SQL Data Warehouse の作成

> [AZURE.SELECTOR]
- [Azure ポータル](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

このチュートリアルでは、Azure ポータルを使用して、Azure SQL Data Warehouse データベースを数分で作成する方法について説明します。 

このチュートリアルでは、次のことについて説明します。

- データベースをホストするサーバーを作成する。
- AdventureWorksDW サンプル データベースを含んだデータベースを作成する。

SQL Data Warehouse に既存のデータベースを移行する場合を参照してください、 [移行の概要](./sql-data-warehouse-get-started-overview-migrate.md) 不正使用を [移行ユーティリティ](./sql-data-warehouse-migrate-migration-utility.md)します。

SQL Data Warehouse にデータを読み込むを参照してください、 [の概要を読み込んで](./sql-data-warehouse-overview-load.md)します。

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

## 手順 1: サインインして作業を開始する

1. サインイン、 [Azure ポータル](https://portal.azure.com)します。

2. クリックして **新しい** > **データ + ストレージ** > **SQL Data Warehouse**します。

    ![作成](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

1. [SQL Data Warehouse ] ブレードにデータベースの名前を入力します。 この例では、データベースに AdventureWorksDW という名前を付けます。

    ![データベース名の入力](./media/sql-data-warehouse-get-started-provision/database-name.png)


## 手順 2: サーバーを構成して作成する

SQL Database および SQL Data Warehouse では、各データベースがサーバーに割り当てられ、それぞれのサーバーが地理的な場所に割り当てられます。 このサーバーを論理 SQL サーバーといいます。

> [AZURE.NOTE] <a name="note"></a>論理 SQL server:
  >
  > + 地理的に同じ場所に複数のデータベースを構成するための、一貫性のある方法を提供します。
  > + オンプレミスのサーバーのような物理的なハードウェアではありません。 サービスのソフトウェアの一部です。 これは、ため、名前には、 *論理サーバー*します。
  > + 複数のデータベースを、それぞれのパフォーマンスに影響を与えずにホストできます。
  > + 小文字のみを使用して *s* 名前にします。 SQL **s**ーバーは Azure の論理サーバーに対し SQL **S**ーバーはマイクロソフトの内部設置型データベース製品です。

1. クリックして **Server** > **サーバーの新規作成**します。 サーバーの料金は発生しません。 使用する V12 論理 SQL サーバーが既にある場合は、既存のサーバーを選択し、次の手順に進みます。 

    ![新しいサーバーの作成](./media/sql-data-warehouse-get-started-provision/create-server.png)

3. 入力、 **新しいサーバー** 情報。 
    
    - **サーバー名**します。 論理サーバーの名前を入力します。 これは、地理的な場所ごとに一意です。
    - **サーバー管理者名**します。 サーバーの管理者アカウントのユーザー名を入力します。
    - **パスワード**します。 サーバーの管理者パスワードを入力します。 
    - **場所**します。 サーバーの地理的な場所を選択します。 データの転送時間を短縮するために、このデータベースがアクセスする他のデータ リソースと地理的に近い場所にサーバーを配置することをお勧めします。
    - **V12 サーバーの作成**します。 SQL Data Warehouse の場合は常に [はい] を選択してください。 
    - **Azure のサービスをサーバーにアクセスできるように**します。 SQL Data Warehouse の場合は常にオンになります。

    >[AZURE.NOTE] サーバー名、サーバー管理者名、およびパスワードをどこかに格納することを確認します。  この情報は、サーバーにログオンするときに必要になります。

1. クリックして **OK** を論理 SQL サーバーの構成設定を保存し、SQL Data Warehouse のブレードに戻ります。

    ![新しいサーバーの構成](./media/sql-data-warehouse-get-started-provision/configure-server.png)

## 手順 3: データベースを構成して作成する

論理 SQL サーバーを選択したら、データベースの作成に着手します。
 
2.  **SQL Data Warehouse** ブレードでその他のフィールドに入力します。 

    ![データベースを作成する](./media/sql-data-warehouse-get-started-provision/create-database.png)
    
    - **パフォーマンス**: 400 Dwu から始まることをお勧めします。 データベースのパフォーマンス レベルは、スライダーを左右に動かして調整します。データベースの作成後に調整することもできます。 

        > [AZURE.NOTE] SQL Data Warehouse では、Data Warehouse ユニット (Dwu) のパフォーマンスを測定します。 DWU を増やすと、データベース操作に使用できるコンピューティング リソースが SQL Data Warehouse によって増やされます。 ワークロードのパフォーマンスと DWU との関係は、ワークロードを実行するうちにわかるようになります。 
        > 
        > パフォーマンス レベルは、データベースの作成後に迅速かつ簡単に変更できます。  たとえば、データベースを使用していない場合は、左にスライダーを移動し、コストを削減します。 または、さらにリソースが必要な場合はパフォーマンスを上げます。 コストを 0 にするには、データベースを一時停止します。 これが、SQL Data Warehouse の拡張性です。

    - **[ソース**します。 クリックして **Select ソース** > **サンプル**します。 サンプル データベースの 1 つだけ使用できるため自動的に Azure のサンプルを選択すると、この時点での設定、 **Select サンプル** AdventureWorksDW でオプションです。 
  
        ![サンプルの選択](./media/sql-data-warehouse-get-started-provision/select-source.png)

    - **リソース グループ**します。 既定値を保持できます。 リソース グループは、Azure リソースのコレクション管理のサポートを目的としたコンテナーです。 詳細について [リソース グループ](../azure-portal/resource-group-portal.md)します。
    
    - **サブスクリプション**します。 このデータベースで請求する、サブスクリプションを選択します。

1. クリックして **作成** SQL Data Warehouse データベースを作成します。 

1. 数分するとデータベースの準備が整います。 返される終了したら、 [Azure ポータル](https://portal.azure.com)します。 SQL Data Warehouse データベースがダッシュボードに追加されていることに注意してください。

    ![ポータルの表示](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)


## 手順 4: クライアントの IP からアクセスできるようにサーバーのファイアウォールを構成する

現在ご使用の IP アドレスからサーバーに接続するには、そのクライアントの IP アドレスをファイアウォールの規則に追加します。 この手順では、その方法を紹介します。

1. クリックして **参照** > **の SQL Server** >、サーバーを選択 > **設定** > **ファイアウォール**します。

    ![ファイアウォール設定の検索](./media/sql-data-warehouse-get-started-provision/find-firewall-settings.png)

4. をクリックして **クライアントの ip アドレスを追加** して、Azure がクライアント IP アドレスに対してルールを作成します。 クリックして **保存**します。

    ![IP アドレスの追加](./media/sql-data-warehouse-get-started-provision/add-client-ip.png)

1. 一連の IP アドレスでファイアウォールの規則を作成します。 この作業は後から行うこともできます。

    >[AZURE.IMPORTANT] あなたの IP アドレスに、変更される可能性は、新しいファイアウォール ルールを作成するまでサーバーにアクセスすることはできません。 一貫したアクセスを確保するために、IP アドレスの範囲を追加することをお勧めします。 詳細については、次を参照してください。 [ファイアウォール設定を構成する方法](../sql-database/sql-database-configure-firewall-settings.md)します。

    ルールを作成するには、名前と IP アドレスの範囲を入力し、クリックして **保存**します。 

    ![ファイアウォール規則の追加](./media/sql-data-warehouse-get-started-provision/add-rule.png)

ファイアウォールの構成が完了したので、デスクトップから、作成した SQL Data Warehouse データベースへの接続を作成できます。

## 次のステップ

SQL Data Warehouse のサンプル データベースを作成する準備が整いましたに [接続](./sql-data-warehouse-get-started-connect.md) 、データベースにします。






