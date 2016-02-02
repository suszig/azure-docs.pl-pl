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
- [Azure classic portal](virtual-machines-provision-sql-server.md)
- [PowerShell](virtual-machines-sql-server-create-vm-with-powershell.md)


## 概要

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。
.

Azure の仮想マシン イメージ ギャラリーには、Microsoft SQL Server を含むイメージがいくつか用意されています。 ギャラリーからいずれかの仮想マシン イメージを選択すると、わずか数クリックで、Azure 環境への仮想マシンのプロビジョニングを行うことができます。

このチュートリアルでは、次のことについて説明します。

* [Azure クラシック ポータルに接続し、ギャラリーから仮想マシンのプロビジョニング](#Provision)
* [リモート デスクトップと完全なセットアップを使用して仮想マシンを開く](#RemoteDesktop)
* [別のコンピューターで SQL Server Management Studio を使用して仮想マシンに接続するための構成手順を完了します。](#SSMS)
* [次のステップ](#Optional)

>[AZURE.NOTE] この記事では、既存のポータルを使用して SQL Server VM をプロビジョニングする方法を説明します。 ただし、作成し、SQL Server Vm を管理することはまた、 [新しいポータル](https://manage.windowsazure.com)します。 Premium Storage の使用が既定に設定されることや、自動修正、自動バックアップ、AlwaysOn 構成といったその他のオプションなど、新しいポータルにはいくつかの利点があります。 今後のコンテンツで、詳細な手順を説明します。

## <a id="Provision">ギャラリーからの SQL Server 仮想マシンのプロビジョニングします。</a>

1. ログイン、 [Azure クラシック ポータル](http://manage.windowsazure.com) 、アカウントを使用します。 Azure アカウントがないを参照してください。 [Azure 無料評価版](http://azure.microsoft.com/pricing/free-trial/)します。

2. Azure クラシック ポータルで、Web ページの左下にある **[+ 新規]** をクリックし、**[Compute]**、**[仮想マシン]**、**[ギャラリーから]** の順にクリックします。

3. **[イメージの選択]** ページで、**[SQL SERVER]** をクリックします。 SQL Server イメージを選択します。 ページの右下にある [次へ] 矢印をクリックします。

    ![イメージの選択](./media/virtual-machines-provision-sql-server/choose-sql-vm.png)

Azure でサポートされている SQL Server イメージの最新情報については、次を参照してください。 [Azure 仮想マシンの概要 [SQL Server](virtual-machines-sql-server-infrastructure-services.md)します。
>[AZURE.NOTE] プラットフォーム イメージの SQL Server 評価エディションを使用して仮想マシンを作成した場合、それをギャラリーにある分単位課金エディション イメージにアップグレードすることはできません。 次の 2 つのオプションのいずれかを選択できます。
>
> ギャラリーから時間単位有償 SQL Server のエディションを使用して、新しいバーチャル マシンを作成して」の手順に従ってデータベース ファイルをこの新しい仮想マシンに移行- [Azure VM 上の SQL Server にデータベースを移行する](virtual-machines-migrate-onpremises-database)
> または、[SQL Server の別のエディションに SQL Server 評価エディションの既存のインスタンスをアップグレードすることができます、 [Azure でのソフトウェア アシュアランスによるライセンス モビリティ](http://azure.microsoft.com/pricing/license-mobility/) 許諾契約書」の手順に従って、 [SQL Server の別のエディションにアップグレード](https://msdn.microsoft.com/library/cc707783.aspx)します。 SQL Server のライセンス コピーを購入する方法については、次を参照してください。 [SQL Server](http://www.microsoft.com/sqlserver/get-sql-server/how-to-buy.aspx)します。

4. 最初の **[仮想マシンの構成]** ページで、次の情報を指定します。
    - **バージョンのリリース日**。 複数のイメージが利用できる場合は、最新のイメージを選択します。
    - 一意の**仮想マシン名**。
    - **[新しいユーザー名]** ボックスに、コンピューターのローカル管理者アカウントの一意のユーザー名を入力します。
    - **[新しいパスワード]** ボックスに、強力なパスワードを入力します。
    - **[パスワードの確認]** ボックスに、パスワードを再度入力します。
    - **[サイズ]** ドロップダウン リストで、適切なサイズを選択します。

    ![VM 構成](./media/virtual-machines-provision-sql-server/4VM-Config.png)
    >[AZURE.NOTE] 仮想マシンのサイズは、プロビジョニングで指定します。
    >
    > -実稼働ワークロードをお勧め Premium Storage を使用すると次の最小限の推奨サイズ: **DS3** SQL Server Enterprise edition と **DS2** SQL Server Standard edition。 詳細については、次を参照してください。 [Azure 仮想マシンにおける SQL Server のパフォーマンスのベスト プラクティス](virtual-machines-sql-server-performance-best-practices.md)します。
    > -選択したサイズでは、構成できるデータ ディスクの数を制限します。 使用可能な仮想マシンのサイズと仮想マシンに接続できるデータ ディスクの数に関する最新情報については、次を参照してください。 [Azure の仮想マシンのサイズ](virtual-machines-size-specs.md)します。

5. VM 構成の詳細を入力後、右下にある次へ進む矢印をクリックして続行します。

5. 2 回目の **[仮想マシンの構成]** ページで、ネットワーク、ストレージ、可用性に関するリソースを構成します。
    - **[クラウド サービス]** ボックスで、**[新しいクラウド サービスの作成]** を選択します。
    - **[クラウド サービス DNS 名]** ボックスに、目的の DNS 名の最初の部分を入力すると、**TESTNAME.cloudapp.net** という形式の完全な名前を指定できます。
    - 複数のサブスクリプションがある場合は、**[サブスクリプション]** を選択します。 選択すると、どれかを判断 ** ストレージ アカウント **を利用します。
    - **[リージョン/アフィニティ グループ/仮想ネットワーク]** ボックスで、この仮想イメージをホストするリージョンを選択します。
    - **[ストレージ アカウント]** で、自動的にアカウントを生成するか、一覧から 1 つ選択します。 **[サブスクリプション]** を変更して複数のアカウントを表示します。
    - **[可用性セット]** ボックスの一覧の **[(なし)]** を選択します。
    - 法律条項を読み、同意します。

6. 矢印をクリックして次へ進みます。

7. 続行するには、右下隅にあるチェック マークをクリックします。

8. Azure によって仮想マシンの準備が行われるまで待ちます。 仮想マシンの状態は次のように進行します。

    - **開始中 (プロビジョニング)**
    - **Stopped**
    - **開始中 (プロビジョニング)**
    - **実行中 (プロビジョニング)**
    - **実行中**


## <a id="RemoteDesktop">セットアップを完了して、リモート デスクトップを使用して、VM を開く</a>

1. プロビジョニングが完了したら、仮想マシンの名前をクリックして [ダッシュボード] ページに移動します。 ページの下部にある **[接続]** をクリックします。

2. **[開く]** ボタンをクリックします。

    ![[開く] をクリックする](./media/virtual-machines-provision-sql-server/click-open-to-connect.png)

3. **[Windows セキュリティ]** ダイアログ ボックスで、**[別のアカウントを使用]** をクリックします。

    ![Click Use another account](./media/virtual-machines-provision-sql-server/credentials.png)

4. その後に次の形式で、管理者名、ドメイン名とマシンの名前を使用して: `machinename \username`します。 パスワードを入力し、マシンに接続します。

4. 初めてログオンした場合は、デスクトップのセットアップや Windows の更新プログラムの適用、Windows の初期構成タスク (sysprep) の完了など複数のプロセスが実行されます。 Windows sysprep の完了後、SQL Server セットアップは、構成タスクを完了します。 これらのタスクによって完了に数分の遅延が発生します。 `SELECT @@SERVERNAME` SQL Server セットアップが完了すると、SQL Server Management Studio をスタート ページに表示されない場合がありますまで、正しい名前を返さない場合があります。

Windows リモート デスクトップで仮想マシンに接続したら、仮想マシンは他のコンピューターと同様に使用できます。 SQL Server Management Studio (仮想マシン上で実行) を使用して、通常どおりに SQL Server の既定インスタンスに接続します。

## <a id="SSMS">別のコンピューターで SSMS から SQL Server VM インスタンスに接続します。</a>

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

## <a id="cdea">アプリケーションからデータベース エンジンへの接続します。</a>

Azure の仮想マシンで実行されている SQL Server のインスタンスに Management Studio から接続できる場合は、次のような接続文字列を使用して接続できます。

    connectionString = "Server=tutorialtestVM.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

詳細については、次を参照してください。 [方法、SQL Server データベース エンジンへの接続のトラブルシューティングを行うと](http://social.technet.microsoft.com/wiki/contents/articles/how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)します。

## <a id="Optional">次のステップ</a>

プラットフォーム イメージを使用してAzure に SQL Server 仮想マシンを作成し、構成する方法を見てきました。 多くの場合、次の手順はこの新しい SQL Server VM にデータベースを移行することです。 データベースの移行のガイドラインを参照してください。 [Azure VM 上の SQL Server にデータベースを移行する](virtual-machines-migrate-onpremises-database.md)します。

次の一覧には、Azure Virtual Machines における SQL Server のその他のリソースが示されています。

### Azure VM 上の SQL Server 用のお勧めのリソース:

- [Azure の仮想マシンの概要上の SQL Server](virtual-machines-sql-server-infrastructure-services.md)

- [Azure で SQL Server 仮想マシンへの接続します。](virtual-machines-sql-server-connectivity.md)

- [Azure の仮想マシンにおける SQL Server のパフォーマンスのベスト プラクティス](virtual-machines-sql-server-performance-best-practices.md)

- [Azure の仮想マシンにおける SQL Server のセキュリティの考慮事項](virtual-machines-sql-server-security-considerations.md)

### 高可用性と障害復旧

- [高可用性と Azure の仮想マシンにおける SQL Server の災害復旧](virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions.md)

- [Azure の仮想マシンにおける SQL Server のバックアップと復元](virtual-machines-sql-server-backup-and-restore.md)

### Azure での SQL Server ワークロード

- [Azure の仮想マシンで SQL Server Business Intelligence](virtual-machines-sql-server-business-intelligence.md)

### ホワイト ペーパー

- [Azure SQL Database と Azure の仮想マシンにおける SQL Server をについてください。](sql-database/data-management-azure-sql-database-and-sql-server-iaas.md)

- [アプリケーション パターンと Azure の仮想マシンにおける SQL Server の開発計画](virtual-machines-sql-server-application-patterns-and-development-strategies.md)





