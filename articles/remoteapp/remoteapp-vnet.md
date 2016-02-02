<properties
    pageTitle ="Azure RemoteApp で使用する Azure VNET の検証 |Microsoft Azure]
    description =「Azure VNET が Azure RemoteApp で使用する準備ができていることを確認する方法について説明します」
    サービス =「remoteapp」
    documentationCenter=""
    authors ="lizap"
    manager ="mbaldwin"/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/05/2015"
    ms.author="elizapo"/>



# Azure RemoteApp で使用する Azure VNET の検証

Azure RemoteApp で Azure VNET を使用する前に、VNET を検証できます。 これは、接続の問題を防ぐのに役立ちます。

Azure VNET を検証するには、次の手順を行います。

1. Azure RemoteApp で使用する Azure VNET のサブネット内に Azure 仮想マシンを作成します。

2. 管理ポータルで **[接続]** オプションを使用して、作成した VM に接続します。
3. 仮想マシンを Azure RemoteApp を使用するのと同じドメインに参加させます。 オンプレミスのネットワークに接続するハイブリッド コレクションを作成する場合は、仮想マシンをローカル ドメインに参加させます。

これに成功した場合は、Azure VNET は RemoteApp で使用する準備が整っています。

エンド ツー エンドのハイブリッド コレクションのワークフローの詳細については、次の記事を参照してください。

- [Azure RemoteApp の仮想ネットワークを計画する方法](remoteapp-planvpn.md)
- [ハイブリッド コレクションを作成します。](remoteapp-create-hybrid-deployment.md)
- [Azure RemoteApp コレクションを (ExpressRoute のサポート) で、Azure の仮想ネットワークに配置します。](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)





