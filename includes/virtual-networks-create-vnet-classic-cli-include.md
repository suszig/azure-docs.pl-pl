## Azure CLI を使用して従来の VNet を作成する方法

Azure CLI を使用すると、Windows、Linux、または OSX を実行している任意のコンピューターのコマンド プロンプトから Azure のリソースを管理することができます。 Azure CLl を使用して VNet を作成するには、次の手順に従います。

1. Azure CLI を初めて使用する場合は、次を参照してください。 [のインストールと Azure CLI の構成](xplat-cli-install.md) Azure アカウントとサブスクリプションを選択する時点までの指示に従います。
2. 実行、 **azure ネットワーク vnet 作成** コマンドを次に示すように、VNet とサブネットを作成します。 出力の後に表示されるリストは、使用されたパラメーターについての説明です。

            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    
    予想される出力:

            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    - **-vnet**します。 作成する VNet の名前です。 このシナリオの *TestVNet*
    - **-e (または - アドレス空間)**します。 VNet のアドレス空間です。 このシナリオの *192.168.0.0*
    - **-i (または - cidr)**します。 CIDR 形式のネットワーク マスクです。 このシナリオの *16*します。
    - **-n (または - サブネット名**)。 最初のサブネットの名前です。 このシナリオの *フロント エンド*します。
    - **-p (または - サブネット開始 ip)**します。 サブネットの開始 IP アドレス、またはサブネット アドレス空間です。 このシナリオの *192.168.1.0*します。
    - **-r (または - サブネット cidr)**します。 サブネットの CIDR 形式のネットワーク マスクです。 このシナリオの *24*します。
    - **-l (または --location)**します。 VNet が作成される Azure リージョンです。 このシナリオの *中央アメリカ*します。

3. 実行、 **azure ネットワーク vnet のサブネットを作成** コマンドを次に示すように、サブネットを作成します。 出力の後に表示されるリストは、使用されたパラメーターについての説明です。

            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    
    上記のコマンドで想定される出力を次に示します。

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    - **-t (または--vnet 名**します。 サブネットの作成先となる VNet の名前です。 このシナリオの *TestVNet*します。
    - **n (または - 名前)**します。 新しいサブネットの名前です。 このシナリオの *バックエンド*します。
    - **は、(または - アドレス プレフィックス)**します。 サブネットの CIDR ブロックです。 次の 4 つのシナリオでは、 *192.168.2.0/24*します。

4. 実行、 **azure ネットワーク vnet ショー** コマンドを次に示すように、新しい vnet のプロパティを表示します。

            azure network vnet show

    上記のコマンドで想定される出力を次に示します。

            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

