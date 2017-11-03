---
title: "Odwołanie obraz niestandardowy w skali Azure Ustaw szablon | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dodać do istniejącego zestawu skalowania maszyn wirtualnych Azure szablonu niestandardowego obrazu"
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
ms.date: 5/10/2017
ms.author: negat
ms.openlocfilehash: cf52fc9e95267c4bc5c0106aadf626685ddd5c24
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="add-a-custom-image-to-an-azure-scale-set-template"></a>Dodawanie niestandardowego obrazu do szablonu zestaw skalowania Azure

W tym artykule przedstawiono sposób modyfikowania [minimalnej wielkości Ustaw szablon](./virtual-machine-scale-sets-mvss-start.md) do wdrożenia z niestandardowego obrazu.

## <a name="change-the-template-definition"></a>Zmiany definicji szablonu

Nasze minimalnej wielkości Ustaw szablon może być widoczny [tutaj](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), i naszych szablonu dla wdrażania skali zestawu z niestandardowego obrazu widoczne [tutaj](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json). Przeanalizujmy różnicowego używany do tworzenia tego szablonu (`git diff minimum-viable-scale-set custom-image`) element przez element:

### <a name="creating-a-managed-disk-image"></a>Tworzenie obrazu dysku zarządzanego

Jeśli masz już obraz niestandardowy dysku zarządzanego (zasobu typu `Microsoft.Compute/images`), a następnie można pominąć tę sekcję.

Najpierw dodamy `sourceImageVhdUri` parametr, który jest identyfikatorem URI do ogólnych obiektu blob w magazynie Azure, który zawiera niestandardowy obraz do wdrożenia z.


```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "sourceImageVhdUri": {
+      "type": "string",
+      "metadata": {
+        "description": "The source of the generalized blob containing the custom image"
+      }
     }
   },
   "variables": {},
```

Następnie dodamy zasobu typu `Microsoft.Compute/images`, która jest oparta na ogólnych znajdujący się pod identyfikatorem URI obiektu blob obrazu dysków zarządzanych w `sourceImageVhdUri`. Ten obraz musi być w tym samym regionie co zestaw skalowania, która korzysta z niego. We właściwościach obrazu określono typ systemu operacyjnego, lokalizacji obiektu blob (z `sourceImageVhdUri` parametru), a typ konta magazynu:

```diff
   "resources": [
     {
+      "type": "Microsoft.Compute/images",
+      "apiVersion": "2016-04-30-preview",
+      "name": "myCustomImage",
+      "location": "[resourceGroup().location]",
+      "properties": {
+        "storageProfile": {
+          "osDisk": {
+            "osType": "Linux",
+            "osState": "Generalized",
+            "blobUri": "[parameters('sourceImageVhdUri')]",
+            "storageAccountType": "Standard_LRS"
+          }
+        }
+      }
+    },
+    {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "location": "[resourceGroup().location]",

```

W zestawie skalowania zasobu, dodamy `dependsOn` klauzuli odwołujących się do niestandardowego obrazu, aby upewnić się, że obraz jest tworzony przed skali podejmuje próbę wdrożenia z tego obrazu:

```diff
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
       "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
+        "Microsoft.Network/virtualNetworks/myVnet",
+        "Microsoft.Compute/images/myCustomImage"
       ],
       "sku": {
         "name": "Standard_A1",

```

### <a name="changing-scale-set-properties-to-use-the-managed-disk-image"></a>Zmiana skali Ustaw właściwości, aby używać obrazu dysku twardego zarządzanego

W `imageReference` skali ustawić `storageProfile`, zamiast określania wydawcy, oferty, jednostki sku i wersji obrazu platformy, określono `id` z `Microsoft.Compute/images` zasobów:

```diff
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
-              "publisher": "Canonical",
-              "offer": "UbuntuServer",
-              "sku": "16.04-LTS",
-              "version": "latest"
+              "id": "[resourceId('Microsoft.Compute/images', 'myCustomImage')]"
             }
           },
           "osProfile": {
```

W tym przykładzie używamy `resourceId` funkcji, aby uzyskać identyfikator zasobu obrazu utworzonego w tym samym szablonie. Jeśli wcześniej utworzono obrazu dysku twardego zarządzanego, należy zamiast tego Podaj identyfikator obrazu. Ten identyfikator musi mieć postać: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.


## <a name="next-steps"></a>Następne kroki

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
