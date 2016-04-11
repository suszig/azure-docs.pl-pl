<properties
    pageTitle="SharePoint サーバー ファームの作成 | Microsoft Azure"
    description="Azure ポータルの Marketplace を使用して、SharePoint Server 2013 の基本ファームまたは高可用性ファームをすばやく新規作成します。"
    services="virtual-machines"
    documentationCenter=""
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="Windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/29/2015"
    ms.author="josephd"/>

# SharePoint サーバー ファームの作成

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のモデルです。

Microsoft Azure ポータル Marketplace では、事前に構成された SharePoint Server 2013 ファームを簡単に作成できます。 これにより、開発またはテストの環境で基本または高可用性の SharePoint ファームが必要な場合や、組織のコラボレーション ソリューションとして SharePoint Server 2013 を評価する場合の所要時間を大きく節約できます。

> [AZURE.NOTE]  **SharePoint サーバー ファーム** Azure ポータルの Azure Marketplace の項目が削除されました。

この構成では、次の 3 つの仮想マシンの基本的な SharePoint ファームで構成されます。

![sharepointfarm](./media/virtual-machines-sharepoint-farm-azure-preview/Non-HAFarm.png)

このファーム構成は、SharePoint アプリ開発の簡略化セットアップまたは、SharePoint 2013 の初回の評価で使用することができます。

基本的な (3 サーバー) SharePoint ファームを作成するには:

1. クリックして [ここ](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/)します。
2. クリックして **展開**します。
3.  **SharePoint 2013 非高可用性ファーム** ] ウィンドウで、をクリックして **作成**します。
4. 7 つの手順で設定を指定、 **SharePoint 2013 の作成以外の高可用性ファーム** ] ウィンドウで、クリックして **作成**します。

高可用性 SharePoint ファームは、次に示す構成の 9 台の仮想マシンで構成されます。

![sharepointfarm](./media/virtual-machines-sharepoint-farm-azure-preview/HAFarm.png)

このファーム構成は、より大量のクライアント読み込みや外部の SharePoint サイトの高可用性をテストしたり、SharePoint ファームの SQL Server AlwaysOn をテストしたりする場合に使用できます。 この構成は、高可用環境での SharePoint アプリケーション開発でも使用できます。

高可用性 (9 サーバー) の SharePoint ファームを作成するには:

1. クリックして [ここ](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/)します。
2. クリックして **展開**します。
3.  **SharePoint 2013 の HA ファーム** ] ウィンドウで、をクリックして **作成**します。
4. 7 つの手順で設定を指定、 **作成 SharePoint 2013 HA ファーム** ] ウィンドウで、クリックして **作成**します。

> [AZURE.NOTE] マイクロソフトでは、SharePoint Server 2016 IT Preview をリリースしました。 SharePoint Server 2016 IT Preview とその前提条件があらかじめインストールされた Azure 仮想マシン ギャラリー イメージを使用すると、このプレビュー版を簡単にインストールしてテストすることができます。 詳細については、次を参照してください。 [Azure での SharePoint Server 2016 IT Preview のテスト](http://azure.microsoft.com/blog/test-sharepoint-server-2016-it-preview-4/)します。

## SharePoint ファームの管理

これらのファームのサーバーはリモート デスクトップ接続を通じて管理できます。 詳細については、次を参照してください。 [仮想マシンにログオン](virtual-machines-windows-tutorial.md#log-on-to-the-virtual-machine)します。

SharePoint のサーバーの全体管理サイトで [My Sites]、SharePoint アプリケーション、その他の機能を構成できます。 詳細については、次を参照してください。 [SharePoint 2013 を構成する](http://technet.microsoft.com/library/ee836142.aspx)です。

> [AZURE.NOTE] Azure ポータルでは、インターネットに接続する web プレゼンス持つクラウド専用仮想ネットワークにこれら両ファームを作成します。 組織のネットワークに戻るサイト間 VPN 接続や ExpressRoute 接続はありません。

## その他のリソース

[Azure インフラストラクチャ サービスでホストされる SharePoint ファーム](virtual-machines-sharepoint-infrastructure-services.md)

[テスト用のハイブリッド クラウドでの SharePoint イントラネット ファームの設定](../virtual-network/virtual-networks-setup-sharepoint-hybrid-cloud-testing.md)

[SharePoint 2013 用の Microsoft Azure アーキテクチャ](https://technet.microsoft.com/library/dn635309.aspx)

