---
title: "Udostępnianie widoków niestandardowych usługi Azure Time Series Insights za pomocą sparametryzowanych adresów URL | Microsoft Docs"
description: "W tym artykule opisano sposób opracowywania sparametryzowanych adresów URL w usłudze Azure Time Series Insights na potrzeby łatwego udostępniania widoku klienta."
services: time-series-insights
ms.service: time-series-insights
author: MarkMcGeeAtAquent
ms.author: MarkMcGeeAtAquent
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: rest-api
ms.topic: get-started-article
ms.workload: big-data
ms.date: 11/21/2017
ms.openlocfilehash: ac48969a9166080384dccf606f0401a82016a60a
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2017
---
# <a name="share-a-custom-view-using-a-parameterized-url"></a>Udostępnianie widoku niestandardowego przy użyciu sparametryzowanego adresu URL

Aby udostępnić widok niestandardowy w eksploratorze usługi Time Series Insights, można programowo utworzyć sparametryzowany adres URL widoku niestandardowego.

Eksplorator usługi Time Series Insights obsługuje parametry zapytań URL, za pomocą których możesz określić widoki w środowisku bezpośrednio z adresu URL.  Na przykład przy użyciu tylko adresu URL możesz określić środowisko docelowe, predykat wyszukiwania i żądany przedział czasu. Gdy użytkownik kliknie dostosowany adres URL, interfejs udostępni link bezpośrednio do tego zasobu w portalu usługi Time Series Insights.  Obowiązują zasady dostępu do danych. 

## <a name="environment-id"></a>Identyfikator środowiska

Parametr `environmentId=<guid>` określa identyfikator środowiska docelowego.  Jest to składnik nazwy FQDN dostępu do danych, który można znaleźć w prawym górnym rogu przeglądu środowiska w witrynie Azure Portal.  Jest to cała część poprzedzająca ciąg `env.timeseries.azure.com`. Przykładowy parametr identyfikatora środowiska to `?environmentId=10000000-0000-0000-0000-100000000108`.

## <a name="time"></a>Time

Za pomocą sparametryzowanego adresu URL możesz określać bezwzględne lub względne wartości czasu.

### <a name="absolute-time-values"></a>Bezwzględne wartości czasu

Aby podać bezwzględne wartości czasu, użyj parametrów `from=<integer>` i `to=<integer>`. 

Parametr `from=<integer>` to wartość w milisekundach JavaScript określająca czas początkowy dla zakresu wyszukiwania.

Parametr `to=<integer>` to wartość w milisekundach JavaScript określająca czas końcowy dla zakresu wyszukiwania. 

Aby zidentyfikować liczbę milisekund JavaScript dla określonej daty, zobacz [Epoch & Unix Timestamp Converter (Konwerter znaczników czasu systemu Unix i epoki)](https://www.freeformatter.com/epoch-timestamp-to-date-converter.html).

### <a name="relative-time-values"></a>Względne wartości czasu

W przypadku względnej wartości czasu użyj parametru `relativeMillis=<value>`, gdzie *value* to liczba milisekund JavaScript określająca wiek danych (licząc od najnowszych) w wewnętrznej bazie danych.

Na przykład parametr `&relativeMillis=3600000` spowoduje wyświetlenie danych z ostatnich 60 minut.

Akceptowane wartości odpowiadają wartościom w **szybkim menu czasu** eksploratora usługi Time Series Insights, w tym:

- 1800000 (ostatnie 30 minut)
- 3600000 (ostatnie 60 minut)
- 10800000 (ostatnie 3 godziny)
- 21600000 (ostatnie 6 godzin)
- 43200000 (ostatnie 12 godzin)
- 86400000 (ostatnie 24 godziny)
- 604800000 (ostatnie 7 dni)
- 2592000000 (ostatnie 30 godzin)

### <a name="optional-parameters"></a>Parametry opcjonalne

Parametr `timeSeriesDefinitions=<collection of term objects>` określa terminy widoku usługi Time Series Insights, gdzie:

- `name=<string>`
  - Nazwa *terminu*.
- `splitBy=<string>`
  - Nazwa kolumny, według której ma zostać wykonany *podział*.
- `measureName=<string>`
  - Nazwa kolumny *miary*.
- `predicate=<string>`
  - Klauzula *where* na potrzeby filtrowania po stronie serwera.

 
### <a name="examples"></a>Przykłady

Aby na przykład dodać definicje szeregów czasowych, możesz użyć następującej wartości:

```https
&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},
{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

Przy użyciu tych przykładowych definicji szeregów czasowych dla następujących elementów: 

- identyfikator środowiska
- ostatnie 60 minut danych
- terminy (F1PressureID, F2TempStation i F3VibrationPL) składające się na parametry opcjonalne
 
możesz utworzyć następujący sparametryzowany adres URL dla widoku:

```https
https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

Jeśli widok opisany przez poprzedzający adres URL zostałby utworzony w eksploratorze usługi Time Series Insights, wyglądałby następująco:

![Terminy eksploratora usługi Time Series Insights](media/parameterized-url/url1.png)

Pełny widok (w tym wykres) wyglądałby następująco:

![Widok wykresu](media/parameterized-url/url2.png)

## <a name="next-steps"></a>Następne kroki
[Wykonywanie zapytań o dane przy użyciu języka C#](time-series-insights-query-data-csharp.md)
