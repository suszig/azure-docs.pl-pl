---
title: "Wymuszanie zabezpieczeń przy użyciu zasad na maszynach wirtualnych systemu Linux na platformie Azure | Dokumentacja firmy Microsoft"
description: "Sposób stosowania zasad do usługi Azure Resource Manager maszyny wirtualnej systemu Linux"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 06778ab4-f8ff-4eed-ae10-26a276fc3faa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: singhkay
ms.openlocfilehash: 72abb01a3ce7f4dea2ee97219e9a406c69cda7c5
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="apply-policies-to-linux-vms-with-azure-resource-manager"></a>Stosowania zasad do maszyn wirtualnych systemu Linux za pomocą Menedżera zasobów Azure
Korzystając z zasad, organizacja może wymusić różnych konwencje i zasady w całym przedsiębiorstwie. Wymuszanie zachowanie można zmniejszenia ryzyka podczas pracy nad dla sukcesu organizacji. W tym artykule opisano sposób można użyć zasad usługi Azure Resource Manager do definiowania zachowanie w przypadku maszyn wirtualnych w organizacji.

Aby obejrzeć wprowadzenie do zasad, zobacz [co to jest Azure zasad?](../../azure-policy/azure-policy-introduction.md).

## <a name="permitted-virtual-machines"></a>Dozwolone maszyny wirtualne
Aby upewnić się, że maszyny wirtualne w Twojej organizacji są zgodne z aplikacji, można ograniczyć dozwolonych systemów operacyjnych. W poniższym przykładzie zasad musisz zezwolić tylko Ubuntu 14.04.2-LTS maszyn wirtualnych ma zostać utworzony.

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "in": [
          "Microsoft.Compute/disks",
          "Microsoft.Compute/virtualMachines",
          "Microsoft.Compute/VirtualMachineScaleSets"
        ]
      },
      {
        "not": {
          "allOf": [
            {
              "field": "Microsoft.Compute/imagePublisher",
              "in": [
                "Canonical"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "UbuntuServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "14.04.2-LTS"
              ]
            },
            {
              "field": "Microsoft.Compute/imageVersion",
              "in": [
                "latest"
              ]
            }
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

Użyj symbolu wieloznacznego, aby zmodyfikować poprzedniego zasadę, aby dopuścić żadnego obrazu Ubuntu LTS: 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

Aby uzyskać informacji o polach zasad, zobacz [aliasy zasad](../../azure-policy/policy-definition.md#aliases).

## <a name="managed-disks"></a>Dyski zarządzane

Aby wymagać używania dysków zarządzanych, należy użyć następujące zasady:

```json
{
  "if": {
    "anyOf": [
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/osDisk.uri",
            "exists": true
          }
        ]
      },
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/VirtualMachineScaleSets"
          },
          {
            "anyOf": [
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osDisk.vhdContainers",
                "exists": true
              },
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl",
                "exists": true
              }
            ]
          }
        ]
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="images-for-virtual-machines"></a>Obrazy maszyn wirtualnych

Ze względów bezpieczeństwa może wymagać, aby zatwierdzone niestandardowych obrazów są wdrażane w środowisku. Można określić albo grupę zasobów, która zawiera obrazy zatwierdzone lub konkretnym zatwierdzone obrazów.

Poniższy przykład wymaga obrazów z grupy zasobów zatwierdzonych:

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in": [
                    "Microsoft.Compute/virtualMachines",
                    "Microsoft.Compute/VirtualMachineScaleSets"
                ]
            },
            {
                "not": {
                    "field": "Microsoft.Compute/imageId",
                    "contains": "resourceGroups/CustomImage"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
} 
```

W poniższym przykładzie identyfikatorów zatwierdzonych obrazu:

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a>Rozszerzenia maszyn wirtualnych

Może zajść potrzeba zabraniać użycia pewnych typów rozszerzeń. Na przykład rozszerzenie nie może być zgodna z niektórych obrazy niestandardowe maszyny wirtualnej. Poniższy przykład pokazuje, jak mają być blokowane z określonym rozszerzeniem. Aby określić, które rozszerzenia, aby zablokować używa wydawcy i typu.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                "equals": "Microsoft.Compute"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/type",
                "equals": "{extension-type}"

      }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```


## <a name="next-steps"></a>Następne kroki
* Po zdefiniowaniu reguły zasad (jak pokazano w powyższych przykładach), należy utworzyć definicji zasad i przypisać je do zakresu. Zakres może być subskrypcji, grupy zasobów lub zasobów. Aby przypisać zasady, zobacz [portal Azure używany do przypisywania i zarządzanie zasadami zasobów](../../azure-policy/assign-policy-definition.md), [Użyj programu PowerShell, aby przypisać zasady](../../azure-policy/assign-policy-definition-ps.md), lub [Użyj wiersza polecenia platformy Azure można przypisać zasady](../../azure-policy/assign-policy-definition-cli.md).
* Aby obejrzeć wprowadzenie do zasad zasobów, zobacz [co to jest Azure zasad?](../../azure-policy/azure-policy-introduction.md).
* Aby uzyskać instrukcje dla przedsiębiorstw dotyczące użycia usługi Resource Manager w celu efektywnego zarządzania subskrypcjami, zobacz [Azure enterprise scaffold - prescriptive subscription governance](../../azure-resource-manager/resource-manager-subscription-governance.md) (Szkielet platformy Azure dla przedsiębiorstwa — narzucony nadzór subskrypcji).
