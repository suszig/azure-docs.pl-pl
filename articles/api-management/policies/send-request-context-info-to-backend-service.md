---
title: "Azure próbce interfejsu API zarządzania zasad — informacje o kontekście żądania wysłania do usługi zaplecza | Dokumentacja firmy Microsoft"
description: "Przykładowe zasady zarządzania Azure API - pokazuje, jak wysłać informacje o kontekście żądania do usługi zaplecza."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: af4d0a8aa3705b290fa5e4801eaa25264229696e
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="send-request-context-information-to-the-backend-service"></a>Wyślij informacje o kontekście żądania do usługi zaplecza

W tym artykule przedstawiono przykładowe zasady zarządzania interfejsu API usługi Azure, który demonstruje sposób wysłać informacje o kontekście żądania do usługi zaplecza. Aby ustawić lub zmodyfikować kod zasad, wykonaj czynności opisane w [zestawu lub Edytuj zasady](../set-edit-policies.md). Aby wyświetlić inne przykłady, zobacz [Przykłady zasad](../policy-samples.md).

## <a name="policy"></a>Zasady

Wklej kod do **przychodzących** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Send request context information to the backend service.policy.xml)]

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o zasadach APIM:

+ [Zasady transformacji](../api-management-transformation-policies.md)
+ [Przykłady zasad](../policy-samples.md)

