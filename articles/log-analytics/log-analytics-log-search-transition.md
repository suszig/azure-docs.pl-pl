---
title: "Azure ściągawka języka zapytań usługi Analiza dzienników | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera pomoc na przechodzenia do nowego języka zapytań dla analizy dzienników, jeśli już znasz starszej wersji języka."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/06/2017
ms.author: bwren
ms.openlocfilehash: 38cb11befe844178572981abb29fe5439286dbc1
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2017
---
# <a name="transitioning-to-azure-log-analytics-new-query-language"></a>Przejście do nowego języka zapytań usługi Analiza dzienników Azure
Analiza dzienników zaimplementowana ostatnio nowy język kwerendy.  Ten artykuł zawiera pomoc na przechodzenie ten język dla analizy dzienników, jeśli znasz już starszej wersji języka i nadal potrzebujesz pomocy.

## <a name="language-converter"></a>Konwerter języka

Jeśli znasz starszych język zapytań usługi Analiza dzienników, najprostszym sposobem tworzenia tego samego zapytania w nowym języku jest użyć konwertera języka, który jest zainstalowany w portalu wyszukiwania dziennika po przekonwertowaniu obszaru roboczego.  Za pomocą konwertera jest tak proste, jak wpisanie starszych zapytanie w polu tekstowym górny, a następnie klikając pozycję **przekonwertować**.  Można albo kliknij przycisk Wyszukaj, aby uruchamiać zapytania lub kopiowania i wklej go, aby użyć go w innym miejscu.

![Konwerter języka](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="cheat-sheet"></a>Ściągawka

Poniższa tabela zawiera porównanie różnych typowych kwerend do równoważnych poleceń między język kwerendy nowych i starszych Analiza dzienników Azure.

| Opis | Starsza wersja | Nowy |
|:--|:--|:--|
| Wszystkie tabele wyszukiwania      | error | Wyszukaj "error" (bez rozróżniania wielkości liter) |
| Wybierz dane z tabeli | Typ zdarzenia = |  Wydarzenie |
|                        | Typ = zdarzeń &#124; Wybierz źródło dziennika zdarzeń, identyfikator zdarzenia | Zdarzenie &#124; Źródło dziennika zdarzeń, EventID projektu |
|                        | Typ = zdarzeń &#124; 100 najpopularniejszych | Zdarzenie &#124; podejmij 100 |
| Porównanie ciągów      | Typ = Computer=srv01.contoso.com zdarzeń   | Zdarzenie &#124; gdy komputer == "srv01.contoso.com" |
|                        | Typ = Computer=contains("contoso") zdarzeń | Zdarzenie &#124; gdy komputer zawiera "contoso" (bez rozróżniania wielkości liter)<br>Zdarzenie &#124; gdy komputer contains_cs "Contoso" (z uwzględnieniem wielkości liter) |
|                        | Typ = komputer zdarzenia = wyrażenie regularne ("@contoso@")  | Zdarzenie &#124; gdy komputer zgodny z wyrażeniem regularnym ". *contoso*" |
| Porównanie dat        | Typ zdarzenia TimeGenerated = > 1DAYS teraz | Zdarzenie &#124; gdzie TimeGenerated > ago(1d) |
|                        | Typ zdarzenia TimeGenerated = > TimeGenerated 2017-05-01 < 2017-05-31 | Zdarzenie &#124; gdzie TimeGenerated między (datetime(2017-05-01)... DATETIME(2017-05-31)) |
| Wartość logiczna porównania     | Typ = IsGatewayInstalled pulsu = false  | Puls \| gdzie IsGatewayInstalled == false |
| Sortuj                   | Typ = zdarzeń &#124; Sortowanie asc komputera, desc dziennika zdarzeń, EventLevelName asc | Zdarzenie \| Sortuj według komputera asc, desc dziennika zdarzeń, EventLevelName asc |
| Różne               | Typ = zdarzeń &#124; Funkcja deduplikacji komputera \| Wybierz komputer | Zdarzenie &#124; Podsumuj według komputera, dziennika zdarzeń |
| Rozszerzenie kolumny         | Typ = CounterName wydajności = "% czasu procesora" &#124; ROZSZERZ if(map(CounterValue,0,50,0,1),"HIGH","LOW") jako wykorzystania | Wydajności &#124; w przypadku, gdy CounterName == "% czasu procesora" \| Rozszerzanie wykorzystania = Jeśli ("Od" równowartości > 50, "HIGH") |
| Agregacji            | Typ = zdarzeń &#124; Miara count() jako liczność według komputera | Zdarzenie &#124; Podsumuj Count = count() przez komputer |
|                                | Typ = wydajności ObjectName = CounterName procesora = "% czasu procesora" &#124; Miara avg(CounterValue) interwał komputera 5 minut | Wydajności &#124; Gdzie ObjectName == "Procesor" i CounterName == "% czasu procesora" &#124; Podsumuj avg(CounterValue) przez komputer, bin (TimeGenerated, 5 minut) |
| Agregacja limit | Typ = zdarzeń &#124; Miara count() przez komputer &#124; 10 pierwszych | Zdarzenie &#124; Podsumuj AggregatedValue = count() przez komputer &#124; limit 10 |
| Unii                  | Typ = zdarzeń lub typ = Syslog | Unia zdarzenia dziennika systemowego |
| Join                   | Typ = NetworkMonitoring &#124; Dołącz AgentIP wewnętrzny (typ = pulsu) ComputerIP | NetworkMonitoring &#124; Dołącz rodzaj = wewnętrzny (wyszukiwania typu == "Pulsu") na $left. AgentIP == $right.ComputerIP |



## <a name="next-steps"></a>Następne kroki
- Zapoznaj się z [Samouczek o pisaniu zapytań](https://go.microsoft.com/fwlink/?linkid=856078) przy użyciu nowego języka zapytań.
- Zapoznaj się [Query Language Reference](https://go.microsoft.com/fwlink/?linkid=856079) szczegółowe informacje dotyczące polecenia, Operatorzy i funkcje dla nowego języka zapytań.  
