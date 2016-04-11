<properties
   pageTitle="Azure リソース マネージャーと PowerShell を使用してサイト間 VPN 接続で仮想ネットワークを作成する | Microsoft Azure"
   description="この記事では、リソース マネージャ モデルを使用して VNet を作成し、S2S VPN ゲートウェイ接続を使用してローカルのオンプレミス ネットワークにこれを接続する方法を説明します。 サイト間接続は、ハイブリッド構成に使用できます。 既存のローカル サイトの IP アドレスのプレフィックスを変更する追加の手順が含まれています。"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carolz"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/14/2015"
   ms.author="cherylmc"/>

# PowerShell を使用してサイト間 VPN 接続で仮想ネットワークを作成する

> [AZURE.SELECTOR]
- [Azure クラシック ポータル](vpn-gateway-site-to-site-create.md)
- [PowerShell - Resource Manager](vpn-gateway-create-site-to-site-rm-powershell.md)

この記事では、Azure リソース マネージャーのデプロイ モデルを使用して、仮想ネットワークと、オンプレミス ネットワークに対するサイト間 VPN 接続を作成する手順について説明します。 この構成に対して別のデプロイ モデルを探す場合は、上のタブで必要な記事を選択してください。 同時に、Vnet を接続するには、内部設置型の場所への接続を作成するのでを参照してください。 [VNet に接続を構成する](vpn-gateway-vnet-vnet-rm-ps.md)です。


**Azure のデプロイ モデルについて**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## 開始する前に

構成を開始する前に、以下がそろっていることを確認します。

- 互換性のある VPN デバイスおよびデバイスを構成できる人員。 参照してください [VPN デバイスについて](vpn-gateway-about-vpn-devices.md)します。 VPN デバイスの構成に詳しくない場合や、オンプレミス ネットワーク構成の IP アドレス範囲を把握していない場合は、詳細な情報を把握している担当者と協力して作業を行ってください。

- VPN デバイスの外部接続用パブリック IP アドレス。 この IP アドレスを NAT の内側に割り当てることはできません。
    
- Azure サブスクリプション。 アクティブ化する Azure サブスクリプションがない場合、 [MSDN サブスクライバーの特典](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) サインアップ、または、 [無料評価版](http://azure.microsoft.com/pricing/free-trial/)します。

## PowerShell モジュールのインストール

接続を構成するには、Azure リソース マネージャー PowerShell コマンドレットの最新版が必要です。

[AZURE.INCLUDE [vpn-gateway-ps-rm-howto](../../includes/vpn-gateway-ps-rm-howto-include.md)] 

## 1.サブスクリプションへの接続 


リソース マネージャー コマンドレットを使用するように PowerShell モードを切り替えてください。 詳細については、次を参照してください。 [リソース マネージャーで Windows PowerShell を使用して](../powershell-azure-resource-manager.md)します。

PowerShell コンソールを開き、アカウントに接続します。 接続するには、次のサンプルを参照してください。

            Login-AzureRmAccount

アカウントのサブスクリプションを確認します。

            Get-AzureRmSubscription 

使用するサブスクリプションを指定します。

            Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


## 2.仮想ネットワークとゲートウェイ サブネットを作成する

- 既にゲートウェイ サブネットを含む仮想ネットワークがある場合は、途中のレッスンできますに **手順 3. - ローカル サイトを追加**します。 
- 仮想ネットワークが既にある場合に、VNet にゲートウェイ サブネットを追加するを参照してください [VNet にゲートウェイ サブネットを追加](#gatewaysubnet)します。

### 仮想ネットワークとゲートウェイ サブネットを作成するには

仮想ネットワークとゲートウェイ サブネットを作成するには、次のサンプルを使用してください。 サンプル値は実際の値に変更してください。 

まず、リソース グループを作成します。

    
        New-AzureRmResourceGroup -Name testrg -Location 'West US'

次に、仮想ネットワークを作成します。 指定したアドレス空間がオンプレミス ネットワーク内に存在するあらゆるアドレス空間と重複していないことを確認します。

次の例は、という名前の仮想ネットワークを作成 *testvnet* と呼ばれる 2 つのサブネット *GatewaySubnet* と呼ばれる、もう一方と *Subnet1*します。 具体的にはという名前の 1 つのサブネットを作成することが重要 *GatewaySubnet*します。 別の名前にすると、接続の構成は失敗します。 

        $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
        $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'
        New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="gatewaysubnet"></a>VNet にゲートウェイ サブネットを追加するには (省略可能)

この手順は、前の手順で作成した VNet にゲートウェイ サブネットを追加する必要がある場合にのみ必要です。

既存の仮想ネットワークがあり、仮想ネットワークにゲートウェイ サブネットを追加する場合は、次のサンプルを使用してゲートウェイ サブネットを作成できます。 ゲートウェイ サブネットには、必ず 'GatewaySubnet' という名前を付けてください。 別の名前を付けてサブネットを作成しても、Azure はそれをゲートウェイ サブネットとして認識しません。

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet
        Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

次に、構成を設定します。 

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## 3.ローカル サイトを追加する

仮想ネットワークで、 *ローカル サイト* 一般に、内部設置型の場所を指します。 Azure から参照できるように、ローカル サイトに名前を付けます。 

また、ローカル サイトのアドレス空間のプレフィックスも指定します。 指定した IP アドレス プレフィックスは、Azure がローカル サイトに送信するトラフィックを特定するときに使用されます。 つまり、ローカル サイトに関連付ける各アドレス プレフィックスを指定する必要があります。 オンプレミス ネットワークが変わる場合でも、アドレス プレフィックスを簡単に更新できます。 

PowerShell の例を使用する場合は、以下の点に注意してください。
    
-  *GatewayIPAddress* 内部設置型 VPN デバイスの IP アドレスです。 VPN デバイスを NAT の内側に配置することはできません。 
-  *AddressPrefix* 、オンプレミス アドレス空間のです。

1 つのアドレス プレフィックスを含むローカル サイトを追加するには

        New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

複数のアドレス プレフィックスを含むローカル サイトを追加するには

        New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### ローカル サイト用の IP アドレスのプレフィックスを変更するには

ローカル サイト プレフィックスは変わる場合があります。 IP アドレス プレフィックスを変更する手順は、VPN ゲートウェイの接続を作成したかどうかによって異なります。 参照してください [ローカル サイトの変更の IP アドレスのプレフィックス](#to-modify-ip-address-prefixes-for-a-local-site)します。


## 4.ゲートウェイのパブリック IP アドレスを要求する

次に、Azure VNet VPN ゲートウェイに割り当てるパブリック IP アドレスを要求します。 これは VPN デバイスに割り当てられているものと同じ IP アドレスではなく、Azure VPN ゲートウェイに割り当てられている IP アドレスです。 使用する IP アドレスは指定できません。IP アドレスはゲートウェイに動的に割り当てられます。 ゲートウェイに接続するオンプレミス VPN デバイスを構成するときに、この IP アドレスを使用します。

次の PowerShell サンプルを使用してください。 このアドレスの割り当て方法は動的にする必要があります。 

        $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

## 5.ゲートウェイ IP アドレス指定の構成を作成する

ゲートウェイの構成で、使用するサブネットとパブリック IP アドレスを定義します。 次のサンプルを使用して、ゲートウェイの構成を作成します。 


        $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
        $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## 6.ゲートウェイを作成する

この手順では、仮想ネットワーク ゲートウェイを作成します。 ゲートウェイの作成には、しばらく時間がかかる場合があります。 20 分以上かかることも珍しくありません。 

次の値を使用します。

- ゲートウェイの種類は *Vpn*します。
- 指定は、RouteBased * (一部のドキュメントで動的ゲートウェイと呼ばれます)、または *ポリシー ベースの* (一部のドキュメントで静的ゲートウェイと呼ばれます)。 VPN ゲートウェイの種類の詳細については、次を参照してください。 [VPN ゲートウェイについて](vpn-gateway-about-vpngateways.md)します。     

        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

## 7.VPN デバイスの構成

オンプレミス VPN デバイスを構成するには、この時点で仮想ネットワーク ゲートウェイのパブリック IP アドレスが必要です。 具体的な構成方法については、お使いのデバイスの製造元の情報を参照してください。 さらを参照してください、 [VPN デバイス](http://go.microsoft.com/fwlink/p/?linkid=615099) の詳細。

仮想ネットワーク ゲートウェイのパブリック IP アドレスを検索するには、次のサンプルを使用します。

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## 8.VPN 接続を作成する

次に、仮想ネットワーク ゲートウェイと VPN デバイス間にサイト間 VPN 接続を作成します。 サンプルの値は必ず実際の値に変更してください。 共有キーは、VPN デバイスの構成に使用したものと同じ値にする必要があります。

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
        $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

        New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

しばらくすると、接続が確立されます。 

## 9.VPN 接続の検証

この時点では、リソース マネージャーを使用して作成されたサイト間 VPN 接続はプレビュー ポータルに表示されません。 ただしを使用して、接続が成功したことを確認することは *Get AzureRmVirtualNetworkGatewayConnection – デバッグ*します。 将来、これを行うためのコマンドレットと、プレビュー ポータルで接続を表示する機能が提供される予定です。

次のコマンドレット サンプルを使用して、実際に使用する値と同じ値を構成できます。 メッセージが表示されたら、[ *A* すべてを実行するためにします。

        Get-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Debug

 コマンドレットが完了したら、スクロールして値を表示します。 として次の例で、接続の状態を示しています *接続* 送信と受信バイト数を確認できます。

    Body:
    {
      "name": "localtovon",
      "id":
    "/subscriptions/086cfaa0-0d1d-4b1c-9455-f8e3da2a0c7789/resourceGroups/testrg/providers/Microsoft.Network/connections/loca
    ltovon",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
        "virtualNetworkGateway1": {
          "id":
    "/subscriptions/086cfaa0-0d1d-4b1c-9455-f8e3da2a0c7789/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworkGa
    teways/vnetgw1"
        },
        "localNetworkGateway2": {
          "id":
    "/subscriptions/086cfaa0-0d1d-4b1c-9455-f8e3da2a0c7789/resourceGroups/testrg/providers/Microsoft.Network/localNetworkGate
    ways/LocalSite"
        },
        "connectionType": "IPsec",
        "routingWeight": 10,
        "sharedKey": "abc123",
        "connectionStatus": "Connected",
        "ingressBytesTransferred": 33509044,
        "egressBytesTransferred": 4142431
      }


## ローカル サイト用の IP アドレスのプレフィックスを変更するには

ローカル サイト用のプレフィックスを変更する必要がある場合は、以下の手順を使用します。  VPN ゲートウェイ接続を作成したかどうかに応じて、2 種類の手順が示されます。 

### VPN ゲートウェイ接続を使用しないプレフィックスの追加または削除


- **追加する** 、追加のアドレス プレフィックスをローカル サイトを作成するが、VPN ゲートウェイの接続がまだない、次の例を使用します。

        $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')


- **削除する** VPN 接続を持たないローカル サイトからのアドレス プレフィックスは、次の例を使用します。 不要になったプレフィックスは削除します。 この例では、プレフィックス 20.0.0.0/24 (前の例に含まれる) が不要になったため、ローカル サイトを更新してそのプレフィックスを削除します。

        local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### VPN ゲートウェイ接続を使用したプレフィックスの追加または削除

VPN 接続を作成していた場合に、ローカル サイトに含まれている IP アドレスのプレフィックスを追加または削除するには、次の手順を順番に実行する必要があります。 これにより、ゲートウェイを削除して再作成する必要があるため、VPN 接続のためにある程度のダウンタイムが発生します。  ただし、その接続用の IP アドレスは要求済みであるため、以前に使用した値を変更する場合を除き、オンプレミスの VPN ルーターを再構成する必要はありません。

 
1. ゲートウェイの接続を削除します。 
2. ローカル サイトのプレフィックスを変更します。 
3. 新しいゲートウェイ接続を作成します。 

次のサンプルをガイドラインとして使用できます。


        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
        $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

        remove-AzureRmVirtualNetworkGatewayConnection -Name vnetgw1 -ResourceGroupName testrg

        $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
    

        New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'


## 次のステップ

接続が完成したら、仮想ネットワークに仮想マシンを追加することができます。 参照してください [仮想マシンを作成する](../virtual-machines/virtual-machines-windows-tutorial.md) の手順を実行します。

