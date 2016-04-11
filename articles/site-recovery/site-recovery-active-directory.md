<properties
    pageTitle="Azure Site Recovery で Active Directory と DNS を保護する | Microsoft Azure" 
    description="この記事では、Azure Site Recovery を使って Active Directory の障害復旧ソリューションを実装する方法について説明します。" 
    services="site-recovery" 
    documentationCenter="" 
    authors="prateek9us" 
    manager="abhiag" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="12/14/2015" 
    ms.author="pratshar"/>

# Azure Site Recovery で Active Directory と DNS を保護する

SharePoint、Dynamics AX、SAP などのエンタープライズ アプリケーションは、Active Directory と DNS インフラストラクチャを基盤として正常な動作が実現されています。 アプリケーションの障害復旧ソリューションを作成するときに重要なことは、Active Directory と DNS を保護しておき、他のアプリケーション コンポーネントよりも先に復旧させる必要があるということです。そうしないと、障害が発生したときにアプリケーションが正しく機能しません。 

Site Recovery は、仮想マシンのレプリケーション、フェールオーバー、復旧を指揮することによって障害復旧機能をつかさどる Azure サービスです。 Site Recovery は、仮想マシンとアプリケーションの一貫した保護、およびプライベート/パブリック クラウドまたはホスト側のクラウドへのシームレスなフェールオーバーを実行するための多くのレプリケーション シナリオをサポートしています。 

Site Recovery を使用すると、Active Directory に合わせて完全な自動障害復旧計画を作成できます。 障害が発生した場合、どこからでも数秒でフェールオーバーを開始し、数分以内に Active Directory を稼働させることができます。 プライマリ サイトで複数のアプリケーション (SharePoint と SAP など) 用に Active Directory をデプロイした場合で、かつサイト全体をフェールオーバーする必要がある場合、Site Recovery を使って先に Active Directory をフェールオーバーした後、アプリケーションごとの復旧計画を使って他のアプリケーションをフェールオーバーすることができます。

この記事では、Active Directory 用の障害復旧ソリューションの作成方法や、ワンクリック復旧計画を使用した計画済み、計画外、テスト フェールオーバーの実行方法、サポートされている構成と前提条件について説明します。  先に進む前に、Active Directory と Azure Site Recovery について理解しておく必要があります。

推奨される選択肢は 2 つあり、実際の環境の複雑さに応じて使い分けてください。

### 方法 1

少数のアプリケーションと、1 つのドメイン コント ローラーがあり、サイト全体をフェールオーバーする場合はお勧め Site Recovery を使用して、ドメイン コント ローラーを (かどうかフェールオーバーを Azure またはセカンダリ サイトに)、セカンダリ サイトにレプリケートするとします。 レプリケートされた同じ仮想マシンをテスト フェールオーバー用にも使用できます。

### 方法 2

環境内に多数のアプリケーションがあってドメイン コントローラーが複数存在する場合、または少数のアプリケーションを一度にフェールオーバーすることを検討している場合は、Site Recovery でドメイン コントローラーの仮想マシンをレプリケートすることに加え、別のドメイン コントローラーをターゲット サイト (Azure またはセカンダリのオンプレミス データセンター) にセットアップすることをお勧めします。 

>[AZURE.NOTE] 場合でも、Site Recovery を使用してドメイン コント ローラーをレプリケートする必要がありますテスト フェールオーバーを行うためのオプション 2 を実装しています。 読み取り [テスト フェールオーバーの考慮事項](#considerations-for-test-failover) の詳細。 


以降のセクションでは、Site Recovery でドメイン コントローラーの保護を有効にする方法と、Azure でドメイン コントローラーをセットアップする方法について説明します。 


## 前提条件

- Active Directory と DNS サーバーのオンプレミス デプロイ。
- Microsoft Azure サブスクリプションにおける Azure Site Recovery Services の資格情報コンテナー。 
- レプリケート先が Azure である場合、Azure の仮想マシン準備状況評価ツールを VM で実行し、Azure VM と Azure Site Recovery Services に互換性があることを確認してください。


## Site Recovery を使用して保護を有効にする


### 仮想マシンの保護

Site Recovery でドメイン コントローラー/DNS 仮想マシンの保護を有効にします。 仮想マシンの種類 (Hyper-V または VMware) に応じて Site Recovery の設定を構成します。 クラッシュ整合性のレプリケーション間隔には 15 分をお勧めします。

###仮想マシンのネットワーク設定の構成

ドメイン コントローラー/DNS 仮想マシンに関して、フェールオーバー後、VM が適切なネットワークに接続されるように Site Recovery でネットワーク設定を構成します。 たとえば、Hyper-V の VM を Azure にレプリケートする場合、VMM クラウド内の VM または保護グループ内の VM を選択して、以下のようにネットワーク設定を構成することができます。

![VM ネットワークの設定](./media/site-recovery-active-directory/VM-Network-Settings.png)

## Active Directory レプリケーションで Active Directory を保護する 

### サイト間の保護

セカンダリ サイトにドメイン コントローラーを作成し、そのサーバーの役割をドメイン コントローラーに昇格させるときに、プライマリ サイト側と同じドメイン名を指定します。 使用することができます、 **Active Directory サイトとサービス** スナップインで、サイトの追加サイト リンク オブジェクトで設定を構成します。 サイト リンクで設定を構成することで、2 つまたはそれ以上のサイト間でレプリケーションを発生させる時間と頻度を制御できます。 参照してください [サイト間レプリケーションのスケジュール設定](https://technet.microsoft.com/library/cc731862.aspx) 詳細です。

###サイトと Azure 間の保護

指示に従って、 [Azure の仮想ネットワークでドメイン コント ローラーを作成](../virtual-network/virtual-networks-install-replica-active-directory-domain-controller.md)します。 ドメイン コント ローラーの役割をサーバーに昇格するときに、プライマリ サイトで使用されている同じドメイン名を指定します。

 [仮想ネットワークの DNS サーバーを再構成](../virtual-network/virtual-networks-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), 、Azure で DNS サーバーを使用します。
  
![Azure ネットワーク](./media/site-recovery-active-directory/azure-network.png)

## テスト フェールオーバーの考慮事項

テスト フェールオーバーは、運用環境のワークロードに影響が生じないよう、運用ネットワークから分離されたネットワークで行われます。 

また、ほとんどのアプリケーションは、ドメイン コントローラーと DNS サーバーがないと正常に機能しません。そのため、アプリケーションのフェールオーバーを実行する前に、テスト フェールオーバー用の分離されたネットワーク内にドメイン コントローラーを作成しておく必要があります。 最も簡単な方法としては、ドメイン コントローラー/DNS 仮想マシンに対する保護を Site Recovery で有効にし、その仮想マシンのテスト フェールオーバーを実行した後で、アプリケーション用の復旧計画のテスト フェールオーバーを実行します。 その方法は次のとおりです。

1. Site Recovery でドメイン コントローラー/DNS 仮想マシンの保護を有効にします。
2. 分離されたネットワークを作成します。 既定では、Azure で作成されるすべての仮想ネットワークは、その他のネットワークから分離します。 このネットワークの IP アドレス範囲は、運用ネットワークと同じものを使用することをお勧めします。 このネットワーク上でサイト間接続を有効化しないでください。
3. DNS の仮想マシンを取得すると予想される IP アドレスとして作成し、ネットワーク上の DNS の IP アドレスを提供します。 Azure にレプリケートする場合はフェールオーバーで使用される VM の IP アドレスを指定して **ターゲット IP** で VM のプロパティを設定します。 別の内部設置型サイトにレプリケートして、表示される指示に DHCP のフォローを使用している場合 [テスト フェールオーバー用の DNS と DHCP のセットアップ](site-recovery-failover.md#prepare-dhcp) 

>[AZURE.NOTE] テスト フェールオーバー中に仮想マシンに割り当てられた IP アドレスは、IP アドレスが、テスト フェールオーバー用ネットワークで使用可能な場合にアドレスが取得、計画または計画外のフェールオーバー時に IP アドレスと同じです。 されていない場合は、仮想マシンは、テスト フェールオーバー用ネットワークで使用できる別の IP アドレスを受け取ります。 

4. ドメイン コントローラーの仮想マシンで、分離されたネットワークでテスト フェールオーバーを実行します。 
5. アプリケーション復旧計画のテスト フェールオーバーを実行します。
6. テストが完了した後は、ドメイン コント ローラー仮想マシンのジョブと、復旧計画のテスト フェールオーバーをマーク ' Complete'on、 **ジョブ** Site Recovery ポータルでタブをクリックします。 

### DNS とドメイン コントローラーが異なるマシン上に存在する場合
 
DNS がドメイン コントローラーと同じ仮想マシン上にない場合、テスト フェールオーバー用の DNS VM を作成する必要があります。 両者が同じ VM 上にある場合は、このセクションを省略できます。 

新規の DNS サーバーを使用し、必要なすべてのゾーンを作成することができます。 たとえば、Active Directory ドメインが contoso.com である場合には、contoso.com という名前で DNS ゾーンを作成することができます。 Active Directory に対応するエントリは DNS で次のように更新する必要があります。

1. 復旧計画内の他のすべての仮想マシンが起動する前に、次の設定が有効になっているようにします。

    - ゾーンは、フォレスト ルート名に従って名前を付ける必要があります。
    - ゾーンは、ファイルに格納される必要があります。
    - ゾーンは、セキュリティ保護された更新とセキュリティ保護されていない更新の両方に対して有効である必要があります。
    - ドメイン コントローラーの仮想マシンのリゾルバーは、DNS の仮想マシンの IP アドレスをポイントする必要があります。

2. ドメイン コントローラーの仮想マシンのディレクトリで、次のコマンドを実行します。

    `nltest /dsregdns`

3. DNS サーバー上にゾーンを追加し、セキュリティで保護されていない更新を許可し、その DNS サーバー用のエントリを DNS に追加します。

        dnscmd /zoneadd contoso.com  /Primary 
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1 
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM> 
        dnscmd /config contoso.com /allowupdate 1


## 次のステップ

読み取り [どのようなワークロードで保護できますか。](../site-recovery/site-recovery-workload.md) Azure Site Recovery によるエンタープライズ ワークロードの保護の詳細を表示します。



