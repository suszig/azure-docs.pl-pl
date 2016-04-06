<properties
   pageTitle="リソース ロックのリソース マネージャー テンプレート | Microsoft Azure"
   description="リソース ロックのリソース マネージャーのスキーマを示しています。"
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
   ms.date="10/25/2015"
   ms.author="tomfitz"/>

# リソース ロック - テンプレート スキーマ

リソースとその子リソースに対応する新しいロックを作成します。

## スキーマの形式

ロックを作成するには、テンプレートのリソース セクションに次のスキーマを追加します。
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "level": enum,
            "notes": string
        }
    }



## 値

次の表では、スキーマに設定する必要がある値について説明します。

| 名前 | 型 | 必須 | 使用できる値 | 説明 |
| ---- | ---- | -------- | ---------------- | ----------- |
| type | enum | あり | リソースの場合。 <br />**{名前空間}/{型}/プロバイダー/ロック**<br /><br />リソース グループ。<br />**Microsoft.Authorization/locks** | 作成するリソースの種類。 |
| apiVersion | enum | あり | **2015-01-01** | リソースの作成に使用する API バージョン。 |  
| name | string | あり | リソースの場合。<br />**{resouce}/Microsoft.Authorization/{lockname}**<br /><br />リソース グループ。<br />**{lockname}**<br /><br />最大 64 文字<br /><>, % を含めることはできません (&)、?、いずれかの制御文字またはです。 | 両方のリソース ロックおよびロックの名前を指定する値します。 |
| dependsOn | array | いいえ |  リソース名またはリソースの一意識別子のコンマ区切りリスト。 | このロックは、[依存先のリソースのコレクションです。 ロックしているリソースが同じテンプレートで展開されている場合は、リソースが最初に展開されていることを確認するには、この要素にそのリソース名を含めます。 | 
| プロパティ | オブジェクト | はい | (下記参照)  | ロックとロックに関する注意事項の種類を識別するオブジェクト。 |  

### プロパティ オブジェクト

| 名前 | 型 | 必須 | 使用できる値 | 説明 |
| ------- | ---- | ---------------- | -------- | ----------- |
| level   | enum | あり | **CannotDelete** <br /> **ReadOnly**  | スコープに適用するロックの種類。 CanNotDelete は変更はできますが削除はできません。ReadOnly は変更も削除もできません。 |
| notes   | string | いいえ | 512 文字 | ロックの説明。 |


## ロック リソースの使用方法

リソースに対する指定したアクションを防ぐには、このリソースをテンプレートに追加します。 ロックは、すべてのユーザーとグループに適用されます。 通常は、プロセスの実行中に、組織内の他のユーザーが誤ってリソースを変更したり削除したりしないように、一定の期間のみロックを適用します。

を作成または管理のロックを削除するにはへのアクセスが必要 **Microsoft.Authorization/*** または **Microsoft.Authorization/locks/*** アクション。 組み込みのロールののみ **所有者** と **ユーザー アクセスの管理者** は 
これらの操作を許可します。 役割に基づいたアクセス制御の詳細については、次を参照してください。 [リソースへのアクセスを管理する](resource-group-rbac.md)です。

ロックは、指定されたリソースとすべての子リソースに適用されます。 1 つのリソースに複数のロックを適用すると、最も制限の厳しいロックが優先されます。 読み取り専用でを適用する場合など、 
(リソース グループなど) の親レベルとそのグループ内のリソースに CanNotDelete、親からの強いロック (読み取り専用) が優先されます。 

PowerShell コマンドを使用してロックを解除する **削除 AzureRmResourceLock** 、または、 [削除操作](https://msdn.microsoft.com/library/azure/mt204562.aspx) REST API のです。

## 例

次の例では、Web アプリに読み取り専用のロックが適用されます。

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "hostingPlanName": {
                "type": "string"
            }
        },
        "variables": {
            "siteName": "[concat('site',uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                },
            },
            {
                "type": "Microsoft.Web/sites/providers/locks",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties":
                {
                    "level": "ReadOnly",
                    "notes": "my notes"
                }
             }
        ],
        "outputs": {}
    }

次の例では、リソース グループに読み取り専用のロックが適用されます。

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Authorization/locks",
                "apiVersion": "2015-01-01",
                "name": "MyGroupLock",
                "properties":
                {
                    "level": "ReadOnly",
                    "notes": "my notes"
                }
            }
        ],
        "outputs": {}
    }

## 次のステップ

- テンプレートの構造については、次を参照してください。 [Azure リソース マネージャーの作成テンプレート](resource-group-authoring-templates.md)します。
- ロックの詳細については、次を参照してください。 [Azure リソース マネージャーによるリソースのロック](resource-group-lock-resources.md)します。


