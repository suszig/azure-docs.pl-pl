---
title: "Wyrażeń regularnych w Azure Log Analytics dziennika wyszukiwania | Dokumentacja firmy Microsoft"
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
ms.date: 01/18/2018
ms.author: bwren
ms.openlocfilehash: 8915e0e35951871ff10fd84453d55bd5102e97df
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
---
# <a name="using-regular-expressions-to-filter-log-searches-in-log-analytics"></a>Za pomocą wyrażeń regularnych do filtrowania dziennika przeszukuje analizy dzienników

>[!NOTE]
> W tym artykule opisano wyrażeń regularnych w analizy dzienników przy użyciu języka zapytań starszej wersji.  Jeśli obszaru roboczego został uaktualniony do [języka zapytań nowe analizy dzienników](log-analytics-log-search-upgrade.md), a następnie należy zapoznać się [wyrażeń regularnych w dokumentacji języka](https://docs.loganalytics.io/docs/Language-Reference/References/Regular-Expressions-syntax).


[Zaloguj się wyszukiwanie](log-analytics-log-searches.md) umożliwiają wyodrębnienia informacji z obszaru roboczego analizy dzienników.  [Wyrażenia filtru](log-analytics-search-reference.md#filter-expressions) umożliwiają filtrowanie wyników wyszukiwania według określonych kryteriów.  **RegEx** — słowo kluczowe pozwala określić wyrażenie regularne dla tego filtru.  

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
| a | Jedno wystąpienie znaku. | Computer=RegEx("srv01.contoso.com") | srv01.contoso.com |
| . | Dowolny pojedynczy znak. | Computer=RegEx("srv...contoso.com") | srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| a? | Wystąpienie zero lub jeden znak. | Computer=RegEx("srv01?.contoso.com") | srv0.contoso.com<br>srv01.contoso.com |
| a* | Zero lub więcej wystąpień znaku. | Computer=RegEx("srv01*.contoso.com") | srv0.contoso.com<br>srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| a+ | Jedno lub więcej wystąpień znaku. | Computer=RegEx("srv01+.contoso.com") | srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| [*abc*] | Dopasowuje dowolny pojedynczy znak w nawiasach | Computer=RegEx("srv0[123].contoso.com") | srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| [*a*-*z*] | Odpowiada pojedynczy znak z zakresu.  Może zawierać wiele zakresów. | Computer=RegEx("srv0[1-3].contoso.com") | srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| [^*abc*] | Brak znaków w nawiasach | Computer=RegEx("srv0[^123].contoso.com") | srv05.contoso.com<br>srv06.contoso.com<br>srv07.contoso.com |
| [^*a*-*z*] | Brak znaków w zakresie. | Computer=RegEx("srv0[^1-3].contoso.com") | srv05.contoso.com<br>srv06.contoso.com<br>srv07.contoso.com |
| [*n*-*m*] | Zgodne zakres znaków liczbowych. | Computer=RegEx("srv[01-03].contoso.com") | srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| @ | Dowolny ciąg znaków. | Computer=RegEx("srv@.contoso.com") | srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |


## <a name="multiple-occurences-of-character"></a>Wiele wystąpień znaku
Określ wiele wystąpień określonych znaków.

| Znak | Opis | Przykład | Przykładowe dopasowań |
|:--|:--|:--|:--|
| a{n} |  *n*wystąpienia znak. | Computer=RegEx("bw-win-sc01{3}.bwren.lab") | bw-win-sc0111.bwren.lab |
| a{n,} |  *n*lub więcej wystąpień znaku. | Computer=RegEx("bw-win-sc01{3,}.bwren.lab") | bw-win-sc0111.bwren.lab<br>bw-win-sc01111.bwren.lab<br>bw-win-sc011111.bwren.lab<br>bw-win-sc0111111.bwren.lab |
| a{n,m} |  *n*Aby *m* wystąpień znaku. | Computer=RegEx("bw-win-sc01{3,5}.bwren.lab") | bw-win-sc0111.bwren.lab<br>bw-win-sc01111.bwren.lab<br>bw-win-sc011111.bwren.lab |


## <a name="logical-expressions"></a>Wyrażenia logiczne
Wybierz z wieloma wartościami.

| Znak | Opis | Przykład | Przykładowe dopasowań |
|:--|:--|:--|:--|
| &#124; | Operatora logicznego OR.  Zwraca wynik, jeśli odpowiada na jedno z tych wyrażeń. | Type=Alert AlertSeverity=RegEx("Warning&#124;Error") | Ostrzeżenie<br>Błąd |
| & | Operatora logicznego AND.  Zwraca wynik, jeśli odpowiada na oba wyrażenia | EventData=regex("(Security.\*&.\*success.\*)") | Inspekcji pomyślnego zabezpieczeń |


## <a name="literals"></a>Literały
Konwertuj znaki specjalne literał znaków.  Dotyczy to również znaki, które udostępnia funkcji wyrażeń regularnych, takich jak?-\*^\[\]{}\(\)+\|. &.

| Znak | Opis | Przykład | Przykładowe dopasowań |
|:--|:--|:--|:--|
| \\ | Konwertuje znak specjalny literału. | Status_CF =\\[błąd\\] @<br>Status_CF = błąd\\-@ | [Błąd] Nie można odnaleźć pliku.<br>Nie można odnaleźć pliku błędu. |


## <a name="next-steps"></a>Następne kroki

* Zapoznaj się z [dziennika wyszukiwania](log-analytics-log-searches.md) do wyświetlania i analizowania danych w obszarze roboczym analizy dzienników.
