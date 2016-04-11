<properties
    pageTitle="ARM テンプレートを使用した Ubuntu での WordPress |Microsoft Azure"
    description="リソース マネージャーのテンプレートと Azure ポータル、Azure PowerShell、または Azure CLI を使用して、Ubuntu 上で実行する 1 台の WordPress サーバーを簡単にデプロイします。"
    services="virtual-machines"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2015"
    ms.author="davidmu"/>

# Azure リソース マネージャーのテンプレートを使用した Ubuntu への WordPress サーバーのデプロイ

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。 このリソースは、クラシック デプロイメント モデルを使用して作成することはできません。

この記事の手順では、リソース マネージャーのテンプレートを使用して、Ubuntu 上で実行する WordPress サーバーをデプロイします。 このテンプレートは、新しい仮想ネットワークに単一の仮想マシンを作成します。

![](./media/virtual-machines-workload-template-wordpress/one-server-wordpress.png)

テンプレートは、Azure ポータル、Azure PowerShell、または Azure CLI を使用して実行できます。

## Azure ポータル

リソース マネージャー テンプレートと Azure ポータルを使用してこのワークロードを展開する] をクリックして [ここ](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fwordpress-single-vm-ubuntu%2Fazuredeploy.json)します。

![](./media/virtual-machines-workload-template-wordpress/azure-portal-template.png)

1.   **テンプレート** ] ウィンドウで、をクリックして **保存**します。
2.  クリックして **パラメーター**します。  **パラメーター** ] ウィンドウで、新しい値を入力、使用できる値から選択してまたは既定値を受け入れをクリックして **OK**します。
3.  必要な場合はクリックして **サブスクリプション** し、適切な Azure サブスクリプションを選択します。
4.  クリックして **リソース グループ** 既存のリソース グループを選択します。 または、クリックして **新規作成** にこのワークロード用の新しいものを作成します。
5.  必要な場合はクリックして **リソース グループの場所** し、適切な Azure の場所を選択します。
6.  必要な場合はクリックして **法律条項** 、テンプレートの使用許諾契約書条項を確認します。
7.  クリックして **作成**します。

テンプレートによっては、Azure でのワークロードの構築に時間がかかる場合があります。 完了すると、Ubuntu 上で実行する新しい WordPress サーバーが既存または新規のリソース グループに作成されます。

## Azure PowerShell

[AZURE.INCLUDE [powershell-preview](../../includes/powershell-preview-inline-include.md)]

次のコマンド セットに、Azure のデプロイメント名、新しいリソース グループ名、Azure データ センターの場所を入力します。 削除を含む引用符内のすべての < と > 文字です。

    $deployName="<deployment name>"
    $RGName="<resource group name>"
    $locName="<Azure location, such as West US>"
    $templateURI="https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/wordpress-single-vm-ubuntu/azuredeploy.json"
    New-AzureRmResourceGroup -Name $RGName -Location $locName
    New-AzureRmResourceGroupDeployment -Name $deployName -ResourceGroupName $RGName -TemplateUri $templateURI

たとえば次のようになります。

    $deployName="TestDeployment"
    $RGName="TestRG"
    $locname="West US"
    $templateURI="https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/wordpress-single-vm-ubuntu/azuredeploy.json"
    New-AzureRmResourceGroup -Name $RGName -Location $locName
    New-AzureRmResourceGroupDeployment -Name $deployName -ResourceGroupName $RGName -TemplateUri $templateURI

次に、Azure PowerShell プロンプトでコマンド ブロックを実行します。

実行すると、 **新規 AzureRmResourceGroupDeployment** コマンドを一連のパラメーターの値を指定するように求められます。 すべてのパラメーター値を指定したら **新規 AzureRmResourceGroupDeployment** を作成して仮想マシンを構成します。

テンプレートの実行が完了すると、Ubuntu 上で実行する WordPress サーバーが新しいリソース グループに作成されます。

## Azure CLI

開始する前に、適切なバージョンの Azure CLI がインストールされていること、ログインしていること、新しいリソース マネージャー モードに切り替えていることを確認します。 詳細については、次のようにクリックします。 [ここ](virtual-machines-deploy-rmtemplates-azure-cli.md#getting-ready)します。

まず、新しいリソース グループを作成します。 次のコマンドを使用して、グループの名前と、デプロイ先の Azure データ センターの場所を指定します。

    azure group create <group name> <location>

次に、次のコマンドを使用して、新しいリソース グループの名前と、Azure デプロイメントの名前を指定します。

    azure group deployment create --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/wordpress-single-vm-ubuntu/azuredeploy.json <group name> <deployment name>

たとえば次のようになります。

    azure group create wordpress eastus2
    azure group deployment create --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/wordpress-single-vm-ubuntu/azuredeploy.json wordpress wpdevtest

実行すると、 **azure グループの展開を作成** コマンドを一連のパラメーターの値を指定するように求められます。 すべてのパラメーター値を指定したら、Azure により仮想マシンが作成、構成されます。

テンプレートの実行が完了すると、Ubuntu 上で実行する WordPress サーバーが新しいリソース グループに作成されます。

## その他のリソース

[Azure リソース マネージャー テンプレートと Azure PowerShell を使用した Virtual Machines のデプロイと管理](virtual-machines-deploy-rmtemplates-powershell.md)

[Azure リソース マネージャーにおける Azure Compute、Network、ストレージ プロバイダー](virtual-machines-azurerm-versus-azuresm.md)

[Azure リソース マネージャーの概要](../resource-group-overview.md)

[Azure リソース マネージャー テンプレートと Azure CLI を使用した Virtual Machines のデプロイと管理](virtual-machines-deploy-rmtemplates-azure-cli.md)

[仮想マシンに関するドキュメント](http://azure.microsoft.com/documentation/services/virtual-machines/)

[Azure PowerShell のインストールおよび構成方法](../install-configure-powershell.md)

