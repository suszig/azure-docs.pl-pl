---
title: "Inżynieria w nauce danych | Dokumentacja firmy Microsoft"
description: "Zawiera opis na potrzeby funkcji inżynierii oraz przykłady swoją rolę w procesie rozszerzenie danych usługi machine learning."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 3fde69e8-5e7b-49ad-b3fb-ab8ef6503a4d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: zhangya;bradsev
ms.openlocfilehash: c0933fec67bbcfc2a8c450c1e2472440cf38f338
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="feature-engineering-in-data-science"></a>Inżynieria w nauce danych
W tym temacie opisano na potrzeby funkcji engineering i zawiera przykłady swoją rolę w procesie rozszerzenie danych usługi machine learning. Przykłady użyta w celu przedstawienia tego procesu są pobierane z usługi Azure Machine Learning Studio. 

[!INCLUDE [cap-create-features-data-selector](../../../includes/cap-create-features-selector.md)]

To **menu** linki do tematów opisujących sposób tworzenia funkcji dla danych w różnych środowiskach. To zadanie jest krokiem w [zespołu danych nauki procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Funkcja engineering próby zwiększyć możliwości predykcyjnej algorytmów uczenia przez tworzenie funkcji z danych pierwotnych ułatwić proces uczenia. Inżynieryjne i wybór funkcji jest częścią TDSP opisane w temacie [co to jest cyklu życia procesu nauki danych zespołu?](overview.md) Funkcja inżynieryjne i wyboru są częściami **opracowywania funkcji** krok TDSP. 

* **Inżynieria**: ten proces próbuje utworzyć dodatkowe istotne cechy z istniejących funkcji pierwotnych danych i zwiększyć możliwości predykcyjnej Algorytm uczenia.
* **Wybór funkcji**: tego procesu wybiera klucza podzbioru cech oryginalnych danych w celu zmniejszenia wymiarach problemu szkolenia.

Zwykle **Inżynieria** zostanie zastosowana jako pierwsza, aby wygenerować dodatkowe funkcje, a następnie **funkcji wyboru** kroku jest wykonywana w celu wyeliminowania nie ma znaczenia, nadmiarowe lub dużej skorelowane funkcji.

Dane szkoleniowe używane w uczeniu maszynowym wspomagają często wyodrębniania funkcji w zebranych danych pierwotnych. Przykład funkcji odtworzone w kontekście obrazów znaków odręcznie klasyfikowania uczenia jest tworzenie nieco mapy gęstość zbudowane z bitowego pierwotnych danych dystrybucji. Ta mapa można lokalizowania krawędzi znaki efektywniej niż po prostu bezpośrednio przy użyciu rozkładu raw.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="creating-features-from-your-data---feature-engineering"></a>Tworzenie funkcji z danych - Inżynieria
Dane szkoleniowe składa się z macierzy składa się z przykładami (rekordy lub obserwacji przechowywane w wierszach), z których każda ma zestaw funkcji (zmienne lub pól przechowywane w kolumnach). Oczekiwano funkcji określony w projekcie doświadczenia charakteryzujących wzorce w danych. Mimo że wiele pól danych pierwotnych mogą być bezpośrednio uwzględnione w wybranej funkcji używany do nauczenia modelu, często jest przypadek dodatkowe funkcje (odtworzone) muszą zostać utworzone na podstawie funkcje nieprzetworzone dane można wygenerować obiektu dataset rozszerzone szkolenia.

Jakiego rodzaju funkcje należy utworzyć w celu zwiększenia zestawu danych podczas uczenia modelu? Odtworzone funkcji, które zwiększają szkolenia dostarczają informacji lepiej odróżniającą wzorce w danych. Oczekuje nowe funkcje, które zapewniają dodatkowe informacje, które nie jest wyraźnie przechwycony lub łatwo widoczna w zestawie funkcji oryginalny lub istniejącego. Jednak ten proces jest coś grafiki. Decyzje dźwięku i produktywności często wymagają doświadczenia z niektórych domen.

Począwszy od usługi Azure Machine Learning, najłatwiej ujmij ten proces, konkretnie przy użyciu przykłady podane w Studio. Poniżej przedstawiono dwa przykłady:

* Przykład regresji [prognozowanie liczby dzierżawy roweru](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) w nadzorowanym eksperymentu, gdy wiadomo, że wartości docelowej
* Using przykład klasyfikacji wyszukiwania tekstu [Tworzenie skrótu funkcji](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/)

## <a name="example-1-adding-temporal-features-for-regression-model"></a>Przykład 1: Dodawanie funkcji danych czasowych dla modelu regresji
Użyjmy eksperymentu "żądanie Prognozowanie rowerów" w usłudze Azure Machine Learning Studio aby zademonstrować sposób opracowywać funkcje zadania regresji. Celem tego eksperymentu jest do prognozowania popytu na rowerów, oznacza to, że liczba roweru dzierżawy w ramach określonej miesiąc/dzień/godziny. Zestaw danych "dzierżawa roweru UCI zestawu danych" jest używana jako nieprzetworzone dane wejściowe. Ten zestaw danych jest oparty na prawdziwe dane z firmy Bikeshare kapitału, która obsługuje sieci wynajem roweru w stanie Waszyngton DC w Stanach Zjednoczonych. Zestaw danych reprezentuje liczbę roweru dzierżawy w ramach określoną godzinę dnia w latach i 2011 roku 2012 i zawiera 17379 wierszy i kolumn 17. Zestaw funkcji raw zawiera pogodą (knie temperatury/wilgotności szybkość) i typ dzień (dni wolnych/dzień tygodnia). Pole do prognozowania jest "cnt", liczba reprezentuje dzierżawy roweru w ciągu godziny określonych i które zakresów z zakresu od 1 do 977.

W celu tworzenia skuteczne funkcje dane szkoleniowe cztery regresji, modeli są tworzone za pomocą tego samego algorytmu, ale z czterech różnych szkolenia zestawami danych. Cztery zestawy danych reprezentują tego samego pierwotnych danych wejściowych, ale z coraz więcej funkcji ustawiona. Te funkcje są podzielone na kategorie cztery:

1. A = pogody + świątecznych dzień tygodnia + weekendowe funkcje przewidywane dzień
2. B = liczba rowerów, które zostały dzierżawione w każdym z ostatnich 12 godzin
3. C = Liczba rowerów, które zostały dzierżawione w każdym z ostatnich 12 dni na tę samą godzinę
4. D = liczba rowerów, które zostały dzierżawione w każdym z poprzednich 12 tygodni na tę samą godzinę i tego samego dnia

Oprócz A zestaw funkcji, które już istnieją w oryginalnej danych pierwotnych, trzy inne zestawy funkcji są tworzone za pomocą funkcji inżynierii procesu. Zestaw funkcji przechwytywania B bardzo ostatnie żądanie dla rowerów. Skonfigurować funkcji przechwytywania C popytu na rowerów o określonej godzinie. Funkcja ustawiona D przechwytywania żądanie dla rowerów w szczególności godziny i dnia tygodnia. Szkolenie cztery zestawy danych każdy zawiera odpowiednio A zestaw funkcji, A + B, A + B + C i A + B + C + D.

W eksperymencie usługi Azure Machine Learning te cztery zestawy danych szkoleniowych są tworzone za pomocą czterech oddziałów z wstępnie przetworzonych wejściowego zestawu danych. Z wyjątkiem po lewej stronie większości gałęzi, każdy z tymi gałęziami zawiera [wykonanie skryptu języka R](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) modułu, w którym zestaw funkcji pochodnych (funkcja ustawiona B, C i D), są odpowiednio zbudowane i dołączona do importowanych obiektów dataset. Na poniższym rysunku pokazano skrypt języka R, użyty do utworzenia zestawu funkcji B w drugim gałęzi po lewej stronie.

![Tworzenie funkcji](./media/create-features/addFeature-Rscripts.png)

Porównanie wyników wydajności cztery modele podsumowano w poniższej tabeli. Najlepsze wyniki są wyświetlane przez funkcje A + B + C. Współczynnik błędów zmniejsza, kiedy Uwaga zestaw dodatkowych funkcji znajdują się w danych szkoleniowych. Sprawdza naszych założenie, że zestaw funkcji B, C zapewniają dodatkowe istotne informacje o zadaniu regresji. Ale dodawanie funkcji D nie wydaje się Podaj wszelkie dodatkowe zmniejszenie współczynnik błędów.

![wynik porównania](./media/create-features/result1.png)

## <a name="example2"></a>Przykład 2: Tworzenie funkcji wyszukiwania tekstu
Funkcja engineering jest powszechnie stosowane w zadania związane z wyszukiwania tekstu, takich jak analiza dokumentu klasyfikacji i wskaźniki nastrojów klientów. Na przykład gdy chcemy, aby klasyfikować dokumenty na kilka kategorii, typowe założeń jest możliwość word fraz zawarte w jednej kategorii doc mniej prawdopodobne w innej kategorii dokumentu. W innym słowa częstotliwość dystrybucji słowa/fraz jest w stanie scharakteryzowania kategoriami innego dokumentu. W aplikacjach wyszukiwania tekstu ponieważ pojedynczych zawartość tekstową zwykle służą jako dane wejściowe funkcji inżynierii procesu jest potrzebne do utworzenia funkcje dotyczące częstotliwości word/frazę.

Do wykonania tego zadania, nazywany technika **Tworzenie skrótu funkcji** jest stosowany do wydajnie Włączanie funkcji dowolnego tekstu do indeksów. Zamiast kojarzenie każdej funkcji tekst (słowa/wyrażenia) do konkretnego indeksu, to funkcje — metoda stosowania funkcji skrótu do funkcji i używając ich wartości skrótu jako indeksy bezpośrednio.

W usłudze Azure Machine Learning [Tworzenie skrótu funkcji](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) moduł, który tworzy te word/frazy funkcje wygodny sposób. Poniższej ilustracji przedstawiono przykład za pomocą tego modułu. Wejściowy zestaw danych zawiera dwie kolumny: Klasyfikacja książki zakresu od 1 do 5 i przeglądu rzeczywistej zawartości. Celem tego [Tworzenie skrótu funkcji](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) modułu jest pobrać licznych nowe funkcje przedstawiające częstotliwość wystąpienie odpowiedniej wyrazy / Przejrzyj wyrażenia w ramach wybranej książki. Aby użyć tego modułu, należy wykonać następujące czynności:

* Najpierw wybierz kolumnę, która zawiera tekst wejściowy ("Col2" w tym przykładzie).
* Po drugie, ustaw "Hashing bitsize" na 8, co oznacza, że 2 ^ 8 = 256 funkcje zostaną utworzone. Word/fazie cały tekst zostanie wartość skrótu, aby 256 indeksów. Parametr "Hashing bitsize" zakresu od 1 do 31. Wyrazy / wyrażenia jest mniej prawdopodobne skrótu do tego samego indeksu, jeśli zostanie ustawiona na większą liczbę.
* Trzecie Ustaw parametr "N-gramów" do 2. Częstotliwość występowania unigrams (funkcja dla każdego pojedynczego wyrazu) i bigrams (funkcja dla każdej pary słów sąsiadujące) to pobiera z teksu wejściowego. Parametr "N-gramów" zakresu od 0 do 10, który wskazuje maksymalną liczbę kolejnych słowa do uwzględnienia w funkcji.  

![Moduł "funkcji Hashing"](./media/create-features/feature-Hashing1.png)

Na poniższej ilustracji przedstawiono co tych nowych funkcji wyglądu np.

![Przykład "funkcji Hashing"](./media/create-features/feature-Hashing2.png)

## <a name="conclusion"></a>Podsumowanie
Odtworzone i wybranych funkcji zwiększyć wydajność procesu szkolenia, która podejmuje próbę wyodrębnić klucza informacje zawarte w danych. Poprawiają również uprawnienia tych modeli dokładnie klasyfikowania danych wejściowych oraz do przewidywania wyników odsetek bardziej niezawodnie. Funkcja inżynieryjne i wyboru można także połączyć dokonanie więcej praktyce tractable learning. Robi to udoskonalanie, a następnie zmniejszenie liczby potrzeby kalibrować lub uczenie modelu przy użyciu funkcji. Funkcje wybrane do nauczenia modelu ze sobą matematycznie rzecz biorąc, są minimalny zbiór zmienne niezależne, które wyjaśniają wzorce w danych, a następnie pomyślnie przewidywania wyników.

Należy pamiętać, że nie zawsze musi przeprowadzić wybór funkcji engineering lub funkcji. Określa, czy jest potrzebna, lub nie zależy od tego, firma Microsoft ma lub zbierać dane, algorytmu, który mamy pobranie i celem eksperymentu.

