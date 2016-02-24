<properties
   pageTitle="Key Vault のリソース マネージャー テンプレート | Microsoft Azure"
   description="Key Vault のリソース マネージャー スキーマについて説明します。"
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

# Key Vault テンプレートのスキーマ

Key Vault を作成します。

## スキーマの形式

Key Vault を作成するには、テンプレートのリソース セクションに次のスキーマを追加します。

    {
        "type": "Microsoft.KeyVault/vaults",
        "apiVersion": "2015-06-01",
        "name": string,
        "location": string,
        "properties": {
            "enabledForDeployment": bool,
            "enabledForTemplateDeployment": bool,
            "enabledForVolumeEncryption": bool,
            "tenantId": string,
            "accessPolicies": [
                {
                    "tenantId": string,
                    "objectId": string,
                    "permissions": {
                        "keys": [ keys permissions ],
                        "secrets": [ secrets permissions ]
                    }
                }
            ],
            "sku": {
                "name": enum,
                "family": "A"
            }
        },
        "resources": [
             child resources
        ]
    }

## 値

次の表では、スキーマに設定する必要がある値について説明します。

| 名前 | 型 | 必須 | 使用できる値 | 説明 |
| ---- | ---- | -------- | ---------------- | ----------- |
| type | enum | あり | **Microsoft.KeyVault/vaults** | 作成するリソースの種類。 |
| apiVersion | enum | あり | **2015-06-01** <br /> **2014年-12-19-プレビュー** | リソースの作成に使用する API バージョン。 | 
| name | string | はい |   | 作成する Key Vault 名。 Azure 全体で重複しない、一意の名前にしてください。 使用を検討して、 [uniqueString](resource-group-template-functions.md#uniquestring) 名前付け規則では、次の例で示すように動作します。 |
| location | string | あり | 有効な地域を確認するのを参照してください。 [リージョン](resource-manager-supported-services.md#supported-regions)します。  | Key Vault をホストするリージョン。 |
| properties | オブジェクト | あり | ([以下に示す](#properties)) | 作成する Key Vault の種類を指定するオブジェクト。 |
| resources | array | いいえ | [Key Vault シークレット](resource-manager-template-keyvault-secret.md)  | Key Vault の子リソース。 |

<a id="properties" />
### プロパティ オブジェクト

| 名前 | 型 | 必須 | 使用できる値 | 説明 |
| ---- | ---- | -------- | ---------------- | ----------- |
| enabledForDeployment | ブール値 | いいえ | **true** または **false** | 仮想マシン デプロイまたは Service Fabric デプロイで Key Vault が有効かどうかを指定します。 |
| enabledForTemplateDeployment | ブール値 | いいえ | **true** または **false** | リソース マネージャー テンプレート デプロイで Key Vault を使用できるかどうかを指定します。 詳細については、次を参照してください [の展開時にセキュリティで保護された値を渡す。](resource-manager-keyvault-parameter.md) |
| enabledForVolumeEncryption | ブール値 | いいえ | **true** または **false** | Key Vault でボリュームの暗号化が有効かどうかを指定します。 |
| tenantId | string | はい | グローバル一意識別子 | サブスクリプションのテナント識別子。 取得できる、 **Get AzureRMSubscription** PowerShell コマンドレット。 |
| accessPolicies | array | あり | ([以下に示す](#accesspolicies)) | ユーザーまたはサービス プリンシパルのアクセス許可を指定した、最大 16 オブジェクトの配列。 |
| sku | オブジェクト | あり | ([以下に示す](#sku)) | Key Vault の SKU。 |

<a id="accesspolicies" />
### properties.accessPolicies オブジェクト

| 名前 | 型 | 必須 | 使用できる値 | 説明 |
| ---- | ---- | -------- | ---------------- | ----------- |
| tenantId | string | はい | グローバル一意識別子 | 格納している Azure Active Directory テナントのテナント id、 **objectId** このアクセス ポリシーの |
| objectId | string | はい | グローバル一意識別子 | Key Vault に対するアクセス権が付与される AAD ユーザーまたはサービス プリンシパルのオブジェクト識別子。 いずれかの値を取得することができます、 **Get AzureRMADUser** または **Get AzureRMADServicePrincipal** コマンドレットです。 |
| アクセス許可 | オブジェクト | あり | ([以下に示す](#permissions)) | この Key Vault に付与される、Active Directory オブジェクトに対するアクセス許可。 |

<a id="permissions" />
### properties.accessPolicies.permissions オブジェクト

| 名前 | 型 | 必須 | 使用できる値 | 説明 |
| ---- | ---- | -------- | ---------------- | ----------- |
| キー | array | あり | 次の値のコンマ区切りの一覧:<br />**すべて**<br />**バックアップ**<br />**作成**<br />**を復号化**<br />**を削除**<br />**暗号化**<br />**を取得**<br />**インポート**<br />**] ボックスの一覧**<br />**復元**<br />**記号**<br />**unwrapkey**<br/>**更新**<br />**ことを確認**<br />**wrapkey** | この Key Vault 内のキーに付与される、Active Directory オブジェクトに対するアクセス許可。 この値は、許可される値の配列として指定する必要があります。 |
| secrets | array | あり | 次の値のコンマ区切りの一覧:<br />**すべて**<br />**削除**<br />**取得**<br />**リスト**<br />**設定** | この Key Vault 内のシークレットに付与される、Active Directory オブジェクトに対するアクセス許可。 この値は、許可される値の配列として指定する必要があります。 |

<a id="sku" />
### properties.sku オブジェクト

| 名前 | 型 | 必須 | 使用できる値 | 説明 |
| ---- | ---- | -------- | ---------------- | ----------- |
| name | enum | あり | **標準的な**<br />**premium** | 使用する KeyVault のサービス階層。  Standard は、シークレットとソフトウェアで保護されたキーをサポートしています。  Premium は、さらに HSM で保護されたキーもサポートしています。 |
| family | enum | あり | **A** | 使用する sku ファミリ。 
 
    
## 例

次に、Key Vault とシークレットをデプロイする例を示します。

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the vault"
                }
            },
            "tenantId": {
                "type": "string",
                "metadata": {
                   "description": "Tenant Id for the subscription and use assigned access to the vault. Available from the Get-AzureRMSubscription PowerShell cmdlet"
                }
            },
            "objectId": {
                "type": "string",
                "metadata": {
                    "description": "Object Id of the AAD user or service principal that will have access to the vault. Available from the Get-AzureRMADUser or the Get-AzureRMADServicePrincipal cmdlets"
                }
            },
            "keysPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
                }
            },
            "secretsPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to secrets in the vault. Valid values are: all, get, set, list, and delete."
                }
            },
            "vaultSku": {
                "type": "string",
                "defaultValue": "Standard",
                "allowedValues": [
                    "Standard",
                    "Premium"
                ],
                "metadata": {
                    "description": "SKU for the vault"
                }
            },
            "enabledForDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for VM or Service Fabric deployment"
                }
            },
            "enabledForTemplateDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for ARM template deployment"
                }
            },
            "enableVaultForVolumeEncryption": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for volume encryption"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[parameters('keyVaultName')]",
            "apiVersion": "2015-06-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "KeyVault"
            },
            "properties": {
                "enabledForDeployment": "[parameters('enabledForDeployment')]",
                "enabledForTemplateDeployment": "[parameters('enabledForTemplateDeployment')]",
                "enabledForVolumeEncryption": "[parameters('enableVaultForVolumeEncryption')]",
                "tenantId": "[parameters('tenantId')]",
                "accessPolicies": [
                {
                    "tenantId": "[parameters('tenantId')]",
                    "objectId": "[parameters('objectId')]",
                    "permissions": {
                        "keys": "[parameters('keysPermissions')]",
                        "secrets": "[parameters('secretsPermissions')]"
                    }
                }],
                "sku": {
                    "name": "[parameters('vaultSku')]",
                    "family": "A"
                }
            },
            "resources": [
            {
                "type": "secrets",
                "name": "[parameters('secretName')]",
                "apiVersion": "2015-06-01",
                "tags": { "displayName": "secret" },
                "properties": {
                    "value": "[parameters('secretValue')]"
                },
                "dependsOn": [
                    "[concat('Microsoft.KeyVault/vaults/', parameters('keyVaultName'))]"
                ]
            }]
        }]
    }


## 次のステップ

- Key vault の概要については、次を参照してください。 [Azure Key Vault を使ってみる](./key-vault/key-vault-get-started.md)します。
- テンプレートを展開するときに資格情報コンテナーのシークレットの参照の例は、次を参照してください。 [の展開時にセキュリティで保護された値を渡す](resource-manager-keyvault-parameter.md)します。


