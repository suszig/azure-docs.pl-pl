---
title: "Odwołanie istniejącej sieci wirtualnej w szablonie zestaw skalowania Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dodać sieć wirtualną do istniejącego zestawu skalowania maszyn wirtualnych Azure szablonu"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: negat
ms.openlocfilehash: 28117d467b491704aed8d45e5eba42530579dfa2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>Dodaj odwołanie do istniejącej sieci wirtualnej w szablonie zestaw skalowania Azure

W tym artykule przedstawiono sposób modyfikowania [minimalnej wielkości Ustaw szablon](./virtual-machine-scale-sets-mvss-start.md) do wdrożenia w ramach istniejącej sieci wirtualnej, zamiast tworzyć nowy.

## <a name="change-the-template-definition"></a>Zmiany definicji szablonu

Nasze minimalnej wielkości Ustaw szablon może być widoczny [tutaj](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), i naszych szablon do wdrażania do istniejącej sieci wirtualnej zestaw skali są widoczne [tutaj](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json). Przeanalizujmy różnicowego używany do tworzenia tego szablonu (`git diff minimum-viable-scale-set existing-vnet`) element przez element:

Najpierw dodamy `subnetId` parametru. Ten ciąg zostanie przekazany Konfiguracja zestawu skali, dzięki czemu zestaw do identyfikowania wstępnie utworzone podsieci do wdrażania maszyn wirtualnych do skalowania. Ten ciąg musi mieć postać: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`. Na przykład, aby wdrożyć skali należy ustawić w istniejącej sieci wirtualnej o nazwie `myvnet`, podsieci `mysubnet`, grupy zasobów `myrg`i subskrypcji `00000000-0000-0000-0000-000000000000`, będzie subnetId: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "subnetId": {
+      "type": "string"
     }
   },
```

Następnie można usunąć zasobu sieci wirtualnej z `resources` tablicy, ponieważ firma Microsoft używają istniejącej sieci wirtualnej, a nie potrzeba wdrożenia nowej.

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2016-12-01",
-      "properties": {
-        "addressSpace": {
-          "addressPrefixes": [
-            "10.0.0.0/16"
-          ]
-        },
-        "subnets": [
-          {
-            "name": "mySubnet",
-            "properties": {
-              "addressPrefix": "10.0.0.0/16"
-            }
-          }
-        ]
-      }
-    },
```

Sieć wirtualna już istnieje przed wdrożeniem szablon, więc nie istnieje potrzeba do określenia klauzuli dependsOn od skali ustawioną sieci wirtualnej. W związku z tym usunąć te wiersze:

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

Na koniec jest przekazywana w `subnetId` parametru ustawiony przez użytkownika (zamiast `resourceId` można pobrać identyfikatora sieci wirtualnej w ramach tego samego wdrożenia, który ma co minimalnej wielkości ustawić szablon jest).

```diff
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
-                          "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
+                          "id": "[parameters('subnetId')]"
                         }
                       }
                     }
```




## <a name="next-steps"></a>Następne kroki

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
