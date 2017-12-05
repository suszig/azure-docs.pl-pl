---
title: "Przykładowe zasady zarządzania interfejsu API platformy Azure — Dodawanie nagłówka przekazane | Dokumentacja firmy Microsoft"
description: "Przykładowe zasady zarządzania Azure API - pokazano, jak dodać nagłówek przekazane w żądania przychodzącego, aby umożliwić zaplecza interfejsu API, aby utworzyć odpowiednie adresy URL."
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
ms.openlocfilehash: cc2df914532b6cda37c951b65b243e90b63d57cb
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="add-a-forwarded-header"></a>Dodaj nagłówek przekazane

W tym artykule przedstawiono przykładowe zasad zarządzania interfejsu API Azure, który demonstruje sposób dodawania nagłówka przekazane w żądania przychodzącego, aby umożliwić zaplecza interfejsu API, aby utworzyć odpowiednie adresy URL. Aby ustawić lub zmodyfikować kod zasad, wykonaj czynności opisane w [zestawu lub Edytuj zasady](../set-edit-policies.md). Aby wyświetlić inne przykłady, zobacz [Przykłady zasad](../policy-samples.md).

## <a name="code"></a>Kod

Wklej kod do **przychodzących** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Forward gateway hostname to backend for generating correct urls in responses.policy.xml)]

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o zasadach APIM:

+ [Zasady transformacji](../api-management-transformation-policies.md)
+ [Przykłady zasad](../policy-samples.md)
