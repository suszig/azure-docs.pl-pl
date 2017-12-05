---
title: "Azure próbce interfejsu API zarządzania zasad - X-CSRF implementacji wzorca | Dokumentacja firmy Microsoft"
description: "Przykładowe zasady zarządzania Azure API - pokazano, jak zaimplementować wzorzec X CSRF używany przez wiele interfejsów API. W tym przykładzie jest specyficzne dla programu SAP bramy."
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
ms.openlocfilehash: a99ff25f7f8a3851936ba8d28e76eff98b1b0458
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="implement-x-csrf-pattern"></a>Implementowanie X-CSRF wzorca

W tym artykule przedstawiono przykładowe zasady zarządzania interfejsu API usługi Azure, który demonstruje sposób wykonania X CSRF wzorzec używany przez wiele interfejsów API. W tym przykładzie jest specyficzne dla programu SAP bramy. Aby ustawić lub zmodyfikować kod zasad, wykonaj czynności opisane w [zestawu lub Edytuj zasady](../set-edit-policies.md). Aby wyświetlić inne przykłady, zobacz [Przykłady zasad](../policy-samples.md).

## <a name="policy"></a>Zasady

Wklej kod do **przychodzących** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Get X-CSRF token from SAP gateway using send request policy.xml)]

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o zasadach APIM:

+ [Zasady transformacji](../api-management-transformation-policies.md)
+ [Przykłady zasad](../policy-samples.md)

