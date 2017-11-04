---
title: "Konwertuj szablonu zestaw skalowania usługi Azure Resource Manager pod kątem używania dysku zarządzanego | Dokumentacja firmy Microsoft"
description: "Konwertuj szablonu zestaw skali do szablonu zestawu dysków zarządzanych w skali."
keywords: zestawy skalowania maszyny wirtualnej
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: bc8c377a-8c3f-45b8-8b2d-acc2d6d0b1e8
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/18/2017
ms.author: negat
ms.openlocfilehash: 2f5cb85703888c5056611d466f508547ee72e44b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="convert-a-scale-set-template-to-a-managed-disk-scale-set-template"></a>Konwertuj szablonu zestaw skalowania do szablonu zestawu dysków zarządzanych w skali

Klienci z szablonem usługi Resource Manager służący do tworzenia zestawu skali nie przy użyciu dysków zarządzanych w możesz zmodyfikować, aby użyć dysków zarządzanych. W tym artykule pokazano, jak to zrobić, używając jako przykład żądanie ściągnięcia z [szablonów Szybki Start Azure](https://github.com/Azure/azure-quickstart-templates), społeczność repozytorium dla przykładowych szablonów usługi Resource Manager. Żądanie ściągnięcia pełne są widoczne w tym miejscu: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), oraz odpowiednich części różnicowego są poniżej, oraz objaśnienia:

## <a name="making-the-os-disks-managed"></a>Tworzenie dysków systemu operacyjnego zarządzania

W poniższych różnicowego możemy stwierdzić, usunęliśmy kilku zmiennych związane z właściwości dysk i konto magazynu. Typ konta magazynu nie jest już konieczne (Standard_LRS jest wartość domyślna), ale firma Microsoft może nadal określić czy firma zamierza. Tylko Standard_LRS i Premium_LRS są obsługiwane z dyskiem zarządzanym. Nowy sufiks konta magazynu, macierzy unikatowy ciąg i liczba sa były używane w starego szablonu, aby wygenerować nazw kont magazynu. Te zmienne nie są już wymagane w nowym szablonie ponieważ dysków zarządzanych automatycznie tworzy kont magazynu w imieniu klienta. Podobnie, nazwa kontenera wirtualnego dysku twardego i nazwa dysku systemu operacyjnego nie są już wymagane ponieważ dysków zarządzanych automatycznie nazwy jest kontenery magazynu obiektów blob i dyski.

```diff
   "variables": {
-    "storageAccountType": "Standard_LRS",
     "namingInfix": "[toLower(substring(concat(parameters('vmssName'), uniqueString(resourceGroup().id)), 0, 9))]",
     "longNamingInfix": "[toLower(parameters('vmssName'))]",
-    "newStorageAccountSuffix": "[concat(variables('namingInfix'), 'sa')]",
-    "uniqueStringArray": [
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '0')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '1')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '2')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '3')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '4')))]"
-    ],
-    "saCount": "[length(variables('uniqueStringArray'))]",
-    "vhdContainerName": "[concat(variables('namingInfix'), 'vhd')]",
-    "osDiskName": "[concat(variables('namingInfix'), 'osdisk')]",
     "addressPrefix": "10.0.0.0/16",
     "subnetPrefix": "10.0.0.0/24",
     "virtualNetworkName": "[concat(variables('namingInfix'), 'vnet')]",
```


W poniższych różnicowego, zobaczysz, że Zaktualizowaliśmy mocy obliczeniowej wersja interfejsu api do 2016-04-30-preview, czyli tak szybko, jak wersja wymagane do obsługi dysków zarządzanych zestawów skali. Należy pamiętać, że firma Microsoft może nadal niezarządzane dysków w nowej wersji interfejsu api z stara składnia w razie potrzeby. Innymi słowy Jeśli możemy aktualizować tylko obliczeniowe wersja interfejsu api i nie należy zmieniać cokolwiek innego, szablon będą nadal działać tak jak poprzednio.

```diff
@@ -86,7 +74,7 @@
       "version": "latest"
     },
     "imageReference": "[variables('osType')]",
-    "computeApiVersion": "2016-03-30",
+    "computeApiVersion": "2016-04-30-preview",
     "networkApiVersion": "2016-03-30",
     "storageApiVersion": "2015-06-15"
   },
```

W poniższych różnicowego możemy stwierdzić, że zostaną usunięte zasobów konta magazynu z tablicy zasobów całkowicie. Firma Microsoft nie jest już potrzebne dysków zarządzanych w tworzy je automatycznie w naszym imieniu.

```diff
@@ -113,19 +101,6 @@
       }
     },
-    {
-      "type": "Microsoft.Storage/storageAccounts",
-      "name": "[concat(variables('uniqueStringArray')[copyIndex()], variables('newStorageAccountSuffix'))]",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "[variables('storageApiVersion')]",
-      "copy": {
-        "name": "storageLoop",
-        "count": "[variables('saCount')]"
-      },
-      "properties": {
-        "accountType": "[variables('storageAccountType')]"
-      }
-    },
     {
       "type": "Microsoft.Network/publicIPAddresses",
       "name": "[variables('publicIPAddressName')]",
       "location": "[resourceGroup().location]",
```

W poniższych różnicowego, możemy stwierdzić, możemy usuwanie zależy od klauzuli odwołujących się od skali ustawioną pętli, które tworzenia kont magazynu. Starego szablonu to został zapewnienie, że konta magazynu zostały utworzone przed zestaw skalowania rozpoczęcia tworzenia, ale ta klauzula nie jest już konieczne z dysków zarządzanych. Możemy również usunąć właściwość kontenery dysku vhd, a właściwość nazwa dysku systemu operacyjnego zgodnie z tymi właściwościami automatycznie są obsługiwane pod maską przez dysków zarządzanych. Jeśli firma zamierza, firma Microsoft może dodać `"managedDisk": { "storageAccountType": "Premium_LRS" }` w konfiguracji "osDisk", jeśli firma chce dysków systemu operacyjnego w warstwie premium. Tylko maszyny wirtualne z wielkie lub małe firmy "w maszynie Wirtualnej sku można było używać dysków premium.

```diff
@@ -183,7 +158,6 @@
       "location": "[resourceGroup().location]",
       "apiVersion": "[variables('computeApiVersion')]",
       "dependsOn": [
-        "storageLoop",
         "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
         "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
       ],
@@ -200,16 +174,8 @@
         "virtualMachineProfile": {
           "storageProfile": {
             "osDisk": {
-              "vhdContainers": [
-                "[concat('https://', variables('uniqueStringArray')[0], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[1], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[2], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[3], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[4], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]"
-              ],
-              "name": "[variables('osDiskName')]",
             },
             "imageReference": "[variables('imageReference')]"
           },

```

Nie ma jawnego właściwości w konfiguracji zestaw skali, czy należy użyć dysku zarządzane lub niezarządzane. Zestaw skali wie, który ma zostać użyty na podstawie właściwości, które znajdują się w profilu magazynu. W związku z tym ważne jest podczas modyfikowania szablonu w celu upewnij się, że właściwości prawa w profilu magazynu zestawu skali.


## <a name="data-disks"></a>Dyski danych

Z powyższych zmiany skali zestaw używa zarządzanych dysków dla systemu operacyjnego na dysku, ale informacje o dyskach danych? Aby dodać dysk danych, należy dodać właściwości "dataDisks" w "storageProfile" na tym samym poziomie jako "osDisk". Wartość właściwości jest JSON listę obiektów, z których każdy ma właściwości "lun" (która muszą być unikatowe dla każdego dysku danych na maszynie Wirtualnej), "createOption" ("pusty" jest obecnie jedyną obsługiwaną opcją), a "diskSizeGB" (rozmiar dysku w gigabajtach; musi być większa niż 0 i mniejsza niż 1024) jak w poniższym przykładzie: 

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

Jeśli określisz `n` dysków w tej macierzy, pobiera zestawu każdej maszyny Wirtualnej w skali `n` dysków z danymi. Należy jednak pamiętać, że te dyski danych są urządzeniach niesformatowanych. Nie są sformatowane. Jest klienta, aby dołączyć, paritition i sformatować dyski przed ich użyciem. Opcjonalnie można również określono `"managedDisk": { "storageAccountType": "Premium_LRS" }` w każdy obiekt dysku danych, aby określić, czy powinien być dysk danych — warstwa premium. Tylko maszyny wirtualne z wielkie lub małe firmy "w maszynie Wirtualnej sku można było używać dysków premium.

Aby dowiedzieć się więcej o korzystaniu z dysków z danymi z zestawy skalowania, zobacz [w tym artykule](./virtual-machine-scale-sets-attached-disks.md).


## <a name="next-steps"></a>Następne kroki
Na przykład szablony Menedżera zasobów za pomocą zestawów skali, wyszukaj termin "vmss" w [repozytorium github szablonów Szybki Start Azure](https://github.com/Azure/azure-quickstart-templates).

Aby uzyskać ogólne informacje, zapoznaj się z [strony głównej docelowej dla zestawów skalowania](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

