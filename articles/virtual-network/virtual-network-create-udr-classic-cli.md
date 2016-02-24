<properties 
   pageTitle="クラシック デプロイメント モデルで Azure CLI を使用してルーティングを制御し、仮想アプライアンスを使用する | Microsoft Azure"
   description="クラシック デプロイメント モデルで Azure CLI を使用して VNet でのルーティングを制御する方法を説明する"
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

#Azure CLI を使用してルーティングを制御し仮想アプライアンス (クラシック) を使用する

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] この記事では、従来のデプロイ モデルについて説明します。 こともできます [ルーティングを制御し、仮想アプライアンスを使用して、リソース マネージャーの展開モデルの](virtual-network-create-udr-arm-cli.md)です。

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

以下の Azure CLI のサンプル コマンドでは、上記シナリオに基づいて単純な環境が既に作成されていると想定します。 このドキュメントに表示されているように、コマンドを実行する場合に示すように環境を作成 [Azure CLI を使用して VNet (クラシック) を作成する](virtual-networks-create-vnet-classic-cli.md)です。

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## フロントエンドのサブネットの UDR を作成する
上記のシナリオに基づいて、フロントエンドのサブネットに必要なルート テーブルとルートを作成するには、次の手順に従います。

1. 実行、 **`azure config mode`** クラシック モードに切り替えます。

        azure config mode asm

    出力:

        info:    New mode is asm

3. 実行、 **`azure network route-table create`** フロント エンド サブネットのルート テーブルを作成するコマンドです。

        azure network route-table create -n UDR-FrontEnd -l uswest

    出力:

        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK

    パラメーター:
    - **-l (または --location)**します。 NSG が作成される Azure リージョンです。 このシナリオの *westus*します。
    - **n (または - 名前)**します。 新しい NSG の名前です。 このシナリオの *NSG フロント エンド*します。

4. 実行、 **`azure network route-table route set`** バック エンドのサブネット (192.168.2.0/24) に向かうすべてのトラフィックを送信する上記で作成したルート テーブル内のルートを作成するコマンド、 **FW1** VM (192.168.0.4)。

        azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4

    出力:

        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK

    パラメーター:
    - **-r (または - ルート テーブル名)**します。 ルートが追加されるルート テーブルの名前です。 このシナリオの *UDR フロント エンド*します。
    - **は、(または - アドレス プレフィックス)**します。 パケットの送信先であるサブネットのアドレス プレフィックスです。 このシナリオの *192.168.2.0/24*します。
    - **-t (または - 次ホップ タイプ)**します。 トラフィックの送信先となるオブジェクトの種類です。 指定できる値は *VirtualAppliance*, 、*VirtualNetworkGateway*, 、*VNETLocal*, 、*インターネット*, 、または *None*します。
    - **-p (または - 次ホップ ip アドレス**)。 次のホップ先の IP アドレスです。 このシナリオの *192.168.0.4*します。

5. 実行、 **`azure network vnet subnet route-table add`** に上記で作成したルート テーブルに関連付けるコマンド、 **フロント エンド** サブネットです。

        azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd

    出力:

        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK 

    パラメーター:
    - **-t (または--vnet 名)**します。 サブネットが置かれている VNet の名前です。 このシナリオの *TestVNet*します。
    - **-n (または - サブネット名**します。 ルート テーブルが追加されるサブネットの名前です。 このシナリオの *フロント エンド*します。
 
## バックエンドのサブネットの UDR を作成する
上記のシナリオに基づいて、バックエンドのサブネットに必要なルート テーブルとルートを作成するには、次の手順に従います。

3. 実行、 **`azure network route-table create`** バックエンド サブネットにルート テーブルを作成するコマンドです。

        azure network route-table create -n UDR-BackEnd -l uswest

4. 実行、 **`azure network route-table route set`** にフロント エンド サブネット (192.168.1.0/24) に向かうすべてのトラフィックを送信する上記で作成したルート テーブル内のルートを作成するコマンド、 **FW1** VM (192.168.0.4)。

        azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4

5. 実行、 **`azure network vnet subnet route-table add`** に上記で作成したルート テーブルに関連付けるコマンド、 **バックエンド** サブネットです。

        azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd


