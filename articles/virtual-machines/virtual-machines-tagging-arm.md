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

このテンプレートには、*Department*、*Application*、および *Created By* というタグが含まれています。 別のタグ名を使いたい場合は、テンプレートにタグを追加したり、タグを編集したりすることができます。

![テンプレートの Azure タグ](./media/virtual-machines-tagging-arm/azure-tags-in-a-template.png)

上の例のように、タグはキーと値のペアとして定義され、コロン (:) で区切られています。 タグは次の形式で定義する必要があります。

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

選択したタグの編集を完了したら、テンプレート ファイルを保存します。

次に、**[パラメーターの編集]** セクションで、タグの値を入力します。

![Azure ポータルでタグを編集する](./media/virtual-machines-tagging-arm/edit-tags-in-azure-portal.png)

**[作成]** をクリックして、タグの値が指定されたこのテンプレートをデプロイします。


## ポータルからタグを付ける

タグを使用してリソースを作成したら、ポータルでタグの表示、追加、および削除を行うことができます。

タグを表示するには、タグ アイコンを選択します。

![Azure ポータルのタグ アイコン](./media/virtual-machines-tagging-arm/azure-portal-tags-icon.png)

ポータルで新しいタグを追加するには、キーと値のペアを定義して保存します。

![Azure ポータルで新しいタグを追加する](./media/virtual-machines-tagging-arm/azure-portal-add-new-tag.png)

リソースのタグ一覧に新しいタグが表示されます。

![Azure ポータルで保存された新しいタグ](./media/virtual-machines-tagging-arm/azure-portal-saved-new-tag.png)


## PowerShell を使用したタグ付け

作成するには、追加、および PowerShell を設定する最初必要からタグを削除、 [Azure リソース マネージャーので、PowerShell 環境][]します。 設定を完了したら、PowerShell で Compute、Network、および Storage リソースを作成するとき、またはリソースを作成した後にタグを追加できます。 この記事では、Virtual Machines で設定したタグを表示または編集する方法を中心に説明します。

使用して仮想マシンに最初に、移動、 `Get-azurevm` コマンドレットです。

        PS C:\> Get-AzureVM -ResourceGroupName "MyResourceGroup" -Name "MyWindowsVM"

仮想マシンにタグが既に含まれている場合、リソースのすべてのタグが表示されます。

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

PowerShell でタグを追加する場合は、使用して、 `Set-azureresource` コマンドです。 PowerShell でタグを更新すると、タグ全体が更新されます。 そのため、既にタグが設定されているリソースに 1 つのタグを追加する場合、リソースに設定するすべてのタグを含める必要があります。 次に、PowerShell コマンドレットでリソースにタグを追加する例を示します。

この最初のコマンドレットでは、すべてに設定したタグの設定 *MyWindowsVM* に、 *タグ* 変数を使用して、 `Get-azureresource` と `タグ` 関数です。 注意パラメーター `ApiVersion` 省略可能です。 指定しない場合、リソース プロバイダーの最新の API バージョンを使用します。

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

3 番目のコマンドで、*tags* 変数にタグが追加されます。 新しいキーと値のペアを *tags* リストに付加するために **+=** を使用している点に注目してください。

        PS C:\> $tags +=@{Name="Location";Value="MyLocation"}

4 番目のコマンドでは、*tags* 変数に定義されているすべてのタグが、指定したリソースに設定されます。 この例では MyWindowsVM です。

        PS C:\> Set-AzureResource -Name MyWindowsVM -ResourceGroupName MyResourceGroup -ResourceType "Microsoft.Compute/VirtualMachines" -ApiVersion 2015-05-01-preview -Tag $tags

5 番目のコマンドでは、リソースのすべてのタグが表示されます。 次のように、値が *MyLocation* のタグとして *Location* の定義が追加されます。

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

PowerShell とは異なり、タグが格納されているリソースにタグを追加する場合、必要はありません (新旧) のすべてのタグを使用する前に指定する、 `azure vm セット` コマンドです。 代わりに、このコマンドを使用すると、リソースにタグが付加されます。 Azure CLI を使用して新しい VM タグを追加するには、使用することができます、 `azure vm セット` タグ パラメーターと共にコマンド **-t**:

        azure vm set -g MyResourceGroup -n MyVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

使用できるすべてのタグを削除する、 **– T** 内のパラメーター、 `azure vm セット` コマンドです。

        azure vm set – g MyResourceGroup –n MyVM -T

ここでは、PowerShell、Azure CLI、およびポータルを使用してリソースにタグを適用しました。次に、課金ポータルの使用量の詳細でタグを確認してみましょう。


## 使用量の詳細でタグを確認する

タグは、コンピューティング、ネットワークに配置されの使用状況の詳細に記憶域リソースを Azure リソース マネージャーが入力されます、 [課金ポータル](https://account.windowsazure.com/)します。

**[使用量の詳細のダウンロード]** をクリックして、サブスクリプションの使用量の詳細を表示します。

![Azure ポータルの使用量の詳細](./media/virtual-machines-tagging-arm/azure-portal-tags-usage-details.png)

課金明細と **[Version 2]** の使用量の詳細を選択します。

![Azure ポータルに表示される Version 2 プレビューの使用量の詳細](./media/virtual-machines-tagging-arm/azure-portal-version2-usage-details.png)

使用量の詳細の **[タグ]** 列には、すべてのタグが表示されます。

![Azure ポータルのタグ列](./media/virtual-machines-tagging-arm/azure-portal-tags-column.png)

これらのタグと使用量を分析すると、新たな視点で組織の使用データを調査できるようになります。


## その他のリソース

* [Azure リソース マネージャーの概要][]
* [タグを使用して、Azure リソースのを整理するには][]
* [Azure の請求:operator[] を理解します。][]
* [Microsoft Azure リソースの消費 [を把握します。][]





[powershell environment with azure resource manager]: ../powershell-azure-resource-manager.md 
[azure resource cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx 
[azure cli environment]: ./xplat-cli-azure-resource-manager.md 
[azure resource manager overview]: ../resource-group-overview.md 
[using tags to organize your azure resources]: ../resource-group-using-tags.md 
[understanding your azure bill]: ../billing-understand-your-bill.md 
[gain insights into your microsoft azure resource consumption]: ../billing-usage-rate-card-overview.md 

