---
title: "Tworzenie potoku usługi Data Factory za pomocą witryny Azure Portal | Microsoft Docs"
description: "Ten samouczek zawiera instrukcje krok po kroku dotyczące tworzenia fabryki danych z potokiem za pomocą witryny Azure Portal. Potok wykorzystuje działanie kopiowania do skopiowania danych z usługi Azure Blob Storage do usługi Azure SQL Database. "
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
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: 7486e7c6816538fc120fd0b0a8bea0b006fb21f0
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="copy-data-from-azure-blob-to-azure-sql-database-using-azure-data-factory"></a>Kopiowanie danych z obiektu blob platformy Azure do bazy danych Azure SQL Database przy użyciu usługi Azure Data Factory
W tym samouczku utworzysz fabrykę danych przy użyciu interfejsu użytkownika usługi Azure Data Factory. Potok w tej fabryce danych kopiuje dane z usługi Azure Blob Storage do usługi Azure SQL Database. Wzorzec konfiguracji w tym samouczku ma zastosowanie do kopiowania danych z magazynu opartego na plikach do relacyjnego magazynu danych. Aby zapoznać się z listą magazynów danych obsługiwanych jako źródła i ujścia, zobacz tabelę zawierającą [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats).

> [!NOTE]
> - Jeśli jesteś nowym użytkownikiem usługi Azure Data Factory, zobacz [Wprowadzenie do usługi Azure Data Factory](introduction.md).
>
> - Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz artykuł z [wprowadzeniem do usługi Data Factory w wersji 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Ten samouczek obejmuje następujące procedury:

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Tworzenie potoku z działaniem kopiowania.
> * Testowe uruchamianie potoku
> * Ręczne wyzwalanie potoku
> * Wyzwalanie potoku zgodnie z harmonogramem
> * Monitorowanie uruchomień potoku i działań.

## <a name="prerequisites"></a>Wymagania wstępne
* **Subskrypcja platformy Azure**. Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.
* **Konto usługi Azure Storage**. Magazyn obiektów blob jest używany jako **źródłowy** magazyn danych. Jeśli nie masz konta usługi Azure Storage, utwórz je, wykonując czynności przedstawione w artykule [Tworzenie konta magazynu](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* **Usługa Azure SQL Database**. Baza danych jest używana jako magazyn danych **ujścia**. Jeśli nie masz bazy danych Azure SQL Database, utwórz ją, wykonując czynności przedstawione w artykule [Create an Azure SQL database (Tworzenie bazy danych Azure SQL Database)](../sql-database/sql-database-get-started-portal.md).

### <a name="create-a-blob-and-a-sql-table"></a>Tworzenie obiektu blob i tabeli SQL

Teraz przygotuj swój obiekt Blob platformy Azure i bazę danych Azure SQL Database w ramach tego samouczka, wykonując następujące kroki:

#### <a name="create-a-source-blob"></a>Tworzenie źródłowego obiektu Blob

1. Uruchom program Notatnik. Skopiuj poniższy tekst i zapisz go na dysku jako plik **emp.txt**.

    ```
    John,Doe
    Jane,Doe
    ```

2. W usłudze Azure Blob Storage utwórz kontener o nazwie **adftutorial**. W tym kontenerze utwórz folder o nazwie **input**. Następnie przekaż plik **emp.txt** do folderu **input**. Do wykonania tych zadań użyj witryny Azure Portal lub narzędzi takich jak [Eksplorator usługi Azure Storage](http://storageexplorer.com/).

#### <a name="create-a-sink-sql-table"></a>Tworzenie tabeli SQL ujścia

1. Poniższy skrypt SQL umożliwia utworzenie tabeli **dbo.emp** w bazie danych Azure SQL Database.

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

2. Zezwól usługom platformy Azure na dostęp do serwera SQL. Upewnij się, że ustawienie **Zezwalaj na dostęp do usług Azure** jest włączone i ma wartość **WŁĄCZ** dla serwera SQL platformy Azure, aby usługa Data Factory mogła zapisywać dane na serwerze Azure SQL. W celu sprawdzenia i włączenia tego ustawienia wykonaj następujące kroki:

    1. Kliknij centrum **Więcej usług** po lewej stronie, a następnie kliknij przycisk **Serwery SQL**.
    2. Wybierz swój serwer, a następnie kliknij przycisk **Zapora** w obszarze **USTAWIENIA**.
    3. Na stronie **Ustawienia zapory** kliknij pozycję **WŁĄCZ** dla ustawienia **Zezwalaj na dostęp do usług platformy Azure**.

## <a name="create-a-data-factory"></a>Tworzenie fabryki danych
W tym kroku utworzysz fabrykę danych i uruchomisz interfejs użytkownika usługi Azure Data Factory, aby utworzyć potok w fabryce danych. 

1. Kliknij przycisk **Nowy** w lewym menu, kliknij pozycję **Dane + analiza**, a następnie kliknij pozycję **Data Factory**. 
   
   ![Nowy-> Fabryka danych](./media/tutorial-copy-data-portal/new-azure-data-factory-menu.png)
2. Na stronie **Nowa fabryka danych** wprowadź wartość **ADFTutorialDataFactory** w polu **Nazwa**. 
      
     ![Strona Nowa fabryka danych](./media/tutorial-copy-data-portal/new-azure-data-factory.png)
 
   Nazwa fabryki danych platformy Azure musi być **globalnie unikatowa**. Jeśli dla pola nazwy wystąpi poniższy błąd, zmień nazwę fabryki danych (np. twojanazwaADFTutorialDataFactory). Artykuł [Data Factory — Naming Rules (Usługa Data Factory — reguły nazewnictwa)](naming-rules.md) zawiera reguły nazewnictwa artefaktów usługi Data Factory.
  
     ![Strona Nowa fabryka danych](./media/tutorial-copy-data-portal/name-not-available-error.png)
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

    ![kafelek Wdrażanie fabryki danych](media/tutorial-copy-data-portal/deploying-data-factory.png)
9. Po zakończeniu tworzenia zostanie wyświetlona strona **Fabryka danych**, jak pokazano na poniższej ilustracji.
   
   ![Strona główna fabryki danych](./media/tutorial-copy-data-portal/data-factory-home-page.png)
10. Kliknij kafelek **Tworzenie i monitorowanie**, aby w osobnej karcie uruchomić interfejs użytkownika usługi Azure Data Factory.

## <a name="create-a-pipeline"></a>Tworzenie potoku
W tym kroku utworzysz potok z działaniem kopiowania w fabryce danych. Działanie kopiowania kopiuje dane z usługi Azure Blob Storage do usługi Azure SQL Database. W [samouczku szybkiego startu](quickstart-create-data-factory-portal.md) utworzono potok, wykonując następujące czynności:

1. Utworzenie połączonej usługi. 
2. Utworzenie wejściowych i wyjściowych zestawów danych.
3. Następnie utworzenie potoku.

W tym samouczku rozpoczniesz od utworzenia potoku, a połączone usługi i zestawy danych zostaną utworzone, gdy będą potrzebne do skonfigurowania potoku. 

1. Na stronie wprowadzenia kliknij kafelek **Utwórz potok**. 

   ![Kafelek Utwórz potok](./media/tutorial-copy-data-portal/create-pipeline-tile.png)
3. W oknie **Właściwości** potoku ustaw **nazwę** potoku na **CopyPipeline**.

    ![Nazwa potoku](./media/tutorial-copy-data-portal/pipeline-name.png)
4. W przyborniku **Działania** rozwiń kategorię **Przepływ danych**, a następnie przeciągnij działanie **Kopiowanie** z przybornika i upuść je na powierzchni projektanta. 

    ![Przeciąganie i upuszczanie działania kopiowania](./media/tutorial-copy-data-portal/drag-drop-copy-activity.png)
5. Na karcie **Ogólne** w oknie **Właściwości** podaj wartość **CopyFromBlobToSql** jako nazwę działania.

    ![Nazwa działania](./media/tutorial-copy-data-portal/activity-name.png)
6. Przejdź do karty **Źródło**. Kliknij przycisk **+ Nowy**, aby utworzyć zestaw danych źródłowych. 

    ![Menu Nowy zestaw danych źródłowych](./media/tutorial-copy-data-portal/new-source-dataset-button.png)
7. W oknie **Nowy zestaw danych** wybierz pozycję **Azure Blob Storage** i kliknij przycisk **Zakończ**. Dane źródłowe znajdują się w usłudze Azure Blob Storage, musisz więc wybrać usługę Azure Blob Storage dla zestawu danych źródłowych. 

    ![Wybieranie pozycji Azure Blob Storage](./media/tutorial-copy-data-portal/select-azure-storage.png)
8. W aplikacji zostanie otwarta nowa **karta** zatytułowana **AzureBlob1**.

    ![Karta AzureBlob1 ](./media/tutorial-copy-data-portal/new-tab-azure-blob1.png)        
9. Na karcie **Ogólne** w oknie **Właściwości** u dołu podaj wartość **SourceBlobDataset** jako **nazwę**.

    ![Nazwa zestawu danych](./media/tutorial-copy-data-portal/dataset-name.png)
10. W oknie Właściwości przejdź do karty **Połączenie**.   

    ![Karta Połączenie](./media/tutorial-copy-data-portal/source-dataset-connection-tab.png)
11. Kliknij przycisk **+ Nowy** obok pola tekstowego **Połączona usługa**. Połączona usługa łączy magazyn danych lub zasoby obliczeniowe z fabryką danych. W takim przypadku tworzysz połączoną usługę Azure Storage w celu połączenia konta usługi Azure Storage z magazynem danych. Połączona usługa ma informacje o połączeniu, których usługa Data Factory używa do nawiązywania połączenia z usługą Blob Storage w środowisku uruchomieniowym. Zestaw danych określa kontener, folder i plik (opcjonalnie) zawierający dane źródłowe. 

    ![Przycisk Nowa połączona usługa](./media/tutorial-copy-data-portal/source-dataset-new-linked-service-button.png)
12. W oknie **Nowa połączona usługa** wykonaj następujące czynności: 

    1. Podaj wartość **AzureStorageLinkedService** w polu **Nazwa**. 
    2. Wybierz swoje konto usługi Azure Storage jako wartość pola **Nazwa konta magazynu**.
    3. Kliknij przycisk **Testuj połączenie**, aby przetestować połączenie z kontem usługi Azure Storage.  
    4. Kliknij przycisk **Zapisz**, aby zapisać połączoną usługę.

        ![Nowa połączona usługa Azure Storage](./media/tutorial-copy-data-portal/new-azure-storage-linked-service.png)
13. Kliknij przycisk **Przeglądaj** dla pola **Ścieżka pliku**.  

    ![Przycisk Przeglądaj do wyszukiwania pliku](./media/tutorial-copy-data-portal/file-browse-button.png)
14. Przejdź do folderu **adftutorial/input**, wybierz plik **emp.txt**, a następnie kliknij przycisk **Zakończ**. Możesz również kliknąć dwukrotnie plik emp.txt. 

    ![Wybieranie pliku wejściowego](./media/tutorial-copy-data-portal/select-input-file.png)
15. Upewnij się, że pozycja **Format pliku** ma ustawioną wartość **Format tekstowy**, a **Ogranicznik kolumny** ma wartość **Przecinek (`,`)**. Jeśli plik źródłowy używa innych ograniczników wiersza i kolumny, możesz kliknąć pozycję **Wykryj format tekstowy** dla pola **Format pliku**. Narzędzie do kopiowania danych automatycznie wykrywa format pliku i ograniczniki. Ciągle możesz zastąpić te wartości. Klikając pozycję **Podgląd danych**, możesz wyświetlić podgląd danych na tej stronie.

    ![Wykrywanie formatu tekstowego](./media/tutorial-copy-data-portal/detect-text-format.png)
17. Przejdź do karty **Schemat** w oknie Właściwości i kliknij pozycję **Importuj schemat**. Zwróć uwagę, że aplikacja wykryła dwie kolumny w pliku źródłowym. W tym miejscu importujesz schemat, dzięki czemu można mapować kolumny z magazynu danych źródłowych na magazyn danych ujścia. Jeśli nie ma potrzeby mapowania kolumn, możesz pominąć ten krok. W tym samouczku zaimportuj schemat.

    ![Wykrywanie schematu źródłowego](./media/tutorial-copy-data-portal/detect-source-schema.png)  
19. Teraz przejdź do **karty z potokiem** lub kliknij potok w **widoku drzewa** po lewej stronie.  

    ![Karta potoku](./media/tutorial-copy-data-portal/pipeline-tab.png)
20. Upewnij się, że w oknie Właściwości dla pola Zestaw danych źródłowych zaznaczono pozycję **SourceBlobDataset**. Klikając pozycję **Podgląd danych**, możesz wyświetlić podgląd danych na tej stronie. 
    
    ![Zestaw danych źródłowych](./media/tutorial-copy-data-portal/source-dataset-selected.png)
21. Przejdź do karty **Ujście**, a następnie kliknij pozycję **Nowy**, aby utworzyć zestaw danych ujścia. 

    ![Menu Nowy zestaw danych ujścia](./media/tutorial-copy-data-portal/new-sink-dataset-button.png)
22. W oknie **Nowy zestaw danych** wybierz pozycję **Azure SQL Database** i kliknij przycisk **Zakończ**. W tym samouczku skopiujemy dane do usługi Azure SQL Database. 

    ![Wybieranie usługi Azure SQL Database](./media/tutorial-copy-data-portal/select-azure-sql-database.png)
23. Na karcie **Ogólne** w oknie Właściwości ustaw nazwę na **OutputSqlDataset**. 
    
    ![Nazwa zestawu danych wyjściowych](./media/tutorial-copy-data-portal/output-dataset-name.png)
24. Przejdź do karty **Połączenie**, a następnie kliknij pozycję **Nowy** dla pozycji **Połączona usługa**. Zestaw danych musi być skojarzony z połączoną usługą. Połączona usługa ma parametry połączenia, których usługa Data Factory używa do nawiązywania połączenia z usługą Azure SQL Database w środowisku uruchomieniowym. Zestaw danych określa kontener, folder i plik (opcjonalnie), do którego dane są kopiowane. 
    
    ![Przycisk Nowa połączona usługa](./media/tutorial-copy-data-portal/new-azure-sql-database-linked-service-button.png)       
25. W oknie **Nowa połączona usługa** wykonaj następujące czynności: 

    1. Wprowadź wartość **AzureSqlDatabaseLinkedService** w polu **Nazwa**. 
    2. W polu **Nazwa serwera** wybierz serwer usługi Azure SQL.
    4. W polu **Nazwa bazy danych** wybierz bazę danych usługi Azure SQL. 
    5. W polu **Nazwa użytkownika** podaj nazwę użytkownika. 
    6. W polu **Hasło** podaj hasło użytkownika. 
    7. Kliknij pozycję **Testuj połączenie** w celu przetestowania połączenia.
    8. Kliknij przycisk **Zapisz**, aby zapisać połączoną usługę. 
    
        ![Nowa połączona usługa Azure SQL Database](./media/tutorial-copy-data-portal/new-azure-sql-linked-service-window.png)

26. Wybierz element **[dbo].[emp]** dla pozycji **Tabela**. 

    ![Wybór tabeli emp](./media/tutorial-copy-data-portal/select-emp-table.png)
27. Przejdź do karty **Schemat** i kliknij polecenie Importuj schemat. 

    ![Importowanie schematu docelowego](./media/tutorial-copy-data-portal/import-destination-schema.png)
28. Wybierz kolumnę **ID**, a następnie kliknij przycisk **Usuń**. Kolumna ID jest kolumną tożsamości w bazie danych SQL, więc działanie kopiowania nie musi wstawiać danych do tej kolumny.

    ![Usuwanie kolumny ID](./media/tutorial-copy-data-portal/delete-id-column.png)
30. Przejdź do karty z **potokiem** i upewnij się, że dla pozycji **Zestaw danych ujścia** wybrano wartość **OutputSqlDataset**.

    ![Karta potoku](./media/tutorial-copy-data-portal/pipeline-tab-2.png)        
31. Przejdź do karty **Mapowanie** w oknie Właściwości u dołu i kliknij polecenie **Importuj schematy**. Zwróć uwagę, że pierwsza i druga kolumna w pliku źródłowym są mapowane na pola **Imię** i **Nazwisko** w bazie danych usługi SQL.

    ![Mapowanie schematów](./media/tutorial-copy-data-portal/map-schemas.png)
33. Zweryfikuj potok, klikając przycisk **Weryfikuj**. Kliknij **strzałkę w prawo**, aby zamknąć okno weryfikacji.

    ![Dane wyjściowe weryfikacji potoku](./media/tutorial-copy-data-portal/pipeline-validation-output.png)   
34. Kliknij przycisk **Kod** znajdujący się w prawym rogu. Zostanie wyświetlony kod JSON skojarzony z potokiem. 

    ![Przycisk Kod](./media/tutorial-copy-data-portal/code-button.png)
35. Wyświetlony kod JSON będzie podobny do następującego fragmentu kodu:  

    ```json
    {
        "name": "CopyPipeline",
        "properties": {
            "activities": [
                {
                    "name": "CopyFromBlobToSql",
                    "type": "Copy",
                    "dependsOn": [],
                    "policy": {
                        "timeout": "7.00:00:00",
                        "retry": 0,
                        "retryIntervalInSeconds": 20
                    },
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "SqlSink",
                            "writeBatchSize": 10000
                        },
                        "enableStaging": false,
                        "parallelCopies": 0,
                        "cloudDataMovementUnits": 0,
                        "translator": {
                            "type": "TabularTranslator",
                            "columnMappings": "Prop_0: FirstName, Prop_1: LastName"
                        }
                    },
                    "inputs": [
                        {
                            "referenceName": "SourceBlobDataset",
                            "type": "DatasetReference",
                            "parameters": {}
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "OutputSqlDataset",
                            "type": "DatasetReference",
                            "parameters": {}
                        }
                    ]
                }
            ]
        }
    }
    ```

## <a name="test-run-the-pipeline"></a>Testowe uruchamianie potoku
Przed opublikowaniem artefaktów (połączone usługi, zestawy danych i potok) w usłudze Data Factory lub własnym repozytorium GIT usługi VSTS możesz testowo uruchomić potok. 

1. Aby testowo uruchomić potok, kliknij przycisk **Uruchomienie testowe** na pasku narzędzi. Na karcie **Dane wyjściowe** u dołu okna wyświetlany jest stan uruchomienia potoku. 

    ![Przycisk Uruchomienie testowe](./media/tutorial-copy-data-portal/test-run-output.png)
2. Upewnij się, że dane z pliku źródłowego są wstawione do docelowej bazy danych usługi SQL. 

    ![Weryfikacja danych wyjściowych usługi SQL](./media/tutorial-copy-data-portal/verify-sql-output.png)
3. W okienku po lewej stronie kliknij pozycję **Opublikuj**. Ta akcja powoduje opublikowanie utworzonych jednostek (połączone usługi, zestawy danych i potok) w usłudze Azure Data Factory.

    ![Przycisk Opublikuj](./media/tutorial-copy-data-portal/publish-button.png)
4. Poczekaj na wyświetlenie komunikatu **Pomyślnie opublikowano**. Aby wyświetlić komunikaty powiadomień, kliknij kartę **Pokaż powiadomienia** na lewym pasku bocznym. Zamknij okno powiadomień, klikając przycisk **X**.

    ![Wyświetlanie powiadomień](./media/tutorial-copy-data-portal/show-notifications.png)

## <a name="configure-code-repository"></a>Konfigurowanie repozytorium kodu
Kod skojarzony z Twoimi artefaktami fabryki danych możesz opublikować w repozytorium kodu platformy Visual Studio Team System (VSTS). W tym kroku utworzysz repozytorium kodu. 

Jeśli nie chcesz pracować z repozytorium kodu platformy VSTS, możesz pominąć ten krok i kontynuować publikowanie w usłudze Data Factory, tak jak w poprzednim kroku. 

1. Kliknij przycisk **Fabryka danych** w lewym rogu lub przycisk strzałki w dół znajdujący się obok i kliknij pozycję **Konfiguruj repozytorium kodu**. 

    ![Przycisk Kod](./media/tutorial-copy-data-portal/configure-code-repository-button.png)
2. Na stronie **Ustawienia repozytorium** wykonaj następujące czynności: 
    1. W polu **Typ repozytorium** wybierz pozycję **Repozytorium Git usługi Visual Studio Team Services**.
    2. W polu **Konto usługi Visual Studio Team Services** wybierz swoje konto usługi VSTS.
    3. W polu **Nazwa projektu** wybierz projekt na swoim koncie usługi VSTS.
    4. Wprowadź wartość **Tutorial2** jako **nazwę repozytorium Git**, które ma być skojarzone z fabryką danych. 
    5. Upewnij się, że zaznaczono opcję **Importuj istniejące zasoby fabryki danych do repozytorium**. 
    6. Kliknij polecenie **Zapisz**, aby zapisać ustawienia. 

        ![Ustawienia repozytoriów](./media/tutorial-copy-data-portal/repository-settings.png)
3. Upewnij się, że wybrano pozycję **VSTS GIT** dla repozytorium.

    ![Wybrana pozycja VSTS GIT](./media/tutorial-copy-data-portal/vsts-git-selected.png)
4. W osobnej karcie w przeglądarce internetowej przejdź do repozytorium **Tutorial2**. Zobaczysz dwie gałęzie: **master** i **adf_publish**.

    ![Gałęzie master i adf_publish](./media/tutorial-copy-data-portal/initial-branches-vsts-git.png)
5. Upewnij się, że **pliki JSON** dla jednostek usługi Data Factory znajdują się w gałęzi **master**.

    ![Pliki w gałęzi master](./media/tutorial-copy-data-portal/master-branch-files.png)
6. Upewnij się, że **pliki JSON** nie znajdują się jeszcze w gałęzi **adf_publish**. 

    ![Pliki w gałęzi adf_publish](./media/tutorial-copy-data-portal/adf-publish-files.png)
7. Dodaj **opis** do **potoku** i kliknij przycisk **Zapisz** znajdujący się na pasku narzędzi. 

    ![Dodawanie opisu potoku](./media/tutorial-copy-data-portal/pipeline-description.png)
8. Teraz powinna zostać wyświetlona **gałąź** z Twoją nazwą użytkownika w repozytorium **Tutorial2**. Wprowadzone zmiany mają zastosowanie do Twojej gałęzi, a nie gałęzi master. Publikować można jedynie jednostki z gałęzi master.

    ![Twoja gałąź](./media/tutorial-copy-data-portal/your-branch.png)
9. Przenieś kursor nad przycisk **Synchronizuj** (jeszcze go nie klikaj), zaznacz opcję **Zatwierdź zmiany**, a następnie kliknij przycisk **Synchronizuj**, aby zsynchronizować zmiany z gałęzią **master**. 

    ![Zatwierdzanie i synchronizowanie zmian](./media/tutorial-copy-data-portal/commit-and-sync.png)
9. W oknie Synchronizacja zmian wykonaj następujące akcje: 

    1. Upewnij się, że element **CopyPipeline** jest wyświetlany na zaktualizowanej liście potoków.
    2. Upewnij się, że zaznaczono opcję **Opublikuj zmiany po synchronizacji**. Jeśli ta opcja nie zostanie zaznaczona, zmiany wprowadzone w Twojej gałęzi zostaną zsynchronizowane tylko z gałęzią master, ale nie zostaną opublikowane w usłudze Data Factory. Możesz je opublikować później, klikając przycisk **Opublikuj**. Jeśli ta opcja zostanie zaznaczona, zmiany są najpierw synchronizowane z gałęzią master, a następnie publikowane w usłudze Data Factory.
    3. Kliknij przycisk **Synchronizuj**. 

    ![Okno Synchronizowanie zmian](./media/tutorial-copy-data-portal/sync-your-changes.png)
10. Teraz pliki będą widoczne w gałęzi **adf_publish** repozytorium **Tutorial2**. W tej gałęzi możesz również znaleźć szablon usługi Azure Resource Manager dla Twojego rozwiązania fabryki danych.  

    ![Pliki w gałęzi adf_publish](./media/tutorial-copy-data-portal/adf-publish-files-after-publish.png)


## <a name="trigger-the-pipeline-manually"></a>Ręczne wyzwalanie potoku
W tym kroku ręcznie wyzwolisz potok, który został opublikowany w poprzednim kroku. 

1. Kliknij pozycję **Wyzwól** na pasku narzędzi, a następnie kliknij pozycję **Wyzwól teraz**. 

    ![Menu Wyzwól teraz](./media/tutorial-copy-data-portal/trigger-now-menu.png)
2. Przejdź do karty **Monitorowanie** po lewej stronie. Widoczne jest uruchomienie potoku, które zostało wyzwolone za pomocą wyzwalacza ręcznego. Linków w kolumnie Akcje możesz użyć, aby wyświetlić szczegóły działań i ponownie uruchomić potok.

    ![Monitorowanie uruchomienia potoku](./media/tutorial-copy-data-portal/monitor-pipeline.png)
3. Aby wyświetlić uruchomienia działań skojarzone z uruchomieniem potoku, kliknij link **Wyświetl uruchomienia działań** w kolumnie **Akcje**. W tym przykładzie istnieje tylko jedno działanie, dlatego na liście widnieje tylko jedna pozycja. Aby uzyskać szczegółowe informacje na temat operacji kopiowania, kliknij link **Szczegóły** (ikona okularów) w kolumnie **Działania**. Aby powrócić do widoku **Uruchomienia potoków**, kliknij pozycję **Potoki** znajdującą się u góry. Aby odświeżyć widok, kliknij przycisk **Odśwież**.

    ![Wyświetlanie uruchomień działania](./media/tutorial-copy-data-portal/view-activity-runs.png)
4. Sprawdź, czy dodano jeszcze dwa wiersze do tabeli **emp** w usłudze Azure SQL Database. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>Wyzwalanie potoku zgodnie z harmonogramem
W tym kroku utworzysz wyzwalacz harmonogramu potoku. Wyzwalacz uruchamia potok zgodnie z określonym harmonogramem (co godzinę, codziennie itp.). W tym przykładzie ustawisz wyzwalacz, aby uruchamiał się co minutę, aż do określonej daty/godziny zakończenia. 

1. Przejdź do karty **Edycja** po lewej stronie. 

    ![Karta Edycja](./media/tutorial-copy-data-portal/edit-tab.png)
2. Kliknij przycisk **Wyzwól** i wybierz pozycję **Nowy/Edytuj**. Jeśli potok nie jest aktywny, aktywuj go. 

    ![Menu Nowy/Edytuj wyzwalacz](./media/tutorial-copy-data-portal/trigger-new-edit-menu.png)
3. W oknie **Dodawanie wyzwalaczy** kliknij przycisk **Wybierz wyzwalacz...**, a następnie kliknij przycisk **+ Nowy**. 

    ![Dodawanie wyzwalaczy — nowy wyzwalacz](./media/tutorial-copy-data-portal/add-trigger-new-button.png)
4. W oknie **Nowy wyzwalacz** wykonaj następujące czynności: 

    1. Ustaw **nazwę** na wartość **RunEveryMinute**.
    2. Dla pozycji **Koniec** wybierz opcję **W dniu**. 
    3. Kliknij listę rozwijaną przy pozycji **Dzień zakończenia**.
    4. Wybierz **bieżący dzień**. Domyślnie dzień zakończenia jest ustawiony na następny dzień. 
    5. Zaktualizuj część **Minuty** tak, aby była o kilka minut późniejsza od bieżącej godziny. Wyzwalacz zostanie aktywowany tylko w przypadku, gdy opublikujesz zmiany. W związku z tym jeśli ustawisz tylko kilkuminutowy odstęp od bieżącej godziny i nie opublikujesz zmian do tego czasu, wyzwalacz się nie uruchomi.  
    6. Kliknij przycisk **Zastosuj**. 

        ![Ustawianie właściwości wyzwalacza](./media/tutorial-copy-data-portal/set-trigger-properties.png)
    7. Zaznacz opcję **Aktywowany**. Przy użyciu tego pola wyboru możesz dezaktywować wyzwalacz i aktywować go później.
    8. Kliknij przycisk **Dalej**.

        ![Aktywowany wyzwalacz — dalej](./media/tutorial-copy-data-portal/trigger-activiated-next.png)

    > [!IMPORTANT]
    > Z każdym uruchomieniem potoku wiąże się koszt. W związku z tym odpowiednio ustaw datę zakończenia. 
6. Na stronie **Parametry uruchamiania wyzwalacza** przejrzyj ostrzeżenie, a następnie kliknij przycisk **Zakończ**. Potok w tym przykładzie nie przyjmuje żadnych parametrów. 

    ![Parametry potoku](./media/tutorial-copy-data-portal/trigger-pipeline-parameters.png)
7. Kliknij przycisk **Opublikuj**, aby opublikować zmiany w repozytorium. Wyzwalacz nie zostanie faktycznie aktywowany do czasu pomyślnego opublikowania zmian. 

    ![Publikowanie wyzwalacza](./media/tutorial-copy-data-portal/publish-trigger.png) 
8. Przejdź do karty **Monitorowanie** po lewej stronie, aby zobaczyć wyzwolone uruchomienia potoku. 

    ![Wyzwolone uruchomienia potoku](./media/tutorial-copy-data-portal/triggered-pipeline-runs.png)    
9. Aby przełączyć się z widoku uruchomień potoku do widoku uruchomień wyzwalacza, kliknij przycisk Uruchomienia potoku i wybierz pozycję Uruchomienia wyzwalacza.
    
    ![Menu Uruchomienia wyzwalacza](./media/tutorial-copy-data-portal/trigger-runs-menu.png)
10. Uruchomienia wyzwalacza znajdują się na liście. 

    ![Lista uruchomień wyzwalacza](./media/tutorial-copy-data-portal/trigger-runs-list.png)
11. Sprawdź, czy do tabeli **emp** są wstawiane dwa wiersze na minutę (dla każdego uruchomienia potoku), aż do określonego czasu zakończenia. 

## <a name="next-steps"></a>Następne kroki
Potok w tym przykładzie kopiuje dane z jednej lokalizacji do innej lokalizacji w usłudze Azure Blob Storage. W tym samouczku omówiono: 

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Tworzenie potoku z działaniem kopiowania.
> * Testowe uruchamianie potoku
> * Ręczne wyzwalanie potoku
> * Wyzwalanie potoku zgodnie z harmonogramem
> * Monitorowanie uruchomień potoku i działań.


Przejdź do następującego samouczka, aby dowiedzieć się więcej o kopiowaniu danych lokalnych do chmury: 

> [!div class="nextstepaction"]
>[Kopiowanie danych ze środowiska lokalnego do chmury](tutorial-hybrid-copy-data-tool.md)
