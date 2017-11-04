---
title: "moreLikeThis w usłudze Azure Search (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Wstępne dokumentacji dla funkcji moreLikeThis (wersja zapoznawcza) w interfejsu API REST usługi Azure Search."
services: search
documentationCenter: na
authors: mhko
manager: jlembicz
editor: na
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: nateko
ms.openlocfilehash: 20f6a1166d47799f15275cb5b6a9ea4934ae57e2
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="morelikethis-in-azure-search-preview"></a>moreLikeThis w usłudze Azure Search (wersja zapoznawcza)

`moreLikeThis=[key]`to parametr zapytania w [interfejsu API Search](https://docs.microsoft.com/rest/api/searchservice/search-documents). Określając `moreLikeThis` parametru w kwerendzie wyszukiwania, możesz znaleźć dokumenty, które są podobne do dokumentu, określony przez klucz dokumentu. Po wysłaniu żądania wyszukiwania z `moreLikeThis`, zapytanie jest generowany z warunkami wyszukiwania wyodrębnione z danego dokumentu, najlepiej opisujące tego dokumentu. Wygenerowane zapytanie jest następnie używany do przesyłania żądania wyszukiwania. Domyślnie zawartość wszystkich `searchable` pola są traktowane jako, chyba że `searchFields` parametr jest używany do ograniczania pola. `moreLikeThis` Parametru nie można używać z parametrem wyszukiwania `search=[string]`.

## <a name="examples"></a>Przykłady 

Poniżej znajduje się przykład moreLikeThis zapytania. Zapytanie znajdzie dokumentów, których opis pola są najbardziej zbliżone do pola dokumentu źródłowego określony przez `moreLikeThis` parametru.

```  
Get /indexes/hotels/docs?moreLikeThis=1002&searchFields=description&api-version=2016-09-01-Preview
```  

```  
POST /indexes/hotels/docs/search?api-version=2016-09-01-Preview
    {  
      "moreLikeThis": "1002",  
      "searchFields": "description"  
    }  
```  

## <a name="feature-availability"></a>Dostępność funkcji

Funkcja moreLikeThis jest obecnie w wersji zapoznawczej i obsługiwane tylko w wersji interfejsu api w wersji zapoznawczej, `2015-02-28-Preview` i `2016-09-01-Preview`. Ponieważ w żądaniu jest określona wersja interfejsu API, jest możliwe łączenie ogólnie dostępna (GA) i Podgląd interfejsów API w tej samej aplikacji. Jednak Podgląd interfejsów API nie są w ramach umowy dotyczącej poziomu usług i funkcji mogą ulec zmianie, dlatego nie zaleca się stosowanie ich w aplikacjach produkcyjnych.