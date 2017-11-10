---
title: "Klucz tajny usługi Key Vault przy użyciu szablonu usługi Azure Resource Manager | Dokumentacja firmy Microsoft"
description: "Przedstawia sposób przekazywania klucza tajnego z magazynu kluczy jako parametr podczas wdrażania."
services: azure-resource-manager,key-vault
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2017
ms.author: tomfitz
ms.openlocfilehash: e789a234979be877d990665902fd6219ae7ec40b
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Przekaż wartość parametru bezpieczne podczas wdrażania za pomocą usługi Azure Key Vault

Gdy należy przekazać wartość bezpiecznego (na przykład hasło) jako parametr podczas wdrażania można pobrać wartości z [usługi Azure Key Vault](../key-vault/key-vault-whatis.md). Możesz pobrać wartość przez odwołanie do magazynu kluczy i klucz tajny w pliku parametrów. Wartość nigdy nie jest uwidaczniana, ponieważ można odwoływać się tylko jego identyfikator magazynu kluczy. Nie trzeba ręcznie wprowadź wartość klucz tajny w każdym wdrożeniu zasobów. Magazyn kluczy może istnieć w innej subskrypcji niż grupa zasobów, które wdrażasz. Podczas odwoływania się do magazynu kluczy, obejmują identyfikatora subskrypcji.

Podczas tworzenia magazynu kluczy, skonfigurować *enabledForTemplateDeployment* właściwości *true*. Ustawiając tę wartość na true, należy zezwolić na dostęp z szablonów usługi Resource Manager podczas wdrażania.

## <a name="deploy-a-key-vault-and-secret"></a>Wdrażanie magazynu kluczy oraz klucz tajny

Aby utworzyć magazyn kluczy i klucz tajny, należy użyć wiersza polecenia platformy Azure lub programu PowerShell. Zwróć uwagę, że magazynu kluczy jest włączona dla wdrożenia szablonu. 

W przypadku interfejsu wiersza polecenia platformy Azure użyj polecenia:

```azurecli-interactive
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create \
  --name $vaultname \
  --resource-group examplegroup \
  --location 'South Central US' \
  --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

W przypadku programu PowerShell użyj polecenia:

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault `
  -VaultName $vaultname `
  -ResourceGroupName examplegroup `
  -Location "South Central US" `
  -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-to-the-secret"></a>Zapewnianie dostępu do klucza tajnego

Czy używasz nowego magazynu kluczy lub istniejącego upewnij się, że użytkownik wdrażanie szablonu może uzyskać dostęp klucz tajny. Wdrażanie szablonu, który odwołuje się do klucza tajnego użytkownik musi mieć `Microsoft.KeyVault/vaults/deploy/action` uprawnienia dla magazynu kluczy. [Właściciela](../active-directory/role-based-access-built-in-roles.md#owner) i [współautora](../active-directory/role-based-access-built-in-roles.md#contributor) ról zarówno udzielić dostępu.

## <a name="reference-a-secret-with-static-id"></a>Odwołanie klucza tajnego o identyfikatorze statyczne

Szablon, który odbiera klucz tajny magazynu kluczy jest podobnie jak każdy inny szablon. Jest to spowodowane tym **możesz odwoływać się do magazynu kluczy w pliku parametrów nie szablonu.** Na przykład następujący szablon wdraża bazy danych SQL, która zawiera hasła administratora. Parametr hasła wynosi ciągiem bezpiecznym. Jednak szablonu nie określa, z której pochodzi tej wartości.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminLogin": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    },
    "sqlServerName": {
      "type": "string"
    }
  },
  "resources": [
    {
      "name": "[parameters('sqlServerName')]",
      "type": "Microsoft.Sql/servers",
      "apiVersion": "2015-05-01-preview",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
        "administratorLogin": "[parameters('adminLogin')]",
        "administratorLoginPassword": "[parameters('adminPassword')]",
        "version": "12.0"
      }
    }
  ],
  "outputs": {
  }
}
```

Teraz Utwórz plik parametrów dla poprzedniego szablonu. W pliku parametrów należy określić parametr, który jest zgodna z nazwą parametru w szablonie. Wartość parametru odwołania klucza tajnego z magazynu kluczy. Klucz tajny odwoływać się przez przekazanie identyfikator zasobu magazynu kluczy i nazwę klucza tajnego. W poniższym przykładzie klucz tajny magazynu kluczy musi już istnieć, i podaj wartość statyczną dla jego identyfikator zasobu.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminLogin": {
            "value": "exampleadmin"
        },
        "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/<subscription-id>/resourceGroups/examplegroup/providers/Microsoft.KeyVault/vaults/<vault-name>"
              },
              "secretName": "examplesecret"
            }
        },
        "sqlServerName": {
            "value": "<your-server-name>"
        }
    }
}
```

Teraz wdrożyć szablon i podaj pliku parametrów. W przypadku interfejsu wiersza polecenia platformy Azure użyj polecenia:

```azurecli-interactive
az group create --name datagroup --location "Central US"
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-file sqlserver.json \
    --parameters @sqlserver.parameters.json
```

W przypadku programu PowerShell użyj polecenia:

```powershell
New-AzureRmResourceGroup -Name datagroup -Location "Central US"
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateFile sqlserver.json `
  -TemplateParameterFile sqlserver.parameters.json
```

## <a name="reference-a-secret-with-dynamic-id"></a>Odwołanie klucza tajnego o identyfikatorze dynamiczne

Poprzedniej sekcji pokazano, jak przekazać Identyfikatora zasobu statycznych dla klucza tajnego klucza magazynu. Jednak w niektórych scenariuszach, należy odwoływać magazynu kluczy klucz tajny, który jest różny oparte na bieżącym wdrożeniu. W takim przypadku nie można trwale kodować identyfikator zasobu w pliku parametrów. Niestety, nie można dynamicznie wygenerować identyfikator zasobu w pliku parametrów, ponieważ szablon wyrażenia są niedozwolone w pliku parametrów.

Można dynamicznie wygenerować identyfikator zasobu magazynu kluczy klucz tajny, należy przenieść zasobu, który wymaga klucza tajnego do szablonu zagnieżdżonego. W szablonie głównym dodać zagnieżdżony szablon i podaj parametr, który zawiera identyfikator dynamicznie generowanym zasobu. Zagnieżdżone szablonu musi być dostępny za pomocą zewnętrznego identyfikatora URI. Pozostałej części w tym artykule przyjęto założenie, powyższy szablon został dodany do konta magazynu, i jest dostępny za pomocą identyfikatora URI - `https://<storage-name>.blob.core.windows.net/templatecontainer/sqlserver.json`.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "vaultName": {
        "type": "string"
      },
      "vaultResourceGroup": {
        "type": "string"
      },
      "secretName": {
        "type": "string"
      },
      "adminLogin": {
        "type": "string"
      },
      "sqlServerName": {
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
          "uri": "https://<storage-name>.blob.core.windows.net/templatecontainer/sqlserver.json",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[resourceId(subscription().subscriptionId,  parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          },
          "adminLogin": { "value": "[parameters('adminLogin')]" },
          "sqlServerName": {"value": "[parameters('sqlServerName')]"}
        }
      }
    }],
    "outputs": {}
}
```

Wdrażanie Powyższy szablon i podaj wartości parametrów.

## <a name="next-steps"></a>Następne kroki
* Aby uzyskać ogólne informacje o magazynów kluczy, zobacz [wprowadzenie do usługi Azure Key Vault](../key-vault/key-vault-get-started.md).
* Przykłady pełną odwołujące się do kluczy tajnych kluczy, zobacz [przykłady Key Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
