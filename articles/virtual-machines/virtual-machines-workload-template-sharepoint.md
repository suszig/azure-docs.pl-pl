<properties
    pageTitle="ARM テンプレートを使用して SharePoint ファームをデプロイする | Microsoft Azure"
    description="リソース マネージャーのテンプレートと、Azure ポータル、Azure PowerShell、または Azure CLI を使用して、3 サーバーまたは 9 サーバーの SharePoint ファームを簡単にデプロイします。"
    services="virtual-machines"
    documentationCenter=""
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sharepoint"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/20/2015"
    ms.author="josephd"/>

# Azure リソース マネージャーのテンプレートを使用した SharePoint ファームのデプロイ

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。 このリソースは、クラシック デプロイメント モデルを使用して作成することはできません。

この記事の手順では、リソース マネージャーのテンプレートを使用して、3 台または 9 台のサーバーで構成される新しい SharePoint Server 2013 ファームをデプロイします。

## 3 サーバーの SharePoint ファームのデプロイ

基本的な SharePoint Server 2013 ファームの場合、リソース マネージャーのテンプレートは、新しい仮想ネットワークの 3 つの異なるサブネットに 3 つの仮想マシンを作成します。

![](./media/virtual-machines-workload-template-sharepoint/three-server-sharepoint-farm.png)

テンプレートは、Azure ポータル、Azure PowerShell、または Azure CLI を使用して実行できます。

> [AZURE.NOTE] 使用してこの構成を作成することも、 [SharePoint 2013 非高可用性ファーム](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/) Azure ポータルの Azure Marketplace の項目。

### Azure ポータル

リソース マネージャー テンプレートと Azure ポータルを使用してこのワークロードを展開する] をクリックして [ここ](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsharepoint-three-vm%2Fazuredeploy.json)します。

![](./media/virtual-machines-workload-template-sharepoint/azure-portal-template.png)

1.  クリックして **パラメーター**します。  **パラメーター** ] ウィンドウで、新しい値を入力、使用できる値から選択してまたは既定値を受け入れをクリックして **OK**します。
2.  必要な場合はクリックして **サブスクリプション** し、適切な Azure サブスクリプションを選択します。
3.  クリックして **リソース グループ** 既存のリソース グループを選択します。 または、クリックして **新規作成** にこのワークロード用の新しいものを作成します。
4.  必要な場合はクリックして **リソース グループの場所** し、適切な Azure の場所を選択します。
6.  クリックして **法律条項** 、テンプレートの使用許諾契約書条項を確認し **購入**します。
7.  クリックして **作成**します。

テンプレートによっては、Azure でのワークロードの構築に時間がかかる場合があります。 完了すると、新しい 3 サーバーの SharePoint ファームが、既存または新規のリソース グループに作成されます。

### Azure PowerShell

> [AZURE.NOTE] この記事には、Azure PowerShell のプレビュー 1.0 コマンドが含まれています。 で Azure PowerShell 0.9.8 と以前のバージョンをこれらのコマンドを実行するには、置換 **新規 AzureRMResourceGroup** と **New-azureresourcegroup**, 、置換 **新規 AzureRMResourceGroupDeployment** と **New-azureresourcegroupdeployment**, 、を追加し、 **Switch-azuremode AzureResourceManager** コマンドする前に、 **New-azureresourcegroup** コマンドです。 詳細については、次を参照してください。 [Azure PowerShell 1.0 プレビュー](https://azure.microsoft.com/blog/azps-1-0-pre/)します。

次のコマンド セットに、Azure のデプロイメント名、新しいリソース グループ名、Azure データ センターの場所を入力します。 削除を含む引用符内のすべての < と > 文字です。

    $deployName="<deployment name>"
    $RGName="<resource group name>"
    $locName="<Azure location, such as West US>"
    $templateURI="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sharepoint-three-vm/azuredeploy.json"
    New-AzureRMResourceGroup -Name $RGName -Location $locName
    New-AzureRMResourceGroupDeployment -Name $deployName -ResourceGroupName $RGName -TemplateUri $templateURI

たとえば次のようになります。

    $deployName="TestDeployment"
    $RGName="TestRG"
    $locname="West US"
    $templateURI="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sharepoint-three-vm/azuredeploy.json"
    New-AzureRMResourceGroup -Name $RGName -Location $locName
    New-AzureRMResourceGroupDeployment -Name $deployName -ResourceGroupName $RGName -TemplateUri $templateURI

次に、Azure PowerShell プロンプトでコマンド ブロックを実行します。

実行すると、 **新規 AzureRMResourceGroupDeployment** コマンドを一連のパラメーターの値を指定するように求められます。 すべてのパラメーター値を指定したら **新規 AzureRMResourceGroupDeployment** を作成して仮想マシンを構成します。

テンプレートの実行が完了すると、新しい 3 サーバーの SharePoint ファームが、新しいリソース グループに作成されます。

### Azure CLI

開始する前に、適切なバージョンの Azure CLI がインストールされていること、ログインしていること、新しいリソース マネージャー モードに切り替えていることを確認します。 詳細については、次のようにクリックします。 [ここ](virtual-machines-deploy-rmtemplates-azure-cli.md#getting-ready)します。

まず、新しいリソース グループを作成します。 次のコマンドを使用して、グループの名前と、デプロイ先の Azure データ センターの場所を指定します。

    azure group create <group name> <location>

次に、次のコマンドを使用して、新しいリソース グループの名前と、Azure デプロイメントの名前を指定します。

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sharepoint-three-vm/azuredeploy.json <group name> <deployment name>

たとえば次のようになります。

    azure group create sp3serverfarm eastus2
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sharepoint-three-vm/azuredeploy.json sp3serverfarm spdevtest

実行すると、 **azure グループの展開を作成** コマンドを一連のパラメーターの値を指定するように求められます。 すべてのパラメーター値を指定したら、Azure により仮想マシンが作成、構成されます。

新しいリソース グループに新しい 3 サーバーの SharePoint ファームが作成されます。

## 9 サーバーの SharePoint ファームのデプロイ

高可用性の SharePoint Server 2013 ファームの場合、リソース マネージャーのテンプレートは、新しい仮想ネットワークの 4 つの異なるサブネットに 9 つの仮想マシンを作成します。

![](./media/virtual-machines-workload-template-sharepoint/nine-server-sharepoint-farm.png)

> [AZURE.NOTE] 使用してこの構成を作成することも、 [SharePoint 2013 の HA ファーム](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/) Azure ポータルの Azure Marketplace の項目。

### Azure ポータル

リソース マネージャー テンプレートと Azure ポータルを使用してこのワークロードを展開する] をクリックして [ここ](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsharepoint-server-farm-ha%2Fazuredeploy.json)します。

![](./media/virtual-machines-workload-template-sharepoint/azure-portal-template.png)

1.  クリックして **パラメーター**します。  **パラメーター** ] ウィンドウで、新しい値を入力、使用できる値から選択してまたは既定値を受け入れをクリックして **OK**します。
2.  必要な場合はクリックして **サブスクリプション** し、適切な Azure サブスクリプションを選択します。
3.  クリックして **リソース グループ** 既存のリソース グループを選択します。 または、クリックして **新規作成** にこのワークロード用の新しいものを作成します。
4.  必要な場合はクリックして **リソース グループの場所** し、適切な Azure の場所を選択します。
5.  クリックして **法律条項** 、テンプレートの使用許諾契約書条項を確認し **購入**します。
6.  クリックして **作成**します。

テンプレートによっては、Azure でのワークロードの構築に時間がかかる場合があります。 完了すると、新しい 9 サーバーの SharePoint ファームが、既存または新規のリソース グループに作成されます。

### Azure PowerShell

> [AZURE.NOTE] この記事には、Azure PowerShell のプレビュー 1.0 コマンドが含まれています。 で Azure PowerShell 0.9.8 と以前のバージョンをこれらのコマンドを実行するには、置換 **新規 AzureRMResourceGroup** と **New-azureresourcegroup**, 、置換 **新規 AzureRMResourceGroupDeployment** と **New-azureresourcegroupdeployment**, 、を追加し、 **Switch-azuremode AzureResourceManager** コマンドする前に、 **New-azureresourcegroup** コマンドです。 詳細については、次を参照してください。 [Azure PowerShell 1.0 プレビュー](https://azure.microsoft.com/blog/azps-1-0-pre/)します。

次のコマンド セットに、Azure のデプロイメント名、新しいリソース グループ名、Azure データ センターの場所を入力します。 削除を含む引用符内のすべての < と > 文字です。

    $deployName="<deployment name>"
    $RGName="<resource group name>"
    $locName="<Azure location, such as West US>"
    $templateURI="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sharepoint-server-farm-ha/azuredeploy.json"
    New-AzureRMResourceGroup -Name $RGName -Location $locName
    New-AzureRMResourceGroupDeployment -Name $deployName -ResourceGroupName $RGName -TemplateUri $templateURI

たとえば次のようになります。

    $deployName="TestDeployment"
    $RGName="TestRG"
    $locname="West US"
    $templateURI="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sharepoint-server-farm-ha/azuredeploy.json"
    New-AzureRMResourceGroup -Name $RGName -Location $locName
    New-AzureRMResourceGroupDeployment -Name $deployName -ResourceGroupName $RGName -TemplateUri $templateURI

次に、Azure PowerShell コマンド プロンプトでコマンド ブロックを実行します。

実行すると、 **新規 AzureRMResourceGroupDeployment** コマンドを一連のパラメーターの値を指定するように求められます。 すべてのパラメーター値を指定したら **新規 AzureRMResourceGroupDeployment** を作成して仮想マシンを構成します。

テンプレートの実行が完了すると、新しい 9 サーバーの SharePoint ファームが、新しいリソース グループに作成されます。

### Azure CLI

開始する前に、適切なバージョンの Azure CLI がインストールされていること、ログインしていること、新しいリソース マネージャー モードに切り替えていることを確認します。 詳細については、次のようにクリックします。 [ここ](virtual-machines-deploy-rmtemplates-azure-cli.md#getting-ready)します。

最初に、新しいリソース グループを作成します。 次のコマンドを使用して、グループの名前と、デプロイ先の Azure データ センターの場所を指定します。

    azure group create <group name> <location>

次に、次のコマンドを使用して、新しいリソース グループの名前と、Azure デプロイメントの名前を指定します。

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sharepoint-server-farm-ha/azuredeploy.json <group name> <deployment name>

たとえば次のようになります。

    azure group create sphaserverfarm eastus2
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sharepoint-server-farm-ha/azuredeploy.json sphaserverfarm spdevtest

実行すると、 **azure グループの展開を作成** コマンドを一連のパラメーターの値を指定するように求められます。 すべてのパラメーター値を指定したら、Azure により仮想マシンが作成、構成されます。

テンプレートの実行が完了すると、新しい 9 サーバーの SharePoint Server 2013 ファームが、新しいリソース グループに作成されます。


## その他のリソース

[Azure インフラストラクチャ サービスでホストされる SharePoint ファーム](virtual-machines-sharepoint-infrastructure-services.md)

[Azure リソース マネージャー テンプレートと Azure PowerShell を使用した Virtual Machines のデプロイと管理](virtual-machines-deploy-rmtemplates-powershell.md)

[Azure リソース マネージャーにおける Azure Compute、Network、Storage プロバイダー](virtual-machines-azurerm-versus-azuresm.md)

[Azure リソース マネージャーの概要](../resource-group-overview.md)

[Azure リソース マネージャー テンプレートと Azure CLI を使用した Virtual Machines のデプロイと管理](virtual-machines-deploy-rmtemplates-azure-cli.md)

[仮想マシンに関するドキュメント](http://azure.microsoft.com/documentation/services/virtual-machines/)

[Azure PowerShell のインストールおよび構成方法](../install-configure-powershell.md)



