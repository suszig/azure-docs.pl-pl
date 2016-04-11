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

Azure リソース マネージャーは、リソース管理のロックを通じて、リソースに対する操作を制限する機能を提供します。 ロックは、特定のスコープでのロックのレベルを適用するポリシーです。 スコープには、サブスクリプション、リソース グループまたはリソースを使用できます。 ロック レベルが、ポリシーのうち 2 つの値を持つ現在の適用の種類を識別する **CanNotDelete** と **ReadOnly**します。 **CanNotDelete** の制限付きのリソースを削除することはできませんが、承認されたユーザーがまだ読み取りおよび、リソースを変更することを意味します。 **読み取り専用** 手段が承認されたユーザーは、リソースからのみ読み取ることができますが、それらを変更または制限付きのリソースを削除できません。

ロックは、ロールベースのアクセス制御を使用して特定の操作を実行するアクセス許可をユーザーに割り当てる処理とは異なります。 ユーザーおよびロールのアクセス許可を設定する方法については、次を参照してください。 
[ポータルでのロールベースのアクセス制御](role-based-access-control-configure.md) と [の管理とリソースへのアクセスの監査](resource-group-rbac.md)します。 ロールベースのアクセス制御とは異なり、管理ロックを使用すると、すべてのユーザーとロールに対して制限を適用することができます。一般的に、ロックは指定した期間にのみ適用します。

## 一般的なシナリオ

一般的なシナリオの 1 つに、オンとオフを繰り返して使用されるリソースがリソース グループ内に存在する場合が挙げられます。  VM のリソースがオンになって定期的にデータを処理する一定の期間 
時間し、無効になっています。 このシナリオでは、Vm のシャット ダウンを有効するが、命令型であります。 
あるストレージ アカウントを削除できません。 このシナリオでのロック レベルを持つリソース ロックを使用するが **CanNotDelete** ストレージ アカウントにします。

また、別のシナリオとして、運用システムに更新を適用しない時間帯を設定する場合もあります。  **ReadOnly** ロック レベルを作成または更新を停止します。 小売企業であれば、祝日のショッピング期間中に更新を許可しないようにすることができます。  金融サービス企業の方は場合、中のデプロイに関連する制約があります。 
時間をマーケット特定します。 リソース ロックでは、適切にリソースをロックするポリシーを提供できます。 これを、特定のリソースにのみ、またはリソース グループ全体に適用することができます。

## 組織でロックを作成または削除できるユーザー

を作成または管理のロックを削除するにはへのアクセスが必要 **Microsoft.Authorization/\*** または **Microsoft.Authorization/locks/\*** アクション。 のみ組み込みロールの **所有者** と **ユーザー アクセスの管理者** それらのアクションが付与されます。 アクセス制御の割り当ての詳細については、次を参照してください。 [リソースへのアクセスを管理する](resource-group-rbac.md)です。

## ロックの継承

親スコープでロックを適用すると、すべての子リソースは同じロックを継承します。

1 つのリソースに複数のロックを適用すると、最も制限の厳しいロックが優先されます。 適用する場合など **読み取り専用** (リソース グループなど) の親レベルと **CanNotDelete** のリソース グループ内にある親からの強いロック (読み取り専用) が優先されます。

## テンプレートでのロックの作成

次の例では、ストレージ アカウントに対するロックを作成するテンプレートを示します。 ロックを適用するストレージ アカウントは、パラメーターとして提供されており、ために使用します。 
concat() 関数と併用します。  結果、リソース名は 'microsoft.authorization' とロックの名をここで **myLock**します。

指定した種類は、リソースの種類に固有です。 ストレージの場合は、この種類は、"Microsoft.Storage/storageaccounts/providers/locks" です。

使用して、スコープ レベルを設定、 **レベル** 、リソースのプロパティです。 例が誤って削除されないことに重点を置いて、レベル設定 **CannotDelete**します。

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

スコープには、サブスクリプション、リソース グループまたはリソースを使用できます。 lock-name には、ロックの名前を指定できます。 Api バージョンを使用して **2015年-01-01**します。

要求に、ロックのプロパティを指定する JSON オブジェクトを含めます。

    {
        "properties": {
            "level": {lock-level},
            "notes": "Optional text notes."
        }
    } 

ロック レベルを行うためには、どちらかを指定 **CanNotDelete** または **ReadOnly**します。

例については、次を参照してください。 [管理ロック用の REST API](https://msdn.microsoft.com/library/azure/mt204563.aspx)します。

## Azure PowerShell を使用したロックの作成

[AZURE.INCLUDE [powershell-preview-inline-include](../includes/powershell-preview-inline-include.md)]

Azure PowerShell を使用したデプロイ済みのリソースをロックするにを使用して、 **新規 AzureRmResourceLock** 次のようにします。 PowerShell からしか設定できない、 **LockLevel** に **CanNotDelete**します。

    PS C:\> New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites

Azure PowerShell ではその他のコマンドを作業ロックなど **セット AzureRmResourceLock** 、ロックを更新して **削除 AzureRmResourceLock** ロックを削除します。

## 次のステップ

- リソースのロックを使用した作業の詳細については、次を参照してください [、Azure リソースのロック ダウン。](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
- リソースを論理的に構成する詳細については、「 [タグを使用して、リソースを整理するには](resource-group-using-tags.md)
- リソースが存在するリソース グループを変更するを参照してください。 [リソースを新しいリソース グループに移動します。](resource-group-move-resources.md)
- カスタマイズしたポリシーを使用して、サブスクリプションの制約と規則を適用できます。 詳細については、次を参照してください。 [ポリシーを使ってリソースを管理し、アクセスを制御する](resource-manager-policy.md)です。

