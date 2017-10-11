---
title: "Eksportowanie szablonu usługi Resource Manager z programem Azure PowerShell | Dokumentacja firmy Microsoft"
description: "Użyj usługi Azure Resource Manager i programu Azure PowerShell, aby wyeksportować szablon na podstawie grupy zasobów."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/01/2017
ms.author: tomfitz
ms.openlocfilehash: 7543811eb9448222b6e7c266756e68debc7d54be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="export-azure-resource-manager-templates-with-powershell"></a>Eksportowanie szablonów usługi Azure Resource Manager przy użyciu programu PowerShell

Usługa Resource Manager umożliwia wyeksportowanie szablonu usługi Resource Manager z istniejących zasobów w ramach subskrypcji. Możesz użyć wygenerowanego szablonu, aby dowiedzieć się więcej o składni szablonu lub aby zautomatyzować ponowne wdrożenie rozwiązania, w razie potrzeby.

Należy pamiętać, że istnieją dwa różne sposoby eksportowania szablonu:

* Możesz wyeksportować szablon, który faktycznie został użyty na potrzeby wdrożenia. W wyeksportowanym szablonie wszystkie parametry i zmienne występują dokładnie tak, jak w oryginalnym szablonie. Takie podejście jest przydatne, gdy jest potrzebne do pobierania szablonu.
* Możesz wyeksportować szablon, który reprezentuje bieżący stan grupy zasobów. Wyeksportowany szablon nie jest oparty na żadnym szablonie użytym do wdrożenia. Utworzony szablon będzie stanowić migawkę grupy zasobów. W wyeksportowanym szablonie zawartych jest wiele zakodowanych wartości i prawdopodobnie mniej parametrów, niż się zwykle definiuje. Takie rozwiązanie jest przydatne, gdy zostały zmodyfikowane grupy zasobów. Będzie więc trzeba przechwycić grupę zasobów jako szablon.

W tym temacie opisano obie te metody.

## <a name="deploy-a-solution"></a>Wdrażanie rozwiązania

Aby zilustrować obu podejść eksportowania szablonu, Zacznijmy od wdrażanie rozwiązania do subskrypcji. Jeśli masz już grupę zasobów w ramach subskrypcji, który chcesz wyeksportować, nie trzeba wdrożyć to rozwiązanie. Jednak w dalszej części tego artykułu odwołuje się do szablonu dla tego rozwiązania. Przykładowy skrypt wdraża konta magazynu.

```powershell
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -DeploymentName NewStorage
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```  

## <a name="save-template-from-deployment-history"></a>Zapisz szablon z historii wdrożenia

Można pobrać szablonu z historii wdrożenia za pomocą [AzureRmResourceGroupDeploymentTemplate Zapisz](/powershell/module/azurerm.resources/save-azurermresourcegroupdeploymenttemplate) polecenia. Poniższy przykład zapisuje szablon, który wcześniej wdrażania:

```powershell
Save-AzureRmResourceGroupDeploymentTemplate -ResourceGroupName ExampleGroup -DeploymentName NewStorage
```

Zwraca lokalizację szablonu.

```powershell
Path
----
C:\Users\exampleuser\NewStorage.json
```

Otwórz plik i zwróć uwagę, że jest dokładne szablon, którego można użyć do wdrożenia. Parametry i zmienne pasuje do szablonu z serwisu GitHub. Tego szablonu można wdrożyć ponownie.

## <a name="export-resource-group-as-template"></a>Eksportowanie grupy zasobów jako szablon

Zamiast pobierania szablonu z historii wdrażania, można pobrać szablonu, która reprezentuje bieżący stan grupy zasobów za pomocą [Export-AzureRmResourceGroup](/powershell/module/azurerm.resources/export-azurermresourcegroup) polecenia. Użyj tego polecenia, gdy wprowadzono wiele zmian w danej grupie zasobów, a nie istniejący szablon reprezentuje wszystkie zmiany.

```powershell
Export-AzureRmResourceGroup -ResourceGroupName ExampleGroup
```

Zwraca lokalizację szablonu.

```powershell
Path
----
C:\Users\exampleuser\ExampleGroup.json
```

Otwórz plik i zwróć uwagę, że różni się od szablonu w witrynie GitHub. Zawiera różne parametry i żadnych zmiennych. Magazyn jednostki SKU i lokalizacji są zakodowane na stałe wartości. W poniższym przykładzie przedstawiono wyeksportowanego szablonu, ale szablon ma nazwę parametru nieco inne:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccounts_nf3mvst4nqb36standardsa_name": {
      "defaultValue": null,
      "type": "String"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "Storage",
      "name": "[parameters('storageAccounts_nf3mvst4nqb36standardsa_name')]",
      "apiVersion": "2016-01-01",
      "location": "southcentralus",
      "tags": {},
      "properties": {},
      "dependsOn": []
    }
  ]
}
```

Można ponownie wdrożyć tego szablonu, ale wymaga to odgadnięcie unikatową nazwę konta magazynu. Nazwa parametru jest nieco inne.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -TemplateFile C:\Users\exampleuser\ExampleGroup.json `
  -storageAccounts_nf3mvst4nqb36standardsa_name tfnewstorage0501
```

## <a name="customize-exported-template"></a>Dostosowywanie wyeksportowanego szablonu

Można zmodyfikować tego szablonu, aby była łatwiejsza w użyciu i bardziej elastyczne. Aby umożliwić więcej lokalizacji, zmień właściwość lokalizacji do użycia w tej samej lokalizacji co grupa zasobów:

```json
"location": "[resourceGroup().location]",
```

Aby uniknąć konieczności odgadnąć uniques nazwę konta magazynu, należy usunąć parametr nazwy konta magazynu. Dodaj parametr sufiks nazwy magazynu oraz Magazyn wersji:

```json
"parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
},
```

Dodawanie zmiennej, która tworzy nazwę konta magazynu przy użyciu funkcji uniqueString:

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

Do zmiennej, należy ustawić nazwę konta magazynu:

```json
"name": "[variables('storageAccountName')]",
```

Jednostka SKU zestawu do parametru:

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

Twój szablon wygląda teraz następująco:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), parameters('storageSuffix'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "[parameters('storageAccountType')]",
        "tier": "Standard"
      },
      "kind": "Storage",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {},
      "dependsOn": []
    }
  ]
}
```

Należy ponownie wdrożyć zmodyfikowany szablon.

## <a name="next-steps"></a>Następne kroki
* Aby uzyskać informacje dotyczące korzystania z portalu, aby wyeksportować szablon, zobacz [Eksportowanie szablonu usługi Azure Resource Manager z istniejących zasobów](resource-manager-export-template.md).
* Aby określić parametry w szablonie, zobacz [tworzenia szablonów](resource-group-authoring-templates.md#parameters).
* Aby uzyskać wskazówki dotyczące rozwiązania typowych błędów wdrażania, zobacz [Rozwiąż typowe błędy wdrożenia usługi Azure z usługą Azure Resource Manager](resource-manager-common-deployment-errors.md).
