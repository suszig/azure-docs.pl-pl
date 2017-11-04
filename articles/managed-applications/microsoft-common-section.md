---
title: "Azure zarządzanych aplikacji sekcji elementu interfejsu użytkownika | Dokumentacja firmy Microsoft"
description: "Opis elementu Microsoft.Common.Section interfejsu użytkownika dla aplikacji Azure"
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
ms.openlocfilehash: 5a460fde88982c53a7ef3a1ec444d50c1c482fc4
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftcommonsection-ui-element"></a>Element Microsoft.Common.Section interfejsu użytkownika
Formant, który grupuje co najmniej jeden element w pozycji. Użyj tego elementu po [tworzenie aplikacji zarządzanych Azure](publish-service-catalog-app.md).

## <a name="ui-sample"></a>Przykład interfejsu użytkownika
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a>Schemat
```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Some section",
  "elements": [
    {
      "name": "element1",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 1"
    },
    {
      "name": "element2",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>Uwagi
- `elements`musi zawierać co najmniej jeden element i może zawierać wszystkie typy elementów, z wyjątkiem `Microsoft.Common.Section`.
- Ten element nie obsługuje `toolTip` właściwości.

## <a name="sample-output"></a>Przykładowe dane wyjściowe
Aby uzyskać dostęp do danych wyjściowych wartości elementów w `elements`, użyj [basics()](create-uidefinition-functions.md#basics) lub [steps()](create-uidefinition-functions.md#steps) funkcje i kropkowego:

```json
basics('section1').element1
```

Elementy typu `Microsoft.Common.Section` ma same wartości nie danych wyjściowych.

## <a name="next-steps"></a>Następne kroki
* Aby obejrzeć wprowadzenie do aplikacji zarządzanych, zobacz [zarządzanej aplikacji Azure — omówienie](overview.md).
* Aby obejrzeć wprowadzenie do tworzenia definicji interfejsu użytkownika, zobacz [wprowadzenie CreateUiDefinition](create-uidefinition-overview.md).
* Opis właściwości wspólnych elementów interfejsu użytkownika, zobacz [elementy CreateUiDefinition](create-uidefinition-elements.md).
