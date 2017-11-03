---
title: "Przypisz i zarządzanie zasadami zasobów platformy Azure | Dokumentacja firmy Microsoft"
description: "Opisuje sposób stosowania zasad zasobów platformy Azure do subskrypcji i grup zasobów oraz sposób wyświetlania zasad zasobów."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: tomfitz
ms.openlocfilehash: 64bdd6ed41e98079c8d4112e895aaeddcd629282
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="assign-and-manage-resource-policies"></a>Przypisz i zarządzanie zasadami zasobów

Aby wdrożyć zasady, należy wykonać następujące kroki:

1. Sprawdź definicje zasad (w tym wbudowane zasady dostarczany przez platformę Azure) aby zobaczyć, jeśli istnieje już w ramach subskrypcji, który spełnia wymagania.
2. Jeśli istnieje, należy pobrać jego nazwy.
3. Jeśli nie istnieje, zdefiniuj reguły z JSON i dodaj go jako definicję zasad w ramach subskrypcji. Ten krok powoduje, że zasady dostępne do przypisania, ale nie ma zastosowania reguły do subskrypcji.
4. Dla obu przypadkach należy przypisać zasady do zakresu (np. grupy zasobów lub subskrypcji). Obecnie są wymuszane reguły zasad.

Ten artykuł skupia się na kroki, aby utworzyć definicję zasad i przypisać tej definicji do zakresu za pośrednictwem interfejsu API REST, programu PowerShell lub interfejsu wiersza polecenia Azure. Jeśli wolisz korzystać z portalu do przypisania zasad, zobacz [portal Azure Użyj przypisania zasad i zarządzania nimi zasobów](resource-manager-policy-portal.md). W tym artykule nie dotyczą składni do utworzenia definicji zasad. Informacje na temat zasad składni, zobacz [Przegląd zasad zasobów](resource-manager-policy.md).

## <a name="exclusion-scopes"></a>Zakresy wykluczeń

Można wykluczyć zakres podczas przypisywania zasad. Ta możliwość upraszcza przypisania zasad, ponieważ można przypisać zasady na poziomie subskrypcji, ale określono, gdy zasady nie jest stosowany. Na przykład w ramach subskrypcji, masz grupę zasobów, która jest przeznaczona dla infrastruktury sieci. Zespoły aplikacji wdrożenie ich zasobów do innych grup zasobów. Nie ma tych zespołów, aby utworzyć zasobów sieciowych, które mogą prowadzić do problemów z zabezpieczeniami. Jednak w grupie zasobów sieciowych umożliwia zasobów sieciowych. Przypisania zasad na poziomie subskrypcji, ale wykluczyć grupa zasobów sieciowych. Można określić wiele zakresów sub.

```json
{
    "properties":{
        "policyDefinitionId":"<ID for policy definition>",
        "notScopes":[
            "/subscriptions/<subid>/resourceGroups/networkresourceGroup1"
        ]
    }
}
```

Jeśli określisz zakres wykluczeń przypisania użyć **2017-06-01-preview** wersja interfejsu API.

## <a name="rest-api"></a>Interfejs API REST

### <a name="create-policy-definition"></a>Utwórz definicję zasad

Możesz utworzyć zasady z [interfejsu API REST dla definicji zasad](/rest/api/resources/policydefinitions). Interfejs API REST umożliwia tworzenie i usuwanie definicji zasad oraz uzyskać informacje o istniejących definicji.

Aby utworzyć definicję zasad, uruchom polecenie:

```HTTP
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

Dołącz treści żądania, podobnie do poniższego przykładu:

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

### <a name="assign-policy"></a>Przypisz zasady

Można stosować na żądany zakres za pośrednictwem definicji zasad [interfejsu API REST dla przypisania zasad](/rest/api/resources/policyassignments). Interfejs API REST umożliwia tworzenie i usuwanie przypisania zasad oraz uzyskać informacje na temat istniejące przypisania.

Aby utworzyć przypisanie zasad, uruchom polecenie:

```HTTP
PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}
```

{Przypisania zasad} jest nazwą przypisania zasad.

Dołącz treści żądania, podobnie do poniższego przykładu:

```json
{
  "properties":{
    "displayName":"West US only policy assignment on the subscription ",
    "description":"Resources can only be provisioned in West US regions",
    "parameters": {
      "allowedLocations": { "value": ["northeurope", "westus"] }
     },
    "policyDefinitionId":"/subscriptions/{subscription-id}/providers/Microsoft.Authorization/policyDefinitions/{definition-name}",
      "scope":"/subscriptions/{subscription-id}"
  },
}
```

### <a name="view-policy"></a>Wyświetl zasady
Aby pobrać zasady, użyj [pobrać definicji zasad](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) operacji.

### <a name="get-aliases"></a>Pobierz aliasów
Można pobrać aliasy za pośrednictwem interfejsu API REST:

```HTTP
GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
```

Poniższy przykład przedstawia definicję aliasu. Jak widać, alias definiuje ścieżek w różnych wersjach interfejsu API, nawet wtedy, gdy istnieje zmiana nazwy właściwości. 

```json
"aliases": [
    {
      "name": "Microsoft.Storage/storageAccounts/sku.name",
      "paths": [
        {
          "path": "properties.accountType",
          "apiVersions": [
            "2015-06-15",
            "2015-05-01-preview"
          ]
        },
        {
          "path": "sku.name",
          "apiVersions": [
            "2016-01-01"
          ]
        }
      ]
    }
]
```

## <a name="powershell"></a>PowerShell

Przed kontynuowaniem pracy z przykładami dla programu PowerShell, upewnij się, masz [zainstalowaną najnowszą wersję](/powershell/azure/install-azurerm-ps) programu Azure PowerShell. Parametry zasad zostały dodane w wersji 3.6.0. Jeśli masz starszą wersję przykłady zwrócony błąd wskazujący, że nie można odnaleźć parametru.

### <a name="view-policy-definitions"></a>Wyświetlanie definicji zasad
Aby wyświetlić wszystkie definicje zasad w ramach subskrypcji, użyj następującego polecenia:

```powershell
Get-AzureRmPolicyDefinition
```

Zwraca wszystkie definicje dostępnych zasad, w tym wbudowane zasad. Każda zasada jest zwracany w następującym formacie:

```powershell
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

Przed przystąpieniem do tworzenia definicji zasad, obejrzyj wbudowanych zasad. Jeśli znajdziesz wbudowanych zasad, które ma zastosowanie ograniczeń, które są potrzebne, można pominąć tworzenie definicji zasad. Zamiast tego należy przypisać zasady wbudowanych żądany zakres.

### <a name="create-policy-definition"></a>Utwórz definicję zasad
Można utworzyć definicji zasad przy użyciu opcji `New-AzureRmPolicyDefinition` polecenia cmdlet.

Aby utworzyć definicję zasad z pliku, należy przekazać ścieżkę do pliku. Dla pliku zewnętrznego użyj polecenia:

```powershell
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -DisplayName "Deny cool access tiering for storage" `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Dla pliku lokalnego należy użyć:

```powershell
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -Description "Deny cool access tiering for storage" `
    -Policy "c:\policies\coolAccessTier.json"
```

Aby utworzyć definicję zasad przy użyciu reguły z wbudowanego, należy użyć:

```powershell
$definition = New-AzureRmPolicyDefinition -Name denyCoolTiering -Description "Deny cool access tiering for storage" -Policy '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'
```            

Dane wyjściowe są przechowywane w `$definition` obiektu, który jest używany podczas przypisywania zasad. 

Poniższy przykład tworzy definicji zasad, które zawiera parametry:

```powershell
$policy = '{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying storage accounts.",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
    }
}' 

$definition = New-AzureRmPolicyDefinition -Name storageLocations -Description "Policy to specify locations for storage accounts." -Policy $policy -Parameter $parameters 
```

### <a name="assign-policy"></a>Przypisz zasady

Zastosuj zasady na żądany zakres przy użyciu `New-AzureRmPolicyAssignment` polecenia cmdlet. W poniższym przykładzie przypisano zasady grupy zasobów.

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
New-AzureRMPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

Aby przypisać zasady, które wymaga parametrów, Utwórz i obiekt z tych wartości. W poniższym przykładzie pobiera zasady wbudowanych i przekazuje w wartości parametrów:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
$array = @("West US", "West US 2")
$param = @{"listOfAllowedLocations"=$array}
New-AzureRMPolicyAssignment -Name locationAssignment -Scope $rg.ResourceId -PolicyDefinition $definition -PolicyParameterObject $param
```

### <a name="view-policy-assignment"></a>Widok przypisania zasad

Aby uzyskać przypisanie określonych zasad, należy użyć:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
(Get-AzureRmPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId
```

Aby wyświetlić reguły zasad dla definicji zasad, należy użyć:

```powershell
(Get-AzureRmPolicyDefinition -Name coolAccessTier).Properties.policyRule | ConvertTo-Json
```

### <a name="remove-policy-assignment"></a>Usuń przypisanie zasad 

Aby usunąć przypisanie zasad, należy użyć:

```powershell
Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

### <a name="view-policy-definitions"></a>Wyświetlanie definicji zasad
Aby wyświetlić wszystkie definicje zasad w ramach subskrypcji, użyj następującego polecenia:

```azurecli
az policy definition list
```

Zwraca wszystkie definicje dostępnych zasad, w tym wbudowane zasad. Każda zasada jest zwracany w następującym formacie:

```azurecli
{                                                            
  "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",                      
  "displayName": "Allowed locations",
  "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "in": "[parameters('listOfAllowedLocations')]"
      }
    },
    "then": {
      "effect": "Deny"
    }
  },
  "policyType": "BuiltIn"
}
```

Przed przystąpieniem do tworzenia definicji zasad, obejrzyj wbudowanych zasad. Jeśli znajdziesz wbudowanych zasad, które ma zastosowanie ograniczeń, które są potrzebne, można pominąć tworzenie definicji zasad. Zamiast tego należy przypisać zasady wbudowanych żądany zakres.

### <a name="create-policy-definition"></a>Utwórz definicję zasad

Możesz utworzyć definicję zasad przy użyciu wiersza polecenia platformy Azure przy użyciu polecenia definicji zasad.

Aby utworzyć definicję zasad przy użyciu reguły z wbudowanego, należy użyć:

```azurecli
az policy definition create --name denyCoolTiering --description "Deny cool access tiering for storage" --rules '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'    
```

### <a name="assign-policy"></a>Przypisz zasady

Możesz zastosować zasady do żądany zakres za pomocą polecenia przypisania zasad. W poniższym przykładzie przypisano zasady grupy zasobów.

```azurecli
az policy assignment create --name coolAccessTierAssignment --policy coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

### <a name="view-policy-assignment"></a>Widok przypisania zasad

Aby wyświetlić przypisanie zasad, należy podać nazwa przypisania zasad oraz zakres:

```azurecli
az policy assignment show --name coolAccessTierAssignment --scope "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}"
```

### <a name="remove-policy-assignment"></a>Usuń przypisanie zasad 

Aby usunąć przypisanie zasad, należy użyć:

```azurecli
az policy assignment delete --name coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="next-steps"></a>Następne kroki
* Aby uzyskać instrukcje dla przedsiębiorstw dotyczące użycia usługi Resource Manager w celu efektywnego zarządzania subskrypcjami, zobacz [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Szkielet platformy Azure dla przedsiębiorstwa — narzucony nadzór subskrypcji).

