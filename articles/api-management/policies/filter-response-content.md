---
title: "Azure próbce interfejsu API zarządzania zasad - zawartości odpowiedzi | Dokumentacja firmy Microsoft"
description: "Przykładowe zasady zarządzania Azure API - pokazano, jak filtrować elementy danych z ładunku odpowiedzi oparte na produktu skojarzonego z żądaniem."
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
ms.openlocfilehash: 2acaad9e97f18cca22893c8c41ead4533ac5ae85
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="filter-response-content"></a>Filtrowanie zawartości odpowiedzi

W tym artykule przedstawiono przykład zasad interfejsu API usługi Azure zarządzania, który demonstruje sposób filtrowania elementów danych z ładunku odpowiedzi oparte na produktu skojarzonego z żądaniem. Aby ustawić lub zmodyfikować kod zasad, wykonaj czynności opisane w [zestawu lub Edytuj zasady](../set-edit-policies.md). Aby wyświetlić inne przykłady, zobacz [Przykłady zasad](../policy-samples.md).

## <a name="policy"></a>Zasady

Wklej kod do **wychodzących** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Filter response content based on product name.policy.xml)]

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o zasadach APIM:

+ [Zasady transformacji](../api-management-transformation-policies.md)
+ [Przykłady zasad](../policy-samples.md)

