---
title: Azure ściągawka języka zapytań usługi Analiza dzienników | Dokumentacja firmy Microsoft
description: Ten artykuł zawiera pomoc na przechodzenia do nowego języka zapytań dla analizy dzienników, jeśli już znasz starszej wersji języka.
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2017
ms.author: bwren
ms.openlocfilehash: 9c487ab33859ae453a0074ef0344f61de19c7b4d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="transitioning-to-azure-log-analytics-new-query-language"></a>Przejście do nowego języka zapytań usługi Analiza dzienników Azure
Analiza dzienników zaimplementowana ostatnio nowy język kwerendy.  Ten artykuł zawiera pomoc na przechodzenie ten język dla analizy dzienników, jeśli znasz już starszej wersji języka i nadal potrzebujesz pomocy.

## <a name="resources"></a>Zasoby


## <a name="language-converter"></a>Konwerter języka

Jeśli znasz starszych język zapytań usługi Analiza dzienników, najprostszym sposobem tworzenia tego samego zapytania w nowym języku jest użyć konwertera języka, który jest zainstalowany w portalu wyszukiwania dziennika po przekonwertowaniu obszaru roboczego.  Za pomocą konwertera jest tak proste, jak wpisanie starszych zapytanie w polu tekstowym górny, a następnie klikając pozycję **przekonwertować**.  Można albo kliknij przycisk Wyszukaj, aby uruchamiać zapytania lub kopiowania i wklej go, aby użyć go w innym miejscu.

![Konwerter języka](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="resources"></a>Zasoby
[Język zapytań usługi analiza dziennika witryny dokumentacji](https://docs.loganalytics.io) zawiera wszystkie zasoby muszą pochodzić się wszystkiego o nowym języku.  W tym samouczki, przykłady i dokumentację języka ukończone.


## <a name="cheat-sheet"></a>Ściągawka

Poniższa tabela zawiera porównanie różnych typowych kwerend do równoważnych poleceń między język kwerendy nowych i starszych Analiza dzienników Azure.

| Opis | Starsza wersja | nowy |
|:--|:--|:--|
| Wszystkie tabele wyszukiwania      | błąd | Wyszukaj "error" (bez rozróżniania wielkości liter) |
| Wybierz dane z tabeli | Typ zdarzenia = |  Zdarzenie |
|                        | Typ zdarzenia = &#124; wybierz źródło dziennika zdarzeń, identyfikator zdarzenia | Zdarzenia &#124; projektu źródło dziennika zdarzeń, identyfikator zdarzenia |
|                        | Typ zdarzenia = &#124; pierwszych 100 | Zdarzenia &#124; zająć 100 |
| Porównanie ciągów      | Type=Event Computer=srv01.contoso.com   | Zdarzenia &#124; gdzie komputera == "srv01.contoso.com" |
|                        | Type=Event Computer=contains("contoso") | Zdarzenia &#124; gdzie komputer zawiera "contoso" (bez rozróżniania wielkości liter)<br>Zdarzenia &#124; gdzie contains_cs komputera "Contoso" (z uwzględnieniem wielkości liter) |
|                        | Type=Event Computer=RegEx("@contoso@")  | Zdarzenia &#124; gdzie komputer zgodny z wyrażeniem regularnym ". *contoso*" |
| Porównanie dat        | Typ zdarzenia TimeGenerated = > 1DAYS teraz | Zdarzenia &#124; gdzie TimeGenerated > ago(1d) |
|                        | Typ zdarzenia TimeGenerated = > TimeGenerated 2017-05-01 < 2017-05-31 | Zdarzenia &#124; gdzie TimeGenerated między (datetime(2017-05-01)... DATETIME(2017-05-31)) |
| Wartość logiczna porównania     | Type=Heartbeat IsGatewayInstalled=false  | Puls \| gdzie IsGatewayInstalled == false |
| Sortuj                   | Typ zdarzenia = &#124; sortowania asc komputera, desc dziennika zdarzeń, EventLevelName asc | Zdarzenie \| sortowania asc komputera, desc dziennika zdarzeń, EventLevelName asc |
| Różne               | Typ zdarzenia = &#124; deduplikacji komputera \| wybierz komputer | Zdarzenia &#124; podsumowywania przez komputer, w dzienniku zdarzeń |
| Rozszerzenie kolumny         | Typ = CounterName wydajności = "% czasu procesora" &#124; if(map(CounterValue,0,50,0,1),"HIGH","LOW") ROZSZERZ jako wykorzystania | Wydajności &#124; w przypadku, gdy CounterName == "% czasu procesora" \| rozszerzyć wykorzystania = Jeśli ("Od" równowartości > 50, "HIGH") |
| Agregacja            | Typ zdarzenia = &#124; miar count() jako liczba w przeliczeniu na komputer | Event &#124; summarize Count = count() by Computer |
|                                | Typ = wydajności ObjectName = CounterName procesora = "% czasu procesora" &#124; miar avg(CounterValue) interwał komputera 5 minut | Wydajności &#124; gdzie ObjectName == "Procesor" i CounterName == "% czasu procesora" &#124; Podsumuj avg(CounterValue) przez komputer, bin (TimeGenerated, 5 minut) |
| Agregacja limit | Typ zdarzenia = &#124; miar count() przez komputer &#124; 10 pierwszych | Event &#124; summarize AggregatedValue = count() by Computer &#124; limit 10 |
| Unii                  | Typ = zdarzeń lub typ = Syslog | union Event, Syslog |
| Dołączanie                   | Typ = NetworkMonitoring &#124; join AgentIP wewnętrzny (typ = pulsu) ComputerIP | NetworkMonitoring &#124; join rodzaj = wewnętrzny (wyszukiwania typu == "Pulsu") na $left. AgentIP == $right.ComputerIP |



## <a name="next-steps"></a>Kolejne kroki
- Zapoznaj się z [Samouczek o pisaniu zapytań](https://go.microsoft.com/fwlink/?linkid=856078) przy użyciu nowego języka zapytań.
- Zapoznaj się [Query Language Reference](https://go.microsoft.com/fwlink/?linkid=856079) szczegółowe informacje dotyczące polecenia, Operatorzy i funkcje dla nowego języka zapytań.  
