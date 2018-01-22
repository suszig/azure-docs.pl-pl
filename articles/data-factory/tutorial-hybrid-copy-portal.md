---
title: "Kopiowanie danych z programu SQL Server do usługi Blob Storage przy użyciu usługi Azure Data Factory | Microsoft Docs"
description: "Dowiedz się, jak skopiować dane z lokalnego magazynu danych do chmury platformy Azure przy użyciu własnego środowiska Integration Runtime w usłudze Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/11/2018
ms.author: jingwang
ms.openlocfilehash: 7b734a76545dbcbddac3c7ad7beae60d662a9129
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="tutorial-copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage"></a>Samouczek: kopiowanie danych z lokalnej bazy danych programu SQL Server do usługi Azure Blob Storage
W tym samouczku użyjesz interfejsu użytkownika usługi Azure Data Factory, aby utworzyć potok usługi Data Factory, który kopiuje dane z lokalnej bazy danych programu SQL Server do usługi Azure Blob Storage. Utworzysz własne środowisko Integration Runtime (Self-hosted), służące do przenoszenia danych między lokalnym magazynem danych i magazynem danych w chmurze. 

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Azure Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz [dokumentację dotyczącą usługi Data Factory w wersji 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Ten artykuł nie zawiera szczegółowego wprowadzenia do usługi Data Factory. Aby uzyskać więcej informacji, zobacz [Wprowadzenie do usługi Azure Data Factory](introduction.md). 

Ten samouczek obejmuje wykonanie następujących kroków:

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Utwórz własne środowisko Integration Runtime.
> * Tworzenie połączonych zestawów programu SQL Server i usługi Azure Storage. 
> * Tworzenie zestawów danych programu SQL Server i usługi Azure Blob.
> * Tworzenie potoku z działaniem kopiowania do przenoszenia danych.
> * Uruchamianie przebiegu potoku.
> * Monitorowanie uruchomienia potoku.

## <a name="prerequisites"></a>Wymagania wstępne
### <a name="azure-subscription"></a>Subskrypcja platformy Azure
Jeśli nie masz jeszcze subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/).

### <a name="azure-roles"></a>Role platformy Azure
Aby utworzyć wystąpienia usługi Data Factory, konto użytkownika używane do logowania się na platformie Azure musi mieć przypisaną rolę *współautora* lub *właściciela* albo być *administratorem* subskrypcji platformy Azure. 

W witrynie Azure Portal wybierz swoją nazwę użytkownika w prawym górnym rogu i wybierz pozycję **Uprawnienia**, aby wyświetlić uprawnienia, które masz w subskrypcji. Jeśli masz dostęp do wielu subskrypcji, wybierz odpowiednią subskrypcję. Aby uzyskać przykładowe instrukcje dotyczące dodawania użytkownika do roli, zobacz artykuł [Dodawanie ról](../billing/billing-add-change-azure-subscription-administrator.md).

### <a name="sql-server-2014-2016-and-2017"></a>Program SQL Server 2014, 2016 oraz 2017
Podczas pracy z tym samouczkiem użyjesz lokalnej bazy danych programu SQL Server jako *źródłowego* magazynu danych. Potok usługi Data Factory tworzony w tym samouczku kopiuje dane z tej lokalnej bazy danych programu SQL Server (źródła) do usługi Azure Blob Storage (ujścia). Następnie utworzysz tabelę o nazwie **emp** w bazie danych programu SQL Server i wstawisz kilka przykładowych wpisów w tabeli. 

1. Uruchom program SQL Server Management Studio. Jeśli program nie jest jeszcze zainstalowany na używanej maszynie, przejdź do strony [pobierania programu SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). 

2. Połącz się z wystąpieniem programu SQL Server przy użyciu swoich poświadczeń. 

3. Utwórz przykładową bazę danych. W widoku drzewa kliknij prawym przyciskiem myszy pozycję **Bazy danych**, a następnie wybierz pozycję **Nowa baza danych**. 
 
4. W oknie **Nowa baza danych** wprowadź nazwę bazy danych, a następnie wybierz przycisk **OK**. 

5. Aby utworzyć tabelę **emp** i wstawić do niej przykładowe dane, uruchom następujący skrypt zapytania w bazie danych:

   ```
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO
    
    INSERT INTO emp (FirstName, LastName) VALUES ('John', 'Doe')
    INSERT INTO emp (FirstName, LastName) VALUES ('Jane', 'Doe')
    GO
   ```

6. W widoku drzewa kliknij prawym przyciskiem myszy utworzoną bazę danych, a następnie wybierz pozycję **Nowe zapytanie**.

### <a name="azure-storage-account"></a>Konto usługi Azure Storage
W tym samouczku używasz konta usługi Azure Storage ogólnego przeznaczenia (a dokładniej usługi Azure Blob Storage) jako docelowego magazynu danych (ujścia). Jeśli nie masz konta usługi Azure Storage ogólnego przeznaczenia, zobacz [Tworzenie konta magazynu](../storage/common/storage-create-storage-account.md#create-a-storage-account). Potok usługi Data Factory tworzony w tym samouczku kopiuje dane z lokalnej bazy danych programu SQL Server (źródła) do tego konta usługi Azure Blob Storage (ujścia). 

#### <a name="get-storage-account-name-and-account-key"></a>Pobieranie nazwy konta i klucza konta magazynu
W tym samouczku używasz nazwy i klucza swojego konta usługi Azure Storage. Pobierz nazwę i klucz konta usługi Storage, wykonując następujące czynności: 

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) przy użyciu nazwy użytkownika i hasła do konta platformy Azure. 

2. W okienku po lewej stronie wybierz pozycję **Więcej usług**, zastosuj filtrowanie według słowa kluczowego **magazyn**, a następnie wybierz pozycję **Konta magazynu**.

    ![Wyszukiwanie konta magazynu](media/tutorial-hybrid-copy-powershell/search-storage-account.png)

3. Na liście kont magazynu znajdź swoje konto magazynu, w razie potrzeby używając filtrowania, a następnie wybierz to konto. 

4. W oknie **Konto magazynu** wybierz pozycję **Klucze dostępu**.

    ![Pobieranie nazwy i klucza konta magazynu](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)

5. Skopiuj wartości z pól **Nazwa konta magazynu** i **klucz1** i wklej je do Notatnika lub innego edytora do późniejszego użycia z tym samouczkiem. 

#### <a name="create-the-adftutorial-container"></a>Tworzenie kontenera adftutorial 
W tej sekcji utworzysz kontener obiektów blob o nazwie **adftutorial** w usłudze Azure Blob Storage. 

1. W oknie **Konto magazynu** przełącz się do widoku **Przegląd**, a następnie wybierz pozycję **Obiekty blob**. 

    ![Wybieranie opcji Obiekty blob](media/tutorial-hybrid-copy-powershell/select-blobs.png)

2. W oknie **Blob Service** wybierz pozycję **Kontener**. 

    ![Przycisk dodawania kontenera](media/tutorial-hybrid-copy-powershell/add-container-button.png)

3. W oknie **Nowy kontener** w polu **Nazwa** wpisz nazwę **adftutorial**, a następnie wybierz pozycję **OK**. 

    ![Wprowadzanie nazwy kontenera](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)

4. Kliknij pozycję **adftutorial** na liście kontenerów.  

    ![Wybieranie kontenera](media/tutorial-hybrid-copy-powershell/seelct-adftutorial-container.png)

5. Pozostaw otwarte okno **kontenera** **adftutorial**. Będzie ona używana do weryfikacji danych wyjściowych na końcu tego samouczka. Usługa Data Factory automatycznie tworzy folder wyjściowy w tym kontenerze, więc Ty nie musisz go tworzyć.

    ![Okno kontenera](media/tutorial-hybrid-copy-powershell/container-page.png)


## <a name="create-a-data-factory"></a>Tworzenie fabryki danych
W tym kroku utworzysz fabrykę danych i uruchomisz interfejs użytkownika usługi Azure Data Factory, aby utworzyć potok w fabryce danych. 

1. Kliknij przycisk **Nowy** w lewym menu, kliknij pozycję **Dane + analiza**, a następnie kliknij pozycję **Data Factory**. 
   
   ![Nowy-> Fabryka danych](./media/tutorial-hybrid-copy-portal/new-azure-data-factory-menu.png)
2. Na stronie **Nowa fabryka danych** wprowadź wartość **ADFTutorialDataFactory** w polu **Nazwa**. 
      
     ![Strona Nowa fabryka danych](./media/tutorial-hybrid-copy-portal/new-azure-data-factory.png)
 
   Nazwa fabryki danych platformy Azure musi być **globalnie unikatowa**. Jeśli dla pola nazwy wystąpi poniższy błąd, zmień nazwę fabryki danych (np. twojanazwaADFTutorialDataFactory). Artykuł [Data Factory — Naming Rules (Usługa Data Factory — reguły nazewnictwa)](naming-rules.md) zawiera reguły nazewnictwa artefaktów usługi Data Factory.
  
     ![Strona Nowa fabryka danych](./media/tutorial-hybrid-copy-portal/name-not-available-error.png)
3. Wybierz **subskrypcję** Azure, w której chcesz utworzyć fabrykę danych. 
4. Dla opcji **Grupa zasobów** wykonaj jedną z następujących czynności:
     
      - Wybierz pozycję **Użyj istniejącej**, a następnie wybierz istniejącą grupę zasobów z listy rozwijanej. 
      - Wybierz pozycję **Utwórz nową**, a następnie wprowadź nazwę grupy zasobów.   
         
        Informacje na temat grup zasobów znajdują się w artykule [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md) (Używanie grup zasobów do zarządzania zasobami platformy Azure).  
4. Wybierz wartość **V2 (wersja zapoznawcza)** dla **wersji**.
5. Na liście **lokalizacja** wybierz lokalizację fabryki danych. Na liście rozwijanej są wyświetlane tylko obsługiwane lokalizacje. Magazyny danych (Azure Storage, Azure SQL Database itp.) i jednostki obliczeniowe (HDInsight itp.) używane przez fabrykę danych mogą mieścić się w innych regionach.
6. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**.     
7. Kliknij przycisk **Utwórz**.      
8. Na pulpicie nawigacyjnym jest widoczny następujący kafelek ze stanem: **Wdrażanie fabryki danych**. 

    ![kafelek Wdrażanie fabryki danych](media/tutorial-hybrid-copy-portal/deploying-data-factory.png)
9. Po zakończeniu tworzenia zostanie wyświetlona strona **Fabryka danych**, jak pokazano na poniższej ilustracji.
   
   ![Strona główna fabryki danych](./media/tutorial-hybrid-copy-portal/data-factory-home-page.png)
10. Kliknij kafelek **Tworzenie i monitorowanie**, aby w osobnej karcie uruchomić interfejs użytkownika usługi Azure Data Factory. 


## <a name="create-a-pipeline"></a>Tworzenie potoku

1. Na stronie **Wprowadzenie** kliknij przycisk **Utwórz potok**. Potok zostanie utworzony automatycznie. Potok jest wyświetlany w widoku drzewa, otwarty w edytorze. 

   ![Strona Wprowadzenie](./media/tutorial-hybrid-copy-portal/get-started-page.png)
2. Na karcie **Ogólne** okna **Właściwości** w dolnej części wprowadź jako **nazwę** wartość **SQLServerToBlobPipeline**.

   ![Nazwa potoku](./media/tutorial-hybrid-copy-portal/pipeline-name.png)
2. W przyborniku **Działania** rozwiń pozycję **Przepływ danych**, a następnie przeciągnij i upuść działanie **Copy** (Kopiowanie) do powierzchni projektu potoku. Ustaw nazwę działania na wartość **CopySqlServerToAzureBlobActivity**.

   ![Nazwa działania](./media/tutorial-hybrid-copy-portal/copy-activity-name.png)
3. W oknie Właściwości przejdź do karty **Źródło**, a następnie kliknij pozycję **+ Nowy**.

   ![Nowy zestaw danych źródłowych — przycisk](./media/tutorial-hybrid-copy-portal/source-dataset-new-button.png)
4. W oknie **Nowy zestaw danych** wyszukaj pozycję **SQL Server**, wybierz pozycję **SQL Server**, a następnie kliknij pozycję **Zakończ**. Zostanie wyświetlona nowa karta zatytułowana **SqlServerTable1**. Widoczny będzie także zestaw danych **SqlServerTable1** w widoku drzewa po lewej stronie. 

   ![Wybieranie pozycji SQL Server](./media/tutorial-hybrid-copy-portal/select-sql-server.png)
5. Na karcie **Ogólne** okna Właściwości wprowadź jako **nazwę** wartość **SqlServerDataset**.
    
   ![Zestaw danych źródłowych — nazwa](./media/tutorial-hybrid-copy-portal/source-dataset-name.png)
6. Przejdź do karty **Połączenia**, a następnie kliknij pozycję **+ Nowy**. W tym kroku tworzone jest połączenie z magazynem danych źródłowych (bazą danych programu SQL Server). 

   ![Zestaw danych źródłowych — przycisk nowego połączenia](./media/tutorial-hybrid-copy-portal/source-connection-new-button.png)
7. W oknie **Nowa połączona usługa** kliknij przycisk **Nowe środowisko Integration Runtime**. W tej sekcji utworzysz własne środowisko Integration Runtime i skojarzysz je z maszyną lokalną za pomocą bazy danych programu SQL Server. Własne środowisko Integration Runtime jest składnikiem, który kopiuje dane z bazy danych programu SQL Server na Twojej maszynie do usługi Azure Blob Storage. 

   ![Nowe środowisko Integration Runtime](./media/tutorial-hybrid-copy-portal/new-integration-runtime-button.png)
8. W oknie **instalatora środowiska Integration Runtime** wybierz pozycję **Sieć prywatna**, a następnie kliknij przycisk **Dalej**. 

   ![Wybór sieci prywatnej](./media/tutorial-hybrid-copy-portal/select-private-network.png)
9. Wprowadź nazwę środowiska Integration Runtime i kliknij przycisk **Dalej**.  
    
   ![Środowisko Integration Runtime — nazwa](./media/tutorial-hybrid-copy-portal/integration-runtime-name.png)
10. Kliknij pozycję **Click here to launch the express setup for this computer** (Kliknij tutaj, aby uruchomić instalację ekspresową dla tego komputera) w sekcji **Opcja 1: Instalacja ekspresowa**. 

   ![Kliknij link instalacji ekspresowej](./media/tutorial-hybrid-copy-portal/click-exress-setup.png)
11. W oknie **Instalacja ekspresowa środowiska Integration Runtime (Self-hosted)** kliknij przycisk **Zamknij**. 

   ![Instalacja środowiska Integration Runtime — pomyślna](./media/tutorial-hybrid-copy-portal/integration-runtime-setup-successful.png)
12. W przeglądarce internetowej w oknie **instalatora środowiska Integration Runtime** kliknij przycisk **Zakończ**. Powinien nastąpić powrót do okna **Nowa połączona usługa**.

   ![Instalacja środowiska Integration Runtime — zakończenie](./media/tutorial-hybrid-copy-portal/click-finish-integration-runtime-setup.png)
13. W oknie **Nowa połączona usługa** wykonaj następujące czynności:

    1. Wprowadź jako **nazwę** wartość **SqlServerLinkedService**.
    2. Upewnij się, że utworzone wcześniej środowisko Integration Runtime jest wyświetlane dla opcji **Connect via integration runtime** (Połącz za pomocą środowiska Integration Runtime).
    3. Jako **Nazwę serwera** podaj nazwę swojego serwera SQL Server. 
    4. Określ nazwę bazy danych za pomocą tabeli **emp** dla pola **Nazwa bazy danych**. 
    5. Wybierz odpowiedni **typ uwierzytelniania**, którego usługa Data Factory powinna używać do nawiązania połączenia z bazą danych SQL Server. 
    6. Wprowadź **nazwę użytkownika** i **hasło**. Jeśli musisz użyć znaku ukośnika (\\) w nazwie konta użytkownika lub nazwie serwera, poprzedź go znakiem ucieczki (\\). Na przykład: *mydomain\\\\myuser*. 
    7. Kliknij pozycję **Testuj połączenie**. Wykonaj ten krok, aby potwierdzić, że usługa Data Factory może połączyć się z bazą danych SQL Server przy użyciu utworzonego własnego środowiska Integration Runtime. 
    8. Aby zapisać połączoną usługę, kliknij przycisk **Zapisz**.

       ![Usługę połączona z serwerem SQL Server — ustawienia](./media/tutorial-hybrid-copy-portal/sql-server-linked-service-settings.png)
14. Powinien nastąpić powrót do okna z otwartym zestawem danych źródłowych. W sekcji **Połączenia** okna **Właściwości** wykonaj następujące czynności: 

    1. Upewnij się, że dla ustawienia **Połączona usługa** jest wyświetlana wartość **SqlServerLinkedService**. 
    2. Wybierz element **[dbo].[emp]** dla pozycji **Tabela**.

        ![Źródłowy zestaw danych — informacje o połączeniu](./media/tutorial-hybrid-copy-portal/source-dataset-connection.png)
15. Przejdź do karty z elementem SQLServerToBlobPipeline (lub) kliknij pozycję **SQLServerToBlobPipeline** w widoku drzewa. 

    ![Karta potoku](./media/tutorial-hybrid-copy-portal/pipeliene-tab.png)
16. W oknie **Właściwości** przejdź do karty **Ujście**, a następnie kliknij pozycję **+ Nowy**. 

    ![Zestaw danych ujścia — przycisk Nowy](./media/tutorial-hybrid-copy-portal/sink-dataset-new-button.png)
17. W oknie **Nowy zestaw danych** wybierz pozycję **Azure Blob Storage** i kliknij przycisk **Zakończ**. Zostanie otwarta nowa karta dla tego zestawu danych. Zestaw danych powinien być też widoczny w widoku drzewa. 

    ![Wybieranie pozycji Azure Blob Storage](./media/tutorial-hybrid-copy-portal/select-azure-blob-storage.png)
18. Wprowadź wartość **AzureBlobDataset** w polu **Nazwa**.

    ![Zestaw danych będący ujściem — nazwa](./media/tutorial-hybrid-copy-portal/sink-dataset-name.png)
19. Przejdź do karty **Połączenie** w oknie **Właściwości**, a następnie kliknij przycisk **+ Nowy** dla pozycji **Połączona usługa**. 

    ![Nowa połączona usługa— przycisk](./media/tutorial-hybrid-copy-portal/new-storage-linked-service-button.png)
20. W oknie **Nowa połączona usługa** wykonaj następujące czynności:

    1. Wprowadź wartość **AzureStorageLinkedService** w polu **Nazwa**.
    2. W polu **Nazwa konta magazynu** wybierz konto usługi Azure Storage. 
    3. Kliknij przycisk **Połączenie testowe**, aby przetestować połączenie z kontem usługi Azure Storage.
    4. Kliknij pozycję **Zapisz**.

        ![Połączona usługa Azure Storage — ustawienia](./media/tutorial-hybrid-copy-portal/azure-storage-linked-service-settings.png) 
21.  Powinien nastąpić powrót do okna z otwartym zestawem danych ujścia. Na karcie **Połączenie** wykonaj następujące czynności: 

        1. Upewnij się, że dla ustawienia **Połączona usługa** wybrano wartość **AzureStorageLinkedService**.
        2. Wprowadź wartość **adftutorial/incrementalcopy** jako część **folder** w polu **Ścieżka pliku**. Jeśli folder wyjściowy nie istnieje w kontenerze adftutorial, usługa Data Factory automatycznie utworzy folder wyjściowy.
        3. Wprowadź wartość `@CONCAT(pipeline().RunId, '.txt')` dla elementu **nazwa pliku** w polu **Ścieżka pliku**.

            ![Zestaw danych będący ujściem — połączenie](./media/tutorial-hybrid-copy-portal/sink-dataset-connection.png)
22. Przejdź do karty z otwartym potokiem lub kliknij **potok** w **widoku drzewa**. Upewnij się, że dla ustawienia **Zestaw danych ujścia** wybrano wartość **AzureBlobDataset**. 

    ![Wybrany zestaw danych ujścia ](./media/tutorial-hybrid-copy-portal/sink-dataset-selected.png)
23. Aby sprawdzić poprawność ustawień potoku, kliknij pozycję Weryfikuj na pasku narzędzi dla potoku. Zamknij **raport weryfikacji potoku**, klikając **X** w prawym rogu ekranu. 

    ![Weryfikowanie potoku](./media/tutorial-hybrid-copy-portal/validate-pipeline.png)
1. Opublikuj utworzone jednostki w usłudze Azure Data Factory, klikając przycisk **Opublikuj**.

    ![Przycisk Opublikuj](./media/tutorial-hybrid-copy-portal/publish-button.png)
24. Poczekaj, aż zostanie wyświetlone okienko wyskakujące z komunikatem **Publikowanie powiodło się**. Możesz również sprawdzić stan publikowania, klikając link **Pokaż powiadomienia** po lewej stronie. Zamknij okno powiadomienia, klikając przycisk **X**. 

    ![Publikowanie powiodło się](./media/tutorial-hybrid-copy-portal/publishing-succeeded.png)
    

## <a name="trigger-a-pipeline-run"></a>Wyzwalanie uruchomienia potoku
Kliknij pozycję **Wyzwól** na pasku narzędzi dla potoku, a następnie kliknij pozycję **Wyzwól teraz**.

![Wyzwól teraz](./media/tutorial-hybrid-copy-portal/trigger-now.png)

## <a name="monitor-the-pipeline-run"></a>Monitorowanie działania potoku

1. Przejdź do karty **Monitorowanie**. Zostanie wyświetlony potok, który został ręcznie uruchomiony w poprzednim kroku. 

    ![Uruchomienia potoków](./media/tutorial-hybrid-copy-portal/pipeline-runs.png)
2. Aby wyświetlić uruchomienia działań skojarzone z uruchomieniem potoku, kliknij link **Wyświetl uruchomienia działań** w kolumnie **Akcje**. W potoku jest tylko jedno działanie, dlatego na liście są wyświetlane uruchomienia tylko jednego działania. Aby wyświetlić szczegółowe informacje na temat operacji kopiowania, kliknij link **Szczegóły** (ikona okularów) w kolumnie **Działania**. Aby wrócić do widoku uruchomień potoków, kliknij pozycję **Potoki** u góry.

    ![Uruchomienia działania](./media/tutorial-hybrid-copy-portal/activity-runs.png)

## <a name="verify-the-output"></a>Sprawdzanie danych wyjściowych
Potok automatycznie tworzy folder wyjściowy o nazwie *fromonprem* w kontenerze obiektów blob `adftutorial`. Upewnij się, że plik *dbo.emp.txt* jest widoczny w folderze wyjściowym. 

1. W witrynie Azure Portal w oknie kontenera **adftutorial** kliknij przycisk **Odśwież**, aby wyświetlić folder wyjściowy.

    ![Utworzony folder wyjściowy](media/tutorial-hybrid-copy-portal/fromonprem-folder.png)
2. Wybierz pozycję `fromonprem` na liście folderów. 
3. Upewnij się jest wyświetlany plik o nazwie `dbo.emp.txt`.

    ![Plik wyjściowy](media/tutorial-hybrid-copy-portal/fromonprem-file.png)


## <a name="next-steps"></a>Następne kroki
Potok w tym przykładzie kopiuje dane z jednej lokalizacji do innej lokalizacji w usłudze Azure Blob Storage. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Utwórz własne środowisko Integration Runtime.
> * Tworzenie połączonych zestawów programu SQL Server i usługi Azure Storage. 
> * Tworzenie zestawów danych programu SQL Server i usługi Azure Blob.
> * Tworzenie potoku z działaniem kopiowania do przenoszenia danych.
> * Uruchamianie przebiegu potoku.
> * Monitorowanie uruchomienia potoku.

Lista magazynów danych obsługiwanych przez usługę Data Factory znajduje się w artykule dotyczącym [obsługiwanych magazynów danych](copy-activity-overview.md#supported-data-stores-and-formats).

Przejdź do poniższego samouczka, aby dowiedzieć się o zbiorczym kopiowaniu danych z lokalizacji źródłowej do docelowej:

> [!div class="nextstepaction"]
>[Zbiorcze kopiowanie danych](tutorial-bulk-copy-portal.md)
