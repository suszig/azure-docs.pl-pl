<properties
   pageTitle="リソース マネージャーと従来のデプロイ モデルの違いを理解する"
   description="リソース マネージャーのデプロイ モデルと従来 (あるいはサービス管理) のデプロイ モデルの違いについて説明します。"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/02/2015"
   ms.author="tomfitz"/>

# リソース マネージャー デプロイと従来のデプロイを理解する

リソース マネージャーのデプロイ モデルは、アプリケーションを構築するサービスをデプロイし、管理するための新しい方法を提供します。 この新しいモデルが重要ですが含まれています 
従来のデプロイ モデルと 2 つのモデルとの違いは、互いに完全に互換性のある受けません。 展開を簡略化するには 
リソースの管理、Microsoft ことをお勧めする新しいリソースのリソース マネージャーを使用して、可能であれば、リソース マネージャーを通して既存のリソースを再展開します。

従来のデプロイ モデルはサービス管理モデルとしても知られています。

このトピックでは、2 つのモデルの違いと、従来のモデルからリソース マネージャーに移行するときに遭遇する問題について説明します。 
モデルの概要は説明しますが、個々のサービスの細かな違いについては取り上げません。 移行の詳細についてコンピューティング、ストレージ、および 
ネットワーク リソースをご覧ください [Azure コンピューティング、ネットワーク、記憶域プロバイダー Azure リソース マネージャーにおける](./virtual-machines/virtual-machines-azurerm-versus-azuresm.md)します。

多くのリソースは従来のモデルとリソース マネージャーの両方で問題なく動作します。 これらのリソースはリソース マネージャーを完全にサポートで作成された場合でも 
従来のモデルです。 何の心配もなく、特別な労力も必要とせず、リソース マネージャーに移行できます。

ただし、いくつかのリソース プロバイダーはモデル間の構造の違いに起因して 2 つのバージョンのリソースを提供します (従来のモデルに 1 つ、リソース マネージャーに 1 つ)。 
2 つのモデルを区別するリソース プロバイダー:

- Compute
- Storage
- Network

これらの書類のリソースの場合、サポートされる操作が異なるため、使用しているバージョンに注意してください。 

2 つのモデルのアーキテクチャ上の違いを理解するのを参照してください。 [Azure リソース マネージャーのアーキテクチャ](virtual-machines/virtual-machines-azure-resource-manager-architecture.md)します。

## リソース マネージャーの特性

リソース マネージャーで作成したリソースには次の特性があります。

- 次のいずれかのメソッドで作成:

  -  [Azure ポータル](https://portal.azure.com/)します。

        ![Azure portal](./media/resource-manager-deployment-model/preview-portal.png)

        For Compute, Storage, and Networking resources, you have the option of using either Resource Manager or Classic deployment. Select **Resource Manager**.

        ![Resource Manager deployment](./media/resource-manager-deployment-model/select-resource-manager.png)

  - Azure PowerShell バージョン 1.0 より前のコマンドで実行、 **AzureResourceManager** モードです。

            PS C:\> Switch-AzureMode -Name AzureResourceManager

  - Azure PowerShell 1.0 では、リソース マネージャー版のコマンドを使用してください。 これらのコマンドの形式である *動詞 AzureRm*, をクリックします。

            PS C:\> Get-AzureRmResourceGroupDeployment

  - [Azure リソース マネージャー REST API](https://msdn.microsoft.com/library/azure/dn790568.aspx) の REST 操作します。
  - Azure CLI コマンドの実行、 **arm** モードです。

            azure config mode arm

- リソースの種類を含まない **(クラシック)** 名前にします。 次の図の種類は **ストレージ アカウント**します。

    ![Web アプリ](./media/resource-manager-deployment-model/resource-manager-type.png)

## 従来のデプロイの特性

従来のデプロイで作成されたリソースには次の特性があります。

- 次のいずれかのメソッドで作成:

  - [クラシック ポータル](https://manage.windowsazure.com)

        ![Classic portal](./media/resource-manager-deployment-model/azure-portal.png)

        Or, the portal and you specify **Classic** deployment (for Compute, Storage, and Networking).

        ![Classic deployment](./media/resource-manager-deployment-model/select-classic.png)

  - 1.0 よりも前のバージョンの Azure PowerShell のコマンドの実行、 **AzureServiceManagement** モード (これは、その場合、既定のモードをしない AzureResourceManager に切り替えると具体的には、AzureServiceManagement モードで実行している)。

            PS C:\> Switch-AzureMode -Name AzureServiceManagement

  - Azure PowerShell 1.0 では、サービス管理版のコマンドを使用してください。 これらのコマンド名 **しない** 形式 *動詞 AzureRm*, 、次のようにします。

            PS C:\> Get-AzureDeployment

  - [サービス管理 REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) の REST 操作します。
  - Azure CLI コマンドの実行 **asm** または既定のモードです。
- リソースの種類を含む **(従来のもの)** 名前にします。 次の図の種類は **ストレージ アカウント (クラシック)**します。

    ![従来のタイプ](./media/resource-manager-deployment-model/classic-type.png)

ポータルを利用し、従来のデプロイで作成されたリソースを引き続き管理できます。

## リソース マネージャーとリソース グループを使用する利点

リソース マネージャーはリソース グループの概念を加えます。 リソース マネージャーで作成したリソースはすべてリソース グループ内に存在します。 リソース マネージャーのデプロイ モデルにはいくつかの利点があります。

- ソリューションのサービスを個別に処理するのではなく、すべてのサービスをデプロイ、管理、監視できます。
- アプリケーションをアプリのライフサイクルを通して繰り返しデプロイできます。常にリソースが一貫した状態でデプロイされます。
- 宣言型のテンプレートを利用し、デプロイを定義できます。 
- 正しい順序でデプロイされるようにリソース間の依存性を定義できます。
- ロールベースの Access Control (RBAC) が管理プラットフォームにネイティブ統合されるため、リソース グループのすべてのサービスにアクセス制御を適用できます。
- タグをリソースに適用し、サブスクリプションのすべてのリソースを論理的に整理できます。


リソース マネージャーの導入前は、従来のデプロイで作成されたリソースはリソース グループ内に存在しませんでした。 リソース マネージャーが追加されたとき、すべてのリソースが遡及的に既定のリソース グループに追加されました。 これで、従来のデプロイを使用して、リソースを作成する場合、リソースは 
展開時に、そのリソース グループを指定しなかった場合でも、そのサービスの既定のリソース グループ内で自動的に作成します。 ただし、リソース グループ内に存在するだけでは、リソースがリソース マネージャー モデルに変換されたことになりません。 Virtual Machines、Storage、および Virtual Networks の場合、リソースが従来のデプロイで作成された場合、引き続き従来の操作でそれを操作する必要があります。 

リソースを異なるリソース グループに移動したり、新しいリソースを既存のリソース グループに追加したりできます。 そのため、リソース グループを含めることができます、 
リソース マネージャーと従来のデプロイを使って作成されたリソースを混在させます。 このリソースの組み合わせが予期しない結果を作成できるリソース 
同じ操作をサポートしていません。

場合によっては、宣言型のテンプレートを利用し、デプロイのためにスクリプトを簡略化できます。 リソース マネージャーにサービスの管理からの既存のスクリプトを変換しようとすると、代わりにする、展開戦略を再動作を検討してください。 
テンプレートで、インフラストラクチャと構成を定義する手順を利用します。

## タグの使用

タグを利用すれば、リソースを論理的に整理できます。 リソース マネージャーで作成したリソースだけがタグに対応しています。 従来のリソースにタグを適用することはできません。

詳細については、リソース マネージャーでタグを使用して、次を参照してください。 [タグを使用した Azure リソースの整理に](resource-group-using-tags.md)します。

## デプロイ モデルで対応している操作

従来のデプロイ モデルで作成したリソースはリソース マネージャーの操作に対応していません。 場合によってで、リソース マネージャーのコマンドを取得できます。 
リソースに関する情報は、従来のデプロイを使用して作成または別のリソース グループを従来のリソースの移動などの管理タスクを実行できますが、 
このような場合は、種類がリソース マネージャーの操作をサポートしているという印象しないようにします。 たとえば、リソース グループ 
リソース マネージャーとクラシックで作成された仮想マシンを含むです。 次の PowerShell コマンドを実行すると、すべての Virtual Machines が表示されます。

    PS C:\> Get-AzureRmResourceGroup -Name ExampleGroup
    ...
    Resources :
     Name                 Type                                          Location
     ================     ============================================  ========
     ExampleClassicVM     Microsoft.ClassicCompute/domainNames          eastus
     ExampleClassicVM     Microsoft.ClassicCompute/virtualMachines      eastus
     ExampleResourceVM    Microsoft.Compute/virtualMachines             eastus
    ...

しかしながら、Get-AzureVM コマンドを実行すると、リソース マネージャーで作成した Virtual Machines のみが表示されます。

    PS C:\> Get-AzureVM -ResourceGroupName ExampleGroup
    ...
    Id       : /subscriptions/xxxx/resourceGroups/ExampleGroup/providers/Microsoft.Compute/virtualMachines/ExampleResourceVM
    Name     : ExampleResourceVM
    ...

一般的に、従来のデプロイで作成したリソースはリソース マネージャーのコマンドと連動しないと考えてください。

リソース マネージャーで作成したリソースを使用するとき、リソース マネージャーの操作を利用し、サービス管理の操作には切り替えないでください。

## Virtual Machines に関する考慮事項

Virtual Machines を使用するときには、重要な考慮事項がいくつかあります。

- 従来のデプロイ モデルで作成された仮想マシンはリソース マネージャーで作成された仮想ネットワークに含めることができません。
- リソース マネージャーのデプロイ モデルでデプロイされた仮想マシンは仮想ネットワークに含める必要があります。
- 従来のデプロイ モデルでデプロイされた仮想マシンは仮想ネットワークに含める必要がありません。

場合は、バーチャル マシンのダウンタイムが許さことができますできますから移行するに従来のデプロイとリソース マネージャーに、 [ASM2ARM PowerShell スクリプト](https://github.com/fullscale180/asm2arm)します。 

従来のデプロイからリソース マネージャーへの遷移するときに対応する Azure CLI コマンドの一覧は、次を参照してください。 [VM 操作に使用するリソース マネージャーおよびサービス管理コマンド](./virtual-machines/xplat-cli-azure-manage-vm-asm-arm.md)します。

移行の詳細についてコンピューティング、ストレージ、および 
ネットワーク リソースをご覧ください [Azure コンピューティング、ネットワーク、記憶域プロバイダー Azure リソース マネージャーにおける](./virtual-machines/virtual-machines-azurerm-versus-azuresm.md)します。

詳細については、仮想ネットワークを接続するさまざまな展開モデルから、次を参照してください。 [従来の Vnet を新しい Vnet に接続する](./virtual-network/virtual-networks-arm-asm-s2s.md)です。

## 次のステップ

- 宣言型の展開テンプレートの作成方法については、次を参照してください。 [Azure リソース マネージャーの作成テンプレート](resource-group-authoring-templates.md)します。
- テンプレートを展開するためのコマンドを表示するには、次を参照してください。 [Azure リソース マネージャー テンプレートを使用してアプリケーションを配置](resource-group-template-deploy.md)します。


