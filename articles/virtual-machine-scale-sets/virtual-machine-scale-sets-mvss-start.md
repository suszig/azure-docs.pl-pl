---
title: "Więcej informacji na temat szablonów zestaw skali maszyny wirtualnej | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć szablon zestaw minimalnej wielkości zestawy skalowania maszyny wirtualnej"
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
ms.date: 06/01/2017
ms.author: negat
ms.openlocfilehash: e1672474e22411e7f7fca4082ce83146e40ebfbc
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="learn-about-virtual-machine-scale-set-templates"></a>Więcej informacji na temat szablonów zestaw skali maszyny wirtualnej
[Szablony usługi Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) to doskonały sposób wdrażania grup powiązanych zasobów. Tej serii samouczek przedstawia sposób tworzenia szablonu zestaw minimalnej wielkości i sposobu modyfikowania ten szablon służy do potrzeb różnych scenariuszy. Wszystkie przykłady pochodzą od tego [repozytorium GitHub](https://github.com/gatneil/mvss). 

Ten szablon ma być proste. Bardziej szczegółowy przykłady skali szablonów, zobacz [repozytorium GitHub szablonów Szybki Start Azure](https://github.com/Azure/azure-quickstart-templates) i Wyszukaj foldery zawierające ciąg `vmss`.

Jeśli znasz już tworzenie szablonów, możesz przejść do sekcji "Następne kroki", aby zobaczyć, jak modyfikować tego szablonu.

## <a name="review-the-template"></a>Przejrzyj szablonu

Aby przejrzeć naszych minimalnej wielkości Ustaw szablon, użyj GitHub [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).

W tym samouczku omówione różnicowego (`git diff master minimum-viable-scale-set`) do utworzenia minimalnej wielkości Ustaw szablon element przez element.

## <a name="define-schema-and-contentversion"></a>Zdefiniuj $schema i contentVersion
Najpierw należy zdefiniować `$schema` i `contentVersion` w szablonie. `$schema` Element definiuje wersji języka szablonu i jest używany dla programu Visual Studio wyróżnianie składni i podobne funkcje sprawdzania poprawności. `contentVersion` Element nie jest używany przez platformę Azure. Zamiast tego pomaga zachować informacje o wersji szablonu.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```
## <a name="define-parameters"></a>Zdefiniuj parametry
Następnie należy zdefiniować dwa parametry `adminUsername` i `adminPassword`. Parametry są wartościami, które określisz w czasie wdrażania. `adminUsername` Parametr jest po prostu `string` typu, ale ponieważ `adminPassword` jest klucz tajny, zapewniamy im typu `securestring`. Później te parametry są przekazywane do konfiguracji zestaw skali.

```json
  "parameters": {
    "adminUsername": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    }
  },
```
## <a name="define-variables"></a>Definiowanie zmiennych
Szablony Menedżera zasobów pozwalają również zdefiniować zmienne, które mają być używane w dalszej części szablonu. Naszym przykładzie nie używa żadnych zmiennych, więc mamy już puste obiekt JSON.

```json
  "variables": {},
```

## <a name="define-resources"></a>Definiowanie zasobów
Następnie to sekcja zasobów szablonu. W tym miejscu Zdefiniuj co faktycznie chcesz wdrożyć. W odróżnieniu od `parameters` i `variables` (które są obiektów JSON), `resources` znajduje się lista JSON obiektów JSON.

```json
   "resources": [
```

Wszystkie zasoby wymagają `type`, `name`, `apiVersion`, i `location` właściwości. W tym przykładzie pierwszy zasób ma typ `Microsft.Network/virtualNetwork`, nazwa `myVnet`i apiVersion `2016-03-30`. (Aby uzyskać najnowszą wersją interfejsu API dla typu zasobu, zobacz [dokumentacji interfejsu API REST Azure](https://docs.microsoft.com/rest/api/).)

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2016-12-01",
```

## <a name="specify-location"></a>Określ lokalizację
Aby określić lokalizację dla sieci wirtualnej, używamy [funkcji szablonu usługi Resource Manager](../azure-resource-manager/resource-group-template-functions.md). Ta funkcja musi być ujęta w cudzysłowy i nawiasy kwadratowe następująco: `"[<template-function>]"`. W takim przypadku stosujemy `resourceGroup` funkcji. Go przyjmuje żadnych argumentów i zwraca obiekt JSON z metadanych dotyczących grupy zasobów, który jest wdrażany tego wdrożenia. Grupy zasobów jest ustawiony przez użytkownika w czasie wdrażania. Firma Microsoft, a następnie indeks do tego obiektu JSON `.location` można pobrać lokalizacji z obiektu JSON.

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a>Określ właściwości sieci wirtualnej
Każdy zasób usługi Resource Manager ma własną `properties` sekcji konfiguracji specyficznych dla zasobu. W takim przypadku Określ firma Microsoft, aby sieć wirtualna składała się z jednej podsieci przy użyciu zakresu prywatnych adresów IP `10.0.0.0/16`. Zestaw skali zawsze znajduje się w obrębie jednej podsieci. Nie może występować w podsieci.

```json
       "properties": {
         "addressSpace": {
           "addressPrefixes": [
             "10.0.0.0/16"
           ]
         },
         "subnets": [
           {
             "name": "mySubnet",
             "properties": {
               "addressPrefix": "10.0.0.0/16"
             }
           }
         ]
       }
     },
```

## <a name="add-dependson-list"></a>Dodaj listę dependsOn
Oprócz wymaganych `type`, `name`, `apiVersion`, i `location` właściwości, każdy zasób może mieć opcjonalny `dependsOn` lista ciągów. Określa tej listy, która innych zasobów z tego wdrożenia musi zakończyć się przed wdrożeniem tego zasobu.

W takim przypadku istnieje tylko jeden element na liście sieci wirtualnej z poprzedniego przykładu. Możemy określić tę zależność, ponieważ wymaga zestawu skalowania sieci istnieć przed utworzeniem żadnej maszyny wirtualnej. W ten sposób zestaw skali można nadać tych maszyn wirtualnych prywatnych adresów IP z zakresu adresów IP wcześniej określona we właściwościach sieci. Format każdego ciągu na liście dependsOn jest `<type>/<name>`. Używać tego samego `type` i `name` wcześniej używane w definicji zasobu sieci wirtualnej.

```json
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "apiVersion": "2016-04-30-preview",
       "location": "[resourceGroup().location]",
       "dependsOn": [
         "Microsoft.Network/virtualNetworks/myVnet"
       ],
```
## <a name="specify-scale-set-properties"></a>Określ właściwości zestawu skali
Zestawy skalowania ma wiele właściwości dostosowywania w zestawie skalowania maszyn wirtualnych. Aby uzyskać pełną listę tych właściwości, zobacz [dokumentacja interfejsu API REST zestawu skalowania](https://docs.microsoft.com/rest/api/virtualmachinescalesets/create-or-update-a-set). W tym samouczku będziemy ustawi tylko kilka właściwości często używane.
### <a name="supply-vm-size-and-capacity"></a>Rozmiar maszyny Wirtualnej i pojemności
Zestaw skalowania musi wiedzieć, jaki rozmiar maszyny Wirtualnej do utworzenia ("Nazwa jednostki sku") i jak wiele takich maszyn wirtualnych do utworzenia ("pojemność jednostki sku"). Aby wyświetlić rozmiarów maszyn wirtualnych, które są dostępne, zobacz [dokumentacji rozmiarów maszyn wirtualnych](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a>Wybierz typ aktualizacji
Także zestaw skalowania musi wiedzieć, jak obsługiwać aktualizacje na zestawie skali. Obecnie dostępne są dwie opcje `Manual` i `Automatic`. Aby uzyskać więcej informacji na temat różnic między nimi, zobacz dokumentację na [sposobu uaktualniania zestaw skali](./virtual-machine-scale-sets-upgrade-scale-set.md).

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a>Wybierz system operacyjny maszyny Wirtualnej
Trzeba określić systemu operacyjnego, które mają zostać umieszczone na maszynach wirtualnych zestawu skalowania. W tym miejscu utworzymy maszyn wirtualnych z obrazem 16.04 LTS Ubuntu pełni poprawioną.

```json
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
               "publisher": "Canonical",
               "offer": "UbuntuServer",
               "sku": "16.04-LTS",
               "version": "latest"
             }
           },
```

### <a name="specify-computernameprefix"></a>Określ element computerNamePrefix
Zestaw skali wdraża wiele maszyn wirtualnych. Zamiast określania nazwy maszyny Wirtualnej, jest określona `computerNamePrefix`. Zestaw skali dołącza indeksu prefiks dla każdej maszyny Wirtualnej, więc nazw maszyn wirtualnych jest formularz `<computerNamePrefix>_<auto-generated-index>`.

W poniższy fragment używamy parametry z przed można ustawić nazwy użytkownika dla administratora i hasła dla wszystkich maszyn wirtualnych w zestawie skalowania. Jak to z `parameters` funkcji szablonu. Ta funkcja przyjmuje ciąg, który określa parametr, których dotyczy i wyświetla wartość tego parametru.

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a>Określ konfigurację sieci maszyny Wirtualnej
Na koniec należy określić konfigurację sieci dla maszyn wirtualnych w zestawie skalowania. W takim przypadku tylko należy określić identyfikator podsieci utworzony wcześniej. Ta wartość informuje skali ustawioną put interfejsów sieciowych w tej podsieci.

Można pobrać Identyfikatora sieci wirtualnej zawierający podsieci przy użyciu `resourceId` funkcji szablonu. Ta funkcja przyjmuje typ i nazwa zasobu i zwraca pełny identyfikator zasobu. Ten identyfikator ma postać:`/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`

Jednak identyfikator sieci wirtualnej nie jest wystarczająco. Należy określić musi należeć do określonej podsieci, czy skala ustawić maszyn wirtualnych. W tym celu Połącz `/subnets/mySubnet` na identyfikator sieci wirtualnej. Wynik jest pełny identyfikator podsieci. Czy to łączenia z `concat` funkcji, która przyjmuje w szeregu ciągi i zwraca ich łączenie.

```json
           "networkProfile": {
             "networkInterfaceConfigurations": [
               {
                 "name": "myNic",
                 "properties": {
                   "primary": "true",
                   "ipConfigurations": [
                     {
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
                           "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
                         }
                       }
                     }
                   ]
                 }
               }
             ]
           }
         }
       }
     }
   ]
}

```

## <a name="next-steps"></a>Następne kroki

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
