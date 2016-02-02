<properties 
   pageTitle="ExpressRoute 回線とルーティング ドメイン | Microsoft Azure"
   description="このページでは、ExpressRoute 回線とルーティング ドメインの概要を示します。"
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carolz"
   editor=""/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/22/2015"
   ms.author="cherylmc"/>


# ExpressRoute 回線とルーティング ドメイン

 接続プロバイダーを経由してオンプレミス インフラストラクチャを Microsoft に接続するには、*ExpressRoute 回線*を注文する必要があります。 下の図は、お客様の WAN と Microsoft の接続を論理的に表現したものです。

![](./media/expressroute-circuit-peerings/expressroute-basic.png)

## ExpressRoute 回線

*ExpressRoute 回線*は、接続プロバイダー経由のオンプレミス インフラストラクチャと Microsoft クラウド サービス間の論理接続を表します。 複数の ExpressRoute 回線を注文することができます。 回線をそれぞれ同じリージョンや異なるリージョンに配置したり、異なる接続プロバイダーを経由して社内に接続したりすることができます。

ExpressRoute 回線は物理エンティティにはマップされません。 回線は、サービス キー (s キー) という標準 GUID によって一意に識別されます。 サービス キーは、Microsoft、接続プロバイダー、およびユーザー間で交換される情報のほんの一部です。 s キーは、セキュリティ目的のシークレットではありません。 ExpressRoute 回線と s キーは 1 対 1 で対応しています。

ExpressRoute 回線では、Azure パブリック、Azure プライベート、および Microsoft という最大 3 つの独立したピアリングを使用することができます。各ピアリングは独立した BGP セッションのペアで、高可用性を実現するためにセッションはそれぞれ冗長的に構成されています。ExpressRoute 回線とルーティング ドメインは 1 対 N (1 <= N <= 3) で対応しています。ExpressRoute 回線 は、回線ごとに 1 つ、2 つ、または 3 つすべてのピアリングを使用することができます。

各接続には固定帯域幅 (50 Mbps、100 Mbps、200 Mbps、500 Mbps、1 Gbps、10 Gbps) があり、接続プロバイダーとピアリング場所にマップされます。 選択する帯域幅は、この回線のすべてのピアリング間で共有されます。

### クォータ、制限、および制限事項

既定のクォータと制限は、すべての ExpressRoute 回線に適用されます。 参照してください、 [Azure サブスクリプションとサービスの制限、クォータ、および制約](../articles/azure-subscription-service-limits.md) のクォータの最新の状態に関する情報のページです。

## ExpressRoute のルーティング ドメイン

ExpressRoute 回線には、複数のルーティング ドメイン (Azure パブリック、Azure プライベート、Microsoft) が関連付けられます。 各ルーティング ドメインは、高可用性を実現するために、ルーターのペアに同じ方法 (アクティブ/アクティブ構成または負荷分散構成) で構成されます。 Azure サービスは *Azure パブリック*および *Azure プライベート*として分類され、IP アドレス指定スキームを表します。


![](./media/expressroute-circuit-peerings/expressroute-peerings.png)


### プライベート ピアリング

Azure Compute Services、つまり、仮想ネットワーク内にデプロイされる仮想マシン (IaaS) とクラウド サービス (PaaS) には、プライベート ピアリング ドメイン経由で接続できます。 プライベート ピアリング ドメインは、お客様のコア ネットワークを Microsoft Azure に信頼できる方法で拡張したものと言えます。 コア ネットワークと Azure Virtual Network (VNET) の間に双方向接続を設定できます。 これにより、仮想マシンとクラウド サービスにプライベート IP アドレスで直接接続できます。

プライベート ピアリング ドメインには複数の仮想ネットワークを接続できます。 レビュー、 [FAQ のページ](expressroute-faqs.md) 制限および制約事項についてです。 お客様がアクセスできる、 [Azure サブスクリプションとサービスの制限、クォータ、および制約](../articles/azure-subscription-service-limits.md) 制限に関する最新情報についてのページです。 参照してください、 [ルーティング](expressroute-routing.md) ルーティング構成の詳細についてはページです。

### パブリック ピアリング

Azure Storage、SQL Database、Websites などのサービスは、パブリック IP アドレスで提供されます。 パブリック ピアリング ルーティング ドメインを経由して、(クラウド サービスの VIP などの) パブリック IP アドレスでホストされているサービスにプライベート接続できます。 パブリック ピアリング ドメインをご使用の DMZ に接続すれば、インターネット経由で接続しなくても WAN からパブリック IP アドレス上のすべての Azure サービスに接続できます。

接続は、常に WAN から Microsoft Azure サービスへ開始されます。 このルーティング ドメインを経由して Microsoft Azure サービスからお客様のネットワークに接続を開始することはできません。 パブリック ピアリングを有効になると、すべての Azure サービスに接続できるようになります。 ルートをアドバタイズするサービスを選択することはできません。 このピアリングを経由してアドバタイズされるプレフィックスの一覧を確認することができます、 [Microsoft Azure データ センターの IP 範囲](http://www.microsoft.com/download/details.aspx?id=41653) ページです。 このページは毎週更新されます。

ネットワーク内でカスタム ルート フィルターを定義して、必要なルートのみを使用することができます。 参照してください、  [ルーティング](expressroute-routing.md) ルーティング構成の詳細についてはページです。 ネットワーク内でカスタム ルート フィルターを定義して、必要なルートのみを使用することができます。

参照してください、 [FAQ のページ](expressroute-faqs.md) パブリック ピアリング ルーティング ドメインでサポートされるサービスの詳細についてです。

### Microsoft ピアリング

上記以外の Microsoft Online Services (Office 365 サービスなど) への接続はすべて Microsoft ピアリングを経由します。 Microsoft ピアリング ルーティング ドメイン経由で、ご使用の WAN と Microsoft クラウド サービスの双方向接続を実現できます。 お客様または接続プロバイダーが所有するパブリック IP アドレスのみを使用して Microsoft クラウド サービスに接続する必要があり、定義されてするすべての規則を遵守する必要があります。 参照してください、 [ExpressRoute の前提条件](expressroute-prerequisites.md) 詳細については、ページです。

参照してください、 [FAQ のページ](expressroute-faqs.md) の詳細についてはサポートされているサービス、コスト、および構成の詳細。 参照してください、 [ExpressRoute の場所](expressroute-locations.md) Microsoft ピアリングのサポートを提供している接続プロバイダーの一覧に関する詳細についてのページです。

## ルーティング ドメインの比較

次の表は、3 つのルーティング ドメインを比較しています。

| | **プライベート ピアリング**| **パブリック ピアリング**| **Microsoft ピアリング**|
|---|---|---|---|
| * * Max です。# プレフィックス ピアリングごとにサポートされる * *| 既定では、ExpressRoute Premium 10,000 4000| 200| 200|
| **サポートされている IP アドレスの範囲**| お客様の WAN 内で有効な IPv4 アドレスです。| パブリック IPv4 アドレスが、または接続プロバイダーが所有します。| パブリック IPv4 アドレスが、または接続プロバイダーが所有します。|
| **番号の要件**| プライベートおよびパブリックの AS 番号。お客様はパブリックで所有する必要があります数。| プライベートおよびパブリックの AS 番号。お客様はパブリックで所有する必要があります数。| パブリック AS 番号のみです。数は、所有権を検証するルーティング レジストリに対して検証する必要があります。|
| **ルーティング インターフェイスの IP アドレス**| RFC1918 およびパブリック IP アドレスします。| パブリック IP アドレスがルーティング レジストリでお客様に登録されています。| パブリック IP アドレスがルーティング レジストリでお客様に登録されています。|
| **MD5 ハッシュのサポート**| はい| あり| あり|

ExpressRoute 回線の一部として 1 つ以上のルーティング ドメインを有効にするように選択できます。 すべてのルーティング ドメインを 1 つのルーティング ドメインに取り込みたい場合は、同じ VPN 上に配置するように選択できます。 また、上図に似た別のルーティング ドメイン上に配置することもできます。 推奨構成は、プライベート ピアリングをコア ネットワークに直接接続し、パブリック ピアリングと Microsoft ピアリングのリンクを DMZ に接続する構成です。

3 つのピアリング セッションすべてを使用する場合は、BGP セッションのペアを 3 つ (ピアリングの種類ごとに 1 つのペア) 設定する必要があります。 BGP セッションのペアによって、高可用性リンクが実現されます。 レイヤー 2 接続プロバイダーを経由して接続している場合、ルーティングの構成と管理はお客様の責任となります。 詳細を確認して、 [ワークフロー](expressroute-workflows.md) ExpressRoute を設定するためです。

## 次のステップ

- サービス プロバイダーを検索します。 参照してください [ExpressRoute サービス プロバイダーとロケーション](expressroute-locations.md)します。
- すべての前提条件を満たしていることを確認します。 参照してください [ExpressRoute の前提条件](expressroute-prerequisites.md)します。
- ExpressRoute 接続を構成します。
    - [ExpressRoute 回線を作成します。](expressroute-howto-circuit-classic.md)
    - [ルーティング (回路のピアリング) を構成します。](expressroute-howto-routing-classic.md)
    - [VNet を ExpressRoute 回線にリンクします。](expressroute-howto-linkvnet-classic.md)




