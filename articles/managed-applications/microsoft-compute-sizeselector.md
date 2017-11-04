---
title: "Azure zarządzanych aplikacji SizeSelector elementu interfejsu użytkownika | Dokumentacja firmy Microsoft"
description: "Opis elementu Microsoft.Compute.SizeSelector interfejsu użytkownika dla aplikacji Azure"
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: 72278b1999f89e5bd5f203794ba3a403a695c933
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Element Microsoft.Compute.SizeSelector interfejsu użytkownika
Formant wyboru rozmiar dla co najmniej jedno wystąpienie maszyny wirtualnej. Użyj tego elementu po [tworzenie aplikacji zarządzanych Azure](publish-service-catalog-app.md).

## <a name="ui-sample"></a>Przykład interfejsu użytkownika
![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

## <a name="schema"></a>Schemat
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.SizeSelector",
  "label": "Size",
  "toolTip": "",
  "recommendedSizes": [
    "Standard_D1",
    "Standard_D2",
    "Standard_D3"
  ],
  "constraints": {
    "allowedSizes": [],
    "excludedSizes": []
  },
  "osPlatform": "Windows",
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2012-R2-Datacenter"
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Uwagi
- `recommendedSizes`powinien zawierać co najmniej jeden rozmiar. Zalecany rozmiar pierwszy jest używany jako domyślny.
- Jeśli zalecany rozmiar nie jest dostępny w wybranej lokalizacji, rozmiar automatycznie zostanie pominięty. Zamiast tego dalej zalecany rozmiar jest używany.
- Dowolnej wielkości, nie jest określona w `constraints.allowedSizes` jest ukryta i rozmiarze nie jest określona w `constraints.excludedSizes` jest wyświetlany.
`constraints.allowedSizes`i `constraints.excludedSizes` są opcjonalne, ale nie mogą być używane jednocześnie. Listę dostępnych rozmiarów można ustalić wywołując [listy dostępne rozmiary maszyny wirtualnej w ramach subskrypcji](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).
- `osPlatform`należy określić i mogą być **Windows** lub **Linux**. Służy do określenia koszty sprzętu, maszyn wirtualnych.
- `imageReference`Pominięto obrazów firmy, ale podany dla obrazów innych firm. Służy do określania oprogramowania kosztów maszyn wirtualnych.
- `count`Służy do ustawiania odpowiednich mnożnik dla elementu. Obsługuje ona wartość statyczną, takie jak **2**, lub wartość dynamiczną z innego elementu, tak jak `[steps('step1').vmCount]`. Wartość domyślna to **1**.

## <a name="sample-output"></a>Przykładowe dane wyjściowe
```json
"Standard_D1"
```

## <a name="next-steps"></a>Następne kroki
* Aby obejrzeć wprowadzenie do aplikacji zarządzanych, zobacz [zarządzanej aplikacji Azure — omówienie](overview.md).
* Aby obejrzeć wprowadzenie do tworzenia definicji interfejsu użytkownika, zobacz [wprowadzenie CreateUiDefinition](create-uidefinition-overview.md).
* Opis właściwości wspólnych elementów interfejsu użytkownika, zobacz [elementy CreateUiDefinition](create-uidefinition-elements.md).
