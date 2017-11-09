---
title: "Azure zastrzeżone błędów nazw zasobów | Dokumentacja firmy Microsoft"
description: "Opisuje sposób Rozwiązywanie błędów podczas tworzenia nazwy zasobu, zawierający słowo zastrzeżone."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: 2f06b7bc65ea309bc5374b41f402f8e7bc4d2a38
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2017
---
# <a name="resolve-reserved-resource-name-errors"></a>Rozwiązywanie błędów nazw zarezerwowanych zasobów

W tym artykule opisano błąd napotykanych podczas wdrażania zasobu, który zawiera słowo zastrzeżone w jego nazwy.

## <a name="symptom"></a>Objaw

W przypadku wdrażania z zasobem, który jest dostępny za pośrednictwem publicznego punktu końcowego, może zostać wyświetlony następujący błąd:

```
Code=ReservedResourceName;
Message=The resource name <resource-name> or a part of the name is a trademarked or reserved word.
```

## <a name="cause"></a>Przyczyna

Zasoby, które mają publiczny punkt końcowy nie można użyć słowa zastrzeżone lub znakami w nazwie.

Wyrazy są zarezerwowane:

* DOSTĘP
* AZURE
* USŁUGI BING
* BIZSPARK
* BIZTALK
* CORTANA
* PROGRAM DIRECTX
* DOTNET
* DYNAMICS
* EXCEL
* PROGRAM EXCHANGE
* FOREFRONT
* GROOVE
* HOLOLENS
* FUNKCJI HYPER-V
* KINECT
* LYNC
* MSDN
* O365
* OFFICE
* USŁUGI OFFICE 365
* USŁUGI ONEDRIVE
* PROGRAM ONENOTE
* PROGRAM OUTLOOK
* POWERPOINT
* PROGRAM SHAREPOINT
* SKYPE
* VISIO
* VISUAL STUDIO

Nie można użyć słowa jako całe wyrazy lub podciąg nazwy:

* LOGOWANIE
* MICROSOFT
* SYSTEMU WINDOWS
* XBOX

## <a name="solution"></a>Rozwiązanie

Podaj nazwę, która nie używa jedno ze słów zastrzeżonych.