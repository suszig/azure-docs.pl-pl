<properties
    pageTitle="Azure でのレプリカ ドメイン コント ローラーのインストール |Microsoft Azure"
    description="このチュートリアルでは、Azure の仮想マシンにオンプレミス Active Directory フォレストからドメイン コントローラーをインストールする方法について説明します。"
    services="virtual-network"
    documentationCenter=""
    authors="curtand"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2015"
    ms.author="curtand"/>


# Azure の仮想ネットワークでのレプリカ Active Directory ドメイン コントローラーのインストール

このトピックでは、Azure の仮想ネットワークの Azure 仮想マシン (VM) 上に、オンプレミス Active Directory ドメインの追加のドメイン コントローラー (レプリカ DC とも呼ばれます) をインストールする方法を説明します。

必要に応じて次の関連するトピックも参照してください。

-  オプションで、Azure の仮想ネットワーク上に新しい Active Directory フォレストをインストールすることもできます。 これらの手順については、次を参照してください。 [Azure の仮想ネットワークで Active Directory フォレストのインストール](../active-directory-new-forest-virtual-machine.md)します。
-  Azure の仮想ネットワークで Active Directory ドメイン サービス (AD DS) のインストールに関する概念的なガイダンスを参照してください。 [を展開する Windows Server Active Directory を Azure Virtual Machines に関するガイドライン](https://msdn.microsoft.com/library/azure/jj156090.aspx)します。


## シナリオ図

このシナリオでは、外部ユーザーは、ドメインに参加しているサーバーで稼働するアプリケーションにアクセスする必要があります。 アプリケーション サーバーとレプリカ DC を実行する VM は、Azure の仮想ネットワーク内にインストールされています。 仮想ネットワークを内部設置型ネットワークに接続することができます、 [サイト対サイト VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) には、次の図に示すように、接続で使用できる [ExpressRoute](../../services/expressroute/) 速く接続します。

アプリケーション サーバーと Dc が個別に展開されている [クラウド サービス](../cloud-services-what-is.md) コンピューティング処理を配布する内 [可用性セット](../virtual-machines/virtual-machines-manage-availability.md) のフォールト トレランスを強化します。
DC は、Active Directory レプリケーションを使用して、DC 同士で相互にレプリケートします。また、オンプレミス DC でレプリケートされます。 同期ツールは必要ありません。

![][1]

## Azure の仮想ネットワークの Active Directory サイトを作成する

仮想ネットワークに対応するネットワーク地域の Active Directory にサイトを作成することをお勧めします。 認証、レプリケーション、他の DC の検出オペレーションが最適化されます。 次の手順をサイトを作成し、背景情報については、次を参照してください。 方法を説明 [新しいサイトを追加する](https://technet.microsoft.com/library/cc781496.aspx)です。

1. Active Directory サイトとサービスを開きます。 **サーバー マネージャー** > **ツール** > **Active Directory サイトとサービス**します。
2. Azure の仮想ネットワークを作成するリージョンを表すサイトを作成する:] をクリックして **サイト** > **アクション** > **新しいサイト** > Azure 米国西部など、新しいサイトの名前を入力 > サイト リンクを選択 > **[ok]**します。
3. サブネットを作成し、新しいサイトと関連付ける: ダブルクリック **サイト** > を右クリックして **サブネット** > **新しいサブネット** > (シナリオ図の 10.1.0.0/16) などの仮想ネットワークの IP アドレス範囲を入力 > 新しい Azure サイトを選択 > **[ok]**します。

## Azure の仮想ネットワークを作成する

1. Azure クラシック ポータルで、クリックして **新規** > **ネットワーク サービス** > **仮想ネットワーク** > **カスタム作成** し、次の値を使用して、ウィザードを完了します。

    ウィザードのページ  | 指定する値
    ------------- | -------------
    **Virtual Network Details**  | <p>[名前]:「westusvnet」などの仮想ネットワーク名前を入力します。</p><p>リージョン: 最寄りのリージョンを選択します。</p>
    **DNS と VPN 接続**  | <p>DNS サーバー: は、1 つまたは複数の内部設置型 DNS サーバーの IP アドレスと名前を指定します。</p><p>接続: 選択 **サイト対サイト VPN の構成**します。</p><p>ローカル ネットワーク: 新しいローカル ネットワークを指定します。</p><p>VPN ではなく ExpressRoute を使用している場合は、次を参照してください。 [Exchange プロバイダーを通じて ExpressRoute 接続の構成](../expressroute/expressroute-configuring-exps.md)します。</p>
    **サイト間接続**  | <p>[名前]: は、内部設置型ネットワークの名前を入力します。</p><p>VPN デバイスの IP アドレス: 仮想ネットワークに接続するデバイスのパブリック IP アドレスを指定します。 VPN デバイスは NAT の内側に配置することはできません。</p><p>アドレス: シナリオ図の 192.168.0.0/16) など、内部設置型ネットワークのアドレスの範囲を指定します。</p>
    **仮想ネットワーク アドレス空間**  | <p>アドレス空間: シナリオ図の 10.1.0.0/16) などの Azure 仮想ネットワークで実行する Vm の IP アドレスの範囲を指定します。 このアドレス範囲を、内部設置型ネットワークのアドレスの範囲と重複させることはできません。</p><p>サブネット: アプリケーション サーバー用のサブネットのアドレスと名前を指定する (frontend、10.1.1.0/24) と、Dc (backend、10.1.2.0/24)。</p><p>クリックして **ゲートウェイ サブネットの追加**します。</p>

2. 次に、仮想ネットワーク ゲートウェイを構成して、セキュリティで保護されたサイト間 VPN 接続を作成します。 参照してください [仮想ネットワーク ゲートウェイを構成する](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) 手順については、です。
3. 新しい仮想ネットワークと内部設置型 VPN デバイスの間に、サイト間 VPN 接続を作成します。 参照してください [仮想ネットワーク ゲートウェイを構成する](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) 手順については、です。



## DC ロールの Azure VM を作成する

次の手順を繰り返して、必要に応じて、DC ロールをホストする VM を作成します。 フォールト トレランスと冗長性を確保するには 2 つ以上の仮想 DC をデプロイする必要があります。 Azure の仮想ネットワークに、類似した構成の DC (いずれも GCで、DNS サーバーを実行し、FSMO ロールを保持していないなど) が 2 つ以上あると、それらの DC を実行する仮想マシンを可用性セットに置くとフォールト トレランスが向上します。
UI ではなく Windows PowerShell を使用して Vm を作成するを参照してください。 [を作成し、Windows ベースの仮想マシンを事前構成する Azure PowerShell を使用して](../virtual-machines/virtual-machines-ps-create-preconfigure-windows-vms.md)します。

1. Azure クラシック ポータルで、クリックして **新規** > **コンピューティング** > **仮想マシン** > **ギャラリーから**します。 次の値を使用して、ウィザードを完了します。 別の値が推奨されたり、要求される場合を除いては、既定の設定値を受け入れます。

    ウィザードのページ  | 指定する値
    ------------- | -------------
    **イメージの選択**  | Windows Server 2012 R2 Datacenter
    **仮想マシンの構成**  | <p>仮想マシン名: 単一ラベルの名前 (azuredc1 など) を入力します。</p><p>新しいユーザー名: ユーザーの名前を入力します。 このユーザーは、VM 上のローカルの Administrators グループのメンバーになります。 最初に VM にサインインするときは、この名前でサインインする必要があります。 Administrator という名前の組み込みのアカウントは機能しません。</p><p>新しいパスワード/確認入力: パスワードを入力します。</p>
    **仮想マシンの構成**  | <p>クラウド サービス: 選択 <b>新しいクラウド サービスを作成する</b> 多くの Vm を作成するときにその同じクラウド サービス名が DC ロールをホストし、最初の VM にします。</p><p>グローバルに一意の名前を指定するクラウド サービス DNS 名。</p><p>リージョン/アフィニティ グループ/仮想ネットワーク: 仮想ネットワーク名 (例: westusvnet) を指定します。</p><p>ストレージ アカウント: 選択 <b>自動的に生成されたストレージ アカウントを使用して</b> クリックして最初の VM の追加の Vm を作成するときにその同じストレージ アカウント名が DC ロールをホストします。</p><p>可用性セット]: 選択 <b>可用性セットの作成</b>します。</p><p>可用性セットの名前: 可用性セット、最初の VM を作成し、選択したときに多くの Vm を作成するときに同じ名前の名前を入力します。</p>
    **仮想マシンの構成**  | <p>選択 <b>VM エージェントをインストール</b> とする必要がある他の拡張機能です。</p>
2. DC サーバーのロールを実行する各 VM にディスクを接続します。 AD データベース、ログ、SYSVOL を格納するには、追加のディスクが必要です。 (10 GB など)、ディスクのサイズを指定して、 **ホスト キャッシュ設定]** 設定 **None**します。 手順については、次を参照してください。 [Windows 仮想マシンへのデータ ディスクを接続する方法](../virtual-machines/storage-windows-attach-disk.md)します。
3. VM に初めてサインインした後に開く **サーバー マネージャー** > **ファイルおよび記憶域サービス** NTFS を使用してこのディスクにボリュームを作成します。
4. DC ロールを実行する VM の静的 IP アドレスを予約します。 静的 IP アドレスを予約するには、Microsoft Web Platform Installer をダウンロードし、 [Azure PowerShell のインストール](../powershell-install-configure.md) Set-azurestaticvnetip コマンドレットを実行します。 次に例を示します。

    'Get-AzureVM -ServiceName AzureDC1 -Name AzureDC1 | Set-AzureStaticVNetIP -IPAddress 10.0.0.4 | Update-AzureVM

静的 IP アドレスを設定する方法の詳細については、次を参照してください。 [VM の静的内部 IP アドレスを構成する](../virtual-network/virtual-networks-reserved-private-ip.md)です。

## Azure VM に AD DS をインストールする

VM にサインインし、サイト間 VPN 接続または ExpressRoute 接続で、オンプレミス ネットワーク上のリソースに接続できることを確認します。 次に、Azure VM に AD DS をインストールします。 オンプレミス ネットワーク上に追加の DC をインストールするために使用するのと同じプロセス (UI、Windows PowerShell、応答ファイル) を使用できます。 AD DS をインストールするときには、AD データベース、ログ、SYSVOL の場所の新しいボリュームを必ず指定します。 AD DS のインストールの更新を実行する場合を参照してください。  [Active Directory ドメイン サービスをインストール (レベル 100)](https://technet.microsoft.com/library/hh472162.aspx) または [を既存のドメイン (レベル 200) レプリカ Windows Server 2012 ドメイン コント ローラーをインストール](https://technet.microsoft.com/library/jj574134.aspx)します。

## 仮想ネットワークの DNS サーバーを再構成する

1. Azure クラシック ポータルで仮想ネットワークの名前をクリックし、 **構成** ] タブと [、仮想ネットワークの DNS サーバーの IP アドレスを再構成](virtual-networks-manage-dns-in-vnet.md) 内部設置型 DNS サーバーの IP アドレスではなく、レプリカ Dc に割り当てられた静的 IP アドレスを使用します。

2. クリックすると、仮想ネットワークの DNS サーバーを使用すると、仮想ネットワークのすべてのレプリカ DC Vm が構成されていることを確認するには、 **仮想マシン**, を各仮想マシンの状態列をクリックして、クリックして **再起動**します。 VM が表示されるまで待機 **を実行している** サインインしようとする前にします。

## アプリケーション サーバー用の VM を作成する

1. 次の手順を繰り返して、アプリケーション サーバーとして実行するように VM を設定します。 別の値が推奨されたり、要求される場合を除いては、既定の設定値を受け入れます。


    On this wizard page…  | Specify these values
    ------------- | -------------
    **Choose an Image**  | Windows Server 2012 R2 Datacenter
    **Virtual Machine Configuration**  | <p>Virtual Machine Name: Type a single label name (such as  AppServer1).</p><p>New User Name: Type the name of a user. This user will be a member of the local Administrators group on the VM. You will need this name to sign in to the VM for the first time. The built-in account named Administrator will not work.</p><p>New Password/Confirm: Type a password</p>
    **Virtual Machine Configuration**  | <p>Cloud Service: Choose **Create a new cloud service** for the first VM and select that same cloud service name when you create more VMs that will host the application.</p><p>Cloud Service DNS Name: Specify a globally unique name</p><p>Region/Affinity Group/Virtual Network: Specify the virtual network name (such as WestUSVNet).</p><p>Storage Account: Choose **Use an automatically generated storage account** for the first VM and then select that same storage account name when you create more VMs that will host the application.</p><p>Availability Set: Choose **Create an availability set**.</p><p>Availability set name: Type a name for the availability set when you create the first VM and then select that same name when you create more VMs.</p>
    **Virtual Machine Configuration**  | <p>Select <b>Install the VM Agent</b> and any other extensions you need.</p>

2. 各 VM がプロビジョニングされたら、サインインし、ドメインに参加させます。  **サーバー マネージャー**, 、クリックして **ローカル サーバー** > **ワークグループ** > **変更しています.** クリックして **ドメイン** し、内部設置型ドメインの名前を入力します。 ドメイン ユーザーの資格情報を入力し、VM を再起動して、ドメインへの参加を完了します。

UI ではなく Windows PowerShell を使用して Vm を作成するを参照してください。 [を作成し、Windows ベースの仮想マシンを事前構成する Azure PowerShell を使用して](../virtual-machines/virtual-machines-ps-create-preconfigure-windows-vms.md)します。

詳細については、Windows PowerShell を使用して、次を参照してください。 [Azure コマンドレットの概要](https://msdn.microsoft.com/library/azure/jj554332.aspx) と [Azure コマンドレット リファレンス](https://msdn.microsoft.com/library/azure/jj554330.aspx)します。

## その他のリソース

-  [Azure の仮想マシンでの Windows Server Active Directory の展開ガイドライン](https://msdn.microsoft.com/library/azure/jj156090.aspx)
-  [Azure PowerShell を使用して既存のオンプレミス ハイパー-V のドメイン コント ローラーを Azure にアップロードする方法](http://support.microsoft.com/kb/2904015)
-  [Azure Virtual Network での新しい Active Directory フォレストのインストール](../active-directory-new-forest-virtual-machine.md)
-  [Azure 仮想ネットワーク](../virtual-network/virtual-networks-overview.md)
-  [Microsoft Azure IT Pro IaaS: (01) 仮想マシンの基礎](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) 仮想ネットワークとクロスプレミス接続の作成](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure の管理コマンドレット](https://msdn.microsoft.com/library/azure/jj152841)

<!--Image references-->
[1]: ./media/virtual-networks-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png


