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

仮想マシンとその各種リソースを 1 つの論理的なデプロイメント単位として作成および管理する手段として、Azure リソース マネージャー テンプレートが最近導入されました。 このアプローチに関する手順が、以下で該当する箇所に記載されています。 Azure リソース マネージャーと 1 つの単位としてリソースを管理する方法の詳細については、次を参照してください。、 [の概要 []][]します。

## ツールの選択肢

### GUI: Azure クラシック ポータルまたは Azure ポータル

ポータルのグラフィカル ユーザー インターフェイスを使用すれば、特に Azure を初めて使用する場合に、仮想マシンを簡単に試すことができます。 いずれかの方法、 [Azure クラシック ポータル](http://manage.windowsazure.com) または [Azure ポータル](http://portal.azure.com) VM を作成します。 一般的な手順については、次を参照してください。 [カスタム仮想マシンの [] を作成する][] からの Linux イメージを選択し、 **ギャラリー**します。 なお、 [Azure クラシック ポータル](http://manage.windowsazure.com) 従来のデプロイ モデルのみを使用して仮想マシンを作成します。

### コマンド シェル: Azure CLI または Azure PowerShell

コマンド シェルを使用する場合は、Mac および Linux ユーザー向けの Azure コマンド ライン インターフェイス (CLI) と、Azure 用 Windows PowerShell コマンドレットおよびカスタム コンソールを備えた Azure PowerShell のいずれかを選択します。

Azure CLI では、次を参照してください。 [Linux の仮想マシンを実行している [] を作成する][]します。 テンプレートを使用するには、次を参照してください。 [のデプロイと管理仮想マシンが Azure リソース マネージャー テンプレートと Azure CLI [] を使用して][]します。

Azure PowerShell を参照してください。 [Linux ベースの仮想マシンの [] を事前に構成を作成して Azure PowerShell を使用して][]します。 テンプレートを使用するには、次を参照してください。 [のデプロイと管理仮想マシンが Azure リソース マネージャー テンプレートと PowerShell の [] を使用して][]します。

### 開発環境: Visual Studio

[Visual Studio ので、web サイト用の仮想マシンを作成します。][]

[コンピューティング、ネットワーク、および Storage .NET ライブラリのを使用して Azure のリソースをデプロイします。][]

## オペレーティング システムとイメージの選択肢

実行するオペレーティング システムに基づいてイメージを選択します。 Azure とそのパートナーから多数のイメージが提供されており、中にはアプリケーションやツールが含まれているイメージもあります。 また、独自のイメージを使用することもできます。

### Azure のイメージ

この手順では、Azure のイメージを使用して、ネットワークや負荷分散などのオプションを使用してカスタマイズされた仮想マシンを作成する方法について説明しています。 参照してください [を Azure で Linux を実行するカスタム仮想マシンを作成する方法][]します。

### 独自のイメージを使用する

既存の Azure 仮想マシンをベースにしたイメージを使用するには、その VM を*キャプチャする*か、独自のイメージを仮想ハード ディスク (VHD) に格納してアップロードします。

- [CLI でテンプレートとして使用するための Linux 仮想マシンをキャプチャする方法][]
- [作成して、Linux オペレーティング システムのを含むバーチャル ハード ディスクをアップロード][]

## 次のステップ

[仮想マシンのにログオン][]

[データをアタッチするディスク][]

## その他のリソース

[基本構成テスト環境][]

[Azure ハイブリッド クラウド テスト環境][]

[リソース マネージャーおよび Mac、Linux、および Windows の Azure CLI での VM 操作のサービス管理コマンド][]



[overview]: ../resource-group-overview.md 
[create a virtual machine running windows]: virtual-machines-windows-tutorial.md 
[create a virtual machine running linux]: virtual-machines-linux-tutorial.md 
[equivalent resource manager and service management commands for vm operations with the azure cli for mac, linux, and windows]: xplat-cli-azure-manage-vm-asm-arm.md 
[deploy and manage virtual machines using azure resource manager templates and the azure cli]: virtual-machines-deploy-rmtemplates-azure-cli.md 
[deploy and manage virtual machines using azure resource manager templates and powershell]: virtual-machines-deploy-rmtemplates-powershell.md 
[use azure powershell to create and preconfigure linux-based virtual machines]: virtual-machines-ps-create-preconfigure-linux-vms.md 
[how to create a custom virtual machine running linux in azure]: virtual-machines-linux-create-custom.md 
[how to capture a linux virtual machine to use as a template with the cli]: virtual-machines-linux-capture-image.md 
[creating and uploading a virtual hard disk that contains the linux operating system]: virtual-machines-linux-create-upload-vhd.md 
[creating a virtual machine for a website with visual studio]: virtual-machines-dotnet-create-visual-studio-powershell.md 
[deploy azure resources using the compute, network, and storage .net libraries]: virtual-machines-arm-deployment.md 
[log on to the virtual machine]: virtual-machines-linux-how-to-log-on.md 
[attach a data disk]: virtual-machines-linux-how-to-attach-disk.md 
[base configuration test environment]: virtual-machines-base-configuration-test-environment.md 
[azure hybrid cloud test environments]: virtual-machines-hybrid-cloud-test-environments.md 
[create a virtual machine running linux]: virtual-machines-linux-tutorial.md 
[create a custom virtual machine]: virtual-machines-create-custom.md 

