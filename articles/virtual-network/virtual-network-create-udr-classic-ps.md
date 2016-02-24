<properties 
   pageTitle="クラシック デプロイメント モデルで PowerShell を使用してルーティングを制御し、仮想アプライアンスを使用する | Microsoft Azure"
   description="クラシック デプロイメント モデルで PowerShell を使用して VNet でのルーティングを制御する方法を使用する"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/11/2015"
   ms.author="telmos" />

#PowerShell を使用してルーティングを制御し仮想アプライアンス (クラシック) を使用する

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] この記事では、従来のデプロイ モデルについて説明します。 こともできます [アクションの ARM をここに入力](armToken)します。

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

以下の Azure PowerShell のサンプル コマンドでは、上記シナリオに基づいて単純な環境が既に作成されていると想定します。 このドキュメントに表示されているように、コマンドを実行する場合に示すように環境を作成 [PowerShell を使用して VNet (クラシック) を作成する](virtual-networks-create-vnet-classic-ps.md)です。

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## フロントエンドのサブネットの UDR を作成する
上記のシナリオに基づいて、フロントエンドのサブネットに必要なルート テーブルとルートを作成するには、次の手順に従います。

3. 実行、 **`New-AzureRouteTable`** コマンドレットをフロント エンド サブネットのルート テーブルを作成します。

        ```powershell
        New-AzureRouteTable -Name UDR-FrontEnd `
            -Location uswest `
            -Label "Route table for front end subnet"
        ```

    出力:

        Name         Location   Label                          
        ----         --------   -----                          
        UDR-FrontEnd West US    Route table for front end subnet

4. 実行、 **`Set-AzureRoute`** をバック エンドのサブネット (192.168.2.0/24) に向かうすべてのトラフィックを送信する上記で作成したルート テーブル内のルートを作成するコマンドレット、 **FW1** VM (192.168.0.4)。
    
        ```powershell
        Get-AzureRouteTable UDR-FrontEnd `
            |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4
        ```

    出力:

        Name     : UDR-FrontEnd
        Location : West US
        Label    : Route table for frontend subnet
        Routes   : 
                   Name                 Address Prefix    Next hop type        Next hop IP address
                   ----                 --------------    -------------        -------------------
                   RouteToBackEnd       192.168.2.0/24    VirtualAppliance     192.168.0.4  

5. 実行、 **`Set-AzureSubnetRouteTable`** に上記で作成したルート テーブルを関連付けるコマンドレット、 **フロント エンド** サブネットです。

        ```powershell
        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName FrontEnd `
            -RouteTableName UDR-FrontEnd
        ```
 
## バックエンドのサブネットの UDR を作成する
上記のシナリオに基づいて、バックエンドのサブネットに必要なルート テーブルとルートを作成するには、次の手順に従います。

3. 実行、 **`New-AzureRouteTable`** コマンドレットをバック エンドのサブネットにルート テーブルを作成します。

        ```powershell
        New-AzureRouteTable -Name UDR-BackEnd `
            -Location uswest `
            -Label "Route table for back end subnet"
        ```

4. 実行、 **`Set-AzureRoute`** にフロント エンド サブネット (192.168.1.0/24) に向かうすべてのトラフィックを送信する上記で作成したルート テーブル内のルートを作成するコマンドレット、 **FW1** VM (192.168.0.4)。

        ```powershell
        Get-AzureRouteTable UDR-BackEnd `
            |Set-AzureRoute -RouteName RouteToFrontEnd -AddressPrefix 192.168.1.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4
        ```

5. 実行、 **`Set-AzureSubnetRouteTable`** に上記で作成したルート テーブルを関連付けるコマンドレット、 **バックエンド** サブネットです。

        ```powershell
        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName FrontEnd `
            -RouteTableName UDR-FrontEnd
        ```
## FW1 VM で IP 転送を有効にする
FW1 VM で IP 転送を有効にするには、次の手順に従います。

1. 実行、 **`Get-AzureIPForwarding`** IP 転送の状態をチェックするコマンドレット

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Get-AzureIPForwarding

    出力:

        Disabled

2. 実行、 **`Set-AzureIPForwarding`** IP の転送を有効にするコマンド、 *FW1* VM です。

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Set-AzureIPForwarding -Enable
