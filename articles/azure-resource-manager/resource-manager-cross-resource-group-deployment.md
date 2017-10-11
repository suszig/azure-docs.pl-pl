---
title: "Wdrażanie zasobów platformy Azure do wielu grup zasobów | Dokumentacja firmy Microsoft"
description: "Pokazuje, jak docelowy kilku grup zasobów platformy Azure podczas wdrażania."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: tomfitz
ms.openlocfilehash: d8b041213b269775175a810e585103d3c538557f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-azure-resources-to-more-than-one-resource-group"></a>Wdrażanie zasobów platformy Azure do więcej niż jednej grupy zasobów

Zazwyczaj jest wdrażany, wszystkie zasoby w szablonie do pojedynczej grupy zasobów. Istnieją jednak scenariuszy, w której chcesz wdrożyć razem zestaw zasobów, ale umieścić je w różnych grupach zasobów. Na przykład możesz wdrażanie kopii zapasowej maszyny wirtualnej dla usługi Azure Site Recovery na oddzielnej grupie zasobów i lokalizacji. Menedżer zasobów pozwala na użycie zagnieżdżone szablony pod kątem różnych grup zasobów niż grupa zasobów używany do szablonu nadrzędnego.

Grupa zasobów to kontener cyklem życia aplikacji i jej kolekcji zasobów. Utwórz grupę zasobów poza szablon i określ grupę zasobów do docelowego podczas wdrażania. Aby obejrzeć wprowadzenie do grup zasobów, zobacz [Omówienie usługi Azure Resource Manager](resource-group-overview.md).

## <a name="example-template"></a>Przykład szablonu

Aby inny zasób docelowy, należy użyć szablonu zagnieżdżonych lub połączonych podczas wdrażania. `Microsoft.Resources/deployments` Udostępnia typ zasobu `resourceGroup` parametr, który umożliwia określenie innej grupie zasobów dla wdrożenia zagnieżdżonego. Wszystkie grupy zasobów musi istnieć przed uruchomieniem wdrożenia. Poniższy przykład wdraża dwóch kont magazynu — w grupie zasobów określony podczas wdrażania, a druga w grupie zasobów o nazwie `crossResourceGroupDeployment`:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "StorageAccountName1": {
            "type": "string"
        },
        "StorageAccountName2": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "crossResourceGroupDeployment",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "name": "[parameters('StorageAccountName2')]",
                            "apiVersion": "2015-06-15",
                            "location": "West US",
                            "properties": {
                                "accountType": "Standard_LRS"
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('StorageAccountName1')]",
            "apiVersion": "2015-06-15",
            "location": "West US",
            "properties": {
                "accountType": "Standard_LRS"
            }
        }
    ]
}
```

Jeśli ustawisz `resourceGroup` Nazwa grupy zasobów, która nie istnieje, wdrożenie zakończy się niepowodzeniem. Jeśli nie zostanie określona wartość `resourceGroup`, Resource Manager korzysta z nadrzędnej grupy zasobów.  

## <a name="deploy-the-template"></a>Wdrożenie szablonu

Aby wdrożyć szablon przykładzie, można użyć portalu, programu Azure PowerShell lub wiersza polecenia platformy Azure. Dla programu Azure PowerShell lub interfejsu wiersza polecenia Azure należy użyć wersji z maja 2017 lub nowszego. Przykłady założono zapisany szablon lokalnie jako plik o nazwie **crossrgdeployment.json**.

Dla środowiska PowerShell:

```powershell
Login-AzureRmAccount

New-AzureRmResourceGroup -Name mainResourceGroup -Location "South Central US"
New-AzureRmResourceGroup -Name crossResourceGroupDeployment -Location "Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName mainResourceGroup `
  -TemplateFile c:\MyTemplates\crossrgdeployment.json
```

Dla platformy Azure interfejsu wiersza polecenia:

```azurecli
az login

az group create --name mainResourceGroup --location "South Central US"
az group create --name crossResourceGroupDeployment --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group mainResourceGroup \
    --template-file crossrgdeployment.json
```

Po zakończeniu wdrażania, zobacz się dwie grupy zasobów. Każda grupa zasobów zawiera konta magazynu.

## <a name="use-resourcegroup-function"></a>Funkcja resourceGroup()

Dla wielu wdrożenia grupy zasobów, [funkcja resouceGroup()](resource-group-template-functions-resource.md#resourcegroup) jest rozpoznawana inaczej w zależności od określania szablon zagnieżdżony. 

Po osadzeniu jeden szablon w innym szablonie resouceGroup() w szablonie zagnieżdżonym rozpoznaje w nadrzędnej grupie zasobów. Osadzony szablonu używany następujący format:

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() refers to parent resource group
    }
}
```

Jeśli możesz połączyć się z oddzielnych szablonu, resouceGroup() w szablonie połączonego rozpoznawany jako grupa zasobów zagnieżdżonych. Połączone szablonu używany następujący format:

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() in linked template refers to linked resource group
    }
}
```

## <a name="next-steps"></a>Następne kroki

* Aby poznać sposób definiowania parametry w szablonie, zobacz [poznać strukturę i składni szablonów usługi Azure Resource Manager](resource-group-authoring-templates.md).
* Aby uzyskać wskazówki dotyczące rozwiązania typowych błędów wdrażania, zobacz [Rozwiąż typowe błędy wdrożenia usługi Azure z usługą Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Aby uzyskać informacje o wdrażaniu szablonu, który wymaga tokenu sygnatury dostępu Współdzielonego, zobacz [wdrażanie szablonu prywatnej przy użyciu tokenu sygnatury dostępu Współdzielonego](resource-manager-powershell-sas-token.md).
