<properties 
   pageTitle="Azure ネットワーク リソース プロバイダーのパブリック IP アドレスとプライベート IP アドレス | Microsoft Azure"
   description="IP アドレスをパブリックとプライベートの Azure リソース マネージャーでネットワーク リソース プロバイダーについてください。"
   services="virtual-network"
   documentationCenter="na"
   authors="joaoma"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/07/2015"
   ms.author="joaoma" />


# Azure Virtual Network の IP アドレス

この記事では、Virtual Machines、Load Balancer、VPN Gateway、Application Gateway の IP アドレスの指定について説明します。

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)] .

## パブリック IP アドレス

パブリック IP アドレスは、インターネットや他の Azure の公開されたサービスと通信する Azure のリソースを許可するように [Azure Redis Cache](https://azure.microsoft.com/services/cache), 、[Azure Event Hubs](https://azure.microsoft.com/services/event-hubs)します。 パブリック IP アドレスは、独立したリソースであり、さまざまな種類のように Azure のリソースに関連付けられていることができます [仮想マシン](virtual-machines-about.md) (VM) [ロード バランサー](load-balancer-overview.md) (LB)。

### パブリック IP アドレスの割り当て方法

パブリック IP アドレスは、特定の場所で提供される IP アドレスのプールから割り当てられます。 Microsoft Azure データ センターで使用される IP アドレス範囲の完全なリストで公開されている [Azure Datacenter の IP 範囲](https://www.microsoft.com/download/details.aspx?id=41653)します。

IP アドレスをパブリック IP リソースに割り当てる方法には、*動的*と*静的*の 2 種類があります。 既定の割り当て方法は*動的*です。 *動的*な割り当て方法では、IP アドレスは*パブリック IP アドレス リソース*の作成時には**割り当てられません**。 *パブリック IP アドレス リソース*に関連付けられたリソース (VM やロード バランサーなど) を作成または開始すると、IP アドレスが IP アドレスのプールから割り当てられます。 この IP アドレスは、関連付けられたリソースを削除または停止すると、割り当てが解除され、使用可能なプールに解放されます。

*静的*な割り当て方法を使用する場合、IP アドレスは、*パブリック IP アドレス リソース*の作成時に割り当てられます。 この場合、関連付けられたリソースの状態に関係なく、同じ IP アドレスが割り当てられたままになります。 *パブリック IP アドレス リソース*を削除したときと割り当て方法を*動的*に変更したときのみ、IP アドレスは使用可能なプールに開放されます。

### DNS の解決

パブリック IP リソースに対して DNS ドメイン名のラベルを指定できます。これによって、対応する DNS エントリが Azure DNS サーバーに作成されます。 対応する FQDN *ドメイン名ラベル*.*場所*.cloudapp.azure.com は、パブリック IP リソースに割り当てられた IP アドレスにグローバルに解決されます。

以下で、パブリック IP アドレスに関連付けることができるさまざまな種類のリソースについて説明します。

### 仮想マシン

関連付けられているパブリック IP アドレス、 [仮想マシン](virtual-machines-about.md) (VM) ネットワーク インターフェイス カード (NIC) を使用します。 関連付けられている NIC の数に関係なく、各 VM に割り当てることができるパブリック IP アドレスは 1 つのみです。 VM の NIC には、*動的に*割り当てられる*パブリック IP アドレス リソース*のみを関連付けることができます。 複数の NIC を備える VM の場合、*パブリック IP アドレス リソース*は、プライマリ NIC のみに関連付けることができます。

### Azure Load Balancer

パブリック IP アドレスの関連付けできる、 [Azure ロード バランサー](load-balancer-overview.md) として機能する負荷分散された仮想 IP アドレス (VIP) 到達可能なインターネット経由で (lb による) フロント エンド構成します。 LB には、*動的*および*静的*の両方のパブリック IP リソースを関連付けることができます。 複数の関連付けを行うことが *のパブリック IP アドレス リソース* 、LB フロント エンドの構成を有効にする複数の SSL ベースの web サイトでのマルチ テナント環境のようなシナリオです。

### Application Gateway

パブリック IP アドレスの関連付けできる、 [Azure Application Gateway](application-gateway-introduction.md) フロント エンド構成か、到達可能なインターネットを使用して構成を負荷分散仮想 IP (VIP)。 現在のところ、Application Gateway には、*動的*に割り当てられるパブリック IP リソースのみを関連付けることができます。 ただし、複数のパブリック IP アドレスに関連付けることができます。

### VPN Gateway

関連付けられるパブリック IP アドレスが必要な [Azure VPN Gateway](vpn-gateway-about-vpngateways.md) IP 構成、Azure の仮想ネットワークと内部設置型ネットワークまたは別の Azure 仮想ネットワークの間の VPN 接続を作成するプロセスです。 現在のところ、VPN Gateway には、*動的*に割り当てられるパブリック IP リソースのみを関連付けることができます。

### 早見表

| リソース| 静的割り当て| 動的割り当て| 複数の IP アドレス|
|---|---|---|---|
| 仮想マシン (VM)/ネットワーク インターフェイス カード (NIC)| あり| いいえ| なし|
| Load Balancer のフロント エンド| あり| あり| あり|
| Application Gateway のフロント エンド| あり| なし| あり|
| VPN Gateway| あり| いいえ| なし|

## プライベート IP アドレス

プライベート IP アドレスを使用すると、インターネットから到達可能な IP アドレスを使用せずに、仮想ネットワーク内のリソース間で通信できるようになります。 この IP アドレスは、仮想ネットワーク内のサブネットのアドレス範囲から割り当てます。

### プライベート IP アドレスの割り当て方法

プライベート IP アドレスを割り当てる方法には、*動的*と*静的*の 2 種類があります。 既定の割り当て方法は*動的*です。IP アドレスは DHCP を使用して割り当てられます。 また、割り当て方法を*静的*と明示的に指定して、IP アドレスを指定することもできます。 この場合、IP アドレスがサブネットのアドレスの範囲内にあり、自由である (他のリソースに割り当てられていない) 限り、リソースには指定した IP アドレスが割り当てられます。

さまざまな種類のリソースにプライベート IP アドレスを割り当てることができます。 両方の割り当て方法 (*静的*と*動的*) が、以下のすべての種類のリソースに対して機能します。

### 仮想マシン

ネットワーク インターフェイス カード (NIC) に、プライベート IP アドレスが割り当てられている、 [仮想マシン](virtual-machines-about.md) (VM)。 各 VM は、VM に関連付けられた NIC と同数のプライベート IP アドレスを持つことができます。

#### 内部 DNS ホスト名の解決

すべての Azure Vm が構成されて [Azure で管理された DNS サーバー](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution), の作成時に明示的に指定されていない限り、します。 Azure で管理される DNS サーバーを使用する場合は、DNS レコードが自動的に作成されます。このレコードを使用して、VM のホスト名が VM のプライベート IP アドレスに解決されます。 複数の NIC を備える VM の場合、ホスト名はプライマリ NIC のプライベート IP アドレスに解決されます。

### 内部ロード バランサー

プライベート IP アドレスを割り当てることが、 [Azure 内部ロード バランサー](load-balancer-internal-overview.md) (ILB) フロント エンドの仮想ネットワーク内のリソースの負荷分散された仮想 IP アドレス (VIP) として機能します。 これにより、インターネットに IP アドレスを公開することなく負荷分散できます。

### Application Gateway

プライベート IP アドレスを割り当てることが、 [Azure Application Gateway](application-gateway-introduction.md) フロント エンドは、内部ロード バランサー (ILB) エンドポイントとも呼ばれる、インターネットに公開されていない内部エンドポイントを構成します。 動作および割り当て方法は、上で説明した ILB と同じです。

### 早見表

| リソース| 静的割り当て| 動的割り当て| 複数の IP アドレス|
|---|---|---|---|
| 仮想マシン (VM)/ネットワーク インターフェイス カード (NIC)| あり| あり| あり|
| Load Balancer のフロント エンド| あり| あり| あり|
| Application Gateway のフロント エンド| あり| あり| あり|

## 次のステップ

[ネットワーク用の azure の PowerShell リファレンス](https://msdn.microsoft.com/library/azure/mt163510.aspx)

[Azure リソース マネージャー テンプレートの言語](../resource-group-authoring-templates.md)

[Azure のネットワーク – 一般的に使用されるテンプレート](https://github.com/Azure/azure-quickstart-templates)

[コンピューティング リソース プロバイダー](../virtual-machines-azurerm-versus-azuresm)

[Azure リソース マネージャーの概要](../resource-group-overview)

[ロール ベースのアクセス制御では、Azure リソース マネージャー](https://msdn.microsoft.com/library/azure/dn906885.aspx)

[Azure リソース マネージャーでタグを使用します。](https://msdn.microsoft.com/library/azure/dn848368.aspx)

[テンプレートのデプロイ](https://msdn.microsoft.com/library/azure/dn790549.aspx)






