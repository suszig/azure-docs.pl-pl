<properties
   pageTitle="リソース マネージャー テンプレートで Key Vault シークレットを使用する | Microsoft Azure"
   description="デプロイメント時にパラメーターとして Key Vault からシークレットを渡す方法について説明します。"
   services="azure-resource-manager,key-vault"
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
   ms.date="12/15/2015"
   ms.author="tomfitz"/>

# デプロイメント時にセキュリティで保護された値を渡す

シークレットとしてその値を格納するには展開時にパラメーターとして (パスワード) のようなセキュリティで保護された値を渡す必要がある場合、 [Azure Key Vault](./key-vault/key-vault-whatis.md) とその他のリソース マネージャー テンプレート内の値を参照します。 機密情報への参照のみが含まれます 
テンプレートでは、シークレットが公開されていませんし、手動で値シークレットを毎回入力する必要はありませんのでリソースがデプロイします。 どのユーザーを指定するか、 
サービス プリンシパルは、機密情報にアクセスできます。  

> [AZURE.NOTE] 現時点では、Azure CLI だけでは、資格情報コンテナーのシークレットを参照することができます。 できるだけ早く、Azure PowerShell にこの機能を追加する予定です。 

## Key Vault とシークレットのデプロイ

その他のリソース マネージャーのテンプレートから参照できる資格情報コンテナーを作成するに設定する必要があります、 **enabledForTemplateDeployment** プロパティを **true**, 、ユーザーにアクセス権を付与する必要がありますか 
機密情報を参照する展開を実行するサービス プリンシパル。

詳細については、資格情報コンテナーのキーとシークレットを展開する、次を参照してください。 
[資格情報コンテナー スキーマ](resource-manager-template-keyvault.md) と [Key vault のシークレット スキーマ](resource-manager-template-keyvault-secret.md)します。

## シークレットの参照

値をテンプレートに渡すパラメーター ファイル内から、シークレットを参照します。 シークレットを参照するには、Key Vault のリソース識別子とシークレットの名前を渡します。

    "parameters": {
        "adminPassword": {
            "reference": {
                "keyVault": {
                    "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
                }, 
                "secretName": "sqlAdminPassword" 
            } 
        }
    }

パラメーター ファイルの全体は次のような内容です。

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "sqlsvrAdminLogin": {
                "value": ""
            },
            "sqlsvrAdminLoginPassword": {
                "reference": {
                    "keyVault": {
                        "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
                    },
                    "secretName": "adminPassword"
                }
            }
        }
    }

機密情報を受け取るパラメーターは必ず、 **securestring**します。 次に、管理者のパスワードが必要な、SQL Server をデプロイするテンプレートの関連するセクションを示します。

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "sqlsvrAdminLogin": {
                "type": "string",
                "minLength": 4
            },
            "sqlsvrAdminLoginPassword": {
                "type": "securestring"
            },
            ...
        },
        "resources": [
        {
            "name": "[variables('sqlsvrName')]",
            "type": "Microsoft.Sql/servers",
            "location": "[resourceGroup().location]",
            "apiVersion": "2014-04-01-preview",
            "properties": {
                "administratorLogin": "[parameters('sqlsvrAdminLogin')]",
                "administratorLoginPassword": "[parameters('sqlsvrAdminLoginPassword')]"
            },
            ...
        }],
        "variables": {
            "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
        },
        "outputs": { }
    }




## 次のステップ

- Key vault の概要については、次を参照してください。 [Azure Key Vault を使ってみる](./key-vault/key-vault-get-started.md)します。
- テンプレートの展開についての詳細については、次を参照してください。 [Azure リソース マネージャー テンプレートを使用してアプリケーションを配置](resource-group-template-deploy.md)します。
- キーの機密情報を参照する完全な例については、次を参照してください。 [Key Vault 例](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)します。


