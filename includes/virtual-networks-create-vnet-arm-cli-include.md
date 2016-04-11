## Azure CLI を使用して VNet を作成する方法

Azure CLI を使用すると、Windows、Linux、または OSX を実行している任意のコンピューターのコマンド プロンプトから Azure のリソースを管理することができます。 Azure CLl を使用して VNet を作成するには、次の手順に従います。

1. Azure CLI を初めて使用する場合は、次を参照してください。 [のインストールと Azure CLI の構成](xplat-cli-install.md) Azure アカウントとサブスクリプションを選択する時点までの指示に従います。
2. 実行、 **azure config モード** コマンドを次に示すように、リソース マネージャー モードに切り替えます。

        azure config mode arm

    上記のコマンドで想定される出力を次に示します。

        info:    New mode is arm

3. 必要に応じて、実行、 **azure グループを作成** を次に示すように、新しいリソース グループを作成します。 コマンドの出力が表示されます。 出力の後に表示されるリストは、使用されたパラメーターについての説明です。 リソース グループの詳細については、次を参照してください。 [Azure リソース マネージャーの概要](resource-group-overview.md/#resource-groups)します。

        azure group create -n TestRG -l centralus

    上記のコマンドで想定される出力を次に示します。

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **n (または - 名前)**します。 新しいリソース グループの名前です。 このシナリオの *TestRG*します。
    - **-l (または --location)**します。 新しいリソース グループが作成される Azure リージョンです。 このシナリオの *centralus*します。

4. 実行、 **azure ネットワーク vnet 作成** コマンドを次に示すように、VNet とサブネットを作成します。 

        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus

    上記のコマンドで想定される出力を次に示します。

        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK

    - **-g (または - リソース グループ)**します。 VNet の作成場所となるリソース グループの名前です。 このシナリオの *TestRG*します。
    - **n (または - 名前)**します。 作成する VNet の名前です。 このシナリオの *TestVNet*
    - **は、(または - アドレス プレフィックス)**します。 VNet のアドレス空間に使用される CIDR ブロックの一覧です。 このシナリオの *192.168.0.0/16*
    - **-l (または --location)**します。 VNet が作成される Azure リージョンです。 このシナリオの *centralus*します。

5. 実行、 **azure ネットワーク vnet のサブネットを作成** コマンドを次に示すように、サブネットを作成します。 コマンドの出力が表示されます。 出力の後に表示される一覧では、使用されたパラメーターについて説明されています。

        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24

    上記のコマンドで想定される出力を次に示します。

        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK

    - **e (または--vnet 名**します。 サブネットの作成先となる VNet の名前です。 このシナリオの *TestVNet*します。
    - **n (または - 名前)**します。 新しいサブネットの名前です。 このシナリオの *フロント エンド*します。
    - **は、(または - アドレス プレフィックス)**します。 サブネットの CIDR ブロックです。 次の 4 つのシナリオでは、 *192.168.1.0/24*します。

6. 必要に応じて、上記の手順 5. を繰り返して他のサブネットを作成します。 このシナリオの実行を作成するには、次のコマンド、 *バックエンド* サブネットです。

        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24

4. 実行、 **azure ネットワーク vnet ショー** コマンドを次に示すように、新しい vnet のプロパティを表示します。

        azure network vnet show -g TestRG -n TestVNet

    上記のコマンドで想定される出力を次に示します。

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK

