---
title: "Śledzenia wywołań z interfejsu API inspektora - Azure API Management | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak śledzić wywołania Inspektor interfejsu API w usłudze Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 4b222327-c8a4-4f33-9a06-adff2a9834d9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: a9d4d3be7f046af975f6dc25670070204848588c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>Sposób użycia interfejsu API inspektora śledzenia wywołań w usłudze Azure API Management
Zarządzanie interfejsami API zawiera narzędzie inspektora interfejsu API, które zapewniają pomoc podczas debugowania i rozwiązywania problemów z interfejsów API. Kontroler interfejsu API można programowo i można również bezpośrednio z portalu dla deweloperów. 

Oprócz operacji śledzenia śledzi także inspektora interfejsu API [wyrażenie zasad](https://msdn.microsoft.com/library/azure/dn910913.aspx) oceny. Aby demonstracyjne, zobacz [177 epizodu obejmują chmury: więcej funkcji interfejsu API zarządzania](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) i szybkie przewijanie do przodu do 21:00.

Ten przewodnik zawiera omówienie Inspektor interfejsu API.

> [!NOTE]
> Ślady inspektora interfejsu API tylko były generowane i dostępne dla żądań zawierających klucze subskrypcji, które należą do [administratora](api-management-howto-create-groups.md) konta.
> 
> 

## <a name="trace-call"></a> Użyj interfejsu API inspektora śledzenie wywołania
Aby używać interfejsu API inspektora, Dodaj **ocp apim śledzenia: true** żądania nagłówka do wywołania operacji, a następnie Pobierz i sprawdzić śledzenia przy użyciu adresu URL, wskazane przez **ocp-apim śledzenia lokalizacji** nagłówka odpowiedzi. Można wykonać programowo i można również wykonać bezpośrednio z portalu dla deweloperów.

Ten samouczek przedstawia sposób użycia inspektora interfejsu API przy użyciu interfejsu API podstawowe Kalkulator, skonfigurowanego w operacji śledzenia [pierwszy interfejs API zarządzania](api-management-get-started.md) Wprowadzenie — samouczek. Nie ukończono ten samouczek zajmuje tylko kilka minut zaimportować podstawowy interfejs API Kalkulator, czy można użyć innego interfejsu API Wybieranie np. interfejsu API Echo. Każde wystąpienie usługi API Management ma wstępnie skonfigurowany interfejs Echo API, którego można używać do eksperymentów oraz poznawania usługi API Management. Interfejs API Echo zwraca wstecz niezależnie od danych wejściowych jest wysyłane do niego. Aby go użyć, może wywołać żadnych zlecenie HTTP, a wartość zwracana jest zostały wysłane. 

Aby rozpocząć, kliknij przycisk **portalu dla deweloperów** w portalu Azure usługi Zarządzanie interfejsami API. Operacje można wywołać bezpośrednio z portalu dla deweloperów, które oferują wygodny sposób, aby wyświetlić i przetestować operacje interfejsu API.

> Jeśli jeszcze nie utworzono wystąpienie usługi API Management, zobacz [Utwórz wystąpienie usługi Zarządzanie interfejsami API] [ Create an API Management service instance] w [wprowadzenie do usługi Azure API Management] [ Get started with Azure API Management] samouczka.
> 
> 

![Zarządzanie interfejsami API portalu dla deweloperów][api-management-developer-portal-menu]

Kliknij przycisk **interfejsów API** z menu u góry, a następnie kliknij przycisk **podstawowe Kalkulator**.

![Interfejs Echo API][api-management-api]

Kliknij przycisk **wypróbuj** próby **Dodaj dwie liczb całkowitych** operacji.

![Wypróbuj][api-management-open-console]

Zachowaj ustawienie domyślne wartości parametrów, a następnie wybierz subskrypcję klucz produktu ma być używany z **klucza subskrypcji** listy rozwijanej.

Domyślnie w portalu dla deweloperów **Ocp Apim śledzenia** nagłówka są już ustawione na **true**. Ten nagłówek Określa, czy śledzenie jest generowany.

![Send][api-management-http-get]

Kliknij przycisk **wysyłania** do wywołania operacji.

![Send][api-management-send-results]

W odpowiedzi będzie nagłówki **ocp-apim śledzenia lokalizacji** o wartości podobnie do poniższego przykładu.

```
ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742
```

Śledzenie można pobrać z określonej lokalizacji i sprawdzić, jak pokazano w następnym kroku. Zauważ, że są przechowywane tylko ostatnich wpisów dziennika 100 i lokalizacje dziennika są ponownie używane w obrotu. Dlatego w przypadku wywołań więcej niż 100 z włączonym śledzeniem po pewnym czasie rozpoczęcia zastępowanie pierwsze dane śledzenia w miejscu.

## <a name="inspect-trace"></a>Sprawdź dane śledzenia
Aby zapoznać się z wartości w śledzeniu, Pobierz plik śledzenia z **ocp-apim śledzenia lokalizacji** adresu URL. Jest to plik tekstowy w formacie JSON i zawiera wpisy podobne do poniższego przykładu.

```json
{
    "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
    "traceEntries": {
        "inbound": [
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0725926",
                "data": {
                    "request": {
                        "method": "GET",
                        "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                        "headers": [
                            {
                                "name": "Ocp-Apim-Subscription-Key",
                                "value": "5d7c41af64a44a68a2ea46580d271a59"
                            },
                            {
                                "name": "Connection",
                                "value": "Keep-Alive"
                            },
                            {
                                "name": "Host",
                                "value": "contoso5.azure-api.net"
                            }
                        ]
                    }
                }
            },
            {
                "source": "mapper",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0726213",
                "data": {
                    "configuration": {
                        "api": {
                            "from": "/calc",
                            "to": {
                                "scheme": "http",
                                "host": "calcapi.cloudapp.net",
                                "port": 80,
                                "path": "/api",
                                "queryString": "",
                                "query": {},
                                "isDefaultPort": true
                            }
                        },
                        "operation": {
                            "method": "GET",
                            "uriTemplate": "/add?a={a}&b={b}"
                        },
                        "user": {
                            "id": 1,
                            "groups": [
                                "Administrators",
                                "Developers"
                            ]
                        },
                        "product": {
                            "id": 1
                        }
                    }
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0727522",
                "data": {
                    "message": "Request is being forwarded to the backend service.",
                    "request": {
                        "method": "GET",
                        "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                        "headers": [
                            {
                                "name": "Ocp-Apim-Subscription-Key",
                                "value": "5d7c41af64a44a68a2ea46580d271a59"
                            },
                            {
                                "name": "X-Forwarded-For",
                                "value": "33.52.215.35"
                            }
                        ]
                    }
                }
            }
        ],
        "outbound": [
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1960601",
                "data": {
                    "response": {
                        "status": {
                            "code": 200,
                            "reason": "OK"
                        },
                        "headers": [
                            {
                                "name": "Pragma",
                                "value": "no-cache"
                            },
                            {
                                "name": "Content-Length",
                                "value": "124"
                            },
                            {
                                "name": "Cache-Control",
                                "value": "no-cache"
                            },
                            {
                                "name": "Content-Type",
                                "value": "application/xml; charset=utf-8"
                            },
                            {
                                "name": "Date",
                                "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                            },
                            {
                                "name": "Expires",
                                "value": "-1"
                            },
                            {
                                "name": "Server",
                                "value": "Microsoft-IIS/8.5"
                            },
                            {
                                "name": "X-AspNet-Version",
                                "value": "4.0.30319"
                            },
                            {
                                "name": "X-Powered-By",
                                "value": "ASP.NET"
                            }
                        ]
                    }
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1961112",
                "data": {
                    "message": "Response headers have been sent to the caller. Starting to stream the response body."
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1963155",
                "data": {
                    "message": "Response body streaming to the caller is complete."
                }
            }
        ]
    }
}
```

## <a name="next-steps"> </a>Następne kroki
* Obejrzyj pokaz śledzenia wyrażenie zasad w [177 epizodu obejmują chmury: więcej funkcji zarządzania interfejsu API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Szybko przewinąć do 21:00, aby wyświetlić pokaz.

> [!VIDEO https://channel9.msdn.com/Shows/Cloud+Cover/Episode-177-More-API-Management-Features-with-Vlad-Vinogradsky/player]
> 
> 

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




