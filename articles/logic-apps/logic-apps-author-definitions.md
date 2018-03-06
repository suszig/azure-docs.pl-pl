---
title: Tworzenie w definicji aplikacji logiki JSON - Azure Logic Apps | Dokumentacja firmy Microsoft
description: "Dodaj parametry, przetworzyć ciągi tworzenie map parametru i Pobierz dane z funkcji daty"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 01/31/2018
ms.author: LADocs; estfan
ms.openlocfilehash: d05f7e34cbe670db6733c199e3420c810c304a84
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2018
---
# <a name="build-on-your-logic-app-definition-with-json"></a>Tworzenie na definicję aplikacji logiki z JSON

Aby wykonać więcej zaawansowane zadania związane z [Azure Logic Apps](../logic-apps/logic-apps-overview.md), można edytować definicję aplikacji logiki, który używa języka JSON prosty, deklaratywny widoku kodu. Jeśli nie jest jeszcze, najpierw należy przejrzeć [tworzenie pierwszej aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md). Zobacz też [pełne odwołania dla języka definicji przepływu pracy](http://aka.ms/logicappsdocs).

> [!NOTE]
> Niektóre możliwości usługi Azure Logic Apps, takich jak parametry, są dostępne tylko wtedy, gdy użytkownik pracuje w widoku kodu dla definicji aplikacji logiki. Parametry pozwalają na ponowne używanie wartości w całej aplikacji logiki. Na przykład jeśli chcesz użyć tego samego adresu e-mail przez kilka akcji, należy zdefiniować tego adresu e-mail jako parametr.

## <a name="view-and-edit-your-logic-app-definitions-in-json"></a>Umożliwia wyświetlanie i edytowanie użytkownika logiki aplikacji definicji w formacie JSON

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com "Azure Portal").

2. Z menu po lewej stronie wybierz **więcej usług**. W obszarze **Integracja w przedsiębiorstwie** wybierz pozycję **Logic Apps**. Wybierz aplikację logiki.

3. Z menu aplikacji logiki w obszarze **narzędzi programistycznych**, wybierz **widoku kodu aplikacji logiki**.

   Okno widoku kodu zostanie otwarty i zawiera definicję aplikacji logiki.

## <a name="parameters"></a>Parametry

Parametry umożliwia ponowne użycie wartości w całej aplikacji logiki i są odpowiednie do zastępowania wartości, które można zmienić często. Na przykład jeśli masz adres e-mail, która ma zostać użyta w wielu miejscach, należy zdefiniować tego adresu e-mail jako parametr. 

Parametry są także przydatne, gdy trzeba zastąpić parametry w różnych środowiskach, Dowiedz się więcej o [parametry w celu wdrożenia](#deployment-parameters) i [interfejsu API REST usługi Azure Logic Apps dokumentacji](https://docs.microsoft.com/rest/api/logic).

> [!NOTE]
> Parametry są dostępne tylko w widoku kodu.

W [pierwszej aplikacji logiki przykład](../logic-apps/quickstart-create-first-logic-app-workflow.md), utworzyć przepływ pracy, który wysyła wiadomości e-mail, gdy pojawią się nowe ogłoszenia w kanału informacyjnego RSS witryny sieci Web. Adres URL źródła jest zapisane na stałe, dlatego w tym przykładzie pokazano, jak zastąpić wartość zapytania parametr, aby łatwiej można zmienić adres URL źródła.

1. W widoku kodu, Znajdź `parameters : {}` obiekt, a następnie dodaj `currentFeedUrl` obiektu:

   ``` json
     "currentFeedUrl" : {
      "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
   }
   ```

2. W `When_a_feed-item_is_published` akcji, Znajdź `queries` sekcji i Zastąp wartości zapytania z `"feedUrl": "#@{parameters('currentFeedUrl')}"`. 

   **Przed**
   ``` json
   }
      "queries": {
          "feedUrl": "https://s.ch9.ms/Feeds/RSS"
       }
   },   
   ```

   **Po**
   ``` json
   }
      "queries": {
          "feedUrl": "#@{parameters('currentFeedUrl')}"
       }
   },   
   ```

   Aby przyłączyć się co najmniej dwa ciągi, umożliwia także `concat` funkcji. 
   Na przykład `"@concat('#',parameters('currentFeedUrl'))"` działa tak samo, co w poprzednim przykładzie.

3.  Gdy wszystko będzie gotowe, wybierz pozycję **Zapisz**. 

Teraz możesz zmienić danych RSS witryny sieci Web przez przekazanie do innego adresu URL za pośrednictwem `currentFeedURL` obiektu.

<a name="deployment-parameters"></a>

## <a name="deployment-parameters-for-different-environments"></a>Parametry wdrożenia dla różnych środowisk

Zwykle cykle wdrożenia mają środowiska programowania, tymczasową i produkcyjną. Na przykład może użyć tej samej definicji aplikacji logiki w tych środowiskach, ale Użyj różnych baz danych. Podobnie możesz użyć tej samej definicji w różnych regionach, wysokiej dostępności, ale mają każde wystąpienie aplikacji logiki do używania bazy danych w tym regionie. 

> [!NOTE] 
> W tym scenariuszu różni się od podejmowania parametrów w *środowiska uruchomieniowego* gdzie należy używać `trigger()` zamiast tego działania.

Poniżej przedstawiono podstawowe definicji:

``` json
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "request": {
          "type": "request",
          "kind": "http"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```
W rzeczywistym `PUT` żądania dla aplikacji logiki możesz podać parametr `uri`. W każdym środowisku można podać inną wartość dla `connection` parametru. Ponieważ istnieje już wartość domyślną, ładunku aplikacji logiki wymaga tego parametru:

``` json
{
    "properties": {},
        "definition": {
          /// Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

Aby dowiedzieć się więcej, zobacz [interfejsu API REST usługi Azure Logic Apps dokumentacji](https://docs.microsoft.com/rest/api/logic/).

## <a name="process-strings-with-functions"></a>Funkcje ciągów procesu

Logic Apps ma funkcje służące do pracy z ciągami znaków. Na przykład załóżmy, że chcesz przekazać nazwę firmy z zamówienia do innego systemu. Jednak nie wiadomo o obsługę właściwe dla kodowania znaków. Można wykonać kodowania base64 na ciąg, aby uniknąć specjalne w adresie URL, można jednak zastąpić kilku znaków zamiast tego. Ponadto wystarczy podciąg nazwy firmy ponieważ pięć pierwszych znaków nie są używane. 

``` json
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1",
        "companyName": "NAME=Contoso"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "request": {
      "type": "Request",
      "kind": "Http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').companyName,5,sub(length(parameters('order').companyName), 5) )),'+','-') ,'/' ,'_' )}"
      }
    }
  },
  "outputs": {}
}
```

Te kroki opisano, jak w tym przykładzie przetwarza tych parametrów, Praca z wewnątrz na zewnątrz:

``` 
"uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').companyName,5,sub(length(parameters('order').companyName), 5) )),'+','-') ,'/' ,'_' )}"
```

1. Pobierz [ `length()` ](../logic-apps/logic-apps-workflow-definition-language.md) o nazwę firmy, dlatego możesz uzyskać całkowita liczba znaków.

2. Aby uzyskać krótszego ciągu, odejmuje wartość `5`.

3. Pobierz teraz [ `substring()` ](../logic-apps/logic-apps-workflow-definition-language.md). Rozpocznij od indeksu `5`, a następnie przejdź do końca ciągu.

4. Konwertuj to podciąg do [ `base64()` ](../logic-apps/logic-apps-workflow-definition-language.md) ciąg.

5. Teraz [ `replace()` ](../logic-apps/logic-apps-workflow-definition-language.md) wszystkie `+` znaków i zawierają `-` znaków.

6. Na koniec [ `replace()` ](../logic-apps/logic-apps-workflow-definition-language.md) wszystkie `/` znaków i zawierają `_` znaków.

## <a name="map-list-items-to-property-values-then-use-maps-as-parameters"></a>Mapy do wartości właściwości elementów listy, a następnie użyj mapy jako parametry

Aby uzyskać różne wyniki na podstawie wartości właściwości, można utworzyć mapę odpowiadającą każdej wartości właściwości wyniku, a następnie użyć tej mapy jako parametr. 

Na przykład ten przepływ pracy definiuje kilka kategorii jako parametry i mapowanie zgodny z tych kategorii z określonym adresem URL. Po pierwsze przepływ pracy pobiera listę artykułów. Następnie przepływ pracy używa mapy adres URL dopasowanie kategorii dla każdego artykułu.

*   [ `intersection()` ](../logic-apps/logic-apps-workflow-definition-language.md) Funkcja sprawdza, czy kategoria zgodny znanej kategorii zdefiniowane.

*   Po otrzymaniu pasującą kategorię, przykładzie ściąga elementu z tablicy przy użyciu nawiasy kwadratowe: `parameters[...]`

``` json
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "specialCategories": {
      "defaultValue": [
        "science",
        "google",
        "microsoft",
        "robots",
        "NSA"
      ],
      "type": "Array"
    },
    "destinationMap": {
      "defaultValue": {
        "science": "http://www.nasa.gov",
        "microsoft": "https://www.microsoft.com/en-us/default.aspx",
        "google": "https://www.google.com",
        "robots": "https://en.wikipedia.org/wiki/Robot",
        "NSA": "https://www.nsa.gov/"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "http"
    }
  },
  "actions": {
    "getArticles": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
      }
    },
    "forEachArticle": {
      "type": "foreach",
      "foreach": "@body('getArticles').responseData.feed.entries",
      "actions": {
        "ifGreater": {
          "type": "if",
          "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)",
          "actions": {
            "getSpecialPage": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
              }
            }
          }
        }
      },
      "runAfter": {
        "getArticles": [
          "Succeeded"
        ]
      }
    }
  }
}
```

## <a name="get-data-with-date-functions"></a>Pobierz dane z funkcji daty

Można pobrać danych ze źródła danych, która nie obsługuje natywnie *wyzwalaczy*, można użyć daty funkcje do pracy z godziny i daty zamiast tego. Na przykład, wyrażenie znajduje, jak długo kroki tego przepływu pracy są tworzone, Praca z wewnątrz na zewnątrz:

``` json
"expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
```

1. Z `order` akcji, Wyodrębnij `startTime`. 
2. Pobierz bieżący czas z `utcNow()`.
3. Odejmowanie jednej sekundy:

   [`addseconds(..., -1)`](../logic-apps/logic-apps-workflow-definition-language.md) 

   Inne jednostki czasu, można używać tak samo, jak `minutes` lub `hours`. 

3. Teraz możesz porównać te dwie wartości. 

   Jeśli pierwsza wartość jest mniejsza niż wartość drugiej, a następnie więcej niż jednej sekundy są spełnione, ponieważ najpierw umieszczono kolejności.

Do formatowania daty, można użyć ciągu elementy formatujące. Na przykład, aby uzyskać RFC1123, należy użyć [ `utcnow('r')` ](../logic-apps/logic-apps-workflow-definition-language.md). Dowiedz się więcej o [Data formatowania](../logic-apps/logic-apps-workflow-definition-language.md).

``` json
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder-id"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{parameters('order').id}"
      }
    },
    "ifTimingWarning": {
      "type": "If",
      "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
      "actions": {
        "timingWarning": {
          "type": "Http",
          "inputs": {
            "method": "GET",
            "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
          }
        }
      },
      "runAfter": {
        "order": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```


## <a name="next-steps"></a>Kolejne kroki

* [Wykonanie kroków na podstawie warunku (warunkowe instrukcje)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Wykonanie kroków na podstawie różnych wartości (instrukcji switch)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Uruchom i powtórz kroki (pętle)](../logic-apps/logic-apps-control-flow-loops.md)
* [Uruchom lub scalania czynności równoległe (gałęzi)](../logic-apps/logic-apps-control-flow-branches.md)
* [Wykonanie kroków na podstawie stanu akcji grupowanych (zakresy)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
* Dowiedz się więcej o [schematu język definicji przepływu pracy dla usługi Azure Logic Apps](../logic-apps/logic-apps-workflow-definition-language.md)
* Dowiedz się więcej o [działania przepływu pracy i Wyzwalacze dla usługi Azure Logic Apps](../logic-apps/logic-apps-workflow-actions-triggers.md)