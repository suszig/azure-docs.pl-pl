<properties
   pageTitle="ロール割り当て用のリソース マネージャー テンプレート | Microsoft Azure"
   description="デプロイ時にロールの割り当てを作成するためのリソース マネージャーのスキーマを示しています。"
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
   ms.date="11/10/2015"
   ms.author="tomfitz"/>

# ロールの割り当て: テンプレート スキーマ

ロールに指定したスコープで、ユーザー、グループ、またはサービス プリンシパルを割り当てます。

## スキーマの形式

ロールの割り当てを作成するには、テンプレートのリソース セクションに次のスキーマを追加します。
    
    {
        "type": "Microsoft.Authorization/roleAssignments",
        "apiVersion": "2015-07-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "roleDefinitionId": string,
            "principalId": string,
            "scope": string
        }
    }

## 値

次の表では、スキーマに設定する必要がある値について説明します。

| 名前 | 型 | 必須 | 使用できる値 | 説明 |
| ---- | ---- | -------- | ---------------- | ----------- |
| type | enum | あり | **Microsoft.Authorization/roleAssignments** | 作成するリソースの種類。 |
| apiVersion | enum | あり | **2015-07-01** | リソースの作成に使用する API バージョン。 |  
| name | string | はい | グローバル一意識別子 | 新しいロールの割り当て用の識別子。 |
| dependsOn | array | いいえ |  リソース名またはリソースの一意識別子のコンマ区切りリスト。 | このロールの割り当てが依存するリソースのコレクション。 あるリソースに範囲指定されたロールを割り当てるとき、そのリソースが同じテンプレートにデプロイされている場合は、そのリソースが最初にデプロイされるように、この要素にそのリソース名を含めます。 | 
| properties | オブジェクト | はい | (下記参照)  | ロール定義、プリンシパルおよびスコープを識別するオブジェクト。 |  

### プロパティ オブジェクト

| 名前 | 型 | 必須 | 使用できる値 | 説明 |
| ------- | ---- | ---------------- | -------- | ----------- |
| roleDefinitionId   | string | あり | **/subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}**  | ロールの割り当てで使用する既存のロール定義識別子。 |
| principalId   | string | はい | グローバル一意識別子 | 既存のプリンシパルの識別子。 これは、ディレクトリ内の ID にマップされ、ユーザー、サービス プリンシパルまたはセキュリティ グループを指すことができます。 |
| scope | string | あり | リソース グループ。<br />**/subscriptions/{subscription-id}-/resourcegroups/{リソース グループ名}**<br /><br />リソース。<br />**/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}** | このロールが割り当てられるスコープ。 |


## ロック リソースの使用方法

デプロイ時にロールに、ユーザー、グループ、またはサービス プリンシパルを追加する必要がある場合に、テンプレートにロール割り当てを追加します。 ロールの割り当てがそのためのより高いレベルのスコープから継承されます。 
サブスクリプション レベルのロールに既にプリンシパルを追加した場合は、リソース グループまたはリソースの再割り当てする必要はありません。

新しい識別子を生成する **名** とします。

    PS C:\> $name = [System.Guid]::NewGuid().toString()

ロール定義用のグローバル一意識別子を取得するには、次を使用します。

    PS C:\> $roledef = (Get-AzureRmRoleDefinition -Name Reader).id

プリンシパル用の ID を取得するには、次のいずれかのコマンドを使用します。

という名前のグループの **監査担当者**:

    PS C:\> $principal = (Get-AzureRmADGroup -SearchString Auditors).id

という名前のユーザーの **exampleperson**:

    PS C:\> $principal = (Get-AzureRmADUser -SearchString exampleperson).id

名前付きサービス プリンシパルの **exampleapp**:

    PS C:\> $principal = (Get-AzureRmADServicePrincipal -SearchString exampleapp).id 
 

## 例

次の例では、リソース グループ用のロールをグループに割り当てます。

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "roleDefinitionId": {
                "type": "string"
            },
            "roleAssignmentId": {
                "type": "string"
            },
            "principalId": {
                "type": "string"
            }
        },
        "resources": [
            {
              "type": "Microsoft.Authorization/roleAssignments",
              "apiVersion": "2015-07-01",
              "name": "[parameters('roleAssignmentId')]",
              "properties":
              {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]",
                "scope": "[concat(subscription().id, '/resourceGroups/', resourceGroup().name)]"
              }
            }
        ],
        "outputs": {}
    }


## 次のステップ

- テンプレートの構造については、次を参照してください。 [Azure リソース マネージャーの作成テンプレート](resource-group-authoring-templates.md)します。
- ロールベースのアクセス制御の詳細については、次を参照してください。 [Azure Active Directory のロール ベースのアクセス制御](active-directory/role-based-access-control-configure.md)します。


