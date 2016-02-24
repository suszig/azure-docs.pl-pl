<properties 
   pageTitle="ExpressRoute の概要 | Microsoft Azure"
   description="このページでは、ExpressRoute 接続が機能するしくみをなど、ExpressRoute サービスの概要を提供します。"
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

# ExpressRoute の技術概要

Microsoft Azure ExpressRoute を利用すれば、接続プロバイダーが提供する専用プライベート接続で、オンプレミスのネットワークを Microsoft クラウドに拡張できます。 ExpressRoute では、Microsoft Azure、Office 365、CRM Online などの Microsoft クラウド サービスへの接続を確立できます。 接続には、任意の環境間 (IP VPN) 接続、ポイントツーポイントのイーサネット接続、共有施設での接続プロバイダーによる仮想交差接続があります。 ExpressRoute 接続では、公共のインターネットを利用できません。 それにより、ExpressRoute 接続はインターネット経由の一般的な接続に比べて、安全性と信頼性が高く、待機時間も短く、高速です。  

![](./media/expressroute-introduction/expressroute-basic.png)

**主な利点:**

- 接続プロバイダーによるオンプレミス ネットワークと Microsoft Cloud 間のレイヤー 3 接続。 接続には、任意の環境間 (IP VPN) 接続、ポイント ツー ポイントのイーサネット接続、イーサネット交換による仮想交差接続があります。
- 地政学的領域のすべての地域にわたる Microsoft のクラウド サービスへの接続。
- ExpressRoute プレミアム アドオンを持つすべての領域にわたる Microsoft サービスへのグローバル接続。
- 業界標準プロトコル (BGP) でのネットワークと Microsoft 間の動的ルーティング。
- あらゆるピアリングの場所に冗長性を組み込むという高い信頼性。
- 接続の稼働時間 [SLA](http://azure.microsoft.com/support/legal/sla/)します。
- QoS と、Skype for Business など、特別なアプリケーションのためのサービスの複数のクラス。

参照してください、 [ExpressRoute の FAQ](expressroute-faqs.md) 詳細です。

## ExpressRoute でネットワークを Microsoft に接続するにはどうすればよいですか。

次の 3 つの方法でオンプレミス ネットワークと Microsoft クラウドの間に接続を構築できます。

1. **クラウド エクス チェンジで配置されます。**クラウドの Exchange がある施設に同一場所配置している場合、併置プロバイダーのイーサネット交換経由で Microsoft クラウドに仮想交差接続を要請できます。 併置プロバイダーは、共有施設のインフラストラクチャと Microsoft クラウドの間に、レイヤー 2 交差接続と管理レイヤー 3 交差接続のいずれかを提供します。
2.  **ポイント ツー ポイントのイーサネット接続します。**オンプレミス データセンター/オフィスと Microsoft クラウドをポイント ツー ポイントのイーサネット リンクで接続できます。 ポイント ツー ポイントのイーサネットのプロバイダーは、サイトと Microsoft クラウドの間にレイヤー 2 接続と管理レイヤー 3 接続のいずれかを提供できます。
3.  **任意のストレージ (IPVPN) ネットワーク。**WAN と Microsoft クラウドを統合できます。 IPVPN プロバイダー (通常、MPLS VPN) は、ブランチ オフィスとデータセンターの間に任意の環境間の接続を提供できます。 Microsoft クラウドを WAN に相互接続し、ブランチ オフィスのように見せることができます。 通常、WAN プロバイダーは管理レイヤー 3 接続を提供します。

![](./media/expressroute-introduction/expressroute-connectivitymodels.png)

ExpressRoute の機能は上記の接続モデルのすべてに共通しています。 接続プロバイダーでは、上記のリストから 1 つまたは複数の接続モデルを提供できます。 最適なモデルは接続プロバイダーに相談の上でお選びください。

## ExpressRoute の機能

ExpressRoute は次のような機能をサポートしています。 

### レイヤー 3 接続

Microsoft は業界標準の動的ルーティング プロトコル (BGP) を利用し、オンプレミス ネットワーク、Azure のインスタンス、および Microsoft パブリック アドレスの間のルートを交換します。  さまざまなトラフィック プロファイルに合わせ、ネットワークとさまざまな BGP セッションを確立します。 詳細については記載されて、 [ExpressRoute 回線とルーティング ドメイン](expressroute-circuit-peerings.md) 記事です。

### 冗長性

各 ExpressRoute 回線は、接続プロバイダー/ネットワーク エッジから 2 つの Microsoft Enterprise エッジ ルーター (WSEE) への 2 つの接続で構成されます。 Microsoft は接続プロバイダー/あなたの側から 2 つの BGP 接続を必要とします。MSEE につき 1 つです。 あなたの側で冗長デバイス/イーサネット回線をデプロイしないことを選択できます。 ただし、接続プロバイダーは冗長デバイスを利用することで冗長性が与えられる方法で接続を Microsoft に渡します。 冗長なレイヤー 3 接続構成の要件とは、 [SLA](http://azure.microsoft.com/support/legal/sla/) を有効にします。 

### Microsoft クラウド サービスへの接続

ExpressRoute 接続は次のサービスへのアクセスを有効にします。

- Microsoft Azure サービス
- Microsoft Office 365 サービス
- Microsoft CRM Online サービス 
 
お客様がアクセスできる、 [ExpressRoute の FAQ](expressroute-faqs.md) ExpressRoute でサポートされているサービスの詳細な一覧のページです。

### 地政学的な地域内のすべての地域への接続

いずれかで Microsoft に接続できる、 [ピアリングの場所](expressroute-locations.md) と地理的リージョン内のすべての領域にアクセスします。 

たとえば、ExpressRoute を介してアムステルダムの Microsoft に接続している場合、北ヨーロッパと西ヨーロッパでホストされているすべての Microsoft クラウド サービスにアクセスできます。 参照してください、 [ExpressRoute パートナーとピアリングの場所](expressroute-locations.md) 地理的リージョン、関連付けられている Microsoft クラウド領域、および対応する ExpressRoute のピアリング場所の概要についての記事です。

### ExpressRoute プレミアム アドオンによるグローバル接続

ExpressRoute プレミアム アドオン機能を有効にすると、地政学的境界を越えて接続を拡張できます。 たとえば、ExpressRoute を介してアムステルダムの Microsoft に接続している場合、世界中のあらゆる領域でホストされているすべての Microsoft クラウド サービスにアクセスできます (国内のクラウドは除外します)。 北ヨーロッパ地域と西ヨーロッパ地域にアクセスする場合と同じ方法で南アメリカやオーストラリアにデプロイされているサービスにアクセスできます。

### 大勢の接続パートナーが属するエコシステム

ExpressRoute のエコシステムには継続的に新しい接続パートナーと SI パートナーが加わり、拡大します。 参照することができます、 [ExpressRoute プロバイダーとロケーション](expressroute-locations.md) 最新の情報の記事です。

### 国内クラウドへの接続

Microsoft は特殊な地政学的領域と顧客区分のために隔離されたクラウド環境を運営しています。 参照してください、 [ExpressRoute プロバイダーとロケーション](expressroute-locations.md) national クラウドとプロバイダーの一覧のページです。

### サポートされる帯域幅のオプション

さまざまな帯域幅に合った ExpressRoute 回線を購入できます。 サポートされる帯域幅の一覧は次のとおりです。 サポートされる帯域幅の一覧は接続パートナーに必ず確認してください。

- 50 Mbps
- 100 Mbps
- 200 Mbps
- 500 Mbps
- 1 Gbps
- 2 Gbps
- 5 Gbps
- 10 Gbps

### 帯域幅の動的スケーリング

接続を壊さずに (ベスト エフォート基準で) ExpressRoute 回線の帯域幅を増やすことができます。 

### 柔軟な課金モデル

最適な請求モデルを選択できます。 次に示す請求モデルから選択します。 参照してください、 [ExpressRoute の FAQ](expressroute-faqs.md) 詳細についてはページです。 

- **無制限のデータ**します。 ExpressRoute 回線は月額料金に基づいて課金されます。すべての受信または送信データ転送が無料で含まれます。 
- **データの使用状況を測定**します。 ExpressRoute 回線は月額料金に基づいて課金されます。 すべての受信データ転送は無料で含まれます。 送信データ転送は 1 GB のデータ転送単位で課金されます。 データ転送料金は地域によって異なります。
- **ExpressRoute premium アドオン**します。 ExpressRoute プレミアムは ExpressRoute 回線のアドオンです。 ExpressRoute プレミアム アドオンには次の機能があります。 
    - Azure パブリックと Azure プライベートのピアリングのルート上限の増加 (4,000 ルートから 10,000 ルートに)。
    - サービスのグローバル接続。 (国内のクラウドを除く) あらゆる地域で作成された ExpressRoute 回線に与えられる世界中の他の地域にあるリソースへのアクセス。 たとえば、西ヨーロッパで作成された仮想ネットワークにシリコン バレーでプロビジョニングされた ExpressRoute 回線からアクセスできます。
    - ExpressRoute ごとの VNet リンクの増加 (回線の帯域幅にもよりますが、10 からそれ以上に)。

## 次のステップ

- ExpressRoute 接続とルーティング ドメインについて説明します。 参照してください [ExpressRoute 回路とルーティング ドメイン](expressroute-circuit-peerings.md)します。
- サービス プロバイダーを検索します。 参照してください [ExpressRoute パートナーとピアリングの場所](expressroute-locations.md)します。
- すべての前提条件を満たしていることを確認します。 参照してください [ExpressRoute の前提条件](expressroute-prerequisites.md)します。
- 要件を参照して [ルーティング](expressroute-routing.md), 、[NAT](expressroute-nat.md) と [QoS](expressroute-qos.md)します。
- ExpressRoute 接続を構成します。
    - [ExpressRoute 回線の作成](expressroute-howto-circuit-classic.md)
    - [ルーティングの構成](expressroute-howto-routing-classic.md)
    - [ExpressRoute 回線への VNet のリンク](expressroute-howto-linkvnet-classic.md)

