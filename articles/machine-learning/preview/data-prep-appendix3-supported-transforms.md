---
title: "Użyć transformacji danych w celu przygotowania danych w usłudze Azure Machine Learning | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera pełną listę przekształceń dostępne do przygotowania bazy danych usługi Azure Machine Learning"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 10/09/2017
ms.openlocfilehash: 4331bb4a16d56f027dd63a97868822fa6ecc92d0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-data-transforms-for-data-preparation-in-azure-machine-learning"></a>Przekształcenia danych użycia w celu przygotowania danych w usłudze Azure Machine Learning

A *przekształcenie* w usłudze Azure Machine Learning pobiera dane w danym formacie, wykonuje operacje na danych (np. zmiana typu danych) i następnie tworzy danych do nowego formatu. Przekształcanie poszczególnych ma swój własny interfejs i zachowania. Tworzenia łańcucha kilka przekształceń ze sobą za pomocą kroków przepływu danych, co umożliwia wykonywanie złożonych i powtarzalne przekształcenia na podstawie danych. Jest to podstawowe funkcje przygotowywania danych.

Poniżej znajduje się lista dostępnych w usłudze Azure Machine Learning przekształcenia. 

## <a name="column-selection"></a>Wybór kolumny 
Wiele z następujących przekształcenia wymienionych działają w pojedynczej kolumny lub wiele. Aby wybrać wiele kolumn, użyj **Ctrl** klucza; lub aby wybrać zakres kolumn, użyj **Shift** klucza.

## <a name="transforms-from-main-menu-andor-grid-header"></a>Przekształca z głównego nagłówka menu i/lub siatki 
Transformacje mogą być używane z opcją transformacje w menu głównym. Transformacje również może zostać wybrany przez kliknięcie prawym przyciskiem myszy nazwę kolumny w siatce danych. Jeśli wybrano wiele kolumn, następnie prawym przyciskiem myszy dowolnego z nich zawiera menu transformacji.

Tylko prawidłowe przekształceń dla z typem danych wybranym są dostępne w menu kliknij prawym przyciskiem myszy. Menu głównego oferuje wszystkie transformacje, ale wyłącza przekształcenia, które nie mają znaczenia dla wybranych kolumn.

Mały podzbiór kontekstowe transformacji jest dostępna przez kliknięcie prawym przyciskiem myszy komórki. Widoczne mogą podlegać kopiowaniu, Zamień i filtrowania. Widoczne są pamiętać — typ danych, więc opcje dla numeru kolumny są inne niż dla kolumny typu string.

## <a name="derive-column-by-example"></a>Kolumna przykładzie pochodzi
Tej transformacji umożliwia utworzenie nowej kolumny jako pochodną co najmniej jeden z istniejących kolumn. Przekształcenia analizuje kolumny wejściowe (wybrane) i podanym przykładzie, a następnie określa desire dane wyjściowe w nowej kolumnie. 

Aby użyć tej transformacji, wybierz co najmniej jedną kolumnę. Dodaj nową kolumnę pochodnych (pusty) przykładzie. Wpisz na przykład co, które chcesz wyświetlić w kolumnie pochodnych (przy założeniu, że pochodzi od innych kolumn) i "W przykładzie" technologii próbuje Wypełnij wszystkie komórki w kolumnie. 

Przykłady skomplikowane może być konieczne zapewnienie więcej niż jednym z przykładów. Aby to zrobić, wybierz inną komórkę, a następnie wpisz inny przykład.

Technologia "Według przykładu" używa wybranych kolumn próbę ustalić znaczenie przykładem. Jeśli nie wybrano kolumn po wywołaniu tej transformacji, używane są wszystkie komórki dla bieżącego wiersza. Wybieranie wymaganych kolumn prowadzi do bardziej dokładne wyniki.

Można wybrać kolumny przed wywołaniem transformacji. Po Edytor przekształcenia została uruchomiona, pola wyboru w górnej części każdej kolumny wskazują, jakie kolumny są zaznaczone jako dane wejściowe. Można dodać lub usunąć kolumny z zaznaczenia za pomocą pola wyboru w nagłówkach kolumn.

Aby uzyskać bardziej szczegółowy opis **pochodnych kolumny według przykładu** przekształcenia wraz z większej liczby próbek, zobacz: [pochodzi kolumny przez odwołanie przykład](data-prep-derive-column-by-example.md)  

## <a name="split-column-by-example"></a>Podział kolumny według przykładu
Tej transformacji przyjmuje istniejącej kolumny i jest używany aparat "Według przykładu" próbuje podziału kolumny w  *n*  innych kolumn. Istnieje możliwość uruchomienia podziału automatycznie na kolejnych kolumn wygenerowany.

Aby uzyskać bardziej szczegółowy opis **kolumny podzielone przez przykład** przekształcenia wraz z większej liczby próbek, zobacz: [kolumny podzielone przez odwołanie przykład](data-prep-split-column-by-example.md)

## <a name="expand-json"></a>Rozwijanie danych JSON

Tej transformacji umożliwia dodanie wiele kolumn, rozwijając kolumny za pomocą prawidłowego tekstu JSON.

Aby uzyskać bardziej szczegółowy opis **rozwiń JSON** przekształcenia wraz z większej liczby próbek, zobacz: [rozwiń odwołanie JSON](data-prep-expand-json.md)


## <a name="combine-columns-by-example"></a>Łączenie kolumn w przykładzie

Tej transformacji umożliwia Dodaj nową kolumnę przez połączenie wartości z wielu kolumn. 

Aby uzyskać bardziej szczegółowy opis **łączenie kolumn, na przykład** przekształcenia wraz z większej liczby próbek, zobacz: [łączenie kolumn na przykład odwołania](data-prep-combine-columns-by-example.md)


## <a name="duplicate-column"></a>Zduplikowana kolumna
Tej transformacji zostanie utworzona kopia dokładnie jeden lub więcej wybranych kolumn oraz zapewnia każda nowa nazwa pochodzi od oryginalnej nazwy kolumny.

## <a name="text-clustering"></a>Tekst klastra 
Tej transformacji umożliwia niespójne wartości, które powinny być takie same i grupować je razem.  

Jeśli używasz tej transformacji wartości w jednej kolumnie są przeanalizowane pod kątem podobieństwa i zgrupowane w klastrach. Dla każdego klastra jest canonical wartość, która jest wartość, która zastępuje wszystkie wystąpienia klastra i wartości wystąpienia. Zakończenie klastrów można je usunąć i canonical wartość można edytować. Można usunąć wystąpienia z danego klastra. Ponadto można zmienić podobieństwa filtr próg wynik, który został użyty do grupy wystąpień w klastrze.

Domyślnie tej transformacji zastępuje wszystkie wartości wystąpienia klastra wartości kanonicznej dla tego klastra, tworzenie nowej kolumny zawiera nowe wartości. Istnieje również możliwość do dodania na wynik podobieństwa dla każdego wystąpienia do nowej kolumny, (tj. można go nazwać) umożliwia wykorzystanie przez niego później w przepływie danych.

## <a name="replace-values"></a>Zastąp wartości
Tej transformacji umożliwia jednego ciągu, które mają zostać zastąpione przez inny. Ciąg źródłowy może być ciąg częściowe lub pełne komórki; zastąpienie można zastosować do jednej kolumny lub wiele. Ciąg wyszukiwania obsługuje wyszukiwanie znaków specjalnych, a także regularne znaków. 

## <a name="replace-na-values"></a>Zamień NA wartości
Tej transformacji umożliwia różnych różne rodzaje NA (Brak, NA, null, NaN, itp.) lub być puste ciągi do pojedynczej wartości, aby były zgodne. Tej transformacji obsługuje jednego lub wielu kolumn. Tej transformacji znajduje się tylko, jeśli kolumna jest zaznaczona i nie jest dostępny w menu głównym przekształcenie, gdy nie wybrano kolumn.

## <a name="replace-missing-values"></a>Zastąp brakujące wartości
Tej transformacji zastępuje brakujące dane z pojedynczej wartości. Tej transformacji obsługuje jednego lub wielu kolumn. Tej transformacji znajduje się tylko, jeśli kolumna jest zaznaczona i nie jest dostępny w menu głównym przekształcenie, gdy nie wybrano kolumn.

## <a name="replace-error-values"></a>Zastąp wartości błędów
Tej transformacji zastępuje błędy pojedynczą wartość. Tej transformacji obsługuje jednego lub wielu kolumn. Tej transformacji znajduje się tylko, jeśli kolumna jest zaznaczona i nie jest dostępny w menu głównym przekształcenie, gdy nie wybrano kolumn.

## <a name="trim-string"></a>TRIM ciąg
Usuwa tej transformacji początkowe i końcowe znaki "odstępy" (zawiera spacje, tabulatory *itp.*), z co najmniej jedną kolumnę.

## <a name="adjust-precision"></a>Dostosuj dokładności
Tej transformacji umożliwia określenie liczby miejsc dziesiętnych kolumny liczbowej.

## <a name="rename-column"></a>Zmień nazwę kolumny
Tej transformacji zmienia nazwę zaznaczonej kolumny. Tej transformacji można także wywołanej wbudowany w nagłówku kolumny, klikając nazwę kolumny.

## <a name="remove-column"></a>Usuń kolumnę
Tej transformacji spowoduje usunięcie wybranych kolumn. Tej transformacji działa w przypadku zaznaczenia jednej kolumny lub wiele. 

## <a name="keep-column"></a>Zachowaj kolumny
Tej transformacji chroni tylko wybranych kolumn. Tej transformacji działa w przypadku zaznaczenia jednej kolumny lub wiele.

## <a name="handle-path-column"></a>Obsługa kolumny
Podczas importowania pliku ścieżka kolumny jest automatycznie dodawany do zestawu danych w kreatorze Dodaj źródło danych. Zawiera pełną nazwę pliku, który wchodzi w skład ścieżka do zestawu danych. Tej transformacji dodaje lub usuwa dodatkowe kolumny z zestawu danych.

## <a name="convert-field-type-to-numeric"></a>Konwertuj typ pola na liczbowe
Tej transformacji zmienia typ kolumny liczbowe. Można określić separatora, jeżeli ma danych nie jest liczbą całkowitą. Domyślnie tej transformacji nie Monituj separatora, użyj **Edytuj** element menu do wywołania edytora. Tej transformacji działa w przypadku zaznaczenia jednej kolumny lub wiele.

## <a name="convert-field-type-to-date"></a>Konwertowanie typu pola datę
Tej transformacji zmienia typ kolumny na typ date. Domyślny format daty/godziny jest używany, ale można przesłonić przy użyciu `strftime` dyrektywy. Istnieje również możliwość dołączy wartości typu time ze stałym daty.

Domyślnie tej transformacji nie Monituj o formacie **Edytuj** element menu na etapie wynikowe do wywołania edytora. Tej transformacji działa w przypadku zaznaczenia jednej kolumny lub wiele.

Tylko daty między 9-22 — 1677 i 4-11-2262 można przekonwertować na typ date.

## <a name="convert-field-type-to-boolean"></a>Konwertuj typ pola na wartość logiczną
Tej transformacji zmiany kolumny typu PRAWDA/FAŁSZ. Tej transformacji obsługuje mapowanie wielu wartości na wartość PRAWDA lub FAŁSZ, a istnieje możliwość edytować te mapowania. Obsługuje ona również stałą, do którego można mapować wartości, które nie istnieją w tabeli mapowania PRAWDA/FAŁSZ. Tej transformacji działa w przypadku zaznaczenia jednej kolumny lub wiele.

## <a name="convert-field-type-to-string"></a>Konwertuj typ pola na ciąg
Tej transformacji zmienia typ kolumny na ciąg. Tej transformacji działa w przypadku zaznaczenia jednej kolumny lub wiele.

## <a name="convert-unix-timestamp-to-datetime"></a>Przekonwertować na typ DateTime sygnatury czasowej systemu Unix
Tej transformacji rozumie format sygnatury czasowej systemu Unix, nawet jeśli jest reprezentowany jako ciąg w danych i konwertuje ją poprawnie typu Data na przygotowanie bazy danych.

## <a name="filter"></a>Filtr
Tej transformacji obsługuje filtrowanie wierszy na podstawie wartości w jednej lub wielu kolumn. Warunki filtru są zależne od typu danych kolumn, co pozwala na kolumnach typu ciąg filtru "zawiera" lub "nie zawiera". Kolumny liczbowe można filtrować według "większe niż" "poniżej" warunki.

Gdy przekształcenie filtru jest wywoływana z poziomu menu głównego lub prawym przyciskiem myszy nagłówek kolumny, opcja rozwidlania wierszy niepowodzenie do innego przepływu danych jest dostępna. Przepływ danych głównych będzie kontynuowane przy użyciu filtrowane **w** wierszy i nowy przepływ danych utworzono, która zawiera wszystkie wiersze, które zostały przefiltrowane **limit**.

## <a name="use-first-row-as-headers"></a>Użyj pierwszego wiersza jako nagłówków
Tej transformacji zamienia pierwszy wiersz z zestawu danych jako nazwy kolumny. Jeśli niektóre kolumny nie ma danych w pierwszym wierszu, nazwę został wygenerowany automatycznie. Przy użyciu tej transformacji oznacza, że import danych zaczyna się w wierszu 2 co najmniej. W zależności od ustawienia wierszy pomijania importu może rozpocząć nawet dalsze w dół w zestawie danych. Może również służyć do usunięcia podwyższenia poziomu i mieć generowanych automatycznie nazw tylko.

## <a name="join"></a>Join
Tej transformacji jest używany do przyłączenia dwóch przepływów danych ze sobą. Można wybrać, które sprzężenie powinna zwracać wynik: pomyślnie wiersze z sprzężenia, "po lewej stronie" Niepowodzenie wiersze z sprzężenie lub "prawa" Niepowodzenie wiersze z sprzężenia.

Kreator sprzężenia jest uruchamiana z przepływem danych jednego i zaznacza ten przepływ danych po jednej stronie sprzężenia. Następnie monituje o podanie inny przepływ danych dla strony sprzężenia. Po wybraniu dwóch przepływów Kreator wymaga jednej kolumny na każdej stronie wspólnych można wybrać do połączenia. Jeśli sprzężenie wymaga więcej niż jedną kolumnę, Utwórz pochodnej kolumnę, przed uruchomieniem kreatora w celu utworzenia nowej kolumny (połączonych), można użyć tylko w celu utworzenia sprzężenia. Kreator podejmie próbę Sugeruj klucze sprzężenia i jeśli to możliwe automatycznie wygenerować kolumny pochodnej.

Po zakończeniu sprzężenia, widok diagramu Sankey sprzężenia jest widoczne. Szerokość linii względem siebie reprezentuje liczbę wierszy, Nawigacja po część przepływu sprzężenia. Panel po prawej stronie służy do wybierania pomyślne wiersze, po lewej stronie, które nie lub niepowodzeniem wyłącznie po prawej. Istnieje również możliwość wybrać tylko jedna gałąź.

## <a name="append-rows"></a>Dołącz wiersze
Tej transformacji dołącza dane z innego przepływu danych do bieżącej. Mapowania kolumn za pomocą pozycji, aby dodać nowe wiersze na końcu.

## <a name="append-columns"></a>Dołącz kolumny
Tej transformacji dołącza nowe kolumny z innej przepływ danych do bieżącej. Dodaje nowe kolumny do prawej; nie próbować wiersz w górę danych wierszy.

## <a name="summarize"></a>Podsumowanie
Tej transformacji oblicza agregacji dla kombinacji unikatowe wartości w co najmniej jeden z zaznaczonych kolumn. 

Agregacje, obsługiwane są: liczba, SUM, MIN, MAX, średnia, odchylenie i odchylenie standardowe. Lista wartości zagregowanych dla podanej kolumny są filtrowane tylko te, które są stosowane na typ danych. Wartości zagregowane, które nie są stosowane są wyłączone. Na przykład nie jest możliwe do obliczenia średniej kolumny typu string, ale istnieje możliwość obliczeniowe min i max.

Po udostępnieniu edytora, przeciągnij z nagłówka kolumny do panelu w lewej górnej części. gdzie są wyświetlane kolumny, które mają być łączone. Ten panel jest hierarchiczne, dlatego jest możliwe zagnieżdżonych wartości zagregowanych. Panel Edytor u góry, prawej służy do wybierania który agregacji do zastosowania do kolumny. Pojedyncza kolumna może agregować jeden lub więcej razy. Po co najmniej jedną wartość zagregowaną została wybrana, siatki w prawym dolnym rogu Wyświetla podgląd danych w postaci agregacji. 

Tej transformacji jest odpowiednikiem `ANSI-SQL GROUP BY`.

## <a name="remove-duplicates"></a>Usuń duplikaty
Tej transformacji usuwa cały wiersz, w którym występują zduplikowane wartości w jednej lub więcej wybranych kolumn. Jeśli nie kolumn są wybrane, nie można używać tylko wiersze, które zostały usunięte te, których wszystkie wartości w kolumnach są takie same.

## <a name="sort"></a>Sortuj
Dane są sortowane w tej transformacji. Sortowanie może odbywać się przez pojedynczą kolumnę lub wiele i każdej kolumny można sortować (ustawienie domyślne) rosnący lub malejący (można go zmienić w edytorze).

Tej transformacji jest odpowiednikiem `ANSI-SQL ORDER BY`.

## <a name="output-transforms"></a>Przekształcenia danych wyjściowych
Dane wyjściowe następujące przekształcenia. Użytkownik może mieć wiele bloków zapisu w jednym przepływie mógł zapisać danych w różnych punktach.

### <a name="write-to-csv"></a>Zapis do pliku CSV
Tej transformacji zapisuje dane w postaci CSV z bieżącego punktu w przepływie danych. Umożliwia kontrolę nad lokalizacji (lokalnym lub zdalnym) i różne ustawienia wokół pliku.

### <a name="write-to-parquet"></a>Zapisać Parquet
Tej transformacji zapisuje dane w postaci Parquet od bieżącego punktu w przepływie danych. Umożliwia kontrolę nad lokalizacji (lokalnym lub zdalnym) i różne ustawienia wokół pliku.

## <a name="script-based-transforms"></a>Skrypt na podstawie transformacji
Następujące przekształcenia użyć skryptu (Python) do wykonywania funkcji, które nie istnieje w produkcie podstawowym. Przed użyciem dowolnej z tych transformacje, przeczytaj [rozszerzalność przy użyciu języka Python](data-prep-python-extensibility-overview.md).

### <a name="add-column-script"></a>Dodaj kolumnę (skrypt)
Tej transformacji umożliwia kolumny mają zostać dodane do danych za pomocą wyrażenia języka Python.
Aby uzyskać więcej informacji, zobacz [Python przykładowy kod wyprowadzanie nowe kolumny](data-prep-appendix10-sample-custom-column-transforms-python.md).

### <a name="advanced-filter-script"></a>Filtru zaawansowanego (skrypt)
Tej transformacji umożliwia filtrze poziomu Python wierszy do zapisania.
Aby uzyskać więcej informacji, zobacz [przykładowe wyrażenia filtru](data-prep-appendix6-sample-filter-expressions-python.md).

### <a name="transform-dataflow-script"></a>Przekształć przepływu danych (skrypt)
Tej transformacji umożliwia Python, które mają być stosowane do całego zestawu danych.
Aby uzyskać więcej informacji, zobacz [przykład przekształcania danych przepływ przekształcenia](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="transform-dataflow-script"></a>Przekształć przepływu danych (skrypt)
Tej transformacji umożliwia Python ma zostać zastosowany do partycji danych.
Aby uzyskać więcej informacji, zobacz [przykład przekształcania danych przepływ przekształcenia](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="write-dataflow-script"></a>Pisanie przepływu danych (skrypt)
Tej transformacji umożliwia Python ma zostać zastosowany do zapisywania i cały zestaw danych.
Aby uzyskać więcej informacji, zobacz [Python próbki dla wynikających z nowymi kolumnami](data-prep-appendix9-sample-destination-connections-python.md).



