---
title: "Tworzenie zapytań względem indeksu usługi Azure Search | Microsoft Docs"
description: "Konstruowanie zapytania wyszukiwania w usłudze Azure Search oraz filtrowanie i sortowanie wyników wyszukiwania za pomocą parametrów wyszukiwania."
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 69205d7a-363f-4b92-a53f-6ca818a3d2c7
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 04/26/2017
ms.author: ashmaka
ms.openlocfilehash: 01be1b14e838c4f1b6f2498111fb8369c2bbb92a
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="query-your-azure-search-index"></a>Tworzenie zapytań względem indeksu usługi Azure Search
> [!div class="op_single_selector"]
> * [Omówienie](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

W ramach przesyłania żądań wyszukiwania do usługi Azure Search dostępnych jest szereg parametrów, które można określić, aby użyć ich razem z rzeczywistymi słowami wpisywanymi w polu wyszukiwania w aplikacji. Te parametry zapytań zapewniają lepszą kontrolę nad [wyszukiwaniem pełnotekstowym](search-lucene-query-architecture.md).

Poniższa lista zawiera krótkie opisy typowych zastosowań parametrów zapytań w usłudze Azure Search. Aby uzyskać pełne, szczegółowe informacje na temat parametrów zapytań i ich działania, zobacz strony dotyczące [interfejsu API REST](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) i [zestawu .NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters#microsoft_azure_search_models_searchparameters#properties_summary).

## <a name="types-of-queries"></a>Typy zapytań
Usługa Azure Search udostępnia wiele opcji tworzenia niezwykle wydajnych zapytań. Dwa najczęściej używane typy zapytań to `search` i `filter`. Zapytanie `search` umożliwia wyszukanie jednego lub większej liczby terminów we wszystkich polach *z możliwością wyszukiwania* w indeksie. Działa ono podobnie jak wyszukiwarki takie jak Google czy Bing. Zapytanie `filter` ocenia wyrażenie logiczne w odniesieniu do wszystkich pól *z możliwością filtrowania* w indeksie. W przeciwieństwie do zapytań `search` w zapytaniach `filter` jest uwzględniania pełna zawartość pól, co oznacza, że w przypadku pól ciągów jest ważna wielkości liter.

Wyszukiwań i filtrów można używać razem lub oddzielnie. W przypadku połączenia tych zapytań najpierw jest stosowany filtr do całego indeksu, a wyszukiwanie jest wykonywane na wynikach filtrowania. Z tego względu filtrowanie może być przydatne, jeśli chcemy poprawić wydajność zapytań, ponieważ pozwala ono zawęzić zestaw dokumentów przetwarzany przez zapytanie wyszukiwania.

Elementy składni wyrażeń filtrów należą do podzbioru [języka filtru OData](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search). W przypadku zapytań wyszukiwania można używać [składni uproszczonej](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) lub [składni zapytań Lucene](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search). Informacje na ten temat zostały podane poniżej.

### <a name="simple-query-syntax"></a>Prosta składnia zapytań
[Prosta składnia zapytań](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) jest domyślnym językiem zapytań używanym w usłudze Azure Search. Prosta składnia zapytań obsługuje szereg typowych operatorów wyszukiwania, na przykład AND, OR, NOT, a także frazy, sufiksy i pierwszeństwo operatorów.

### <a name="lucene-query-syntax"></a>Składnia zapytań Lucene
[Składnia zapytań Lucene](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) pozwala korzystać z popularnego i wszechstronnego języka zapytań biblioteki [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html).

Używanie tej składni pozwala łatwo korzystać z następujących funkcji: [zapytania należące do zakresu pola](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fields), [wyszukiwanie rozmyte](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fuzzy), [wyszukiwanie w sąsiedztwie](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_proximity), [promowanie terminów](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_termboost), [wyszukiwanie za pomocą wyrażeń regularnych](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_regex), [wyszukiwanie za pomocą symboli wieloznacznych](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_wildcard), [podstawy składni](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_syntax) oraz [zapytania korzystające z operatorów logicznych](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_boolean).

## <a name="ordering-results"></a>Porządkowanie wyników
Usługa Azure Search udostępnia możliwość przekazania wyników zapytania wyszukiwania uporządkowanych według wartości w określonym polu. Domyślnie usługa Azure Search porządkuje wyniki w oparciu o rangę wyniku wyszukiwania poszczególnych dokumentów, który jest określany na podstawie wagi [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).

Jeśli chcesz, aby usługa Azure Search zwracała wyniki uporządkowane według wartości innej niż wynik wyszukiwania, możesz użyć parametru wyszukiwania `orderby`. Można określić wartość parametru `orderby`, dołączając nazwy pól i wywołania funkcji [`geo.distance()`](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search) w przypadku wartości geoprzestrzennych. Po każdym wyrażeniu można umieścić element `asc` w celu wskazania, że wyniki mają być sortowane w kolejności rosnącej, lub element `desc`, jeśli wyniki mają być sortowane w kolejności malejącej. Domyślnie jest stosowana kolejność rosnąca.

## <a name="paging"></a>Stronicowanie
Usługa Azure Search ułatwia implementowanie stronicowania wyników wyszukiwania. Przy użyciu parametrów `top` i `skip` można sprawnie wysyłać żądania wyszukiwania, które umożliwiają uzyskanie całego zbioru wyników w postaci zarządzanych, uporządkowanych podzbiorów, co pozwala łatwo stosować dobre praktyki dotyczące obsługi interfejsu użytkownika wyszukiwania. Razem z mniejszymi podzbiorami wyników można również odbierać liczbę dokumentów w całym zbiorze wyników wyszukiwania.

Więcej informacji o stronicowaniu wyników wyszukiwania można znaleźć w artykule [How to page search results in Azure Search](search-pagination-page-layout.md) (Sposoby stronicowania wyników wyszukiwania w usłudze Azure Search).

## <a name="hit-highlighting"></a>Wyróżnianie trafień
Dostępne w usłudze Azure Search parametry `highlight`, `highlightPreTag` i `highlightPostTag` umożliwiają łatwe wyróżnianie tej części wyników, która dokładnie odpowiada zapytaniu wyszukiwania. Można wskazać, w których polach *z możliwością wyszukiwania* ma zostać wyróżniony dopasowany tekst, a także dokładnie określić tagi ciągów, które mają zostać dodane na początku i na końcu dopasowanego tekstu zwracanego przez usługę Azure Search.

## <a name="try-out-query-syntax"></a>Wypróbowywanie składni zapytania

Najlepszym sposobem na zrozumienie różnic w składni jest przesłanie zapytań i przejrzenie wyników.

+ Użyj [Eksploratora wyszukiwania](search-explorer.md) w witrynie Azure Portal. Jeśli wdrożysz [przykładowy indeks](search-get-started-portal.md), będziesz w stanie wykonywać zapytania o indeks w ciągu minut, używając narzędzi w portalu.

+ Użyj narzędzia Telerik Fiddler lub Chrome Postman do przesyłania zapytań do indeksu przekazanego do usługi wyszukiwania. Oba narzędzia obsługują wywołania REST do punktu końcowego HTTP. 