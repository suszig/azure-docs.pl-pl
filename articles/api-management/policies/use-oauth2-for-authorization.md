---
title: "Azure próbce interfejsu API zarządzania zasad - OAuth2 używany do autoryzacji między bramą a wewnętrznej bazie danych | Dokumentacja firmy Microsoft"
description: "Przykładowe zasady zarządzania Azure API - przedstawiono sposób użycia protokołu OAuth2 autoryzacji między bramą a wewnętrznej bazie danych. Widoczny jest sposób uzyskać token dostępu z usługi AAD i przesyła je do wewnętrznej bazy danych."
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
ms.openlocfilehash: e0aeec66f23033f916c782c8a895e725b0735b62
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="use-oauth2-for-authorization-between-the-gateway-and-a-backend"></a>Użyj protokołu OAuth2 autoryzacji między bramą a wewnętrznej bazie danych

W tym artykule przedstawiono przykład zasad interfejsu API usługi Azure zarządzania, który demonstruje sposób użycia protokołu OAuth2 autoryzacji między bramą a wewnętrznej bazie danych. Widoczny jest sposób uzyskać token dostępu z usługi AAD i przesyła je do wewnętrznej bazy danych. Aby ustawić lub zmodyfikować kod zasad, wykonaj czynności opisane w [zestawu lub Edytuj zasady](../set-edit-policies.md). Aby wyświetlić inne przykłady, zobacz [Przykłady zasad](../policy-samples.md).

## <a name="policy"></a>Zasady

Wklej kod do **przychodzących** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Get OAuth2 access token from AAD and forward it to the backend.xml)]

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o zasadach APIM:

+ [Zasady transformacji](../api-management-transformation-policies.md)
+ [Przykłady zasad](../policy-samples.md)

