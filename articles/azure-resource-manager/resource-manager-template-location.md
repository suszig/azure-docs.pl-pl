---
title: "Lokalizacja zasobów platformy Azure w szablonie | Dokumentacja firmy Microsoft"
description: "Przedstawiono sposób ustawiania lokalizacji zasobu w szablonie usługi Azure Resource Manager"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: tomfitz
ms.openlocfilehash: 73e50a593c41e841dcaf184abb895406ff5001e9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="set-resource-location-in-azure-resource-manager-templates"></a>Ustaw lokalizację zasobów w szablonach usługi Azure Resource Manager
W przypadku wdrażania szablonu, należy podać lokalizację każdego zasobu. W tym temacie przedstawiono sposób określenia lokalizacji, które są dostępne dla Twojej subskrypcji dla każdego typu zasobu.

## <a name="determine-supported-locations"></a>Określić obsługiwane lokalizacje

Aby uzyskać pełną listę obsługiwanych lokalizacji dla każdego typu zasobu, zobacz [produkty, które są dostępne w regionie](https://azure.microsoft.com/regions/services/). Jednak subskrypcji może nie mieć dostępu do wszystkich lokalizacji na tej liście. Aby wyświetlić listę niestandardowe lokalizacje, które są dostępne dla Twojej subskrypcji, użyj programu Azure PowerShell lub interfejsu wiersza polecenia platformy Azure. 

W poniższym przykładzie użyto programu PowerShell, aby pobrać lokalizacji dla `Microsoft.Web\sites` typu zasobu:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

W poniższym przykładzie użyto 2.0 interfejsu wiersza polecenia platformy Azure można pobrać lokalizacji dla `Microsoft.Web\sites` typu zasobu:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="set-location-in-template"></a>Ustaw lokalizację, w szablonie

Po ustaleniu obsługiwane lokalizacje dla zasobów, należy określić tę lokalizację w szablonie. Najprostszym sposobem ta wartość jest utworzenie grupy zasobów w lokalizacji, która obsługuje typy zasobów i ustawioną każdej lokalizacji `[resourceGroup().location]`. Można ponownie wdrożyć szablon do grup zasobów w różnych lokalizacjach i nie zmieniać wartości w szablonie lub parametrów. 

W poniższym przykładzie przedstawiono konta magazynu, który jest wdrożony w tej samej lokalizacji co grupa zasobów:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

Jeśli potrzebujesz kodowania lokalizację w szablonie, podaj nazwę jednego z obsługiwanych regionów. W poniższym przykładzie przedstawiono konta magazynu, który jest zawsze wdrożony w północno-środkowe stany:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "North Central US",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

## <a name="next-steps"></a>Następne kroki
* Aby uzyskać zalecenia dotyczące tworzenia szablonów, zobacz [najlepszych rozwiązań dotyczących tworzenia szablonów usługi Azure Resource Manager](resource-manager-template-best-practices.md).

