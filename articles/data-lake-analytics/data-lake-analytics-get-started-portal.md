<properties 
   pageTitle="Rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu Portalu Azure | Azure" 
   description="Poznaj sposoby tworzenia konta usługi Data Lake Analytics przy użyciu Portalu Azure, tworzenia zadania usługi Data Lake Analytics przy użyciu języka U-SQL oraz przesyłania zadania. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# Samouczek: rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu Portalu Azure

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Poznaj sposoby tworzenia konta usługi Azure Data Lake Analytics przy użyciu witryny Azure Portal, definiowania zadań usługi Data Lake Analytics w języku [U-SQL](data-lake-analytics-u-sql-get-started.md) oraz przesyłania zadań na konta usługi Data Lake Analytics. Więcej informacji na temat usługi Data Lake Analytics można znaleźć w artykule [Omówienie usługi Azure Data Lake Analytics](data-lake-analytics-overview.md).

W ramach tego samouczka utworzysz zadanie, które odczytuje zawartość pliku z wartościami rozdzielanymi tabulatorami (TSV) i konwertuje je do pliku z wartościami rozdzielanymi przecinkami (CSV). Aby wykonać kroki opisane w tym samouczku, korzystając z innych obsługiwanych narzędzi, kliknij odpowiednią kartę w górnej części tej sekcji. Po pomyślnym utworzeniu pierwszego zadania możesz zacząć tworzyć bardziej złożone przekształcenia danych w języku U-SQL.

##Wymagania wstępne

Przed przystąpieniem do wykonania kroków opisanych w tym samouczku należy dysponować następującymi elementami:

- **Subskrypcja platformy Azure**. Zobacz artykuł [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

##Tworzenie konta usługi Data Lake Analytics

Aby można było uruchomić jakiekolwiek zadanie, musisz mieć konto usługi Data Lake Analytics.

Każde konto usługi Data Lake Analytics jest w relacji zależności z kontem usługi [Azure Data Lake Store]().  To konto jest określane jako domyślne konto usługi Data Lake Store.  Konto usługi Data Lake Store można utworzyć wcześniej lub podczas tworzenia konta usługi Data Lake Analytics. Podczas pracy z tym samouczkiem utworzysz konto usługi Data Lake Store z kontem usługi Data Lake Analytics.

**Aby utworzyć konto usługi Data Lake Analytics**

1. Zaloguj się do nowego [klasycznego portalu Azure](https://portal.azure.com).
2. Kliknij przycisk **Nowe**, kliknij pozycję **Dane i analiza**, a następnie kliknij pozycję **Data Lake Analytics**.
6. Wpisz lub wybierz wartości poniższych pozycji:

    ![Blok portalu usługi Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **Nazwa**: nazwa konta usługi Analytics.
    - **Data Lake — magazyn**: każde konto usługi Data Lake Analytics ma zależne konto usługi Data Lake Store. Konto usługi Data Lake Analytics i zależne konto usługi Data Lake Store muszą znajdować się w tym samym centrum danych Azure. Postępuj zgodnie z instrukcjami, aby utworzyć nowe konto usługi Data Lake Store lub wybierz konto istniejące.
    - **Subskrypcja**: wybierz subskrypcję platformy Azure używaną na potrzeby konta usługi Analytics.
    - **Grupa zasobów**. Wybierz istniejącą grupę zasobów platformy Azure lub utwórz nową. Usługa Azure Resource Manager (ARM) umożliwia pracę z zasobami w aplikacji w ramach grupy. Aby uzyskać więcej informacji, zobacz temat [Omówienie usługi Azure Resource Manager](resource-group-overview.md). 
    - **Lokalizacja**. Wybierz centrum danych platformy Azure na potrzeby konta usługi Data Lake Analytics. 
7. Wybierz pozycję **Przypnij do tablicy startowej**. Jest to wymagane w celu wykonania kroków tego samouczka.
8. Kliknij przycisk **Utwórz**. Spowoduje to przejście do tablicy startowej portalu. Do tablicy startowej zostanie dodany nowy kafelek z etykietą „Deploying Azure Data Lake Analytics” (Wdrażanie usługi Azure Data Lake Analytics). Utworzenie konta usługi Data Lake Analytics może potrwać kilka minut. Utworzone konto jest wyświetlane w nowym bloku otwartego portalu.

    ![Blok portalu usługi Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)


Po utworzeniu konta usługi Data Lake Analytics można dodać dodatkowe konta usługi Data Lake Store i konta usługi Azure Storage. Aby uzyskać instrukcje, zobacz temat [Zarządzanie źródłami danych konta usługi Data Lake Analytics](data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

##Przygotowanie danych źródłowych

W ramach tego samouczka przetworzymy wybrane dzienniki wyszukiwania.  Dziennik wyszukiwania może być przechowywany w usłudze Data Lake Store lub w usłudze Azure Blob Storage. 

Portal Azure udostępnia interfejs użytkownika umożliwiający skopiowanie przykładowych plików danych na domyślne konto usługi Data Lake. Pliki te obejmują również dziennik wyszukiwania.

**Aby skopiować przykładowe pliki danych**

1. W witrynie Azure Portal kliknij w lewym górnym rogu pozycję **Microsoft Azure**.
2. Kliknij kafelek z nazwą konta usługi Data Lake Analytics.  Został on przypięty w tym miejscu podczas tworzenia konta.
Jeśli konto nie zostało przypięte, zobacz temat [Otwieranie konta usługi Data Lake Analytics w portalu](data-lake-analytics-manage-use-portal.md#access-adla-account), aby otworzyć konto.
3. Rozwiń węzeł **Essentials** (Podstawowe elementy), a następnie kliknij pozycję **Explore sample jobs** (Eksploruj przykładowe zadania). Spowoduje to otwarcie kolejnego bloku o nazwie **Sample Jobs** (Przykładowe zadania).
4. Kliknij pozycję **Copy Sample Data** (Kopiuj przykładowe dane), a następnie kliknij przycisk **OK**, aby potwierdzić.
5. Kliknij przycisk **Notification** (Powiadomienie), który ma postać ikony w kształcie dzwonka. Powinien zostać wyświetlony dziennik zawierający komunikat **Updating sample data completed** (Aktualizowanie przykładowych danych zostało ukończone). Kliknij w dowolnym miejscu poza okienkiem powiadomienia, aby je zamknąć.
7. W bloku konta usługi Data Lake Analytics kliknij przycisk **Eksplorator danych** w górnej części. 

    ![Przycisk eksploratora danych usługi Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-data-explorer-button.png)

    Zostaną otwarte dwa bloki. Jeden z nich to blok **Eksplorator danych**, a drugi to blok domyślnego konta usługi Data Lake Store.
8. W bloku domyślnego konta usługi Data Lake Store kliknij pozycję **Przykłady**, aby rozwinąć folder, a następnie kliknij pozycję **Dane**, aby rozwinąć folder. Powinny zostać wyświetlone następujące pliki i foldery.

    - AmbulanceData/
    - AdsLog.tsv
    - SearchLog.tsv
    - version.txt
    - WebLog.log
    
    Na potrzeby tego samouczka zostanie użyty plik SearchLog.tsv.

W praktyce należy zaprogramować aplikacje do zapisywania danych na połączonych kontach magazynu lub przekazać dane. Aby uzyskać informacje na temat przekazywania plików, zobacz temat [Przekazywanie danych do usługi Data Lake Store](data-lake-analytics-manage-use-portal.md#upload-data-to-adls) lub [Przekazywanie danych do usługi Blob Storage](data-lake-analytics-manage-use-portal.md#upload-data-to-wasb).

##Tworzenie i przesyłanie zadań usługi Data Lake Analytics

Po przygotowaniu danych źródłowych możesz rozpocząć tworzenie skryptu U-SQL.  

**Aby przesłać zadanie**

1. W bloku konta usługi Data Lake Analytics w portalu kliknij pozycję **Nowe zadanie**. 

    ![Przycisk nowego zadania usługi Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job-button.png)

    Jeśli blok nie jest widoczny, zobacz temat [Otwieranie konta usługi Data Lake Analytics w portalu](data-lake-analytics-manage-use-portal.md#access-adla-account).
4. Wprowadź nazwę w polu **Nazwa zadania** oraz poniższy skrypt U-SQL:

    ![Tworzenie zadań U-SQL w usłudze Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job.png)

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Ten skrypt U-SQL odczytuje źródłowy plik danych przy użyciu ekstraktora **Extractors.Tsv()**, a następnie tworzy plik csv przy użyciu ekstraktora **Outputters.Csv()**. 
    
    Nie należy modyfikować tych dwóch ścieżek, jeśli plik źródłowy nie został skopiowany do innej lokalizacji.  Jeśli folder wyjściowy nie istnieje, usługa Data Lake Analytics go utworzy.  W tym przypadku użyto prostych ścieżek względnych.  
    
    Użycie ścieżek względnych jest łatwiejsze w przypadku plików przechowywanych na domyślnych kontach usługi Data Lake. Można także użyć ścieżek bezwzględnych.  Na przykład: 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
      

    Aby dowiedzieć się więcej o języku U-SQL, zobacz [Wprowadzenie do języka U-SQL w usłudze Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md) i [Dokumentację języka U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).
     
5. Kliknij przycisk **Prześlij zadanie** u górnej części. Zostanie otwarte nowe okienko Szczegóły zadania. Na pasku tytułu widoczny jest stan zadania.   
6. Poczekaj, aż stan zadania zmieni się na **Zakończono pomyślnie**. Po zakończeniu zadania szczegóły zadania zostaną wyświetlone w nowym bloku portalu:

    ![Szczegóły zadania usługi Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-job-completed.png)

    Na poprzednim zrzucie ekranu widać, że wykonanie zadania i zmiana jego stanu z Przesłano na Zakończono zajęło około 1,5 minuty.
    
    Jeśli zadanie zakończyło się niepowodzeniem, zobacz temat [Monitorowanie zadań usługi Data Lake Analytics i rozwiązywanie problemów](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorials.md).

7. W dolnej części bloku **Szczegóły zadania** kliknij nazwę zadania w pozycji **SearchLog-from-Data-Lake.csv**. Można wyświetlić podgląd pliku wyjściowego, pobrać go, zmienić jego nazwę i go usunąć.

    ![Właściwości pliku wyjściowego zadania usługi Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-output-file-properties.png)
8. Kliknij przycisk **Podgląd**, aby wyświetlić plik wyjściowy.

    ![Podgląd pliku wyjściowego zadania usługi Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-job-output-preview.png)

##Zobacz też

- Aby uzyskać informacje na temat bardziej złożonego zapytania, zobacz temat [Analizowanie dzienników witryn sieci Web przy użyciu usługi Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Aby rozpocząć tworzenie aplikacji w języku U-SQL, zobacz artykuł [Tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools dla Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Aby dowiedzieć się więcej o języku U-SQL, zobacz [Wprowadzenie do języka U-SQL w usłudze Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
- Informacje o zadaniach zarządzania znajdziesz w artykule [Zarządzanie usługą Azure Data Lake Analytics przy użyciu witryny Azure Portal](data-lake-analytics-manage-use-portal.md).
- Aby zapoznać się z omówieniem usługi Data Lake Analytics, zobacz [Omówienie usługi Azure Data Lake Analytics](data-lake-analytics-overview.md).
- Aby wyświetlić ten samouczek przy użyciu innych narzędzi, kliknij odpowiedni selektor karty w górnej części strony.
- Aby rejestrować informacje diagnostyczne, zobacz [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md) (Dostęp do dzienników diagnostycznych usługi Azure Data Lake Analytics)



<!--HONumber=Sep16_HO3-->


