---
title: "Ograniczenia i znane problemy podczas importowania interfejsu API usługi Azure API Management | Dokumentacja firmy Microsoft"
description: "Szczegóły znane problemy i ograniczenia dotyczące importowania do usługi Azure API Management przy użyciu interfejsu API otwarty, WSDL lub WADL formatów."
services: api-management
documentationcenter: 
author: vladvino
manager: vlvinogr
editor: 
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: apipm
ms.openlocfilehash: 4cb6ad53b59b81f906a85027f4ff988bbb78706a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="api-import-restrictions-and-known-issues"></a>Ograniczenia importu interfejsu API i znane problemy
## <a name="about-this-list"></a>Ta lista — informacje
Czasie wszelkich starań, aby upewnić się, że importowanie interfejsu API usługi Azure API Management jest jako bezproblemowe i bezproblemowe jak to możliwe, firma Microsoft od czasu do czasu nakłada ograniczenia lub zidentyfikować problemy, które należy rozwiązać, aby można było pomyślnie zaimportować. W tym artykule opisano, zorganizowanych w formacie importu interfejsu API.

## <a name="open-api"></a>Otwórz interfejs API/Swagger
Ogólnie rzecz biorąc, w przypadku otrzymania błędy importowania dokument otwarty interfejs API, upewnij się, zweryfikowaniu jej — przy użyciu narzędzia Projektant nowego portalu Azure (projekt - Front End — Otwórz interfejs API specyfikacji Edytor), lub z 3rd strony narzędzia takie jak <a href="http://www.swagger.io">edytora programu Swagger</a>.

* **Nazwa hosta** wymagamy atrybutu nazwy hosta.
* **Podstawowa ścieżka** wymagamy atrybut ścieżki podstawowej.
* **Schematy** wymagamy tablicy schematu. 

## <a name="wsdl"></a>WSDL
Pliki WSDL są używane do generowania interfejsów API przekazywanego SOAP lub służyć jako zaplecza interfejsu API SOAP-REST.

* **WSDL: import** interfejsów API przy użyciu tego atrybutu nie jest obecnie obsługiwana. Klienci, należy scalić importowanych elementów w jednym dokumencie.
* **Komunikaty z wielu części** nie są obecnie obsługiwane.
* **WCF wsHttpBinding** usług SOAP utworzone za pomocą programu Windows Communication Foundation należy użyć klasy basicHttpBinding — wsHttpBinding nie jest obsługiwane.
* **MTOM** usług przy użyciu mechanizmu MTOM <em>może</em> pracy. W tej chwili nie jest oferowany oficjalnego wsparcia.
* **Rekursja** typy, które są zdefiniowane cyklicznie (np. Zobacz tablicę sami) nie są obsługiwane.

## <a name="wadl"></a>WADL
Obecnie nie istnieją żadne znane problemy importu WADL.


[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to cache operation results in Azure API Management]: api-management-howto-cache.md
