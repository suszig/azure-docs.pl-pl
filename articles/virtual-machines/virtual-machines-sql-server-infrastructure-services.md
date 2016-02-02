<properties 
    pageTitle="Virtual Machines での SQL Server の概要 | Microsoft Azure"
    description="この記事では、Azure Virtual Machines でホストされている SQL Server の概要について説明します。この記事には詳しいコンテンツへのリンクが掲載されています。" 
    services="virtual-machines" 
    documentationCenter="" 
    authors="rothja" 
    manager="jeffreyg"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services" 
    ms.date="11/12/2015"
    ms.author="jroth"/>


# Azure Virtual Machines における SQL Server の概要

## 使用の開始

ホストできます [Azure 仮想マシン上の SQL Server](http://azure.microsoft.com/services/virtual-machines/sql-server/) さまざまな構成では、1 つのデータベース サーバーから AlwaysOn 可用性グループと Azure の仮想ネットワークを使用して複数コンピューター構成までの範囲内です。
>[AZURE.NOTE] Azure VM で SQL Server を実行することは、リレーショナル データを Azure に格納するための 1 つのオプションです。 Azure SQL Database サービスを使用することもできます。 詳細については、次を参照してください。 [理解する Azure SQL Database と Azure Vm における SQL Server](../sql-database/data-management-azure-sql-database-and-sql-server-iaas.md)します。

Azure で SQL Server 仮想マシンを作成するには、最初に Azure プラットフォーム サブスクリプションを入手する必要があります。 Azure サブスクリプションを購入する [購入オプション](http://azure.microsoft.com/pricing/purchase-options/)します。 参照してください。 無料で試用、 [Azure 無料評価版](https://azure.microsoft.com/pricing/free-trial/)します。

### 1 つの VM での SQL Server インスタンスのデプロイ

Azure で SQL Server 仮想マシンを展開する最も簡単な方法が、サブスクリプションにサインアップした後で [Azure クラシック ポータルでは、SQL Server マシン ギャラリー イメージをプロビジョニング](virtual-machines-provision-sql-server.md)します。 それらのイメージには、VM の価格での SQL Server のライセンスが含まれます。
>[AZURE.NOTE] Azure ポータルを使用して、SQL Server Virtual Machines をプロビジョニングおよび管理します。 これにより、既定で Premium Storage が使用されるように設定され、自動修正、自動バックアップ、および AlwaysOn 構成が提供されます。

次の表には、仮想マシン ギャラリーで使用可能な SQL Server イメージのマトリックスが示されています。

| SQL Server のバージョン| オペレーティング システム| SQL Server のエディション|
|---|---|---|
| SQL Server 2008 R2 SP2| Windows Server 2008 R2| Enterprise、Standard、Web|
| SQL Server 2008 R2 SP3| Windows Server 2008 R2| Enterprise、Standard、Web|
| SQL Server 2012 SP2| Windows Server 2012| Enterprise、Standard、Web|
| SQL Server 2012 SP2| Windows Server 2012 R2| Enterprise、Standard、Web|
| SQL Server 2014| Windows Server 2012 R2| Enterprise、Standard、Web|
| SQL Server 2014 SP1| Windows Server 2012 R2| Enterprise、Standard、Web|
| SQL Server 2016 CTP| Windows Server 2012 R2| 評価|

>[AZURE.NOTE] データ ウェアハウスおよびトランザクション ワークロード用の仮想マシンのギャラリー イメージ (前に記載のない) は間もなく廃止され、ギャラリーから削除される予定です。 前の表の標準イメージを使用し、特定のワークロードのパフォーマンスを最適化してください。

これらの事前構成済みのイメージだけでなくすることもできます [Azure の仮想マシンの作成](virtual-machines-windows-tutorial.md) プレインストールされている SQL サーバーがない場合。 ライセンスのある SQL Server のインスタンスであればインストールできます。 使用して、Azure の仮想マシンで SQL Server を実行するため、ライセンスを Azure に移行する [Azure でのソフトウェア アシュアランスによるライセンス モビリティ](http://azure.microsoft.com/pricing/license-mobility/)します。 このシナリオでのみ支払いが Azure コンピューティングおよび記憶域の [コスト](http://azure.microsoft.com/pricing/details/virtual-machines) 仮想マシンに関連付けられています。

確認して、最適な仮想マシンの構成設定、SQL Server イメージを決定するために、 [Azure 仮想マシンにおける SQL Server のパフォーマンスのベスト プラクティス](virtual-machines-sql-server-performance-best-practices.md)します。 実稼働ワークロードでは、SQL Server Enterprise Edition の最小推奨仮想マシン サイズは **DS3** であり、Standard edition の最小推奨仮想マシン サイズは **DS2** です。

パフォーマンスのベスト プラクティスを確認するほかに、以下の初期タスクがあります。

- [Azure Vm における SQL Server のセキュリティのベスト プラクティスを確認します。](virtual-machines-sql-server-security-considerations.md)
- [接続をセットアップします。](virtual-machines-sql-server-connectivity.md)

### データの移行

SQL Server 仮想マシンを起動した後、マシンに既存のデータベースを移行できます。 複数の手法がありますが、SQL Server Management Studio のデプロイ ウィザードは、ほとんどのシナリオに適してします。 シナリオの詳細については、ウィザードのチュートリアルは「 [Azure VM 上の SQL Server データベースを移行する](virtual-machines-migrate-onpremises-database.md)します。

## 高可用性

高可用性が必要な場合は、SQL Server AlwaysOn 高可用性グループの構成を検討してください。 そのために、1 つの仮想ネットワークで複数の Azure VM を使用します。 Azure ポータルには、この構成が事前に設定されたテンプレートがあります。 詳細については、次を参照してください。 [Azure ギャラリーで提供する SQL Server AlwaysOn](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx)します。

可用性グループおよび関連付けられているリスナーを手動で構成する場合は、次の記事を参照してください。

- [(GUI) を Azure で AlwaysOn 可用性グループを構成します。](virtual-machines-sql-server-alwayson-availability-groups-gui.md)
- [Azure で AlwaysOn 可用性グループ用の ILB リスナーを構成します。](virtual-machines-sql-server-configure-ilb-alwayson-availability-group-listener.md)
- [Azure リソース マネージャー テンプレートを使用して SQL Server AlwaysOn のデプロイします。](virtual-machines-workload-template-sql-alwayson.md)
- [Azure への AlwaysOn 可用性グループの内部設置型の拡張します。](virtual-machines-sql-server-extend-on-premises-alwayson-availability-groups.md)

その他の高可用性の考慮事項を参照してください。 [高可用性と災害復旧の Azure 仮想マシンにおける SQL Server](virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions.md)します。

## バックアップと復元

オンプレミス データベースを使う場合は、SQL Server のバックアップ ファイルを格納するためのセカンダリ データ センターとして Azure を利用できます。 バックアップと復元のオプションの概要については、次を参照してください。 [Azure 仮想マシンにおける SQL Server のバックアップと復元](virtual-machines-sql-server-backup-and-restore.md)します。

[SQL Server Backup to URL](https://msdn.microsoft.com/library/dn435916.aspx) Azure のバックアップ ファイルを Azure blob ストレージに格納します。 [SQL Server Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) バックアップのスケジュールと Azure での保持期間を使用するとします。 これらのサービスは、オンプレミスの SQL Server インスタンスか、Azure VM で実行されている SQL Server で使用できます。 Azure Vm 活用することも [自動化されたバックアップ](virtual-machines-sql-server-automated-backup.md) と [自動修正](virtual-machines-sql-server-automated-patching.md) for SQL Server です。

## SQL Server VM イメージ構成の詳細

次の表では、プラットフォームによって提供される SQL Server の仮想マシン イメージの構成について説明します。

### Windows Server

プラットフォーム イメージでの Windows Server のインストールには、次の構成設定とコンポーネントが含まれています。

| 機能| 構成
|---|---|
| リモート デスクトップ| 管理アカウントに対して有効|
| Windows Update| 有効|
| ユーザー アカウント| 既定では、プロビジョニング中に指定されるユーザー アカウントは、ローカルの管理グループのメンバーです。この管理者アカウントは、SQL Server の sysadmin サーバー ロールのメンバーでもあります|
| ワークグループ| 仮想マシンは、WORKGROUP という名前のワークグループのメンバーです|
| ゲスト アカウント| 無効|
| セキュリティが強化された Windows ファイアウォール| 反対に、|
| .NET Framework| バージョン 4|
| ディスク| 選択したサイズにより、構成できるデータ ディスクの数が制限されます。を参照してください [Azure の仮想マシンのサイズ](virtual-machines-size-specs.md)|

### SQL Server

プラットフォーム イメージでの SQL Server のインストールには、次の構成設定とコンポーネントが含まれています。

| 機能| 構成|
|---|---|
| データベース エンジン| インストール済み|
| Analysis Services| インストール済み|
| Integration Services| インストール済み|
| Reporting Services| ネイティブ モードで構成|
| AlwaysOn 可用性グループ| SQL Server 2012 以降で使用できます。を必要と [追加の構成](virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions.md)
| レプリケーション| インストール済み|
| 検索用のフルテキストおよびセマンティック抽出| インストール済み (セマンティック抽出は SQL Server 2012 以降のみ)|
| Data Quality Services| インストール済み (SQL Server 2012 以降のみ)|
| Master Data Services| インストール済み (SQL Server 2012 以降のみ)。を必要と [追加の構成とコンポーネント](https://msdn.microsoft.com/library/ee633752.aspx)
| PowerPivot for SharePoint| 使用可能 (SQL Server 2012 以降のみ)追加構成とコンポーネントが必要です (SharePoint を含む)|
| 分散再生クライアント| 使用可能 (SQL Server 2012 以降のみ)、ただし未インストールを参照してください [プラットフォームによって提供される SQL Server イメージからセットアップを実行している SQL Server](#run-sql-server-setup-from-the-platform-provided-sql-server-image)|
| ツール| SQL Server Management Studio、SQL Server Configuration Manager、Business Intelligence Development Studio、SQL Server Setup、Client Tools Connectivity、Client Tools SDK、SQL Client Connectivity SDK や、データ層アプリケーション (DAC) などのアップグレードおよび移行ツール、バックアップ、リストア、アタッチ、および切断用ツールなど、すべてのツール|
| SQL Server オンライン ブック| インストールされているが、ヘルプ ビューアーを使用して構成する必要があります|

### データベース エンジンの構成

次のデータベース エンジンの設定が構成されています。 その他の設定については、SQL Server のインスタンスを調べてください。

| 機能| 構成|
|---|---|
| インスタンス| 共有メモリー プロトコルでのみライセンスを受けている SQL Server データベース エンジンの既定 (名前なし) インスタンスが含まれています|
| 認証| 既定で、Azure は SQL Server 仮想マシンのセットアップ中に Windows 認証を選択します。sa ログインを使用するか、新しい SQL Server アカウントを作成する場合は、この認証モードを変更する必要があります。詳細については、次を参照してください。 [Azure 仮想マシンにおける SQL Server のセキュリティの考慮事項](virtual-machines-sql-server-security-considerations.md)します。|
| sysadmin| 仮想マシンをインストールした Azure ユーザーが最初に属するのは、SQL Server sysadmin 固定サーバー ロールのメンバーのみです|
| メモリ| データベース エンジンのメモリは、動的メモリ構成に設定されます|
| データベース認証を含む| オフ|
| 既定の言語| 英語|
| データベース間での所有権の継承| オフ|

### カスタマー エクスペリエンス向上プログラム (CEIP)

[カスタマー エクスペリエンス向上プログラム (CEIP)](https://technet.microsoft.com/library/cc730757.aspx) が有効になっています。 SQL Server エラーと使用状況レポート ユーティリティを使用して、CEIP を無効にすることができます。 SQL Server エラーと使用状況レポート ユーティリティを起動するには、[スタート] メニューで [すべてのプログラム] をクリックし、[Microsoft SQL Server バージョン]、[構成ツール、]、[SQL Server エラーと使用状況レポート] の順にクリックします。 CEIP を有効にした状態で SQL Server のインスタンスを使用しない場合は、独自の仮想マシン イメージを Azure にデプロイすることも検討できます。 詳細については、次を参照してください。 [の作成と Windows Server オペレーティング システムを含むバーチャル ハード ディスクをアップロード](virtual-machines-create-upload-vhd-windows-server.md)します。

## プラットフォームによって提供される SQL Server イメージからの SQL Server セットアップの実行

プラットフォームによって提供される SQL Server イメージを使用して仮想マシンを作成する場合、仮想マシンの **C:\SqlServer_SQLMajorVersion.SQLMinorVersion_Full** ディレクトリに保存されている SQL Server セットアップ メディアを使用できます。 ディスク スペースに空きがある場合には、このディレクトリからセットアップを実行して、機能の追加や削除、新しいインスタンスの追加、またはインスタンスの修復などのセットアップ アクションを実行できます。
>[AZURE.NOTE] Azure では、複数のバージョンの SQL Server イメージを提供しています。 SQL Server プラットフォームによって提供されるイメージのバージョン リリース日が 2014 年 5 月 15 日以降の場合、既定でプロダクト キーが含まれています。 この日よりも前にパブリッシュされる、プラットフォームによって提供される SQL Server イメージを使用して仮想マシンをプロビジョニングする場合、VM にプロダクト キーは含まれません。 ベスト プラクティスとして、新しい VM をプロビジョニングするときは常に、最新のバージョンのイメージを選択することをお勧めします。

## リソース

- [Azure で SQL Server 仮想マシンのプロビジョニング](virtual-machines-provision-sql-server.md)
- [Azure VM で SQL Server にデータベースを移行します。](virtual-machines-migrate-onpremises-database.md)
- [高可用性と Azure の仮想マシンにおける SQL Server の災害復旧](virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions.md)
- [アプリケーション パターンと Azure の仮想マシンにおける SQL Server の開発計画](virtual-machines-sql-server-application-patterns-and-development-strategies.md)
- [Azure の仮想マシン](virtual-machines-about.md)





