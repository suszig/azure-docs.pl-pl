<properties 
   pageTitle="リソース マネージャーで Azure CLI を使用してルーティングを制御し、仮想アプライアンスを使用する | Microsoft Azure"
   description="Azure CLI を使用してルーティングを制御し仮想アプライアンスを使用する方法を説明する"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/11/2015"
   ms.author="telmos" />

#Azure CLI でユーザー定義のルート (UDR) を作成する

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] この記事では、リソース マネージャーの配置モデルについて説明します。 こともできます [Udr を従来のデプロイ モデルで作成](virtual-network-create-udr-classic-cli.md)します。

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

以下の Azure CLI のサンプル コマンドでは、上記シナリオに基づいて単純な環境が既に作成されていると想定します。 このドキュメントに表示されているように、コマンドを実行する場合は、最初にビルド、テスト環境を展開して [このテンプレート](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), をクリックして **Deploy to Azure**, 、必要に応じて、既定のパラメーター値を置き換えるし、ポータルの指示に従います。

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## フロントエンドのサブネットの UDR を作成する
上記のシナリオに基づいて、フロントエンドのサブネットに必要なルート テーブルとルートを作成するには、次の手順に従います。

3. 実行、 **`azure network route-table create`** フロント エンド サブネットのルート テーブルを作成するコマンドです。

        azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest

    出力:

        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK

    パラメーター:
    - **-g (または - リソース グループ)**します。 NSG の作成場所となるリソース グループの名前です。 このシナリオの *TestRG*します。
    - **-l (または --location)**します。 NSG が作成される Azure リージョンです。 このシナリオの *westus*します。
    - **n (または - 名前)**します。 新しい NSG の名前です。 このシナリオの *NSG フロント エンド*します。

4. 実行、 **`azure network route-table route create`** バック エンドのサブネット (192.168.2.0/24) に向かうすべてのトラフィックを送信する上記で作成したルート テーブル内のルートを作成するコマンド、 **FW1** VM (192.168.0.4)。

        azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4

    出力:

        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK

    パラメーター:
    - **-r (または - ルート テーブル名)**します。 ルートが追加されるルート テーブルの名前です。 このシナリオの *UDR フロント エンド*します。
    - **は、(または - アドレス プレフィックス)**します。 パケットの送信先であるサブネットのアドレス プレフィックスです。 このシナリオの *192.168.2.0/24*します。
    - **-y (--次ホップ タイプ)**します。 トラフィックの送信先となるオブジェクトの種類です。 指定できる値は *VirtualAppliance*, 、*VirtualNetworkGateway*, 、*VNETLocal*, 、*インターネット*, 、または *None*します。
    - **-p (または - 次ホップ ip アドレス**)。 次のホップ先の IP アドレスです。 このシナリオの *192.168.0.4*します。

5. 実行、 **`azure network vnet subnet set`** に上記で作成したルート テーブルに関連付けるコマンド、 **フロント エンド** サブネットです。

        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd

    出力:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

    パラメーター:
    - **e (または--vnet 名)**します。 サブネットが置かれている VNet の名前です。 このシナリオの *TestVNet*します。
 
## バックエンドのサブネットの UDR を作成する
上記のシナリオに基づいて、バックエンドのサブネットに必要なルート テーブルとルートを作成するには、次の手順に従います。

1. 実行、 **`azure network route-table create`** バックエンド サブネットにルート テーブルを作成するコマンドです。

        azure network route-table create -g TestRG -n UDR-BackEnd -l westus

4. 実行、 **`azure network route-table route create`** にフロント エンド サブネット (192.168.1.0/24) に向かうすべてのトラフィックを送信する上記で作成したルート テーブル内のルートを作成するコマンド、 **FW1** VM (192.168.0.4)。

        azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4

5. 実行、 **`azure network vnet subnet set`** に上記で作成したルート テーブルに関連付けるコマンド、 **バックエンド** サブネットです。

        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd

## FW1 で IP 転送を有効にする
使用される NIC の IP 転送を有効にする **FW1**, 、次の手順に従います。

1. 実行、 **`azure network nic show`** コマンド、およびの値に注意してください **を有効にする IP 転送**します。 設定する必要がある *false*します。

        azure network nic show -g TestRG -n NICFW1

    出力:
        
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK

2. 実行、 **`azure network nic set`** コマンドを IP 転送を有効にします。

        azure network nic set -g TestRG -n NICFW1 -f true

    出力:

        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK

    パラメーター:

    - **-f (または - ip 転送を有効に)**します。 *true* または *false*します。

