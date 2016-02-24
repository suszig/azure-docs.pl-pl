<properties
   pageTitle="PowerShell を使用して、クラシック デプロイメント モデルの ExpressRoute 回線のルーティングを構成する方法 |Microsoft Azure"
   description="この記事では、ExpressRoute 回線のプライベート、パブリックおよび Microsoft ピアリングを作成し、プロビジョニングする手順について説明します。 この記事では、回線のピアリングの状態確認、更新、または削除の方法も示します。"
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carolz"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="11/05/2015"
   ms.author="cherylmc"/>

# PowerShell を使用した ExpressRoute 回線のルーティングの作成と変更

> [AZURE.SELECTOR]
[PowerShell - クラシック](expressroute-howto-routing-classic.md)
[リソース マネージャーの PowerShell](expressroute-howto-routing-arm.md)

この記事では、PowerShell コマンドレットとクラシック デプロイメント モデルを使用して、ExpressRoute 回線のルーティング構成を作成して管理する手順について説明します。  次の手順も紹介する状態、更新、または削除を確認し、ExpressRoute 回線のピアリングのプロビジョニングを解除する方法です。

[AZURE.INCLUDE [vpn-gateway-sm-rm](../../includes/vpn-gateway-sm-rm-include.md)] 

## 構成の前提条件

- Azure PowerShell モジュールの最新バージョンが必要になります。 PowerShell セクションから最新の PowerShell モジュールをダウンロードすることができます、 [Azure のダウンロード ページ](http://azure.microsoft.com/downloads)します。 指示に従って、 [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md) 手順については、Azure PowerShell モジュールを使用するようコンピューターを構成する方法に関するページ。 
- 参照ことを確認、 [の前提条件](expressroute-prerequisites.md) ] ページで、 [ルーティング要件を満たす](expressroute-routing.md) ページおよび [ワークフロー](expressroute-workflows.md) 構成を開始する前に] ページします。
- アクティブな ExpressRoute 回線が必要です。 指示に従って、 [ExpressRoute 回線を作成](expressroute-howto-circuit-classic.md) を続行する前に、接続プロバイダーが有効になっている回線があるとします。 ExpressRoute 回線をプロビジョニングされ、有効になっている状態にする必要があります。そうすれば、以下で説明されているコマンドレットを実行できます。

>[AZURE.IMPORTANT] 次の手順は、レイヤー 2 接続サービスを提供するサービス プロバイダーを作成する回線ののみ適用されます。 サービス プロバイダーが提供する管理対象レイヤー 3 サービス (MPLS など、通常は IPVPN) を使用する場合、接続プロバイダーがユーザーに代わってルーティングを構成および管理します。 そのような場合は、ユーザーがピアリングを作成したり、管理したりすることはできません。 

ExpressRoute 回線用に 1 つ、2 つ、または 3 つすべてのピアリング (Azure プライベート、Azure パブリックおよび Microsoft) を構成することができます。 ピアリングは任意の順序で構成することができます。 ただし、各ピアリングの構成は必ず一度に 1 つずつ完了するようにしてください。 

## Azure プライベート ピアリング

このセクションでは、ExpressRoute 回線用の Azure プライベート ピアリング構成を作成、取得、更新、および削除する方法について説明します。 

### Azure プライベート ピアリングを作成するには

1. **ExpressRoute 用の PowerShell モジュールをインポートします。**
    
    ExpressRoute コマンドレットの使用を開始するには、PowerShell セッションに Azure および ExpressRoute モジュールをインポートする必要があります。 PowerShell セッションに Azure および ExpressRoute モジュールをインポートするには、次のコマンドを実行します。  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **ExpressRoute 回線を作成します。**
    
    作成するための指示に従って、 [ExpressRoute 回線](expressroute-howto-circuit-classic.md) 接続プロバイダーから提供されたことがあるとします。 接続プロバイダーが管理対象レイヤー 3 サービスを提供する場合は、Azure プライベート ピアリングを有効にするように接続プロバイダーに要求できます。 その場合は、次のセクションにリストされている手順に従う必要はありません。 ただし、接続プロバイダーがルーティングを管理しない場合は、回線を作成した後、以下の手順に従います。 

3. **ExpressRoute 回線がプロビジョニングされていることを確認します。**

    まず、ExpressRoute 回線がプロビジョニングされており、有効にもなっているかどうかを確認する必要があります。 次の例を見てください。

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    回線が Provisioned および Enabled と示されていることを確認します。 示されていない場合は、接続プロバイダーに連絡して、回線が必要な状態とステータスになるようにします。

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **回線用に Azure プライベート ピアリングを構成します。**

    次の手順に進む前に、以下のものがそろっていることを確認します。

    - プライマリ リンク用の /30 サブネット。 これを、仮想ネットワーク用に予約されたアドレス空間の一部にすることはできません。
    - セカンダリ リンク用の /30 サブネット。 これを、仮想ネットワーク用に予約されたアドレス空間の一部にすることはできません。
    - このピアリングを確立するための有効な VLAN ID。 回線の他のピアリングが同じ VLAN ID を使用しないようにしてください。
    - ピアリングの AS 番号。 2 バイトと 4 バイトの AS 番号の両方を使用することができます。 このピアリングではプライベート AS 番号を使用できます。 65515 を使用しないようにしてください。
    - いずれかを使用する場合は、MD5 ハッシュ。 **これは省略可能な**です。
    
    次のコマンドレットを実行して、回線用に Azure プライベート ピアリングを構成することができます。

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100

    MD5 ハッシュを使用する場合は、以下のコマンドレットを使用できます。

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] ピアリング ASN、ASN 顧客としての AS 番号を指定することを確認します。

### Azure プライベート ピアリングの詳細を取得するには

次のコマンドレットを使用して、構成の詳細を取得することができます。

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### Azure プライベート ピアリングの構成を更新するには

次のコマンドレットを使用して、構成のどの部分でも更新することができます。 次の例では、回路の VLAN ID が 100 から 500 に更新されています。

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### Azure プライベート ピアリングを削除するには

以下のコマンドレットを実行して、ピアリング構成を削除することができます。

>[AZURE.WARNING] このコマンドレットを実行する前にすべての仮想ネットワークが ExpressRoute 回線のリンクでないことを確認する必要があります。 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## Azure パブリック ピアリング

このセクションでは、ExpressRoute 回線用の Azure パブリック ピアリング構成を作成、取得、更新および削除する方法について説明します。

### Azure パブリック ピアリングを作成するには

1. **ExpressRoute 用の PowerShell モジュールをインポートします。**
    
    ExpressRoute コマンドレットの使用を開始するには、PowerShell セッションに Azure および ExpressRoute モジュールをインポートする必要があります。 PowerShell セッションに Azure および ExpressRoute モジュールをインポートするには、次のコマンドを実行します。 

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **ExpressRoute 回線の作成**
    
    作成するための指示に従って、 [ExpressRoute 回線](expressroute-howto-circuit-classic.md) 接続プロバイダーから提供されたことがあるとします。 接続プロバイダーが管理対象レイヤー 3 サービスを提供する場合は、Azure プライベート ピアリングを有効にするように接続プロバイダーに要求できます。 その場合は、次のセクションにリストされている手順に従う必要はありません。 ただし、接続プロバイダーがルーティングを管理しない場合は、回線を作成した後、以下の手順に従います。

3. **ExpressRoute 回線がプロビジョニングされていることを確認します。**

    まず、ExpressRoute 回線がプロビジョニングされており、有効にもなっているかどうかを確認する必要があります。 次の例を見てください。

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    回線が Provisioned および Enabled と示されていることを確認します。 示されていない場合は、接続プロバイダーに連絡して、回線が必要な状態とステータスになるようにします。

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled

    

4. **回線用に Azure パブリック ピアリングを構成します。**

    作業を続行する前に、次の情報がそろっていることを確認します。

    - プライマリ リンク用の /30 サブネット。 これは有効なパブリック IPv4 プレフィックスである必要があります。
    - セカンダリ リンク用の /30 サブネット。 これは有効なパブリック IPv4 プレフィックスである必要があります。
    - このピアリングを確立するための有効な VLAN ID。 回線の他のピアリングが同じ VLAN ID を使用しないようにしてください。
    - ピアリングの AS 番号。 2 バイトと 4 バイトの AS 番号の両方を使用することができます。 このピアリングにはパブリック AS 番号を使用する必要があります。
    - いずれかを使用する場合は、MD5 ハッシュ。 **これは省略可能な**です。
    
    次のコマンドレットを実行して、回線用に Azure プライベート ピアリングを構成することができます。

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200

    MD5 ハッシュを使用する場合は、次のコマンドレットを使用することができます。

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] ピアリング ASN あり、ASN お客様として AS 番号を指定することを確認します。

### Azure パブリック ピアリングの詳細を取得するには

次のコマンドレットを使用して、構成の詳細を取得することができます。

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### Azure パブリック ピアリング構成を更新するには

次のコマンドレットを使用して、構成のどの部分も更新することができます。

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

上記の例では、回線の VLAN ID は 200 から 600 に更新されています。

### Azure パブリック ピアリングを削除するには

次のコマンドレットを実行して、ピアリング構成を削除することができます。

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## Microsoft ピアリング

このセクションでは、ExpressRoute 回線の Microsoft ピアリング構成を作成、取得、更新および削除する方法について説明します。 

### Microsoft ピアリングを作成するには

1. **ExpressRoute 用の PowerShell モジュールをインポートします。**
    
    ExpressRoute コマンドレットの使用を開始するには、PowerShell セッションに Azure および ExpressRoute モジュールをインポートする必要があります。 PowerShell セッションに Azure および ExpressRoute モジュールをインポートするには、次のコマンドを実行します。  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **ExpressRoute 回線の作成**
    
    作成するための指示に従って、 [ExpressRoute 回線](expressroute-howto-circuit-classic.md) 接続プロバイダーから提供されたことがあるとします。 接続プロバイダーが管理対象レイヤー 3 サービスを提供する場合は、Azure プライベート ピアリングを有効にするように接続プロバイダーに要求できます。 その場合は、次のセクションにリストされている手順に従う必要はありません。 ただし、接続プロバイダーがルーティングを管理しない場合は、回線を作成した後、以下の手順に従います。

3. **ExpressRoute 回線がプロビジョニングされていることを確認します。**

    まず、ExpressRoute 回線がプロビジョニングされており、有効な状態になっているかどうかを確認する必要があります。

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    回線が Provisioned および Enabled と示されていることを確認します。 示されていない場合は、接続プロバイダーに連絡して、回線が必要な状態とステータスになるようにします。

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **回線用に Microsoft ピアリングを構成します。**

    続行する前に、次の情報を確認してください。

    - プライマリ リンク用の /30 サブネット。 これは、自分が所有しており、RIR/IRR に登録されている有効なパブリック IPv4 プレフィックスである必要があります。
    - セカンダリ リンク用の /30 サブネット。 これは、自分が所有しており、RIR/IRR に登録されている有効なパブリック IPv4 プレフィックスである必要があります。
    - このピアリングを確立するための有効な VLAN ID。 回線の他のピアリングが同じ VLAN ID を使用しないようにしてください。
    - ピアリングの AS 番号。 2 バイトと 4 バイトの AS 番号の両方を使用することができます。 パブリック AS 番号のみを使用する必要があります。 AS 番号を所有する必要があります。
    - アドバタイズされたプレフィックス: BGP セッションを介してアドバタイズする予定のすべてのプレフィックスのリストを指定する必要があります。 パブリック IP アドレス プレフィックスのみが受け入れられます。 一連のプレフィックスを送信する予定の場合は、コンマ区切りのリストを送信できます。 これらのプレフィックスは、RIR/IRR に登録する必要があります。
    - 顧客 ASN: ピアリング AS 番号に登録されていないプレフィックスをアドバタイズする場合は、そのプレフィックスが登録されている AS 数を指定できます。 **これは省略可能な**です。
    - ルーティング レジストリ名: AS 番号とプレフィックスを登録する RIR/IRR を指定することができます。
    - いずれかを使用する場合は、MD5 ハッシュ。 **これは省略可能です。**
    
    次のコマンドレットを実行して、回線用に Microsoft ピアリングを構成することができます。

        New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"


### Microsoft ピアリングの詳細を取得するには

次のコマンドレットを使用して、構成の詳細を取得できます。

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### Microsoft ピアリング構成を更新するには

次のコマンドレットを使用して、構成のどの部分でも更新することができます。

        Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### Microsoft ピアリングを削除するには

以下のコマンドレットを実行して、ピアリング構成を削除することができます。

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## 次のステップ

次に、 [VNet を ExpressRoute 回線にリンク](expressroute-howto-linkvnet-classic.md)します。


-  ワークフローの詳細については、次を参照してください。 [ExpressRoute ワークフロー](expressroute-workflows.md)します。
-  回路のピアリングの詳細については、次を参照してください。 [ExpressRoute 回路とルーティング ドメイン](expressroute-circuit-peerings.md)します。


