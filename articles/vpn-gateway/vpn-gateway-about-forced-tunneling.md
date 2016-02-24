<properties 
   pageTitle ="強制トンネリングの PowerShell を使用する VPN ゲートウェイの構成 |Microsoft Azure]
   description ="ではクロスプレミス VPN ゲートウェイを含む仮想ネットワークがあれば、リダイレクトしたり、内部設置型の場所にすべてのインターネットにバインドされたトラフィックを「強制」できます。 この記事では、従来のデプロイ モデルを使用して作成された VPN ゲートウェイには適用されます"
   サービス"vpn gateway"=
   documentationCenter ="na"
   作成者"cherylmc"=
   manager ="carolz"
   エディター =""
   タグ =「azure サービス管理」/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/21/2015"
   ms.author="cherylmc" />

# 強制トンネリングについて

> [AZURE.SELECTOR]
- [PowerShell - サービスの管理](vpn-gateway-about-forced-tunneling.md)
- [リソース マネージャーの PowerShell](vpn-gateway-forced-tunneling-rm.md)

この記事は、クラシック デプロイメント モデル (別名、サービス管理) を使用して作成された VNet および VPN ゲートウェイを対象としています。 Vnet の強制トンネリングを構成するし、VPN ゲートウェイの作成、リソース マネージャーの配置モデルを使用して表示 [強制トンネリング PowerShell と Azure リソース マネージャーを使用して構成](vpn-gateway-forced-tunneling-rm.md)します。 

[AZURE.INCLUDE [vpn-gateway-sm-rm](../../includes/vpn-gateway-sm-rm-include.md)] 

## 強制トンネリングについて

強制トンネリングによって、検査および監査には、サイト間の VPN トンネルを介して、インターネットへのすべてのトラフィックをオンプレミス ロケーションにリダイレクトまたは "強制" できます。 これは、ほとんどの企業 IT ポリシーの重要なセキュリティ要件です。 強制トンネリングを使用しない場合、Azure の VM からインターネットへのトラフィックは、トラフィックを検査または監査できるオプションを使用せずに、常に Azure ネットワーク インフラストラクチャからインターネットへ直接トラバースします。 認証されていないインターネット アクセスは、情報の漏えいまたは他の種類のセキュリティ侵害を招く可能性があります。

以下の図には、強制トンネリングのしくみを示しています。

![強制トンネリング](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

上記の例では、フロントエンドのサブネットは、トンネリングを強制されません。 フロントエンドのサブネット内のワークロードは、直接、インターネットから顧客の要求を承認し応答し続けることができます。 Mid-tier およびMid-tier のサブネットは、トンネリングを強制されます。 これら 2 つのサブネットからのインターネットへのオウトバウンド接続は、S2S VPN トンネルの 1 つを介して、オンプレミス サイトに ”強制” リダイレクトされます。 これにより、必要な多層サービス アーキテクチャが継続的に使用可能になっている間は、Azure Virtual Machines やクラウド サービスからのインターネット アクセスを制限および検査することができます。 また、仮想ネットワーク内に、インターネットに接続されたワークロードがない場合、全体の仮想ネットワークに強制トンネリングを適用するオプションがあります。

## 要件と考慮事項

Azure では、強制トンネリングは仮想ネットワークのユーザー定義ルートを使用して構成されます。 オンプレミス サイトへのトラフィックのリダイレクトは、Azure VPN Gateway への既定のルートとして表されます。 以下のセクションでは、Azure Virtual Network のルーティング テーブルおよびルートの現在の制限を一覧表示します。



-  各仮想ネットワーク サブネットには、システム ルーティング テーブルが組み込まれています。 システム ルーティング テーブルには、次の 3 つのグループがあります。

    - **ローカル VNet ルーティング:** 同じ仮想ネットワーク内の宛先 Vm へ直接
    
    - **オンプレミス ルート:** Azure VPN ゲートウェイへ
    
    - **既定のルート:** インターネットに直接します。 前の 2 つのルートが網羅していないプライベート IP アドレスへ宛先送信されるパケットは削除されることに注意してください。



-  ユーザー定義ルートをリリースすることにより、既定のルートを追加するルーティング テーブルを作成し、そのルーティング テーブルを、ご使用の VNet サブネットに関連付け、それらのサブネットでの強制トンネリングを有効にします。

- 強制トンネリングは、動的ルーティング VPN ゲートウェイ (静的ゲートウェイではない) を持つ VNet に関連付ける必要があります。 仮想ネットワークに接続されたクロスプレミス ローカル サイト間で「既定のサイト」を設定する必要があります。

- ExpressRoute の強制トンネリングは、このメカニズムを介して構成されていませんが、代わりに ExpressRoute BGP ピアリング セッションを介して既定ルートを告知することで有効になっています。 参照してください、 [ExpressRoute に関するドキュメント](https://azure.microsoft.com/documentation/services/expressroute/) の詳細。

## 構成の概要

以下の手順は、仮想ネットワークで強制トンネリングを指定するのに役立ちます。 構成手順は、以下に示す仮想ネットワークの netcfg ファイルの例に対応します。 

例では、仮想ネットワーク"Multitier-vnet"が 3 つのサブネット: *フロント エンド*, 、*Midtier*, と *バックエンド* サブネット、4 つのクロスプレミス接続: *DefaultSiteHQ*, 、および 3 *分岐*します。 手順は、設定、 *DefaultSiteHQ* の既定サイト接続として強制トンネリング、および構成、 *Midtier* と *バックエンド* を使用するサブネットは強制トンネリングします。

    <VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>

### 前提条件

- Azure サブスクリプション

- 構成済みの仮想ネットワーク。 

- Web Platform Installer を使用した、Azure PowerShell コマンドレットの最新バージョン。 ダウンロードしてからの最新バージョンをインストール、 **Windows PowerShell** のセクションで、 [ダウンロード ページ](http://azure.microsoft.com/downloads/)します。

## 強制トンネリングについて

強制トンネリングを構成するには、以下の手順を使用します。

1. ルーティング テーブルを作成します。 ルート テーブルを作成するには、以下のコマンドレットを使用します。

        New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"

1. 既定のルートをルーティング テーブルに追加します。 

    次のコマンドレット例は、手順 1. で作成された、ルーティング テーブルに既定のルートを追加します。 サポートされている唯一のルートには、"VPNGateway" ネクストホップへの宛先プレフィックスの「0.0.0.0/0」が付いています。
 
        Set-AzureRoute –RouteTableName "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway

1. サブネットには、ルーティング テーブルを関連付けます。 

    ルーティング テーブルが作成され、ルートが追加された後は、以下のコマンドレットを使用して、ルート テーブルを VNet サブネットに追加または関連付けます。 以下のサンプルでは、ルート テーブル "MyRouteTable" を VNet MultiTier-VNet の Midtier および Backend サブネットに追加します。

        Set-AzureSubnetRouteTable -VNetName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"

        Set-AzureSubnetRouteTable -VNetName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"

1. 強制トンネリングに既定のサイトを割り当てます。 

    前の手順で、サンプルのコマンドレット スクリプトが、ルーティング テーブルを作成して、ルート テーブルを 2 つの VNet のサブネットに関連付けました。 残りの手順は、仮想ネットワークの複数サイト間接続でローカルのサイトを規定のサイトまたはトンネルとして選択します。

        $DefaultSite = @("DefaultSiteHQ")
        Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"

## その他の PowerShell コマンドレット

強制トンネリングの構成に役立つその他の PowerShell コマンドレットを以下に示します。

**ルート テーブルを削除するには:**

    Remove-AzureRouteTable -RouteTableName <routeTableName>

**ルート テーブルを一覧表示するには:**

    Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]

**ルート テーブルからルートを削除するには:**

    Remove-AzureRouteTable –Name <routeTableName>

**サブネットからのルートを削除するには:**

    Remove-AzureSubnetRouteTable –VNetName <virtualNetworkName> -SubnetName <subnetName>

**サブネットに関連付けられたルート テーブルの一覧表示するには:**
    
    Get-AzureSubnetRouteTable -VNetName <virtualNetworkName> -SubnetName <subnetName>

**VNet VPN ゲートウェイから既定のサイトを削除するには:**

    Remove-AzureVnetGatewayDefaultSites -VNetName <virtualNetworkName>







