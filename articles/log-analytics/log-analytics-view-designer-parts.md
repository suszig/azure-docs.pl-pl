---
title: "Część odwołania dla projektanta widoków w OMS Log Analytics | Dokumentacja firmy Microsoft"
description: "Widok projektanta w analizy dzienników umożliwia tworzenie niestandardowych widoków w konsoli OMS, która zawiera różne wizualizacje danych w repozytorium OMS. Ten artykuł zawiera odwołanie do ustawienia dla każdej części wizualizacji dostępne do użycia w niestandardowych widoków."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 5718d620-b96e-4d33-8616-e127ee9379c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 40a6101576708936404447576d704a49666143fe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="log-analytics-view-designer-visualization-part-reference"></a>Odwołanie do części wizualizacji Projektant widoków analizy dziennika
Projektant widoków w analizy dzienników umożliwia tworzenie niestandardowych widoków w konsoli OMS, która zawiera różne wizualizacje danych z repozytorium OMS. Ten artykuł zawiera odwołanie do ustawienia dla każdej części wizualizacji dostępne do użycia w niestandardowych widoków.

Inne artykuły, które są dostępne dla projektanta widoku są następujące:

* [Wyświetl projektanta](log-analytics-view-designer.md) — Omówienie projektanta widoków i procedur tworzenia i edytowania widoków niestandardowych.
* [Kafelek odwołanie](log-analytics-view-designer-tiles.md) — odwołanie do ustawienia dla każdego z dostępnych do użycia w niestandardowych widoków Kafelki.

>[!NOTE]
> Jeśli obszaru roboczego został uaktualniony do [języka zapytań nowe analizy dzienników](log-analytics-log-search-upgrade.md), a następnie zapytania we wszystkich widokach musi być napisana w [nowy język kwerendy](https://go.microsoft.com/fwlink/?linkid=856078).  Wszystkie widoki, które zostały utworzone przed obszaru roboczego został uaktualniony będzie automtically przekonwertować.

W poniższej tabeli opisano różne typy dostępnych w Projektancie Widok kafelków.  Poniższe rozdziały zawierają opis każdego typu kafelka w szczegółów i ich właściwości.

| Typ widoku | Opis |
|:--- |:--- |
| [Listy zapytań](#list-of-queries-part) |Wyświetla listę dziennika zapytania wyszukiwania.  Użytkownik może kliknąć na każde zapytanie, aby wyświetlić wyniki. |
| [Liczba & listy](#number-amp-list-part) |Nagłówek ma jeden przedstawiający liczbę rekordów z zapytania wyszukiwania dziennika.  Lista przedstawia top dziesięć wyników z kwerendy z Wykres wskazujący względne wartości liczbowej lub zmian w czasie. |
| [Dwie liczb & listy](#two-numbers-amp-list-part) |Nagłówek ma dwie liczby przedstawiający liczba rekordów dziennika oddzielne zapytania wyszukiwania.  Lista przedstawia top dziesięć wyników z kwerendy z Wykres wskazujący względne wartości liczbowej lub zmian w czasie. |
| [Pierścień & listy](#donut-amp-list-part) |Nagłówek Wyświetla jeden numer podsumowanie z kolumną wartości w zapytaniu dziennika.  Pierścień graficznie wyświetla wyniki top trzy rekordów. |
| [Dwóch osiach czasu & listy](#two-timelines-amp-list-part) |Nagłówek wyświetla wyniki dwa zapytania dziennika w czasie, gdy wykresy kolumnowe z objaśnienia wyświetlanie jeden numer podsumowanie z kolumną wartości w zapytaniu dziennika.  Lista przedstawia top dziesięć wyników z kwerendy z Wykres wskazujący względne wartości liczbowej lub zmian w czasie. |
| [Informacje](#information-part) |Nagłówek Wyświetla tekst statyczny oraz opcjonalnie łącza.  Lista zawiera jeden lub więcej elementów z tekst statyczny oraz tytułu. |
| [Wykres liniowy, objaśnienia & listy](#line-chart-callout-amp-list-part) |Nagłówek Wyświetla wykres liniowy z wielu serii w wyniku zapytania dziennika przez czas i objaśnienie z podsumowaniem wartości.  Lista przedstawia top dziesięć wyników z kwerendy z Wykres wskazujący względne wartości liczbowej lub zmian w czasie. |
| [Wykres liniowy i listę](#line-chart-amp-list-part) |Nagłówek przedstawia wykres liniowy z wielu serii w wyniku zapytania dziennika wraz z upływem czasu.  Lista przedstawia top dziesięć wyników z kwerendy z Wykres wskazujący względne wartości liczbowej lub zmian w czasie. |
| [Stos część wykresów wiersza](#stack-of-line-charts-part) |Wyświetla trzech oddzielnych wierszach wykresów z wielu serii w wyniku zapytania dziennika wraz z upływem czasu. |

## <a name="list-of-queries-part"></a>Listę części zapytania
Wyświetla listę dziennika zapytania wyszukiwania.  Użytkownik może kliknąć na każde zapytanie, aby wyświetlić wyniki.  Widok domyślnie zawierają pojedynczego zapytania i kliknięcie **+ zapytania** można dodać dodatkowych zapytań.

![Lista Widok kwerendy](media/log-analytics-view-designer/view-list-queries.png)

| Ustawienie | Opis |
|:--- |:--- |
| **Ogólne** | |
| Tytuł |Tekst wyświetlany u góry widoku. |
| Nowa grupa |Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Wstępnie wybrane filtry |Rozdzielana przecinkami lista właściwości do włączenia w okienku po lewej stronie filtru, gdy użytkownik wybierze zapytania. |
| Tryb renderowania |Widok początkowy wyświetlane, gdy wybrano zapytania.  Użytkownik może wybrać wszystkie widoki dostępne po otwarciu zapytania. |
| **Zapytania** | |
| Zapytania wyszukiwania |Zapytanie do uruchomienia. |
| Przyjazna nazwa |Opisowa nazwa zapytania do użytkownika. |

## <a name="number--list-part"></a>Listy i numer części
Nagłówek ma jeden przedstawiający liczbę rekordów z zapytania wyszukiwania dziennika.  Lista przedstawia top dziesięć wyników z kwerendy z Wykres wskazujący względne wartości liczbowej lub zmian w czasie.

![Lista Widok kwerendy](media/log-analytics-view-designer/view-number-list.png)

| Ustawienie | Opis |
|:--- |:--- |
| **Ogólne** | |
| Tytuł grupy |Tekst wyświetlany u góry widoku. |
| Nowa grupa |Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Ikona |Plik obrazu do wyświetlenia obok wynik w nagłówku. |
| Użyj ikony |Umożliwia wyświetlanie ikon. |
| **Tytuł** | |
| Legenda |Tekst wyświetlany w górnej części nagłówka. |
| Zapytanie |Zapytania w celu uruchomienia nagłówka.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| **Lista** | |
| Zapytanie |Wyślij zapytanie do uruchamiania dla listy.  Pojawi się właściwości pierwszych dwóch pierwszych dziesięciu rekordów w wynikach.  Pierwszą właściwością powinna być wartość tekstową i drugą właściwością wartość liczbową.  Paski są tworzone automatycznie na podstawie względnej wartości liczbowe kolumny.<br><br>Aby posortować rekordy na liście, użyj polecenia sortowania w zapytaniu.  Użytkownik może kliknąć, aby wyświetlić całą uruchomić zapytanie i zwrócić wszystkie rekordy. |
| Ukryj wykresu |Wybierz, aby wyłączyć wykres z prawej strony kolumny liczbowej. |
| Włącz wykresy przebiegu w czasie |Wybierz, aby wyświetlić wykres przebiegu w czasie zamiast poziomy pasek.  Zobacz [typowe ustawienia](#sparklines) szczegółowe informacje. |
| Kolor |Kolor słupków lub wykresy przebiegu w czasie. |
| Nazwa i wartość separatora |Ogranicznik pojedynczy znak, jeśli chcesz przeanalizować właściwości text w wielu wartości.  Zobacz [typowe ustawienia](#name-value-separator) szczegółowe informacje. |
| Zapytanie nawigacji |Wyślij zapytanie do uruchomienia, gdy użytkownik wybierze element na liście.  Zobacz [typowe ustawienia](#navigation-query) szczegółowe informacje. |
| **Lista** |**> Tytuły kolumn** |
| Nazwa |Tekst wyświetlany w górnej części pierwszej kolumny listy. |
| Wartość |Tekst do wyświetlenia na początku drugiej kolumny listy. |
| **Lista** |**> Progi** |
| Włącz progi |Zaznacz, aby włączyć progów.  Zobacz [typowe ustawienia](#thresholds) szczegółowe informacje. |

## <a name="two-numbers--list-part"></a>Dwie liczby i listy
Nagłówek ma dwie liczby przedstawiający liczba rekordów dziennika oddzielne zapytania wyszukiwania.  Lista przedstawia top dziesięć wyników z kwerendy z Wykres wskazujący względne wartości liczbowej lub zmian w czasie.

![Dwie liczby i widok listy](media/log-analytics-view-designer/view-two-numbers-list.png)

| Ustawienie | Opis |
|:--- |:--- |
| **Ogólne** | |
| Tytuł grupy |Tekst wyświetlany u góry widoku. |
| Nowa grupa |Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Ikona |Plik obrazu do wyświetlenia obok wynik w nagłówku. |
| Użyj ikony |Umożliwia wyświetlanie ikon. |
| **Tytuł** | |
| Legenda |Tekst wyświetlany w górnej części nagłówka. |
| Zapytanie |Zapytania w celu uruchomienia nagłówka.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| **Lista** | |
| Zapytanie |Wyślij zapytanie do uruchamiania dla listy.  Pojawi się właściwości pierwszych dwóch pierwszych dziesięciu rekordów w wynikach.  Pierwszą właściwością powinna być wartość tekstową i drugą właściwością wartość liczbową.  Paski są tworzone automatycznie na podstawie względnej wartości liczbowe kolumny.<br><br>Aby posortować rekordy na liście, użyj polecenia sortowania w zapytaniu.  Użytkownik może kliknąć, aby wyświetlić całą uruchomić zapytanie i zwrócić wszystkie rekordy. |
| Ukryj wykresu |Wybierz, aby wyłączyć wykres z prawej strony kolumny liczbowej. |
| Włącz wykresy przebiegu w czasie |Wybierz, aby wyświetlić wykres przebiegu w czasie zamiast poziomy pasek.  Zobacz [typowe ustawienia](#sparklines) szczegółowe informacje. |
| Kolor |Kolor słupków lub wykresy przebiegu w czasie. |
| Operacja |Operacja do wykonania dla przebiegu w czasie.  Zobacz [typowe ustawienia](#sparklines) szczegółowe informacje. |
| Nazwa i wartość separatora |Ogranicznik pojedynczy znak, jeśli chcesz przeanalizować właściwości text w wielu wartości.  Zobacz [typowe ustawienia](#name-value-separator) szczegółowe informacje. |
| Zapytanie nawigacji |Wyślij zapytanie do uruchomienia, gdy użytkownik wybierze element na liście.  Zobacz [typowe ustawienia](#navigation-query) szczegółowe informacje. |
| **Lista** |**> Tytuły kolumn** |
| Nazwa |Tekst wyświetlany w górnej części pierwszej kolumny listy. |
| Wartość |Tekst do wyświetlenia na początku drugiej kolumny listy. |
| **Lista** |**> Progi** |
| Włącz progi |Zaznacz, aby włączyć progów.  Zobacz [typowe ustawienia](#thresholds) szczegółowe informacje. |

## <a name="donut--list-part"></a>Część pierścień & listy
Nagłówek Wyświetla jeden numer podsumowanie z kolumną wartości w zapytaniu dziennika.  Pierścień graficznie wyświetla wyniki top trzy rekordów.

![Widok listy i pierścień](media/log-analytics-view-designer/view-donut-list.png)

| Ustawienie | Opis |
|:--- |:--- |
| **Ogólne** | |
| Tytuł grupy |Tekst wyświetlany w górnej części fragmentu. |
| Nowa grupa |Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Ikona |Plik obrazu do wyświetlenia obok wynik w nagłówku. |
| Użyj ikony |Umożliwia wyświetlanie ikon. |
| **Nagłówek** | |
| Tytuł |Tekst wyświetlany w górnej części nagłówka. |
| Podtytuł |Tekst do wyświetlenia pod tytułem w górnej części nagłówka. |
| **Pierścień** | |
| Zapytanie |Wyślij zapytanie do uruchamiania dla pierścień.  Pierwszą właściwością powinna być wartość tekstową i drugą właściwością wartość liczbową. |
| **Pierścień** |**> Centrum** |
| Tekst |Tekst wyświetlany w obszarze wartość w pierścień. |
| Operacja |Operacja do wykonania na wartość właściwości Podsumowując pojedyncza wartość.<br><br>-Sumy: Dodaj odpowiednie wartości wszystkich rekordów.<br>— Procent: Procent rekordów zwróconych przez wartości **powoduje wartości używana podczas operacji centrum** całkowita liczba rekordów w zapytaniu. |
| Wynik wartości używana podczas operacji Centrum |Opcjonalnie kliknij znak plus, aby dodać jedną lub więcej wartości.  Wyniki zapytania będzie ograniczona do rekordów z wartościami właściwości, które określisz.  Jeśli wartości nie zostaną dodane, wszystkie rekordy są uwzględnione w zapytaniu. |
| **Dodatkowe opcje** |**> Kolory** |
| Kolor 1<br>Kolor 2<br>Kolor 3 |Wybierz kolor dla każdego z wartości wyświetlane w pierścień. |
| **Dodatkowe opcje** |**> Mapowanie kolorów zaawansowane** |
| Wartość pola |Wpisz nazwę pola do wyświetlenia jako inny kolor, jeśli znajduje się on w pierścień. |
| Kolor |Wybierz kolor unikatowe pole. |
| **Lista** | |
| Zapytanie |Wyślij zapytanie do uruchamiania dla listy.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| Ukryj wykresu |Wybierz, aby wyłączyć wykres z prawej strony kolumny liczbowej. |
| Włącz wykresy przebiegu w czasie |Wybierz, aby wyświetlić wykres przebiegu w czasie zamiast poziomy pasek.  Zobacz [typowe ustawienia](#sparklines) szczegółowe informacje. |
| Kolor |Kolor słupków lub wykresy przebiegu w czasie. |
| Operacja |Operacja do wykonania dla przebiegu w czasie.  Zobacz [typowe ustawienia](#sparklines) szczegółowe informacje. |
| Nazwa i wartość separatora |Ogranicznik pojedynczy znak, jeśli chcesz przeanalizować właściwości text w wielu wartości.  Zobacz [typowe ustawienia](#name-value-separator) szczegółowe informacje. |
| Zapytanie nawigacji |Wyślij zapytanie do uruchomienia, gdy użytkownik wybierze element na liście.  Zobacz [typowe ustawienia](#navigation-query) szczegółowe informacje. |
| **Lista** |**> Tytuły kolumn** |
| Nazwa |Tekst wyświetlany w górnej części pierwszej kolumny listy. |
| Wartość |Tekst do wyświetlenia na początku drugiej kolumny listy. |
| **Lista** |**> Progi** |
| Włącz progi |Zaznacz, aby włączyć progów.  Zobacz [typowe ustawienia](#thresholds) szczegółowe informacje. |

## <a name="two-timelines--list-part"></a>Dwie części listy i osi czasu
Nagłówek wyświetla wyniki dwa zapytania dziennika w czasie, gdy wykresy kolumnowe z objaśnienia wyświetlanie jeden numer podsumowanie z kolumną wartości w zapytaniu dziennika.  Lista przedstawia top dziesięć wyników z kwerendy z Wykres wskazujący względne wartości liczbowej lub zmian w czasie.

![Wyświetlanie dwóch osiach czasu & listy](media/log-analytics-view-designer/view-two-timelines-list.png)

| Ustawienie | Opis |
|:--- |:--- |
| **Ogólne** | |
| Tytuł grupy |Tekst wyświetlany w górnej części fragmentu. |
| Nowa grupa |Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Ikona |Plik obrazu do wyświetlenia obok wynik w nagłówku. |
| Użyj ikony |Umożliwia wyświetlanie ikon. |
| **Najpierw wykresu<br>drugie wykresu** | |
| Legenda |Tekst wyświetlany w obszarze objaśnienia pierwszy serii. |
| Kolor |Kolor używany do kolumn w serii. |
| Zapytanie |Wyślij zapytanie do uruchamiania dla pierwszej serii.  Liczba rekordów przez każdego interwału czasu będą reprezentowane przez kolumny wykresu. |
| Operacja |Operacja do wykonania na wartość właściwości Podsumowując pojedyncza wartość objaśnienia.<br><br>— Suma: Suma wartość ze wszystkich rekordów.<br>— Średnia: Średnia wartość ze wszystkich rekordów.<br>-Ostatniej próbki: Wartość od ostatniego interwału uwzględnione na wykresie.<br>— Próbki pierwszy: Wartość z pierwszy interwał uwzględnione na wykresie.<br>-Liczba: Liczba wszystkich rekordów zwróconych przez kwerendę. |
| **Lista** | |
| Zapytanie |Wyślij zapytanie do uruchamiania dla listy.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| Ukryj wykresu |Wybierz, aby wyłączyć wykres z prawej strony kolumny liczbowej. |
| Włącz wykresy przebiegu w czasie |Wybierz, aby wyświetlić wykres przebiegu w czasie zamiast poziomy pasek.  Zobacz [typowe ustawienia](#sparklines) szczegółowe informacje. |
| Kolor |Kolor słupków lub wykresy przebiegu w czasie. |
| Operacja |Operacja do wykonania dla przebiegu w czasie.  Zobacz [typowe ustawienia](#sparklines) szczegółowe informacje. |
| Zapytanie nawigacji |Wyślij zapytanie do uruchomienia, gdy użytkownik wybierze element na liście.  Zobacz [typowe ustawienia](#navigation-query) szczegółowe informacje. |
| **Lista** |**> Tytuły kolumn** |
| Nazwa |Tekst wyświetlany w górnej części pierwszej kolumny listy. |
| Wartość |Tekst do wyświetlenia na początku drugiej kolumny listy. |
| **Lista** |**> Progi** |
| Włącz progi |Zaznacz, aby włączyć progów.  Zobacz [typowe ustawienia](#thresholds) szczegółowe informacje. |

## <a name="information-part"></a>Część informacji
Nagłówek Wyświetla tekst statyczny oraz opcjonalnie łącza.  Lista zawiera jeden lub więcej elementów z tekst statyczny oraz tytułu.

![Wyświetlanie informacji](media/log-analytics-view-designer/view-information.png)

| Ustawienie | Opis |
|:--- |:--- |
| **Ogólne** | |
| Tytuł grupy |Tekst wyświetlany w górnej części fragmentu. |
| Nowa grupa |Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Kolor |Kolor tła nagłówka. |
| **Nagłówek** | |
| Image (Obraz) |Plik obrazu do wyświetlenia w nagłówku. |
| Etykieta |Tekst wyświetlany w nagłówku. |
| **Nagłówek** |**> Link** |
| Etykieta |Tekst łącza. |
| Url |Adres URL dla łącza. |
| **Elementy informacji** | |
| Tytuł |Tekst do wyświetlenia tytułu poszczególnych elementów. |
| Zawartość |Tekst do wyświetlenia dla każdego elementu. |

## <a name="line-chart-callout--list-part"></a>Wykres liniowy, objaśnienia & part listy
Nagłówek Wyświetla wykres liniowy z wielu serii w wyniku zapytania dziennika przez czas i objaśnienie z podsumowaniem wartości.  Lista przedstawia top dziesięć wyników z kwerendy z Wykres wskazujący względne wartości liczbowej lub zmian w czasie.

![Wykres liniowy, objaśnienia i widok listy](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Ustawienie | Opis |
|:--- |:--- |
| **Ogólne** | |
| Tytuł grupy |Tekst wyświetlany w górnej części fragmentu. |
| Nowa grupa |Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Ikona |Plik obrazu do wyświetlenia obok wynik w nagłówku. |
| Użyj ikony |Umożliwia wyświetlanie ikon. |
| **Nagłówek** | |
| Tytuł |Tekst wyświetlany w górnej części nagłówka. |
| Podtytuł |Tekst do wyświetlenia pod tytułem w górnej części nagłówka. |
| **Wykres liniowy** | |
| Zapytanie |Zapytania w celu uruchomienia wykresu liniowego.  Pierwszą właściwością powinna być wartość tekstową i drugą właściwością wartość liczbową.  Jest to zazwyczaj kwerendę, która używa **miary** — słowo kluczowe do podsumowania wyników.  Jeśli zapytanie używa **interwał** — słowo kluczowe następnie osi x wykresu użyje tego przedziału czasu.  Jeśli zapytanie nie obejmuje **interwał** interwałów — słowo kluczowe, a następnie co godzinę używanych dla osi x. |
| **Wykres liniowy** |**> Objaśnienie** |
| Tytuł objaśnienia |Tekst do wyświetlenia powyżej wartości objaśnienia. |
| Nazwa serii |Wartość właściwości serii użyć wartości objaśnienia.  W przypadku serii nie są używane wszystkie rekordy z zapytania. |
| Operacja |Operacja do wykonania na wartość właściwości Podsumowując pojedyncza wartość objaśnienia.<br><br>— Średnia: Średnia wartość ze wszystkich rekordów.<br>— Count Liczba wszystkich rekordów zwróconych przez kwerendę.<br>-Ostatniej próbki: Wartość od ostatniego interwału uwzględnione na wykresie.<br>-Max: Wartość maksymalna z interwałów uwzględnione na wykresie.<br>-Min: Wartość minimalna z interwałów uwzględnione na wykresie.<br>— Suma: Suma wartość ze wszystkich rekordów. |
| **Wykres liniowy** |**> Oś Y** |
| Użyj skali logarytmicznej |Zaznacz, aby użyć skali logarytmicznej dla osi y. |
| Jednostki |Określ jednostki dla wartości zwróconych przez kwerendę.  Te informacje jest używany do wyświetlania etykiet na wykresie wskazujący typy wartości i opcjonalnie do konwertowania wartości.  Typ jednostki określa kategorię jednostki i definiuje wartości bieżącego typu jednostki, które są dostępne.  W przypadku wybrania wartości w przekonwertować, a następnie wartości liczbowych są konwertowane z typem bieżącej jednostce do konwersji do typu. |
| Etykiety niestandardowej |Tekst do wyświetlenia dla osi Y obok etykiety dla tego typu jednostki.  Jeśli etykieta nie jest określony, wyświetlana jest tylko typ jednostki. |
| **Lista** | |
| Zapytanie |Wyślij zapytanie do uruchamiania dla listy.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| Ukryj wykresu |Wybierz, aby wyłączyć wykres z prawej strony kolumny liczbowej. |
| Włącz wykresy przebiegu w czasie |Wybierz, aby wyświetlić wykres przebiegu w czasie zamiast poziomy pasek.  Zobacz [typowe ustawienia](#sparklines) szczegółowe informacje. |
| Kolor |Kolor słupków lub wykresy przebiegu w czasie. |
| Operacja |Operacja do wykonania dla przebiegu w czasie.  Zobacz [typowe ustawienia](#sparklines) szczegółowe informacje. |
| Nazwa i wartość separatora |Ogranicznik pojedynczy znak, jeśli chcesz przeanalizować właściwości text w wielu wartości.  Zobacz [typowe ustawienia](#name-value-separator) szczegółowe informacje. |
| Zapytanie nawigacji |Wyślij zapytanie do uruchomienia, gdy użytkownik wybierze element na liście.  Zobacz [typowe ustawienia](#navigation-query) szczegółowe informacje. |
| **Lista** |**> Tytuły kolumn** |
| Nazwa |Tekst wyświetlany w górnej części pierwszej kolumny listy. |
| Wartość |Tekst do wyświetlenia na początku drugiej kolumny listy. |
| **Lista** |**> Progi** |
| Włącz progi |Zaznacz, aby włączyć progów.  Zobacz [typowe ustawienia](#thresholds) szczegółowe informacje. |

## <a name="line-chart--list-part"></a>Część wiersza wykresu & listy
Nagłówek przedstawia wykres liniowy z wielu serii w wyniku zapytania dziennika wraz z upływem czasu.  Lista przedstawia top dziesięć wyników z kwerendy z Wykres wskazujący względne wartości liczbowej lub zmian w czasie.

![Widok wykresu & listy wiersza](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Ustawienie | Opis |
|:--- |:--- |
| **Ogólne** | |
| Tytuł grupy |Tekst wyświetlany w górnej części fragmentu. |
| Nowa grupa |Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Ikona |Plik obrazu do wyświetlenia obok wynik w nagłówku. |
| Użyj ikony |Umożliwia wyświetlanie ikon. |
| **Nagłówek** | |
| Tytuł |Tekst wyświetlany w górnej części nagłówka. |
| Podtytuł |Tekst do wyświetlenia pod tytułem w górnej części nagłówka. |
| **Wykres liniowy** | |
| Zapytanie |Zapytania w celu uruchomienia wykresu liniowego.  Pierwszą właściwością powinna być wartość tekstową i drugą właściwością wartość liczbową.  Jest to zazwyczaj kwerendę, która używa **miary** — słowo kluczowe do podsumowania wyników.  Jeśli zapytanie używa **interwał** — słowo kluczowe następnie osi x wykresu użyje tego przedziału czasu.  Jeśli zapytanie nie obejmuje **interwał** interwałów — słowo kluczowe, a następnie co godzinę używanych dla osi x. |
| **Wykres liniowy** |**> Oś Y** |
| Użyj skali logarytmicznej |Zaznacz, aby użyć skali logarytmicznej dla osi y. |
| Jednostki |Określ jednostki dla wartości zwróconych przez kwerendę.  Te informacje jest używany do wyświetlania etykiet na wykresie wskazujący typy wartości i opcjonalnie do konwertowania wartości.  Typ jednostki określa kategorię jednostki i definiuje wartości bieżącego typu jednostki, które są dostępne.  W przypadku wybrania wartości w przekonwertować, a następnie wartości liczbowych są konwertowane z typem bieżącej jednostce do konwersji do typu. |
| Etykiety niestandardowej |Tekst do wyświetlenia dla osi Y obok etykiety dla tego typu jednostki.  Jeśli etykieta nie jest określony, wyświetlana jest tylko typ jednostki. |
| **Lista** | |
| Zapytanie |Wyślij zapytanie do uruchamiania dla listy.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| Ukryj wykresu |Wybierz, aby wyłączyć wykres z prawej strony kolumny liczbowej. |
| Włącz wykresy przebiegu w czasie |Wybierz, aby wyświetlić wykres przebiegu w czasie zamiast poziomy pasek.  Zobacz [typowe ustawienia](#sparklines) szczegółowe informacje. |
| Kolor |Kolor słupków lub wykresy przebiegu w czasie. |
| Operacja |Operacja do wykonania dla przebiegu w czasie.  Zobacz [typowe ustawienia](#sparklines) szczegółowe informacje. |
| Nazwa i wartość separatora |Ogranicznik pojedynczy znak, jeśli chcesz przeanalizować właściwości text w wielu wartości.  Zobacz [typowe ustawienia](#name-value-separator) szczegółowe informacje. |
| Zapytanie nawigacji |Wyślij zapytanie do uruchomienia, gdy użytkownik wybierze element na liście.  Zobacz [typowe ustawienia](#navigation-query) szczegółowe informacje. |
| **Lista** |**> Tytuły kolumn** |
| Nazwa |Tekst wyświetlany w górnej części pierwszej kolumny listy. |
| Wartość |Tekst do wyświetlenia na początku drugiej kolumny listy. |
| **Lista** |**> Progi** |
| Włącz progi |Zaznacz, aby włączyć progów.  Zobacz [typowe ustawienia](#thresholds) szczegółowe informacje. |

## <a name="stack-of-line-charts-part"></a>Stos część wykresów wiersza
Wyświetla trzech oddzielnych wierszach wykresów z wielu serii w wyniku zapytania dziennika wraz z upływem czasu.

![Stos wykresy liniowe](media/log-analytics-view-designer/view-stack-line-charts.png)

| Ustawienie | Opis |
|:--- |:--- |
| **Ogólne** | |
| Tytuł grupy |Tekst wyświetlany w górnej części fragmentu. |
| Nowa grupa |Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Ikona |Plik obrazu do wyświetlenia obok wynik w nagłówku. |
| **Wykres 1<br>wykresu 2<br>wykresu 3** |**> Nagłówek** |
| Tytuł |Tekst wyświetlany w górnej części wykresu. |
| Podtytuł |Tekst do wyświetlenia pod tytułem w górnej części wykresu. |
| **Wykres 1<br>wykresu 2<br>wykresu 3** |**Wykres liniowy** |
| Zapytanie |Zapytania w celu uruchomienia wykresu liniowego.  Pierwszą właściwością powinna być wartość tekstową i drugą właściwością wartość liczbową.  Jest to zazwyczaj kwerendę, która używa **miary** — słowo kluczowe do podsumowania wyników.  Jeśli zapytanie używa **interwał** — słowo kluczowe następnie osi x wykresu użyje tego przedziału czasu.  Jeśli zapytanie nie obejmuje **interwał** interwałów — słowo kluczowe, a następnie co godzinę używanych dla osi x. |
| **Wykres** |**> Oś Y** |
| Użyj skali logarytmicznej |Zaznacz, aby użyć skali logarytmicznej dla osi y. |
| Jednostki |Określ jednostki dla wartości zwróconych przez kwerendę.  Te informacje jest używany do wyświetlania etykiet na wykresie wskazujący typy wartości i opcjonalnie do konwertowania wartości.  Typ jednostki określa kategorię jednostki i definiuje wartości bieżącego typu jednostki, które są dostępne.  W przypadku wybrania wartości w przekonwertować, a następnie wartości liczbowych są konwertowane z typem bieżącej jednostce do konwersji do typu. |
| Etykiety niestandardowej |Tekst do wyświetlenia dla osi Y obok etykiety dla tego typu jednostki.  Jeśli etykieta nie jest określony, wyświetlana jest tylko typ jednostki. |

## <a name="common-settings"></a>Typowe ustawienia
W poniższych sekcjach opisano ustawienia wspólne dla kilku części wizualizacji.

### <a name="name-value-separator">Nazwa i wartość separatora</a>
Ogranicznik pojedynczy znak, jeśli chcesz przeanalizować właściwości text w wyniku zapytania listy do wielu wartości.  Jeśli zostanie określony ogranicznik, musisz podać nazwy dla każdego pola rozdzielonych tego samego ogranicznika w polu Nazwa.

Rozważmy na przykład właściwość o nazwie *lokalizacji* który dostępnych wartości takich jak *41 budowania Redmond* i *Bellevue Building12*.  Można określić — nazwy i wartości separatora i *budowania miast* dla nazwy.  Spowoduje to Przeanalizuj wartość każdej w dwie właściwości o nazwie *miasta* i *budynku*.

### <a name="navigation-query">Zapytanie nawigacji</a>
Wyślij zapytanie do uruchomienia, gdy użytkownik wybierze element na liście.  Użyj *{wybranego elementu}* uwzględnienie składnia elementu wybranego użytkownika.

Na przykład, jeśli zapytanie ma kolumnę o nazwie *komputera* zapytanie nawigacji *{wybranego elementu}*, zapytania, takie jak *komputer = "Mój komputer"* będzie można uruchamiać, gdy użytkownik Wybrany komputer.  Jeśli zapytanie nawigacji jest *typu = zdarzeń {wybranego elementu}* następnie zapytania *typu = zdarzeń komputer = "Mój komputer"* zostanie uruchomione.

### <a name="sparklines">Wykresy przebiegu w czasie</a>
Wykres przebiegu w czasie jest wykres liniowy małych, która ilustruje wartość wpisu listy wraz z upływem czasu.  Wizualizacji składników z listy można wybrać, czy mają być wyświetlane poziomym paskiem wskazujący względne wartości liczbowej lub wykres przebiegu w czasie jej wartość wskazującą, w czasie.

W poniższej tabeli opisano ustawienia wykresy przebiegu w czasie.

| Ustawienie | Opis |
|:--- |:--- |
| Włącz wykresy przebiegu w czasie |Wybierz, aby wyświetlić wykres przebiegu w czasie zamiast poziomy pasek. |
| Operacja |Wykresy przebiegu w czasie są włączone, to działanie do wykonania na każdej właściwości w listy w celu obliczenia wartości dla przebiegu w czasie.<br><br>-Ostatniej próbki: Ostatnia wartość serii przedziałach czasu.<br>-Max: Wartość maksymalna serii przedziałach czasu.<br>-Min: Wartość minimalna serii przedziałach czasu.<br>— Suma: Suma wartości serii dla interwału czasu.<br>-Podsumowanie: Używa tego samego polecenia miary kwerendy w nagłówku. |

### <a name="thresholds">Progi</a>
Progi pozwalają na wyświetlanie kolorowe ikony obok każdego elementu na liście, umożliwiając szybkie wizualnej elementów, które przekracza określoną wartość, lub mieścić się w określonym zakresie.  Na przykład można wyświetlić zieloną ikonę dla elementów z dozwolonej wartości, żółty, jeśli wartość znajduje się w zakresie, który wyświetla ostrzeżenie i czerwony, w razie przekroczenia wartości błędu.

Po włączeniu progi dla części, należy określić co najmniej jeden progów.  Jeśli wartość elementu jest większa niż wartość progowa i niższa niż wartość progowa dalej, że kolor jest używany.  Jeśli element jest większa niż wartość progowa następnie najwyższy, tego koloru zostanie ustawiona.   

Każdy zestaw próg ma jeden próg o wartości **domyślne**.  Jest to kolor ustawiony w przypadku przekroczenia żadnych innych wartości.  Można dodawać i usuwać progów, klikając  **+**  lub **x** przycisku.

W poniższej tabeli opisano ustawienia progów.

| Ustawienie | Opis |
|:--- |:--- |
| Włącz progi |Umożliwia wyświetlanie ikon koloru z lewej strony każdego wskazujące, jego kondycja względem określonego progów. |
| Nazwa |Nazwę identyfikującą wartość progową. |
| Próg |Wartość progu.  Kolor najwyższą wartość progu przekroczenia wartości elementu ustawiony jest kolor kondycji dla każdego elementu listy.  Istnieje jeden domyślny próg będący kolor, jeśli nie wartości progowe. |
| Kolor |Kolor wartości progowej. |

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [dziennika wyszukiwania](log-analytics-log-searches.md) do obsługi zapytań w częściach wizualizacji.
