<properties 
    pageTitle="SQL Server 仮想マシンに接続する | Microsoft Azure"
    description="このトピックでは、クラシック デプロイ モデルで作成されたリソースを使用し、Azure の仮想マシンで実行している SQL Server に接続する方法について説明します。シナリオは、ネットワーク構成とクライアントの場所によって異なります。"
    services="virtual-machines"
    documentationCenter="na"
    authors="rothja"
    manager="jeffreyg"
    editor="monicar"    
    tags="azure-service-management"/>
<tags 
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="11/12/2015"
    ms.author="jroth" />


# Azure での SQL Server 仮想マシンへの接続

## 概要

Azure の仮想マシンで実行されている SQL Server への接続の構成は、オンプレミスの SQL Server インスタンスに必要な手順と大きく異なってはいません。 ファイアウォール、認証、データベースのログインに関連する構成は引き続き行う必要があります。

ただし、SQL Server への接続には、Azure VM に固有の側面がいくつかあります。 この記事では一部紹介 [の一般的な接続シナリオ](#connection-scenarios) を提供 [Azure VM で SQL Server の接続を構成するための詳細手順](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)します。

この記事では、接続に重点を置いて説明します。 プロビジョニングと接続の両方の完全なチュートリアルについては、次を参照してください。 [Azure で SQL Server 仮想マシンをプロビジョニング](virtual-machines-provision-sql-server.md)します。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。

## 接続のシナリオ

クライアントから仮想マシンで実行されている SQL Server に接続する方法は、クライアントの場所と、マシンやネットワークの構成によって異なります。 これらのシナリオは、次のとおりです。

- [同じクラウド サービス内の SQL Server に接続します。](#connect-to-sql-server-in-the-same-cloud-service)
- [インターネット経由で SQL Server に接続します。](#connect-to-sql-server-over-the-internet)
- [同じ仮想ネットワーク内の SQL Server に接続します。](#connect-to-sql-server-in-the-same-virtual-network)

### 同一クラウド サービス内の SQL Server に接続する方法

同じクラウド サービス内で複数の仮想マシンを作成できます。 この仮想マシンのシナリオを理解するのを参照してください。 [仮想ネットワークまたはクラウド サービスと仮想マシンを接続する方法](cloud-services-connect-virtual-machine.md)します。

まず、に従って、 [接続を構成するには、この記事の手順を](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)します。 同じクラウド サービス内のマシン間で接続する場合は、パブリック エンドポイントをセットアップする必要はありません。

クライアントの接続文字列で VM の**ホスト名**を使用できます。 このホスト名は、作成時に VM に付けた名前です。 たとえば、SQL VM の名前が **mysqlvm** で、クラウド サービスの DNS 名が **mycloudservice.cloudapp.net** の場合、同じクラウド サービス内のクライアント VM では、次の接続文字列を使用して接続できます。

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### インターネット経由で SQL Server に接続する方法

インターネットから、SQL Server データベース エンジンに接続する場合は、着信 TCP 通信用の仮想マシンのエンドポイントを作成する必要があります。 この Azure 構成手順により、仮想マシンからアクセスできる TCP ポートに、着信する TCP ポート トラフィックが送信されます。

まず、に従って、 [接続を構成するには、この記事の手順を](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)します。 クラウド サービスの DNS 名 (**mycloudservice.cloudapp.net** など) と VM のエンドポイント (**57500** など) を指定することによって、インターネットにアクセスできるクライアントから SQL Server インスタンスに接続できます。

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

これでクライアントからインターネット経由での接続は有効になりますが、すべてのユーザーが SQL Server に接続できるわけではありません。 外部のクライアントは、ユーザー名とパスワードを修正する必要があります。 セキュリティを強化するために、既知のポート 1433 を仮想マシンのパブリック エンドポイントに使用しないでください。 また可能であれば、エンドポイントに ACL を追加して、許可されたクライアントのみにトラフィックを制限することを検討してください。 エンドポイントと Acl を使用する方法の詳細については、次を参照してください。 [エンドポイントの ACL の管理](virtual-machines-set-up-endpoints.md#manage-the-acl-on-an-endpoint)します。
>[AZURE.NOTE] この手法を使用して SQL Server と通信する場合は、返されるすべてのデータがデータセンターからの送信トラフィックと見なされることに注意してください。 標準の影響を受けます [送信データ転送の料金](http://azure.microsoft.com/pricing/details/data-transfers)します。 同じ Azure データ センター内の別のマシンまたはクラウド サービスからこの手法を使用する場合でも、やはりトラフィックが Azure のパブリック ロード バランサーを経由するため、同様の価格が適用されます。

### 同一仮想ネットワーク内で SQL Server に接続する方法

[仮想ネットワーク](..\virtual-network\virtual-networks-overview.md) 追加シナリオを実現できます。 同じ仮想ネットワーク内の VM が異なるクラウド サービスに存在する場合でも、それらの VM に接続できます。 使用して、 [サイト対サイト VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md), 、Vm を内部設置型ネットワークとマシンに接続するハイブリッド アーキテクチャを作成することができます。

仮想ネットワークを使うと、Azure VM をドメインに参加させることもできます。 これは、SQL Server に Windows 認証を使用する唯一の方法です。 その他の接続シナリオでは、ユーザー名とパスワードによる SQL 認証が必要です。

まず、に従って、 [接続を構成するには、この記事の手順を](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)します。 ドメイン環境と Windows 認証を構成する場合は、この記事の手順を使用して SQL 認証とログインを構成する必要はありません。 またこのシナリオでは、パブリック エンドポイントは不要です。

DNS が構成済みであることを前提として、接続文字列で SQL Server VM のホスト名を指定することによって、SQL Server インスタンスに接続できます。 次の例は、Windows 認証も構成されていることと、ユーザーが SQL Server インスタンスへのアクセスを許可されていることを前提としています。

    "Server=mysqlvm;Integrated Security=true" 

このシナリオでは、VM の IP アドレスも指定できます。

## Azure VM で SQL Server への接続を構成する手順

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

## 次のステップ

プロビジョニング接続手順と指示を表示するには、次を参照してください。 [Azure で SQL Server 仮想マシンをプロビジョニング](virtual-machines-provision-sql-server.md)します。

高可用性と障害復旧のために AlwaysOn 可用性グループを使用する予定もある場合、リスナーの実装を検討してください。 データベース クライアントは、SQL Server インスタンスの 1 つに直接接続せずに、リスナーに接続します。 その上でリスナーがクライアントを可用性グループのプライマリ レプリカにルーティングします。 詳細については、次を参照してください。 [Azure で AlwaysOn 可用性グループ用の ILB リスナーを構成する](virtual-machines-sql-server-configure-ilb-alwayson-availability-group-listener.md)します。

Azure の仮想マシンで実行されている SQL Server のセキュリティに関するベスト プラクティスをすべて確認することが重要です。 詳細については、次を参照してください。 [Azure 仮想マシンにおける SQL Server のセキュリティの考慮事項](virtual-machines-sql-server-security-considerations.md)します。

Azure Vm での SQL Server の実行に関連するその他のトピックを参照してください。 [Azure 仮想マシン上の SQL Server](virtual-machines-sql-server-infrastructure-services.md)します。





