<properties 
    pageTitle="SQL Server 仮想マシンのプロビジョニング | Microsoft Azure" 
    description="このチュートリアルでは、Azure で SQL Server VM を作成および構成する方法を説明します。" 
    services="virtual-machines" 
    documentationCenter="" 
    authors="rothja" 
    manager="jeffreyg" 
    editor="monicar"
    tags="azure-service-management"
    />

<tags 
    ms.service="virtual-machines" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows-sql-server" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2015" 
    ms.author="jroth"/>

# Azure での SQL Server 仮想マシンのプロビジョニング

> [AZURE.SELECTOR]
- [Azure クラシック ポータル](virtual-machines-provision-sql-server.md)
- [PowerShell](virtual-machines-sql-server-create-vm-with-powershell.md)

## 概要

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。
. 

Azure の仮想マシン イメージ ギャラリーには、Microsoft SQL Server を含むイメージがいくつか用意されています。 ギャラリーからいずれかの仮想マシン イメージを選択すると、わずか数クリックで、Azure 環境への仮想マシンのプロビジョニングを行うことができます。

このチュートリアルでは、次のことについて説明します。

* [Azure クラシック ポータルに接続し、ギャラリーから仮想マシンのプロビジョニングを行う](#Provision)
* [リモート デスクトップを使用して仮想マシンを開き、セットアップを完了する](#RemoteDesktop)
* [別のコンピューターにある SQL Server Management Studio を使用して仮想マシンに接続するための構成手順を完了する](#SSMS)
* [次のステップ](#Optional)

>[AZURE.NOTE] この記事では、既存のポータルでの SQL Server VM をプロビジョニングする方法について説明します。 ただし、作成し、SQL Server Vm を管理することはまた、 [新しいポータル](https://manage.windowsazure.com)します。 Premium Storage の使用が既定に設定されることや、自動修正、自動バックアップ、AlwaysOn 構成といったその他のオプションなど、新しいポータルにはいくつかの利点があります。 今後のコンテンツで、詳細な手順を説明します。

##<a id="Provision">ギャラリーからの SQL Server 仮想マシンのプロビジョニング</a>

1. ログイン、 [Azure クラシック ポータル](http://manage.windowsazure.com) 、アカウントを使用します。 Azure アカウントがないを参照してください。 [Azure 無料評価版](http://azure.microsoft.com/pricing/free-trial/)します。

2. Azure クラシック ポータルで web ページの左下にある] クリックして **+ 新規**, 、] をクリックして **コンピューティング**, 、] をクリックして **仮想マシン**, 、順にクリック **ギャラリーから**します。

3.  **イメージの選択** ] ページで [ **SQL SERVER**します。 SQL Server イメージを選択します。 ページの右下にある [次へ] 矢印をクリックします。 

    ![イメージの選択](./media/virtual-machines-provision-sql-server/choose-sql-vm.png)

Azure でサポートされている SQL Server イメージの最新情報については、次を参照してください。 [Azure 仮想マシンの概要 [SQL Server](virtual-machines-sql-server-infrastructure-services.md)します。

>[AZURE.NOTE] プラットフォーム イメージの SQL Server 評価エディションを使用して作成された仮想マシンがある場合は、ギャラリー内にある分単位課金エディション イメージにアップグレードできません。 次の 2 つのオプションのいずれかを選択できます。
>
> - 分単位課金エディション、ギャラリーの SQL Server を使用して新しいバーチャル マシンを作成し、次の手順に従って、この新しい仮想マシンにデータベース ファイルを移行 [Azure VM 上の SQL Server にデータベースを移行します。](virtual-machines-migrate-onpremises-database)
> - または、[SQL Server の別のエディションに SQL Server 評価エディションの既存のインスタンスをアップグレードすることができます、 [Azure でのソフトウェア アシュアランスによるライセンス モビリティ](http://azure.microsoft.com/pricing/license-mobility/) 許諾契約書」の手順に従って、 [SQL Server の別のエディションにアップグレード](https://msdn.microsoft.com/library/cc707783.aspx)します。 SQL Server のライセンス コピーを購入する方法については、次を参照してください。 [SQL Server](http://www.microsoft.com/sqlserver/get-sql-server/how-to-buy.aspx)します。

4. 最初の **仮想マシンの構成** ] ページで、次の情報を提供します。
    - A **バージョンのリリース日**します。 複数のイメージが利用できる場合は、最新のイメージを選択します。
    - 一意な **仮想マシン名**します。
    -  **新しいユーザー名** ボックスに、コンピューターのローカル管理者アカウントの一意のユーザー名を入力します。
    -  **新しいパスワード** ボックスに、強力なパスワードを入力します。 
    -  **パスワードの確認** ボックスで、パスワードを再入力します。
    - 適切な選択 **サイズ** ドロップダウン リストからです。 

    ![VM 構成](./media/virtual-machines-provision-sql-server/4VM-Config.png)

    >[AZURE.NOTE] 仮想マシンのサイズはプロビジョニング中に指定します。
    >
    > - 実稼働ワークロードをお勧め Premium Storage を使用すると次の最小限の推奨サイズ: **DS3** SQL Server Enterprise edition と **DS2** SQL Server Standard edition。 詳細については、次を参照してください。 [Azure 仮想マシンにおける SQL Server のパフォーマンスのベスト プラクティス](virtual-machines-sql-server-performance-best-practices.md)します。
    > - 選択したサイズにより、構成できるデータ ディスクの数が制限されます。 使用可能な仮想マシンのサイズと仮想マシンに接続できるデータ ディスクの数に関する最新情報については、次を参照してください。 [Azure の仮想マシンのサイズ](virtual-machines-size-specs.md)します。

5. VM 構成の詳細を入力後、右下にある次へ進む矢印をクリックして続行します。

5. 2 番目の **仮想マシンの構成** ] ページで、ネットワー キング、ストレージ、および可用性に関するリソースを構成します。
    -  **クラウド サービス** ボックスで、選択 **新しいクラウド サービスを作成する**です。
    -  **クラウド サービス DNS 名** 形式の名前が完了すると、任意の DNS 名の最初の部分を入力するボックスに、 **TESTNAME.cloudapp.net** 
    - 選択、 **サブスクリプション**, から選択する複数のサブスクリプションがあります場合。 この選択で、利用できる**ストレージ アカウントが決まります。
    -  **リージョン/アフィニティ グループ/仮想ネットワーク** ボックスで、この仮想イメージをホストするリージョンを選択します。
    -  **ストレージ アカウント**, 、自動的に、アカウントを生成するか、一覧からいずれかを選択します。 変更、 **サブスクリプション** して複数のアカウントを表示します。 
    -  **可用性セット** ボックスで、 **(なし)**します。
    - 法律条項を読み、同意します。
    

6. 矢印をクリックして次へ進みます。


7. 続行するには、右下隅にあるチェック マークをクリックします。

8. Azure によって仮想マシンの準備が行われるまで待ちます。 仮想マシンの状態は次のように進行します。

    - **開始中 (プロビジョニング)**
    - **停止済み**
    - **開始中 (プロビジョニング)**
    - **実行中 (プロビジョニング)**
    - **実行中**
    

##<a id="RemoteDesktop">リモート デスクトップを使用して VM を開き、設定を完了します。</a>

1. プロビジョニングが完了したら、仮想マシンの名前をクリックして [ダッシュボード] ページに移動します。 ページの下部にあるをクリックして **接続**します。

2. クリックして、 **開く** ] ボタンをクリックします。

    ![[開く] をクリックする](./media/virtual-machines-provision-sql-server/click-open-to-connect.png)

3.  **Windows セキュリティ** ダイアログ ボックスで、をクリックして **別のアカウントを使用して**します。

    ![[別のアカウントを使用] をクリックする](./media/virtual-machines-provision-sql-server/credentials.png) 

4. `machinename\username` の形式で、ドメイン名に管理者名が付くマシンの名前を使用します。 パスワードを入力し、マシンに接続します。

4. 初めてログオンした場合は、デスクトップのセットアップや Windows の更新プログラムの適用、Windows の初期構成タスク (sysprep) の完了など複数のプロセスが実行されます。 Windows sysprep の完了後、SQL Server セットアップは、構成タスクを完了します。 これらのタスクによって完了に数分の遅延が発生します。 `SELECT @@SERVERNAME` SQL Server セットアップが完了し、SQL Server Management Studio をスタート ページに表示されない場合がありますまで、正しい名前を返されない場合があります。

Windows リモート デスクトップで仮想マシンに接続したら、仮想マシンは他のコンピューターと同様に使用できます。 SQL Server Management Studio (仮想マシン上で実行) を使用して、通常どおりに SQL Server の既定インスタンスに接続します。 

##<a id="SSMS">別のコンピューターで SSMS から SQL Server VM インスタンスに接続します。</a>

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

## <a id="cdea">アプリケーションからデータベース エンジンに接続する</a>

Azure の仮想マシンで実行されている SQL Server のインスタンスに Management Studio から接続できる場合は、次のような接続文字列を使用して接続できます。

    connectionString = "Server=tutorialtestVM.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

詳細については、次を参照してください。 [方法、SQL Server データベース エンジンへの接続のトラブルシューティングを行うと](http://social.technet.microsoft.com/wiki/contents/articles/how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)です。

##<a id="Optional">次のステップ</a>

プラットフォーム イメージを使用してAzure に SQL Server 仮想マシンを作成し、構成する方法を見てきました。 多くの場合、次の手順はこの新しい SQL Server VM にデータベースを移行することです。 データベースの移行のガイドラインを参照してください。 [Azure VM 上の SQL Server にデータベースを移行する](virtual-machines-migrate-onpremises-database.md)です。

次の一覧には、Azure Virtual Machines における SQL Server のその他のリソースが示されています。

### Azure VM 上の SQL Server 用のお勧めのリソース:
- [Azure Virtual Machines における SQL Server の概要](virtual-machines-sql-server-infrastructure-services.md)

- [Azure での SQL Server 仮想マシンへの接続](virtual-machines-sql-server-connectivity.md)

- [Azure Virtual Machines における SQL Server のパフォーマンスに関するベスト プラクティス](virtual-machines-sql-server-performance-best-practices.md)

- [Azure Virtual Machines における SQL Server のセキュリティに関する考慮事項](virtual-machines-sql-server-security-considerations.md)

### 高可用性と障害復旧
- [Azure Virtual Machines における SQL Server の高可用性と障害復旧](virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions.md)

- [Azure Virtual Machines おける SQL Server のバックアップと復元](virtual-machines-sql-server-backup-and-restore.md)

### Azure での SQL Server ワークロード
- [Azure Virtual Machines での SQL Server Business Intelligence](virtual-machines-sql-server-business-intelligence.md)

### ホワイト ペーパー
- [Understand Azure SQL Database and SQL Server in Azure Virtual Machines (Azure Virtual Machines における Azure SQL Database と SQL Server について)](sql-database/data-management-azure-sql-database-and-sql-server-iaas.md)

- [Azure Virtual Machines における SQL Server のアプリケーション パターンと開発計画](virtual-machines-sql-server-application-patterns-and-development-strategies.md)


