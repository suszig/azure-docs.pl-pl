---
title: "Wdrażanie wielu wystąpień zasobów platformy Azure | Dokumentacja firmy Microsoft"
description: "Użyj operacji kopiowania i tablic w szablonie usługi Azure Resource Manager w celu wykonania iteracji wiele razy podczas wdrażania zasobów."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 94d95810-a87b-460f-8e82-c69d462ac3ca
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: 8e6d68612be4b7d4e1d6cea13e0f29636931abd8
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2017
---
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a>Wdrażanie wielu wystąpień zasobów lub właściwości w szablonach usługi Azure Resource Manager
W tym temacie przedstawiono sposób wykonania iteracji w szablonie usługi Azure Resource Manager, aby utworzyć wiele wystąpień zasobu lub wiele wystąpień właściwości w zasobie.

Jeśli musisz dodać logikę do szablonu, który pozwala określić, czy zasób jest wdrażany, zobacz [warunkowo wdrażanie zasobów](#conditionally-deploy-resource).

Na przykład tworzenie wielu elementów w tablicy zmiennej, zobacz [zmienne](resource-group-authoring-templates.md#variables).

## <a name="resource-iteration"></a>Iteracja zasobów
Aby utworzyć wiele wystąpień tego typu zasobu, należy dodać `copy` elementu Typ zasobu. Copy element służy do określenia liczby iteracji i nazwę tej pętli. Wartość licznika musi być dodatnią liczbą całkowitą i nie może przekraczać 800. Menedżer zasobów tworzy zasoby równolegle. W związku z tym nie jest gwarantowana kolejności ich utworzenia. Aby utworzyć iterowane zasobów w sekwencji, zobacz [Serial kopiowania](#serial-copy). 

Zasobu do utworzenia wielokrotnie ma następujący format:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        }
    ],
    "outputs": {}
}
```

Należy zauważyć, że nazwa każdego zasobu zawiera `copyIndex()` funkcji, która zwraca bieżącą iterację w pętli. `copyIndex()`jest liczony od zera. Zatem w poniższym przykładzie:

```json
"name": "[concat('storage', copyIndex())]",
```

Tworzy następujące nazwy:

* storage0
* storage1
* storage2.

Aby przesunąć wartość indeksu, można przekazać wartość w funkcji copyIndex(). Liczba iteracji, aby wykonać nadal jest określona w elemencie kopiowania, ale wartość copyIndex zostanie przesunięty o określonej wartości. Zatem w poniższym przykładzie:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Tworzy następujące nazwy:

* storage1
* storage2
* storage3

Operacja kopiowania jest przydatne podczas pracy z tablicami, ponieważ można wykonać iterację każdego elementu w tablicy. Użyj `length` funkcji w macierzy, aby określić liczbę elementów w iteracji, i `copyIndex` można pobrać bieżącego indeksu tablicy. Zatem w poniższym przykładzie:

```json
"parameters": { 
  "org": { 
     "type": "array", 
     "defaultValue": [ 
         "contoso", 
         "fabrikam", 
         "coho" 
      ] 
  }
}, 
"resources": [ 
  { 
      "name": "[concat('storage', parameters('org')[copyIndex()])]", 
      "copy": { 
         "name": "storagecopy", 
         "count": "[length(parameters('org'))]" 
      }, 
      ...
  } 
]
```

Tworzy następujące nazwy:

* storagecontoso
* storagefabrikam
* storagecoho

## <a name="serial-copy"></a>Kopiuj szeregowe

Jeśli używasz elementu kopiowania do utworzenia wielu wystąpień typu zasobu, Menedżer zasobów, domyślnie wdroży je równolegle. Można określić, że zasoby są wdrażane w sekwencji. Na przykład podczas aktualizacji do środowiska produkcyjnego, można tak skonfigurować aktualizacje tylko pewne są aktualizowane w dowolnym momencie.

Menedżer zasobów udostępnia właściwości w elemencie kopiowania umożliwiających szeregowo wdrażanie wielu wystąpień. W zestawie elementów kopiowania `mode` do **serial** i `batchSize` liczby wystąpień do wdrożenia w czasie. Serial w trybie Menedżera zasobów tworzy zależność w wystąpieniach wcześniej w pętli, więc nie zostanie uruchomiony serii do chwili zakończenia poprzedniej wsadowym.

```json
"copy": {
    "name": "iterator",
    "count": "[parameters('numberToDeploy')]",
    "mode": "serial",
    "batchSize": 2
},
```

Właściwość mode również akceptuje **równoległych**, która jest wartością domyślną.

Aby przetestować serial kopiowania bez tworzenia rzeczywistych zasobów, wpisz następujący szablon, który wdraża pusty zagnieżdżone szablony:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "numberToDeploy": {
      "type": "int",
      "minValue": 2,
      "defaultValue": 5
    }
  },
  "resources": [
    {
      "apiVersion": "2015-01-01",
      "type": "Microsoft.Resources/deployments",
      "name": "[concat('loop-', copyIndex())]",
      "copy": {
        "name": "iterator",
        "count": "[parameters('numberToDeploy')]",
        "mode": "serial",
        "batchSize": 1
      },
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [],
          "outputs": {
          }
        }
      }
    }
  ],
  "outputs": {
  }
}
```

W historii wdrożenia Zwróć uwagę, że zagnieżdżone wdrożenia są przetwarzane w sekwencji.

![Serial wdrożenia](./media/resource-group-create-multiple/serial-copy.png)

Dla scenariusza bardziej realistyczne poniższy przykład wdraża dwóch wystąpień w czasie z zagnieżdżonych szablonu maszyny wirtualnej systemu Linux:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "User name for the Virtual Machine."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Password for the Virtual Machine."
            }
        },
        "dnsLabelPrefix": {
            "type": "string",
            "metadata": {
                "description": "Unique DNS Name for the Public IP used to access the Virtual Machine."
            }
        },
        "ubuntuOSVersion": {
            "type": "string",
            "defaultValue": "16.04.0-LTS",
            "allowedValues": [
                "12.04.5-LTS",
                "14.04.5-LTS",
                "15.10",
                "16.04.0-LTS"
            ],
            "metadata": {
                "description": "The Ubuntu version for the VM. This will pick a fully patched image of this given Ubuntu version."
            }
        }
    },
    "variables": {
        "templatelink": "https://raw.githubusercontent.com/rjmax/Build2017/master/Act1.TemplateEnhancements/Chapter03.LinuxVM.json"
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "[concat('nestedDeployment',copyIndex())]",
            "type": "Microsoft.Resources/deployments",
            "copy": {
                "name": "myCopySet",
                "count": 4,
                "mode": "serial",
                "batchSize": 2
            },
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "adminUsername": {
                        "value": "[parameters('adminUsername')]"
                    },
                    "adminPassword": {
                        "value": "[parameters('adminPassword')]"
                    },
                    "dnsLabelPrefix": {
                        "value": "[parameters('dnsLabelPrefix')]"
                    },
                    "ubuntuOSVersion": {
                        "value": "[parameters('ubuntuOSVersion')]"
                    },
                    "index":{
                        "value": "[copyIndex()]"
                    }
                }
            }
        }
    ]
}
```

## <a name="property-iteration"></a>Właściwość iteracji

Aby utworzyć wiele wartości dla właściwości do zasobu, należy dodać `copy` tablicy w elemencie właściwości. Ta tablica zawiera obiekty, a każdy obiekt ma następujące właściwości:

* Nazwa — Nazwa właściwości do utworzenia wielu wartości
* Liczba — liczba wartości w celu utworzenia
* dane wejściowe - obiekt, który zawiera wartości, które można przypisać do właściwości  

Poniższy przykład przedstawia sposób zastosowania `copy` właściwości dataDisks na maszynie wirtualnej:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "copy": [{
          "name": "dataDisks",
          "count": 3,
          "input": {
              "lun": "[copyIndex('dataDisks')]",
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

Zwróć uwagę, że przy użyciu `copyIndex` wewnątrz iteracji właściwości, należy podać nazwę iteracji. Nie masz Podaj nazwę, gdy jest używany z zasobów iteracji.

Menedżer zasobów rozszerza `copy` tablicy podczas wdrażania. Nazwa tablicy staje się nazwa właściwości. Wartości wejściowe stają się właściwości obiektu. Wdrożone szablon staje się:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
          {
              "lun": 0,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 1,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 2,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

Iteracja zasobów i właściwości można użyć razem. Odwołanie do iteracji właściwości według nazwy.

```json
{
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[concat(parameters('vnetname'), copyIndex())]",
    "apiVersion": "2016-06-01",
    "copy":{
        "count": 2,
        "name": "vnetloop"
    },
    "location": "[resourceGroup().location]",
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "copy": [
            {
                "name": "subnets",
                "count": 2,
                "input": {
                    "name": "[concat('subnet-', copyIndex('subnets'))]",
                    "properties": {
                        "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
                    }
                }
            }
        ]
    }
}
```

We właściwościach dla każdego zasobu może zawierać tylko jeden element kopiowania. Aby określić iteracji pętli dla więcej niż jedną właściwość, należy zdefiniować wiele obiektów w tablicy kopiowania. Każdy obiekt jest iterowane oddzielnie. Na przykład, aby utworzyć wiele wystąpień obu `frontendIPConfigurations` właściwości i `loadBalancingRules` właściwości modułu równoważenia obciążenia, zdefiniuj zarówno do obiektów w elemencie pojedynczej kopii: 

```json
{
    "name": "[variables('loadBalancerName')]",
    "type": "Microsoft.Network/loadBalancers",
    "properties": {
        "copy": [
          {
              "name": "frontendIPConfigurations",
              "count": 2,
              "input": {
                  "name": "[concat('loadBalancerFrontEnd', copyIndex('frontendIPConfigurations', 1))]",
                  "properties": {
                      "publicIPAddress": {
                          "id": "[variables(concat('publicIPAddressID', copyIndex('frontendIPConfigurations', 1)))]"
                      }
                  }
              }
          },
          {
              "name": "loadBalancingRules",
              "count": 2,
              "input": {
                  "name": "[concat('LBRuleForVIP', copyIndex('loadBalancingRules', 1))]",
                  "properties": {
                      "frontendIPConfiguration": {
                          "id": "[variables(concat('frontEndIPConfigID', copyIndex('loadBalancingRules', 1)))]"
                      },
                      "backendAddressPool": {
                          "id": "[variables('lbBackendPoolID')]"
                      },
                      "protocol": "tcp",
                      "frontendPort": "[variables(concat('frontEndPort' copyIndex('loadBalancingRules', 1))]",
                      "backendPort": "[variables(concat('backEndPort' copyIndex('loadBalancingRules', 1))]",
                      "probe": {
                          "id": "[variables('lbProbeID')]"
                      }
                  }
              }
          }
        ],
        ...
    }
}
```

## <a name="depend-on-resources-in-a-loop"></a>Są zależne od zasobów w pętli
Określ, czy zasób jest wdrażane za pomocą po inny zasób `dependsOn` elementu. Aby wdrożyć z zasobem, który jest zależny od kolekcji zasobów w pętli, podaj nazwę pętlę kopiowania w elemencie dependsOn. Poniższy przykład przedstawia sposób wdrażania trzy konta magazynu przed wdrożeniem maszyny wirtualnej. Nie ma pełnej definicji maszyny wirtualnej. Zwróć uwagę, że copy element ma nazwę ustawioną `storagecopy` element dependsOn dla maszyn wirtualnych jest również ustawiono `storagecopy`.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        },
        {
            "apiVersion": "2015-06-15", 
            "type": "Microsoft.Compute/virtualMachines", 
            "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
            "dependsOn": ["storagecopy"],
            ...
        }
    ],
    "outputs": {}
}
```

## <a name="create-multiple-instances-of-a-child-resource"></a>Tworzenie wielu wystąpień zasobu podrzędnego
Nie można używać pętli kopii zasobu podrzędnego. Aby utworzyć wiele wystąpień z zasobem, który zazwyczaj zdefiniowane jako zagnieżdżony w ramach innego zasobu, należy zamiast tego utworzyć tego zasobu jako zasób najwyższego poziomu. Można zdefiniować relacji z zasobem nadrzędnej za pośrednictwem typem i nazwą właściwości.

Na przykład załóżmy, że zazwyczaj Definiowanie zestawu danych jako zasób podrzędnych w fabryce danych.

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
    "resources": [
    {
        "type": "datasets",
        "name": "exampleDataSet",
        "dependsOn": [
            "exampleDataFactory"
        ],
        ...
    }
}]
```

Aby utworzyć wiele wystąpień zestawów danych, przenieś go poza fabryki danych. Zestaw danych musi być na tym samym poziomie jako fabryki danych, ale nadal jest zasobem podrzędnych fabryki danych. Możesz zachować relacji między zestawu danych i fabryki danych za pośrednictwem typem i nazwą właściwości. Ponieważ nie można wywnioskować typu z jej położenie w szablonie, należy podać pełną typu w formacie: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

Aby ustalić relacji nadrzędny/podrzędny z wystąpienia fabryki danych, należy podać nazwę dla zestawu danych, która zawiera nazwę zasobu nadrzędnego. Użyj formatu: `{parent-resource-name}/{child-resource-name}`.  

W poniższym przykładzie przedstawiono implementacji:

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
},
{
    "type": "Microsoft.DataFactory/datafactories/datasets",
    "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
    "dependsOn": [
        "exampleDataFactory"
    ],
    "copy": { 
        "name": "datasetcopy", 
        "count": "3" 
    } 
    ...
}]
```

## <a name="conditionally-deploy-resource"></a>Warunkowo wdrażanie zasobów

Aby określić, czy zasób jest wdrażany, użyj `condition` elementu. Wartość dla tego elementu jest rozpoznawany jako PRAWDA lub FAŁSZ. Gdy ma wartość true, zasób zostanie wdrożona. Gdy wartość jest równa false, zasób nie została wdrożona. Na przykład aby określić, czy nowe konto magazynu jest wdrażana, czy istniejące konto magazynu jest używana, należy użyć:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

Na przykład za pomocą nowego lub istniejącego zasobu, zobacz [nowy lub istniejący szablon warunku](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).

Przykład wdrożenia maszyny wirtualnej za pomocą hasła lub klucza SSH, zobacz [nazwa użytkownika lub SSH szablonu warunku](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).

## <a name="next-steps"></a>Następne kroki
* Jeśli chcesz dowiedzieć się więcej o części szablonu, zobacz [Authoring Azure Resource Manager szablony](resource-group-authoring-templates.md).
* Aby dowiedzieć się, jak wdrożyć szablon, zobacz [wdrażanie aplikacji przy użyciu szablonu usługi Resource Manager Azure](resource-group-template-deploy.md).

