<properties
    pageTitle="SharePoint Server 2013 ファームのデプロイ | Microsoft Azure"
    description="5 つのフェーズで、Azure の SQL Server AlwaysOn 可用性グループを使用して、高可用性 SharePoint Server 2013 ファームをデプロイします。"
    documentationCenter=""
    services="virtual-machines"
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
    ms.date="12/11/2015"
    ms.author="josephd"/>


# Azure での SharePoint と SQL Server AlwaysOn 可用性グループのデプロイ

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。

このトピックには、イントラネットのみの SharePoint 2013 ファームと SQL Server AlwaysOn 可用性グループをデプロイするための詳細な手順へのリンクが含まれます。 ファームには次のコンピューターが含まれます。

- 2 台の SharePoint Web サーバー
- 2 台の SharePoint アプリケーション サーバー
- 2 台のデータベース サーバー
- 1 台のクラスター マジョリティ ノード サーバー
- 2 台のドメイン コントローラー

構成を次に示します。各サーバーの名前はプレースホルダーです。

![](./media/virtual-machines-workload-intranet-sharepoint-overview/workload-spsqlao_05.png)

ロールごとに 2 台のマシンで高可用性を保証します。 すべての仮想マシンは 1 つのリージョンに存在します。 特定のロール用の仮想マシンの各グループは、専用の可用性セットに含まれます。

この構成を次のフェーズでデプロイします。

- [フェーズ 1: Azure を構成する](virtual-machines-workload-intranet-sharepoint-phase1.md)します。 ストレージ アカウント、可用性セット、およびクロスプレミス仮想ネットワークを作成します。
- [フェーズ 2: ドメイン コント ローラー構成](virtual-machines-workload-intranet-sharepoint-phase2.md)します。 レプリカ Active Directory ドメイン サービス (AD DS) ドメイン コントローラーを作成して構成します。
- [フェーズ 3: SQL Server インフラストラクチャの構成](virtual-machines-workload-intranet-sharepoint-phase3.md)します。 SQL Server 仮想マシンを作成して構成し、SharePoint 用に準備して、クラスターを作成します。
- [フェーズ 4: SharePoint の構成サーバー](virtual-machines-workload-intranet-sharepoint-phase4.md)します。 4 つの SharePoint 仮想マシンを作成して構成します。
- [フェーズ 5: 可用性グループを作成し、SharePoint データベースの追加](virtual-machines-workload-intranet-sharepoint-phase5.md)します。 データベースを準備し、SQL Server AlwaysOn 可用性グループを作成します。

この SharePoint と SQL Server AlwaysOn のデプロイに付属するように設計された、 [SharePoint と SQL Server AlwaysOn のインフォ グラフィック](http://go.microsoft.com/fwlink/?LinkId=394788) し、最新の推奨事項を組み込みます。

この構成は、Azure インフラストラクチャ サービスに機能する高可用性のイントラネット SharePoint ファームを作成するための、事前に定義されたアーキテクチャの規範的なフェーズ単位のガイドです。 Azure で SharePoint 2013 の実装に関する詳細なアーキテクチャ ガイダンスを参照してください。 [SharePoint 2013 用の Microsoft Azure アーキテクチャ](https://technet.microsoft.com/library/dn635309.aspx)します。

次の点に注意してください。

- SharePoint の実装に関する経験が豊富な場合は、フェーズ 3 ～ 5 の指示を自由に調整して、ニーズに最も適したファームを構築してください。
- 既存の Azure ハイブリッド クラウド デプロイが既にある場合は、フェーズ 1 および 2 の指示を自由に調整するかスキップし、適切なサブネットで新しい SharePoint ファームをホストしてください。
- すべてのサーバーは、Azure Virtual Network 内の 1 つのサブネット上の存在します。 サブネットの分離に相当する追加のセキュリティを提供する場合は、使用 [ネットワーク セキュリティ グループ](../virtual-network/virtual-networks-nsg.md)します。

開発/テスト環境や、概念実証の - この構成をビルドするを参照してください。 [テスト用のハイブリッド クラウドでの SharePoint イントラネット ファームの設定](../virtual-network/virtual-networks-setup-sharepoint-hybrid-cloud-testing.md)します。

SharePoint と SQL Server AlwaysOn 可用性グループの追加については、次を参照してください。 [を構成する SQL Server 2012 AlwaysOn 可用性グループを SharePoint 2013 用](https://technet.microsoft.com/library/jj715261.aspx)します。
> [AZURE.NOTE] Microsoft は、SharePoint Server 2016 IT Preview をリリースしました。 SharePoint Server 2016 IT Preview とその前提条件があらかじめインストールされた Azure 仮想マシン ギャラリー イメージを使用すると、このプレビュー版を簡単にインストールしてテストすることができます。 詳細については、次を参照してください。 [Azure での SharePoint Server 2016 IT Preview のテスト](http://azure.microsoft.com/blog/test-sharepoint-server-2016-it-preview-4/)します。

## 次のステップ

- 使用してこのワークロードの構成を起動 [フェーズ 1](virtual-machines-workload-intranet-sharepoint-phase1.md)します。






