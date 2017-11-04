---
title: "Definiuje przepływy pracy z JSON - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Jak napisać definicji przepływu pracy w formacie JSON dla usługi logic apps"
author: jeffhollan
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
ms.date: 03/29/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 7f9e5a10066df8a464c285273e77a85c0d562ebb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-workflow-definitions-for-logic-apps-using-json"></a>Tworzenie definicji przepływu pracy dla usługi logic apps za pomocą formatu JSON

Można utworzyć definicji przepływu pracy dla [Azure Logic Apps](logic-apps-what-are-logic-apps.md) prosty, deklaratywny języka JSON. Jeśli nie jest jeszcze, najpierw należy przejrzeć [tworzenie pierwszej aplikacji logiki z projektanta aplikacji logiki](logic-apps-create-a-logic-app.md). Zobacz też [pełne odwołania dla języka definicji przepływu pracy](http://aka.ms/logicappsdocs).

## <a name="repeat-steps-over-a-list"></a>Powtórz kroki od listy

Aby wykonać iterację tablicę, która zawiera elementy do 10 000 i wykonania czynności dla każdego elementu, użyj [typu foreach](logic-apps-loops-and-scopes.md).

## <a name="handle-failures-if-something-goes-wrong"></a>Obsługa błędów, jeśli jakaś nieprawidłowość

Zwykle, które chcesz dołączyć *krok korygowania* — niektóre logikę, która wykonuje *tylko wtedy, gdy* co najmniej jednego wywołania zakończyć się niepowodzeniem. W tym przykładzie pobiera dane z różnych miejsc, ale w przypadku niepowodzenia wywołania chcemy, aby wysłać wiadomość gdzieś, dlatego firma Microsoft może śledzić awarii później:  

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      }
    },
    "postToErrorMessageQueue": {
      "type": "ApiConnection",
      "inputs": "...",
      "runAfter": {
        "readData": [
          "Failed"
        ]
      }
    }
  },
  "outputs": {}
}
```

Aby określić, że `postToErrorMessageQueue` działa tylko `readData` ma `Failed`, użyj `runAfter` właściwości, na przykład, aby określić listę możliwych wartości, tak aby `runAfter` może być `["Succeeded", "Failed"]`.

Na koniec, ponieważ w tym przykładzie obsługuje teraz błąd, firma Microsoft nie jest już oznaczyć Uruchom jako `Failed`. Ponieważ dodaliśmy kroku obsługi tego błędu w tym przykładzie ma `Succeeded` chociaż jeden krok `Failed`.

## <a name="execute-two-or-more-steps-in-parallel"></a>Wykonaj kroki co najmniej dwa równolegle

Do uruchomienia wielu akcji równolegle, `runAfter` właściwości musi być taka sama w czasie wykonywania. 

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "triggers": {
    "Request": {
      "kind": "http",
      "type": "Request"
    }
  },
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      }
    },
    "branch1": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "branch2": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

W tym przykładzie zarówno `branch1` i `branch2` są ustawione na uruchamianie `readData`. W związku z tym obu gałęziach są uruchamiane równolegle. Znacznik czasu dla obu gałęziach są identyczne.

![Równoległe](media/logic-apps-author-definitions/parallel.png)

## <a name="join-two-parallel-branches"></a>Dołącz do dwóch równoległych gałęziach

Możesz także dołączyć do dwóch akcje, które są ustawione na uruchamianie równoległe przez dodanie elementów do `runAfter` właściwości co w poprzednim przykładzie.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {}
    },
    "branch1": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "branch2": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "join": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "branch1": [
          "Succeeded"
        ],
        "branch2": [
          "Succeeded"
        ]
      }
    }
  },
  "parameters": {},
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "Http",
      "inputs": {
        "schema": {}
      }
    }
  },
  "contentVersion": "1.0.0.0",
  "outputs": {}
}
```

![Równoległe](media/logic-apps-author-definitions/join.png)

## <a name="map-list-items-to-a-different-configuration"></a>Mapa elementów listy do innej konfiguracji

Następnie Załóżmy, że chcemy uzyskać różne zawartości na podstawie wartości właściwości. Można utworzyć mapy wartości do miejsc docelowych jako parametr:  

```
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

W takim przypadku najpierw pobrać listę artykułów. W oparciu o kategorię, która została zdefiniowana jako parametru, drugi etap używa mapy do wyszukania adres URL pobierania zawartości.

Sytuacje, w tym miejscu należy pamiętać: 

*   [ `intersection()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) Funkcja sprawdza, czy kategoria zgodny jedną znane zdefiniowanych kategoriach.

*   Po uzyskujemy kategorii, możemy pobierać elementu z tablicy przy użyciu nawiasy kwadratowe:`parameters[...]`

## <a name="process-strings"></a>Ciągi procesu

Można użyć różnych funkcji do manipulowania ciągami. Na przykład załóżmy, że mamy ciąg, który chcemy przekazać do systemu, ale nie wątpliwości właściwe obsługę kodowania znaków. Jedną z opcji jest w formacie base64 ten ciąg do zakodowania. Jednak aby uniknąć anulowanie w adresie URL, zamierzamy Zastąp kilku znaków. 

Również chcemy podciąg nazwy zamówienia, ponieważ nie są używane przez pięć pierwszych znaków.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1",
        "orderer": "NAME=Contoso"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
      }
    }
  },
  "outputs": {}
}
```

Praca z wewnątrz do poza:

1. Pobierz [ `length()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) o nazwę osoby zamawiającej, dlatego firma Microsoft wrócić całkowita liczba znaków.

2. Odejmowanie 5, ponieważ chcemy krótszego ciągu.

3. W rzeczywistości zająć [ `substring()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring). Rozpoczniemy pod indeksem `5` i przejdź do końca ciągu.

4. Konwertuj to podciąg do [ `base64()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) ciąg.

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)wszystkie `+` znaków i zawierają `-` znaków.

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)wszystkie `/` znaków i zawierają `_` znaków.

## <a name="work-with-date-times"></a>Praca z daty i godziny

Daty i godziny mogą być użyteczne, zwłaszcza w przypadku próby pobierania danych ze źródła danych, który nie obsługuje naturalnie *wyzwalaczy*. Umożliwia także daty i godziny do znajdowania, jak długo różne kroki są tworzone.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1"
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

W tym przykładzie mamy wyodrębnić `startTime` z poprzedniego kroku. Następnie możemy pobrać bieżącego czasu i odjąć 1 sekundy:

[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) 

Inne jednostki czasu, można używać tak samo, jak `minutes` lub `hours`. Na koniec mamy Porównaj te dwie wartości. Jeśli pierwsza wartość jest mniejsza niż wartość drugiej, a następnie więcej niż jednej sekundy są spełnione, ponieważ najpierw umieszczono kolejności.

Do formatowania daty, możemy użyć ciągu elementy formatujące. Na przykład, aby uzyskać RFC1123, używamy [ `utcnow('r')` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). Aby uzyskać informacje dotyczące formatowania daty, zobacz [język definicji przepływu pracy](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).

## <a name="deployment-parameters-for-different-environments"></a>Parametry wdrożenia dla różnych środowisk

Zazwyczaj cyklami życia wdrożenia ma środowiska programowania, środowisku przemieszczania i środowiska produkcyjnego. Na przykład może użyć tej samej definicji w tych środowiskach, ale Użyj różnych baz danych. Podobnie możesz użyć tej samej definicji w różnych regionach, wysokiej dostępności, ale mają każde wystąpienie aplikacji logiki, aby komunikował się z bazą danych tego regionu.
W tym scenariuszu różni się od podejmowania parametrów w *środowiska uruchomieniowego* gdzie zamiast tego należy używać `trigger()` działać tak jak w poprzednim przykładzie.

Można uruchomić z podstawową definicję, tak jak ten przykład:

```
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

W rzeczywistym `PUT` żądania dla aplikacji logiki możesz podać parametr `uri`. Ponieważ istnieje już wartość domyślną, ładunku aplikacji logiki wymaga tego parametru:

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
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

W każdym środowisku można podać inną wartość dla `connection` parametru. 

Aby uzyskać wszystkie opcje, które mają do tworzenia i zarządzania aplikacji logiki, zobacz [dokumentacja interfejsu API REST](https://msdn.microsoft.com/library/azure/mt643787.aspx). 
