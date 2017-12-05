---
title: "Zarządzanie interfejsami API Azure — Przykłady zasad | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej o zasadach dostępne do użycia w usłudze Azure API Management."
services: api-management
documentationcenter: 
author: Juliako
manager: cflower
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: apimpm
ms.openlocfilehash: 0e8089cbcc5e38504d6b4c7ced372781f9a5e6d8
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="api-management-policy-samples"></a>Zarządzanie interfejsami API Przykłady zasad

[Zasady](api-management-howto-policies.md) są zaawansowanych możliwości system, który umożliwia wydawcy, aby zmienić zachowanie interfejsu API za pomocą konfiguracji. Zasady to zbiór instrukcji, które są wykonywane sekwencyjnie podczas żądania lub odpowiedzi interfejsu API. Poniższa tabela zawiera łącza do przykładów i zawiera krótki opis każdej próbki.

|||
|---|---|
|**Zasady ruchu przychodzącego**||
|[Dodaj nagłówek przekazane, aby umożliwić zaplecza interfejsu API, aby utworzyć odpowiednie adresy URL](./policies/set-header-to-enable-backend-to-construct-urls.md?toc=api-management/toc.json) |Pokazuje, jak dodać nagłówek przekazane w żądania przychodzącego, aby umożliwić zaplecza interfejsu API, aby utworzyć odpowiednie adresy URL.|
|[Dodaj nagłówek zawierający identyfikator korelacji](./policies/add-correlation-id.md?toc=api-management/toc.json) |Pokazuje, jak dodać nagłówek zawierający identyfikator korelacji żądania przychodzącego.|
|[Dodawanie funkcji do usługi zaplecza i buforowania odpowiedzi](./policies/cache-response.md?toc=api-management/toc.json) |Przedstawiono sposób dodawania funkcji do usługi zaplecza. Na przykład zaakceptuj nazwę miejsca zamiast współrzędne geograficzne w prognozie pogody interfejsu API.|
|[Autoryzacja dostępu na podstawie oświadczeń JWT](./policies/authorize-request-based-on-jwt-claims.md?toc=api-management/toc.json) |Przedstawia sposób autoryzowania dostępu do określonej metody HTTP na interfejs API na podstawie oświadczeń JWT.|
|[Autoryzacja dostępu za pomocą tokenu Google OAuth](./policies/use-google-as-oauth-token-provider.md?toc=api-management/toc.json) |Przedstawia sposób autoryzowania dostępu do punktów końcowych przy użyciu usługi Google jako dostawcy tokenu OAuth.|
|[Generowanie sygnatura dostępu współdzielonego i przekazanie żądania do usługi Azure storage](./policies/generate-shared-access-signature.md?toc=api-management/toc.json) |Pokazuje sposób generowania [sygnatura dostępu współdzielonego](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1) przy użyciu wyrażeń i przekazuje żądanie do magazynu Azure z identyfikatora uri ponownego zapisywania zasad. |
|[Uzyskaj token dostępu OAuth2 z usługi AAD i przesyła je do wewnętrznej bazy danych](./policies/use-oauth2-for-authorization.md?toc=api-management/toc.json) |Udostępnia oraz przykład użycia protokołu OAuth2 autoryzacji między bramą a wewnętrznej bazie danych. Widoczny jest sposób uzyskać token dostępu z usługi AAD i przesyła je do wewnętrznej bazy danych.|
|[CSRF X pobrania tokenu z bramą SAP przy użyciu zasad żądań wysłania](./policies/get-x-csrf-token-from-sap-gateway.md?toc=api-management/toc.json) |Przedstawia sposób wykonania X CSRF wzorzec używany przez wiele interfejsów API. W tym przykładzie jest specyficzne dla programu SAP bramy. |
|[Trasy żądania na podstawie rozmiaru jego treści](./policies/route-requests-based-on-size.md?toc=api-management/toc.json) |Pokazuje, jak można przekierować żądania na podstawie rozmiaru ich jednostki.|
|[Wyślij informacje o kontekście żądania do usługi zaplecza](./policies/send-request-context-info-to-backend-service.md?toc=api-management/toc.json) |Przedstawia sposób wysłania pewne informacje dotyczące kontekstu do wewnętrznej bazy danych usługi rejestrowania lub przetwarzania.|
|[Ustaw czas buforowania odpowiedzi](./policies/set-cache-duration.md?toc=api-management/toc.json) |Pokazuje, jak ustawić czas buforowania odpowiedzi przy użyciu wartości parametru maxAge wysyłane przez zaplecze nagłówka Cache-Control.|
|**Zasady ruchu wychodzącego**||
|[Filtrowanie zawartości odpowiedzi](./policies/filter-response-content.md?toc=api-management/toc.json) | Pokazuje, jak filtrować elementy danych z ładunku odpowiedzi oparte na produktu skojarzonego z żądaniem.|
|**Zasady na błąd**||
|[Rejestruj błędy w Stackify](./policies/log-errors-to-stackify.md?toc=api-management/toc.json) |Przedstawiono sposób dodawania zasady rejestrowania błędów wysłać błędy do Stackify do rejestrowania.|
