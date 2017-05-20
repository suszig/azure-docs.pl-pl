---
title: "Tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools dla Visual Studio | Microsoft Docs"
description: "Poznaj sposoby instalowania narzędzi Data Lake Tools dla Visual Studio oraz tworzenia i testowania skryptów U-SQL. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/06/2017
ms.author: edmaca, yanacai
ms.translationtype: Human Translation
ms.sourcegitcommit: 0b53a5ab59779dc16825887b3c970927f1f30821
ms.openlocfilehash: c26ac89bd7ef494331ba309aacf87de03506ac4c
ms.contentlocale: pl-pl
ms.lasthandoff: 04/07/2017


---
# <a name="tutorial-develop-u-sql-scripts-using-data-lake-tools-for-visual-studio"></a>Samouczek: tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools dla Visual Studio
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Napisz i przetestuj skrypty U-SQL przy użyciu narzędzi Data Lake Tools for Visual Studio.

U-SQL jest wysoko skalowalnym językiem o dużych możliwościach rozszerzania umożliwiającym przygotowywanie, przekształcanie i analizowanie wszystkich danych w ramach usługi Data Lake i poza nią. Więcej informacji znajduje się w temacie [Dokumentacja języka U-SQL](http://go.microsoft.com/fwlink/p/?LinkId=691348).

## <a name="prerequisites"></a>Wymagania wstępne
* **Visual Studio 2017 (z obciążeniem magazynu danych i przetwarzania), Visual Studio 2015 aktualizacja 3, Visual Studio 2013 aktualizacja 4 lub Visual Studio 2012. Wersje Enterprise (Ultimate/Premium), Professional, Community są obsługiwane; wersja Express nie jest obsługiwana.**
* **Zestaw Microsoft Azure SDK dla programu .NET w wersji 2.7.1 lub nowszej**.  Można go zainstalować przy użyciu [Instalatora platformy sieci Web](http://www.microsoft.com/web/downloads/platform.aspx).
* **[Data Lake Tools dla Visual Studio](http://aka.ms/adltoolsvs)**.

    Po zainstalowaniu narzędzi Data Lake Tools dla programu Visual Studio węzeł „Data Lake Analytics” zostanie wyświetlony w Eksploratorze serwera w węźle „Azure” (aby otworzyć Eksploratora serwera, naciśnij klawisze Ctrl+Alt+S).

* **Konto i przykładowe dane usługi Data Lake Analytics** Usługa Data Lake Tools nie obsługuje tworzenia kont usługi Data Lake Analytics. Utwórz konto przy użyciu witryny Azure Portal, programu Azure PowerShell, zestawu SDK platformy .NET lub interfejsu wiersza polecenia platformy Azure.
Dla wygody skrypt programu PowerShell na potrzeby tworzenia usługi Data Lake Analytics i przekazywania źródłowego pliku danych można znaleźć w [przykładowym pakiecie Appx-A programu PowerShell do przygotowania samouczka](data-lake-analytics-data-lake-tools-get-started.md#appx-a-powershell-sample-for-preparing-the-tutorial).

    Opcjonalnie możesz wykonać instrukcje zawarte w następujących dwóch sekcjach tematu [Rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu witryny Azure Portal](data-lake-analytics-get-started-portal.md), aby utworzyć konto i przekazać dane ręcznie:

    1. [Tworzenie konta usługi Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md#create-data-lake-analytics-account).
    2. [Przekazywanie pliku SearchLog.tsv do domyślnego konta usługi Data Lake Storage](data-lake-analytics-get-started-portal.md#prepare-source-data).

## <a name="connect-to-azure"></a>Nawiązywanie połączenia z usługą Azure
**Nawiązywanie połączenia z usługą Data Lake Analytics**

1. Otwórz program Visual Studio.
2. W menu **Widok** kliknij opcję **Eksplorator serwera**, aby otworzyć Eksplorator serwera, lub naciśnij klawisze **[CTRL] + [ALT] + S**.
3. Kliknij prawym przyciskiem myszy pozycję **Azure**, kliknij pozycję „Connect to Microsoft Azure Subscription” (Połącz z subskrypcją Microsoft Azure), a następnie wykonaj instrukcje.
4. W **Eksploratorze serwera** rozwiń węzeł **Azure**, a następnie rozwiń węzeł **Data Lake Analytics**. Zostanie wyświetlona lista kont usługi Data Lake Analytics, o ile jakieś istnieją. Nie można utworzyć kont usługi Data Lake Analytics w programie Visual Studio. Aby utworzyć konto, zobacz temat [Rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu portalu Azure](data-lake-analytics-get-started-portal.md) lub [Rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu programu Azure PowerShell](data-lake-analytics-get-started-powershell.md).

## <a name="upload-source-data-files"></a>Przekazywanie źródłowych plików danych
Niektóre dane zostały przekazane podczas wykonywania instrukcji zawartych we wcześniejszej sekcji **Wymagania wstępne** samouczka.  

Aby skorzystać z własnych danych, wykonaj następujące kroki dotyczące przekazywania danych z narzędzi Data Lake Tools.

**Przekazywanie plików do zależnego konta usługi Azure Data Lake**

1. W **Eksploratorze serwera** rozwiń węzeł **Azure**, rozwiń węzeł **Data Lake Analytics**, rozwiń konto usługi Data Lake Analytics, po czym rozwiń węzeł **Konta magazynu**. Zostanie wyświetlone domyślne konto usługi Data Lake Storage oraz połączone konta usługi Data Lake Storage i połączone konta usługi Azure Storage. Domyślne konto usługi Data Lake jest oznaczone jako „Domyślne konto magazynu”.
2. Kliknij prawym przyciskiem myszy domyślne konto usługi Data Lake Storage, a następnie kliknij pozycję **Explorer**.  Spowoduje to otwarcie okienka narzędzi Data Lake Tools dla programu Visual Studio Explorer.  Po lewej stronie wyświetlany jest widok drzewa, po prawej — widok zawartości.
3. Przejdź do folderu, do którego chcesz przekazać pliki.
4. Kliknij prawym przyciskiem myszy w dowolnym pustym miejscu, a następnie kliknij pozycję **Przekaż**.

    ![Skrypt U-SQL projektu U-SQL programu Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-upload-files.png)

**Przekazywanie plików do połączonego konta usługi Azure Blob Storage**

1. W **Eksploratorze serwera** rozwiń węzeł **Azure**, rozwiń węzeł **Data Lake Analytics**, rozwiń konto usługi Data Lake Analytics, po czym rozwiń węzeł **Konta magazynu**. Zostanie wyświetlone domyślne konto usługi Data Lake Storage oraz połączone konta usługi Data Lake Storage i połączone konta usługi Azure Storage.
2. Rozwiń konto usługi Azure Storage.
3. Kliknij prawym przyciskiem myszy kontener, do którego chcesz przekazać pliki, a następnie kliknij pozycję **Explorer**. Jeśli nie masz kontenera, musisz go najpierw utworzyć przy użyciu Portalu Azure, programu Azure PowerShell lub innych narzędzi.
4. Przejdź do folderu, do którego chcesz przekazać pliki.
5. Kliknij prawym przyciskiem myszy w dowolnym pustym miejscu, a następnie kliknij pozycję **Przekaż**.

## <a name="develop-u-sql-scripts"></a>Tworzenie skryptów U-SQL
Zadania usługi Data Lake Analytics są napisane w języku U-SQL. Aby dowiedzieć się więcej o języku U-SQL, zobacz [Wprowadzenie do języka U-SQL](data-lake-analytics-u-sql-get-started.md) i [Dokumentację języka U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

**Tworzenie i przesyłanie zadania usługi Data Lake Analytics**

1. W menu **Plik** kliknij pozycję **Nowy**, a następnie kliknij pozycję **Projekt**.
2. Wybierz typ **U-SQL Project** (Projekt U-SQL).

    ![nowy projekt U-SQL programu Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. Kliknij przycisk **OK**. Program Visual Studio tworzy rozwiązanie z użyciem pliku **Script.usql**.
4. Wprowadź następujący skrypt do pliku **Script.usql**:

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

        @res =
            SELECT *
            FROM @searchlog;        

        OUTPUT @res   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Ten skrypt U-SQL odczytuje źródłowy plik danych przy użyciu ekstraktora **Extractors.Tsv()**, a następnie tworzy plik csv przy użyciu ekstraktora **Outputters.Csv()**.

    Nie należy modyfikować tych dwóch ścieżek, jeśli plik źródłowy nie został skopiowany do innej lokalizacji.  Jeśli folder wyjściowy nie istnieje, usługa Data Lake Analytics go utworzy.

    Użycie ścieżek względnych jest łatwiejsze w przypadku plików przechowywanych na domyślnych kontach usługi Data Lake. Można także użyć ścieżek bezwzględnych.  Na przykład:

        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Aby uzyskać dostęp do plików na połączonych kontach usługi Storage, należy użyć ścieżek bezwzględnych.  Składnia dla plików przechowywanych na połączonym koncie usługi Azure Storage jest następująca:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

   > [!NOTE]
   > Uprawnienia dostępu do kontenerów obiektów Blob platformy Azure z publicznymi obiektami lub publicznymi kontenerami nie są obecnie obsługiwane.  
   >
   >

    Zwróć uwagę na następujące funkcje:

   * **IntelliSense**

       Nastąpi wyświetlenie automatycznie wypełnianej nazwy i członków dla zestawu wierszy, klas, baz danych, schematów i obiektów zdefiniowanych przez użytkownika (UDO).

       Funkcja IntelliSense dla obiektów katalogu (baz danych, schematów, tabel, obiektów UDO itp.) jest powiązana z kontem obliczeniowym. Możesz sprawdzić bieżące aktywne konto obliczeniowe, bazę danych i schemat na górnym pasku narzędzi oraz przełączać je za pomocą list rozwijanych.
   * **Rozwijanie * kolumn oznaczonych symbolem**

       Kliknij z prawej strony symbolu *. Poniżej symbolu * będzie widoczne niebieskie podkreślenie. Umieść kursor myszy na niebieskim podkreśleniu, a następnie kliknij strzałkę w dół.
       ![Rozwijanie narzędzi programu Visual Studio w usłudze Data Lake za pomocą symbolu *](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-expand-asterisk.png)

       Kliknij opcję **Expand Columns** (Rozwiń kolumny), a narzędzie zastąpi symbol * nazwami kolumn.
   * **Automatyczne formatowanie**

       Użytkownicy mogą zmieniać wcięcie skryptu U-SQL na podstawie struktury kodu w obszarze Edit (Edycja) -> Advanced (Zaawansowane):

     * Format Document (Formatuj dokument) (Ctrl+E, D): formatowanie całego dokumentu   
     * Format Selection (Formatuj zaznaczenie) (Ctrl + K, Ctrl + F): formatowanie zaznaczenia. Jeśli nie zaznaczono tekstu, ten skrót formatuje wiersz, w którym znajduje się kursor.  

       Wszystkie reguły formatowania można konfigurować, wybierając kolejno opcje Tools (Narzędzia) -> Options (Opcje) -> Text Editor (Edytor tekstów) -> SIP -> Formatting (Formatowanie).  
   * **Inteligentne wcięcie**

       Narzędzia Data Lake Tools dla programu Visual Studio umożliwiają automatyczne tworzenie wcięć w wyrażeniach podczas pisania skryptów. Funkcja ta jest domyślnie wyłączona. Użytkownicy muszą ją włączyć, wybierając opcje U-SQL -> Options and Settings (Opcje i ustawienia) -> Switches (Przełączniki) -> Enable Smart Indent (Włącz inteligentne wcięcie).
   * **Przejdź do definicji i Znajdź wszystkie odwołania**

       Kliknięcie prawym przyciskiem myszy nazwy zestawu wierszy/parametru/kolumny/obiektu UDO itp. i kliknięcie pozycji Go To Definition (Przejdź do definicji) (F12) umożliwia przejście do odpowiedniej definicji. Kliknięcie pozycji Find All References (Znajdź wszystkie odwołania) (Shift+F12) spowoduje wyświetlenie wszystkich odwołań.
   * **Wstawianie ścieżki platformy Azure**

       Zamiast zapamiętywać ścieżkę pliku na platformie Azure i wpisywać ją ręcznie podczas pisania skryptu, można skorzystać z prostej metody udostępnianej przez narzędzia Data Lake Tools dla programu Visual Studio: kliknąć prawym przyciskiem myszy w edytorze, po czym kliknąć opcję Insert Azure Path (Wstaw ścieżkę platformy Azure). Przejdź do pliku w oknie dialogowym przeglądarki obiektów blob platformy Azure. Kliknij przycisk **OK**. Ścieżka pliku zostanie wstawiona do kodu.
5. Określ konto usługi Data Lake Analytics, bazę danych i schemat. Możesz wybrać opcję **(local)** (lokalnie), aby uruchomić skrypt lokalnie do celów testowych. Aby uzyskać więcej informacji, zobacz temat [Uruchamianie skryptu U-SQL lokalnie](#run-u-sql-locally).

    ![Przesyłanie projektu U-SQL programu Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

    Więcej informacji znajduje się w temacie [Używanie katalogu U-SQL](data-lake-analytics-use-u-sql-catalog.md).
6. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy pozycję **Script.usql**, a następnie kliknij pozycję **Build Script** (Kompiluj skrypt). Sprawdź wynik w okienku danych wyjściowych.
7. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy pozycję **Script.usql**, a następnie kliknij pozycję **Submit Script** (Prześlij skrypt). Opcjonalnie możesz także kliknąć przycisk **Submit** (Prześlij) w okienku Script.usql.  Zobacz poprzedni zrzut ekranu.  Kliknij strzałkę w dół obok przycisku przesyłania, aby przesłać przy użyciu opcji zaawansowanych:
8. Wypełnij pole **Job Name** (Nazwa zadania), sprawdź dane w polu **Analytics Account** (Konto usługi Analytics), a następnie kliknij przycisk **Submit** (Prześlij). Po zakończeniu przesyłania wyniki przesyłania i link do zadania są dostępne w oknie wyników narzędzi Data Lake Tools dla programu Visual Studio.

    ![Przesyłanie projektu U-SQL programu Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
9. Kliknij przycisk Refresh (Odśwież), aby wyświetlić najbardziej aktualny stan zadania i odświeżyć ekran. W przypadku pomyślnego wykonania zadania widoczne będą karty **Job Graph** (Wykres zadania), **Meta Data Operations** (Operacje na metadanych), **State History** (Historia stanu), **Diagnostics** (Diagnostyka):

    ![Wykres wydajności zadania skryptu U-SQL programu Visual Studio w usłudze Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * Job Summary (Podsumowanie zadania). Informacje podsumowujące bieżące zadanie, np.: stan, postęp, czas wykonywania, nazwa środowiska uruchomieniowego, osoba przesyłająca itp.   
   * Job Details (Szczegóły zadania). Szczegółowe informacje dotyczące danego zadania, m.in. skryptu, zasobów, widoku wykonania wierzchołka.
   * Job Graph (Wykres zadania). Cztery wykresy umożliwiające wizualizację informacji o zadaniu, takich jak: postęp, odczyt danych, zapis danych, czas wykonywania, średni czas wykonywania na węzeł, przepływność wejściowa, przepływność wyjściowa.
   * Metadata Operations (Operacje na metadanych). Przedstawienie wszystkich operacji na metadanych.
   * State History (Historia stanu).
   * Diagnostics (Diagnostyka). Narzędzia Data Lake Tools dla programu Visual Studio automatycznie przeprowadzają diagnozę wykonywania zadań. W przypadku wystąpienia błędów lub problemów z wydajnością w zadaniach zostaną wysłane alerty. Aby uzyskać więcej informacji, zobacz część Diagnostyka zadań (link do ustalenia).

**Aby sprawdzić stan zadania**

1. W Eksploratorze serwera rozwiń węzeł **Azure**, rozwiń węzeł **Data Lake Analytics**, po czym rozwiń nazwę konta usługi Data Lake Analytics
2. Kliknij dwukrotnie opcję **Jobs** (Zadania), aby wyświetlić listę zadań.
3. Kliknij zadanie, aby zobaczyć stan.

**Aby wyświetlić dane wyjściowe zadania**

1. W **Eksploratorze serwera** rozwiń węzeł **Azure**, rozwiń węzeł **Data Lake Analytics**, rozwiń konto usługi Data Lake Analytics, rozwiń węzeł **Konta magazynu**, kliknij prawym przyciskiem myszy domyślne konto usługi Data Lake Storage, a następnie kliknij przycisk **Explorer**.
2. Kliknij dwukrotnie **dane wyjściowe**, aby otworzyć folder
3. Kliknij dwukrotnie plik **SearchLog z adltools.csv**.

### <a name="job-playback"></a>Odtwarzanie zadania
Funkcja odtwarzania zadania umożliwia obserwowanie postępu wykonywania zadania oraz wzrokowe wykrycie anomalii wydajności i wąskich gardeł. Ta funkcja może być używana przed zakończeniem wykonywania zadania (tj. w czasie, gdy zadanie jest aktywnie wykonywane), a także po zakończeniu jego wykonywania. Przeprowadzenie odtwarzania podczas wykonywania zadania umożliwia użytkownikowi odtwarzanie postępu aż do chwili obecnej.

**Aby wyświetlić postęp wykonywania zadania**  

1. Kliknij opcję **Load Profile** (Załaduj profil) w prawym górnym rogu. Zobacz poprzedni zrzut ekranu.
2. Kliknij przycisk Play (Odtwórz) w lewym dolnym rogu, aby sprawdzić postęp wykonywania zadania.
3. Podczas odtwarzania kliknij przycisk **Pause** (Wstrzymaj), aby zatrzymać odtwarzanie lub bezpośrednio przeciągnij pasek postępu do określonych pozycji.

### <a name="heat-map"></a>Mapa cieplna
Narzędzia Data Lake Tools dla programu Visual Studio oferują wybierane przez użytkownika kolorowe nakładki widoku zadania, które pozwalają wskazać postęp, dane we/wy, czas wykonywania i przepustowość we/wy każdego etapu. W ten sposób użytkownicy mogą bezpośrednio i intuicyjnie określać potencjalne problemy i dystrybucję właściwości zadania. Źródło danych do wyświetlania można wybrać z listy rozwijanej.  

## <a name="run-u-sql-locally"></a>Uruchamianie skryptu U-SQL lokalnie

Przy użyciu narzędzi Azure Data Lake Tools for Visual Studio i zestawu SDK U-SQL usługi Azure Data Lake można uruchamiać zadania U-SQL na stacji roboczej, podobnie jak w usłudze Azure Data Lake. Te dwie funkcje uruchamiania lokalnego pozwalają zaoszczędzić czas poświęcony na testowanie i debugowanie zadań U-SQL. 

* [Testowanie i debugowanie zadań U-SQL przy użyciu uruchamiania lokalnego i zestawu SDK U-SQL usługi Azure Data Lake](data-lake-analytics-data-lake-tools-local-run.md)


## <a name="see-also"></a>Zobacz też
Aby rozpocząć pracę z usługą Data Lake Analytics przy użyciu różnych narzędzi, zobacz:

* [Rozpoczynanie pracy z usługą Data Lake Analytics przy użyciu portalu Azure](data-lake-analytics-get-started-portal.md)
* [Rozpoczynanie pracy z usługą Data Lake Analytics przy użyciu programu Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Rozpoczynanie pracy z usługą Data Lake Analytics przy użyciu zestawu SDK programu .NET](data-lake-analytics-get-started-net-sdk.md)
* [Debugowanie kodu C# w zadaniach U-SQL](data-lake-analytics-debug-u-sql-jobs.md)

Aby poznać kod narzędzi Data Lake Tools for Visual Studio, zobacz [Use the Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md) (Używanie kodu Azure Data Lake Tools for Visual Studio).

Aby wyświetlić więcej tematów dotyczących programowania:

* [Analiza dzienników w sieci Web przy użyciu usługi Data Lake Analytics](data-lake-analytics-analyze-weblogs.md)
* [Tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools dla Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Wprowadzenie do języka U-SQL w usłudze Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md)
* [Opracowywanie operatorów języka U-SQL definiowanych przez użytkownika na potrzeby zadań usługi Data Lake Analytics](data-lake-analytics-u-sql-develop-user-defined-operators.md)

## <a name="appx-a-powershell-sample-for-preparing-the-tutorial"></a>Przykładowy pakiet Appx-A programu PowerShell do przygotowania samouczka
Poniższy skrypt programu PowerShell przygotowuje konto usługi Azure Data Lake Analytics i dane źródłowe, dlatego możesz przejść do tematu [Tworzenie skryptów U-SQL](data-lake-analytics-data-lake-tools-get-started.md#develop-u-sql-scripts).

    #region - used for creating Azure service names
    $nameToken = "<Enter an alias>"
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion

    #region - service names
    $resourceGroupName = $namePrefix + "rg"
    $dataLakeStoreName = $namePrefix + "adas"
    $dataLakeAnalyticsName = $namePrefix + "adla"
    $location = "East US 2"
    #endregion


    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - Create an Azure Data Lake Analytics service account
    Write-Host "Create a resource group ..." -ForegroundColor Green
    New-AzureRmResourceGroup `
        -Name  $resourceGroupName `
        -Location $location

    Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
    New-AzureRmDataLakeStoreAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeStoreName `
        -Location $location

    Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
    New-AzureRmDataLakeAnalyticsAccount `
        -Name $dataLakeAnalyticsName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultDataLake $dataLakeStoreName

    Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
    Get-AzureRmDataLakeAnalyticsAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeAnalyticsName  
    #endregion

    #region - prepare the source data
    Write-Host "Import the source data ..."  -ForegroundColor Green
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file.
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

    Write-Host "List the source data ..."  -ForegroundColor Green
    Get-AzureRmDataLakeStoreChildItem -Account $dataLakeStoreName -Path  "/Samples/Data/"
    #endregion

