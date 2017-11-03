---
title: "Wymuszanie zabezpieczeń przy użyciu zasad na maszynach wirtualnych systemu Windows na platformie Azure | Dokumentacja firmy Microsoft"
description: "Sposób stosowania zasad do Menedżera zasobów systemu Windows maszyny wirtualnej platformy Azure"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0b71ba54-01db-43ad-9bca-8ab358ae141b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: kasing
ms.openlocfilehash: 246f5958478fd6d9afc9ba990413ab08429bd25d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="apply-policies-to-windows-vms-with-azure-resource-manager"></a>Stosowania zasad do maszyn wirtualnych systemu Windows z usługą Azure Resource Manager
Korzystając z zasad, organizacja może wymusić różnych konwencje i zasady w całym przedsiębiorstwie. Wymuszanie zachowanie można zmniejszenia ryzyka podczas pracy nad dla sukcesu organizacji. W tym artykule opisano sposób można użyć zasad usługi Azure Resource Manager do definiowania zachowanie w przypadku maszyn wirtualnych w organizacji.

Aby obejrzeć wprowadzenie do zasad, zobacz [używanie zasad do zarządzania zasobami i kontrola dostępu](../../azure-resource-manager/resource-manager-policy.md).

## <a name="permitted-virtual-machines"></a>Dozwolone maszyny wirtualne
Aby upewnić się, że maszyny wirtualne w Twojej organizacji są zgodne z aplikacji, można ograniczyć dozwolonych systemów operacyjnych. W poniższym przykładzie zasad musisz zezwolić na tylko systemu Windows Server 2012 R2 Datacenter maszyn wirtualnych należy utworzyć:

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
                "MicrosoftWindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "WindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "2012-R2-Datacenter"
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

Użyj symbolu wieloznacznego, aby zmodyfikować poprzedniego zasadę, aby dopuścić żadnego obrazu systemu Windows Server Datacenter:

```json
{
  "field": "Microsoft.Compute/imageSku",
  "like": "*Datacenter"
}
```

Użyj anyOf zmodyfikować zasady poprzedniego zezwalająca na wszystkie Windows Server 2012 R2 Datacenter lub wyższej obrazu:

```json
{
  "anyOf": [
    {
      "field": "Microsoft.Compute/imageSku",
      "like": "2012-R2-Datacenter*"
    },
    {
      "field": "Microsoft.Compute/imageSku",
      "like": "2016-Datacenter*"
    }
  ]
}
```

Aby uzyskać informacji o polach zasad, zobacz [aliasy zasad](../../azure-resource-manager/resource-manager-policy.md#aliases).

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


## <a name="azure-hybrid-use-benefit"></a>Korzyści Użyj hybrydowe platformy Azure

Gdy użytkownik ma licencję lokalnie, można zapisać opłata licencji na maszynach wirtualnych. Jeśli nie masz licencji, należy zabraniać opcji. Następujące zasady zabrania użycia korzyści Użyj Azure hybrydowych (AHUB):

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in":[ "Microsoft.Compute/virtualMachines","Microsoft.Compute/VirtualMachineScaleSets"]
            },
            {
                "field": "Microsoft.Compute/licenseType",
                "exists": true
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

## <a name="next-steps"></a>Następne kroki
* Po zdefiniowaniu reguły zasad (jak pokazano w powyższych przykładach), należy utworzyć definicji zasad i przypisać je do zakresu. Zakres może być subskrypcji, grupy zasobów lub zasobów. Aby przypisać zasady za pośrednictwem portalu, zobacz [portal Azure używany do przypisywania i zarządzanie zasadami zasobów](../../azure-resource-manager/resource-manager-policy-portal.md). Aby przypisać zasady za pomocą interfejsu API REST, programu PowerShell lub interfejsu wiersza polecenia Azure, zobacz [przypisania zasad i zarządzania nimi za pośrednictwem skryptu](../../azure-resource-manager/resource-manager-policy-create-assign.md).
* Aby obejrzeć wprowadzenie do zasad zasobów, zobacz [Przegląd zasad zasobów](../../azure-resource-manager/resource-manager-policy.md).
* Aby uzyskać instrukcje dla przedsiębiorstw dotyczące użycia usługi Resource Manager w celu efektywnego zarządzania subskrypcjami, zobacz [Azure enterprise scaffold - prescriptive subscription governance](../../azure-resource-manager/resource-manager-subscription-governance.md) (Szkielet platformy Azure dla przedsiębiorstwa — narzucony nadzór subskrypcji).
