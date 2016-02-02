<properties
    pageTitle="Windows VM を作成するさまざまな方法 | Microsoft Azure"
    description="リソース マネージャーで Windows 仮想マシンを作成するさまざまな方法を紹介します。"
    services="virtual-machines"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="index-page"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="10/22/2015"
    ms.author="cynthn"/>


# リソース マネージャーで Windows 仮想マシンを作成するさまざまな方法

仮想マシンは多様なユーザーや目的に適しているため、Azure には仮想マシンを作成するためのさまざまな方法が用意されています。 これは、仮想マシンとその作成方法について、いくつかの選択を行う必要があることを意味します。 この記事には、これらの選択肢の概要と、手順へのリンクが記載されています。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。

仮想マシンとその各種リソースを 1 つの論理的なデプロイメント単位として作成および管理する手段として、Azure リソース マネージャー テンプレートが最近導入されました。 このアプローチに関する手順が、以下で該当する箇所に記載されています。 Azure リソース マネージャーと 1 つの単位としてリソースを管理する方法の詳細については、次を参照してください。、 [の概要 []][]します。

## ツールの選択肢

### GUI: Azure ポータル

Azure クラシック ポータルのグラフィカル ユーザー インターフェイスを使用すれば、特に Azure を初めて使用する場合に、仮想マシンを簡単に試すことができます。 Azure ポータルを使用して、仮想マシンを作成します。

[Windows のを実行する仮想マシンを作成します。][]

### コマンド シェル: Azure CLI または Azure PowerShell

コマンド シェルを使用する場合は、Mac および Linux ユーザー向けの Azure コマンド ライン インターフェイス (CLI) と、Azure 用コマンドレットとカスタム コンソールを備えた Azure PowerShell のいずれかを選択します。

Azure CLI については、以下をご覧ください。

- [Mac、Linux、および Windows の Azure CLI を使用してバーチャル マシン操作のためのリソース マネージャーおよびサービス管理コマンド][]
- [展開し、Azure リソース マネージャー テンプレートと Azure CLI を使用して仮想マシンを管理][]します。

Azure PowerShell については、以下をご覧ください。

- [リソース マネージャーと PowerShell ので Windows VM の作成します。][]
- [作成し、リソース マネージャーと Azure PowerShell ので Windows 仮想マシンを事前構成する.][]
- [展開し、Azure リソース マネージャー テンプレートと PowerShell を使用して仮想マシンの管理][]
- [リソース マネージャー テンプレートと PowerShell で Windows 仮想マシンを作成します。][]

### 開発環境: Visual Studio

[コンピューティング、ネットワーク、および Storage .NET ライブラリのを使用して Azure のリソースをデプロイします。][]

## オペレーティング システムとイメージの選択肢

実行するオペレーティング システムに基づいてイメージを選択します。 Azure とそのパートナーから多数のイメージが提供されており、中にはアプリケーションやツールが含まれているイメージもあります。 また、独自のイメージを使用することもできます。 アプリケーションの情報を使用して、必要なプラットフォーム イメージの検索: [移動し、Windows PowerShell と Azure CLI による Azure の仮想マシン イメージを選択][]します。



[overview]: ../resource-group-overview.md 
[create a virtual machine running windows]: virtual-machines-windows-tutorial.md 
[equivalent resource manager and service management commands for virtual machine operations with the azure cli for mac, linux, and windows]: xplat-cli-azure-manage-vm-asm-arm.md 
[deploy and manage virtual machines using azure resource manager templates and the azure cli]: virtual-machines-deploy-rmtemplates-azure-cli.md 
[create and preconfigure a windows virtual machine with resource manager and azure powershell]: virtual-machines-ps-create-preconfigure-windows-resource-manager-vms.md 
[deploy and manage virtual machines using azure resource manager templates and powershell]: virtual-machines-deploy-rmtemplates-powershell.md 
[create a windows vm with resource manager and powershell]: virtual-machines-create-windows-powershell-resource-manager.md 
[create a windows virtual machine with a resource manager template and powershell]: virtual-machines-create-windows-powershell-resource-manager-template-simple.md 
[navigate and select azure virtual machine images with windows powershell and the azure cli]: resource-groups-vm-searching.md 
[deploy azure resources using the compute, network, and storage .net libraries]: virtual-machines-arm-deployment.md 
[sign in to the virtual machine]: virtual-machines-log-on-windows-server.md 
[base configuration test environment]: virtual-machines-base-configuration-test-environment.md 
[azure hybrid cloud test environments]: virtual-machines-hybrid-cloud-test-environments.md 

