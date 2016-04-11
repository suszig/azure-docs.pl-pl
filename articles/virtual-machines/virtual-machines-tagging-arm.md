<properties
   pageTitle="VM にタグ付けする方法 |Microsoft Azure"
   description="この記事では、リソース マネージャーのデプロイ モデルを使用して作成した Azure 仮想マシンのタグ付けについて説明します。"
   services="virtual-machines"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure-services"
   ms.date="11/10/2015"
   ms.author="dkshir;memccror"/>

# Azure で仮想マシンにタグを付ける方法

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。


この記事では、Azure リソース マネージャーでAzure で仮想マシンにタグを付けるさまざまな方法について説明します。 タグはユーザー定義のキーと値ペアです。リソースまたはリソース グループに直接設定できます。 現在、Azure では、1 つのリソースまたはリソース グループにつき最大 15 個のタグを付けることができます。 タグは、リソースの作成時に付けたり、既存のリソースに追加したりすることができます。 タグは、Azure リソース マネージャー経由で作成されたリソースでのみサポートされます。

## テンプレートを使用して仮想マシンにタグを付ける

まず、テンプレートを使用してタグを付けてみましょう。 [このテンプレート](https://github.com/Azure/azure-quickstart-templates/tree/master/101-tags-vm) 、次のリソースにタグを配置します。 Compute (仮想マシン)、(パブリック IP アドレス、仮想ネットワーク、およびネットワーク インターフェイス) のネットワーク、および Storage (ストレージ アカウント)。

クリックして、 **Deploy to Azure** ボタンから、 [テンプレートへのリンク](https://github.com/Azure/azure-quickstart-templates/tree/master/101-tags-vm)します。 これは、結果に移動、 [Azure ポータル](http://portal.azure.com/) このテンプレートを展開することができます。

![タグを使用した単純なデプロイ](./media/virtual-machines-tagging-arm/deploy-to-azure-tags.png)

このテンプレートには、次のタグが含まれています: *部門*, 、*アプリケーション*, 、および *"作成者"*します。 別のタグ名を使いたい場合は、テンプレートにタグを追加したり、タグを編集したりすることができます。

![テンプレートの Azure タグ](./media/virtual-machines-tagging-arm/azure-tags-in-a-template.png)

上の例のように、タグはキーと値のペアとして定義され、コロン (:) で区切られています。 タグは次の形式で定義する必要があります。

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

選択したタグの編集を完了したら、テンプレート ファイルを保存します。

次に、 **パラメーターの編集** ] セクションを入力する値、タグのです。

![Azure ポータルでタグを編集する](./media/virtual-machines-tagging-arm/edit-tags-in-azure-portal.png)

をクリックして **作成** をタグの値には、このテンプレートをデプロイします。


## ポータルからタグを付ける

タグを使用してリソースを作成したら、ポータルでタグの表示、追加、および削除を行うことができます。

タグを表示するには、タグ アイコンを選択します。

![Azure ポータルのタグ アイコン](./media/virtual-machines-tagging-arm/azure-portal-tags-icon.png)

ポータルで新しいタグを追加するには、キーと値のペアを定義して保存します。

![Azure ポータルで新しいタグを追加する](./media/virtual-machines-tagging-arm/azure-portal-add-new-tag.png)

リソースのタグ一覧に新しいタグが表示されます。

![Azure ポータルで保存された新しいタグ](./media/virtual-machines-tagging-arm/azure-portal-saved-new-tag.png)


## PowerShell を使用したタグ付け

作成するには、追加、および PowerShell を設定する最初必要からタグを削除、 [Azure リソース マネージャーでの PowerShell 環境][]します。 設定を完了したら、PowerShell で Compute、Network、および Storage リソースを作成するとき、またはリソースを作成した後にタグを追加できます。 この記事では、Virtual Machines で設定したタグを表示または編集する方法を中心に説明します。

まず `Get-AzureVM` コマンドレットで仮想マシンを表示します。

        PS C:\> Get-AzureVM -ResourceGroupName "MyResourceGroup" -Name "MyWindowsVM"

仮想マシンにタグが既に含まれている場合、リソースのすべてのタグが表示されます。

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

PowerShell でタグを追加する場合、`Set-AzureResource` コマンドを使用できます。 PowerShell でタグを更新すると、タグ全体が更新されます。 そのため、既にタグが設定されているリソースに 1 つのタグを追加する場合、リソースに設定するすべてのタグを含める必要があります。 次に、PowerShell コマンドレットでリソースにタグを追加する例を示します。

この最初のコマンドレットでは、すべてに設定したタグの設定 *MyWindowsVM* に、 *タグ* 変数を使用して、 `Get-AzureResource` と `Tags` 関数です。 パラメーター `ApiVersion` は省略可能ですが、指定されていない場合、リソース プロバイダーの最新の API バージョンが使用されることに注意してください。 

        PS C:\> $tags = (Get-AzureResource -Name MyWindowsVM -ResourceGroupName MyResourceGroup -ResourceType "Microsoft.Compute/virtualmachines" -ApiVersion 2015-05-01-preview).Tags

2 番目のコマンドで、指定した変数のタグが表示されます。

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment

3 番目のコマンドを追加する追加のタグ、 *タグ* 変数です。 使用に注意してください、 **+ =** に新しいキー/値ペアを追加する、 *タグ* ] ボックスの一覧です。

        PS C:\> $tags +=@{Name="Location";Value="MyLocation"}

4 番目のコマンドは、すべてで定義されたタグの設定、 *タグ* 変数を指定されたリソースにします。 この例では MyWindowsVM です。

        PS C:\> Set-AzureResource -Name MyWindowsVM -ResourceGroupName MyResourceGroup -ResourceType "Microsoft.Compute/VirtualMachines" -ApiVersion 2015-05-01-preview -Tag $tags

5 番目のコマンドでは、リソースのすべてのタグが表示されます。 ご覧のとおり、 *場所* がタグとして定義されました *MyLocation* 値として。

        PS C:\> (Get-AzureResource -ResourceName "MyWindowsVM" -ResourceGroupName "MyResourceGroup" -ResourceType "Microsoft.Compute/VirtualMachines" -ApiVersion 2015-05-01-preview).Tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment
        Value       MyLocation
        Name        Location

PowerShell を使ってタグを付ける方法の詳細については、チェック アウト、 [Azure リソース コマンドレット][]します。


## Azure CLI を使用してタグを付ける

Azure CLI で既に作成されているリソースにも、タグを付けることができます。 設定するのには、 [Azure CLI 環境][]します。 Azure CLI でサブスクリプションにログインし、ARM リソースに切り替えます。 このコマンドを使用すると、タグを含め、指定した仮想マシンのすべてのプロパティを表示できます。

        azure vm show -g MyResourceGroup -n MyVM

PowerShell とは異なり、既にタグが含まれているリソースにタグを追加する場合、`azure vm set` コマンドを使用する前にすべてのタグ (既存のタグと新しいタグ) を指定する必要はありません。 代わりに、このコマンドを使用すると、リソースにタグが付加されます。 Azure CLI を使用して新しい VM タグを追加するには、使用することができます、 `azure vm set` タグ パラメーターと共にコマンド **-t**:

        azure vm set -g MyResourceGroup -n MyVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

使用できるすべてのタグを削除する、 **– T** 内のパラメーター、 `azure vm set` コマンドです。

        azure vm set – g MyResourceGroup –n MyVM -T


ここでは、PowerShell、Azure CLI、およびポータルを使用してリソースにタグを適用しました。次に、課金ポータルの使用量の詳細でタグを確認してみましょう。


## 使用量の詳細でタグを確認する

タグは、コンピューティング、ネットワークに配置されの使用状況の詳細に記憶域リソースを Azure リソース マネージャーが入力されます、 [課金ポータル](https://account.windowsazure.com/)します。

をクリックして **使用状況の詳細情報をダウンロード** サブスクリプションの使用方法の詳細を表示します。

![Azure ポータルの使用量の詳細](./media/virtual-machines-tagging-arm/azure-portal-tags-usage-details.png)

課金明細を選択し、 **バージョン 2** 使用状況の詳細。

![Azure ポータルに表示される Version 2 プレビューの使用量の詳細](./media/virtual-machines-tagging-arm/azure-portal-version2-usage-details.png)

使用状況の詳細から、すべてのタグを確認できます、 **タグ** 列。

![Azure ポータルのタグ列](./media/virtual-machines-tagging-arm/azure-portal-tags-column.png)

これらのタグと使用量を分析すると、新たな視点で組織の使用データを調査できるようになります。


## その他のリソース

* [Azure リソース マネージャーの概要][]
* [タグを使用した Azure リソースの整理][]
* [Microsoft Azure の課金内容の確認][]
* [Microsoft Azure リソースの消費を把握する][]




[PowerShell environment with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Azure Resource Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure CLI environment]: ./xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../resource-group-using-tags.md
[Understanding your Azure Bill]: ../billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../billing-usage-rate-card-overview.md

