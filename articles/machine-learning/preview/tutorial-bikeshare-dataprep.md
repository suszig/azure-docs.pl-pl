---
title: "Samouczek udziału roweru — przygotowanie zaawansowanych danych z usługi Azure Machine Learning Workbench"
description: "I danych na trasie przygotowania samouczka przy użyciu usługi Azure Machine Learning Workbench"
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial, azure
ms.topic: article
ms.date: 09/21/2017
ms.openlocfilehash: 6d1845e27c6b0fff66b80a683f59d14238e2ad71
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/13/2017
---
# <a name="bike-share-tutorial-advanced-data-preparation-with-azure-machine-learning-workbench"></a>Samouczek udziału roweru: Przygotowanie danych z usługi Azure Machine Learning Workbench zaawansowane
Usługi uczenie maszynowe Azure (wersja zapoznawcza) jest nauki zintegrowane, end-to-end danych i zaawansowane metody analizy rozwiązania dla analityków danych professional Przygotowanie danych i tworzenie eksperymentów wdrażanie modeli w skali chmury.

W tym samouczku używasz usługi uczenie maszynowe Azure (wersja zapoznawcza), aby dowiedzieć się jak:
> [!div class="checklist"]
> * Przygotowanie danych interaktywnie za pomocą narzędzia Azure Machine Learning danych przygotowania
> * Importuj, przekształcanie i utworzyć zestawu danych testowych
> * Generowanie pakietu Przygotowanie danych
> * Uruchom pakiet przygotowywania danych przy użyciu języka Python
> * Wygeneruj zestaw danych szkoleniowych przez ponowne użycie w pakiecie Przygotowanie danych dodatkowe pliki wejściowe

> [!IMPORTANT]
> W tym samouczku przygotowuje tylko dane, nie kompiluje modelu prognozy.
>
> Przygotowane dane można użyć w celu przeszkolenia modeli prognozowania. Na przykład może utworzyć model do przewidywania roweru żądanie w oknie 2 godzin.

## <a name="prerequisites"></a>Wymagania wstępne
* Azure Machine Learning Workbench musi być zainstalowana lokalnie. Aby uzyskać więcej informacji, wykonaj [instalacji szybkiego startu](quickstart-installation.md).
* Znajomość tworzenia nowego projektu w Workbench.

## <a name="data-acquisition"></a>Uzyskiwanie danych
W tym samouczku używana [dataset Boston Hubway](https://s3.amazonaws.com/hubway-data/index.html) i Boston pogody danych z [NOAA](http://www.noaa.gov/).

1. Pobierz pliki danych z następującego łącza w środowisku deweloperskim lokalnego. 
   * [Dane o pogodzie Boston](https://azuremluxcdnprod001.blob.core.windows.net/docs/azureml/bikeshare/BostonWeather.csv). 
   * Hubway podróży dane z witryny sieci Web Hubway.

      - [201501-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201501-hubway-tripdata.zip)
      - [201504-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201504-hubway-tripdata.zip)
      - [201510-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201510-hubway-tripdata.zip)
      - [201601-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201601-hubway-tripdata.zip)
      - [201604-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201604-hubway-tripdata.zip)
      - [201610-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201610-hubway-tripdata.zip)
      - [201701-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201701-hubway-tripdata.zip)

2. Po pobraniu, Rozpakuj każdego pliku zip.

## <a name="learn-about-the-datasets"></a>Więcej informacji na temat zestawów danych
1. __Pogody Boston__ pliku zawiera następujące pola związanych z pogodą, zgłoszonych na godzinę:
   * DATA
   * REPORTTPYE
   * HOURLYDRYBULBTEMPF
   * HOURLYRelativeHumidity
   * HOURLYWindSpeed

2. __Hubway__ dane są zorganizowane w plików przez rok i miesiąc. Na przykład plik o nazwie `201501-hubway-tripdata.zip` zawiera plik CSV zawierający dane dla stycznia 2015 r. Dane zawierają następujące pola każdego wiersza reprezentujący podróży roweru:

   * Czas trwania podróży (w sekundach)
   * Data i godzina rozpoczęcia
   * Data i godzina zatrzymania
   * Nazwa stacji Start & Identyfikator
   * Nazwa punktu końcowego stacji & Identyfikator
   * Identyfikator roweru
   * Typ użytkownika (zwykłych = 24-godzinnym lub 72 godziny użytkownika przebiegu. Element członkowski = — członek roczne lub co miesiąc)
   * Kod POCZTOWY (Jeśli użytkownik jest członkiem)
   * Płci (własnym zgłoszone przez element członkowski)

## <a name="create-a-new-project"></a>Tworzenie nowego projektu
1. Uruchom **Azure Machine Learning Workbench** z start menu lub uruchamiania.

2. Utwórz nowy projekt usługi Azure Machine Learning.  Kliknij przycisk  **+**  znajdującego się na **projekty** strony, lub **pliku** > **nowy**.
   - Użyj **pusty projekt** szablonu.
   - Nazwij swój projekt **BikeShare**. 

## <a id="newdatasource"></a>Utwórz nowe źródło danych

1. Utwórz nowe źródło danych. Kliknij przycisk **danych** przycisk (cylinder ikona) na lewym pasku narzędzi. Spowoduje to wyświetlenie **widoku danych**.

   ![Obraz na karcie Widok danych](media/tutorial-bikeshare-dataprep/navigatetodatatab.png)

2. Dodawanie źródła danych. Wybierz  **+**  ikonę, a następnie wybierz **Dodaj źródło danych**.

   ![Obraz wpisu, Dodaj źródło danych](media/tutorial-bikeshare-dataprep/newdatasource.png)

## <a name="add-weather-data"></a>Dodawanie danych pogody

1. **Magazyn danych**: Wybierz magazyn danych, która zawiera dane. Ponieważ są używane pliki, wybierz opcję **plików / Directory**. Wybierz **dalej** aby kontynuować.

   ![Obraz plików / Zapis w katalogu](media/tutorial-bikeshare-dataprep/datasources.png)

2. **Wybór pliku**: Dodawanie danych pogody. Wyszukaj i wybierz `BostonWeather.csv` wcześniej pobranego pliku. Kliknij przycisk **Dalej**.

   ![Obraz wyboru plików z BostonWeater.csv wybrane](media/tutorial-bikeshare-dataprep/pickweatherdatafile.png)

3. **Szczegóły plików**: Sprawdź schematu pliku, do którego zostanie wykryta. Azure Machine Learning Workbench analizuje dane w pliku i wnioskuje Schemat używany.

   ![Obraz szczegółów pliku](media/tutorial-bikeshare-dataprep/fileparameters.png)

   > [!IMPORTANT]
   > Workbench nie może wykryć poprawny schemat w niektórych przypadkach. Należy zawsze sprawdzić, czy parametry są poprawne dla zestawu danych. Danych pogody Sprawdź, że są ustawione na następujące wartości:
   >
   > * __Typ pliku__: plik przecinkami (csv, tsv, txt itp.)
   > * __Separator__: przecinkami [,]
   > * __Komentarz dotyczący znak wiersza__: wartość nie jest ustawiona.
   > * __Pomiń wiersze tryb__: nie Pomijaj
   > * __Kodowanie pliku__: utf-8
   > * __Podwyższ poziom nagłówków tryb__: Użyj nagłówków z pierwszego pliku

   Podgląd danych powinien być wyświetlany następujące kolumny:
   * **Ścieżka**
   * **DATA**
   * **REPORTTYPE**
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

   Aby kontynuować, wybierz **dalej**.

4. **Typy danych**: Sprawdź typy danych, które są wykrywane automatycznie. Azure Machine Learning Workbench analizuje dane w pliku i wnioskuje typów danych do użycia.

   Te dane, można zmienić `DATA TYPE` wszystkich kolumn do `String`.

   > [!NOTE]
   > `String`Służy do Wyróżnij możliwości Workbench w dalszej części tego samouczka. 

   ![Obraz typów danych](media/tutorial-bikeshare-dataprep/datatypedetection.png)

   Aby kontynuować, wybierz __dalej__. 

5. **Próbkowanie**: Aby utworzyć schemat próbkowania, wybierz **+ nowy** przycisku. Wybierz nową __Top 10000__ wiersza, który jest dodany, a następnie wybierz __Edytuj__. Ustaw __strategii próbki__ do **całego pliku**, a następnie wybierz **Zastosuj**.

   ![Obraz Dodawanie nowej strategii pobierania próbek](media/tutorial-bikeshare-dataprep/weatherdatasampling.png)

   Aby użyć __całego pliku__ strategii, wybierz opcję __całego pliku__ wpis, a następnie wybierz __Ustaw jako aktywny__. Gwiazdy wyświetlane obok __całego pliku__ aby wskazać, że aktywnej strategii.

   ![Obraz pełnego pliku jako aktywnej strategii](media/tutorial-bikeshare-dataprep/fullfileactive.png)

   Aby kontynuować, wybierz **dalej**.

6. **Kolumny**: __kolumny__ sekcja umożliwia pełną ścieżkę pliku jako kolumny w importowanych danych. Wybierz __nie dołączaj kolumny__.

   > [!TIP]
   > Łącznie ze ścieżką jako kolumna jest przydatna, Jeśli importujesz folderem wiele plików o różnych nazwach. Jest również przydatne, jeśli nazwy plików zawierają informacje, które mają zostać wyodrębnione później.

   ![Obraz nie zawiera kolumny](media/tutorial-bikeshare-dataprep/pathcolumn.png)

7. **Zakończ**: Aby ukończyć tworzenie źródła danych, wybierz **Zakończ** przycisku.

    Na nowej karcie Źródło danych o nazwie __BostonWeather__ otwiera. Próbka danych jest wyświetlany w widoku siatki. Przykład jest oparty na schemat active próbkowania określony wcześniej.

    Powiadomienie **kroki** w okienku po prawej stronie ekranu wyświetlana poszczególnych akcje wykonywane podczas tworzenia tego źródła danych.

   ![Obraz przedstawiający źródła danych, przykładowe i kroki](media/tutorial-bikeshare-dataprep/weatherdataloaded.png)

### <a name="view-data-source-metrics"></a>Wyświetlaj metryki źródła danych

Wybierz __metryki__ u góry karty widoku siatki w lewo. Ten widok przedstawia rozkład i innych zagregowanych danych statystycznych próbki danych.

![Wyświetla obraz metryk](media/tutorial-bikeshare-dataprep/weathermetrics.png)

> [!NOTE]
> Aby skonfigurować widoczność statystyki, użyj **wybierz metrykę** listy rozwijanej. Sprawdź i usuń zaznaczenie pola wyboru metryki, aby zmienić widok siatki.

Aby przywrócić __widoku danych__, wybierz pozycję __danych__ z lewym górnym rogu strony.

## <a name="add-data-source-to-data-preparation-package"></a>Dodawanie źródła danych do pakietu przygotowywania danych

1. Wybierz __Prepare__ aby rozpocząć przygotowywanie danych. 

2. Po wyświetleniu monitu wprowadź nazwę pakietu Przygotowanie danych, takich jak `BikeShare Data Prep`. 

3. Wybierz __OK__ aby kontynuować.

   ![Obraz okna dialogowego przygotowanie](media/tutorial-bikeshare-dataprep/dataprepdialog.png)

4. Nowy pakiet o nazwie **BikeShare danych w środowisku przedprodukcyjnym** jest wyświetlany w obszarze __Przygotowanie danych__ sekcji __danych__ kartę. 

   Aby wyświetlić pakietu, wybierz ten wpis. 

5. Wybierz  **>>**  przycisk, aby rozwinąć w celu wyświetlenia __przepływy danych__ zawartych w pakiecie. W tym przykładzie __BostonWeather__ jest tylko przepływu danych.

   > [!IMPORTANT]
   > Pakiet może zawierać wiele przepływy danych.

   ![Obraz przepływy danych zawartych w pakiecie](media/tutorial-bikeshare-dataprep/weatherdataloadedingrid.png)

## <a name="filter-data-by-value"></a>Filtrowanie według wartości danych
1. Do filtrowania danych, kliknij prawym przyciskiem myszy na określoną wartość komórki lub wybierz __filtru__, a następnie typ filtru.

2. W tym samouczku, należy zaznaczyć komórkę, która zawiera wartość `FM-15` , a następnie ustaw filtr do filtru **jest równe**.  Teraz filtrowania danych do wierszy zwraca tylko gdy __REPORTTYPE__ jest `FM-15`.

   ![Obraz okna dialogowego filtru](media/tutorial-bikeshare-dataprep/weatherfilterinfm15.png)

   > [!NOTE]
   > FM-15 jest typem z meteorologiczne Terminal lotniczego rutynowych pogody raportu (METAR). Raporty FM-15 empirycznie są przestrzegane się najbardziej zakończona z listtle brakujące dane.

## <a name="remove-a-column"></a>Usuwanie kolumn

Nie są już potrzebne __REPORTTYPE__ kolumny. Kliknij prawym przyciskiem myszy nagłówek kolumny, a następnie wybierz **kolumna usunąć**.

   ![Obraz opcji Usuń kolumny](media/tutorial-bikeshare-dataprep/weatherremovereporttype.png)

## <a name="change-datatypes-and-remove-errors"></a>Zmienianie typów danych i usuń błędy
1. Naciśnięcie przycisku __Ctrl (polecenie ⌘ dla komputerów Mac)__ podczas wybranie nagłówki kolumn służy do wybierania wielu kolumn na raz. Umożliwia wybranie następujące nagłówki kolumn:
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

2. **Kliknij prawym przyciskiem myszy** jeden z nagłówków wybranej kolumny i wybierz **przekonwertować typ pola liczbowego**. To konwertuje typ danych dla kolumny liczbowe.

   ![Konwertowanie wielu kolumn na liczbowe](media/tutorial-bikeshare-dataprep/weatherconverttonumeric.png)

3. Odfiltrować wartości błędów. Niektóre kolumny będą zawierać problemów konwersja typu danych. Ten problem jest określane przez kolor czerwony w __pasek jakości danych__ dla kolumny.

   Aby usunąć wiersze, które mają błędy, kliknij prawym przyciskiem myszy **HOURLYDRYBULBTEMPF** nagłówek kolumny. Wybierz **filtrować kolumny**. Użyj domyślnej **I mają do** jako **Zachowaj wiersze**. Zmień **warunki** listy rozwijanej, aby wybrać **nie jest błąd**. Wybierz **OK** Aby zastosować filtr.

4. Aby usunąć pozostałe wiersze błąd w innych kolumnach, powtórz ten proces filtru, dla **HOURLYRelativeHumidity** i **HOURLYWindSpeed** kolumn.

## <a name="use-by-example-transformations"></a>Aby użyć przykład przekształcenia

Aby użyć danych w Prognozowanie dla bloków czasu dwóch godzin, należy obliczyć pogodą średnia dla okresów dwóch godzin. Aby to zrobić, można użyć następujących czynności:

* Podziel **data** kolumny w oddzielne **data** i **czasu** kolumn. Zobacz następną sekcję, aby uzyskać szczegółowe instrukcje.

* Pochodzi **Hour_Range** kolumny z **czasu** kolumny. Zobacz sekcję poniżej, aby uzyskać szczegółowe instrukcje.

* Pochodzi **data\_godzina\_zakres** kolumny z **data** i **Hour_Range** kolumn. Zobacz sekcję poniżej, aby uzyskać szczegółowe instrukcje.

### <a name="split-column-by-example"></a>Dzielenie kolumn według przykładu

1. Podziel **data** kolumny w oddzielnych Data i godzina kolumny. Kliknij prawym przyciskiem myszy **data** nagłówek kolumny, a następnie wybierz **podzielona kolumna przykładzie**.

   ![Obraz kolumny podzielone przez przykładowy wpis](media/tutorial-bikeshare-dataprep/weathersplitcolumnbyexample.png)

2. Azure Machine Learning Workbench identyfikuje znaczący ogranicznik i automatycznie tworzy dwie kolumny, Podziel dane na wartości daty i godziny. 

3. Wybierz __OK__ do akceptowania podziału wyniki operacji.

   ![Obraz kolumn podziału DATE_1 i DATE_2](media/tutorial-bikeshare-dataprep/weatherdatesplitted.png)

### <a name="derive-column-by-example"></a>Tworzenie kolumn pochodnych według przykładu

1. Do uzyskania zakresu dwóch godzin, kliknij prawym przyciskiem myszy __data\_2__ nagłówek kolumny, a następnie wybierz **kolumny pochodzi przykładzie**.

   ![Obraz przez przykładowy wpis w kolumnie pochodną](media/tutorial-bikeshare-dataprep/weatherdate2range.png)

   Pusta kolumna zostanie dodana o wartości null.

2. Kliknij pierwszy pustej komórki w nowej kolumnie. Podać przykład przedziału czasu wymaganą przez wpisanie `12AM-2AM` nowej kolumny, a następnie naciśnij klawisz wprowadź.

   ![Obraz nową kolumnę o wartości od 00: 00 - 2 AM](media/tutorial-bikeshare-dataprep/weathertimerangeexample.png)

   > [!NOTE]
   > Azure ML Workbench zsyntetyzuje zmniejszonych programu w oparciu o przykłady podane przez Ciebie i stosuje ten sam program dla pozostałych wierszy. Pozostałe wiersze są wypełniane automatycznie na podstawie podanych przykładu. Workbench również analizuje dane i próbuje zidentyfikować przypadki krawędzi. 

   > [!IMPORTANT]
   > Identyfikacja krawędzi przypadkach mogą nie działać dla komputerów Mac w bieżącej wersji środowiska roboczego. Pomiń __krok 3__ i __krok 4__ poniżej na komputerach Mac. Zamiast tego, naciśnij klawisz __OK__ po wypełniony wszystkie wiersze z wartości pochodnych.
   
3. Tekst **analizowanie danych** powyżej siatki wskazuje, że Workbench próbuje wykryć przypadków krawędzi. Po zakończeniu stan zmienia się na **następnego wiersza sugerowane przeglądu** lub **Brak sugestii**. W tym przykładzie **następnego wiersza sugerowane przeglądu** jest zwracany.

4. Aby wyświetlić sugerowane zmiany, wybierz **następnego wiersza sugerowane przeglądu**. Komórki, które należy przejrzeć i poprawić (w razie potrzeby) zostanie wyróżniona na ekranie.

   ![Obraz wiersza przeglądu](media/tutorial-bikeshare-dataprep/weatherreviewnextsuggested.png)

    Wybierz __OK__ do akceptowania transformacji.
 
5. Nastąpi powrót do widoku siatki danych __BostonWeather__. Siatka zawiera teraz trzy kolumny dodane wcześniej.

   ![Obraz widoku siatki z dodanych wierszy](media/tutorial-bikeshare-dataprep/timerangecomputed.png)

   > [!TIP]
   >  Wszystkie wprowadzone zmiany zostaną zachowane na **kroki** okienka. Przejdź do utworzonego w kroku **kroki** okienku, kliknij strzałkę w dół i wybierz **Edytuj**. W oknie zaawansowanych **kolumny pochodzi przykładzie** jest wyświetlany. Wszystkie przykłady są zachowywane w tym miejscu. Przykłady można również dodać ręcznie, klikając dwukrotnie wierszy w siatce poniżej. Wybierz **anulować** aby powrócić do głównego siatki bez zastosowania zmian. Można również dostęp do tego widoku, wybierając **trybu zaawansowanego** podczas wykonywania **kolumny pochodzi przykładzie** transformacji.

6. Aby zmienić nazwę kolumny, dwukrotnie kliknij nagłówek kolumny i typ **zakres godzin**. Naciśnij klawisz **Enter** można zapisać zmiany.

   ![Zmiana nazwy kolumny](media/tutorial-bikeshare-dataprep/weatherhourrangecolumnrename.png)

7. Do uzyskania zakres dat i godzin, wybrać wiele **data\_1** i **zakres godzin** kolumn, kliknij prawym przyciskiem myszy, a następnie wybierz **kolumny pochodną przykładzie**.

   ![Wyprowadzanie kolumny według przykładu](media/tutorial-bikeshare-dataprep/weatherderivedatehourrange.png)

   Typ `Jan 01, 2015 12AM-2AM` co w przykładzie z pierwszego wiersza i naciśnij klawisz **Enter**.

   Workbench określa przekształcania oparta na przykładzie podane przez użytkownika. W tym przykładzie wynik jest data format jest zmienione i połączony z przedziale dwóch godzin.

   ![Obraz przykładu "01 stycznia 2015 AM 00: 00 - 2](media/tutorial-bikeshare-dataprep/wetherdatehourrangeexample.png)

   > [!IMPORTANT]
   > Dla komputerów Mac, należy wykonać następujący krok zamiast __kroku 8__ poniżej.
   >
   > * Przejdź do pierwszej komórki, który zawiera `Feb 01, 2015 12AM-2AM`. Powinien być __wiersz 15__. Popraw wartość `Jan 02, 2015 12AM-2AM`i naciśnij klawisz __Enter__. 
   

8. Poczekaj, aż stan zmiany z **analizowanie danych** do **następnego wiersza sugerowane przeglądu**. Może to potrwać kilka sekund. Wybierz link stanu, aby przejść do sugerowane wiersza. 

   ![Obraz sugerowane wiersz, aby przejrzeć](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguate.png)

   Wiersz zawiera wartość null, ponieważ wartość daty źródło może być dd/mm/rrrr lub mm/dd/rrrr. Wpisz poprawną wartość `Jan 13, 2015 2AM-4AM` i naciśnij klawisz **Enter**. Workbench używa dwóch przykładach zwiększające pochodnym dla pozostałych wierszy.

   ![Obraz poprawnie sformatowane dane](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguated.png)

9. Wybierz **OK** do akceptowania transformacji.

   ![Obraz siatki przekształcania ukończone](media/tutorial-bikeshare-dataprep/weatherdatehourrangecomputed.png)

   > [!TIP]
   > Można użyć trybu zaawansowanego **kolumny pochodną przykładzie** dla tego kroku, klikając strzałkę w dół w **kroki** okienka. W siatce danych są pola wyboru obok nazwy kolumn **data\_1** i **zakres godzin** kolumn. Usuń zaznaczenie pola wyboru obok pozycji **zakres godzin** kolumny, aby zobaczyć, jak to zmianę danych wyjściowych. W przypadku braku **zakres godzin** kolumny jako dane wejściowe, **00: 00 AM 2 -** jest traktowany jako stała i jest dołączany do wartości pochodnych. Wybierz **anulować** aby powrócić do głównego siatki bez zastosowania zmian.

10. Aby zmienić nazwę kolumny, kliknij dwukrotnie plik nagłówka. Zmień nazwę, aby **zakres dat godzinę** , a następnie naciśnij klawisz **Enter**.

11. Wybór wielokrotny **data**, **data\_1**, **data\_2**, i **zakres godzin** kolumn. Kliknij prawym przyciskiem myszy, a następnie wybierz **kolumna usunąć**.

## <a name="summarize-data-mean"></a>Podsumuj dane (średnia)

Następnym krokiem jest Podsumowując pogodą wykonując średniej wartości pogrupowane według zakres godzin.

1. Wybierz **zakres dat godzinę** kolumny, a następnie wybierz **Summarize** z **przekształca** menu.

    ![Przekształca menu](media/tutorial-bikeshare-dataprep/weathersummarizemenu.png)

2. Podsumowując dane przeciągania kolumn z siatki w dolnej części strony do okienka po lewej i prawej u góry. W okienku po lewej stronie zawiera tekst **przeciągnij kolumny tutaj, aby dane grupy**. Prawe okienko zawiera tekst **przeciągnij kolumny tutaj, aby podsumować danych**. 

    Przeciągnij **zakres dat godzinę** kolumnę z siatki w dolnej części okienka po lewej stronie. Przeciągnij **HOURLYDRYBULBTEMPF**, **HOURLYRelativeHumidity**, i **HOURLYWindSpeed** do prawego okienka. 

    W okienku po prawej stronie zaznacz **oznacza** jako **agregacji** miary dla każdej kolumny. Kliknij przycisk **OK** przeprowadzenie podsumowanie.

   ![Obraz ekranu podsumowanie danych](media/tutorial-bikeshare-dataprep/weathersummarize.png)

## <a name="transform-dataflow-using-script"></a>Przekształcanie przy użyciu skryptu przepływu danych

Modyfikowanie danych w liczbowych kolumn do zakresu 0-1 umożliwia niektórych modeli zbieżności szybko. Obecnie nie jest nie wbudowanych przekształcania objęty zrobić to przekształcania, ale skrypt w języku Python może służyć do wykonania tej operacji.

1. Z **przekształcenie** menu, wybierz opcję **przekształcenie przepływu danych**.

2. Wprowadź następujący kod w polu tekstowym, która jest wyświetlana. Jeśli użyto nazwy kolumn kod powinny działać bez żadnych modyfikacji. W języku Python pisania logiki proste normalizacji minimalnej i maksymalnej.

    > [!WARNING]
    > Skrypt oczekuje nazwy kolumny używane wcześniej w tym samouczku. Jeśli masz inną kolumnę nazw, należy zmienić nazwy w skrypcie.

   ```python
   maxVal = max(df["HOURLYDRYBULBTEMPF_Mean"])
   maxVal = max(df["HOURLYDRYBULBTEMPF_Mean"])
   minVal = min(df["HOURLYDRYBULBTEMPF_Mean"])
   df["HOURLYDRYBULBTEMPF_Mean"] = (df["HOURLYDRYBULBTEMPF_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYDRYBULBTEMPF_Mean":"N_DryBulbTemp"},inplace=True)

   maxVal = max(df["HOURLYRelativeHumidity_Mean"])
   minVal = min(df["HOURLYRelativeHumidity_Mean"])
   df["HOURLYRelativeHumidity_Mean"] = (df["HOURLYRelativeHumidity_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYRelativeHumidity_Mean":"N_RelativeHumidity"},inplace=True)

   maxVal = max(df["HOURLYWindSpeed_Mean"])
   minVal = min(df["HOURLYWindSpeed_Mean"])
   df["HOURLYWindSpeed_Mean"] = (df["HOURLYWindSpeed_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYWindSpeed_Mean":"N_WindSpeed"},inplace=True)

   df
   ```

    > [!TIP]
    > Skrypt w języku Python musi zwracać `df` na końcu. Ta wartość jest używana do wypełniania siatki.
    
    ![Przekształć okno dialogowe skryptu przepływu danych](media/tutorial-bikeshare-dataprep/transformdataflowscript.png)

3. Wybierz __OK__ Aby użyć skryptu. Kolumny liczbowe w siatce zawierają teraz wartości z zakresu 0-1.

    ![Siatka zawierająca wartości od 0 do 1](media/tutorial-bikeshare-dataprep/datagridwithdecimals.png)

Zakończono przygotowywania danych pogody. Następnie przygotuj danych podróży.

## <a name="load-trip-data"></a>Ładowanie danych w podróży

1. Aby zaimportować `201701-hubway-tripdata.csv` plików, wykonaj kroki w [Utwórz nowe źródło danych](#newdatasource) sekcji. Podczas procesu importowania, należy użyć następujących opcji:

    * __Schemat próbkowania__: **całego pliku** próbkowania schematu, uaktywnić próbki, i 
    * __Typ danych__: Zaakceptuj ustawienia domyślne.

2. Po zaimportowaniu danych, wybierz __Prepare__ przycisk, aby rozpocząć przygotowywanie danych. Wybierz istniejącą **Prep.dprep danych BikeShare** pakietu, a następnie wybierz __OK__.

    Ten proces powoduje dodanie **przepływu danych** do istniejącej **Przygotowanie danych** pliku zamiast tworzenia nowej.

    ![Wybranie istniejącego pakietu obrazu](media/tutorial-bikeshare-dataprep/addjandatatodprep.png)

3. Po załadowaniu siatki, rozwiń węzeł __przepływy danych__. Istnieją teraz dwa przepływy danych: **BostonWeather** i **201701-hubway-tripdata**. Wybierz **201701-hubway-tripdata** wpisu.

    ![Obraz wprowadzania 201701-hubway-tripdata](media/tutorial-bikeshare-dataprep/twodfsindprep.png)

## <a name="use-map-inspector"></a>Użyj Inspektora mapy

W celu przygotowania danych istnieją różne przydatne wizualizacji o nazwie **inspektorzy** ciągu, numeryczne i geograficzny danych, które pomagają zrozumieć dane lepiej i w identyfikacji wartości odstających. Aby użyć Inspektora mapy, wykonaj następujące kroki:

1. Wybór wielokrotny **rozpocząć stacja szerokości geograficznej** i **rozpocząć stacja geograficzne** kolumn. Jedna z kolumn kliknij prawym przyciskiem myszy, a następnie wybierz **mapy**.

    > [!TIP]
    > Aby włączyć wyborem wielokrotnym, przytrzymaj __Ctrl (polecenie ⌘ dla komputerów Mac)__ i wybierz nagłówek dla każdej kolumny.

    ![Obraz mapy wizualizacji](media/tutorial-bikeshare-dataprep/launchMapInspector.png)

2. Aby zmaksymalizować wizualizacji mapy, wybierz **Maksymalizuj** ikony. Aby dopasować mapy do okna, wybierz **E** ikonę w lewym górnym rogu strony wizualizacji.

    ![W trybie zmaksymalizowanym obrazu](media/tutorial-bikeshare-dataprep/maximizedmap.png)

3. Polecenie **Minimalizuj** przycisk, aby powrócić do widoku siatki.

## <a name="use-column-statistics-inspector"></a>Użyj Inspektora statystyk kolumny

Aby użyć Inspektora statystyk kolumny, kliknij prawym przyciskiem myszy __tripduration__ kolumny i wybierz __statystyk kolumny__.

![Wpis statystyk kolumny](media/tutorial-bikeshare-dataprep/tripdurationcolstats.png)

Ten proces powoduje dodanie nowej wizualizacji zatytułowany __tripduration statystyki__ w __inspektorzy__ okienka.

![Obraz tripduration inspektora statystyki](media/tutorial-bikeshare-dataprep/tripdurationcolstatsinwell.png)

> [!IMPORTANT]
> Wartość maksymalna czas trwania podróży **961,814 minut**, czyli około 2 lata. Wydaje się, że istnieją pewne wartości odstających w zestawie danych.

## <a name="use-histogram-inspector"></a>Za pomocą histogramu Inspektora

Próbuje identyfikować wartości oddalone, kliknij prawym przyciskiem myszy __tripduration__ kolumny i wybierz __Histogram__.

![Inspektor Histogram](media/tutorial-bikeshare-dataprep/tripdurationhistogram.png)

Histogram nie jest przydatne, ponieważ wartości oddalone są pochylanie wykresu.

## <a name="add-column-using-script"></a>Dodaj kolumnę za pomocą skryptu

1. Kliknij prawym przyciskiem myszy **tripduration** kolumny i wybierz **Dodawanie kolumny (skrypt)**.

    ![Obraz menu Dodaj kolumny (skrypt)](media/tutorial-bikeshare-dataprep/computecolscript.png)

2. W __Dodawanie kolumny (skrypt)__ okna dialogowego, użyj następujących wartości:

    * __Nowa nazwa kolumny__: logtripduration
    * __Wstaw tej nowej kolumny po__: tripduration
    * __Nowy kod kolumny__:`math.log(row.tripduration)`
    * __Typ bloku kodu__: wyrażenie

   ![Okno dialogowe Dodawanie kolumny (skrypt)](media/tutorial-bikeshare-dataprep/computecolscriptdialog.png)

3. Wybierz __OK__ można dodać **logtripduration** kolumny.

4. Kliknij prawym przyciskiem myszy na kolumny i wybierz **Histogram**.

    ![Histogram kolumny logtripduration](media/tutorial-bikeshare-dataprep/logtriphistogram.png)

  Efekty wizualne tego histogramu prawdopodobnie rozkładu normalnego z nietypowe tail.

## <a name="use-advanced-filter"></a>Użyj filtru zaawansowanego

Przy użyciu filtru do danych aktualizuje inspektorzy nowej dystrybucji. Kliknij prawym przyciskiem myszy **logtripduration** kolumny i wybierz **kolumny filtru**. W __Edytuj__ okna dialogowego, użyj następujących wartości:

* __Filtrowanie w tej kolumnie numer__: logtripduration
* __Chcę__: Zachowaj wiersze
* __Gdy__: poniższe warunki mają wartość True (lub logicznej)
* __Jeśli ta kolumna__: mniejsza niż
* __Wartość__: 9

![Opcje filtru](media/tutorial-bikeshare-dataprep/loftripfilter.png)

Wybierz __OK__ Aby zastosować filtr.

![Zaktualizowano histogramów po zastosowaniu filtru](media/tutorial-bikeshare-dataprep/loftripfilteredinspector.png)

### <a name="the-halo-effect"></a>Otoczek

1. Maksymalizowanie **logtripduration** histogramu. Brak niebieski histogram umieszczenia na szarym histogramu. Ten ekran jest nazywany **otoczek**:

    * **Szarego histogram** reprezentuje dystrybucji przed operacją (w tym przypadku operacja filtrowania).
    * **Blue histogram** reprezentuje histogram po wykonaniu operacji. 

   Otoczek ułatwia wizualizacja skutków operacji na danych.

   ![Obraz otoczek](media/tutorial-bikeshare-dataprep/loftripfilteredinspectormaximized.png)

    > [!NOTE]
    > Powiadomienia czy niebieski histogramu jest wyświetlany krótszą w porównaniu do poprzedniego. Jest to spowodowane automatyczne ponowne bucketing danych nowego zakresu.

2. Aby usunąć otoczki, zaznacz __Edytuj__ i usuń zaznaczenie pola wyboru __halo Pokaż__.

    ![Opcje histogram](media/tutorial-bikeshare-dataprep/uncheckhalo.png)

3. Wybierz **OK** wyłączyć otoczek, a następnie zminimalizować histogramu.

### <a name="remove-columns"></a>Usuwanie kolumn

W danych podróży każdy wiersz reprezentuje zdarzenie podnoszenia roweru. W tym samouczku, wystarczy **starttime** i **rozpocząć stacja** kolumn. Usuń kolumny przez wybierania wielu tymi dwiema kolumnami, kliknij prawym przyciskiem myszy nagłówek kolumny, a następnie wybierz **Zachowaj kolumny**. Inne kolumny zostaną usunięte.

![Obraz opcji Zachowaj kolumny](media/tutorial-bikeshare-dataprep/tripdatakeepcolumn.png)

### <a name="summarize-data-count"></a>Podsumuj dane (licznik)

Podsumowując roweru żądanie do 2 godzin, użyj kolumn pochodnych.

1. Kliknij prawym przyciskiem myszy **starttime** kolumny i wybierz **pochodzi kolumny według przykładu**

    ![Obraz pochodzi przez opcję przykład](media/tutorial-bikeshare-dataprep/tripdataderivebyexample.png)

2. Na przykład, wprowadź wartość `Jan 01, 2017 12AM-2AM` dla pierwszego wiersza.

    > [!IMPORTANT]
    > W poprzednim przykładzie pochodny kolumn wielu kroków jest użyty do uzyskania kolumnę zawierającą okres daty i godziny. W tym przykładzie widać, że ta operacja może zostać wykonane jako pojedynczy krok zapewniając przykład ostateczne dane wyjściowe.

    > [!NOTE]
    > Możesz zapewnić przykład względem żadnego wiersza. W tym przykładzie wartość `Jan 01, 2017 12AM-2AM` jest prawidłowa dla pierwszego wiersza danych.

    ![Obraz przykładowe dane](media/tutorial-bikeshare-dataprep/tripdataderivebyexamplefirstexample.png)

   > [!IMPORTANT]
   > Dla komputerów Mac, należy wykonać następujący krok zamiast __krok 3__ poniżej.
   >
   > * Przejdź do pierwszej komórki, który zawiera `Jan 01, 2017 1AM-2AM`. Powinien być __wiersz 14__. Popraw wartość `Jan 01, 2017 12AM-2AM`, a następnie naciśnij klawisz __Enter__. 

3. Poczekaj, aż aplikacji oblicza wartości dla wszystkich wierszy. Może potrwać kilka sekund. Po zakończeniu analizowania używają __następnego wiersza sugerowane przeglądu__ łącze w celu sprawdzenia danych.

   ![Obraz analizy ukończone z łączem przeglądu](media/tutorial-bikeshare-dataprep/tripdatabyexanalysiscomplete.png)

    Upewnij się, że obliczanej wartości są poprawne. Jeśli nie, zaktualizuj wartość o oczekiwanej wartości, a następnie naciśnij klawisz enter. Poczekaj, aż do ukończenia analizy. Zakończenie **następnego wiersza sugerowane przeglądu** przetwarzania do momentu wyświetlenia **Brak sugestii**. Przeanalizowanie **Brak sugestii**, aplikacja jest czyszczone przypadków krawędzi i jest zadowalające syntezatora program. Jest najlepszym rozwiązaniem do wykonywania visual kontroli przekształcone dane przed zaakceptowaniem transformacji. 

4. Wybierz **OK** do akceptowania transformacji. Zmień nazwę nowo utworzonej kolumny, która ma **zakres dat godzinę**.

    ![Obraz o zmienionej nazwie kolumny](media/tutorial-bikeshare-dataprep/tripdatasummarize.png)

5. Kliknij prawym przyciskiem myszy **starttime** nagłówek kolumny, a następnie wybierz **kolumna usunąć**.

6. Podsumowywania danych, wybierz __Summarize__ z __przekształcenie__ menu. Aby utworzyć transformację, wykonaj następujące czynności:

    * Przeciągnij __zakres dat godzinę__ i __start identyfikator stacji__ w okienku po lewej (Grupuj według).
    * Przeciągnij __start identyfikator stacji__ w prawo (Podsumuj dane) w okienku.

   ![Obraz opcji podsumowania](media/tutorial-bikeshare-dataprep/tripdatacount.png)

7. Wybierz **OK** do akceptowania podsumowania wyników.

## <a name="join-dataflows"></a>Dołącz przepływy danych

Aby dołączyć dane pogody z danymi podróży, wykonaj następujące kroki:

1. Wybierz __Join__ z __przekształca__ menu.

2. __Tabele__: Wybierz **BostonWeather** jako po lewej stronie przepływu danych i **201701-hubway-tripdata** jako prawy przepływu danych. Aby kontynuować, wybierz **dalej**.

    ![Obraz opcji tabel](media/tutorial-bikeshare-dataprep/jointableselection.png)

3. __Kolumny klucza__: Wybierz **zakres dat godzinę** kolumny w obu tabel, a następnie wybierz __dalej__.

    ![Obraz sprzężenia na kolumny klucza](media/tutorial-bikeshare-dataprep/joinkeyselection.png)

4. __Typ sprzężenia__: Wybierz __pasujących wierszy__ jako typ sprzężenia, a następnie wybierz __Zakończ__.

    ![Typ sprzężenia pasujących wierszy](media/tutorial-bikeshare-dataprep/joinscreen.png)

    Ten proces powoduje utworzenie nowego przepływu danych o nazwie __Dołącz wynik__.

## <a name="create-additional-features"></a>Tworzenie dodatkowych funkcji

1. Aby utworzyć kolumnę zawierającą dzień tygodnia, kliknij prawym przyciskiem myszy **zakres dat godzinę** kolumny i wybierz **kolumny pochodną przykładzie**. Użycie wartości __Sun__ dla daty, który wystąpił w niedzielę. Na przykład **01 stycznia 2017 00: 00 AM 2 -**. Naciśnij klawisz **Enter** , a następnie wybierz **OK**. Zmień nazwę tej kolumny __dzień tygodnia__.

    ![Obraz tworzenia nową kolumnę zawierającą dzień tygodnia](media/tutorial-bikeshare-dataprep/featureweekday.png)

2. Aby utworzyć kolumnę zawierającą okres czasu dla wiersza, kliknij prawym przyciskiem myszy **zakres dat godzinę** kolumny i wybierz **kolumny pochodną przykładzie**. Użycie wartości **00: 00 AM 2 -** dla wiersz zawierający **01 stycznia 2017 00: 00 AM 2 -**. Naciśnij klawisz **Enter** , a następnie wybierz **OK**. Zmień nazwę tej kolumny **okresu**.

    ![Obraz kolumnie okresu](media/tutorial-bikeshare-dataprep/featurehourrange.png)

3. Aby usunąć **zakres dat godzinę** i **rDate zakres godzin** kolumn, naciśnij klawisz **Ctrl (polecenie ⌘ dla komputerów Mac)** i zaznacz każdy nagłówek kolumny. Kliknij prawym przyciskiem myszy, a następnie wybierz **kolumna usunąć**.

## <a name="read-data-from-python"></a>Odczyt danych z języka Python

Można uruchomić pakiet przygotowywania danych z języka Python lub PySpark i pobrać wynik w postaci **ramki danych**.

Aby wygenerować przykładowy skrypt w języku Python, kliknij prawym przyciskiem myszy __BikeShare danych w środowisku przedprodukcyjnym__ i wybierz __Generowanie pliku kodu dostępu do danych__. Przykładowy plik Python jest tworzony w Twojej **folderu projektu**, a także jest załadowany na karcie środowiska roboczego. Poniższy skrypt języka Python jest przykładowy kod, który jest generowany:

```python
# Use the Azure Machine Learning data preparation package
from azureml.dataprep import package

# Use the Azure Machine Learning data collector to log various metrics
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()

# This call will load the referenced package and return a DataFrame.
# If run in a PySpark environment, this call returns a
# Spark DataFrame. If not, it will return a Pandas DataFrame.
df = package.run('BikeShare Data Prep.dprep', dataflow_idx=0)

# Remove this line and add code that uses the DataFrame
df.head(10)
```

W tym samouczku, nazwa pliku jest `BikeShare Data Prep.py`. Ten plik jest używany w dalszej części tego samouczka.

## <a name="save-test-data-as-a-csv-file"></a>Zapisz dane testowe w pliku CSV

Aby zapisać **Dołącz wynik** przepływu danych do. Plik CSV, musisz zmienić `BikeShare Data Prep.py` skryptu. Aktualizacja skryptu języka Python, używając następującego kodu:

```python
from azureml.dataprep.package import run

# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTest.csv
df.to_csv('Your Test Data File Path here')
```

Wybierz **Uruchom** w górnej części ekranu. Skrypt jest przesyłana w postaci **zadania** na komputerze lokalnym. Gdy stan zadania zmieni się na __Ukończono__, plik został zapisany w określonej lokalizacji.

## <a name="substitute-data-sources"></a>Zastąp źródła danych

W poprzednich krokach użyto `201701-hubway-tripdata.csv` i `BostonWeather.csv` źródeł danych, aby przygotować danych testowych. Aby użyć pakietu z innymi plikami danych podróży, użyj następujących kroków:

1. Utwórz nową **źródła danych** przy użyciu kroki podane wcześniej, z następującymi zmianami do procesu:

    * __Wybór pliku__: po wybraniu pliku, wybrać wiele sześć pozostałych tripdata podróży. Pliki CSV.

        ![Ładowanie sześciu pozostałych plików](media/tutorial-bikeshare-dataprep/selectsixfiles.png)

        > [!NOTE]
        > __5__ wpis wskazuje, czy pięć dodatkowych plików innych niż ten, który znajduje się.

    * __Szczegóły plików__: Ustaw __podwyższyć poziom nagłówków tryb__ do **wszystkie pliki mają tego samego nagłówki**. Ta wartość wskazuje, że każdy plik zawiera ten sam nagłówek.

        ![Wybieranie szczegółów pliku](media/tutorial-bikeshare-dataprep/headerfromeachfile.png) 

   Zapisz nazwę tego źródła danych, ponieważ jest używana w dalszych krokach.

2. Wybierz ikonę folderu, aby wyświetlić pliki w projekcie. Rozwiń węzeł __aml\_config__ katalogu, a następnie wybierz `local.runconfig` pliku.

    ![Obraz lokalizacji local.runconfig](media/tutorial-bikeshare-dataprep/localrunconfig.png) 

3. Dodaj następujące wiersze na końcu `local.runconfig` pliku, a następnie wybierz ikonę dysku, aby zapisać plik.

    ```yaml
    DataSourceSubstitutions:
      201701-hubway-tripdata.dsource: 201501-hubway-tripdata.dsource
    ```

    Ta zmiana zastępuje oryginalnego źródła danych, która zawiera pliki danych sześciu podróży.

## <a name="save-training-data-as-a-csv-file"></a>Zapisywanie danych szkoleniowych w pliku CSV

Przejdź do pliku Python `BikeShare Data Prep.py` edytować wcześniej i podaj inną ścieżkę pliku do zapisania danych szkoleniowych.

```python
from azureml.dataprep.package import run
# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTrain.csv
df.to_csv('Your Training Data File Path here')
```

Aby przesłać nowe zadanie, należy użyć **Uruchom** ikonę w górnej części strony. A **zadania** jest przesyłane przy użyciu nowej konfiguracji. Dane wyjściowe tego zadania jest danych szkoleniowych. Te dane są tworzone przy użyciu tej samej procedury przygotowania danych utworzone wcześniej. Może upłynąć kilka minut, aby zakończyć to zadanie.

## <a name="next-steps"></a>Następne kroki
Samouczek udziału roweru Przygotowanie danych została ukończona. W tym samouczku użyto usługi uczenie maszynowe Azure (wersja zapoznawcza), aby dowiedzieć się jak:
> [!div class="checklist"]
> * Przygotowanie danych interaktywnie za pomocą narzędzia Azure Machine Learning danych przygotowania
> * Importuj, przekształcanie i utworzyć zestawu danych testowych
> * Generowanie pakietu Przygotowanie danych
> * Uruchom pakiet przygotowywania danych przy użyciu języka Python
> * Wygeneruj zestaw danych szkoleniowych przez ponowne użycie w pakiecie Przygotowanie danych dodatkowe pliki wejściowe

Następnie Dowiedz się więcej o przygotowaniu danych:
> [!div class="nextstepaction"]
> [Podręcznik użytkownika przygotowywania danych](data-prep-user-guide.md)
