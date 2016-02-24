<properties 
   pageTitle="Azure で従来の VNet と ARM Vnet を接続する方法 - ソリューション ガイド"
   description="従来の VNet と新しい VNet の間の VPN 接続を作成する方法を学習します。"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/11/2015"
   ms.author="telmos" />

# 従来の VNet を新しい VNet に接続する

Azure には現在、Azure サービス マネージャー (従来型と呼ばれます) と Azure リソース マネージャー (ARM) という 2 つの管理モードがあります。 これまで Azure を使用してきているユーザーであれば、おそらく Azure VM および従来の VNet 上で実行されているインスタンス ロールを利用されていることでしょう。 そして、新しい VM とロール インスタンスが、ARM で作成された VNet 上で実行されていることも考えられます。

このような場合、新しいインフラストラクチャと従来のリソースが確実に通信できるようにする必要があります。 これを実行するには、2 つの VNet 間で VPN 接続を作成します。 次の図は、2 つの VNet  (従来の VNet と ARM VNet)、およびこれらの VNet 間にあるセキュリティ保護されたトンネル接続を含む環境の例です。

![](..\virtual-network\media\virtual-networks-arm-asm-s2s\figure01.png)

>[AZURE.NOTE] このドキュメントでは、テスト目的で、エンド ツー エンド ソリューションを説明します。 Vnets のある状態、VPN ゲートウェイと Azure のサイト間の接続に慣れている場合は、次を参照してください。 [ARM VNet と従来の VNet 間の S2S VPN の構成](../virtual-networks-arm-asm-s2s-howto.md)します。

このシナリオをテストするには、次を実行します。

1. [従来の VNet 環境を作成する](#Create-a-classic-VNet-environment)です。
2. [新しい VNet 環境を作成](#Create-a-new-VNet-environment)します。
3. [2 つの Vnet を接続](#Connect-the-two-VNets)します。

上記の手順を実行するにはまず、従来のポータル、ネットワーク構成ファイル、および Azure サービス マネージャーの PowerShell コマンドレットを含む従来の Azure 管理ツールを使用します。その後に、新しい Azure ポータル、ARM テンプレート、および ARM PowerShell コマンドレットを含む、新しい管理ツールを使用します。

>[AZURE.IMPORTANT] 相互に接続する Vnet の CIDR ブロックが競合を回避することもできます。 VNet にはそれぞれ、必ず一意の CIDR ブロックを使用してください。 

## 従来の VNet 環境を作成する

既存の従来の VNet を使用して、新しい ARM VNet に接続できます。 この例では、2 つのサブネット、ゲートウェイ、および VM を含む従来の VNet を、新たにテスト用に作成する方法を説明します。

### 手順 1: 従来の VNet を作成する

上の図 1 に対応する新しい VNet を作成するには、以下の手順に従います。

1. PowerShell コンソールで、次のコマンドを実行してご自身の Azure アカウントを追加します。

        Add-AzureAccount

2. サインイン ダイアログ ボックスに表示される指示に従い、Azure アカウントでログオンします。

3. Azure サービス管理 PowerShell コマンドレットが確実に使用されるように、次のコマンドを実行します。

        Switch-AzureMode AzureServiceManagement

4. 次のコマンドを実行して、Azure ネットワークの構成ファイルをダウンロードします。

        Get-AzureVNetConfig -ExportToFile c:\Azure\classicvnets.netcfg

        XMLConfiguration                                OperationDescription                            OperationId                                     OperationStatus                                
        ----------------                                --------------------                            -----------                                     ---------------                                
        <?xml version="1.0" encoding="utf-8"?>...       Get-AzureVNetConfig                             04ab3224-7e1c-cc1a-8b75-96c6c300ddb8            Succeeded       

5. ダウンロードしたファイルを開き、という名前のローカル ネットワーク サイトを追加 *vnet02* を使用する *10.2.0.0/16* 、アドレスのプレフィックスと VPN ゲートウェイのアドレスとして有効なパブリック IP アドレスです。 できるように追加することで、 **LocalNetworkSite** 要素を **LocalNetworkSites** 構成ファイル内の要素。 次のファイル スニペットは、例を示しています。 **LocalNetworksSites** 要素。

        <LocalNetworkSites>
          <LocalNetworkSite name="vnet02">
            <AddressSpace>
              <AddressPrefix>10.2.0.0/16</AddressPrefix>
            </AddressSpace>
            <VPNGatewayAddress>2.0.0.2</VPNGatewayAddress>
          </LocalNetworkSite>
        </LocalNetworkSites>        

6.  **VirtualNetworkSites** 要素のアドレス プレフィックスを持つ新しい仮想ネットワークを追加する *10.1.0.0/16* シナリオに従って、2 つのサブネットの図の上とします。 次のファイル スニペットは、例を示しています。 **VirtualNetworkSites** 要素。
    
        <VirtualNetworkSites>
          <VirtualNetworkSite name="vnet01" Location="East US">
            <AddressSpace>
              <AddressPrefix>10.1.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="Subnet1">
                <AddressPrefix>10.1.0.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="GatewaySubnet">
                <AddressPrefix>10.1.200.0/29</AddressPrefix>
              </Subnet>
            </Subnets>
          </VirtualNetworkSite>
        </VirtualNetworkSites>

7. ファイルを保存してから、次のコマンドを実行して Azure にアップロードします。 必要に応じて、ファイル パスを環境に合わせて変更してください。

        Set-AzureVNetConfig -ConfigurationPath c:\Azure\classicvnets.netcfg

        OperationDescription                                            OperationId                                                     OperationStatus                                                
        --------------------                                            -----------                                                     ---------------                                                
        Set-AzureVNetConfig                                             e0ee6e66-9167-cfa7-a746-7cab93c22013                            Succeeded 

### 手順 2: 従来の VNet で VM を作成する

Azure サービス マネージャーの PowerShell コマンドレットを使用して、従来の VNet に VM を作成するには、以下の手順に従います。

1. Azure から VM イメージを取得します。 次の PowerShell コマンドを使用すると、入手可能な最新の Windows Server 2012 R2 イメージが取得できます。

        $WinImage = (Get-AzureVMImage `
            | ?{$_.ImageFamily -eq "Windows Server 2012 R2 Datacenter"} `
            | sort PublishedDate -Descending)[0]        

2. 次のコマンドを実行して、VM の仮想ハード ドライブ (VHD) を保存するストレージ アカウントを作成します。

        $storage1 = New-AzureStorageAccount `
            -StorageAccountName v1v2teststorage1 `
            -Location "East US" 

3.  次のコマンドを実行して、VM を作成します。 必ずユーザー名とパスワードの値を置き換えてください。

        $vm1 = New-AzureVMConfig -Name "VM01" -InstanceSize "ExtraLarge" `
            -Image $WinImage.ImageName –AvailabilitySetName "MyAVSet1" `
            -MediaLocation "https://v1v2teststorage1.blob.core.windows.net/vhd/vm01.vhd"
        Add-AzureProvisioningConfig –VM $vm1 -Windows `
            -AdminUserName "user" -Password "P@ssw0rd" 

4.  VM への接続 *Subnet1* 次のコマンドを実行しています。

        Set-AzureSubnet -SubnetNames "Subnet1" -VM $vm1
        Set-AzureStaticVNetIP  -IPAddress "10.1.0.101" -VM $vm1

5. 次のコマンドを実行して、VM をホストする新しいクラウド サービスを作成します。

        New-AzureService -ServiceName "v1v2svc01" -Location "East US"
        New-AzureVM -ServiceName "v1v2svc01" –VNetName "vnet01" –VMs $vm1

### 手順 3: 従来の VNet 用の VPN Gateway を作成する 

従来の Azure ポータルを使用して vnet01 の VPN Gateway を作成するには、以下の手順に従います。

1. Https://manage.windowsazure.com で従来の Azure ポータルを開きます。 必要に応じて、資格情報を指定します。
2. 一覧を下へスクロール **すべてのアイテム** ] をクリック **ネットワーク**します。
3. 仮想ネットワークの一覧で、をクリックして **vnet01**, 、] をクリックし、 **構成**します。
4.  **サイト対サイト接続**, 、有効にする、 **ローカル ネットワークに接続する** チェック ボックスをオンします。
5.  **ローカル ネットワーク** 一覧で、[ **vnet02**, 、] をクリックし、 **保存**, 、] をクリックし、 **[はい]**します。
6. クリックして **ダッシュ ボード** し、次の図 2 に示すようにゲートウェイを示すメッセージがまだ作成されていないに注意してください。

    ![VNet のダッシュ ボード](..\virtual-network\media\virtual-networks-arm-asm-s2s\figure02.png)

7. クリックして **ゲートウェイの作成** vnet01 の VPN ゲートウェイを作成する次の図 3 に示すようにします。

    ![VNet のダッシュ ボード](..\virtual-network\media\virtual-networks-arm-asm-s2s\figure03.png)

8. ゲートウェイの種類の一覧でクリックして **動的**, 、] をクリックし、 **はい**します。 ゲートウェイの作成中、ゲートウェイが黄色で表示されるため、ダッシュ ボード イメージが変わります。 

    ![VNet のダッシュ ボード](..\virtual-network\media\virtual-networks-arm-asm-s2s\figure04.png)

    >[AZURE.NOTE] この操作には数分かかる場合があります。

9. ゲートウェイが作成されたら、以下に示されるそのゲートウェイのパブリック IP アドレスを書き留めます。 このアドレスは、後で ARM VNet のローカル ネットワークを作成するときに必要になります。

    ![VNet のダッシュ ボード](..\virtual-network\media\virtual-networks-arm-asm-s2s\figure05.png)

## 新しい VNet 環境の作成

これで VM とゲートウェイを含む従来の VNet が作成できたので、次に ARM VNet を作成します。

### 手順 1: ARM で新しい VNet を作成する

ARM テンプレートを使用して、2 つのサブネットおよび従来の VNet のローカル ネットワークを含む ARM VNet を作成するには、次の手順に従います。

1. Azuredeploy.json と azuredeploy parameters.json ファイルをダウンロード [git ハブ](https://github.com/Azure/azure-quickstart-templates/tree/master/arm-asm-s2s)します。
2. Visual Studio で azuredeploy.json ファイルを開くと、このテンプレートでは次の 4 つのリソースを作成できることがわかります。 

    - **ローカル ゲートウェイ**: このリソースに接続する VNet 用に作成するゲートウェイを表します。 この場合、vnet01 用のゲートウェイがそれにあたります。
    - **仮想ネットワーク**: このリソースが作成される ARM VNet を表します。 この場合、vnet02 がそれにあたります。
    - **ゲートウェイのパブリック IP**: このリソースは、ARM VNet 用に作成するゲートウェイのパブリック IP アドレスを表します。 
    - **ゲートウェイ**: このリソースは、ARM VNet (vnet02) 用に作成するゲートウェイを表します。

3. このファイルで使用されるパラメーターに注意してください。 これらのほとんどには既定値があります。 これらの値は、必要に応じて変更できます。 

4. Visual Studio で azuredeploy-parameters.json ファイルを開きます。 このファイルには、テンプレート ファイル内のパラメーターに使用する値が含まれます。  必要に応じて、次の値を編集します。

    - **subscriptionId**: を編集し、サブスクリプション id を貼り付けます。 サブスクリプション id を把握していない場合は、実行、 **Get-azuresubscription** PowerShell コマンドレットの id を取得します。
    - **場所**: VNet を作成する Azure の場所を指定します。 このシナリオではなります **中央アメリカ**します。
    - **virtualNetworkName**: これは、作成される ARM VNet の名前。 このシナリオで **vnet02**します。
    - **localGatewayName**: これは、新しい ARM VNet から接続するローカル ネットワークです。 このシナリオで **vnet01**します。
    - **localGatewayIpAddress**: これに接続するネットワークに対して作成されたゲートウェイのパブリック IP アドレスです。 このシナリオでの VPN ゲートウェイを作成するときに上記の手順 9 でメモした IP アドレスは、この **vnet01**します。
    - **localGatewayAddressPrefix**: これは、VNet が接続するローカル ネットワークの CIDR ブロックです。 接続する VNet は、このシナリオで **vnet01**, 、CIDR ブロックは、 **10.1.0.0/16**します。
    - **gatewayPublicIPName**: これは、ARM VNet 用に作成されるゲートウェイのパブリック ip アドレスに対して作成される IP オブジェクトの名前。
    - **gatewayName**: これは、ARM VNet 用に作成されるゲートウェイ オブジェクトの名前。
    - **addressPrefix**: これは、ARM VNet の CIDR ブロックです。 このシナリオで **10.2.0.0/16**します。
    - **subnet1Prefix**: これは、ARM VNet のサブネットの CIDR ブロックです。 このシナリオで **10.2.0.0/24**します。
    - **gatewaySubnetPrefix**: これは、ARM VNet にゲートウェイ サブネットの CIDR ブロックです。 このシナリオで **10.2.200.0/29**します。
    - **connectionName**: これは、作成される接続オブジェクトの名前。
    - **sharedKey**: これは、接続の IPSec 共有キー。 このシナリオで **abc123**します。

5. という名前の新しいリソース グループに、ARM VNet とその関連オブジェクトを作成する **RG1**, 、次の PowerShell コマンドを実行します。 テンプレート ファイルと、パラメーター ファイルのパスを必ず変更してください。  

        Switch-AzureMode AzureResourceManager
        New-AzureResourceGroup -Name RG1 -Location "Central US" `
            -TemplateFile C:\Azure\azuredeploy.json `
            -TemplateParameterFile C:\Azure\azuredeploy-parameters.json     

    >[AZURE.NOTE] この操作には数分かかる場合があります。

7. ブラウザーで https://portal.azure.com/に移動し、必要に応じて、資格情報を入力してください。
8. をクリックして、 **RG1** 次のように、Azure ポータルでリソース グループを並べて表示します。

    ![VNet のダッシュ ボード](..\virtual-network\media\virtual-networks-arm-asm-s2s\figure06.png)

9. ARM テンプレートを使用してグループにリソースが追加されたことがわかります。

### 手順 2: ARM で新しい VM を作成する

Azure ポータルから、新しい VNet 内に VM を作成するには、以下の手順に従います。

1. ポータルで、をクリックして、 **新規** をクリックして、 **コンピューティング**, 、クリックして **Windows Server 2012 R2 Datacenter**します。
2. 右側のウィンドウの下部にあるで、 **コンピューティング スタックを選択して**, を選択 **リソース マネージャー スタックを使用する** 以下に示すように、ARM では、VM を作成するをクリックし、 **作成**します。

    ![VNet のダッシュ ボード](..\virtual-network\media\virtual-networks-arm-asm-s2s\figure07.png)

3.  **基本** ブレードで、VM 名、ユーザー名、パスワード、サブスクリプション、リソース グループと、VM の場所を入力し、をクリックして **OK**します。 次の図は、このシナリオの設定を示しています。

    ![VNet のダッシュ ボード](..\virtual-network\media\virtual-networks-arm-asm-s2s\figure08.png)

4.  **サイズを選択する** ブレードは、サイズを選択し、クリックして **選択**します。 このシナリオでは、次のように選択します。 **D2**します。
5.  **設定** ブレードで、をクリックして **仮想ネットワークの**, 、] をクリックし、 **vnet02**します。
6. をクリックして **サブネット**, 、順にクリックして **Subnet1**, 、] をクリックし、 **OK**します。  **概要** ブレードは次の図のようになります。

    ![VNet のダッシュ ボード](..\virtual-network\media\virtual-networks-arm-asm-s2s\figure09.png)

7. をクリックして **OK**, 、順にクリック **作成** VM を作成します。 以下に示すように、ポータルに新しいタイルが表示され、VM がプロビジョニングされているのがわかります。

    ![VNet のダッシュ ボード](..\virtual-network\media\virtual-networks-arm-asm-s2s\figure10.png)

    >[AZURE.NOTE] この操作には数分かかる場合があります。 このドキュメントの次の説明に進んでください。

## これら 2 つの VNet を接続する

VM に接続された 2 つの VNet が作成できたので、すでに確立済みのゲートウェイを使用してこれらの VNet を接続し、その接続をテストしましょう。

### 手順 1: 従来の VNet 用のゲートウェイを構成する

ARM VNet (vnet02) 用に作成されたゲートウェイの IP アドレスが使用されるように従来の VNet を構成し、各 VNet から接続を確立する必要があります。 これを実行するには、次の手順に従ってください。

1. ARM VNet のゲートウェイで使用されている IP アドレスを取得するには、次のコマンドを実行して出力を参照します。 このアドレスを書き留めます。後で、従来の VNet 用のローカル ネットワーク設定を変更するときに必要になります。

        Get-AzurePublicIpAddress | ?{$_.Name -eq "ArmAsmS2sGatewayIp"}

        Name                     : ArmAsmS2sGatewayIp
        ResourceGroupName        : RG1
        Location                 : centralus
        Id                       : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/RG1/providers/Microsoft.Network/publicIPAddresses/ArmAsmS2sGatewayIp
        Etag                     : W/"1ee6c1bd-8be1-488e-a571-77b05b49e33a"
        ProvisioningState        : Succeeded
        Tags                     : 
        PublicIpAllocationMethod : Dynamic
        IpAddress                : 23.99.213.28
        IdleTimeoutInMinutes     : 4
        IpConfiguration          : {
                                     "Id": "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworkGateways/ArmAsmGateway/ipConfigurations/vn
                                   etGatewayConfig"
                                   }
        DnsSettings              : null

2. 次のコマンドを実行すると、PowerShell コマンドに Azure サービス管理 API が確実に使用されます。

        Switch-AzureMode AzureServiceManagement

3. 次のコマンドを実行して、Azure のネットワーク構成ファイルをダウンロードします。

        Get-AzureVNetConfig -ExportToFile c:\Azure\classicvnets.netcfg

4. 、ダウンロードしたファイルを開き、編集、 **LocalNetworkSite** 要素を **vnet02** 上記の手順 1 で取得した新しい VNet ゲートウェイの IP アドレスを追加します。 この要素は、次の例のように表示されます。

          <LocalNetworkSite name="vnet02">
            <AddressSpace>
              <AddressPrefix>10.2.0.0/16</AddressPrefix>
            </AddressSpace>
            <VPNGatewayAddress>23.99.213.28</VPNGatewayAddress>
          </LocalNetworkSite>

5. ファイルを保存してから、従来の VNet を構成するために次のコマンドを実行します。 上記の手順 4 で保存したファイルをポイントするように、パスを必ず変更してください。

        Set-AzureVNetConfig -ConfigurationPath c:\Azure\classicvnets.netcfg

6. 次のコマンドを実行して、従来の VNet ゲートウェイの IPSec 共有キーを設定します。 次に示すような出力が表示されます。 ご自身の既存の VNet を使用している場合は、VNet の名前を必ず変更してください。

        Set-AzureVNetGatewayKey -VNetName vnet01 -LocalNetworkSiteName vnet02 -SharedKey abc123 

        Error          : 
        HttpStatusCode : OK
        Id             : b2154475-bf00-480e-ad1e-aed893014979
        Status         : Successful
        RequestId      : 08257a09d723cb8982c47b85edb0e08a
        StatusCode     : OK

### 手順 2: ARM VNet 用のゲートウェイを構成する

従来の VNet ゲートウェイを構成できたので、接続を確立しましょう。 これを実行するには、次の手順に従ってください。

1. PowerShell コンソールで、次のコマンドを実行して ARM モードに切り替えます。 

        Switch-AzureMode AzureResourceManager

2. 次のコマンドを実行して、ゲートウェイ間の接続を作成します。

        $vnet01gateway = Get-AzureLocalNetworkGateway -Name vnet01 -ResourceGroupName RG1
        $vnet02gateway = Get-AzureVirtualNetworkGateway -Name ArmAsmGateway -ResourceGroupName RG1
        
        New-AzureVirtualNetworkGatewayConnection -Name arm-asm-s2s-connection `
            -ResourceGroupName RG1 -Location "Central US" -VirtualNetworkGateway1 $vnet02gateway `
            -LocalNetworkGateway2 $vnet01gateway -ConnectionType IPsec `
            -RoutingWeight 10 -SharedKey 'abc123'

3. Https://manage.windowsazure.com で、Azure ポータルを開き、必要に応じて、資格情報を入力します。
4. [ **すべての項目**, 、スクロール ダウンして、をクリックして **ネットワーク**, 、] をクリックし、 **vnet01**, 、] をクリックし、 **ダッシュ ボード**します。 間の接続に注意してください **vnet01** と **vnet02** ここで確立されると、次に示すようにします。

    ![VNet のダッシュ ボード](..\virtual-network\media\virtual-networks-arm-asm-s2s\figure11.png)

5. 従来の VNet とその接続は従来のポータルから管理できますが、新しい Azure ポータルを使用することをお勧めします。 新しいポータルを開くには、https://portal.azure.com に移動します。
6. 新しいポータルで、クリックして **すべてを参照**, 、順にクリックして **仮想ネットワーク (クラシック)**, 、] をクリックし、 **vnet01**します。 通知、 **VPN 接続** 以下に示すウィンドウです。

    ![VNet のダッシュ ボード](..\virtual-network\media\virtual-networks-arm-asm-s2s\figure12.png)

### 手順 3: 接続をテストする

2 つの VNet が接続できたので、一方の VM からもう一方の VM に対して ping を実行することで、接続をテストしましょう。  一方の VM のファイアウォール設定を ICMP が使用できるように変更し、その VM に対してもう一方の VM から ping を実行する必要があります。 これを実行するには、次の手順に従ってください。

1. Azure ポータルからクリックして **すべてを参照**, 、順にクリックして **仮想マシン**, 、] をクリックし、 **VM02**します。
2.  **VM02** ブレードで、をクリックして **接続**します。 必要な場合はクリックして **開く** RDP ファイルを開く、ブラウザーのセキュリティ バナーでします。
3.  **リモート デスクトップ接続** ] ダイアログ ボックスをクリックして **接続**します。
4.  **Windows セキュリティ** ] ダイアログ ボックスで、VM のユーザー名とパスワードを入力します。 
5.  **リモート デスクトップ接続** ダイアログ ボックスで、をクリックして **はい**します。
6. 仮想マシンに接続した後 **サーバー マネージャー**, 、クリックして **ツール**, 、順にクリック **セキュリティが強化された Windows ファイアウォール**します。
7.  **セキュリティが強化された Windows ファイアウォール** ウィンドウで、をクリックして **受信の規則**します。
8.  **受信の規則** ボックスの一覧で、右クリックして **ファイルとプリンターの共有 (エコー要求 - icmpv4 受信)** ] をクリックし、 **規則の有効化**します。
9. タスク バーで、クリックして、 **Windows PowerShell** ] ボタンをクリックし、次のコマンドを実行します。

        ipconfig

        Connection-specific DNS Suffix  . : 4oxp4ce0c5rulb1iey4cdqhasg.gx.internal.cloudapp.net
        Link-local IPv6 Address . . . . . : fe80::8cea:a98a:5c48:4c58%12
        IPv4 Address. . . . . . . . . . . : 10.2.0.101
        Subnet Mask . . . . . . . . . . . : 255.255.255.0
        Default Gateway . . . . . . . . . : 10.2.0.101

10. VM の IP アドレスを書き留めます。 このシナリオで **10.2.0.101**します。 もう一方の VM からそのアドレスを ping することで、VNet 間の接続をテストします。
11. 左側のウィンドウで、Azure ポータルからクリックして **仮想マシン (クラシック)**, 、順にクリックして **VM01**, 、順にクリック **接続**します。 必要な場合はクリックして **開く** RDP ファイルを開く、ブラウザーのセキュリティ バナーでします。
12.  **リモート デスクトップ接続** ] ダイアログ ボックスをクリックして **接続**します。
13.  **Windows セキュリティ** ] ダイアログ ボックスで、VM のユーザー名とパスワードを入力します。 
14.  **リモート デスクトップ接続** ダイアログ ボックスで、をクリックして **はい**します。
15. リモートの仮想マシンのタスク バーで、クリックして、 **Windows PowerShell** ] ボタンをクリックし、次のコマンドを実行します。

        ping 10.2.0.101

        Reply from 10.2.0.101: bytes=32 time=32ms TTL=126
        Reply from 10.2.0.101: bytes=32 time=32ms TTL=126
        Reply from 10.2.0.101: bytes=32 time=32ms TTL=126
        Reply from 10.2.0.101: bytes=32 time=32ms TTL=126

## 次のステップ

- 詳細について [ARM のネットワーク リソース プロバイダー (NRP)](../resource-groups-networking.md)します。
- 方法に関する一般的なガイドラインを表示 [従来の VNet と ARM VNet の間の S2S VPN 接続を作成](../virtual-networks-arm-asm-s2s-howto.md)します。

