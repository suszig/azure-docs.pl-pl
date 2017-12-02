---
title: "Użyj Azure zarządzanych tożsamości usługi na serwerze usługi Azure API Management | Dokumentacja firmy Microsoft"
description: "Dowiedz się jak używać tożsamości zarządzanych usługi Azure w usłudze API Management"
services: api-management
documentationcenter: 
author: miaojiang
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 10/18/2017
ms.author: apimpm
ms.openlocfilehash: cf27e4d9997a796fa61af6e6f0af3c0c5a0c296f
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/02/2017
---
# <a name="use-azure-managed-service-identity-in-azure-api-management"></a>Użyj tożsamości usługi Azure zarządzanych w usłudze Azure API Management

> [!Note]
> Zarządzane tożsamości usługi dla usługi Azure API Management jest obecnie w przeglądzie.

W tym artykule przedstawiono sposób tworzenia tożsamości usługi zarządzanej dla wystąpienia usługi API Management oraz sposób uzyskiwać dostęp do innych zasobów. Tożsamość usługi zarządzanej generowane przez usługę Azure Active Directory (Azure AD) pozwala łatwo i bezpiecznie inne usługi Azure AD zasoby z chronionymi transakcjami, takie jak usługi Azure Key Vault dostępu do wystąpienia interfejsu API zarządzania. Ta tożsamość usługi zarządzanej jest zarządzany przez usługę Azure i nie trzeba zapewniać ani obrócić żadnych kluczy tajnych. Aby uzyskać więcej informacji o tożsamości usługi zarządzania Azure, zobacz [zarządzane tożsamość usługi Azure zasobów](../active-directory/msi-overview.md).

## <a name="create-an-api-management-instance-with-an-identity-by-using-a-resource-manager-template"></a>Utwórz wystąpienie interfejsu API zarządzania przy użyciu tożsamości za pomocą szablonu usługi Resource Manager

Można utworzyć wystąpienia interfejsu API zarządzania przy użyciu tożsamości przez uwzględnienie następujących właściwości w definicji zasobu: 

```json
"identity" : {
    "type" : "SystemAssigned"
}
```

Ta właściwość określa, że Azure do tworzenia i zarządzania tożsamościami dla swojego wystąpienia usługi API Management. 

Na przykład Pełna szablonu usługi Azure Resource Manager może wyglądać następujące czynności:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "parameters": {
        "serviceName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "The name of the api management service"
            }
        },
        "publisherEmail": {
            "type": "string",
            "minLength": 1,
            "defaultValue": "admin@contoso.com",
            "metadata": {
                "description": "The email address of the owner of the service"
            }
        },
        "publisherName": {
            "type": "string",
            "minLength": 1,
            "defaultValue": "Contoso",
            "metadata": {
                "description": "The name of the owner of the service"
            }
        },
        "sku": {
            "type": "string",
            "allowedValues": [
                "Developer",
                "Standard",
                "Premium"
            ],
            "defaultValue": "Developer",
            "metadata": {
                "description": "The pricing tier of this API Management service"
            }
        },
        "skuCount": {
            "type": "int",
            "defaultValue": 1,
            "metadata": {
                "description": "The instance size of this API Management service."
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2017-03-01",
            "name": "[parameters('serviceName')]",
            "type": "Microsoft.ApiManagement/service",
            "location": "[resourceGroup().location]",
            "tags": {},
            "sku": {
                "name": "[parameters('sku')]",
                "capacity": "[parameters('skuCount')]"
            },
            "properties": {
                "publisherEmail": "[parameters('publisherEmail')]",
                "publisherName": "[parameters('publisherName')]"
            },
            "identity": { 
                "type": "systemAssigned" 
            }
        }
    ]
}
```

## <a name="obtain-a-certificate-from-azure-key-vault"></a>Uzyskaj certyfikat z magazynu kluczy Azure

Poniższy przykład pokazuje, jak można uzyskać certyfikatu z usługi Azure Key Vault. Ten przewodnik zawiera następujące czynności:

1. Utwórz wystąpienie interfejsu API zarządzania przy użyciu tożsamości.
2. Aktualizowanie zasad dostępu do wystąpienia usługi Azure Key Vault i pozwolić wystąpienia interfejsu API zarządzania uzyskanie kluczy tajnych z niego.
3. Aktualizuj wystąpienie interfejsu API zarządzania przez ustawienie niestandardowej nazwy domeny za pomocą certyfikatu z wystąpienia usługi Key Vault.

### <a name="prerequisites"></a>Wymagania wstępne
Aby wykonać poniżej szablon arm potrzebujemy następujące 
1. Magazyn kluczy, zawierający certyfikat pfx w tej samej subskrypcji i tej samej grupie zasobów co usługa Api Management. To wymaganie szablon arm. 
2. Typ zawartości klucz tajny powinien być *application/x-pkcs12*. Poniższy skrypt można użyć w celu przekazania certyfikatu

```powershell
$pfxFilePath = "PFX_CERTIFICATE_FILE_PATH" # Change this path 
$pwd = "PFX_CERTIFICATE_PASSWORD" # Change this password 
$flag = [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable 
$collection = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2Collection 
$collection.Import($pfxFilePath, $pwd, $flag) 
$pkcs12ContentType = [System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12 
$clearBytes = $collection.Export($pkcs12ContentType) 
$fileContentEncoded = [System.Convert]::ToBase64String($clearBytes) 
$secret = ConvertTo-SecureString -String $fileContentEncoded -AsPlainText –Force 
$secretContentType = 'application/x-pkcs12' 
Set-AzureKeyVaultSecret -VaultName KEY_VAULT_NAME -Name KEY_VAULT_SECRET_NAME -SecretValue $Secret -ContentType $secretContentType
```

> [!Important]
> Jeśli nie podano wersji obiektu certyfikatu, zarządzanie interfejsami API automatycznie uzyskać nowsza wersja certyfikatu po przekazaniu do magazynu kluczy. 

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publisherEmail": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "The email address of the owner of the service"
            }
        },
        "publisherName": {
            "type": "string",
            "defaultValue": "Contoso",
            "minLength": 1,
            "metadata": {
                "description": "The name of the owner of the service"
            }
        },
        "sku": {
            "type": "string",
            "allowedValues": ["Developer",
            "Standard",
            "Premium"],
            "defaultValue": "Developer",
            "metadata": {
                "description": "The pricing tier of this API Management service"
            }
        },
        "skuCount": {
            "type": "int",
            "defaultValue": 1,
            "metadata": {
                "description": "The instance size of this API Management service."
            }
        },
        "keyVaultName": {
            "type": "string",
            "metadata": {
                "description": "Name of the vault"
            }
        },
        "proxyCustomHostname1": {
            "type": "string",
            "metadata": {
                "description": "Proxy Custom hostname."
            }
        },
        "keyVaultIdToCertificate": {
            "type": "string",
            "metadata": {
                "description": "Reference to the KeyVault certificate."
            }
        }
    },
    "variables": {
        "apiManagementServiceName": "[concat('apiservice', uniqueString(resourceGroup().id))]",
        "apimServiceIdentityResourceId": "[concat(resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName')),'/providers/Microsoft.ManagedIdentity/Identities/default')]"
    },
    "resources": [{
        "apiVersion": "2017-03-01",
        "name": "[variables('apiManagementServiceName')]",
        "type": "Microsoft.ApiManagement/service",
        "location": "[resourceGroup().location]",
        "tags": {
            
        },
        "sku": {
            "name": "[parameters('sku')]",
            "capacity": "[parameters('skuCount')]"
        },
        "properties": {
            "publisherEmail": "[parameters('publisherEmail')]",
            "publisherName": "[parameters('publisherName')]"
        },
        "identity": {
            "type": "systemAssigned"
        }
    },
    {
        "type": "Microsoft.KeyVault/vaults/accessPolicies",
        "name": "[concat(parameters('keyVaultName'), '/add')]",
        "apiVersion": "2015-06-01",        
      "dependsOn": [
        "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
      ],
        "properties": {
            "accessPolicies": [{
                "tenantId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').tenantId]",
                "objectId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').principalId]",
                "permissions": {
                    "secrets": ["get"]
                }
            }]
        }
    },
    { 
      "apiVersion": "2017-05-10", 
      "name": "apimWithKeyVault", 
      "type": "Microsoft.Resources/deployments",
      "dependsOn": [
        "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
      ],
      "properties": { 
        "mode": "incremental", 
        "templateLink": {
          "uri": "https://raw.githubusercontent.com/solankisamir/arm-templates/master/basicapim.keyvault.json",
          "contentVersion": "1.0.0.0"
        }, 
        "parameters": {
            "publisherEmail": { "value": "[parameters('publisherEmail')]"},
            "publisherName": { "value": "[parameters('publisherName')]"},
            "sku": { "value": "[parameters('sku')]"},
            "skuCount": { "value": "[parameters('skuCount')]"},
            "proxyCustomHostname1": {"value" : "[parameters('proxyCustomHostname1')]"},
            "keyVaultIdToCertificate": {"value" : "[parameters('keyVaultIdToCertificate')]"}
        }
      } 
    }]
}
```

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o Azure zarządzanych tożsamości usługi:

* [Zarządzane tożsamości usługi dla zasobów platformy Azure](../active-directory/msi-overview.md)
* [Szablony usługi Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates)

