<properties 
   pageTitle="Azure CLI を使用してクラシック モードで NSG を作成する方法 | Microsoft Azure"
   description="Azure CLI を使用してクラシック モードで NSG を作成してデプロイする方法について"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor="tysonn"
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

# Azure CLI で NSG (クラシック) を作成する方法

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] この記事では、従来のデプロイ モデルについて説明します。 こともできます [Nsg をリソース マネージャーの展開モデルの作成](virtual-networks-create-nsg-arm-cli.md)します。

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

以下の Azure CLI のサンプル コマンドでは、上記シナリオに基づいて単純な環境が既に作成されていると想定します。 このドキュメントに表示されているように、コマンドを実行する場合は、最初、テスト環境にビルド [VNet を作成する](virtual-networks-create-vnet-classic-cli)です。

## フロントエンドのサブネットの NSG を作成する方法
名前を名前付き NSG を作成する *NSG フロント エンド* 上記のシナリオに基づいて、以下の手順です。

1. Azure CLI を初めて使用する場合は、次を参照してください。 [のインストールと Azure CLI の構成](xplat-cli-install.md) Azure アカウントとサブスクリプションを選択する時点までの指示に従います。

2. 実行、 **azure config モード** コマンドを次に示すように、クラシック モードに切り替えます。

        azure config mode asm

    予想される出力:

        info:    New mode is asm

3. 実行、 **azure ネットワーク nsg を作成** NSG を作成するコマンドです。

        azure network nsg create -l uswest -n NSG-FrontEnd

    予想される出力:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    パラメーター:

    - **-l (または --location)**します。 NSG が作成される Azure リージョンです。 このシナリオの *westus*します。
    - **n (または - 名前)**します。 新しい NSG の名前です。 このシナリオの *NSG フロント エンド*します。

4. 実行、 **azure ネットワークの nsg ルールを作成** 、インターネットからポート 3389 (RDP) へのアクセスを許可するルールを作成するコマンドです。

        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389

    予想される出力:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

    パラメーター:

    - **は、(または - nsg 名)**します。 規則が作成される NSG の名前です。 このシナリオの *NSG フロント エンド*します。
    - **n (または - 名前)**します。 新しい規則の名前です。 このシナリオの *rdp ルール*します。
    - **c キーを押します (または - アクション)**します。 規則のアクセス レベルです (拒否または許可)。
    - **-p または - プロトコル**します。 規則のプロトコル (TCP、UDP、または *) です。 
    - **-r (または --type)**します。 接続の方向です (受信または送信)。
    - **-y (または - 優先度)**します。 規則の優先度です。
    - **-f (または - ソース アドレス プレフィックス)**します。 CIDR または既定のタグを使用する発信元アドレスのプレフィックス。
    - **-o (または - ソース ポート範囲)**します。 発信元ポート、またはポート範囲です。
    - **-e (または - 宛先アドレス プレフィックス)**します。 CIDR または既定のタグを使用する接続先アドレスのプレフィックス。
    - **-u (または - 送信先ポート範囲)**します。 接続先ポート、またはポート範囲です。    

5. 実行、 **azure ネットワークの nsg ルールを作成** 、インターネットからポート 80 (HTTP) へのアクセスを許可するルールを作成するコマンドです。

        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80

    予想される出力:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. 実行、 **azure ネットワーク nsg のサブネットの追加** フロント エンド サブネットに NSG をリンクするコマンドです。

        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd 

    予想される出力:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## バックエンドのサブネットの NSG を作成する方法
名前を名前付き NSG を作成する *NSG バックエンド* 上記のシナリオに基づいて、以下の手順です。

3. 実行、 **azure ネットワーク nsg を作成** NSG を作成するコマンドです。

        azure network nsg create -l uswest -n NSG-BackEnd

    予想される出力:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    パラメーター:

    - **-l (または --location)**します。 NSG が作成される Azure リージョンです。 このシナリオの *westus*します。
    - **n (または - 名前)**します。 新しい NSG の名前です。 このシナリオの *NSG フロント エンド*します。

4. 実行、 **azure ネットワークの nsg ルールを作成** フロント エンド サブネットからポート 1433 (SQL) へのアクセスを許可するルールを作成するコマンドです。

        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433

    予想される出力:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK


5. 実行、 **azure ネットワークの nsg ルールを作成** インターネットへのアクセスを拒否するルールを作成するコマンドです。

        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80

    予想される出力:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. 実行、 **azure ネットワーク nsg のサブネットの追加** バックエンド サブネットに NSG をリンクするコマンドです。

        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd 

    予想される出力:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK

