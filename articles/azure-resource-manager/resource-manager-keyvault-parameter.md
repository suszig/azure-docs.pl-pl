---
title: "Klucz tajny usługi Key Vault z szablonu usługi Resource Manager | Dokumentacja firmy Microsoft"
description: "Przedstawia sposób przekazywania klucza tajnego z magazynu kluczy jako parametr podczas wdrażania."
services: azure-resource-manager,key-vault
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c582c144-4760-49d3-b793-a3e1e89128e2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.openlocfilehash: 1ca72599e67e79d42a3d430dbb13e89ea7265334
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Przekaż wartość parametru bezpieczne podczas wdrażania za pomocą usługi Key Vault

Gdy należy przekazać wartość bezpiecznego (na przykład hasło) jako parametr podczas wdrażania można pobrać wartości z [usługi Azure Key Vault](../key-vault/key-vault-whatis.md). Możesz pobrać wartość przez odwołanie do magazynu kluczy i klucz tajny w pliku parametrów. Wartość nigdy nie jest uwidaczniana, ponieważ można odwoływać się tylko jego identyfikator magazynu kluczy. Nie trzeba ręcznie wprowadź wartość klucz tajny w każdym wdrożeniu zasobów. Magazyn kluczy może istnieć w innej subskrypcji niż grupa zasobów, które wdrażasz. Podczas odwoływania się do magazynu kluczy, obejmują identyfikatora subskrypcji.

Podczas tworzenia magazynu kluczy, skonfigurować *enabledForTemplateDeployment* właściwości *true*. Ustawiając tę wartość na true, należy zezwolić na dostęp z szablonów usługi Resource Manager podczas wdrażania.  

## <a name="deploy-a-key-vault-and-secret"></a>Wdrażanie magazynu kluczy oraz klucz tajny

Aby utworzyć magazyn kluczy i klucz tajny, należy użyć wiersza polecenia platformy Azure lub programu PowerShell. Zwróć uwagę, że magazynu kluczy jest włączona dla wdrożenia szablonu. 

W przypadku interfejsu wiersza polecenia platformy Azure użyj polecenia:

```azurecli
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create --name $vaultname --resource-group examplegroup --location 'South Central US' --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

W przypadku programu PowerShell użyj polecenia:

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault -VaultName $vaultname -ResourceGroupName examplegroup -Location "South Central US" -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-to-the-secret"></a>Zapewnianie dostępu do klucza tajnego

Czy używasz nowego magazynu kluczy lub istniejącego upewnij się, że użytkownik wdrażanie szablonu może uzyskać dostęp klucz tajny. Wdrażanie szablonu, który odwołuje się do klucza tajnego użytkownik musi mieć `Microsoft.KeyVault/vaults/deploy/action` uprawnienia dla magazynu kluczy. [Właściciela](../active-directory/role-based-access-built-in-roles.md#owner) i [współautora](../active-directory/role-based-access-built-in-roles.md#contributor) ról zarówno udzielić dostępu. Można również utworzyć [niestandardowej roli zabezpieczeń](../active-directory/role-based-access-control-custom-roles.md) daje to uprawnienie i dodać do tej roli użytkownika. Aby uzyskać informacje dotyczące dodawania użytkownika do roli, zobacz [przypisać użytkownika do ról administratora w usłudze Azure Active Directory](../active-directory/active-directory-users-assign-role-azure-portal.md).


## <a name="reference-a-secret-with-static-id"></a>Odwołanie klucza tajnego o identyfikatorze statyczne

Szablon, który odbiera klucz tajny magazynu kluczy jest podobnie jak każdy inny szablon. Jest to spowodowane tym **możesz odwoływać się do magazynu kluczy w pliku parametrów nie szablonu.** Na przykład następujący szablon wdraża bazy danych SQL, która zawiera hasła administratora. Parametr hasła wynosi ciągiem bezpiecznym. Jednak szablonu nie określa, z której pochodzi tej wartości.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "type": "string"
        },
        "administratorLoginPassword": {
            "type": "securestring"
        },
        "collation": {
            "type": "string"
        },
        "databaseName": {
            "type": "string"
        },
        "edition": {
            "type": "string"
        },
        "requestedServiceObjectiveId": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "maxSizeBytes": {
            "type": "string"
        },
        "serverName": {
            "type": "string"
        },
        "sampleName": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "name": "[parameters('serverName')]",
            "properties": {
                "administratorLogin": "[parameters('administratorLogin')]",
                "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
                "version": "12.0"
            },
            "resources": [
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "[parameters('databaseName')]",
                    "properties": {
                        "collation": "[parameters('collation')]",
                        "edition": "[parameters('edition')]",
                        "maxSizeBytes": "[parameters('maxSizeBytes')]",
                        "requestedServiceObjectiveId": "[parameters('requestedServiceObjectiveId')]",
                        "sampleName": "[parameters('sampleName')]"
                    },
                    "type": "databases"
                },
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "AllowAllWindowsAzureIps",
                    "properties": {
                        "endIpAddress": "0.0.0.0",
                        "startIpAddress": "0.0.0.0"
                    },
                    "type": "firewallrules"
                }
            ],
            "type": "Microsoft.Sql/servers"
        }
    ]
}
```

Teraz Utwórz plik parametrów dla poprzedniego szablonu. W pliku parametrów należy określić parametr, który jest zgodna z nazwą parametru w szablonie. Wartość parametru odwołania klucza tajnego z magazynu kluczy. Klucz tajny odwoływać się przez przekazanie identyfikator zasobu magazynu kluczy i nazwę klucza tajnego. W poniższym przykładzie klucz tajny magazynu kluczy musi już istnieć, i podaj wartość statyczną dla jego identyfikator zasobu.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "value": "exampleadmin"
        },
        "administratorLoginPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/{guid}/resourceGroups/examplegroup/providers/Microsoft.KeyVault/vaults/{vault-name}"
              },
              "secretName": "examplesecret"
            }
        },
        "collation": {
            "value": "SQL_Latin1_General_CP1_CI_AS"
        },
        "databaseName": {
            "value": "exampledb"
        },
        "edition": {
            "value": "Standard"
        },
        "location": {
            "value": "southcentralus"
        },
        "maxSizeBytes": {
            "value": "268435456000"
        },
        "requestedServiceObjectiveId": {
            "value": "455330e1-00cd-488b-b5fa-177c226f28b7"
        },
        "sampleName": {
            "value": ""
        },
        "serverName": {
            "value": "exampleserver"
        }
    }
}
```

## <a name="reference-a-secret-with-dynamic-id"></a>Odwołanie klucza tajnego o identyfikatorze dynamiczne

Poprzedniej sekcji pokazano, jak przekazać Identyfikatora zasobu statycznych dla klucza tajnego klucza magazynu. Jednak w niektórych scenariuszach, należy odwoływać magazynu kluczy klucz tajny, który jest różny oparte na bieżącym wdrożeniu. W takim przypadku nie można trwale kodować identyfikator zasobu w pliku parametrów. Niestety, nie można dynamicznie wygenerować identyfikator zasobu w pliku parametrów, ponieważ szablon wyrażenia są niedozwolone w pliku parametrów.

Można dynamicznie wygenerować identyfikator zasobu magazynu kluczy klucz tajny, należy przenieść zasobu, który wymaga klucza tajnego do szablonu zagnieżdżonego. W szablonie głównym dodać zagnieżdżony szablon i podaj parametr, który zawiera identyfikator dynamicznie generowanym zasobu.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "vaultName": {
        "type": "string"
      },
      "secretName": {
        "type": "string"
      }
    },
    "resources": [
    {
      "apiVersion": "2015-01-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          }
        }
      }
    }],
    "outputs": {}
}
```

## <a name="next-steps"></a>Następne kroki
* Aby uzyskać ogólne informacje o magazynów kluczy, zobacz [wprowadzenie do usługi Azure Key Vault](../key-vault/key-vault-get-started.md).
* Przykłady pełną odwołujące się do kluczy tajnych kluczy, zobacz [przykłady Key Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

