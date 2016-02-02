<properties 
   pageTitle="仮想ネットワークのセキュリティで保護されたクロスプレミス接続について | Microsoft Azure"
   description="サイト間、ポイント対サイト、ExpressRoute 接続などの仮想ネットワークのセキュリティで保護されたクロスプレミス接続の種類について理解する。"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carolz"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/01/2015"
   ms.author="cherylmc" />


# 仮想ネットワークのセキュリティで保護されたクロスプレミス接続について

オンプレミスのサイトを仮想ネットワークに安全に接続するには、サイト間、ポイント対サイト、および ExpressRoute の 3 つのオプションがあります。

選択するオプションは、次のようなさまざまな考慮事項によって異なります。


- ソリューションにどのようなスループットが必要か。
- セキュリティで保護された VPN 経由のパブリックなインターネットで通信するか、またはプライベートな接続で通信するか。
- 使用できるパブリック IP アドレスを持っているか。
- VPN デバイスを使用する予定があるか。 その場合、互換性があるか。
- 接続するコンピューターは数台か、サイトとの永続的な接続が必要か。
- 作成するソリューションにはどのような種類の VPN Gateway が必要か。

次の表は、ソリューションに最適な接続オプションを決定するのに役立ちます。


| -| **ポイント対サイト**| **サイト間**| **ExpressRoute**|
|------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **Azure には、サービスがサポートされています。**| クラウド サービスおよび仮想マシン| クラウド サービスおよび仮想マシン| [サービスの一覧](../expressroute/expressroute-faqs.md#supported-services)|
| **一般的な帯域幅**| 通常 < 100 Mbps の集計| 通常 < 100 Mbps の集計| 50 Mbps、100 Mbps、200 Mbps、500 Mbps、1 Gbps、2 Gbps、5 Gbps、10 Gbps|
| **サポートされるプロトコル**| セキュリティで保護されたソケットのトンネリング プロトコル (SSTP)| IPsec| NSP の VPN テクノロジ (MPLS、VPLS...) の Vlan 経由で直接接続|
| **ルーティング**| ルート ベース (動的)| ポリシー ベース (静的ルーティング)、およびルート ベース (動的ルーティング VPN) サポートします。| BGP|
| **接続の回復**| アクティブ/パッシブ| アクティブ/パッシブ| アクティブ/アクティブ|
| **一般的なユース ケース**| プロトタイプの作成、開発/テスト/クラウド サービスおよび仮想マシンをラボのシナリオ| 開発/テスト/ラボ シナリオや小規模クラウド サービスおよび仮想マシンの実稼働ワークロードを小数点以下桁数。| すべての Azure サービス (検証済みリスト)、エンタープライズ クラスとバックアップ、ビッグ データ、Azure を DR サイトとしてミッション クリティカルなワークロードへのアクセス|
| **SLA**| [SLA](https://azure.microsoft.com/support/legal/sla/)| [SLA](https://azure.microsoft.com/support/legal/sla/)| [SLA](https://azure.microsoft.com/support/legal/sla/)|
| **料金**| [価格設定](http://azure.microsoft.com/pricing/details/vpn-gateway/)| [価格設定](http://azure.microsoft.com/pricing/details/vpn-gateway/)| [価格設定](http://azure.microsoft.com/pricing/details/expressroute/)|
| **テクニカル ドキュメント**| [VPN ゲートウェイのドキュメント](https://azure.microsoft.com/documentation/services/vpn-gateway/)| [VPN ゲートウェイのドキュメント](https://azure.microsoft.com/documentation/services/vpn-gateway/)| [ExpressRoute に関するドキュメント](https://azure.microsoft.com/documentation/services/expressroute/)|
| ** FAQ **| [VPN Gateway に関する FAQ](vpn-gateway-vpn-faq.md)| [VPN Gateway に関する FAQ](vpn-gateway-vpn-faq.md)| [ExpressRoute の FAQ](../expressroute/expressroute-faqs.md)|


## サイト間接続

サイト間 VPN では、オンプレミスのサイトと仮想ネットワークの間にセキュリティで保護された接続を作成することができます。 サイト間接続を作成するには、オンプレミスのネットワークに配置されている VPN デバイスを、Azure VPN Gateway を使用したセキュリティで保護された接続を作成するように構成します。 接続が作成されると、ローカル ネットワーク上のリソースと仮想ネットワーク内のリソースが直接かつ安全に通信できるようになります。 サイト間接続では、仮想ネットワーク内のリソースにアクセスするために、ローカル ネットワーク上のクライアント コンピューターごとに個別の接続を確立する必要はありません。

**サイト間接続を使用する状況:**

- ハイブリッドなソリューションを作成する場合。
- クライアント側の構成を必要としない、オンプレミスの場所と仮想ネットワークの間の接続が必要な場合。
- 永続的な接続が必要な場合。

**要件**

- オンプレミスの VPN デバイスには、インターネットに接続された IPv4 IP アドレスが必要です。 これは、NAT の内側にすることはできません。
- 互換性のある VPN デバイスが必要です。 参照してください [VPN デバイスについて](http://go.microsoft.com/fwlink/p/?LinkID=615099)します。
- 使用する VPN デバイスが、ソリューションに必要なゲートウェイの種類と互換する必要があります。 参照してください [VPN ゲートウェイについて](vpn-gateway-about-vpngateways.md)します。
- ゲートウェイの SKU は、集計スループットにも影響します。 参照してください [ゲートウェイの Sku](vpn-gateway-about-vpngateways.md#gateway-skus) の詳細。

Azure クラシック ポータルと従来のデプロイ モデルを使用して、サイト間 VPN ゲートウェイの接続を構成する方法の詳細については、次を参照してください。 [のサイト間 VPN 接続で仮想ネットワークの構成](vpn-gateway-site-to-site-create.md)します。 リソース マネージャーの配置モデルを使用して、サイト間 VPN の構成については、次を参照してください。 [のサイト間 VPN 接続で仮想ネットワークの作成](vpn-gateway-create-site-to-site-rm-powershell.md)します。


## ポイント対サイト接続

ポイント対サイト VPN では、仮想ネットワークにセキュリティで保護された接続を作成することもできます。 ポイント対サイト構成では、仮想ネットワークに接続する各クライアント コンピューターで、接続が個別に構成されます。 ポイント対サイト接続では、VPN デバイスは必要ありません。 この種類の接続では、各クライアント コンピューターにインストールする VPN クライアントを使用します。 VPN を確立するには、オンプレミスのクライアント コンピューターからの接続を手動で開始します。

ポイント対サイトの構成とサイト間の構成は同時に存在することができますが、ポイント対サイト接続は、サイト間接続とは異なり、同じ仮想ネットワークへの ExpressRoute 接続を同時に構成することはできません。

**ポイント対サイト接続を使用する状況:**

- 仮想ネットワークに接続するために構成するクライアントの数があまり多くない。

- リモートの場所から仮想ネットワークに接続する必要がある。 たとえば、コーヒー ショップや会議場から接続する場合など。

- サイト間の接続はあるが、リモートの場所から接続する必要があるクライアントが含まれている。

- サイト間接続の最小要件を満たす VPN デバイスにアクセスできない。

- インターネットに接続された IPv4 IP アドレスが VPN デバイスにない。

ポイント対サイト接続を構成する方法の詳細については、次を参照してください。 [仮想ネットワークへのポイント対サイト VPN 接続を構成する](vpn-gateway-point-to-site-create.md)します。

## ExpressRoute 接続

Azure ExpressRoute は、Azure のデータセンターとオンプレミスや共用環境にあるインフラストラクチャの間でプライベート接続を作成するサービスです。 ExpressRoute 接続はパブリックなインターネットを経由しないため、インターネット経由の一般的な接続に比べて、安全性と信頼性が高く、待機時間も短く、高速です。

オンプレミスと Azure の間のデータ転送に ExpressRoute 接続を使用することで、大きなコスト上のメリットが得られる場合もあります。 ExpressRoute では、ExpressRoute の場所 (Exchange プロバイダーの施設) にある Azure への接続を確立したり、ネットワーク サービス プロバイダーが提供する既存の WAN ネットワーク (MPLS VPN など) から Azure に直接接続したりできます。

ExpressRoute の詳細については、次を参照してください。、 [ExpressRoute の技術概要](../expressroute/expressroute-introduction.md)します。


## 次のステップ

参照してください、 [ExpressRoute の FAQ](../expressroute/expressroute-faqs.md) と [VPN ゲートウェイ FAQ](vpn-gateway-vpn-faq.md) の詳細。








