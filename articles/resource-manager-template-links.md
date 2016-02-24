<properties
   pageTitle="リソースをリンクするためのリソース マネージャー テンプレート | Microsoft Azure"
   description="関連リソース間のリンクを作成するためのリソース マネージャー テンプレート スキーマを表示します。"
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
   ms.date="12/09/2015"
   ms.author="tomfitz"/>

# リソース リンク - テンプレート スキーマ

2 つのリソース間のリンクを作成します。 リンクは、ソース リソースと呼ばれるリソースに適用されます。 リンクの 2 番目のリソースは、ターゲット リソースと呼ばれます。

## スキーマの形式

リンクを作成するには、テンプレートのリソース セクションに次のスキーマを追加します。
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "targetId": string,
            "notes": string
        }
    }



## 値

次の表では、スキーマに設定する必要がある値について説明します。

| 名前 | 型 | 必須 | 使用できる値 | 説明 |
| ---- | ---- | -------- | ---------------- | ----------- |
| type | enum | あり | **{namespace}/{type}/providers/links** | 作成するリソースの種類。 {namespace} と {type} の値は、ソース リソースのプロバイダー名前空間とリソース タイプです。 |
| apiVersion | enum | あり | **2015-01-01** | リソースの作成に使用する API バージョン。 |  
| name | string | あり | **{resouce}/Microsoft.Resources/{linkname}**<br /><br />最大 64 文字<br /><>, % を含めることはできません (&)、?、いずれかの制御文字またはです。 | ソース リソースの名前とリンクの名前の両方を指定する値。 |
| dependsOn | array | いいえ |  リソース名またはリソースの一意識別子のコンマ区切りリスト。 | このリンクが依存するリソースのコレクション。 リンクしているリソースが同じテンプレートにデプロイされる場合、先にデプロイされるように、それらのリソース名をこの要素に追加します。 | 
| properties | オブジェクト | はい | (下記参照)  | リンク先のリソースを識別し、リンクに関するメモが記されたオブジェクト。 |  

### プロパティ オブジェクト

| 名前 | 型 | 必須 | 使用できる値 | 説明 |
| ------- | ---- | ---------------- | -------- | ----------- |
| targetId   | string | はい |   | リンク先のターゲット リソースの識別子。 |
| notes   | string | いいえ | 512 文字 | ロックの説明。 |


## リンク リソースの使用方法

リソースの依存関係がデプロイ後も続く場合、2 つのリソース間のリンクを適用します。 アプリが接続など、 
データベースは別のリソース グループです。 そのアプリからデータベースへのリンクを作成することで、その依存関係を定義できます。 リンクを使用すると、文書化します 
2 つのリソース間のリレーションシップ。 か、組織の他のユーザーがリソースの作業を発見へのリンクについてのリソースを照会した後で 
他のリソースです。

リンクされるリソースは、すべて同じサブスクリプションに属する必要があります。 各リソースは、そのリソース以外の 50 のリソースにリンクできます。 リンクされたリソースを削除または移動する場合、リンクの所有者は、残りのリンクをクリーンアップする必要があります。

残りの部分でリンクを使用する、次を参照してください。 [リンク済みリソース](https://msdn.microsoft.com/library/azure/mt238499.aspx)します。

次の Azure PowerShell コマンドを使用すると、サブスクリプションのすべてのリンクが表示されます。 他のパラメーターを指定し、結果を絞り込むことができます。

    Get-AzureRmResource -ResourceType Microsoft.Resources/links -isCollection -OutputObjectFormat New

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
                "type": "Microsoft.Web/serverfarms",
                "name": "[parameters('hostingPlanName')]",
                "location": "[resourceGroup().location]",
                "sku": {
                    "tier": "Free",
                    "name": "f1",
                    "capacity": 0
                },
                "properties": {
                    "numberOfWorkers": 1
                }
            },
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "dependsOn": [ "[parameters('hostingPlanName')]" ],
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                }
            },
            {
                "type": "Microsoft.Web/sites/providers/links",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
                    "notes": "This web site uses the storage account to store user information."
                }
            }
        ],
        "outputs": {}
    }


## 次のステップ

- テンプレートの構造については、次を参照してください。 [Azure リソース マネージャーの作成テンプレート](resource-group-authoring-templates.md)します。

