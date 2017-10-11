---
title: "Wyrażeń regularnych w analizy dzienników OMS dziennika wyszukiwania | Dokumentacja firmy Microsoft"
description: "Można użyć słowa kluczowego wyrażenia regularnego podczas wyszukiwania dziennika analizy dzienników do filtru wyników na podstawie wyrażenia regularnego.  Ten artykuł zawiera składnię dla tych wyrażeń z kilka przykładów."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: bwren
ms.openlocfilehash: 9746170f157ed5065adc953a31687ff18bd73708
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="using-regular-expressions-to-filter-log-searches-in-log-analytics"></a>Za pomocą wyrażeń regularnych do filtrowania dziennika przeszukuje analizy dzienników

[Zaloguj się wyszukiwanie](log-analytics-log-searches.md) umożliwiają wyodrębnienia informacji z repozytorium analizy dzienników.  [Wyrażenia filtru](log-analytics-search-reference.md#filter-expressions) umożliwiają filtrowanie wyników wyszukiwania według określonych kryteriów.  **RegEx** — słowo kluczowe pozwala określić wyrażenie regularne dla tego filtru.  

Ten artykuł zawiera szczegółowe informacje o składni wyrażeń regularnych, używany przez analizy dzienników.

> [!NOTE]
> Wyrażenia regularnego można używać tylko z pól z możliwością wyszukiwania.  Aby uzyskać więcej informacji dotyczących pól z możliwością wyszukiwania, zobacz **typy pól** w [wyszukiwanie danych przy użyciu dziennika wyszukiwania w analizy dzienników](log-analytics-log-searches.md#use-additional-filters).


## <a name="regex-keyword"></a>RegEx — słowo kluczowe

Użyj następującej składni, aby użyć **RegEx** — słowo kluczowe w polu wyszukiwania dziennika.  Pozostałe sekcje w tym artykule można użyć składni wyrażenia regularnego.

    field:Regex("Regular Expression")
    field=Regex("Regular Expression")

Na przykład użycie wyrażenia regularnego do zwrócenia alertu rekordy z typem *ostrzeżenie* lub *błąd*, należy użyć następujących wyszukiwania dziennika.

    Type=Alert AlertSeverity=RegEx("Warning|Error")

## <a name="partial-matches"></a>Wyniki pasujące częściowo
Należy pamiętać, że wyrażenie regularne musi odpowiadać właściwości cały tekst.  Wyniki pasujące częściowo nie zwróci żadnych rekordów.  Na przykład, jeśli próbujesz zwraca rekordów z komputera o nazwie srv01.contoso.com, czy następujące wyszukiwania dziennika **nie** zwraca żadnych rekordów.

    Computer=RegEx("srv..")

Jest to spowodowane pierwsza część nazwy odpowiada wyrażeniu regularnemu.  Następujące dwa dziennika operacji wyszukiwania zwróci rekordy z tego komputera, ponieważ spełniają całą nazwę.

    Computer=RegEx("srv..@")
    Computer=RegEx("srv...contoso.com")

## <a name="characters"></a>Znaki
Określ inną znaków.

| Znak | Opis | Przykład | Przykładowe dopasowań |
|:--|:--|:--|:--|
| A | Jedno wystąpienie znaku. | Computer=RegEx("SRV01.contoso.com") | SRV01.contoso.com |
| . | Dowolny pojedynczy znak. | Computer=RegEx("SRV...contoso.com") | SRV01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| ? | Wystąpienie zero lub jeden znak. | Komputer = wyrażenie regularne ("Srw01?. "contoso.com") | srv0.contoso.com<br>SRV01.contoso.com |
| * | Zero lub więcej wystąpień znaku. | Computer=RegEx("SRV01*.contoso.com") | srv0.contoso.com<br>SRV01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| + | Jedno lub więcej wystąpień znaku. | Computer=RegEx("SRV01+.contoso.com") | SRV01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| [*abc*] | Dopasowuje dowolny pojedynczy znak w nawiasach | Computer=RegEx("srv0[123].contoso.com") | SRV01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| [**-*z*] | Odpowiada pojedynczy znak z zakresu.  Może zawierać wiele zakresów. | Computer=RegEx("srv0[1-3].contoso.com") | SRV01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| [^*abc*] | Brak znaków w nawiasach | Computer=RegEx("srv0[^123].contoso.com") | srv05.contoso.com<br>SRV06.contoso.com<br>srv07.contoso.com |
| [^**-*z*] | Brak znaków w zakresie. | Computer=RegEx("srv0[^1-3].contoso.com") | srv05.contoso.com<br>SRV06.contoso.com<br>srv07.contoso.com |
| [*n*-*m*] | Zgodne zakres znaków liczbowych. | Computer=RegEx("SRV[01-03].contoso.com") | SRV01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| @ | Dowolny ciąg znaków. | Komputer = wyrażenie regularne ("srv@.contoso.com") | SRV01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |


## <a name="multiple-occurences-of-character"></a>Wiele wystąpień znaku
Określ wiele wystąpień określonych znaków.

| Znak | Opis | Przykład | Przykładowe dopasowań |
|:--|:--|:--|:--|
| {n} |  *n*wystąpienia znak. | Computer=RegEx("bW-win-sc01{3}.bwren.Lab") | bW Windows sc0111.bwren.lab |
| {n} |  *n*lub więcej wystąpień znaku. | Computer=RegEx("bW-win-sc01{3,}.bwren.Lab") | bW Windows sc0111.bwren.lab<br>bW Windows sc01111.bwren.lab<br>bW Windows sc011111.bwren.lab<br>bW Windows sc0111111.bwren.lab |
| {n, m} |  *n*Aby *m* wystąpień znaku. | Computer=RegEx("bW-win-sc01{3,5}.bwren.Lab") | bW Windows sc0111.bwren.lab<br>bW Windows sc01111.bwren.lab<br>bW Windows sc011111.bwren.lab |


## <a name="logical-expressions"></a>Wyrażenia logiczne
Wybierz z wieloma wartościami.

| Znak | Opis | Przykład | Przykładowe dopasowań |
|:--|:--|:--|:--|
| &#124; | Operatora logicznego OR.  Zwraca wynik, jeśli odpowiada na jedno z tych wyrażeń. | Typ alertu AlertSeverity = = wyrażenie regularne ("ostrzeżenie &#124; Błąd") | Ostrzeżenie<br>Błąd |
| & | Operatora logicznego AND.  Zwraca wynik, jeśli odpowiada na oba wyrażenia | EventData = wyrażenie regularne ("(zabezpieczeń.\* &. \*Powodzenie. \*)") | Inspekcji pomyślnego zabezpieczeń |


## <a name="literals"></a>Literały
Konwertuj znaki specjalne literał znaków.  Dotyczy to również znaki, które udostępnia funkcji wyrażeń regularnych, takich jak?-\*^\[\]{}\(\)+\|. &.

| Znak | Opis | Przykład | Przykładowe dopasowań |
|:--|:--|:--|:--|
| \\ | Konwertuje znak specjalny literału. | Status_CF =\\[błąd\\] @<br>Status_CF = błąd\\-@ | [Błąd] Nie można odnaleźć pliku.<br>Nie można odnaleźć pliku błędu. |


## <a name="next-steps"></a>Następne kroki

* Zapoznaj się z [dziennika wyszukiwania](log-analytics-log-searches.md) do wyświetlania i analizowania danych w repozytorium analizy dzienników.
