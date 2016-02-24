<properties 
   pageTitle="リソース マネージャーでの Azure CLI を使用した内部ロード バランサーの作成 | Microsoft Azure"
   description="リソース マネージャーで Azure CLI を使用して、内部ロード バランサーを作成する方法について説明します"
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="carolz"
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

# Azure CLI を使用した内部ロード バランサーの作成の概要

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)] [従来のデプロイ モデル](load-balancer-get-started-ilb-classic-cli.md)します。

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## 内部ロード バランサーを作成するために必要な項目

ロード バランサーをデプロイするには、次のオブジェクトを作成して構成する必要があります。

- フロント エンド IP 構成 - 受信ネットワーク トラフィックのプライベート IP アドレスを構成します。 

- バック エンド アドレス プール - ロード バランサーからトラフィックを受信するためのネットワーク インターフェイス (NIC) が含まれます。 

- 負荷分散規則 - バック エンド プール内のネットワーク トラフィックの受信ポートへの着信ネットワーク トラフィックのポートのマッピング規則が含まれます。

- 受信 NAT 規則 - ロード バランサーのパブリック ポートをバック エンド アドレス プール内の特定の仮想マシンのポートにマッピングする規則が含まれます。

- プローブ - バック エンド アドレス プールに関連する仮想マシンの可用性を確認するために使用する正常性プローブが含まれます。

Azure リソース マネージャーであるの詳細については、ロード バランサー コンポーネントを取得できます [ロード バランサーに対応する Azure リソース マネージャー](load-balancer-arm.md)します。

## リソース マネージャーを使用するための CLI のセットアップ

1. Azure CLI を初めて使用する場合は、次を参照してください。 [のインストールと Azure CLI の構成](xplat-cli.md) Azure アカウントとサブスクリプションを選択する時点までの指示に従います。

2. 実行、 **azure config モード** コマンドを次に示すように、リソース マネージャー モードに切り替えます。

        azure config mode arm

    予想される出力:

        info:    New mode is arm

## 内部ロード バランサーの作成手順 

次の手順では前述のシナリオに基づいて内部ロード バランサーを作成します。

### 手順 1. 

したがまだ実行していない場合は、最新バージョンをダウンロード [Azure コマンド ライン インターフェイス](https://azure.microsoft.com/downloads/)します。

### 手順 2. 

インストールしたら、アカウントを認証します。

    azure login

認証プロセスでは Azure サブスクリプションのユーザー名とパスワードが要求されます。

### 手順 3.

コマンド ツールを Azure リソース マネージャー モードに変更します。

    azure config mode arm

## リソース グループの作成

Azure リソース マネージャーのすべてのリソースは、リソース グループに関連付けられます。 リソース グループをまだ作成していない場合は、リソース グループを作成してください。

    azure group create <resource group name> <location>


## 内部ロード バランサー セットを作成します。 


### 手順 1. 

`azure network lb create` コマンドを使用して内部ロード バランサーを作成します。 次のシナリオでは、"nrprg" という名前のリソース グループが米国東部リージョンで作成されています。
    
    azure network lb create -n nrprg -l westus

>[AZURE.NOTE] 内部ロード バランサーの仮想ネットワークおよび仮想ネットワーク サブネットなどのすべてのリソースは、同じリソース グループと同じリージョンである必要があります。


### 手順 2. 

内部ロード バランサーのフロント エンド IP アドレスを作成します。 使用される IP アドレスは、仮想ネットワークのサブネットの範囲内にある必要があります。

    
    azure network lb frontend-ip create -g nrprg -l ilbset -n feilb -a 10.0.0.7 -e nrpvnetsubnet -m nrpvnet

使用されるパラメーター:

**-g** -リソース グループ 
**-l** - 内部ロード バランサー セットの名前
**-n** - フロント エンド IP の名前 
**-a** -サブネットの範囲内のプライベート IP アドレスです。
**-e** -サブネット名
**-m** -仮想ネットワーク名 

### 手順 3. 

バック エンド アドレス プールを作成します。 

    azure network lb address-pool create -g nrprg -l ilbset -n beilb

使用されるパラメーター:

**-g** -リソース グループ 
**-l** - 内部ロード バランサー セットの名前
**-n** - バック エンド アドレス プールの名前

フロント エンド IP アドレスとバック エンド アドレス プールを定義した後に、ロード バランサー規則や受信 NAT 規則を作成し、また正常性プローブをカスタマイズすることができます。

### 手順 4.


内部ロード バランサーのロード バランサー規則を作成します。 下のシナリオでは、コマンドによりロード バランサー規則を作成します。フロント エンド プールでポート 1433 をリッスンし、ロード バランス ネットワーク トラフィックをバックエンド アドレス プールにポート 1433 を使用して送信する規則になります。 

    azure network lb rule create -g nrprg -l ilbset -n ilbrule -p tcp -f 1433 -b 1433 -t feilb -o beilb

使用されるパラメーター:

**-g** -リソース グループ 
**-l** - 内部ロード バランサー セットの名前
**-n** - ロード バランサー規則の名前
**-p** -ルールに使用されるプロトコル
**-f** -ロード バランサーのフロント エンドでの着信ネットワーク トラフィックをリッスン ポート
**-b** バック エンド アドレス プール内のネットワーク トラフィックの受信ポート。

### 手順 5.

受信 NAT 規則を作成します。 受信 NAT 規則は、ロード バランサー内で特定の仮想マシン インスタンスに移動するエンドポイントの作成に使用されます。
下の例では、2 つの NAT 規則がリモート デスクトップ アクセス用に作成されました。

    azure network lb inbound-nat-rule create -g nrprg -l ilbset -n NATrule1 -p TCP -f 5432 -b 3389
    
    azure network lb inbound-nat-rule create -g nrprg -l ilbset -n NATrule2 -p TCP -f 5433 -b 3389

使用されるパラメーター:

**-g** -リソース グループ 
**-l** - 内部ロード バランサー セットの名前
**-n** - 受信 NAT 規則の名前
**-p** -ルールに使用されるプロトコル
**-f** -ロード バランサーのフロント エンドでの着信ネットワーク トラフィックをリッスン ポート
**-b** バック エンド アドレス プール内のネットワーク トラフィックの受信ポート。

### 手順 5. 

ロード バランサーの正常性プローブを作成します。 正常性プローブは、すべての仮想マシン インスタンスを確認し、ネットワーク トラフィックを送信できるかどうかを確認します。 プローブのチェックで失敗した仮想マシン インスタンスは、オンラインに戻り、プローブチェックが正常になるまで、ロード バランサーから削除されます。

    azure network lb probe create -g nrprg -l ilbset -n ilbprobe -p tcp -i 300 -c 4

**-g** -リソース グループ 
**-l** - 内部ロード バランサー セットの名前
**-n** - 正常性プローブの名前
**-p** -正常性プローブによって使用されるプロトコル
**-i** -プローブ間隔 (秒)
**-c** チェックの数。 

>[AZURE.NOTE] Microsoft Azure プラットフォームでは、さまざまな管理シナリオの静的パブリック ルーティング可能な IPv4 アドレスを使用します。 IP アドレスは 168.63.129.16 です。 この IP アドレスはファイアウォールによってブロックされないように設定しておく必要があります。ブロックされると、予期しない動作が発生する可能性があるためです。
>Azure 内部負荷分散については、この IP アドレスはロード バランサーからの監視プローブによって使用され、負荷分散セットでの仮想マシンの正常性状態が判別されます。 ネットワーク セキュリティ グループが、内部負荷分散セットで Azure の仮想マシンへのトラフィックを制限するために使用されている場合、または仮想ネットワーク サブネットに適用されている場合、168.63.129.16 からのトラフィックを許可するネットワーク セキュリティの規則が追加されていることを確認します。

## NIC の作成

NIC を作成し (あるいは、既存の NIC を変更し)、それを NAT 規則、ロード バランサー規則、プローブに関連付ける必要があります。

### 手順 1. 

という名前の NIC を作成する *lb nic1 する*, 、関連付けます、 *rdp1* NAT 規則、および *beilb* バック エンド アドレス プール。
    
    azure network nic create -g nrprg -n lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet -d "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" -e "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus

パラメーター:

- **-g** -リソース グループ名
- **-n** - NIC のリソースの名前
- **-サブネット名** - サブネットの名前 
- **-サブネット vnet 名** - 仮想ネットワークの名前
- **-d** /subscription/{subscriptionID/resourcegroups/< リソース グループ名 >/providers/Microsoft.Network/loadbalancers/< ロード バランサー名 >/backendaddresspools/< 名前の-、-バックエンド-プール > 始まり - バック エンド プールのリソースの ID - 
- **-e** - NIC のリソースに関連付けられる、NAT ルールの ID - から始まります/subscriptions/###/resourcegroups/< リソース グループ名 >/providers/Microsoft.Network/loadBalancers/< ロード バランサー名 >/inboundNatRules/< nat 規則名 >


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

という名前の NIC を作成する *lb nic2 する*, 、関連付けます、 *rdp2* NAT 規則、および *beilb* バック エンド アドレス プール。

    azure network nic create -g nrprg -n lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet -d "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" -e "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus

### 手順 3. 

仮想マシン (VM) という名前の作成 *DB1*, 、という名前の NIC に関連付けると *lb nic1 する*です。 ストレージ アカウントと呼ばれる *web1nrp* が次のコマンドを実行する前に作成されました。

    azure vm create --resource-group nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

>[AZURE.IMPORTANT] ロード バランサー内の Vm は、同じ可用性セット内にある必要があります。 可用性セットを作成するには、`azure availset create` を使用します。 

### 手順 4.

仮想マシン (VM) という名前の作成 *DB2*, 、という名前の NIC に関連付けると *lb nic2 する*です。 ストレージ アカウントと呼ばれる *web1nrp* が次のコマンドを実行する前に作成されました。

    azure vm create --resource-group nrprg --name DB2 --location eastus --vnet- name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## ロード バランサーの削除 


ロード バランサーを削除するには、次のコマンドを使用します。

    azure network lb delete -g nrprg -n ilbset 

ここで **nrprg** はリソース グループと **ilbset** 内部ロード バランサー名を指定します。


## 次のステップ

[ソース IP アフィニティを使用したロード バランサー分散モードの構成](load-balancer-distribution-mode.md)

[ロード バランサーのアイドル TCP タイムアウト設定の構成](load-balancer-tcp-idle-timeout.md)


