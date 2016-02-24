<properties
   pageTitle="ストレージのリソース マネージャー テンプレート | Microsoft Azure"
   description="ストレージ アカウントのリソース マネージャーのスキーマを示します。"
   services="azure-resource-manager,storage"
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

# ストレージ アカウント - テンプレート スキーマ

ストレージ アカウントを作成します。

## スキーマの形式

ストレージ アカウントを作成するには、テンプレートのリソース セクションに次のスキーマを追加します。

    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2015-06-15",
        "name": string,
        "location": string,
        "properties": 
        {
            "accountType": string
        }
    }

## 値

次の表では、スキーマに設定する必要がある値について説明します。

| 名前 | 型 | 必須 | 使用できる値 | 説明 |
| ---- | ---- | -------- | ---------------- | ----------- |
| type | enum | あり | **Microsoft.Storage/storageAccounts** | 作成するリソースの種類。 |
| apiVersion | enum | あり | **2015-06-15** <br /> **2015年-05-01-プレビュー** | リソースの作成に使用する API バージョン。 | 
| name | string | はい | 3 ～ 24 文字、数字と小文字のみ  | 作成するストレージ アカウントの名前。 Azure 全体で重複しない、一意の名前にしてください。 使用を検討して、 [uniqueString](resource-group-template-functions.md#uniquestring) 名前付け規則では、次の例で示すように動作します。 |
| location | string | あり | 有効な地域を確認するのを参照してください。 [リージョン](resource-manager-supported-services.md#supported-regions)します。  | ストレージ アカウントをホストするリージョン。 |
| properties | オブジェクト | はい | (下記参照) | 作成するストレージ アカウントの種類を指定するオブジェクト。

### プロパティ オブジェクト

| 名前 | 型 | 必須 | 使用できる値 | 説明 |
| ---- | ---- | -------- | ---------------- | ----------- |
| accountType | string | あり | **Standard_LRS**<br />**が standard_zrs の場合**<br />**Standard_GRS**<br />**Standard_RAGRS**<br />**Premium_LRS** | ストレージ アカウントの種類。 使用できる値は、Standard ローカル冗長、Standard ゾーン冗長、Standard geo 冗長、Standard 読み取りアクセス geo 冗長、および Premium ローカル冗長に対応しています。 この種類のアカウントについては、次を参照してください。 [Azure ストレージのレプリケーション](./storage/storage-redundancy.md )します。 |

    
## 例

次の例では、リソース グループの ID に基づく一意の名前を持つ、標準のローカル冗長ストレージ アカウントをデプロイします。

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2015-06-15",
                "name": "[concat('storage', uniqueString(resourceGroup().id))]",
                 "location": "[resourceGroup().location]",
                 "properties": 
                 {
                      "accountType": "Standard_LRS"
                 }
            }
        ],
        "outputs": {}
    }

## 次のステップ

- 記憶域の概要については、次を参照してください。 [Microsoft Azure Storage の概要](./storage/storage-introduction.md)します。
- たとえば、新しいストレージ アカウントを使用して、バーチャル マシンとテンプレートを参照してください [単純な Linux VM をデプロイ](https://azure.microsoft.com/documentation/templates/101-simple-linux-vm/) または [単純な Windows VM をデプロイ](https://azure.microsoft.com/documentation/templates/101-simple-windows-vm/)します。

