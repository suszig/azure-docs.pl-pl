---
title: "Azure zarządzanej aplikacji tworzenia interfejsu użytkownika funkcji definicji | Dokumentacja firmy Microsoft"
description: "Opisuje funkcje do użycia podczas konstruowania definicji interfejsu użytkownika dla aplikacji zarządzanych platformy Azure"
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: 23e407bf93bc51116ca45339bffcb801d69290f0
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="createuidefinition-elements"></a>Elementy CreateUiDefinition
W tym artykule opisano schematu i właściwości dla wszystkich elementów obsługiwanych CreateUiDefinition. Użyj tych elementów przy [tworzenie aplikacji zarządzanych Azure](publish-service-catalog-app.md). Schemat dla większości elementów jest następujący:

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "foobar",
  "toolTip": "Keep calm and visit the [Azure Portal](portal.azure.com).",
  "constraints": {},
  "options": {},
  "visible": true
}
```

| Właściwość | Wymagane | Opis |
| -------- | -------- | ----------- |
| name | Tak | Wewnętrzny identyfikator, aby odwołać konkretne wystąpienie elementu. Najbardziej typowe użycie w nazwie elementu znajduje się w `outputs`, gdzie wartości określonych elementów danych wyjściowych są mapowane do parametrów szablonu. Można go powiązać wartość wyjściowa elementu `defaultValue` innego elementu. |
| type | Tak | Formant interfejsu użytkownika do renderowania elementu. Aby uzyskać listę obsługiwanych typów, zobacz [elementy](#elements). |
| Etykiety | Tak | Wyświetlany tekst elementu. Niektóre typy elementu zawiera wiele etykiet, może to być obiekt zawierający wiele ciągów. |
| Wartość domyślna | Nie | Wartość domyślna elementu. Niektóre typy elementu obsługuje złożone domyślne wartości, wartość może być obiektem. |
| Etykietka narzędzia | Nie | Tekst wyświetlany w etykietce narzędzia elementu. Podobnie jak `label`, niektóre elementy obsługi wielu ciągów Porada narzędzia. Można ją osadzić łącza wbudowanego przy użyciu składni języka Markdown.
| Ograniczenia | Nie | Co najmniej jednej właściwości, które są używane, aby dostosować zachowanie sprawdzania poprawności elementu. Obsługiwane właściwości ograniczenia zależy od typu elementu. Niektóre typy elementu nie obsługuje dostosowywania sprawdzania poprawności, a w związku z tym mają właściwość nie ograniczeń. |
| Opcje | Nie | Dodatkowe właściwości, które dostosowują zachowanie elementu. Podobnie jak `constraints`, obsługiwanych właściwości zależy od typu elementu. |
| Widoczne | Nie | Wskazuje, czy element jest wyświetlany. Jeśli `true`, element i elementy podrzędne stosowane są wyświetlane. Wartość domyślna to `true`. Użyj [funkcje logiczne](create-uidefinition-functions.md#logical-functions) dynamicznie kontrolować wartość tej właściwości.

## <a name="elements"></a>Elementy

Dokumentację dla każdego elementu zawiera przykładowe interfejsu użytkownika, schematem, uwagi na zachowanie elementu (zazwyczaj dotyczących sprawdzania poprawności i dostosowywania obsługiwanych) i przykładowe dane wyjściowe.

- [Microsoft.Common.DropDown](microsoft-common-dropdown.md)
- [Microsoft.Common.FileUpload](microsoft-common-fileupload.md)
- [Microsoft.Common.OptionsGroup](microsoft-common-optionsgroup.md)
- [Microsoft.Common.PasswordBox](microsoft-common-passwordbox.md)
- [Microsoft.Common.Section](microsoft-common-section.md)
- [Microsoft.Common.TextBox](microsoft-common-textbox.md)
- [Microsoft.Compute.CredentialsCombo](microsoft-compute-credentialscombo.md)
- [Microsoft.Compute.SizeSelector](microsoft-compute-sizeselector.md)
- [Microsoft.Compute.UserNameTextBox](microsoft-compute-usernametextbox.md)
- [Microsoft.Network.PublicIpAddressCombo](microsoft-network-publicipaddresscombo.md)
- [Microsoft.Network.VirtualNetworkCombo](microsoft-network-virtualnetworkcombo.md)
- [Microsoft.Storage.MultiStorageAccountCombo](microsoft-storage-multistorageaccountcombo.md)
- [Microsoft.Storage.StorageAccountSelector](microsoft-storage-storageaccountselector.md)

## <a name="next-steps"></a>Następne kroki
* Aby obejrzeć wprowadzenie do aplikacji zarządzanych, zobacz [zarządzanej aplikacji Azure — omówienie](overview.md).
* Aby obejrzeć wprowadzenie do tworzenia definicji interfejsu użytkownika, zobacz [wprowadzenie CreateUiDefinition](create-uidefinition-overview.md).
