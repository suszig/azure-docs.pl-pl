<properties
    pageTitle="Linux VM を作成するさまざまな方法 | Microsoft Azure"
    description="Azure で Linux 仮想マシンを作成するさまざまな方法を紹介し、手順へのリンクを提供します。"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="08/12/2015"
    ms.author="dkshir"/>

# Linux 仮想マシンを作成するさまざまな方法

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

VM は多様なユーザーや目的に適しているため、Azure には VM を作成するためのさまざまな方法が用意されています。 このため、VM とその作成方法について、いくつかの選択をする必要があります。 この記事には、これらの選択肢の概要と、手順へのリンクが記載されています。

仮想マシンとその各種リソースを 1 つの論理的なデプロイメント単位として作成および管理する手段として、Azure リソース マネージャー テンプレートが最近導入されました。 このアプローチに関する手順が、以下で該当する箇所に記載されています。 Azure リソース マネージャーと 1 つの単位としてリソースを管理する方法に関する詳細については、[概要] を参照してください。

## ツールの選択肢

### GUI: Azure クラシック ポータルまたは Azure ポータル

ポータルのグラフィカル ユーザー インターフェイスを使用すれば、特に Azure を初めて使用する場合に、仮想マシンを簡単に試すことができます。 いずれかの方法、 [Azure クラシック ポータル](http://manage.windowsazure.com) または [Azure ポータル](http://portal.azure.com) VM を作成します。 一般的な手順については、次を参照してください。 [カスタム仮想マシンの作成][] からの Linux イメージを選択し、 **ギャラリー**します。 なお、 [Azure クラシック ポータル](http://manage.windowsazure.com) 従来のデプロイ モデルのみを使用して仮想マシンを作成します。

### コマンド シェル: Azure CLI または Azure PowerShell

コマンド シェルを使用する場合は、Mac および Linux ユーザー向けの Azure コマンド ライン インターフェイス (CLI) と、Azure 用 Windows PowerShell コマンドレットおよびカスタム コンソールを備えた Azure PowerShell のいずれかを選択します。

Azure CLI では、次を参照してください。 [を実行している Linux の仮想マシンを作成する][]です。 テンプレートを使用するには、次を参照してください。 [のデプロイと管理仮想マシンが Azure リソース マネージャー テンプレートと Azure CLI を使用して][]します。

Azure PowerShell を参照してください。 [Linux ベースの仮想マシンを事前に構成を作成して Azure PowerShell を使用して][]します。 テンプレートを使用するには、次を参照してください。 [のデプロイと管理仮想マシンが Azure リソース マネージャー テンプレートと PowerShell を使用して][]します。

### 開発環境: Visual Studio

[Visual Studio で Web サイト用の仮想マシンを作成する][]

[Compute、Network、および Storage .NET ライブラリを使用した Azure リソースのデプロイ][]

## オペレーティング システムとイメージの選択肢

実行するオペレーティング システムに基づいてイメージを選択します。 Azure とそのパートナーから多数のイメージが提供されており、中にはアプリケーションやツールが含まれているイメージもあります。 また、独自のイメージを使用することもできます。

### Azure のイメージ

この手順では、Azure のイメージを使用して、ネットワークや負荷分散などのオプションを使用してカスタマイズされた仮想マシンを作成する方法について説明しています。 参照してください [を Azure で Linux を実行するカスタム仮想マシンを作成する方法][]します。

### 独自のイメージを使用する

既存の Azure 仮想マシンに基づくイメージを使用して *キャプチャ* バーチャル ハード_ディスク (VHD) に格納されているその VM、または、独自のイメージをアップロードします。

- [テンプレートとして使用するために CLI を使って Linux 仮想マシンをキャプチャする方法][]
- [Linux オペレーティング システムを格納した仮想ハード ディスクの作成とアップロード][]

## 次のステップ

[仮想マシンへのログオン][]

[データ ディスクの接続][]

## その他のリソース

[基本構成テスト環境][]

[Azure ハイブリッド クラウド テスト環境][]

[Mac、Linux、および Windows 用 Azure CLI での VM 操作に使用するリソース マネージャーおよびサービス管理コマンドの対応][]

<!-- LINKS -->
[overview]: ../resource-group-overview.md

[Create a Virtual Machine Running Windows]: virtual-machines-windows-tutorial.md
[Create a Virtual Machine Running Linux]: virtual-machines-linux-tutorial.md

[Equivalent Resource Manager and Service Management Commands for VM Operations with the Azure CLI for Mac, Linux, and Windows]:xplat-cli-azure-manage-vm-asm-arm.md
[Deploy and Manage Virtual Machines using Azure Resource Manager Templates and the Azure CLI]: virtual-machines-deploy-rmtemplates-azure-cli.md
[Deploy and Manage Virtual Machines using Azure Resource Manager Templates and PowerShell]:  virtual-machines-deploy-rmtemplates-powershell.md
[Use Azure PowerShell to create and preconfigure Linux-based Virtual Machines]: virtual-machines-ps-create-preconfigure-linux-vms.md

[How to Create a Custom Virtual Machine Running Linux in Azure]: virtual-machines-linux-create-custom.md
[How to Capture a Linux Virtual Machine to Use as a Template with the CLI]: virtual-machines-linux-capture-image.md

[Creating and Uploading a Virtual Hard Disk that Contains the Linux Operating System]: virtual-machines-linux-create-upload-vhd.md

[Creating a virtual machine for a website with Visual Studio]: virtual-machines-dotnet-create-visual-studio-powershell.md
[Deploy Azure Resources Using the Compute, Network, and Storage .NET Libraries]: virtual-machines-arm-deployment.md

[Log On to the Virtual Machine]: virtual-machines-linux-how-to-log-on.md

[Attach a Data Disk]: virtual-machines-linux-how-to-attach-disk.md

[Base Configuration Test Environment]: virtual-machines-base-configuration-test-environment.md
[Azure hybrid cloud test environments]: virtual-machines-hybrid-cloud-test-environments.md

[Create a Virtual Machine Running Linux]: virtual-machines-linux-tutorial.md
[Create a Custom Virtual Machine]: virtual-machines-create-custom.md

