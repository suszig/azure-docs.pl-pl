---
title: "Tworzenie pętli i zakresy lub debatch danych w przepływach pracy - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Tworzenie pętli do iterowania po danych, akcje grupy do zakresów, lub debatch dane w celu uruchomienia jeden przepływ pracy w aplikacjach logiki platformy Azure."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: a17de187f67c075147ea8ff7f69434014eea3fdb
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="logic-apps-loops-scopes-and-debatching"></a>Pętle, zakresy i usuwanie partii aplikacji logiki
  
Logic Apps oferuje następujące sposoby pracy z tablic, kolekcje, partie i wykonuje pętlę w przepływie pracy.
  
## <a name="foreach-loop-and-arrays"></a>ForEach — pętla i tablic
  
Logic Apps umożliwia pętli zestawu danych i wykonywania akcji dla każdego elementu.  Jest to możliwe za pośrednictwem `foreach` akcji.  W projektancie, można określić, aby dodać dla każdej pętli.  Po wybraniu tablicy, która ma zostać wykonana iteracja, możesz rozpocząć dodawanie akcji.  Można dodać wiele akcji na pętli foreach.  Raz w pętli można rozpocząć Określ, co ma mieć miejsce w każdej wartości w tablicy.

Jeśli używasz widoku kodu, można określić dla każdej pętli, takie jak poniżej.  To jest przykład dla każdej pętli, który wysyła wiadomości e-mail dla każdego adresu e-mail, który zawiera "microsoft.com":

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
  
  A `foreach` akcję można wykonać iterację w stałych do 5000 wierszy.  Domyślnie każdej iteracji będą wykonywane równolegle.  

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
  
  Do momentu spełnienia warunku można wykonać akcji lub serii akcji.  Najbardziej typowym scenariuszem dla tego wywołuje punkt końcowy do momentu uzyskania odpowiedzi, którego szukasz.  W projektancie, można określić, aby dodać do pętli.  Po dodaniu Akcje wewnątrz pętli, można ustawić warunku exit, a także pętli limity.  Istnieje opóźnienie 1 minutę, między cykle pętli.
  
  Jeśli używasz widoku kodu, można określić aż pętli, takich jak poniżej.  Jest to przykład wywołaniem punktu końcowego HTTP, dopóki treść odpowiedzi ma wartość "Ukończona".  Proces zostanie zakończony, kiedy użytkownik 
  
  * Odpowiedź HTTP ma stan "Ukończona"
  * Maksymalnie 1 godziny
  * Wystąpiło sprzężenie 100 razy
  
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

Czasami może zostać wyświetlony wyzwalacz tablicę elementów, które mają być debatch i uruchomić przepływ pracy dla każdego elementu.  Można to zrobić za pomocą `spliton` polecenia.  Domyślnie, jeśli programu swagger wyzwalacza określa ładunku, który jest tablicą `spliton` zostanie dodany i uruchomić Uruchom dla każdego elementu.  SplitOn można dodać tylko do wyzwalacza.  Może to ręcznie skonfigurowane lub zastąpiony w definicji widoku kodu.  Obecnie można debatch SplitOn stałych elementów do 5000.  Nie może mieć `spliton` i również implementują wzorzec synchronicznej odpowiedzi.  Każdy przepływ pracy, który wywołuje ma `response` akcji oprócz `spliton` będą uruchamiane asynchronicznie i wysyłać natychmiastowego `202 Accepted` odpowiedzi.  

Można wybrać opcję SplitOn w widoku kodu poniższym przykładzie.  Odbiera tablicę elementów i debatches w każdym wierszu.

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

Istnieje możliwość grupowania serii akcji ze sobą przy użyciu zakresu.  Jest to szczególnie przydatne dla Implementowanie obsługi wyjątków.  W Projektancie można dodać nowego zakresu i rozpocząć dodawanie wszystkie akcje wewnątrz niej.  Zakresy można zdefiniować w widoku kodu podobne do poniższych:


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
