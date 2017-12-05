---
title: "Przykładowe zasady zarządzania interfejsu API platformy Azure — Dodaj nagłówek zawierający identyfikator korelacji | Dokumentacja firmy Microsoft"
description: "Przykładowe zasady zarządzania Azure API - pokazano, jak dodać nagłówek zawierający identyfikator korelacji żądania przychodzącego."
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
ms.openlocfilehash: a550009b4442bb59b9b9f4b18593a7537213bb78
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="add-a-header-containing-a-correlation-id"></a>Dodaj nagłówek zawierający identyfikator korelacji

W tym artykule przedstawiono przykład zasad interfejsu API Azure zarządzania, który pokazuje, jak dodać nagłówek zawierający identyfikator korelacji żądania przychodzącego. Aby ustawić lub zmodyfikować kod zasad, wykonaj czynności opisane w [zestawu lub Edytuj zasady](../set-edit-policies.md). Aby wyświetlić inne przykłady, zobacz [Przykłady zasad](../policy-samples.md).

## <a name="policy"></a>Zasady

Wklej kod do **przychodzących** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Add correlation id to inbound request.policy.xml)]

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o zasadach APIM:

+ [Zasady transformacji](../api-management-transformation-policies.md)
+ [Przykłady zasad](../policy-samples.md)

