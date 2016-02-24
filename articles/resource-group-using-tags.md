<properties
    pageTitle="タグを使用した Azure リソースの整理 | Microsoft Azure"
    description="タグを適用して、課金や管理のためにリソースを整理する方法を示します。"
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="AzurePortal"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/02/2015"
    ms.author="tomfitz"/>


# タグを使用した Azure リソースの整理

リソース マネージャーを使用すると、タグを適用してリソースを理論的に整理できます。 タグは、定義したプロパティによりリソースを識別するキーと値のペアで構成されます。 同じカテゴリに属すリソースとしてマークするには、これらのリソースに同じタグを適用します。

特定のタグを持つリソースを表示した場合、すべてのリソース グループからリソースが表示されます。 同じリソース グループ内のリソースに限定されないため、デプロイ関係にとらわれずにリソースを整理できます。 タグは、課金または管理の目的でリソースを整理する必要がある場合に特に役立ちます。

リソースまたはリソース グループに追加する各タグは、サブスクリプション全体の分類に自動的に追加されます。 タグの名前でサブスクリプションの分類を事前設定し、リソースとして使用する値を後でタグ付けすることもできます。

各リソースまたはリソース グループには、最大で 15 個のタグを含めることができます。 タグ名は 512 文字まで、タグ値は 256 文字までに制限されます。

> [AZURE.NOTE] タグは、リソース マネージャーの操作をサポートするリソースにのみ適用できます。 バーチャル マシン、仮想ネットワークまたは記憶域を従来のデプロイ モデルを作成した場合 (など、Azure ポータルで、または [サービス管理 API](../services/api-management/))、そのリソースにタグを適用することはできません。 タグ付けが必要な場合は、リソース マネージャーを介してこれらのリソースを再デプロイする必要があります。 その他のすべてのリソースでは、タグ付けがサポートされています。

## テンプレート内のタグ

デプロイ時にタグをリソースに追加するのは非常に簡単です。 単に追加、 **タグ** リソースへの要素を展開しているしてタグの名前と値を指定します。 タグの名前と値は、サブスクリプションに事前に存在する必要はありません。 各リソースに対して、最大で 15 個のタグを指定できます。

次の例では、タグを使用したストレージ アカウントを示します。

    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2015-06-15",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "tags": {
                "dept": "Finance"
            },
            "properties": 
            {
                "accountType": "Standard_LRS"
            }
        }
    ]

## ポータルでのタグ

ポータルでリソースやリソース グループをタグ付けするのは簡単です。 [参照] ハブを使用してタグを付けるリソースまたはリソース グループに移動し、ブレードの上部にある [概要] セクションの [タグ] 部分をクリックします。

![リソース ブレードとリソース グループのブレードの [タグ] 部分](./media/resource-group-using-tags/tag-icon.png)

既に適用されているタグの一覧のブレードが開きます。 これが最初のタグの場合、一覧は空になっています。 タグを追加するには、名前と値を指定し、Enter キーを押すだけです。 いくつかのタグを追加すると、既存のタグの名前と値に基づくオートコンプリート オプションが機能します。これにより、リソース間で一貫性のある分類を適用でき、スペル ミスのような一般的なミスを回避することができます。

![名前と値のペアを持つタグ リソース](./media/resource-group-using-tags/tag-resources.png)

ポータル内のタグの分類を表示するには、[参照] ハブを使用してすべての項目を表示し、[タグ] を選択します。

![参照ハブによるタグの検索](./media/resource-group-using-tags/browse-tags.png)

最も重要なタグをスタート画面にピン留めしておくとすぐに利用できるため便利です。 どうぞお楽しみください。

![スタート画面にタグをピン留めする](./media/resource-group-using-tags/pin-tags.png)

## PowerShell を使用したタグ付け

[AZURE.INCLUDE [powershell-preview-inline-include](../includes/powershell-preview-inline-include.md)]

タグはリソースとどのようなタグが既に適用されているために、リソース グループに直接、リソースまたはリソース グループに単に取得できます **Get AzureRmResource** または **Get AzureRmResourceGroup**します。 それでは、リソース グループから始めましょう。

    PS C:\> Get-AzureRmResourceGroup tag-demo

    ResourceGroupName : tag-demo
    Location          : southcentralus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                    Actions  NotActions
                    =======  ==========
                    *

    Resources         :
                    Name                             Type                                  Location
                    ===============================  ====================================  ==============
                    CPUHigh ExamplePlan              microsoft.insights/alertrules         eastus
                    ForbiddenRequests tag-demo-site  microsoft.insights/alertrules         eastus
                    LongHttpQueue ExamplePlan        microsoft.insights/alertrules         eastus
                    ServerErrors tag-demo-site       microsoft.insights/alertrules         eastus
                    ExamplePlan-tag-demo             microsoft.insights/autoscalesettings  eastus
                    tag-demo-site                    microsoft.insights/components         centralus
                    ExamplePlan                      Microsoft.Web/serverFarms             southcentralus
                    tag-demo-site                    Microsoft.Web/sites                   southcentralus


このコマンドレットは、リソース グループに関して、適用されているタグを含むいくつかのメタデータを返します。 リソース グループをタグ付けするだけで使用して、 **セット AzureRmResourceGroup** コマンドを使用し、タグ名と値を指定します。

    PS C:\> Set-AzureRmResourceGroup tag-demo -Tag @( @{ Name="project"; Value="tags" }, @{ Name="env"; Value="demo"} )

    ResourceGroupName : tag-demo
    Location          : southcentralus
    ProvisioningState : Succeeded
    Tags              :
                    Name     Value
                    =======  =====
                    project  tags
                    env      demo

タグは全体として更新されるため、既にタグが付けられているリソースにタグをもう 1 つ追加する場合は、保持しておきたいすべてのタグを含む配列を使用する必要があります。 これを行うために、まず既存のタグを選択して、新しいタグを 1 つ追加することができます。

    PS C:\> $tags = (Get-AzureRmResourceGroup -Name tag-demo).Tags
    PS C:\> $tags += @{Name="status";Value="approved"}
    PS C:\> Set-AzureRmResourceGroup tag-demo -Tag $tags

    ResourceGroupName : tag-demo
    Location          : southcentralus
    ProvisioningState : Succeeded
    Tags              :
                    Name     Value
                    =======  ========
                    project  tags
                    env      demo
                    status   approved


特定のタグを 1 つ以上削除するには、削除するタグが含まれない配列を保存します。

プロセスは、同じリソースについてを使用して点を除いて、 **Get AzureRmResource** と **セット AzureRmResource** コマンドレットです。 

特定のタグを持つリソース グループを取得する **検索 AzureRmResourceGroup** コマンドレットと、 **-タグ** パラメーター。

    PS C:\> Find-AzureRmResourceGroup -Tag @{ Name="env"; Value="demo" } | %{ $_.ResourceGroupName }
    rbacdemo-group
    tag-demo

Azure PowerShell 1.0 よりも前のバージョンで特定のタグが付けられたリソースを取得するには、次のコマンドを使用します。

    PS C:\> Get-AzureResourceGroup -Tag @{ Name="env"; Value="demo" } | %{ $_.ResourceGroupName }
    rbacdemo-group
    tag-demo
    PS C:\> Get-AzureResource -Tag @{ Name="env"; Value="demo" } | %{ $_.Name }
    rbacdemo-web
    rbacdemo-docdb
    ...    

PowerShell を使用してサブスクリプション内のすべてのタグの一覧を取得するには、使用、 **Get AzureRmTag** コマンドレットです。

    PS C:/> Get-AzureRmTag
    Name                      Count
    ----                      ------
    env                       8
    project                   1

"hidden-" や "link:" で始まるタグが表示される場合があります。 これらは内部タグです。これらのタグは単に無視して、変更しないでください。

使用して、 **新規 AzureRmTag** コマンドレットは、分類に新しいタグを追加します。 これらのタグは、リソースまたはリソース グループにまだ適用されていない場合でもオートコンプリートに含められます。 タグの名前と値を削除するタグを削除も一緒に使用する、リソースを使用してから、 **削除 AzureRmTag** コマンドレットを分類から削除します。

## REST API を使用したタグ付け

ポータルと PowerShell の両方を使用して、 [リソース マネージャー REST API](https://msdn.microsoft.com/library/azure/dn848368.aspx) バック グラウンドでします。 別の環境にタグ付けを統合する必要がある場合、リソース ID に対する GET 操作でタグを取得し、PATCH 呼び出しでタグのセットを更新できます。


## タグ付けと課金

サービスがサポートされていれば、タグを使用して課金データをグループ化できます。 たとえば、 [統合された Azure リソース マネージャーでの Virtual Machines](/virtual-machines/virtual-machines-azurerm-versus-azuresm.md) を有効にします。
定義して、仮想マシンの課金データを整理するタグを適用できます。 異なる組織向けに複数の VM を実行している場合は、タグを使用すると、コスト センターごとに課金データをグループ化できます。  
また、タグを使用すると、運用環境で実行されている VM の課金データなどの、ランタイム環境ごとにコストを分類することもできます。

使ってタグに関する情報を取得できます、 [Azure Resource Usage api と RateCard Api](billing-usage-rate-card-overview.md) またはからダウンロードできる使用状況のコンマ区切り値 (CSV) ファイル
 [Azure へようこそ](https://account.windowsazure.com/) または [EA ポータル](https://ea.azure.com)します。 課金情報へのプログラムによるアクセスの詳細については、次を参照してください。 [Microsoft Azure のリソース消費を把握](billing-usage-rate-card-overview.md)します。 REST API 操作は、次を参照してください。 [Azure Billing REST API リファレンス](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c)します。

タグが表示課金のタグをサポートするサービスの使用状況 CSV をダウンロードすると、 **タグ** 列です。 詳細については、次を参照してください。 [Microsoft Azure の課金内容](billing-understand-your-bill.md)します。

![課金タグを参照してください](./media/resource-group-using-tags/billing_csv.png)

## 次のステップ

- カスタマイズしたポリシーを使用して、サブスクリプションの制約と規則を適用できます。 定義するポリシーには、場合によっては、すべてのリソースに特定のタグを設定することが必要になります。 詳細については、次を参照してください。 [ポリシーを使ってリソースを管理し、アクセスを制御する](resource-manager-policy.md)です。
- Azure PowerShell を使用してリソースをデプロイするときの概要については、次を参照してください。 [Azure リソース マネージャーで Azure PowerShell を使用して](./powershell-azure-resource-manager.md)します。
- リソースをデプロイするときに Azure CLI の使用の概要については、次を参照してください。 [Mac、Linux、および Azure リソース管理で Windows 用 Azure CLI の使用](./xplat-cli-azure-resource-manager.md)します。
- ポータルの使用の概要については、次を参照してください [Azure ポータルを使用して、Azure リソースを管理するには。](./resource-group-portal.md)  

