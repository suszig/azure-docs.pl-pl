<properties 
    pageTitle="Azure Virtual Network での Active Directory フォレストのインストール | Microsoft Azure" 
    description="Azure Virtual Network 上の仮想マシン (VM) に新しい Active Directory フォレストを作成する手順について説明したチュートリアルです。" 
    services="active-directory, virtual-network" 
    documentationCenter="" 
    authors="markusvi" 
    manager="stevenpo" 
    tags=""/>

<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/20/2015" 
    ms.author="markusvi"/>


# Azure Virtual Network での新しい Active Directory フォレストのインストール

このトピックでは、仮想マシン (VM) 上の Azure 仮想ネットワークに新しい Windows Server Active Directory 環境を作成する方法を説明する [Azure 仮想ネットワーク](../virtual-network/virtual-networks-overview.md)します。 この場合、Azure の仮想ネットワークはオンプレミスのネットワークには接続されません。 

必要に応じて次の関連するトピックも参照してください。

- 次の手順を示すビデオについては、次を参照してください [Azure の仮想ネットワークに新しい Active Directory フォレストをインストールする方法。](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
- こともできます [サイト対サイト VPN の構成](../vpn-gateway/vpn-gateway-site-to-site-create.md) とし、新しいフォレストをインストールまたは内部設置型フォレスト Azure の仮想ネットワークを拡張します。 これらの手順については、次を参照してください。 [Azure の仮想ネットワークでのレプリカ Active Directory ドメイン コント ローラーのインストール](../virtual-networks-install-replica-active-directory-domain-controller.md)します。
-  Azure の仮想ネットワークで Active Directory ドメイン サービス (AD DS) のインストールに関する概念的なガイダンスを参照してください。 [を展開する Windows Server Active Directory を Azure Virtual Machines に関するガイドライン](https://msdn.microsoft.com/library/azure/jj156090.aspx)します。

## シナリオ図

このシナリオでは、外部ユーザーは、ドメインに参加しているサーバーで稼働するアプリケーションにアクセスする必要があります。 アプリケーション サーバーを実行する VM およびドメイン コントローラーを実行する VM は、Azure Virtual Network の専用のクラウド サービスにインストールされます。 また、フォールト トレランス向上のため、可用性セットにも含まれます。

![][1]
7
## オンプレミスの場合との違い

ドメイン コントローラーのオンプレミスへのインストールと Azure へのインストールにそれほど大きな違いはありません。 主な違いを次の表に示します。 

構成対象  | オンプレミスの
  | Azure 仮想ネットワーク 
------------- | -------------  | ------------
**ドメイン コントローラーの IP アドレス**  | ネットワーク アダプターのプロパティの静的 IP アドレスを割り当てる   | Set-AzureStaticVNetIP コマンドレットを実行して、静的 IP アドレスを割り当てる
**DNS クライアント リゾルバー**  | ドメイン メンバーのネットワーク アダプターのプロパティの優先および代替 DNS サーバー アドレスを設定する   | 仮想ネットワークのプロパティの DNS サーバー アドレスを設定する
**Active Directory データベース ストレージ**  | 既定の保存先を C:\ から変更する (省略可能)  | 既定の保存先を C:\ から変更する (必須)



## Azure の仮想ネットワークを作成する

1. Azure クラシック ポータルにサインインします。
2. 仮想ネットワークを作成します。 クリックして **ネットワーク** > **仮想ネットワークの作成**します。 次の表の値を使用してウィザードの手順を完了します。 

    ウィザードのページ  | 指定する値
    ------------- | -------------
    **Virtual Network Details**  | <p>[名前]: 仮想ネットワークの名前を入力します。</p><p>リージョン: 最寄りのリージョンを選択する</p>
    **DNS と VPN**  | <p>[DNS サーバー] は空白のままにする</p><p>いずれの VPN オプションも選択しない</p>
    **仮想ネットワーク アドレス空間**  | <p>サブネット名: サブネットの名前を入力する</p><p>開始 IP: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p>



## ドメイン コントローラーと DNS サーバーのロールを実行する仮想マシンを作成する
 
次の手順を繰り返して、必要に応じて、DC ロールをホストする VM を作成します。 フォールト トレランスと冗長性を確保するには 2 つ以上の仮想 DC をデプロイする必要があります。 Azure の仮想ネットワークに、類似した構成の DC (いずれも GCで、DNS サーバーを実行し、FSMO ロールを保持していないなど) が 2 つ以上あると、それらの DC を実行する仮想マシンを可用性セットに置くとフォールト トレランスが向上します。

UI ではなく Windows PowerShell を使用して Vm を作成するを参照してください。 [を作成し、Windows ベースの仮想マシンを事前構成する Azure PowerShell を使用して](../virtual-machines-ps-create-preconfigure-windows-vms.md)します。

1. 従来のポータルで、クリックして **新規** > **コンピューティング** > **仮想マシン** > **ギャラリーから**します。 次の値を使用して、ウィザードを完了します。 別の値が推奨されたり、要求される場合を除いては、既定の設定値を受け入れます。

    ウィザードのページ  | 指定する値
    ------------- | -------------
    **イメージの選択**  | Windows Server 2012 R2 Datacenter
    **仮想マシンの構成**  | <p>仮想マシン名: 単一ラベルの名前 (azuredc1 など) を入力します。</p><p>新しいユーザー名: ユーザーの名前を入力します。 このユーザーは、VM 上のローカルの Administrators グループのメンバーになります。 最初に VM にサインインするときは、この名前でサインインする必要があります。 Administrator という名前の組み込みのアカウントは機能しません。</p><p>新しいパスワード/確認入力: パスワードを入力します。</p>
    **仮想マシンの構成**  | <p>クラウド サービス: 選択 <b>新しいクラウド サービスを作成する</b> 多くの Vm を作成するときにその同じクラウド サービス名が DC ロールをホストし、最初の VM にします。</p><p>グローバルに一意の名前を指定するクラウド サービス DNS 名。</p><p>リージョン/アフィニティ グループ/仮想ネットワーク: 仮想ネットワーク名 (例: westusvnet) を指定します。</p><p>ストレージ アカウント: 選択 <b>自動的に生成されたストレージ アカウントを使用して</b> クリックして最初の VM の追加の Vm を作成するときにその同じストレージ アカウント名が DC ロールをホストします。</p><p>可用性セット]: 選択 <b>可用性セットの作成</b>します。</p><p>可用性セットの名前: 可用性セット、最初の VM を作成し、選択したときに多くの Vm を作成するときに同じ名前の名前を入力します。</p>
    **仮想マシンの構成**  | <p>選択 <b>VM エージェントをインストール</b> とする必要がある他の拡張機能です。</p>
2. DC サーバーのロールを実行する各 VM にディスクを接続します。 AD データベース、ログ、SYSVOL を格納するには、追加のディスクが必要です。 (10 GB など)、ディスクのサイズを指定して、 **ホスト キャッシュ設定]** 設定 **None**します。 手順については、次を参照してください。 [Windows 仮想マシンへのデータ ディスクを接続する方法](../storage-windows-attach-disk.md)します。
3. VM に初めてサインインした後に開く **サーバー マネージャー** > **ファイルおよび記憶域サービス** NTFS を使用してこのディスクにボリュームを作成します。
4. DC ロールを実行する VM の静的 IP アドレスを予約します。 静的 IP アドレスを予約するには、Microsoft Web Platform Installer をダウンロードし、 [Azure PowerShell のインストール](../powershell-install-configure.md) Set-azurestaticvnetip コマンドレットを実行します。 次に例を示します。

    'Get-AzureVM -ServiceName AzureDC1 -Name AzureDC1 | Set-AzureStaticVNetIP -IPAddress 10.0.0.4 | Update-AzureVM

静的 IP アドレスを設定する方法の詳細については、次を参照してください。 [VM の静的内部 IP アドレスを構成する](../virtual-network/virtual-networks-reserved-private-ip.md)です。

## Windows Server Active Directory をインストールする

同じルーチンを使用して [AD DS のインストール](https://technet.microsoft.com/library/jj574166.aspx) 内部設置型を使用すること (つまり、使用することができます使用、UI、応答ファイル、または Windows PowerShell)。 管理者の資格情報を指定して新しいフォレストをインストールする必要があります。 Active Directory データベースの場所、ログ、SYSVOL を指定するには、既定の保存先をオペレーティング システム ドライブから、VM にアタッチした追加のデータ ディスクに変更します。 

DC のインストールが完了したら、仮想マシンに再び接続し、DC にログオンします。 必ずドメインの資格情報の指定してください。

## Azure 仮想ネットワークの DNS サーバーをリセットする

1. 新しい DC/DNS サーバーでDNS フォワーダー設定をリセットします。 
  1. サーバー マネージャーで、クリックして **ツール** > **DNS**します。 
  2.  **DNS マネージャー**, DNS サーバーの名前を右クリックし、クリックして、 **プロパティ**します。 
  3.  **フォワーダー** ] タブで、フォワーダーの IP アドレスをクリックし、クリックして **編集**します。  IP アドレスを選択し、クリックして **削除**します。 
  4. をクリックして **OK** エディターを閉じと **Ok** DNS サーバーのプロパティを閉じます。 
2. 仮想ネットワークの DNS サーバーの設定を更新します。 
  1. をクリックして **仮想ネットワーク** > を作成した仮想ネットワークをダブルクリックして > **構成** > **DNS サーバー**, 名前と、DC および DNS サーバーの役割を実行する仮想マシンの 1 つの DIP を入力してをクリックして **保存**します。 
  2. VM を選択し、クリックして **再起動** 、新しい DNS サーバーの IP アドレスを DNS リゾルバーの設定を構成する VM を起動します。 


## ドメイン メンバーの VM を作成する

1. 次の手順を繰り返して、アプリケーション サーバーとして実行するように VM を設定します。 別の値が推奨されたり、要求される場合を除いては、既定の設定値を受け入れます。

    ウィザードのページ  | 指定する値
    ------------- | -------------
    **イメージの選択**  | Windows Server 2012 R2 Datacenter
    **仮想マシンの構成**  | <p>仮想マシン名: 単一ラベルの名前 (例: appserver1) を入力します。</p><p>新しいユーザー名: ユーザーの名前を入力します。 このユーザーは、VM 上のローカルの Administrators グループのメンバーになります。 最初に VM にサインインするときは、この名前でサインインする必要があります。 Administrator という名前の組み込みのアカウントは機能しません。</p><p>新しいパスワード/確認入力: パスワードを入力します。</p>
    **仮想マシンの構成**  | <p>クラウド サービス: 選択 **新しいクラウド サービスを作成する** し、最初の VM の多くの Vm を作成するときにその同じクラウド サービス名は、アプリケーションをホストします。</p><p>グローバルに一意の名前を指定するクラウド サービス DNS 名。</p><p>リージョン/アフィニティ グループ/仮想ネットワーク: 仮想ネットワーク名 (例: westusvnet) を指定します。</p><p>ストレージ アカウント: 選択 **自動的に生成されたストレージ アカウントを使用して** クリックして最初の VM の同じストレージ アカウント名を複数の Vm を作成するときに、アプリケーションをホストします。</p><p>可用性セット]: 選択 **可用性セットの作成**します。</p><p>可用性セットの名前: 可用性セット、最初の VM を作成し、選択したときに多くの Vm を作成するときに同じ名前の名前を入力します。</p>
    **仮想マシンの構成**  | <p>選択 <b>VM エージェントをインストール</b> とする必要がある他の拡張機能です。</p>
2. 各 VM がプロビジョニングされたら、サインインし、ドメインに参加させます。  **サーバー マネージャー**, 、クリックして **ローカル サーバー** > **ワークグループ** > **変更しています.** クリックして **ドメイン** し、内部設置型ドメインの名前を入力します。 ドメイン ユーザーの資格情報を入力し、VM を再起動して、ドメインへの参加を完了します。

UI ではなく Windows PowerShell を使用して Vm を作成するを参照してください。 [を作成し、Windows ベースの仮想マシンを事前構成する Azure PowerShell を使用して](../virtual-machines-ps-create-preconfigure-windows-vms.md)します。

詳細については、Windows PowerShell を使用して、次を参照してください。 [Azure コマンドレットの概要](https://msdn.microsoft.com/library/azure/jj554332.aspx) と [Azure コマンドレット リファレンス](https://msdn.microsoft.com/library/azure/jj554330.aspx)します。


## 関連項目

-  [How to install a new Active Directory forest on an Azure virtual network (Azure Virtual Network に新しい Active Directory フォレストをインストールする方法)](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
-  [Azure の仮想マシンでの Windows Server Active Directory の展開ガイドライン](https://msdn.microsoft.com/library/azure/jj156090.aspx)
-  [Configure a Cloud-Only Virtual Network (クラウド専用 Virtual Network の構成)](../virtual-network/virtual-networks-create-vnet.md)
-  [サイト間 VPN の構成](../vpn-gateway/vpn-gateway-site-to-site-create.md)
-  [Azure の仮想ネットワークでのレプリカ Active Directory ドメイン コントローラーのインストール](../virtual-networks-install-replica-active-directory-domain-controller.md)
-  [Microsoft Azure IT Pro IaaS: (01) 仮想マシンの基礎](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) 仮想ネットワークとクロスプレミス接続の作成](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Virtual Network の概要](../virtual-network/virtual-networks-overview.md)
-  [Azure PowerShell のインストールおよび構成方法](../powershell-install-configure.md)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure コマンドレット リファレンス](https://msdn.microsoft.com/library/azure/jj554330.aspx)
-  [Set Azure VM Static IP Address (Azure VM の静的 IP アドレスの設定)](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
-  [Azure VM への静的 IP アドレスの割り当て方法](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
-  [Install a New Active Directory Forest (新しい Active Directory フォレストのインストール)](https://technet.microsoft.com/library/jj574166.aspx)
-  [Active Directory ドメイン サービス (AD DS) の仮想化 (レベル 100) の概要](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png

 


