---
title: "Samouczek udostępniania roweru — przygotowanie zaawansowanych danych za pomocą usługi Azure Machine Learning Workbench"
description: "Oraz samouczek całościowego przygotowania danych przy użyciu usługi Azure Machine Learning Workbench"
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
ms.openlocfilehash: e4bcf7ec2a18f6068554c2eb85b72ffc36dcc4fc
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="bike-share-tutorial-advanced-data-preparation-with-azure-machine-learning-workbench"></a>Samouczek udostępniania roweru: przygotowanie zaawansowanych danych za pomocą usługi Azure Machine Learning Workbench
Usługa Azure Machine Learning (wersja zapoznawcza) stanowi zintegrowane, kompleksowe rozwiązanie do nauki o danych i do analiz zaawansowanych przeznaczone dla profesjonalnych analityków, którzy będą z niego korzystać w celu przygotowywania danych, opracowywania eksperymentów i wdrażania modeli na skalę chmury.

W tym samouczku użyjesz usługi Azure Machine Learning (wersja zapoznawcza), aby dowiedzieć się, jak:
> [!div class="checklist"]
> * Interaktywnie przygotować dane za pomocą narzędzia do przygotowywania danych usługi Azure Machine Learning
> * Importować, przekształcać i tworzyć zestawy danych testowych
> * Tworzyć pakiet przygotowywania danych
> * Uruchamiać pakiet przygotowywania danych przy użyciu języka Python
> * Generować zestaw danych szkoleniowych poprzez ponowne użycie pakietu przygotowywania danych do dodatkowych plików wejściowych
> * wykonywać skrypty w oknie lokalnego interfejsu wiersza polecenia platformy Azure,
> * wykonywać skrypty w środowisku usługi Azure HDInsight w chmurze.


## <a name="prerequisites"></a>Wymagania wstępne
* Usługa Azure Machine Learning Workbench musi być zainstalowana lokalnie. Aby uzyskać więcej informacji, postępuj zgodnie z [instrukcjami szybkiego startu instalacji](quickstart-installation.md).
* Jeśli nie masz zainstalowanych interfejsu wiersza polecenia Azure, postępuj zgodnie z instrukcjami, aby [zainstaluj najnowszą wersję interfejsu wiersza polecenia Azure]. (https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
* [Klastra HDInsights Spark](how-to-create-dsvm-hdi.md#create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal) musi zostać utworzona na platformie Azure.
* Konto magazynu platformy Azure.
* Znajomość tworzenia nowego projektu w usłudze Workbench.
* Chociaż nie jest wymagana, warto mieć [Eksploratora usługi Storage Azure](https://azure.microsoft.com/features/storage-explorer/) zainstalowany, możesz przekazać, Pobierz i wyświetlać obiekty BLOB na koncie magazynu. 

## <a name="data-acquisition"></a>Pozyskiwanie danych
Ten samouczek używa [zestawu danych Boston Hubway](https://s3.amazonaws.com/hubway-data/index.html) i danych o pogodzie w Bostonie z [NOAA](http://www.noaa.gov/).

1. Pobierz pliki danych z następujących linków do swojego lokalnego środowiska deweloperskiego. 
   * [Dane o pogodzie w Bostonie](https://azuremluxcdnprod001.blob.core.windows.net/docs/azureml/bikeshare/BostonWeather.csv). 
   * Dane podróży Hubway z witryny internetowej firmy Hubway.

      - [201501-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201501-hubway-tripdata.zip)
      - [201504-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201504-hubway-tripdata.zip)
      - [201510-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201510-hubway-tripdata.zip)
      - [201601-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201601-hubway-tripdata.zip)
      - [201604-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201604-hubway-tripdata.zip)
      - [201610-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201610-hubway-tripdata.zip)
      - [201701-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201701-hubway-tripdata.zip)

2. Po pobraniu rozpakuj każdy plik zip.

## <a name="upload-data-files-to-azure-blob-storage"></a>Przekazywanie plików danych do magazynu obiektów Blob platformy Azure
Magazyn obiektów blob służy do obsługi plików danych.

1. Użyj tego samego konta magazynu Azure używanego do klastra usługi HDInsight, którego używasz.

    ![hdinsightstorageaccount.png](media/tutorial-bikeshare-dataprep/hdinsightstorageaccount.png)

2. Utwórz nowy kontener o nazwie "**pliki danych**" do przechowywania plików danych BikeShare.

3. Przekazywanie plików danych. Przekaż `BostonWeather.csv` do folderu o nazwie `weather`i podróży plików danych do folderu o nazwie `tripdata`.

    ![azurestoragedatafile.png](media/tutorial-bikeshare-dataprep/azurestoragedatafile.png)

> [!TIP]
> Można także użyć **Eksploratora usługi Storage Azure** przekazać obiektów blob. Można to narzędzie, aby wyświetlić zawartość pliki wygenerowane w samouczku również.

## <a name="learn-about-the-datasets"></a>Dowiedz się więcej o zestawach danych
1. Plik __pogody w Bostonie__ zawiera następujące pola związane z pogodą, zgłaszane co godzinę:
   * DATE
   * REPORTTPYE
   * HOURLYDRYBULBTEMPF
   * HOURLYRelativeHumidity
   * HOURLYWindSpeed

2. Dane __Hubway__ są zorganizowane w pliki według roku i miesiąca. Na przykład plik o nazwie `201501-hubway-tripdata.zip` zawiera plik csv, w którym znajdują się dane za styczeń 2015 r. Dane zawierają następujące pola, przy czym każdy wiersz reprezentuje podróż rowerową:

   * Czas trwania podróży (w sekundach)
   * Data i godzina rozpoczęcia
   * Data i godzina zakończenia
   * Nazwa i identyfikator stacji rozpoczęcia
   * Nazwa i identyfikator stacji zakończenia
   * Identyfikator roweru
   * Typ użytkownika (zwykły = użytkownik mający dostęp 24-godzinny lub 72-godzinny; Członek = członkostwo roczne lub miesięczne)
   * Kod pocztowy (Jeśli użytkownik jest członkiem)
   * Płeć (zgłaszana przez samego członka)

## <a name="create-a-new-project"></a>Tworzenie nowego projektu
1. Uruchom usługę **Azure Machine Learning Workbench** z menu start lub modułu uruchamiającego.

2. Utwórz nowy projekt usługi Azure Machine Learning.  Kliknij przycisk **+** znajdujący się na stronie **Projekty** lub pozycje **Plik** > **Nowy**.
   - Użyj **udziału roweru** szablonu.
   - Nazwij swój projekt **BikeShare**. 

## <a id="newdatasource"></a>Tworzenie nowego źródła danych

1. Utwórz nowe źródło danych. Kliknij przycisk **Dane** (ikona cylindra) na lewym pasku narzędzi. Spowoduje to wyświetlenie **Widoku danych**.

   ![Obraz karty Widok danych](media/tutorial-bikeshare-dataprep/navigatetodatatab.png)

2. Dodaj źródło danych. Wybierz ikonę **+**, a następnie wybierz pozycję **Dodaj źródło danych**.

   ![Obraz wpisu Dodaj źródło danych](media/tutorial-bikeshare-dataprep/newdatasource.png)

## <a name="add-weather-data"></a>Dodawanie danych o pogodzie

1. **Magazyn danych**: wybierz magazyn danych, który zawiera dane. Ponieważ używasz plików, wybierz pozycję **Pliki/katalog**. Kliknij przycisk **Dalej**, aby kontynuować.

   ![Obraz wpisu Pliki/katalog](media/tutorial-bikeshare-dataprep/datasources.png)

2. **Wybór pliku**: dodaj dane o pogodzie. Wyszukaj i wybierz `BostonWeather.csv` pliku, który został przekazany do __magazyn obiektów Blob Azure__ wcześniej. Kliknij przycisk **Dalej**.

   ![Obraz wyboru pliku z wybranym plikiem BostonWeater.csv](media/tutorial-bikeshare-dataprep/azureblobpickweatherdatafile.png)

3. **Szczegóły plików**: sprawdź schemat pliku, który został wykryty. Usługa Azure Machine Learning Workbench analizuje dane w pliku i rozpoznaje schemat, który ma być używany.

   ![Obraz szczegółów pliku](media/tutorial-bikeshare-dataprep/fileparameters.png)

   > [!IMPORTANT]
   > W niektórych przypadkach usługa Workbench może nie wykryć poprawnego schematu. Musisz zawsze sprawdzić, czy parametry zestawu danych są poprawne. W przypadku danych o pogodzie sprawdź, czy są one ustawione na następujące wartości:
   >
   > * __Typ pliku__: plik ograniczany (csv, tsv, txt itd.)
   > * __Separator__: przecinek [,]
   > * __Znak wiersza komentarza__: nie jest ustawiona żadna wartość.
   > * __Tryb pomijania wierszy__: nie pomijaj
   > * __Kodowanie pliku__: utf-8
   > * __Tryb promowania nagłówków__: użyj nagłówków z pierwszego pliku

   Podgląd danych powinien wyświetlać następujące kolumny:
   * **Ścieżka**
   * **DATE**
   * **REPORTTYPE**
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

   Aby kontynuować, kliknij przycisk **Dalej**.

4. **Typy danych**: sprawdź typy danych, które są wykrywane automatycznie. Usługa Azure Machine Learning Workbench analizuje dane w pliku i rozpoznaje typy danych, które mają być używane.

   Dla tych danych zmień `DATA TYPE` wszystkich kolumn na `String`.

   > [!NOTE]
   > `String` służy do wyróżniania możliwości usługi Workbench w dalszej części tego samouczka. 

   ![Obraz typów danych](media/tutorial-bikeshare-dataprep/datatypedetection.png)

   Aby kontynuować, kliknij przycisk __Dalej__. 

5. **Próbkowanie**: Aby utworzyć schemat próbkowania, wybierz **Edytuj** przycisku. Wybierz nowy wiersz __Top 10000__, który został dodany, a następnie wybierz pozycję __Edytuj__. Ustaw pozycję __Strategia próbki__ na **Cały plik**, a następnie wybierz pozycję **Zastosuj**.

   ![Obraz dodawania nowej strategii próbkowania](media/tutorial-bikeshare-dataprep/weatherdatasamplingfullfile.png)

   Aby użyć strategii __Cały plik__, wybierz wpis __Cały plik__, a następnie wybierz pozycję __Ustaw jako aktywny__. Obok pozycji __Cały plik__ zostanie wyświetlona gwiazdka, aby wskazać, że to jest aktywna strategia.

   ![Obraz całego pliku jako aktywnej strategii](media/tutorial-bikeshare-dataprep/fullfileactive.png)

   Aby kontynuować, kliknij przycisk **Dalej**.

6. **Kolumna ścieżki**:sekcja __Kolumna ścieżki__ umożliwia włączenie pełnej ścieżki pliku jako kolumny do importowanych danych. Wybierz pozycję __Nie dołączaj kolumny ścieżki__.

   > [!TIP]
   > Dołączenie ścieżki jako kolumny jest przydatne, jeśli importujesz folder z wieloma plikami o różnych nazwach. Jest to również przydatne, jeśli nazwy plików zawierają informacje, które chcesz wyodrębnić później.

   ![Obraz kolumny ścieżki ustawionej tak, aby nie była uwzględniana](media/tutorial-bikeshare-dataprep/pathcolumn.png)

7. **Zakończ**: aby zakończyć tworzenie źródła danych, kliknij przycisk **Zakończ**.

    Zostanie otwarta nowa karta źródła danych o nazwie __BostonWeather__. Próbka danych jest wyświetlana w widoku siatki. Przykład jest oparty na wcześniej określonym aktywnym schemacie próbkowania.

    Zwróć uwagę na okienko **Kroki** po prawej stronie ekranu, które wyświetla poszczególne akcje wykonane podczas tworzenia tego źródła danych.

   ![Obraz przedstawiający źródło danych, przykład i kroki](media/tutorial-bikeshare-dataprep/weatherdataloaded.png)

### <a name="view-data-source-metrics"></a>Wyświetlanie metryk źródła danych

Wybierz przycisk __Metryki__ z lewej strony u góry karty widoku siatki. Ten widok przedstawia rozkład i inne zagregowane dane statystyczne próbki danych.

![Obraz wyświetlonych metryk](media/tutorial-bikeshare-dataprep/weathermetrics.png)

> [!NOTE]
> Aby skonfigurować widoczność statystyk, użyj listy rozwijanej **Wybierz metrykę**. Zaznacz i usuń tam zaznaczenie pola wyboru metryki, aby zmienić widok siatki.

Aby powrócić do __widoku danych__, wybierz pozycję __Dane__ w lewym górnym rogu strony.

## <a name="add-data-source-to-data-preparation-package"></a>Dodawanie źródła danych do pakietu przygotowywania danych

1. Wybierz pozycję __Przygotuj__, aby rozpocząć przygotowywanie danych. 

2. Po wyświetleniu monitu wprowadź nazwę pakietu przygotowywania danych, taką jak `BikeShare Data Prep`. 

3. Kliknij przycisk __OK__, aby kontynuować.

   ![Obraz okna dialogowego przygotowywania](media/tutorial-bikeshare-dataprep/dataprepdialog.png)

4. Nowy pakiet o nazwie **BikeShare Data Prep** zostanie wyświetlony w sekcji __Przygotowywanie danych__ karty __Dane__. 

   Aby wyświetlić pakiet, wybierz ten wpis. 

5. Wybierz przycisk **>>**, aby go rozwinąć w celu wyświetlenia pozycji __Przepływy danych__ zawartych w pakiecie. W tym przykładzie __BostonWeather__ jest jedynym przepływem danych.

   > [!IMPORTANT]
   > Pakiet może zawierać wiele przepływów danych.

   ![Obraz przepływów danych zawartych w pakiecie](media/tutorial-bikeshare-dataprep/weatherdataloadedingrid.png)

## <a name="filter-data-by-value"></a>Filtrowanie danych według wartości
1. Aby przefiltrować dane, kliknij prawym przyciskiem myszy komórkę z określoną wartością lub wybierz pozycję __Filtr__, a następnie typ filtru.

2. W tym samouczku wybierz komórkę, która zawiera wartość `FM-15`, a następnie ustaw filtr na typ filtru **Jest równe**.  Teraz dane są filtrowane tak, aby zwracać tylko wiersze, w których __REPORTTYPE__ ma wartość `FM-15`.

   ![Obraz okna dialogowego filtru](media/tutorial-bikeshare-dataprep/weatherfilterinfm15.png)

   > [!NOTE]
   > FM-15 jest typem raportu METAR (Meteorological Terminal Aviation Routine Weather Report). Raporty FM-15 są empirycznie uważane za najbardziej kompletne i z małą liczbą brakujących danych.

## <a name="remove-a-column"></a>Usuwanie kolumny

Nie potrzebujesz już kolumny __REPORTTYPE__. Kliknij prawym przyciskiem myszy nagłówek kolumny, a następnie wybierz pozycję **Usuń kolumnę**.

   ![Obraz opcji usuwania kolumny](media/tutorial-bikeshare-dataprep/weatherremovereporttype.png)

## <a name="change-datatypes-and-remove-errors"></a>Zmienianie typów danych i usuwanie błędów
1. Naciśnięcie przycisku __Ctrl (polecenie ⌘ dla komputerów Mac)__ podczas wybierania nagłówków kolumn umożliwia wybranie wielu kolumn naraz. Umożliwia to wybranie następujących nagłówków kolumn:
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

2. **Kliknij prawym przyciskiem myszy** jeden z wybranych nagłówków kolumny, a następnie wybierz pozycję **Konwertuj typ pola na liczbowy**. Spowoduje to konwersję typu danych dla kolumny na liczbowy.

   ![Konwertowanie wielu kolumn na liczbowe](media/tutorial-bikeshare-dataprep/weatherconverttonumeric.png)

3. Odfiltruj wartości błędów. Niektóre kolumny mają problemy z konwersją typu danych. Ten problem jest wskazywany przez kolor czerwony na __pasku jakości danych__ dla kolumny.

   Aby usunąć wiersze, które mają błędy, kliknij prawym przyciskiem myszy nagłówek kolumny **HOURLYDRYBULBTEMPF**. Wybierz pozycję **Filtruj kolumnę**. Użyj domyślnej pozycji **Chcę** jako **Zachowaj wiersze**. Zmień listę rozwijaną **Warunki**, aby wybrać pozycję **nie jest błędem**. Kliknij przycisk **OK**, aby zastosować filtr.

    ![filtererrorvalues.png](media/tutorial-bikeshare-dataprep/filtererrorvalues.png)

4. Aby usunąć pozostałe wiersze błędów w innych kolumnach, powtórz ten proces filtrowania dla kolumn **HOURLYRelativeHumidity** i **HOURLYWindSpeed**.

## <a name="use-by-example-transformations"></a>Użycie dla przykładowych przekształceń

Aby użyć danych w prognozie dla dwugodzinnych bloków czasu, musisz obliczyć średnie warunki pogodowe dla okresów dwugodzinnych. W tym celu możesz wykonać następujące czynności:

* Podziel kolumnę **DATE** na oddzielne kolumny **Data** i **Godzina**. Szczegółowe instrukcje zostały podane w następnej sekcji.

* Utwórz kolumnę pochodną **Hour_Range** z kolumny **Godzina**. Szczegółowe instrukcje zostały podane w poniższej sekcji.

* Utwórz kolumnę pochodną **Data\_Godzina\_Zakres** z kolumn **DATE** i **Hour_Range**. Szczegółowe instrukcje zostały podane w poniższej sekcji.

### <a name="split-column-by-example"></a>Dzielenie kolumn według przykładu

1. Podziel kolumnę **DATE** na oddzielne kolumny daty i godziny. Kliknij prawym przyciskiem myszy nagłówek kolumny **DATE**, a następnie wybierz pozycję **Podziel kolumnę według przykładu**.

   ![Obraz wpisu podziału kolumny według przykładu](media/tutorial-bikeshare-dataprep/weathersplitcolumnbyexample.png)

2. Usługa Azure Machine Learning Workbench identyfikuje znaczący ogranicznik i automatycznie tworzy dwie kolumny, dzieląc dane na wartości daty i godziny. 

3. Kliknij przycisk __OK__, aby zaakceptować wyniki operacji podziału.

   ![Obraz podzielonych kolumn DATE_1 i DATE_2](media/tutorial-bikeshare-dataprep/weatherdatesplitted.png)

### <a name="derive-column-by-example"></a>Tworzenie kolumn pochodnych według przykładu

1. Aby uzyskać pochodny zakres dwugodzinny, kliknij prawym przyciskiem myszy nagłówek kolumny __DATE\_2__, a następnie wybierz pozycję **Tworzenie kolumn pochodnych według przykładu**.

   ![Obraz wpisu tworzenia kolumn pochodnych według przykładu](media/tutorial-bikeshare-dataprep/weatherdate2range.png)

   Nowa pusta kolumna zostanie dodana z wartościami null.

2. Kliknij pierwszą pustą komórkę w nowej kolumnie. Podaj przykład wymaganego przedziału czasu, wpisując `12AM-2AM` w nowej kolumnie, a następnie naciskając klawisz Enter.

   ![Obraz nowej kolumny z wartością 0:00–2:00](media/tutorial-bikeshare-dataprep/weathertimerangeexample.png)

   > [!NOTE]
   > Usługa Azure ML Workbench syntetyzuje program na podstawie podanych przez Ciebie przykładów i stosuje ten sam program do pozostałych wierszy. Wszystkie pozostałe wiersze są wypełniane automatycznie na podstawie podanego przykładu. Usługa Workbench również analizuje dane i próbuje zidentyfikować przypadki brzegowe. 

   > [!IMPORTANT]
   > Identyfikacja krawędzi przypadkach mogą nie działać dla komputerów Mac w bieżącej wersji środowiska roboczego. Pomiń __krok 3__ i __krok 4__ poniżej na komputerach Mac. Zamiast tego, naciśnij klawisz __OK__ po wypełniony wszystkie wiersze z wartości pochodnych.
   
3. Tekst **Analizowanie danych** nad siatką wskazuje, że usługa Workbench próbuje wykryć przypadki brzegowe. Po zakończeniu stan zmieni się na **Przejrzyj następny sugerowany wiersz** lub **Brak sugestii**. W tym przykładzie jest zwracany stan **Przejrzyj następny sugerowany wiersz**.

4. Aby wyświetlić sugerowane zmiany, wybierz pozycję **Przejrzyj następny sugerowany wiersz**. Komórka, którą należy przejrzeć i poprawić (w razie potrzeby), jest wyróżniona na ekranie.

   ![Obraz wiersza przeglądu](media/tutorial-bikeshare-dataprep/weatherreviewnextsuggested.png)

    Wybierz __OK__, aby zaakceptować transformację.
 
5. Nastąpi powrót do widoku siatki danych __BostonWeather__. Siatka zawiera teraz trzy kolumny dodane wcześniej.

   ![Obraz widoku siatki z dodanymi wierszami](media/tutorial-bikeshare-dataprep/timerangecomputed.png)

   > [!TIP]
   >  Wszystkie wprowadzone zmiany zostaną zachowane w okienku **Kroki**. Przejdź do kroku utworzonego w okienku **Kroki**, kliknij strzałkę w dół i wybierz pozycję **Edytuj**. Zostanie wyświetlone zaawansowane okno dla **Tworzenia kolumn pochodnych według przykładu**. Wszystkie przykłady są zachowywane w tym miejscu. Przykłady możesz również dodać ręcznie, klikając dwukrotnie wiersz w siatce poniżej. Wybierz pozycję **Anuluj**, aby powrócić do głównej siatki bez stosowania zmian. Dostęp do tego widoku możesz też uzyskać, wybierając pozycję **Tryb zaawansowany** podczas wykonywania przekształcenia **Tworzenie kolumn pochodnych według przykładu**.

6. Aby zmienić nazwę kolumny, dwukrotnie kliknij nagłówek kolumny i wpisz **Zakres godzin**. Naciśnij klawisz **Enter**, aby zapisać zmiany.

   ![Zmienianie nazwy kolumny](media/tutorial-bikeshare-dataprep/weatherhourrangecolumnrename.png)

7. Aby utworzyć pochodny zakres dat i godzin, wybierz wiele kolumn **Data\_1** i **Zakres godzin**, kliknij prawym przyciskiem myszy, a następnie wybierz pozycję **Tworzenie kolumn pochodnych według przykładu**.

   ![Tworzenie kolumn pochodnych według przykładu](media/tutorial-bikeshare-dataprep/weatherderivedatehourrange.png)

   Wpisz `Jan 01, 2015 12AM-2AM` jako przykład dla pierwszego wiersza i naciśnij klawisz **Enter**.

   Usługa Workbench określa przekształcenie na podstawie podanego przykładu. W tym przykładzie wynik jest data format jest zmienione i połączony z przedziale dwóch godzin.

   ![Obraz przykładu „01 stycznia 2015 0:00–2:00](media/tutorial-bikeshare-dataprep/wetherdatehourrangeexample.png)

   > [!IMPORTANT]
   > Dla komputerów Mac, należy wykonać następujący krok zamiast __kroku 8__ poniżej.
   >
   > * Przejdź do pierwszej komórki, który zawiera `Feb 01, 2015 12AM-2AM`. Powinien być __wiersz 15__. Popraw wartość `Jan 02, 2015 12AM-2AM`i naciśnij klawisz __Enter__. 
   

8. Poczekaj, aż stan zmieni się z **Analizowanie danych** na **Przejrzyj następny sugerowany wiersz**. Może to potrwać kilka sekund. Wybierz link stanu, aby przejść do sugerowanego wiersza. 

   ![Obraz sugerowanego wiersza do przeglądu](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguate.png)

   Wiersz zawiera wartość null, ponieważ wartość daty źródłowej może mieć format dd/mm/rrrr lub mm/dd/rrrr. Wpisz poprawną wartość `Jan 13, 2015 2AM-4AM` i naciśnij klawisz **Enter**. Usługa Workbench używa dwóch przykładów do ulepszenia tworzenia dla pozostałych wierszy.

   ![Obraz poprawnie sformatowanych danych](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguated.png)

9. Wybierz **OK**, aby zaakceptować przekształcenie.

   ![Obraz ukończonej siatki przekształcenia](media/tutorial-bikeshare-dataprep/weatherdatehourrangecomputed.png)

   > [!TIP]
   > Możesz użyć trybu zaawansowanego **Tworzenie kolumn pochodnych według przykładu** dla tego kroku, klikając strzałkę w dół w okienku **Kroki**. W siatce danych istnieją pola wyboru obok nazw kolumn **DATE\_1** i **Zakres godzin**. Usuń zaznaczenie pola wyboru obok kolumny **Zakres godzin**, aby zobaczyć, jak to zmienia dane wyjściowe. W przypadku braku kolumny **Zakres godzin** jako danych wejściowych zakres **0:00–2:00** jest traktowany jako stała i jest dołączany do wartości pochodnych. Wybierz pozycję **Anuluj**, aby powrócić do głównej siatki bez stosowania zmian.
   ![derivedcolumnadvancededitdeselectcolumn.png](media/tutorial-bikeshare-dataprep/derivedcolumnadvancededitdeselectcolumn.png)

10. Aby zmienić nazwę kolumny, kliknij dwukrotnie nagłówek. Zmień nazwę na **Zakres daty godziny**, a następnie naciśnij klawisz **Enter**.

11. Dokonaj wielokrotnego wyboru kolumn **DATE**, **DATE\_1**, **DATE\_2** i **Zakres godzin**. Kliknij prawym przyciskiem myszy, a następnie wybierz pozycję **Usuń kolumnę**.

## <a name="summarize-data-mean"></a>Podsumowanie danych (średnia)

Następnym krokiem jest podsumowanie warunków pogodowych, obliczając średnią wartości pogrupowanych według zakresu godzin.

1. Wybierz kolumnę **Zakres daty godziny**, a następnie wybierz pozycję **Podsumowanie** z menu **Przekształcenia**.

    ![Menu Przekształcenia](media/tutorial-bikeshare-dataprep/weathersummarizemenu.png)

2. Aby podsumować dane, przeciągnij kolumny z siatki w dolnej części strony do okienka po lewej i prawej stronie u góry. Okienko po lewej stronie zawiera tekst **Przeciągnij kolumny tutaj, aby zgrupować dane**. Okienko po prawej stronie zawiera tekst **Przeciągnij kolumny tutaj, aby podsumować dane**. 

    Przeciągnij kolumnę **Zakres daty godziny** z siatki w dolnej części do okienka po lewej stronie. Przeciągnij kolumny **HOURLYDRYBULBTEMPF**, **HOURLYRelativeHumidity** i **HOURLYWindSpeed** do prawego okienka. 

    W okienku po prawej stronie wybierz pozycję **Średnia** jako miarę **Agregacji** dla każdej kolumny. Kliknij przycisk **OK**, aby zakończyć podsumowywanie.

   ![Obraz ekranu podsumowywania danych](media/tutorial-bikeshare-dataprep/weathersummarize.png)

## <a name="transform-dataflow-using-script"></a>Przekształcanie przepływu danych przy użyciu skryptu

Sprowadzanie danych w kolumnach liczbowych do zakresu 0-1 umożliwia uzyskanie szybkiej zbieżności w niektórych modelach. Obecnie nie ma wbudowanego przekształcenia, aby standardowo wykonywać to przekształcenie, ale do wykonania tej operacji można użyć skryptu w języku Python.

1. Z **przekształcenie** menu, wybierz opcję **przekształcenie przepływu danych (skrypt)**.

2. Wprowadź następujący kod w polu tekstowym, które jest wyświetlane. Jeśli użyto nazw kolumn, kod powinien działać bez żadnych modyfikacji. Piszesz prostą logikę normalizacji minimum-maksimum w języku Python.

    > [!WARNING]
    > Skrypt oczekuje nazw kolumn użytych wcześniej w tym samouczku. Jeśli masz inne nazwy kolumn, musisz zmienić nazwy w skrypcie.

   ```python
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
    > Skrypt w języku Python musi zwracać `df` na końcu. Ta wartość jest używana do zapełniania siatki.
    
    ![Okno dialogowe skryptu przekształcania przepływu danych](media/tutorial-bikeshare-dataprep/transformdataflowscript.png)

3. Wybierz przycisk __OK__, aby użyć skryptu. Kolumny liczbowe w siatce zawierają teraz wartości z zakresu 0-1.

    ![Siatka zawierająca wartości od 0 do 1](media/tutorial-bikeshare-dataprep/datagridwithdecimals.png)

Zakończono przygotowywanie danych o pogodzie. Następnie przygotuj dane podróży.

## <a name="load-trip-data"></a>Ładowanie danych podróży

1. Aby zaimportować plik `201701-hubway-tripdata.csv`, wykonaj kroki podane w sekcji [Tworzenie nowego źródła danych](#newdatasource). W trakcie procesu importowania użyj następujących opcji:

    * __Wybór pliku__: Wybierz **obiektów Blob platformy Azure** podczas przeglądania, aby wybrać plik.
    * __Schemat próbkowania__: schemat próbkowania **Cały plik**, uaktywnij próbkę i 
    * __Typ danych__: zaakceptuj wartości domyślne.

2. Po zaimportowaniu danych naciśnij przycisk __Przygotuj__, aby rozpocząć przygotowywanie danych. Wybierz istniejący pakiet **BikeShare Data Prep.dprep**, a następnie wybierz przycisk __OK__.

    Ten proces powoduje dodanie **Przepływu danych** do istniejącego pliku **Przygotowywanie danych** zamiast tworzenia nowego.

    ![Obraz wybierania istniejącego pakietu](media/tutorial-bikeshare-dataprep/addjandatatodprep.png)

3. Po załadowaniu siatki rozwiń węzeł __DATAFLOWS__. Istnieją teraz dwa przepływy danych: **BostonWeather** i **201701-hubway-tripdata**. Wybierz wpis **201701-hubway-tripdata**.

    ![Obraz wpisu 201701-hubway-tripdata](media/tutorial-bikeshare-dataprep/twodfsindprep.png)

## <a name="use-map-inspector"></a>Używanie inspektora mapy

Istnieje wiele wizualizacji przydatnych w przygotowaniu danych nazywanych **Inspektorami** danych ciągu, numerycznych i geograficznych, które pomagają lepiej zrozumieć dane i identyfikować elementy odstające. Aby użyć Inspektora mapy, wykonaj następujące kroki:

1. Wybierz wiele kolumn **szerokość geograficzna stacji początkowej** i **długość geograficzna stacji początkowej**. Kliknij prawym przyciskiem myszy jedną z kolumn, a następnie wybierz pozycję **Mapy**.

    > [!TIP]
    > Aby włączyć wybór wielokrotny, przytrzymaj klawisz __Ctrl (polecenie ⌘ dla komputerów Mac)__ i wybierz nagłówek każdej kolumny.

    ![Obraz wizualizacji mapy](media/tutorial-bikeshare-dataprep/launchMapInspector.png)

2. Aby zmaksymalizować wizualizację mapy, wybierz ikonę **Maksymalizuj**. Aby dopasować mapę do okna, wybierz ikonę **E** w lewym górnym rogu wizualizacji.

    ![Zmaksymalizowany obraz](media/tutorial-bikeshare-dataprep/maximizedmap.png)

3. Kliknij przycisk **Minimalizuj**, aby powrócić do widoku siatki.

## <a name="use-column-statistics-inspector"></a>Używanie inspektora statystyk kolumny

Aby użyć inspektora statystyk kolumny, kliknij prawym przyciskiem myszy kolumnę __tripduration__ i wybierz pozycję __Statystyki kolumny__.

![Wpis statystyk kolumny](media/tutorial-bikeshare-dataprep/tripdurationcolstats.png)

Ten proces powoduje dodanie nowej wizualizacji zatytułowanej __Statystyki tripduration__ w okienku __INSPEKTORZY__.

![Obraz inspektora statystyk tripduration](media/tutorial-bikeshare-dataprep/tripdurationcolstatsinwell.png)

> [!IMPORTANT]
> Wartość maksymalna czasu trwania podróży wynosi **961 814 minut**, czyli około 2 lat. Wydaje się, że w zestawie danych istnieją pewne elementy odstające.

## <a name="use-histogram-inspector"></a>Używanie inspektora histogramu

Aby spróbować zidentyfikować elementy odstające, kliknij prawym przyciskiem myszy kolumnę __tripduration__ i wybierz pozycję __Histogram__.

![Inspektor histogramu](media/tutorial-bikeshare-dataprep/tripdurationhistogram.png)

Histogram nie jest przydatny, ponieważ elementy odstające pochylają wykres.

## <a name="add-column-using-script"></a>Dodawanie kolumny za pomocą skryptu

1. Kliknij prawym przyciskiem myszy kolumnę **tripduration** i wybierz pozycję **Dodaj kolumnę (skrypt)**.

    ![Obraz menu dodawania kolumny (skrypt)](media/tutorial-bikeshare-dataprep/computecolscript.png)

2. W oknie dialogowym __Dodawanie kolumny (skrypt)__ użyj następujących wartości:

    * __Nowa nazwa kolumny__: logtripduration
    * __Wstaw tę nową kolumnę po__: tripduration
    * __Kod nowej kolumny__: `math.log(row.tripduration)`
    * __Typ bloku kodu__: wyrażenie

   ![Okno dialogowe Dodawanie kolumny (skrypt)](media/tutorial-bikeshare-dataprep/computecolscriptdialog.png)

3. Wybierz przycisk __OK__, aby dodać kolumnę **logtripduration**.

4. Kliknij kolumnę prawym przyciskiem myszy i wybierz pozycję **Histogram**.

    ![Histogram kolumny logtripduration](media/tutorial-bikeshare-dataprep/logtriphistogram.png)

  Wizualnie ten histogram jest podobny do rozkładu normalnego z nietypowym zakończeniem.

## <a name="use-advanced-filter"></a>Używanie filtru zaawansowanego

Użycie filtru do danych aktualizuje inspektorów dla nowej dystrybucji. Kliknij prawym przyciskiem myszy kolumnę **logtripduration** i wybierz pozycję **Filtruj kolumnę**. W oknie dialogowym __Edytowanie__ użyj następujących wartości:

* __Filtruj tę kolumnę liczbową__: logtripduration
* __Chcę__: zachowaj wiersze
* __Gdy__: dowolny z poniższych warunków ma wartość True (logiczne OR)
* __Jeśli ta kolumna__: mniejsza niż
* __Wartość__: 9

![Opcje filtru](media/tutorial-bikeshare-dataprep/loftripfilter.png)

Kliknij przycisk __OK__, aby zastosować filtr.

![Zaktualizowane histogramy po zastosowaniu filtru](media/tutorial-bikeshare-dataprep/loftripfilteredinspector.png)

### <a name="the-halo-effect"></a>Efekt obwódki

1. Maksymalizuj histogram kolumny **logtripduration**. Niebieski histogram nałożony na szary histogram. Ten ekran jest nazywany **efektem obwódki**:

    * **Szary histogram** reprezentuje dystrybucję przed operacją (w tym przypadku operacją filtrowania).
    * **Niebieski histogram** reprezentuje histogram po wykonaniu operacji. 

   Obwódka ułatwia wizualizację skutków operacji na danych.

   ![Obraz efektu obwódki](media/tutorial-bikeshare-dataprep/loftripfilteredinspectormaximized.png)

    > [!NOTE]
    > Zauważ, że niebieski histogram wygląda na krótszy w porównaniu do poprzedniego. Jest to spowodowane automatyczną ponowną obsługą zasobników danych w nowym zakresie.

2. Aby usunąć obwódki, zaznacz pozycję __Edytuj__ i usuń zaznaczenie pola wyboru __Pokaż obwódkę__.

    ![Opcje histogramu](media/tutorial-bikeshare-dataprep/uncheckhalo.png)

3. Wybierz przycisk **OK**, aby wyłączyć efekt obwódek, a następnie zminimalizuj histogram.

### <a name="remove-columns"></a>Usuwanie kolumn

W danych podróży każdy wiersz reprezentuje zdarzenie pobrania roweru. W tym samouczku będą potrzebne tylko kolumny **starttime** i **identyfikator stacji rozpoczęcia**. Usuń inne kolumny, stosując wybór wielokrotny do tych dwóch kolumn, kliknij prawym przyciskiem myszy nagłówek kolumny, a następnie wybierz pozycję **Zachowaj kolumnę**. Inne kolumny zostaną usunięte.

![Obraz opcji zachowywania kolumny](media/tutorial-bikeshare-dataprep/tripdatakeepcolumn.png)

### <a name="summarize-data-count"></a>Podsumowanie danych (licznik)

Aby podsumować zapotrzebowanie na rower dla okresu 2 godzin, użyj kolumn pochodnych.

1. Kliknij prawym przyciskiem myszy kolumnę **starttime** i wybierz pozycję **Tworzenie kolumn pochodnych według przykładu**

    ![Obraz opcji tworzenia według przykładu](media/tutorial-bikeshare-dataprep/tripdataderivebyexample.png)

2. Na przykład wprowadź wartość `Jan 01, 2017 12AM-2AM` dla pierwszego wiersza.

    > [!IMPORTANT]
    > W poprzednim przykładzie kolumn pochodnych użyto wielu kroków, aby uzyskać kolumnę zawierającą okres daty i godziny. W tym przykładzie możesz zobaczyć, że ta operacja może zostać wykonana jako pojedynczy krok, zapewniając przykład końcowych danych wyjściowych.

    > [!NOTE]
    > Możesz podać przykład dla dowolnego wiersza. W tym przykładzie wartość `Jan 01, 2017 12AM-2AM` jest prawidłowa dla pierwszego wiersza danych.

    ![Obraz przykładowych danych](media/tutorial-bikeshare-dataprep/tripdataderivebyexamplefirstexample.png)

   > [!IMPORTANT]
   > Dla komputerów Mac, należy wykonać następujący krok zamiast __krok 3__ poniżej.
   >
   > * Przejdź do pierwszej komórki, który zawiera `Jan 01, 2017 1AM-2AM`. Powinien być __wiersz 14__. Popraw wartość `Jan 01, 2017 12AM-2AM`, a następnie naciśnij klawisz __Enter__. 

3. Poczekaj, aż aplikacja obliczy wartości dla wszystkich wierszy. Może to potrwać kilka sekund. Po zakończeniu analizowania użyj linku __Przejrzyj następny sugerowany wiersz__ w celu sprawdzenia danych.

   ![Obraz ukończonej analizy z linkiem przeglądu](media/tutorial-bikeshare-dataprep/tripdatabyexanalysiscomplete.png)

    Upewnij się, że obliczone wartości są poprawne. Jeśli nie, zaktualizuj wartość za pomocą oczekiwanej wartości, a następnie naciśnij klawisz Enter. Następnie poczekaj na zakończenie analizy. Zakończenie procesu **Przejrzyj następny sugerowany wiersz** nastąpi po wyświetleniu komunikatu **Brak sugestii**. Gdy zobaczysz komunikat **Brak sugestii**, aplikacja sprawdziła przypadki brzegowe i jest zadowolona z syntetyzowanego programu. To jest najlepsze rozwiązanie do wykonania kontroli wizualnej przekształconych danych przed zaakceptowaniem przekształcenia. 

4. Wybierz **OK**, aby zaakceptować przekształcenie. Zmień nazwę nowo utworzonej kolumny na **Zakres daty godziny**.

    ![Obraz kolumny o zmienionej nazwie](media/tutorial-bikeshare-dataprep/tripdatasummarize.png)

5. Kliknij prawym przyciskiem myszy nagłówek kolumny **starttime**, a następnie wybierz pozycję **Usuń kolumnę**.

6. Aby podsumować dane, wybierz pozycję __Podsumuj__ z menu __Przekształcenie__. Aby utworzyć przekształcenie, wykonaj następujące czynności:

    * Przeciągnij kolumny __Zakres daty godziny__ i __Identyfikator stacji początkowej__ do okienka po lewej stronie (grupuj według).
    * Przeciągnij kolumnę __Identyfikator stacji początkowej__ do okienka po prawej stronie (podsumowanie danych).

   ![Obraz opcji podsumowania](media/tutorial-bikeshare-dataprep/tripdatacount.png)

7. Wybierz przycisk **OK**, aby zaakceptować wynik podsumowania.

## <a name="join-dataflows"></a>Dołączanie przepływów danych

Aby dołączyć dane pogody do danych podróży, wykonaj następujące kroki:

1. Wybierz pozycję __Dołącz__ z menu __Przekształcenia__.

2. __Tabele__: wybierz tabelę **BostonWeather** jako przepływ danych po lewej stronie i tabelę **201701-hubway-tripdata** jako przepływ danych po prawej stronie. Aby kontynuować, kliknij przycisk **Dalej**.

    ![Obraz opcji tabel](media/tutorial-bikeshare-dataprep/jointableselection.png)

3. __Kluczowe kolumny__: wybierz kolumnę **Zakres daty godziny** w obu tabelach, a następnie wybierz przycisk __Dalej__.

    ![Obraz przyłączenia kolumn klucza](media/tutorial-bikeshare-dataprep/joinkeyselection.png)

4. __Typ przyłączenia__: wybierz __Pasujące wiersze__ jako typ przyłączenia, a następnie wybierz pozycję __Zakończ__.

    ![Typ przyłączenia pasujących wierszy](media/tutorial-bikeshare-dataprep/joinscreen.png)

    Ten proces tworzy nowy przepływ danych o nazwie __Przyłącz wynik__.

## <a name="create-additional-features"></a>Tworzenie dodatkowych funkcji

1. Aby utworzyć kolumnę zawierającą dzień tygodnia, kliknij prawym przyciskiem myszy kolumnę **Zakres daty godziny** i wybierz pozycję **Tworzenie kolumny według przykładu**. Użyj wartości __Nd__ dla daty, który wystąpiła w niedzielę. Na przykład **01 stycznia 2017 0:00–2:00**. Naciśnij klawisz **Enter**, a następnie wybierz przycisk **OK**. Zmień nazwę tej kolumny na __Dni tygodnia__.

    ![Obraz tworzenia nowej kolumny zawierającej dzień tygodnia](media/tutorial-bikeshare-dataprep/featureweekday.png)

2. Aby utworzyć kolumnę zawierającą okres dla wiersza, kliknij prawym przyciskiem myszy kolumnę **Zakres daty godziny** i wybierz pozycję **Tworzenie kolumny według przykładu**. Użyj wartości **0:00–2:00** dla wiersza zawierającego **01 stycznia 2017 0:00–2:00**. Naciśnij klawisz **Enter**, a następnie wybierz przycisk **OK**. Zmień nazwę tej kolumny na **Okres**.

    ![Obraz kolumny okresu](media/tutorial-bikeshare-dataprep/featurehourrange.png)

3. Aby usunąć kolumny **Zakres daty godziny** i **Zakres rdaty godziny**, naciśnij klawisz **Ctrl (polecenie ⌘ dla komputerów Mac)** i zaznacz nagłówek każdej kolumny. Kliknij prawym przyciskiem myszy, a następnie wybierz pozycję **Usuń kolumnę**.

## <a name="read-data-from-python"></a>Odczytywanie danych z języka Python

Możesz uruchomić pakiet przygotowywania danych z języka Python lub PySpark i pobrać wynik w postaci **ramki danych**.

Aby wygenerować przykładowy skrypt w języku Python, kliknij prawym przyciskiem myszy pakiet __BikeShare Data Prep__ i wybierz pozycję __Generuj plik kodu dostępu do danych__. Przykładowy plik w języku Python jest tworzony w Twoim **folderze projektu** a także jest ładowany na karcie w ramach środowiska roboczego. Poniższy skrypt języka Python jest przykładem generowanego kodu:

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

W tym samouczku nazwa pliku to `BikeShare Data Prep.py`. Ten plik jest używany w dalszej części tego samouczka.

## <a name="save-test-data-as-a-csv-file"></a>Zapisywanie danych testowych jako pliku CSV

Aby zapisać przepływ danych **Dołącz wynik** do pliku CSV, musisz zmienić skrypt `BikeShare Data Prep.py`. 

1. Otwórz projekt do edycji w VSCode.

    ![openprojectinvscode.png](media/tutorial-bikeshare-dataprep/openprojectinvscode.png)

2. Zaktualizuj skrypt w języku Python w `BikeShare Data Prep.py` pliku, używając następującego kodu:

    ```python
    import pyspark

    from azureml.dataprep.package import run
    from pyspark.sql.functions import *

    # start Spark session
    spark = pyspark.sql.SparkSession.builder.appName('BikeShare').getOrCreate()

    # dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
    df = run('BikeShare Data Prep.dprep', dataflow_idx=2)
    df.show(n=10)
    row_count_first = df.count()

    # Example file name: 'wasb://data-files@bikesharestorage.blob.core.windows.net/testata'
    # 'wasb://<your container name>@<your azure storage name>.blob.core.windows.net/<csv folder name>
    blobfolder = 'Your Azure Storage blob path'

    df.write.csv(blobfolder, mode='overwrite') 

    # retrieve csv file parts into one data frame
    csvfiles = "<Your Azure Storage blob path>/*.csv"
    df = spark.read.option("header", "false").csv(csvfiles)
    row_count_result = df.count()
    print(row_count_result)
    if (row_count_first == row_count_result):
        print('counts match')
    else:
        print('counts do not match')
    print('done')
    ```

3. Zastąp `Your Azure Storage blob path` ze ścieżką do pliku wyjściowego, który zostanie utworzony. Zastąp zarówno `blobfolder` i `csvfiles` zmiennych.

## <a name="create-hdinsight-run-configuration"></a>Tworzenie konfiguracji uruchomienia usługi HDInsight

1. W aplikacji Azure Machine Learning Workbench otwórz okno wiersza polecenia, wybierz menu **Plik**, a następnie wybierz polecenie **Otwórz wiersz polecenia**. Wiersz polecenia uruchomi się w folderze projektu z monitem `C:\Projects\BikeShare>`.

 ![opencommandprompt.png](media/tutorial-bikeshare-dataprep/opencommandprompt.png)

   >[!IMPORTANT]
   >Musisz użyć okna wiersza polecenia (otwartego z aplikacji Workbench), aby wykonać poniższe kroki.

2. Użyj wiersza polecenia, aby zalogować się na platformie Azure. 

   Podczas uwierzytelniania w zasobach platformy Azure aplikacja Workbench i interfejs wiersza polecenia używają osobnych pamięci podręcznych poświadczeń. Wystarczy wykonać tę czynność jeden raz i poświadczenia zostaną zapamiętane do czasu wygaśnięcia tokenu z pamięci podręcznej. `az account list` Polecenie zwraca listę dostępnych subskrypcji do użytkownika logowania. Jeśli istnieje więcej niż jedna subskrypcja, użyj wartości identyfikatora z odpowiedniej subskrypcji. Ustaw jako konto domyślne, do korzystania z subskrypcji `az account set -s` polecenia, a następnie podaj wartość Identyfikatora subskrypcji. Potwierdź ustawienia przy użyciu konta `show` polecenia.

   ```azurecli
   REM login by using the aka.ms/devicelogin site
   az login
   
   REM lists all Azure subscriptions you have access to 
   az account list -o table
   
   REM sets the current Azure subscription to the one you want to use
   az account set -s <subscriptionId>
   
   REM verifies that your current subscription is set correctly
   az account show
   ```

3. Tworzenie konfiguracji uruchomienia usługi HDInsight. Konieczne będzie nazwy klastra i sshuser hasła.
    ```azurecli
    az ml computetarget attach --name hdinsight --address <yourclustername>.azurehdinsight.net --username sshuser --password <your password> --type cluster
    az ml experiment prepare -c hdinsight
    ```
> [!NOTE]
> Podczas tworzenia pustego projektu Uruchom domyślne konfiguracje są **lokalnego** i **docker**. Spowoduje to utworzenie nowej konfiguracji uruchamiania, dostępnym w **Azure Machine Learning Workbench** uruchamiania skryptów. 

## <a name="run-in-hdinsight-cluster"></a>Uruchom w klastrze usługi HDInsight

Wróć do **Azure Machine Learning Workbench** aplikację do uruchamiania skryptu w klastrze usługi HDInsight.

1. Wróć do ekranu głównego projektu, klikając **macierzystego** ikony po lewej stronie.

2. Wybierz **hdinsight** z listy rozwijanej, aby uruchomić skrypt w klastrze usługi HDInsight.

3. Wybierz **Uruchom** w górnej części ekranu. Skrypt jest przesyłana w postaci **zadania**. Gdy stan zadania zmieni się na __Ukończono__, plik został zapisany w określonej lokalizacji w sieci **kontenera magazynu Azure**.

    ![hdinsightrunscript.png](media/tutorial-bikeshare-dataprep/hdinsightrunscript.png)


## <a name="substitute-data-sources"></a>Zastępowanie źródeł danych

W poprzednich krokach były używane źródła danych `201701-hubway-tripdata.csv` i `BostonWeather.csv`, aby przygotować dane testowe. Aby użyć pakietu z innymi plikami danych podróży, wykonaj następujące czynności:

1. Utwórz nowe **źródło danych**, korzystając z podanych wcześniej kroków, z następującymi zmianami procesu:

    * __Wybór pliku__: po wybraniu pliku, dokonaj wielokrotnego wyboru sześciu pozostałych plików CSV tripdata.

        ![Ładowanie sześciu pozostałych plików](media/tutorial-bikeshare-dataprep/browseazurestoragefortripdatafiles.png)

        > [!NOTE]
        > Wpis __+5__ wskazuje, że istnieje pięć dodatkowych plików innych niż ten, który jest wymieniony.

    * __Szczegóły pliku__: ustaw __Tryb promocji nagłówków__ na **Wszystkie pliki mają ten sam nagłówek**. Ta wartość wskazuje, że każdy plik zawiera ten sam nagłówek.

        ![Wybieranie szczegółów pliku](media/tutorial-bikeshare-dataprep/headerfromeachfile.png) 

   Zapisz nazwę tego źródła danych, ponieważ jest ona używana w dalszych krokach.

2. Wybierz ikonę folderu, aby wyświetlić pliki w swoim projekcie. Rozwiń katalog __aml\_config__, a następnie wybierz plik `hdinsight.runconfig`.

    ![Obraz lokalizacji hdinsight.runconfig](media/tutorial-bikeshare-dataprep/hdinsightsubstitutedatasources.png) 

3. Kliknij przycisk Edytuj, aby otworzyć plik w VSCode.

4. Dodaj następujące wiersze na końcu pliku `hdinsight.runconfig`, a następnie wybierz ikonę dysku, aby zapisać plik.

    ```yaml
    DataSourceSubstitutions:
      201701-hubway-tripdata.dsource: 201501-hubway-tripdata.dsource
    ```

    Ta zmiana zastępuje oryginalne źródło danych takim, które zawiera sześć plików danych podróży.

## <a name="save-training-data-as-a-csv-file"></a>Zapisywanie danych szkoleniowych jako pliku CSV

Przejdź do pliku Python `BikeShare Data Prep.py`, który był edytowany wcześniej, i podaj inną ścieżkę pliku, aby zapisać dane szkoleniowe.

```python
import pyspark

from azureml.dataprep.package import run
from pyspark.sql.functions import *

# start Spark session
spark = pyspark.sql.SparkSession.builder.appName('BikeShare').getOrCreate()

# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)
df.show(n=10)
row_count_first = df.count()

# Example file name: 'wasb://data-files@bikesharestorage.blob.core.windows.net/traindata'
# 'wasb://<your container name>@<your azure storage name>.blob.core.windows.net/<csv folder name>
blobfolder = 'Your Azure Storage blob path'

df.write.csv(blobfolder, mode='overwrite') 

# retrieve csv file parts into one data frame
csvfiles = "<Your Azure Storage blob path>/*.csv"
df = spark.read.option("header", "false").csv(csvfiles)
row_count_result = df.count()
print(row_count_result)
if (row_count_first == row_count_result):
    print('counts match')
else:
    print('counts do not match')
print('done')
```

1. Użyj nazwy folderu `traindata` dla danych wyjściowych danych szkoleniowych.

2. Aby przesłać nowe zadanie, należy użyć ikony **Uruchom** w górnej części strony. Upewnij się, że **hdinsight** jest zaznaczone. **Zadanie** jest przesyłane z nową konfiguracją. Danymi wyjściowymi tego zadania są dane szkoleniowe. Te dane są tworzone przy użyciu tej samej utworzonej wcześniej procedury przygotowywania danych. Zakończenie tego zadania może potrwać kilka minut.


## <a name="next-steps"></a>Kolejne kroki
Samouczek przygotowywania danych udostępniania roweru został ukończony. W tym samouczku była używana usługa Azure Machine Learning (wersja zapoznawcza), aby dowiedzieć się, jak:
> [!div class="checklist"]
> * Interaktywnie przygotować dane za pomocą narzędzia do przygotowywania danych usługi Azure Machine Learning
> * Importować, przekształcać i tworzyć zestawy danych testowych
> * Tworzyć pakiet przygotowywania danych
> * Uruchamiać pakiet przygotowywania danych przy użyciu języka Python
> * Generować zestaw danych szkoleniowych poprzez ponowne użycie pakietu przygotowywania danych do dodatkowych plików wejściowych

Następnie dowiedz się więcej o przygotowywaniu danych:
> [!div class="nextstepaction"]
> [Data Preparation User Guide](data-prep-user-guide.md) (Podręcznik użytkownika przygotowywania danych)
