---
title: "Tworzenie pętli i zakresy lub debatch danych w przepływach pracy - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Tworzenie pętli do iterowania po danych, akcje grupy do zakresów, lub debatch dane w celu uruchomienia jeden przepływ pracy w aplikacjach logiki platformy Azure."
services: logic-apps
documentationcenter: .net,nodejs,java
author: ecfan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2016
ms.author: estfan; LADocs
ms.openlocfilehash: 24a19ef7d93b4b0d006dfb0aed38f88d3d821d99
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="logic-apps-loops-scopes-and-debatching"></a>Pętle, zakresy i usuwanie partii aplikacji logiki
  
Logic Apps oferuje następujące sposoby pracy z tablic, kolekcje, partie i wykonuje pętlę w przepływie pracy.
  
## <a name="foreach-loop-and-arrays"></a>ForEach — pętla i tablic
  
Logic Apps umożliwia pętli zestawu danych i wykonywania akcji dla każdego elementu.  Zapętlenie przez kolekcję jest możliwe za pośrednictwem `foreach` akcji.  W projektancie, można dodać dla każdej pętli.  Po wybraniu tablicy, która ma zostać wykonana iteracja, możesz rozpocząć dodawanie akcji.  Można dodać wiele akcji na pętli foreach.  Raz w pętli, można rozpocząć Określ, co ma mieć miejsce w każdej wartości w tablicy.

  W tym przykładzie wysyła wiadomość e-mail dla każdego adresu e-mail, który zawiera "microsoft.com". Jeśli używasz widoku kodu, można określić dla każdej pętli, jak w następującym przykładzie:

``` json
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')"
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                },
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                },
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  A `foreach` akcji można Iterowanie przez tablice z tysiącami jednostek.  Domyślnie iteracji wykonywane równolegle.  Zobacz [limity i konfiguracji](logic-apps-limits-and-config.md) szczegółowe informacje na temat limitów tablicy i współbieżności.

### <a name="sequential-foreach-loops"></a>Sekwencyjne pętli ForEach

Aby włączyć pętli foreach można wykonać kolejno `Sequential` opcji operacji powinny zostać dodane.

``` json
"forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "operationOptions": "Sequential",
        "..."
}
```
  
## <a name="until-loop"></a>Do pętli
  
  Do momentu spełnienia warunku można wykonać akcji lub serii akcji.  Najbardziej typowym scenariuszem stosowania aż pętli jest wywołaniem punktu końcowego, do momentu uzyskania odpowiedzi, którego szukasz.  W projektancie, można określić, aby dodać do pętli.  Po dodaniu Akcje wewnątrz pętli, można ustawić warunku exit, a także pętli limity.
  
  W tym przykładzie wywołuje punkt końcowy HTTP, dopóki treść odpowiedzi ma wartość "Ukończona".  Po ukończeniu albo: 
  
  * Odpowiedź HTTP ma stan "Ukończona"
  * Zostały zainstalowane na godzinę
  * Wystąpiło sprzężenie 100 razy
  
  Jeśli używasz widoku kodu, można określić do pętli, jak w następującym przykładzie:
  
  ``` json
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed')",
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn i debatching

Czasami może zostać wyświetlony wyzwalacz tablicę elementów, które mają być debatch i uruchomić przepływ pracy dla każdego elementu.  Ta debatching można zrealizować przy użyciu `spliton` polecenia.  Domyślnie, jeśli programu swagger wyzwalacza określa ładunku, który jest tablicą `spliton` został dodany. `spliton` Polecenie uruchamia Uruchom dla każdego elementu w tablicy.  SplitOn można dodać tylko do wyzwalania, którą można ręcznie skonfigurować lub zastąpione. Nie może mieć `spliton` i również implementują wzorzec synchronicznej odpowiedzi.  Każdy przepływ pracy, który wywołuje ma `response` akcji oprócz `spliton` asynchronicznie uruchamia i wysyła natychmiastowego `202 Accepted` odpowiedzi.  

  W tym przykładzie odbiera tablicę elementów i debatches w każdym wierszu. W widoku kodu można określić SplitOn jak w poniższym przykładzie:

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequencey": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Zakresy

Istnieje możliwość grupowania serii akcji ze sobą przy użyciu zakresu.  Zakresy są przydatne w przypadku implementowania obsługi wyjątków.  W Projektancie można dodać nowego zakresu i rozpocząć dodawanie wszystkie akcje wewnątrz niej.  Zakresy można zdefiniować w widoku kodu, jak w następującym przykładzie:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
