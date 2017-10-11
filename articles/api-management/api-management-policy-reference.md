---
title: "Informacje o zasadach usługi Azure API Management"
description: "Więcej informacji na temat zasad można skonfigurować zarządzanie interfejsami API."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 9d4ac609-b221-4032-93ae-e27a1254d43d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: adc0c4415e10ddd0b4994cecef17f026546e91a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-api-management-policy-reference"></a>Informacje o zasadach usługi Azure API Management
Ta sekcja zawiera indeks dla zasad w [informacje o zasadach usługi API Management][API Management policy reference]. Aby uzyskać informacje dotyczące dodawania i konfigurowania zasad, zobacz [zasad w usłudze API Management][Policies in API Management].

Wyrażenia zasad mogą służyć jako wartości atrybutów lub wartości tekstowe w dowolnej z zasad usługi API Management, o ile w zasadach nie określono inaczej. Niektóre zasady, takie jak [sterowania przepływem] [ Control flow] i [zmiennej zestaw] [ Set variable] zasady są oparte na wyrażeniach zasad. Aby uzyskać więcej informacji, zobacz [zaawansowane zasady] [ Advanced policies] i [wyrażenie zasad][Policy expressions]

## <a name="policy-reference-index"></a>Informacje ogólne o zasadach — indeks
* [Zasady ograniczeń dostępu][Access restriction policies]
  * [Nagłówek HTTP wyboru] [ Check HTTP header] — wymusza istnienia i/lub wartość nagłówka HTTP.
  * [Limit szybkości wywołanie przez subskrypcji] [ Limit call rate by subscription] -API uniemożliwia użycie wzrósł poprzez ograniczenie wywołań szybkości, na podstawie subskrypcji na.
  * [Limit szybkości wywołanie przez klucz](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) — użycie uniemożliwia API wzrósł ograniczając szybkość połączenia, na podstawie na klucz.
  * [Ograniczenia adresów IP wywołującego] [ Restrict caller IPs] -wywołania filtrów (umożliwia/nie zezwala na) z określonych adresów IP i/lub zakresów adresów.
  * [Ustaw przydział użycia subskrypcji] [ Set usage quota by subscription] — umożliwia egzekwowanie odnawialnymi lub okres istnienia wywołania woluminu i/lub przepustowości limit przydziału, na podstawie na subskrypcję.
  * [Ustaw przydział użycia przez klucz](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) — umożliwia egzekwowanie odnawialnymi lub okres istnienia wywołania woluminu i/lub przepustowości limit przydziału, na podstawie według klucza.
  * [Sprawdź poprawność JWT] [ Validate JWT] — wymusza istnienia i ważności wyodrębniony z określonego nagłówka HTTP lub parametr zapytania określony token JWT.
* [Zaawansowane zasady][Advanced policies]
  * [Przepływ kontroli] [ Control flow] — warunkowo stosuje deklaracji zasad, na podstawie wyników oceny Boolean [wyrażenia][expressions].
  * [Przekazanie żądania] [ Forward request] -przekazuje żądanie do usługi zaplecza.
  * [Dziennika do Centrum zdarzeń] [ Log to Event Hub] — wysyła komunikaty w określonym formacie do obiektu docelowego komunikatu zdefiniowane przez [rejestratora](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) jednostki.
  * [Spróbuj ponownie](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) -ponowi próbę wykonania instrukcji objętego zasad, jeśli i do momentu spełnienia warunku. Wykonanie Powtórz w określonych odstępach czasu, a także do określonej liczba ponownych prób.
  * [Odpowiedź zwrócona](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) -przerwań potoku wykonywania i zwraca określoną odpowiedzią bezpośrednio do obiektu wywołującego.
  * [Wyślij żądanie jednokierunkowej](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) — wysyła żądanie pod określony adres URL bez oczekiwania na odpowiedź.
  * [Wyślij żądanie](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) — wysyła żądanie do określonego adresu URL.
  * [Ustawia metodę żądania](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) — umożliwia zmianę metody HTTP dla żądania.
  * [Ustaw stan](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) — zmienia kod stanu HTTP do określonej wartości.
  * [Ustaw zmienną] [ Set variable] -utrwalić wartości w nazwanym [kontekstu] [ context] zmiennej nowsze dostępu.
  * [Śledzenia](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) -dodaje ciąg do [inspektora interfejsu API](api-management-howto-api-inspector.md) danych wyjściowych.
  * [Poczekaj](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) -czeka na zamkniętym wysyłania żądania Get wartość z pamięci podręcznej, lub kontroli przepływu zasady do wykonania przed kontynuowaniem.
* [Zasady uwierzytelniania][Authentication policies]
  * [Uwierzytelnianie za pomocą Basic] [ Authenticate with Basic] -uwierzytelniania za pomocą usługi wewnętrznej bazy danych przy użyciu uwierzytelniania podstawowego.
  * [Uwierzytelniania za pomocą certyfikatu klienta] [ Authenticate with client certificate] -uwierzytelniania za pomocą usługi wewnętrznej bazy danych przy użyciu certyfikatów klienta.
* [Buforowanie zasad][Caching policies] 
  * [Pobierz z pamięci podręcznej] [ Get from cache] -wykonaj pamięci podręcznej wyszukiwania i zwracać prawidłową odpowiedź buforowana, jeśli jest dostępna.
  * [Magazynu pamięci podręcznej] [ Store to cache] -buforuje odpowiedzi zgodnie z określonym pamięci podręcznej konfiguracji kontroli.
  * [Pobiera wartość z pamięci podręcznej](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) -pobrania elementu pamięci podręcznej według klucza.
  * [Przechowywana wartość w pamięci podręcznej](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) -przechowywania elementu w pamięci podręcznej według klucza.
  * [Usuń wartość z pamięci podręcznej](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) -usunięcie elementu w pamięci podręcznej według klucza.
* [Krzyżowe zasady domeny][Cross domain policies] 
  * [Zezwalaj na połączenia między domenami] [ Allow cross-domain calls] — może ułatwić interfejsu API programu Adobe Flash i Microsoft Silverlight bazujące na przeglądarce klientów.
  * [CORS] [ CORS] -dodaje współużytkowanie zasobów między źródłami (CORS) obsługi operacji lub interfejsu API w celu zapewnienia obsługi wywołań między domenami od klientów przeglądarki do udostępniania.
  * [JSONP] [ JSONP] -dodaje JSON z obsługą dopełnienie (JSONP) do operacji lub interfejsu API w celu zapewnienia obsługi wywołań między domenami od klientów przeglądarki JavaScript.
* [Zasad przekształcania][Transformation policies] 
  * [Konwertuj JSON do pliku XML] [ Convert JSON to XML] — konwertuje żądania lub odpowiedzi body z formatu JSON do pliku XML.
  * [Konwertuj do formatu JSON XML] [ Convert XML to JSON] — konwertuje żądania lub odpowiedzi body z pliku XML do formatu JSON.
  * [Znajdowanie i zamienianie ciągów w treści] [ Find and replace string in body] — znajduje podciąg żądania lub odpowiedzi i zastępuje go inny podciąg.
  * [Maski adresów URL w zawartości] [ Mask URLs in content] -ponownie zapisuje (maski) łącza w odpowiedzi body tak, aby wskazywać równoważne połączenie za pośrednictwem bramy.
  * [Ustaw usługę zaplecza] [ Set backend service] — zmienia usługi wewnętrznej bazy danych dla żądania przychodzącego.
  * [Ustaw treści] [ Set body] -ustawia treść komunikatu dla żądań przychodzących i wychodzących.
  * [Set — nagłówek HTTP] [ Set HTTP header] — przypisuje wartość do istniejących odpowiedzi i/lub nagłówek żądania lub dodaje nowy nagłówek odpowiedzi i/lub żądania.
  * [Wartość parametru ciągu zapytania] [ Set query string parameter] — dodaje, zastępuje wartość lub usuwa parametru ciągu zapytania żądania.
  * [Ponowne zapisywanie adresów URL] [ Rewrite URL] — konwertuje adresie URL żądania w postaci publicznego do formularza oczekiwane przez usługę sieci web.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na wyrażenia zasad Zobacz poniższe wideo.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Access restriction policies]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Check HTTP header]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Limit call rate by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Restrict caller IPs]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Set usage quota by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Validate JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[context]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Forward request]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Log to Event Hub]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Authentication policies]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Authenticate with Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Authenticate with client certificate]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Get from cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Store to cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross domain policies]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Allow cross-domain calls]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformation policies]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Convert JSON to XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Convert XML to JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Find and replace string in body]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Mask URLs in content]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Set backend service]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Set body]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Set HTTP header]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Set query string parameter]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Rewrite URL]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Policies in API Management]: api-management-howto-policies.md
[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx


