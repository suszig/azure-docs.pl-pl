---
title: "Jak dodać operacje do interfejsu API w usłudze Azure API Management | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dodać operacje do interfejsu API w usłudze Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 1158a023-1913-4e9c-93de-9164b672f9b3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 105fc51c2d1152a40a5757985da47330e0b7b8cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Jak dodać operacje do interfejsu API w usłudze Azure API Management
Przed użyciem interfejsu API w usłudze API Management operacji musi zostać dodany. W tym przewodniku pokazano, jak dodać i skonfigurować różne rodzaje operacji interfejsu API w usłudze API Management.

## <a name="add-operation"></a>Dodać operację
Działania są dodawane i skonfigurowane do interfejsu API w portalu wydawcy. Aby uzyskać dostęp do portalu wydawcy, kliknij przycisk **portal wydawcy** w portalu Azure usługi Zarządzanie interfejsami API.

![Portal wydawcy][api-management-management-console]

> Jeśli jeszcze nie masz utworzonego wystąpienia usługi API Management, zobacz [Tworzenie wystąpienia usługi API Management][Create an API Management service instance] w samouczku [Wprowadzenie do usługi Azure API Management][Get started with Azure API Management].
> 
> 

Wybierz żądaną interfejsu API w portalu wydawcy, a następnie wybierz **operacji** kartę. 

![Operacje][api-management-operations]

Kliknij przycisk **operacji dodawania** można dodać nowej operacji. **Nowej operacji** będą wyświetlane i **podpisu** będzie domyślnie wybierany kartę.

![Operacja dodania][api-management-add-operation]

Określ **zlecenie HTTP** wybierając z listy rozwijanej.

![Metoda HTTP][api-management-http-method]

<a name="url-template"></a>

Zdefiniuj szablon adresu URL, wpisz w fragmentu adresu URL składające się z jednego lub więcej segmentów ścieżki adresu URL i zero lub więcej parametrów ciągu zapytania. Szablon adresu URL, dołączone do podstawowego adresu URL interfejsu API, identyfikuje jednej operacji HTTP. Może zawierać jeden lub więcej o nazwie części zmiennych, które są identyfikowane za pomocą nawiasów klamrowych. Części te zmienne są nazywane parametrów szablonu i są dynamicznie przypisywane wartości wyodrębnione z adresu URL żądania, gdy żądanie jest przetwarzane przez interfejs API zarządzania platformy.

> Szablon adresu URL może zawierać wzorców symboli wieloznacznych. Na przykład określenie `/*` przekazuj wszystkie żądania dla tej metody HTTP na spód zakończy się usługi.

![Szablon adresu URL][api-management-url-template]

<a name="rewrite-url-template"></a>

W razie potrzeby można określić **szablonu ponowne zapisywanie adresów URL**. Dzięki temu można użyć standardowego szablonu adresu URL do przetwarzania przychodzących żądań na frontonu, podczas wywoływania zaplecza za pośrednictwem przekonwertowanego adresu URL zgodnie z szablonu ponownego zapisywania. Parametry szablonu z szablonu adres URL powinien być używany w szablonie ponownego zapisywania. W poniższym przykładzie pokazano, jak zawartości typu zakodowane jako segment ścieżki w usłudze sieci web z poprzedniego przykładu można podać jako parametr zapytania w interfejsie API opublikowane za pośrednictwem platformy interfejsu API zarządzania za pomocą szablonów adresu URL.

![Ponowne zapisywanie adresów URL szablonu][api-management-url-template-rewrite]

Obiekty wywołujące do operacji zostanie użyty format `/customers?customerid=ALFKI` i to zostaną zmapowane do `/Customers('ALFKI')` po wywołaniu usługi zaplecza.

**Wyświetl** nazwy i **opis** Podaj opis operacji i służą do zapewniania dokumentacji dla deweloperów przy użyciu tego interfejsu API w portalu dla deweloperów.

![Opis][api-management-description]

Opis operacji można określić jako zwykły tekst lub HTML w **opis** pola tekstowego.

## <a name="operation-caching"></a>Buforowanie operacji
Buforowanie odpowiedzi zmniejsza opóźnienia postrzegane przez konsumentów interfejsu API, obniża zużycie przepustowości i zmniejsza obciążenie HTTP sieci web usługi implementacja interfejsu API. 

Aby łatwo i szybko włączyć buforowanie dla operacji, zaznacz **buforowanie** i sprawdź **włączyć** wyboru.

![Buforowanie][api-management-caching-tab]

**Czas trwania** określa okres czasu, w którym odpowiedzi operacji pozostaje w pamięci podręcznej. Wartość domyślna to 3600 sekund lub 1 godzina.

Pamięć podręczna klucze są używane w celu rozróżnienia odpowiedzi, tak aby odpowiedzi odpowiadającą kluczowi każdego innego pamięci podręcznej zostanie uzyskać własną oddzielne wartość w pamięci podręcznej. Opcjonalnie wprowadź parametrów ciągu zapytania określonego i/lub nagłówków HTTP, które ma być używana podczas obliczania wartości klucza pamięci podręcznej w **różne parametry ciągu zapytania** i **Vary przez nagłówki** odpowiednio pola tekstowe. Gdy żaden nie jest adres URL określony, pełną żądania i służą następujące wartości nagłówka HTTP podczas generowania klucza pamięci podręcznej: **Akceptuj** i **Accept-Charset**.

> Aby uzyskać więcej informacji o pamięci podręcznej i buforowania zasady, zobacz [jak pamięci podręcznej operacji wyniki w usłudze Azure API Management][How to cache operation results in Azure API Management].
> 
> 

## <a name="request-parameters"></a>Parametry żądania
Parametry operacji są zarządzane na karcie Parametry. Parametry określone w **szablon adresu URL** na **podpisu** kartę są automatycznie dodawane i można zmienić, edytując szablon adresu URL. Dodatkowe parametry, można wprowadzić ręcznie.

Aby dodać nowy parametr zapytania, kliknij przycisk **dodać parametru zapytania** i wprowadź następujące informacje:

* **Nazwa** — Nazwa parametru.
* **Opis elementu** -krótki opis parametru (opcjonalnie).
* **Typ** — typ parametru, wybrane w polu listy rozwijanej.
* **Wartości** -wartości, które mogą być przypisane do tego parametru. Jedna z wartości może być oznaczony jako domyślny (opcjonalnie).
* **Wymagane** — określ parametr obowiązkowy, zaznaczając pole wyboru. 

![Parametry żądania][api-management-request-parameters]

## <a name="request-body"></a>Treść żądania
Jeśli zezwala na operację (np. PUT, POST) i wymaga treści może podać przykład go we wszystkich obsługiwanych reprezentacja formaty (np. json, XML). 

> Treść żądania jest używana tylko w celach dokumentacji i nie jest weryfikowany.
> 
> 

Aby wprowadzić treści żądania, przełącz się do **treści** kartę.

Kliknij przycisk **dodać reprezentacja**, zacznij pisać nazwa żądanego typu zawartości (np. application/json), wybierz je w listy rozwijanej i Wklej przykład treści żądania żądaną w wybranym formacie w polu tekstowym. 

![Treść żądania][api-management-request-body]

W dodatkowego do oświadczenia, można także określić opcjonalny opis w **opis** pola tekstowego.

## <a name="responses"></a>Odpowiedzi
Jest dobrą praktyką jest zawierają przykłady odpowiedzi dla wszystkich kodów stanu, które wywołują może wykonać operację. Każdy kod stanu może mieć więcej niż jeden przykład treści odpowiedzi, po jednej dla każdej z obsługiwanych typów zawartości. 

Aby dodać odpowiedzi, kliknij przycisk **Dodaj** i zacznij pisać kod żądany stan. W tym przykładzie jest kod stanu **200 OK**. Po wyświetleniu kod w listy rozwijanej wybierz go, a kod odpowiedzi jest tworzony i dodawany do operacji.

![Kod odpowiedzi][api-management-response-code]

Kliknij przycisk **dodać reprezentacja**, zacznij wpisywać nazwę odpowiedniego typu zawartości (np. application/json), a następnie wybierz na liście rozwijanej.

![Typ zawartości treści][api-management-response-body-content-type]

Wklej przykład treści odpowiedzi w wybranym formacie w polu tekstowym. 

![Treść odpowiedzi][api-management-response-body]

W razie potrzeby dodaj opcjonalny opis w **opis** pola tekstowego.

Po skonfigurowaniu operacji, kliknij przycisk **zapisać**.

## <a name="next-steps"> </a>Następne kroki
Po dodaniu działania do interfejsu API, następnym krokiem jest skojarzenia interfejsu API z produktem i opublikuj go, dzięki czemu deweloperzy mogą wywoływać operacjach.

* [Jak utworzyć i opublikować produktu][How to create and publish a product]

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
