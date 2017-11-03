---
title: "Kafelku odwołanie Widok projektanta w OMS Log Analytics | Dokumentacja firmy Microsoft"
description: "Widok projektanta w analizy dzienników umożliwia tworzenie niestandardowych widoków w konsoli OMS, która zawiera różne wizualizacje danych w repozytorium OMS. Ten artykuł zawiera odwołanie do ustawienia dla każdego z dostępnych do użycia w niestandardowych widoków Kafelki."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 41787c8f-6c13-4520-b0d3-5d3d84fcf142
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 2bce5c63b4c6edd3753f1b234cc4f493dcf53dea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="log-analytics-view-designer-tile-reference"></a>Odwołanie kafelka Projektant widoków analizy dziennika
Projektant widoków w analizy dzienników umożliwia tworzenie niestandardowych widoków w konsoli OMS, która zawiera różne wizualizacje danych w repozytorium OMS. Ten artykuł zawiera odwołanie do ustawienia dla każdego z dostępnych do użycia w niestandardowych widoków Kafelki.

Inne artykuły, które są dostępne dla projektanta widoku są następujące:

* [Wyświetl projektanta](log-analytics-view-designer.md) — Omówienie projektanta widoków i procedur tworzenia i edytowania widoków niestandardowych.
* [Odwołanie do części wizualizacji](log-analytics-view-designer-parts.md) — odwołanie do ustawienia dla każdego z dostępnych do użycia w niestandardowych widoków Kafelki.

>[!NOTE]
> Jeśli obszaru roboczego został uaktualniony do [języka zapytań nowe analizy dzienników](log-analytics-log-search-upgrade.md), a następnie zapytania we wszystkich widokach musi być napisana w [nowy język kwerendy](https://go.microsoft.com/fwlink/?linkid=856078).  Wszystkie widoki, które zostały utworzone przed obszaru roboczego został uaktualniony będzie automtically przekonwertować.

W poniższej tabeli wymieniono różne typy dostępnych w Projektancie Widok kafelków.  Poniższe rozdziały zawierają opis każdego typu kafelka w szczegółów i ich właściwości.

| Kafelek | Opis |
|:--- |:--- |
| [Numer](#number-tile) |Numer pojedynczego przedstawiający liczby rekordów w wyniku zapytania. |
| [Dwóch liczb](#two-numbers-tile) |Wyświetlanie liczby rekordów z dwóch różnych zapytań dwóch liczb pojedynczego. |
| [Pierścień](#donut-tile) |Wykres pierścieniowy przedstawiający na podstawie zapytania o wartości podsumowań w Centrum. |
| [Wykres liniowy & objaśnienia](#line-chart-amp-callout-tile) |Wykres liniowy, na podstawie zapytania i objaśnienie z wartością podsumowania. |
| [Wykres liniowy](#line-chart-tile) |Wykres liniowy, na podstawie zapytania. |
| [Dwóch osiach czasu](#two-timelines-tile) |Wykres kolumnowy z dwóch serii, każda oparta na osobne zapytania. |

## <a name="number-tile"></a>Liczba kafelka
**Numer** kafelka zawiera jeden numer przedstawiający liczbę rekordów z zapytaniem dziennika i etykiety.

![Liczba kafelka](media/log-analytics-view-designer/tile-number.png)

| Ustawienie | Opis |
|:--- |:--- |
| Nazwa |Tekst wyświetlany w górnej części fragmentu. |
| Opis |Tekst wyświetlany w obszarze Nazwa kafelka. |
| **Kafelek** | |
| Legenda |Tekst wyświetlany w obszarze wartość. |
| Zapytanie |Zapytanie do uruchomienia.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| **Zaawansowane** |**> Weryfikacja przepływu danych** |
| Enabled (Włączony) |Wybierz, jeśli Weryfikacja przepływu danych powinno być włączone dla fragmentu.  Oferuje komunikat alternatywnego, jeśli dane nie są dostępne dla fragmentu.  Służy to zwykle podać komunikat okresie tymczasowego, gdy widok jest zainstalowany i danych jest dostępne. |
| Zapytanie |Wyślij zapytanie do uruchomienia, aby sprawdzić, czy dane są dostępne dla widoku.  Jeśli zapytanie nie przynosi efektów, zamiast wartości z główne zapytanie zostanie wyświetlony komunikat. |
| Komunikat |Komunikat wyświetlany, jeśli zapytanie weryfikacji przepływu danych zwraca żadnych danych.  Jeśli podasz żaden komunikat *wykonywania oceny* jest wyświetlany. |


## <a name="two-numbers-tile"></a>Kafelek dwóch liczb
**Dwa numer** kafelka Wyświetla dwóch liczb przedstawiający liczba rekordy z dwóch różnych dziennika zapytań i etykiety dla każdego.

![Kafelek dwóch liczb](media/log-analytics-view-designer/tile-two-numbers.png)

| Ustawienie | Opis |
|:--- |:--- |
| Nazwa |Tekst wyświetlany w górnej części fragmentu. |
| Opis |Tekst wyświetlany w obszarze Nazwa kafelka. |
| **Pierwszy Kafelek** | |
| Legenda |Tekst wyświetlany w obszarze wartość. |
| Zapytanie |Zapytanie do uruchomienia.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| **Drugi kafelka** | |
| Legenda |Tekst wyświetlany w obszarze wartość. |
| Zapytanie |Zapytanie do uruchomienia.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| **Zaawansowane** |**> Weryfikacja przepływu danych** |
| Enabled (Włączony) |Wybierz, jeśli Weryfikacja przepływu danych powinno być włączone dla fragmentu.  Oferuje komunikat alternatywnego, jeśli dane nie są dostępne dla fragmentu.  Służy to zwykle podać komunikat okresie tymczasowego, gdy widok jest zainstalowany i danych jest dostępne. |
| Zapytanie |Wyślij zapytanie do uruchomienia, aby sprawdzić, czy dane są dostępne dla widoku.  Jeśli zapytanie nie przynosi efektów, zamiast wartości z główne zapytanie zostanie wyświetlony komunikat. |
| Komunikat |Komunikat wyświetlany, jeśli zapytanie weryfikacji przepływu danych zwraca żadnych danych.  Jeśli podasz żaden komunikat *wykonywania oceny* jest wyświetlany. |


## <a name="donut-tile"></a>Pierścień kafelka
**Pierścień** kafelka Wyświetla jeden numer podsumowanie z kolumną wartości w zapytaniu dziennika.  Pierścień graficznie wyświetla wyniki top trzy rekordów.

![Pierścień kafelka](media/log-analytics-view-designer/tile-donut.png)

| Ustawienie | Opis |
|:--- |:--- |
| Nazwa |Tekst wyświetlany w górnej części fragmentu. |
| Opis |Tekst wyświetlany w obszarze Nazwa kafelka. |
| **Pierścień** | |
| Zapytanie |Wyślij zapytanie do uruchamiania dla pierścień.  Pierwszą właściwością powinna być wartość tekstową i drugą właściwością wartość liczbową.  Jest to zazwyczaj kwerendę, która używa **miary** — słowo kluczowe do podsumowania wyników. |
| **Pierścień** |**> Centrum** |
| Tekst |Tekst wyświetlany w obszarze wartość w pierścień. |
| Operacja |Operacja do wykonania na wartość właściwości Podsumowując pojedyncza wartość.<br><br>-Sumy: Dodaj wartości wszystkich rekordów z wartością właściwości.<br>— Procent: Procent równy określonemu procentowi wartości w rekordach o wartości właściwości w porównaniu z wartościami równy określonemu procentowi wszystkie rekordy. |
| Wynik wartości używana podczas operacji Centrum |Opcjonalnie kliknij znak plus, aby dodać jedną lub więcej wartości.  Wyniki zapytania będzie ograniczona do rekordów z wartościami właściwości, które określisz.  Jeśli wartości nie zostaną dodane, nie wszystkie rekordy są uwzględnione w zapytaniu. |
| **Pierścień** |**> Dodatkowe opcje** |
| Kolory |Kolor, który ma być wyświetlany dla każdej z trzech właściwości top.  Jeśli chcesz określić alternatywne kolory określone wartości właściwości, użyj zaawansowane mapowanie kolorów. |
| Mapowanie kolorów zaawansowane |Wyświetla kolor określone wartości właściwości.  Jeśli podaną wartość znajduje się w trzech top, alternatywny kolor zostanie wyświetlony standardowy koloru.  Jeśli właściwość nie ma trzy pierwsze, kolor nie jest wyświetlana. |
| **Zaawansowane** |**> Weryfikacja przepływu danych** |
| Enabled (Włączony) |Wybierz, jeśli Weryfikacja przepływu danych powinno być włączone dla fragmentu.  Oferuje komunikat alternatywnego, jeśli dane nie są dostępne dla fragmentu.  Służy to zwykle podać komunikat okresie tymczasowego, gdy widok jest zainstalowany i danych jest dostępne. |
| Zapytanie |Wyślij zapytanie do uruchomienia, aby sprawdzić, czy dane są dostępne dla widoku.  Jeśli zapytanie nie przynosi efektów, zamiast wartości z główne zapytanie zostanie wyświetlony komunikat. |
| Komunikat |Komunikat wyświetlany, jeśli zapytanie weryfikacji przepływu danych zwraca żadnych danych.  Jeśli podasz żaden komunikat *wykonywania oceny* jest wyświetlany. |


## <a name="line-chart-tile"></a>Kafelek wykresu wiersza
**Wykres liniowy** kafelka Wyświetla wykres liniowy z wielu serii w wyniku zapytania dziennika wraz z upływem czasu.  

![Wykres liniowy & objaśnienia kafelka](media/log-analytics-view-designer/tile-line-chart.png)

| Ustawienie | Opis |
|:--- |:--- |
| Nazwa |Tekst wyświetlany w górnej części fragmentu. |
| Opis |Tekst wyświetlany w obszarze Nazwa kafelka. |
| **Wykres liniowy** | |
| Zapytanie |Zapytania w celu uruchomienia wykresu liniowego.  Pierwszą właściwością powinna być wartość tekstową i drugą właściwością wartość liczbową.  Jest to zazwyczaj kwerendę, która używa **miary** — słowo kluczowe do podsumowania wyników.  Jeśli zapytanie używa **interwał** — słowo kluczowe następnie osi x wykresu użyje tego przedziału czasu.  Jeśli zapytanie nie obejmuje **interwał** interwałów — słowo kluczowe, a następnie co godzinę używanych dla osi x. |
| **Wykres liniowy** |**> Oś Y** |
| Użyj skali logarytmicznej |Zaznacz, aby użyć skali logarytmicznej dla osi y. |
| Jednostki |Określ jednostki dla wartości zwróconych przez kwerendę.  Te informacje jest używany do wyświetlania etykiet na wykresie wskazujący typy wartości i opcjonalnie do konwertowania wartości.  **Typ jednostki** określa kategorię jednostki i definiuje **bieżący typ jednostki** wartości, które są dostępne.  W przypadku wybrania wartości w **Konwertuj na** , a następnie są konwertowane wartości liczbowych **bieżącej jednostce** typ **przekonwertować** typu. |
| Etykiety niestandardowej |Tekst do wyświetlenia dla osi Y obok etykiety dla tego typu jednostki.  Jeśli etykieta nie jest określony, wyświetlana jest tylko typ jednostki. |
| **Zaawansowane** |**> Weryfikacja przepływu danych** |
| Enabled (Włączony) |Wybierz, jeśli Weryfikacja przepływu danych powinno być włączone dla fragmentu.  Oferuje komunikat alternatywnego, jeśli dane nie są dostępne dla fragmentu.  Służy to zwykle podać komunikat okresie tymczasowego, gdy widok jest zainstalowany i danych jest dostępne. |
| Zapytanie |Wyślij zapytanie do uruchomienia, aby sprawdzić, czy dane są dostępne dla widoku.  Jeśli zapytanie nie przynosi efektów, zamiast wartości z główne zapytanie zostanie wyświetlony komunikat. |
| Komunikat |Komunikat wyświetlany, jeśli zapytanie weryfikacji przepływu danych zwraca żadnych danych.  Jeśli podasz żaden komunikat *wykonywania oceny* jest wyświetlany. |


## <a name="line-chart--callout-tile"></a>Kafelek linii objaśnienia & wykresu
**Linii objaśnienia & wykresu** kafelka przedstawia wykres liniowy z wielu serii w wyniku zapytania dziennika przez godzinę i objaśnienie z podsumowaniem wartość.  

![Wykres liniowy & objaśnienia kafelka](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Ustawienie | Opis |
|:--- |:--- |
| Nazwa |Tekst wyświetlany w górnej części fragmentu. |
| Opis |Tekst wyświetlany w obszarze Nazwa kafelka. |
| **Wykres liniowy** | |
| Zapytanie |Zapytania w celu uruchomienia wykresu liniowego.  Pierwszą właściwością powinna być wartość tekstową i drugą właściwością wartość liczbową.  Jest to zazwyczaj kwerendę, która używa **miary** — słowo kluczowe do podsumowania wyników.  Jeśli zapytanie używa **interwał** — słowo kluczowe następnie osi x wykresu użyje tego przedziału czasu.  Jeśli zapytanie nie obejmuje **interwał** interwałów — słowo kluczowe, a następnie co godzinę używanych dla osi x. |
| **Wykres liniowy** |**> Objaśnienie** |
| Objaśnienie |Tytuł tekst do wyświetlenia powyżej wartości objaśnienia. |
| Nazwa serii |Wartość właściwości serii użyć wartości objaśnienia.  W przypadku serii nie są używane wszystkie rekordy z zapytania. |
| Operacja |Operacja do wykonania na wartość właściwości Podsumowując pojedyncza wartość objaśnienia.<br>— Średnia: Średnia wartość ze wszystkich rekordów.<br><br>-Liczba: Liczba wszystkich rekordów zwróconych przez kwerendę.<br>-Ostatniej próbki: Wartość od ostatniego interwału uwzględnione na wykresie.<br>-Max: Wartość maksymalna z interwałów uwzględnione na wykresie.<br>-Min: Wartość minimalna z interwałów uwzględnione na wykresie.<br>— Suma: Suma wartość ze wszystkich rekordów. |
| **Wykres liniowy** |**> Oś Y** |
| Użyj skali logarytmicznej |Zaznacz, aby użyć skali logarytmicznej dla osi y. |
| Jednostki |Określ jednostki dla wartości zwróconych przez kwerendę.  Te informacje jest używany do wyświetlania etykiet na wykresie wskazujący typy wartości i opcjonalnie do konwertowania wartości.  **Typ jednostki** określa kategorię jednostki i definiuje **bieżący typ jednostki** wartości, które są dostępne.  W przypadku wybrania wartości w **Konwertuj na** , a następnie są konwertowane wartości liczbowych **bieżącej jednostce** typ **przekonwertować** typu. |
| Etykiety niestandardowej |Tekst do wyświetlenia dla osi Y obok etykiety dla tego typu jednostki.  Jeśli etykieta nie jest określony, wyświetlana jest tylko typ jednostki. |
| **Zaawansowane** |**> Weryfikacja przepływu danych** |
| Enabled (Włączony) |Wybierz, jeśli Weryfikacja przepływu danych powinno być włączone dla fragmentu.  Oferuje komunikat alternatywnego, jeśli dane nie są dostępne dla fragmentu.  Służy to zwykle podać komunikat okresie tymczasowego, gdy widok jest zainstalowany i danych jest dostępne. |
| Zapytanie |Wyślij zapytanie do uruchomienia, aby sprawdzić, czy dane są dostępne dla widoku.  Jeśli zapytanie nie przynosi efektów, zamiast wartości z główne zapytanie zostanie wyświetlony komunikat. |
| Komunikat |Komunikat wyświetlany, jeśli zapytanie weryfikacji przepływu danych zwraca żadnych danych.  Jeśli podasz żaden komunikat *wykonywania oceny* jest wyświetlany. |


## <a name="two-timelines-tile"></a>Kafelek dwóch osiach czasu
**Dwóch osiach czasu** kafelka wyświetla wyniki dwa zapytania dziennika w czasie, gdy wykresy kolumnowe.  Objaśnienie jest wyświetlany dla każdej serii.  

![Kafelek dwóch osiach czasu](media/log-analytics-view-designer/tile-two-timelines.png)

| Ustawienie | Opis |
|:--- |:--- |
| Nazwa |Tekst wyświetlany w górnej części fragmentu. |
| Opis |Tekst wyświetlany w obszarze Nazwa kafelka. |
| Pierwszy wykres | |
| Legenda |Tekst wyświetlany w obszarze objaśnienia pierwszy serii. |
| Kolor |Kolor używany dla kolumn w pierwszej serii. |
| Wykres zapytania |Wyślij zapytanie do uruchamiania dla pierwszej serii.  Liczba rekordów przez każdego interwału czasu będą reprezentowane przez kolumny wykresu. |
| Operacja |Operacja do wykonania na wartość właściwości Podsumowując pojedyncza wartość objaśnienia.<br><br>— Średnia: Średnia wartość ze wszystkich rekordów.<br>-Liczba: Liczba wszystkich rekordów zwróconych przez kwerendę.<br>-Ostatniej próbki: Wartość od ostatniego interwału uwzględnione na wykresie.<br>-Max: Wartość maksymalna z interwałów uwzględnione na wykresie. |
| **Drugi wykresu** | |
| Legenda |Tekst wyświetlany w obszarze objaśnienia drugi serii. |
| Kolor |Kolor używany dla kolumn w drugiej serii. |
| Wykres zapytania |Wyślij zapytanie do uruchamiania dla drugiej serii.  Liczba rekordów przez każdego interwału czasu będą reprezentowane przez kolumny wykresu. |
| Operacja |Operacja do wykonania na wartość właściwości Podsumowując pojedyncza wartość objaśnienia.<br><br>— Średnia: Średnia wartość ze wszystkich rekordów.<br>-Liczba: Liczba wszystkich rekordów zwróconych przez kwerendę.<br>-Ostatniej próbki: Wartość od ostatniego interwału uwzględnione na wykresie.<br>-Max: Wartość maksymalna z interwałów uwzględnione na wykresie. |
| **Zaawansowane** |**> Weryfikacja przepływu danych** |
| Enabled (Włączony) |Wybierz, jeśli Weryfikacja przepływu danych powinno być włączone dla fragmentu.  Oferuje komunikat alternatywnego, jeśli dane nie są dostępne dla fragmentu.  Służy to zwykle podać komunikat okresie tymczasowego, gdy widok jest zainstalowany i danych jest dostępne. |
| Zapytanie |Wyślij zapytanie do uruchomienia, aby sprawdzić, czy dane są dostępne dla widoku.  Jeśli zapytanie nie przynosi efektów, zamiast wartości z główne zapytanie zostanie wyświetlony komunikat. |
| Komunikat |Komunikat wyświetlany, jeśli zapytanie weryfikacji przepływu danych zwraca żadnych danych.  Jeśli podasz żaden komunikat *wykonywania oceny* jest wyświetlany. |


## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [dziennika wyszukiwania](log-analytics-log-searches.md) do obsługi zapytań na kafelkach.
* Dodaj [części wizualizacji](log-analytics-view-designer-parts.md) do widoku niestandardowym.
