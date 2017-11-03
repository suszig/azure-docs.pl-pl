---
title: "Łącznik niestandardowy często zadawane pytania — usługi Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Często zadawane pytania dotyczące wymagań, wyzwalaczy i tak dalej, o tworzeniu niestandardowych łączników"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 225e56de3985acae871ddec447b763e7de61cb80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="faq-custom-connectors"></a>— Często zadawane pytania: Łączniki niestandardowe

## <a name="requirements"></a>Wymagania

**Pytanie:** łącznika bez interfejsów API REST można tworzyć? </br>
**Odpowiedź:** nie, aby utworzyć łącznik, zachodzi potrzeba obsługi stabilna interfejsów API REST protokołu HTTP dla usługi. 

**Pytanie:** jakie narzędzia można użyć do utworzenia łącznika? </br>
**Odpowiedź:** platforma Azure ma funkcji i usług, które umożliwiają udostępnianie usługi jako interfejs API, takich jak usługi Azure App Service dla hostingu API Management i inne.

**Pytanie:** typy uwierzytelniania, które są obsługiwane? </br>
**Odpowiedź:** można użyć tych standardów obsługiwanych uwierzytelniania:

* [OAuth 2.0](https://oauth.net/2/), takie jak [usługi Azure Active Directory](https://azure.microsoft.com/develop/identity/) lub określonych usług, takich jak Dropbox, GitHub i SalesForce
* Ogólny OAuth 2.0
* [Uwierzytelnianie podstawowe](https://swagger.io/docs/specification/authentication/basic-authentication/)
* [Klucz interfejsu API](https://swagger.io/docs/specification/authentication/api-keys/)

## <a name="triggers"></a>Wyzwalacze

**Pytanie:** można utworzyć wyzwalaczy bez elementów webhook? </br>
**Odpowiedź:** nie, łączników niestandardowych dla usługi Azure Logic Apps i wyzwalacze oparte tylko na elementu webhook Obsługa Flow firmy Microsoft. Jeśli chcesz zażądać innych wzorcach dla implementacji, skontaktuj się z [ condevhelp@microsoft.com ](mailto:condevhelp@microsoft.com) z bardziej szczegółowymi informacjami na temat interfejsu API.

## <a name="certification"></a>Certyfikat

**Q**: nie mam niezależnego dostawcy oprogramowania (ISV) lub partnera firmy Microsoft. Nadal można utworzyć łączniki? </br>
**A**: tak, możesz zarejestrować te łączniki do użytku wewnętrznego w firmie, ale jeśli chcesz przeprowadzić certyfikację i publicznie wersji łącznika, musi albo posiadanych podległej usłudze lub występuje jawna praw do korzystania z interfejsu API.

## <a name="other"></a>Inne

**Pytanie:** Moje interfejsów API używać dynamicznej hosta. Jak mogę wdrożyć je z OpenAPI? </br>
**Odpowiedź:** łączniki niestandardowe nie obsługuje dynamicznej hostów. Zamiast tego użyj statycznych hosta do programowania i testowania. Jeśli chcesz zatwierdzić z łącznika, poproś kontakt Microsoft o implementacji dynamicznych.

**Pytanie:** czy Postman kolekcji w wersji 2 są obsługiwane? </br>
**Odpowiedź:** nie jest obecnie obsługiwany tylko V1 Postman w kolekcji.

**Pytanie:** czy OpenAPI 3.0 są obsługiwane? </br>
**Odpowiedź:** nie, tylko OpenAPI 2.0 jest obecnie obsługiwany.