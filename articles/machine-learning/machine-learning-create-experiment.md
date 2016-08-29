<properties
    pageTitle="Tworzenie prostego eksperymentu w usłudze Machine Learning Studio | Microsoft Azure"
    description="Pierwszy samouczek dotyczący uczenia maszynowego obejmujący tworzenie prostego eksperymentu w usłudze Azure Machine Learning Studio w celu nauczenia i przetestowania modelu regresji liniowej."
    keywords="experiment,linear regression,machine learning algorithms,machine learning tutorial,predictive modeling techniques"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="03/09/2016"
    ms.author="garye"/>

# Samouczek dotyczący uczenia maszynowego: tworzenie pierwszego eksperymentu w usłudze Azure Machine Learning Studio

W tym pierwszym samouczku dotyczącym uczenia maszynowego utworzymy model regresji liniowej, który prognozuje cenę samochodów na podstawie różnych zmiennych, takich jak marka i specyfikacja techniczna. W tym celu użyjemy usługi Azure Machine Learning Studio, aby opracować prosty eksperyment korzystający z analizy predykcyjnej oraz wykonać jego iterację.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Przeprowadzenie eksperymentu w usłudze Machine Learning Studio polega na przeciągnięciu składników do obszaru roboczego i połączeniu ich w celu *utworzenia*, *nauczenia* *i przetestowania modelu po wygenerowaniu wyników*. W tym eksperymencie są używane techniki modelowania predykcyjnego dostępne w formie modułów usługi Machine Learning Studio, które umożliwiają pobieranie danych, uczenie modelu przy użyciu tych danych oraz zastosowanie tego modelu do nowych danych. Można również dodawać moduły w celu wstępnego przetworzenia danych i wybrania cech, podzielenia danych na zestaw szkoleniowy i testowy oraz ocenienia lub sprawdzenia krzyżowego jakości modelu.  

Otwórz usługę Machine Learning Studio: [https://studio.azureml.net](https://studio.azureml.net) i kliknij przycisk **Get started** (Rozpoczęcie pracy). Możesz wybrać opcję dostępu dla gości lub zalogować się za pomocą konta Microsoft.

Aby uzyskać ogólne informacje o usłudze Machine Learning Studio, zobacz [What is Machine Learning Studio?](machine-learning-what-is-ml-studio.md) (Czym jest Machine Learning Studio?).

>[AZURE.TIP] Aby pobrać i wydrukować diagram, który zawiera przegląd możliwości usługi Machine Learning Studio, zobacz [Diagram przeglądowy możliwości usługi Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md).


## Tworzenie eksperymentu w pięciu krokach

W tym samouczku dotyczącym uczenia maszynowego wykonamy pięć podstawowych kroków, aby uruchomić eksperyment w usłudze Machine Learning Studio obejmujący tworzenie i uczenie modelu oraz generowanie wyników:

- Tworzenie modelu
    - [Krok 1. Pobieranie danych]
    - [Krok 2. Przetwarzanie wstępne danych]
    - [Krok 3. Definiowanie cech]
- Uczenie modelu
    - [Krok 4. Wybieranie i stosowanie algorytmu uczenia]
- Generowanie wyników i testowanie modelu
    - [Krok 5. Przewidywanie nowych cen samochodów]

[Krok 1. Pobieranie danych]: #step-1-get-data
[Krok 2. Przetwarzanie wstępne danych]: #step-2-preprocess-data
[Krok 3. Definiowanie cech]: #step-3-define-features
[Krok 4. Wybieranie i stosowanie algorytmu uczenia]: #step-4-choose-and-apply-a-learning-algorithm
[Krok 5. Przewidywanie nowych cen samochodów]: #step-5-predict-new-automobile-prices


## Krok 1. Pobieranie danych

Usługa Machine Learning Studio udostępnia wiele przykładowych zestawów danych do wyboru, a dane można importować z wielu źródeł. W tym scenariuszu będziemy używać dołączonego przykładowego zestawu danych **Automobile price data (Raw)** (Nieprzetworzone dane z cenami samochodów).
Ten zestaw zawiera dane szeregu modeli samochodów, na przykład informacje dotyczące marki, ceny czy specyfikacji technicznej.

1. Uruchom nowy eksperyment, klikając pozycję **+NEW** (+NOWY) u dołu okna Machine Learning Studio i wybierz kolejno pozycje **EXPERIMENT** (EKSPERYMENT), **Blank Experiment** (Pusty eksperyment). Wybierz domyślną nazwę eksperymentu w górnej części obszaru roboczego i zmień ją, wpisując tekst opisowy, na przykład **Prognozowanie cen samochodów**.

2. Z lewej strony obszaru roboczego eksperymentu znajduje się paleta zawierająca zestawy danych i moduły. Wpisz **automobile** (samochód) w polu wyszukiwania w górnej części tej palety, aby znaleźć zestaw danych z etykietą **Automobile price data (Raw)** (Nieprzetworzone dane z cenami samochodów).

    ![Wyszukiwanie na palecie][screen1a]

3. Przeciągnij zestaw danych do obszaru roboczego eksperymentu.

    ![Zestaw danych][screen1]

Aby wyświetlić graficzną reprezentację danych, kliknij port wyjściowy w dolnej części zestawu danych dotyczących samochodów, a następnie wybierz pozycję **Visualize** (Wizualizacja). Zmienne w zestawie danych są wyświetlane jako kolumny, a poszczególne wystąpienia modeli samochodów są wyświetlane w postaci wierszy. Skrajna prawa kolumna (kolumna 26 zatytułowana „price” [cena]) zawiera zmienną docelową, którą spróbujemy przewidzieć.

![Wizualizacja zestawu danych][screen1b]

Aby zamknąć okno wizualizacji, kliknij znak „**x**” w prawym górnym rogu.

## Krok 2. Przetwarzanie wstępne danych

Zestawy danych zwykle wymagają przetworzenia wstępnego przed rozpoczęciem analizy. Zwróć uwagę na to, że w przypadku niektórych wierszy w kolumnach brakuje wartości. Te brakujące wartości muszą zostać wyczyszczone, aby umożliwić modelowi wykonanie poprawnej analizy danych. W naszym przykładzie usuniemy wszystkie wiersze z brakującymi wartościami. Ponadto w kolumnie **normalized-losses** (znormalizowane straty) występuje wiele przypadków brakujących wartości, dlatego całkowicie wykluczymy tę kolumnę z modelu.

> [AZURE.TIP] Większość modułów wymaga wyczyszczenia brakujących wartości w danych wejściowych.

Najpierw usuniemy kolumnę **normalized-losses** (znormalizowane straty), a następnie usuniemy wszystkie wiersze z brakującymi danymi.

1. Wpisz **select columns** (wybieranie kolumn) w polu wyszukiwania w górnej części palety modułów, aby znaleźć moduł [Select Columns in Dataset][select-columns] (Wybieranie kolumn w zestawie danych), a następnie przeciągnij go do obszaru roboczego eksperymentu i połącz z portem wyjściowym zestawu danych **Automobile price data (Raw)** (Nieprzetworzone dane z cenami samochodów). Ten moduł pozwala wybierać kolumny danych, które mają zostać dołączone do modelu lub wykluczone z niego.

2. Wybierz moduł [Select Columns in Dataset][select-columns] (Wybieranie kolumn w zestawie danych) i kliknij pozycję **Launch column selector** (Uruchom selektora kolumn) w okienku **Properties** (Właściwości).

    - Upewnij się, że na liście rozwijanej filtru **Begin With** (Rozpocznij od) wybrano pozycję **All columns** (Wszystkie kolumny). Spowoduje to przetworzenie wszystkich kolumn (z wyjątkiem tych wykluczonych) przez moduł [Select Columns in Dataset][select-columns] (Wybieranie kolumn w zestawie danych).
    - W kolejnym wierszu wybierz z list pozycje **Exclude** (Wyklucz) i **column names** (nazwy kolumn), a następnie kliknij wewnątrz pola tekstowego. Zostanie wyświetlona lista kolumn. Wybierz pozycję **normalized-losses** (znormalizowane straty), aby dodać ją do pola tekstowego.
    - Kliknij przycisk znacznika wyboru (OK), aby zamknąć selektora kolumn.

    ![Wybieranie kolumn][screen3]

    Zawartość okienka właściwości modułu **Select Columns in Dataset** (Wybieranie kolumn w zestawie danych) określa, że zostaną przetworzone wszystkie kolumny zestawu danych z wyjątkiem kolumny **normalized-losses** (znormalizowane straty).

    ![Właściwości modułu Select Columns in Dataset (Wybieranie kolumn w zestawie danych)][screen4]

    > [AZURE.TIP] Aby dodać komentarz do modułu, kliknij dwukrotnie moduł i wpisz tekst. Pozwoli to od razu sprawdzić rolę modułu w eksperymencie. W tym przypadku kliknij dwukrotnie moduł [Select Columns in Dataset][select-columns] (Wybieranie kolumn w zestawie danych) i dodaj komentarz „Wykluczenie kolumny znormalizowane straty”.

3. Przeciągnij moduł [Clean Missing Data][clean-missing-data] (Czyszczenie brakujących danych) do obszaru roboczego eksperymentu i połącz go z modułem [Select Columns in Dataset][select-columns] (Wybieranie kolumn w zestawie danych). W okienku **Properties** (Właściwości) w obszarze **Cleaning mode** (Tryb czyszczenia) wybierz pozycję **Remove entire row** (Usuń cały wiersz), aby wyczyścić dane przez usunięcie wierszy z brakującymi wartościami. Kliknij dwukrotnie moduł i wpisz komentarz „Usunięcie wierszy z brakującymi wartościami”.

    ![Właściwości modułu Clean Missing Data (Czyszczenie brakujących danych)][screen4a]

4. Uruchom eksperyment, klikając pozycję **RUN** (URUCHOM) pod obszarem roboczym.

Po zakończeniu eksperymentu na wszystkich modułach są widoczne zielone znaczniki wyboru. Oznacza to, że działanie modułów zakończyło się pomyślnie. Zwróć również uwagę na informację o **zakończeniu działania** wyświetlaną w prawym górnym rogu.

![Pierwsze uruchomienie eksperymentu][screen5]

Na tym etapie nasz eksperyment obejmuje tylko czyszczenie danych. Jeśli chcesz wyświetlić oczyszczony zestaw danych, kliknij lewy port wyjściowy modułu [Clean Missing Data][clean-missing-data] (Czyszczenie brakujących danych) („Oczyszczony zestaw danych”) i wybierz pozycję **Visualize** (Wizualizacja). Zwróć uwagę na to, że kolumna **normalized-losses** (znormalizowane straty) nie jest już uwzględniana i nie ma brakujących wartości.

Po oczyszczeniu danych można określić, jakie cechy zostaną użyte w modelu predykcyjnym.

## Krok 3. Definiowanie cech

W uczeniu maszynowym *cechy* to poszczególne mierzalne właściwości określonych informacji. W naszym zestawie danych poszczególne wiersze odpowiadają różnym samochodom, a kolumny — cechom tych samochodów. Znalezienie odpowiedniego zestawu cech, który ma służyć do utworzenia modelu predykcyjnego, wymaga eksperymentowania oraz dysponowania wiedzą na temat bieżącego problemu. Pewne cechy lepiej nadają się do prognozowania danych docelowych. Ponadto niektóre cechy są ściśle powiązane z innymi (na przykład zużycie paliwa w mieście i w trasie), co sprawia, że nie wnoszą one do modelu wielu nowych informacji i dlatego można je usunąć.

Utworzymy model, który korzysta z podzbioru cech zawartych w naszym zestawie danych. W poszukiwaniu lepszych wyników można uruchamiać eksperymenty oparte na różnych podzbiorach cech. Za pierwszym razem wybierzemy następujące cechy (kolumny) za pomocą modułu [Select Columns in Dataset][select-columns] (Wybieranie kolumn w zestawie danych). Pamiętaj o tym, że na potrzeby uczenia modelu musimy dołączyć wartość *price* (cena), którą chcemy przewidzieć.

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price

1. Przeciągnij kolejny moduł [Select Columns in Dataset][select-columns] (Wybieranie kolumn w zestawie danych) do obszaru roboczego eksperymentu i połącz go z lewym portem wyjściowym modułu [Clean Missing Data][clean-missing-data] (Czyszczenie brakujących danych). Kliknij dwukrotnie moduł i wpisz „Wybieranie cech w celu prognozowania”.

2. Kliknij pozycję **Launch column selector** (Uruchom selektora kolumn) w okienku **Properties** (Właściwości).

3. W selektorze kolumn wybierz pozycję **No columns** (Brak kolumn) na liście **Begin With** (Rozpocznij od), a następnie wybierz pozycje **Include** (Dołącz) i **column names** (nazwy kolumn) w wierszu filtru. Wpisz nazwy kolumn z listy. Dzięki temu moduł będzie przetwarzał tylko określone kolumny.

    > [AZURE.TIP] Ponieważ eksperyment został wcześniej uruchomiony, definicje kolumn dla danych zostały przekazane z oryginalnego zestawu danych za pośrednictwem modułu [Clean Missing Data][clean-missing-data] (Czyszczenie brakujących danych). Po połączeniu modułu [Select Columns in Dataset][select-columns] (Wybieranie kolumn w zestawie danych) z modułem [Clean Missing Data][clean-missing-data] (Czyszczenie brakujących danych) moduł [Select Columns in Dataset][select-columns] (Wybieranie kolumn w zestawie danych) uzyska informacje o definicjach kolumn. Po kliknięciu pola **column names** (nazwy kolumn) zostanie wyświetlona lista kolumn, z której można wybrać kolumny w celu ich dodania.

4. Kliknij przycisk znacznika wyboru (OK).

![Wybieranie kolumn][screen6]

Wykonanie tych czynności spowoduje utworzenie zestawu danych, który będzie używany w algorytmie uczenia w następnych krokach. Możesz później wrócić do tego kroku i wybrać inny zbiór cech.

## Krok 4. Wybieranie i stosowanie algorytmu uczenia

Po przygotowaniu danych można przystąpić do konstruowania modelu predykcyjnego, co obejmuje uczenie i testowanie. Użyjemy danych do nauczenia modelu, a następnie przetestujemy go, aby sprawdzić dokładność przewidywanych cen.

Techniki *klasyfikacji* i *regresji* to dwa typy nadzorowanego uczenia maszynowego. Klasyfikacja umożliwia prognozowanie na podstawie zdefiniowanego zestawu wartości, takich jak kolory (czerwony, niebieski lub zielony). Regresja służy do prognozowania na podstawie ciągłego zestawu wartości, na przykład informacji o wieku osób.

Chcemy przewidzieć cenę samochodu, która może być dowolną wartością, dlatego użyjemy modelu regresji. W tym przykładzie nauczymy prosty model *regresji liniowej*, a następnie przetestujemy go.

1. Dane możemy podzielić na dwa oddzielne zestawy, aby użyć ich do celów szkoleniowych i do testów. Wybierz moduł [Split Data][split] (Podział danych) i przeciągnij go do obszaru roboczego eksperymentu, a następnie połącz go z wyjściem ostatniego modułu [Select Columns in Dataset][select-columns] (Wybieranie kolumn w zestawie danych). Ustaw opcję **Fraction of rows in the first output dataset** (Odsetek wierszy w pierwszym zestawie danych wyjściowych) na wartość 0,75. Dzięki temu 75% danych zostanie użytych do nauczenia modelu, a pozostałe 25% do testów.

    > [AZURE.TIP] Zmieniając wartość parametru **Random seed** (Inicjator losowy) można uzyskać różne próbki losowe do celów szkoleniowych i testów. Ten parametr umożliwia sterowanie inicjacją pseudolosowego generatora liczb.

2. Uruchom eksperyment. Pozwoli to modułom [Select Columns in Dataset][select-columns] (Wybieranie kolumn w zestawie danych) i [Split Data][split] (Podział danych) przekazać definicje kolumn do modułów, które będą dodawane później.  

3. Aby wybrać algorytm uczenia, rozwiń kategorię **Machine Learning** (Uczenie maszynowe) na palecie modułów wyświetlanej z lewej strony obszaru roboczego, a następnie rozwiń węzeł **Initialize Model** (Inicjacja modelu). Zostaną wyświetlone różne kategorie modułów, których można użyć do zainicjowania algorytmów uczenia maszynowego.

    W tym eksperymencie wybierz moduł [Linear Regression][linear-regression] (Regresja liniowa) z kategorii **Regression** (Regresja) (możesz również znaleźć go, wpisując „linear regression” [regresja liniowa] w polu wyszukiwania palety) i przeciągnij go do obszaru roboczego eksperymentu.

4. Znajdź moduł [Train Model][train-model] (Uczenie modelu) i przeciągnij go do obszaru roboczego eksperymentu. Połącz lewy port wejściowy z wyjściem modułu [Linear Regression][linear-regression] (Regresja liniowa). Połącz prawy port wejściowy z danymi wyjściowymi uczenia (lewy port) modułu [Split Data][split] (Podział danych).

5. Wybierz moduł [Train Model][train-model] (Uczenie modelu), kliknij pozycję **Launch column selector** (Uruchom selektora kolumn) w okienku **Properties** (Właściwości), a następnie wybierz kolumnę **price** (cena). Jest to wartość, która będzie prognozowana przez nasz model.

    ![Wybieranie kolumny „price” (cena)][screen7]

6. Uruchom eksperyment.

W efekcie powstał nauczony model regresji, który może służyć do generowania wyników na podstawie próbek w celu prognozowania wartości.

![Stosowanie algorytmu uczenia maszynowego][screen8]

## Krok 5. Przewidywanie nowych cen samochodów

Gdy udało się nauczyć model przy użyciu 75% danych, można wygenerować wyniki dla pozostałych 25% danych, aby sprawdzić poprawność funkcjonowania modelu.

1. Znajdź moduł [Score Model][score-model] (Generowanie wyników przez model) i przeciągnij go do obszaru roboczego eksperymentu, a następnie połącz lewy port wejściowy z danymi wyjściowymi modułu [Train Model][train-model] (Uczenie modelu). Połącz prawy port wejściowy z danymi wyjściowymi testów (prawy port) modułu [Split Data][split] (Podział danych).  

    ![Moduł Score Model (Generowanie wyników przez model)][screen8a]

2. Aby uruchomić eksperyment i wyświetlić dane wyjściowe z modułu [Score Model][score-model] (Generowanie wyników przez model), kliknij port wyjściowy i wybierz pozycję **Visualize** (Wizualizacja). Dane wyjściowe zawierają przewidywane wartości cen oraz znane wartości pochodzące z danych testowych.  

3. Aby na koniec przetestować jakość wyników, wybierz moduł [Evaluate Model][evaluate-model] (Ocena modelu) i przeciągnij go do obszaru roboczego eksperymentu, a następnie połącz lewy port wejściowy z danymi wyjściowymi modułu [Score Model][score-model] (Generowanie wyników przez model). (Dostępne są dwa porty wejściowe, ponieważ moduł [Evaluate Model][evaluate-model] (Ocena modelu) może służyć do porównywania dwóch modeli).

4. Uruchom eksperyment.

Aby wyświetlić dane wyjściowe z modułu [Evaluate Model][evaluate-model] (Ocena modelu), kliknij port wyjściowy i wybierz pozycję **Visualize** (Wizualizacja). Wyświetlane są następujące statystyki dla modelu:

- **Średni bezwzględny błąd** (MAE, Mean Absolute Error): wartość średnia bezwzględnych błędów (*błąd* odpowiada różnicy między wartością prognozowaną a wartością rzeczywistą).
- **Pierwiastek błędu średniokwadratowego** (RMSE, Root Mean Squared Error): pierwiastek kwadratowy ze średniej kwadratów błędów prognoz dla zestawu danych testowych.
- **Względny błąd absolutny**: iloraz średniej błędów absolutnych i bezwzględnej wartości różnicy między wartościami rzeczywistymi a średnią wszystkich wartości rzeczywistych.
- **Błąd względny średniokwadratowy**: iloraz średniej kwadratów błędów i kwadratu różnicy między wartościami rzeczywistymi a średnią wszystkich wartości rzeczywistych.
- **Współczynnik determinacji**: znany także jako **wartość R-kwadrat** jest miarą statystyczną jakości dopasowania modelu do danych.

W przypadku wszystkich powyższych statystyk mniejsze wartości oznaczają lepszą jakość modelu. Mniejsze wartości błędów wskazują na ściślejsze dopasowanie prognoz do rzeczywistych wartości. W przypadku **współczynnika determinacji** prognozy są tym lepsze, im jego wartość jest bliższa jedności (1,0).

![Wyniki oceny][screen9]

Końcowy eksperyment powinien wyglądać następująco:

![Samouczek dotyczący uczenia maszynowego: ukończenie eksperymentu opartego na regresji liniowej z użyciem technik modelowania predykcyjnego.][screen10]

## Następne kroki

Po ukończeniu pierwszego samouczka dotyczącego uczenia maszynowego można wykonać iterację skonfigurowanego eksperymentu w celu ulepszenia modelu. Na przykład można zmienić cechy używane do prognozowania. Można też zmodyfikować właściwości algorytmu [regresji liniowej][linear-regression] lub użyć całkowicie innego algorytmu. Można również dodać do eksperymentu wiele algorytmów uczenia maszynowego i porównać dwa z nich przy użyciu modułu [Evaluate Model][evaluate-model] (Ocena modelu).

> [AZURE.TIP] Za pomocą przycisku **SAVE AS** (ZAPISZ JAKO) wyświetlanego pod obszarem roboczym można skopiować dowolną iterację eksperymentu. Aby wyświetlić wszystkie iteracje eksperymentu, kliknij pozycję **VIEW RUN HISTORY** (WYŚWIETL HISTORIĘ URUCHAMIANIA) pod obszarem roboczym. Aby uzyskać bardziej szczegółowe informacje, zobacz [Manage experiment iterations in Azure Machine Learning Studio][runhistory] (Zarządzanie iteracjami eksperymentów w usłudze Azure Machine Learning Studio).

[runhistory]: machine-learning-manage-experiment-iterations.md

Jeśli model spełnia Twoje oczekiwania, możesz go wdrożyć jako usługę sieci Web i używać jej do prognozowania cen samochodów na podstawie nowych danych. Aby uzyskać dodatkowe informacje, zobacz [Deploy an Azure Machine Learning web service][publish] (Wdrażanie usługi sieci Web Azure Machine Learning).

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Aby zapoznać się z bardziej rozbudowanym i szczegółowym przewodnikiem po technikach modelowania predykcyjnego obejmujących tworzenie, uczenie i wdrażanie modelu po wygenerowaniu wyników, zobacz [Develop a predictive solution by using Azure Machine Learning][walkthrough] (Opracowywanie rozwiązania predykcyjnego przy użyciu usługi Azure Machine Learning).

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]:./media/machine-learning-create-experiment/screen1.png
[screen1a]:./media/machine-learning-create-experiment/screen1a.png
[screen1b]:./media/machine-learning-create-experiment/screen1b.png
[screen2]:./media/machine-learning-create-experiment/screen2.png
[screen3]:./media/machine-learning-create-experiment/screen3.png
[screen4]:./media/machine-learning-create-experiment/screen4.png
[screen4a]:./media/machine-learning-create-experiment/screen4a.png
[screen5]:./media/machine-learning-create-experiment/screen5.png
[screen6]:./media/machine-learning-create-experiment/screen6.png
[screen7]:./media/machine-learning-create-experiment/screen7.png
[screen8]:./media/machine-learning-create-experiment/screen8.png
[screen8a]:./media/machine-learning-create-experiment/screen8a.png
[screen9]:./media/machine-learning-create-experiment/screen9.png
[screen10]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/



<!----HONumber=Jun16_HO2-->


