---
title: "Azure zarządzanych aplikacji UserNameTextBox elementu interfejsu użytkownika | Dokumentacja firmy Microsoft"
description: "Opis elementu Microsoft.Compute.UserNameTextBox interfejsu użytkownika dla aplikacji Azure"
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
ms.openlocfilehash: 6a395915af274750eb57a085ee51b55fdd392615
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a>Element Microsoft.Compute.UserNameTextBox interfejsu użytkownika
Formant pola tekstowego z wbudowanych sprawdzania poprawności nazwy użytkownika systemu Windows i Linux. Użyj tego elementu po [tworzenie aplikacji zarządzanych Azure](publish-service-catalog-app.md).

## <a name="ui-sample"></a>Przykład interfejsu użytkownika
![Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a>Schemat
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.UserNameTextBox",
  "label": "User name",
  "defaultValue": "",
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "osPlatform": "Windows",
  "visible": true
}
```

## <a name="remarks"></a>Uwagi
- Jeśli `constraints.required` ustawiono **true**, a następnie w polu tekstowym musi zawierać wartość do zweryfikowania pomyślnie. Wartość domyślna to **true**.
- `osPlatform`należy określić i mogą być **Windows** lub **Linux**.
- `constraints.regex`jest wzorzec wyrażenia regularnego JavaScript. Jeśli jest określony, wartość pola tekstowego musi odpowiadać wzorzec można pomyślnie zweryfikować. Wartość domyślna to **null**.
- `constraints.validationMessage`ciąg do wyświetlenia, gdy wartość w polu tekstowym nie powiodło się sprawdzenie poprawności określonej przez `constraints.regex`. Jeśli nie zostanie określony, komunikaty o błędach weryfikacji wbudowane pole tekstowe są używane. Wartość domyślna to **null**.
- Ten element ma wbudowane weryfikacji, który bazuje na wartość określona dla `osPlatform`. Wbudowane sprawdzania poprawności można wraz z niestandardowe wyrażenie regularne.
Jeśli określono wartość `constraints.regex` jest określony, a następnie poprawności wbudowanych i niestandardowych są wyzwalane.

## <a name="sample-output"></a>Przykładowe dane wyjściowe
```json
"tabrezm"
```

## <a name="next-steps"></a>Następne kroki
* Aby obejrzeć wprowadzenie do aplikacji zarządzanych, zobacz [zarządzanej aplikacji Azure — omówienie](overview.md).
* Aby obejrzeć wprowadzenie do tworzenia definicji interfejsu użytkownika, zobacz [wprowadzenie CreateUiDefinition](create-uidefinition-overview.md).
* Opis właściwości wspólnych elementów interfejsu użytkownika, zobacz [elementy CreateUiDefinition](create-uidefinition-elements.md).
