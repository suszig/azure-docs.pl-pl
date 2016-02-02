<properties 
    pageTitle="Linux と Mac で SSH を使用する | Microsoft Azure" 
    description="Azure で、リソース マネージャーと従来のデプロイメント モデル用に、Linux と Mac の SSH 鍵を生成し、使用します。" 
    services="virtual-machines" 
    documentationCenter="" 
    authors="squillace" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2015" 
    ms.author="rasquill"/>


# Azure 上の Linux または Mac における SSH の使用方法

> [AZURE.SELECTOR]
- [Windows](../articles/virtual-machines/virtual-machines-windows-use-ssh-key.md)
- [Linux/Mac](../articles/virtual-machines/virtual-machines-linux-use-ssh-key.md)


このトピックでは、**ssh-keygen** と **openssl** を Linux や Mac で使用し、**ssh-rsa** 形式または **.pem** 形式のファイルを作成して使用し、Linux を基盤とする Azure VM と安全に通信する方法について紹介します。 リソース マネージャーのデプロイメント モデルによる Linux ベースの Azure Virtual Machines の作成は新しいデプロイメントの場合に推奨され、*ssh-rsa* タイプの公開鍵ファイルや文字列が使われます (デプロイメント クライアントにもよります)。  [プレビュー ポータル](https://portal.azure.com) だけを受け入れる現在、 **ssh rsa** クラシック、またはリソース マネージャーの展開の書式指定文字列。

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]
>Azure での Linux Vm を安全にやり取りするために Windows コンピューターで使用するファイルのこれらの型を作成するを参照してください。 [を使用して SSH キーの Windows で](virtual-machines-windows-use-ssh-key.md)します。

## 必要なファイル

基本的な ssh セットアップを Azure に、 **ssh rsa** 2048 ビットのパブリックおよびプライベートのキー ペア (既定では、 **で問題** としてこれらのファイルを格納 **~/.ssh/id_rsa** と **~/.ssh/id-rsa.pub** 、既定の設定を変更しない限り) と同様に、 `.pem` から生成されたファイル、 **id \_rsa** 従来のポータルの従来のデプロイ モデルで使用するための秘密キー ファイルです。

次にデプロイメント シナリオとそれぞれのシナリオで使用されるファイルの種類を紹介します。

1. **ssh rsa** キーに使用した展開に必要な [プレビュー ポータル](https://portal.azure.com), 配置モデルに関係なく、します。
2. .pem ファイルを使用して Vm を作成するために必要な [旧ポータル](https://manage.windowsazure.com)します。 .pem ファイルが使用する従来の展開でサポートされても、 [AZURE-CLI](xplat-cli-install.md)します。

## SSH で使用する鍵の作成

Azure は、シナリオに応じて、2048 ビットの **ssh-rsa** 形式鍵ファイルまたは同等の .pem ファイルを必要とします。 既にそのようなファイルがある場合、Azure VM の作成時に公開鍵ファイルを渡します。

ファイルを作成する必要がある場合:

1. **ssh-keygen** と **openssl** の実装が最新のものであることを確認してください。 これはプラットフォームによって異なります。

    - For Mac, するを参照してください、 [Apple 製品のセキュリティ web サイト](https://support.apple.com/HT201222) し、必要に応じて、適切な更新プログラムを選択します。
    - Ubuntu、Debian、Mint など、Debian 基盤の Linux 製品の場合:

            sudo apt-get update ssh-keygen
            sudo apt-get update openssl

    - CentOS や Oracle Linux など、RPM 基盤の Linux 製品の場合:

            sudo yum update ssh-keygen
            sudo yum update openssl

    - SLES と OpenSUSE の場合:

            sudo zypper update ssh-keygen
            sudo zypper update openssl


2. 使用 **で問題** 2048年ビット RSA を作成するパブリックとプライベート キーのファイル、および特定の場所や、ファイルに特定の名前がある場合に、既定の場所と名前がそのまま使用 `~/.ssh/id_rsa`します。 基本的なコマンド:

        ssh-keygen -t rsa -b 2048 

    通常、**ssh-keygen** の実装でコメントが追加されます。多くの場合、コンピューターのユーザー名とホスト名です。 使用して、特定のコメントを指定する、 `-c` オプション。

3. .Pem ファイルを作成、 `~/.ssh/id_rsa` ファイルを使用すると、従来のポータルと連携します。 次のように **openssl** を使用します。

        openssl req -x509 -key ~/.ssh/id_rsa -nodes -days 365 -newkey rsa:2048 -out myCert.pem

    別の秘密キー ファイルから .pem ファイルを作成する場合は、変更、 `-キー` 引数。

> [AZURE.NOTE] 従来のデプロイメント モデルでデプロイされたサービスを管理する場合、**.cer** 形式ファイルを作成し、ポータルにアップロードすることもあります。ただし、その場合、**ssh** は使われず、この記事の主題である Linux VMS に接続しません。 Linux や Mac のそれらのファイルを作成するには、次のように入力します。
<br />
> openssl.exe x509-outform der-myCert.pem で-myCert.cer アウト

.pem ファイルを DER 形式でエンコードされた X509 証明書ファイルに変換するには。

## 既にある SSH 鍵を使用する

Ssh rsa を使用することができます (`.pub`) キーのすべての新機能、リソース マネージャーの配置モデルと、プレビュー ポータルで特に; を作成する必要があります、 `.pem` クラシック ポータルを使用する必要がある場合、キー ファイルです。

## 公開鍵ファイルで VM を作成する

必要なファイルを作成したら、さまざまな方法で VM を作成し、公開/秘密鍵の交換で安全に接続できます。 ほとんどすべての状況で、特にリソース マネージャー デプロイメントの利用で、ssh 鍵ファイル パスまたはファイルのコンテンツを文字列として求められたら、.pub ファイルを渡します。

### 例: id_rsa.pub ファイルで VM を作成する

最も一般的な使用方法は VM を強制的に作成するか、VM を作成するためのテンプレートをアップロードするときです。 新しいを作成する次のコード例を示しています、パブリックのファイル名を渡すことによって Azure で Linux VM をセキュリティで保護された (この場合は、既定で `~/.ssh/id_rsa` ファイル) を `azure 仮想マシンの作成` コマンドです。 (他の引数は前に作成されました。)

    azure vm create \
    --nic-name testnic \
    --public-ip-name testpip \
    --vnet-name testvnet \
    --vnet-subnet-name testsubnet \
    --storage-account-name computeteststore 
    --image-urn canonical:UbuntuServer:14.04.3-LTS:latest \
    --username ops \
    -ssh-publickey-file ~/.ssh/id_rsa \
    testrg testvm westeurope linux

次の例では、使用する、 **ssh rsa** リソース マネージャー テンプレートと、ユーザー名との内容で保護されている Ubuntu VM を作成する Azure CLI を使用して形式、 `~/.ssh/id_rsa.pub` を文字列として。 (この場合、公開鍵の文字列が読みやすいように短くされます。)

    azure group deployment create \
    --resource-group test-sshtemplate \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
    --name mysshdeployment
    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    testnewStorageAccountName: testsshvmtemplate3
    adminUserName: ops
    sshKeyData: ssh-rsa AAAAB3NzaC1yc2EAAAADAQA+/L+rHIjz+nXTzxApgnP+iKDZco9 user@macbookpro
    dnsNameForPublicIP: testsshvmtemplate
    location: West Europe
    vmName: sshvm
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "mysshdeployment"
    + Waiting for deployment to complete
    data:    DeploymentName     : mysshdeployment
    data:    ResourceGroupName  : test-sshtemplate
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : 2015-10-08T00:12:12.2529678Z
    data:    Mode               : Incremental
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    Name                   Type    Value
    
    data:    newStorageAccountName  String  testtestsshvmtemplate3
    data:    adminUserName          String  ops
    data:    sshKeyData             String  ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDAkek3P6V3EhmD+xP+iKDZco9 user@macbookpro
    data:    dnsNameForPublicIP     String  testsshvmtemplate
    data:    location               String  West Europe
    data:    vmSize                 String  Standard_A2
    data:    vmName                 String  sshvm
    data:    ubuntuOSVersion        String  14.04.2-LTS
    info:    group deployment create command OK

### 例: .pem ファイルで VM を作成する

従来のいずれかのポータルまたは従来のデプロイ モードにし、.pem ファイルを使用でき、 `azure 仮想マシンの作成`, 、次の例。

    azure vm create \
    -l "West US" -n testpemasm \
    -P -t myCert.pem -e 22 \
    testpemasm \
    b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_3-LTS-amd64-server-20150908-en-us-30GB \
    ops
    info:    Executing command vm create
    warn:    --vm-size has not been specified. Defaulting to "Small".
    + Looking up image b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_3-LTS-amd64-server-20150908-en-us-30GB
    + Looking up cloud service
    info:    cloud service testpemasm not found.
    + Creating cloud service
    + Retrieving storage accounts
    + Configuring certificate
    + Creating VM
    info:    vm create command OK

## VM に接続する

**ssh** コマンドは、ログオンするためのユーザー名、コンピューターのネットワーク アドレス、アドレスに接続するポート、その他さまざまな特殊バリエーションを受け取ります。 (詳細については **ssh**, 、これで十分かもしれません [Secure Shell の記事](https://en.wikipedia.org/wiki/Secure_Shell))

サブドメインとデプロイメント場所を指定しているだけの場合、リソース マネージャー デプロイメントの一般的な使用方法は次のようになります。

    ssh user@subdomain.westus.cloudapp.azure.com -p 22

あるいは、従来のデプロイメント クラウド サービスに接続している場合、使用するアドレスは次のようになります。

    ssh user@subdomain.cloudapp.net -p 22

アドレス形式は変更されることがあるので (いつでも IP アドレスを使用できます。あるいは、カスタムのドメイン名を割り当てることがあります。)、自分の Azure VM のアドレスを見つける必要があります。

### 従来のデプロイメントで Azure VM SSH を見つける

使用してバーチャル マシンと従来のデプロイ モデルで使用するアドレスを検出する、 `azure vm のショー` VM 名でコマンド。

    azure vm show testpemasm
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "testpemasm.cloudapp.net"
    data:    Location "West US"
    data:    VMName "testpemasm"
    data:    IPAddress "100.116.160.154"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Small"
    data:    Image "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_3-LTS-amd64-server-20150908-en-us-30GB"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "testpemasm-testpemasm-0-201510102050230517"
    data:    OSDisk mediaLink "https://portalvhds4blttsxgjj1rf.blob.core.windows.net/vhd-store/testpemasm-2747c9c432b043ff.vhd"
    data:    OSDisk sourceImageName "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_3-LTS-amd64-server-20150908-en-us-30GB"
    data:    OSDisk operatingSystem "Linux"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "40.83.178.221"
    data:    VirtualIPAddresses 0 name "testpemasmContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 localPort 22
    data:    Network Endpoints 0 name "ssh"
    data:    Network Endpoints 0 port 22
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "40.83.178.221"
    data:    Network Endpoints 0 enableDirectServerReturn false
    info:    vm show command OK

### リソース マネージャー デプロイメントで Azure SSH アドレスを見つける

    azure vm show testrg testvm
    info:    Executing command vm show
    + Looking up the VM "testvm"
    + Looking up the NIC "testnic"
    + Looking up the public ip "testpip"

ネットワーク プロファイル セクションを調べます。

    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testnic
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-21-8E-AE
    data:          Provisioning State        :Succeeded
    data:          Name                      :testnic
    data:          Location                  :westeurope
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.115.48.189
    data:            FQDN                    :testsubdomain.westeurope.cloudapp.azure.com
    data:
    data:    Diagnostics Instance View:
    info:    vm show command OK

VM の作成時に、既定の SSH ポート 22 を使用しない場合のポートがあるを検出できます受信の規則と、 `azure ネットワーク nsg の表示` コマンドを次の例のようにします。

    azure network nsg show testrg testnsg
    info:    Executing command network nsg show
    + Looking up the network security group "testnsg"
    data:    Id                              : /subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testnsg
    data:    Name                            : testnsg
    data:    Type                            : Microsoft.Network/networkSecurityGroups
    data:    Location                        : westeurope
    data:    Provisioning state              : Succeeded
    data:    Security group rules:
    data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
    data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
    data:    testnsgrule                    *                  *            *               22                Tcp       Inbound    Allow   1000
    data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000
    data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001
    data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500
    data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000
    data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001
    data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500
    info:    network nsg show command OK

### 例: .pem 鍵と従来のデプロイメントを使用した SSH セッションの出力

作成された .pem ファイルを使用して VM を作成したかどうか、 `~/.ssh/id_rsa` ファイルを直接できます ssh を使用する VM にします。 操作を実行すると証明書のハンドシェイクは、秘密キーを使用する、ことに注意してください `~/.ssh/id_rsa`します。 これは次の例のようになります。

    ssh ops@testpemasm.cloudapp.net -p 22
    The authenticity of host 'testpemasm.cloudapp.net (40.83.178.221)' can't be established.
    RSA key fingerprint is dc:bb:e4:cc:59:db:b9:49:dc:71:a3:c8:37:36:fd:62.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added 'testpemasm.cloudapp.net,40.83.178.221' (RSA) to the list of known hosts.
    Saving password to keychain failed
    Identity added: /Users/rasquill/.ssh/id_rsa (/Users/rasquill/.ssh/id_rsa)
    Welcome to Ubuntu 14.04.3 LTS (GNU/Linux 3.19.0-28-generic x86_64)
    
    * Documentation:  https://help.ubuntu.com/
    
    System information as of Sat Oct 10 20:53:08 UTC 2015
    
    System load: 0.52              Memory usage: 5%   Processes:       80
    Usage of /:  45.3% of 1.94GB   Swap usage:   0%   Users logged in: 0
    
    Graph this data and manage this system at:
        https://landscape.canonical.com/
    
    Get cloud support with Ubuntu Advantage Cloud Guest:
        http://www.ubuntu.com/business/services/cloud
    
    0 packages can be updated.
    0 updates are security updates.
    
    The programs included with the Ubuntu system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.
    
    Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
    applicable law.

## 接続に問題がある場合

検索候補を読み取ることができます [SSH 接続のトラブルシューティング](virtual-machines-troubleshoot-ssh-connections.md) かどうかの状況を解決することができますヘルプを参照しています。

## 次のステップ

これで VM に接続できたので、選択したディストリビューションを必ず更新してから使用を続けます。





