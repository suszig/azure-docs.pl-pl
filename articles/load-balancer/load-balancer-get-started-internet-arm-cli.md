<properties 
   pageTitle="Azure CLI を使用したリソース マネージャーでのインターネットに接続するロード バランサーの作成 | Microsoft Azure"
   description="Azure CLI を使用して、リソース マネージャーでインターネットに接続するロード バランサーを作成する方法について説明します"
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="11/16/2015"
   ms.author="joaoma" />

# Azure CLI を使用したインターネットに接続するロード バランサーの作成の開始

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] この記事では、リソース マネージャーの配置モデルについて説明します。 こともできます [インターネットへの従来のデプロイを使用してロード バランサーを作成する方法について](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

ここでは、ロード バランサーを作成するために実行する必要のある一連の作業を個別に取り上げ、目的を達成するために実行する事柄を詳しく説明します。


## インターネットに接続するロード バランサーを作成するために必要な項目

ロード バランサーをデプロイするには、次のオブジェクトを作成して構成する必要があります。

- フロント エンド IP 構成 - 受信ネットワーク トラフィックのパブリック IP アドレスが含まれます。 

- バック エンド アドレス プール - ロード バランサーからネットワーク トラフィックを受信する、仮想マシンのネットワーク インターフェイス (NIC) が含まれます。 

- 負荷分散規則 - ロード バランサーのパブリック ポートをバック エンド アドレス プール内のポートにマッピングする規則が含まれます。

- 受信 NAT 規則 - ロード バランサーのパブリック ポートをバック エンド アドレス プール内の特定の仮想マシンのポートにマッピングする規則が含まれます。

- プローブ - バック エンド アドレス プール内の仮想マシン インスタンスの可用性を確認するために使用する正常性プローブが含まれます。

Azure リソース マネージャーであるの詳細については、ロード バランサー コンポーネントを取得できます [ロード バランサーに対応する Azure リソース マネージャー](load-balancer-arm.md)します。

## リソース マネージャーを使用するための CLI のセットアップ

1. Azure CLI を初めて使用する場合は、次を参照してください。 [のインストールと Azure CLI の構成](xplat-cli.md) Azure アカウントとサブスクリプションを選択する時点までの指示に従います。

2. 実行、 **azure config モード** コマンドを次に示すように、リソース マネージャー モードに切り替えます。

        azure config mode arm

    予想される出力:

        info:    New mode is arm

## 仮想ネットワークと、フロント エンド IP プールのパブリック IP アドレスの作成

### 手順 1.

仮想ネットワーク (VNet) の名前付きの作成 *nrpvnet に追加して* という名前のリソース グループを使用して米国東部の場所に *NRPRG*します。

    azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16

という名前のサブネットを作成 *NRPVnetSubnet* 10.0.0.0/24」の CIDR ブロックと *nrpvnet に追加して*します。 

    azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24

### 手順 2.

という名前のパブリック IP アドレスを作成する *NRPPublicIP* DNS 名を持つフロント エンド IP プールによって使用される *loadbalancernrp.eastus.cloudapp.azure.com*します。 次のコマンドでは、静的な割り当てタイプと 4 分のアイドル タイムアウトを使用しています。

    azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4


>[AZURE.IMPORTANT] ロード バランサーは、FQDN として、パブリック IP のドメインのラベルを使用します。 これはロード バランサー FQDN としてクラウド サービスを使用する従来のデプロイメントからの変更点です。 
>この例では、FQDN がなります *loadbalancernrp.eastus.cloudapp.azure.com*します。

## ロード バランサーの作成

次の例では、次のコマンドはという名前のロード バランサーを作成 *NRPlb* で、 *NRPRG* 内のリソース グループ、 *米国東部* Azure の場所。  

    azure network lb create NRPRG NRPlb eastus

## フロント エンド IP プールとバック エンド アドレス プールの作成

次の例では、ロード バランサーへの受信ネットワーク トラフィックを受信するフロント エンド IP プールと、負荷分散されたネットワーク トラフィックをフロント エンド プールが送信するバックエンド IP プールを作成します。

### 手順 1 

前の手順で作成したパブリック IP とロード バランサーを関連付けてフロント エンド IP プールを作成します。 

    azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip

### 手順 2 

フロント エンド IP プールから受信トラフィックを受け取るために使用するバック エンド アドレス プールをセットアップします。 

    azure network lb address-pool create NRPRG NRPlb NRPbackendpool

## LB 規則、NAT 規則、およびプローブの作成

次の例では、以下の項目が作成されます。

- ポート 3441 ～ 3389 のすべての受信トラフィックを変換する NAT 規則。<sup>1</sup>
- ポート 3442 ～ 3389 のすべての受信トラフィックを変換する NAT 規則。
- バックエンド プールのアドレスでポート 80 ～ 80 に入ってくるすべてのトラフィックを分散するロード バランサー規則。
- という名前のページ上のヘルス状態を確認するプローブ ルール *HealthProbe.aspx*します。

<sup>1</sup> NAT 規則は、ロード バランサーの背後にある特定の仮想マシン インスタンスに関連付られます。 ポート 3341 への着信ネットワーク トラフィックは、以下の例の NAT 規則に関連付けられている特定の仮想マシンのポート 3389 に送信されます。 NAT 規則、UDP または TCP のプロトコルを選択する必要があります。 両方のプロトコルを、同じポートに割り当てることはできません。

### 手順 1

NAT 規則を作成します。

    azure network lb inbound-nat-rule create -g nrprg -l nrplb -n rdp1 -p tcp -f 3441 -b 3389
    azure network lb inbound-nat-rule create -g nrprg -l nrplb -n rdp2 -p tcp -f 3442 -b 3389

パラメーター:

- **-g** -リソース グループ名
- **-l** -ロード バランサーの名前 
- **-n** - リソースの名前かどうかは、nat ルール、プローブまたは lb ルール。
- **-p** -プロトコル (できる TCP または UDP)  
- **-f** - 前のバックエンドを使用するポート (プローブ コマンドは、プローブ パスを定義する-f を使用)
- **-b** 戻る - 使用されるポート

### 手順 2.

ロード バランサー規則を作成します。

    azure network lb rule create nrprg nrplb lbrule -p tcp -f 80 -b 80 -t NRPfrontendpool -o NRPbackendpool
### 手順 3.

正常性プローブを作成します。

    azure network lb probe create -g nrprg -l nrplb -n healthprobe -p "http" -o 80 -f healthprobe.aspx -i 15 -c 4

    
    

**-g** -リソース グループ 
**-l** - ロード バランサー セットの名前
**-n** - 正常性プローブの名前
**-p** -正常性プローブによって使用されるプロトコル
**-i** -プローブ間隔 (秒)
**-c** チェックの数。 

### 手順 4.

設定を確認します。

    azure network lb show nrprg nrplb

予想される出力:

    info:    Executing command network lb show
    + Looking up the load balancer "nrplb"
    + Looking up the public ip "NRPPublicIP"    
    data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
    data:    Name                            : nrplb
    data:    Type                            : Microsoft.Network/loadBalancers
    data:    Location                        : eastus
    data:    Provisioning State              : Succeeded
    data:    Frontend IP configurations:
    data:      Name                          : NRPfrontendpool
    data:      Provisioning state            : Succeeded
    data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
    data:      Public IP allocation method   : Static
    data:      Public IP address             : 40.114.13.145
    data:
    data:    Backend address pools:
    data:      Name                          : NRPbackendpool
    data:      Provisioning state            : Succeeded
    data:
    data:    Load balancing rules:
    data:      Name                          : HTTP
    data:      Provisioning state            : Succeeded
    data:      Protocol                      : Tcp
    data:      Frontend port                 : 80
    data:      Backend port                  : 80
    data:      Enable floating IP            : false
    data:      Idle timeout in minutes       : 4
    data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
    data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
    data:
    data:    Inbound NAT rules:
    data:      Name                          : rdp1
    data:      Provisioning state            : Succeeded
    data:      Protocol                      : Tcp
    data:      Frontend port                 : 3441
    data:      Backend port                  : 3389
    data:      Enable floating IP            : false
    data:      Idle timeout in minutes       : 4
    data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
    data:
    data:      Name                          : rdp2
    data:      Provisioning state            : Succeeded
    data:      Protocol                      : Tcp
    data:      Frontend port                 : 3442
    data:      Backend port                  : 3389
    data:      Enable floating IP            : false
    data:      Idle timeout in minutes       : 4
    data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
    data:
    data:    Probes:
    data:      Name                          : healthprobe
    data:      Provisioning state            : Succeeded
    data:      Protocol                      : Http
    data:      Port                          : 80
    data:      Interval in seconds           : 15
    data:      Number of probes              : 4
    data:
    info:    network lb show command OK

## NIC の作成

NIC を作成し (あるいは、既存の NIC を変更し)、それを NAT 規則、ロード バランサー規則、プローブに関連付ける必要があります。

### 手順 1. 

という名前の NIC を作成する *lb nic1 する*, 、関連付けます、 *rdp1* NAT 規則、および *NRPbackendpool* バック エンド アドレス プール。
    
    azure network nic create -g nrprg -n lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet -d "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" -e "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus

パラメーター:

- **-g** -リソース グループ名
- **-n** - NIC のリソースの名前
- **-サブネット名** - サブネットの名前 
- **-サブネット vnet 名** - 仮想ネットワークの名前
- **-d** /subscription/{subscriptionID/resourcegroups/< リソース グループ名 >/providers/Microsoft.Network/loadbalancers/ < ロード バランサー名 >/backendaddresspools/< 名前の-、-バックエンド-プール > 始まり - バック エンド プールのリソースの ID - 
- **-e** - NIC のリソースに関連付けられる、NAT ルールの ID - から始まります/subscriptions/###/resourcegroups/< リソース グループ名 >/providers/Microsoft.Network/loadBalancers/ < ロード バランサー名 >/inboundNatRules/< nat 規則名 >


予想される出力:

    info:    Executing command network nic create
    + Looking up the network interface "lb-nic1-be"
    + Looking up the subnet "nrpvnetsubnet"
    + Creating network interface "lb-nic1-be"
    + Looking up the network interface "lb-nic1-be"
    data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    data:    Name                            : lb-nic1-be
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : eastus
    data:    Provisioning state              : Succeeded
    data:    Enable IP forwarding            : false
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 10.0.0.4
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
    data:      Load balancer backend address pools
    data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
    data:      Load balancer inbound NAT rules:
    data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
    data:
    info:    network nic create command OK

### 手順 2.

という名前の NIC を作成する *lb nic2 する*, 、関連付けます、 *rdp2* NAT 規則、および *NRPbackendpool* バック エンド アドレス プール。

    azure network nic create -g nrprg -n lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet -d "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" -e "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus

### 手順 3. 

仮想マシン (VM) という名前の作成 *web1*, 、という名前の NIC に関連付けると *lb nic1 する*です。 ストレージ アカウントと呼ばれる *web1nrp* が次のコマンドを実行する前に作成されました。

    azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

>[AZURE.IMPORTANT] ロード バランサー内の Vm は、同じ可用性セット内にある必要があります。 可用性セットを作成するには、`azure availset create` を使用します。 

出力は次のようになります。

    info:    Executing command vm create
    + Looking up the VM "web1"
    Enter username: azureuser
    Enter password for azureuser: *********
    Confirm password: *********
    info:    Using the VM Size "Standard_A1"
    info:    The [OS, Data] Disk or image configuration requires storage account
    + Looking up the storage account web1nrp
    + Looking up the availability set "nrp-avset"
    info:    Found an Availability set "nrp-avset"
    + Looking up the NIC "lb-nic1-be"
    info:    Found an existing NIC "lb-nic1-be"
    info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
    info:    This is a NIC without publicIP configured
    + Creating VM "web1"
    info:    vm create command OK

>[AZURE.NOTE] 情報メッセージ **これは、構成されている publicIP せず NIC** NIC のロード バランサーのパブリック IP アドレスを使用してインターネットに接続するロード バランサーの作成後に想定します。 

 *Lb nic1 する* に NIC が関連付けられている、 *rdp1* NAT 規則の場合に接続できる *web1* RDP を使用して、ロード バランサー上のポート 3441 経由します。

### 手順 4.

仮想マシン (VM) という名前の作成 *web2*, 、という名前の NIC に関連付けると *lb nic2 する*です。 ストレージ アカウントと呼ばれる *web1nrp* が次のコマンドを実行する前に作成されました。

    azure vm create --resource-group nrprg --name web2 --location eastus --vnet-    name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## 既存のロード バランサーの更新

既存のロード バランサーを参照する規則を追加できます。 次の例では、既存のロード バランサーに新しいロード バランサー規則を追加 **NRPlb**

    azure network lb rule create -g nrprg -l nrplb -n lbrule2 -p tcp -f 8080 -b 8051 -t frontendnrppool -o NRPbackendpool

パラメーター:

**-g** -リソース グループ名<br>
**-l** -ロード バランサーの名前<BR> 
**-n** -ロード バランサー ルール名<BR>
**-p** -プロトコル<BR>
**-f** - 前のバックエンド ポート<BR>
**-b** - 戻るポート<BR>
**-t** -フロント エンド プール名<BR>
**-b** - 戻るエンド プール名<BR>

## ロード バランサーの削除 


ロード バランサーを削除するには、次のコマンドを使用します。

    azure network lb delete -g nrprg -n nrplb 

ここで **nrprg** リソース グループと **nrplb** ロード バランサー名を指定します。

## 次のステップ

[内部ロード バランサーの構成の開始](load-balancer-internal-getstarted.md)

[ロード バランサー分散モードの構成](load-balancer-distribution-mode.md)

[ロード バランサーのアイドル TCP タイムアウト設定の構成](load-balancer-tcp-idle-timeout.md)


