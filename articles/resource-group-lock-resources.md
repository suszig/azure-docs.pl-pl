<properties 
    pageTitle="リソース マネージャーを使用したリソースのロック | Microsoft Azure" 
    description="すべてのユーザーとロールに制限を適用することで、ユーザーが特定のリソースの更新または削除を実行できないようにします。" 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/02/2015" 
    ms.author="tomfitz"/>


# Azure リソース マネージャーによるリソースのロック

管理者は、書き込み操作をコミットしたり、重要なリソースを誤って削除して、組織内の他のユーザーを防ぐためにサブスクリプション、リソース グループまたはリソースにロックを設定する、シナリオがあります。

Azure リソース マネージャーは、リソース管理のロックを通じて、リソースに対する操作を制限する機能を提供します。 ロックは、特定のスコープでのロックのレベルを適用するポリシーです。 スコープには、サブスクリプション、リソース グループまたはリソースを使用できます。 ロックのレベルでは、適用するポリシーの種類を識別します。現在、**CanNotDelete** と **ReadOnly** という 2 つの値があります。 **CanNotDelete** は、正規ユーザーはリソースの読み取りと変更を実行できますが、制限されているリソースは削除できないことを示します。 **ReadOnly** は、正規ユーザーはリソースから読み取りのみを実行できますが、制限されているリソースの変更または削除を実行できないことを示します。

ロックは、ロールベースのアクセス制御を使用して特定の操作を実行するアクセス許可をユーザーに割り当てる処理とは異なります。 ユーザーおよびロールのアクセス許可を設定する方法については、次を参照してください。 
[ポータルでのロールベースのアクセス制御](role-based-access-control-configure.md) と [の管理とリソースへのアクセスの監査](resource-group-rbac.md)します。 ロールベースのアクセス制御とは異なり、管理ロックを使用すると、すべてのユーザーとロールに対して制限を適用することができます。一般的に、ロックは指定した期間にのみ適用します。

## 一般的なシナリオ

一般的なシナリオの 1 つに、オンとオフを繰り返して使用されるリソースがリソース グループ内に存在する場合が挙げられます。 VM のリソースがオンになって定期的にデータを処理する一定の期間 
時間し、無効になっています。 このシナリオでは、Vm のシャット ダウンを有効するが、命令型であります。 
あるストレージ アカウントを削除できません。 このシナリオでは、ストレージ アカウントに対して、ロック レベルが **CanNotDelete** のリソース ロックを使用ことができます。

また、別のシナリオとして、運用システムに更新を適用しない時間帯を設定する場合もあります。 **ReadOnly** のロック レベルを使用すると、作成または更新を停止できます。 小売企業であれば、祝日のショッピング期間中に更新を許可しないようにすることができます。 金融サービス企業の方は場合、中のデプロイに関連する制約があります。 
時間をマーケット特定します。 リソース ロックでは、適切にリソースをロックするポリシーを提供できます。 これを、特定のリソースにのみ、またはリソース グループ全体に適用することができます。

## 組織でロックを作成または削除できるユーザー

を作成または管理のロックを削除するにはへのアクセスが必要 **Microsoft.Authorization/\*** または **Microsoft.Authorization/locks/\*** アクション。 組み込みのロールのうち、**所有者**と**ユーザー アクセスの管理者**にのみこれらのアクションが許可されています。 アクセス制御の割り当ての詳細については、次を参照してください。 [リソースへのアクセスを管理する](resource-group-rbac.md)します。

## ロックの継承

親スコープでロックを適用すると、すべての子リソースは同じロックを継承します。

1 つのリソースに複数のロックを適用すると、最も制限の厳しいロックが優先されます。 たとえば、親レベル (リソース グループなど) に **ReadOnly** を適用し、そのグループ内のリソースに **CanNotDelete** を適用すると、親のより制限の厳しいロック (ReadOnly) が優先されます。

## テンプレートでのロックの作成

次の例では、ストレージ アカウントに対するロックを作成するテンプレートを示します。 ロックを適用するストレージ アカウントは、パラメーターとして提供されており、ために使用します。 
concat() 関数と併用します。 この結果、リソース名は 'Microsoft.Authorization' とロックの名前 (この例では **myLock**) が連結された名前となります。

指定した種類は、リソースの種類に固有です。 ストレージの場合は、この種類は、"Microsoft.Storage/storageaccounts/providers/locks" です。

スコープ レベルは、リソースの **level** プロパティを使用して設定されます。 この例では、誤って削除されないことを重視しているため、レベルを **CannotDelete** に設定します。

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "lockedResource": {
                "type": "string"
            }
        },
        "resources": [
            {
                "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
                "type": "Microsoft.Storage/storageAccounts/providers/locks",
                "apiVersion": "2015-01-01",
                "properties": {
                    "level": "CannotDelete"
                }
            }
        ]
    }

## REST API によるロックの作成

展開済みのリソースをロックする、 [管理ロック用の REST API](https://msdn.microsoft.com/library/azure/mt204563.aspx)します。 REST API を使用すると、作成し、ロックを削除し、 
既存のロックに関する情報を取得します。

ロックを作成するには、次のように実行します。

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

スコープには、サブスクリプション、リソース グループまたはリソースを使用できます。 lock-name には、ロックの名前を指定できます。 api-version には、**2015-01-01** を使用します。

要求に、ロックのプロパティを指定する JSON オブジェクトを含めます。

    {
        "properties": {
            "level": {lock-level},
            "notes": "Optional text notes."
        }
    } 

ロックレベルには、**CanNotDelete** または **ReadOnly** を指定します。

例については、次を参照してください。 [管理ロック用の REST API](https://msdn.microsoft.com/library/azure/mt204563.aspx)します。

## Azure PowerShell を使用したロックの作成

[AZURE.INCLUDE [powershell-preview-inline-include](../includes/powershell-preview-inline-include.md)]

Azure PowerShell を使用してデプロイ済みのリソースをロックするには、次のように **New-AzureRmResourceLock** を使用します。 PowerShell で、**LockLevel** を**CanNotDelete** に設定するだけです。

    PS C:\> New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites

Azure PowerShell では、動作中のロックに対するその他のコマンドを使用できます。たとえば、ロックを更新する **Set-AzureRmResourceLock**、ロックを削除する **Remove-AzureRmResourceLock** があります。

## 次のステップ

- リソースのロックを使用した作業の詳細については、を参照してください [、Azure リソースのロック ダウン](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)。
- リソースを論理的に構成する詳細については、を参照してください [タグを使用して、リソースを整理する](resource-group-using-tags.md)。
- リソースが存在するリソース グループを変更するには、を参照してください [リソースを新しいリソース グループに移動](resource-group-move-resources.md)。
- カスタマイズしたポリシーを使用して、サブスクリプションの制約と規則を適用できます。 詳細については、次を参照してください。 [ポリシーを使ってリソースを管理し、アクセスを制御する](resource-manager-policy.md)します。





