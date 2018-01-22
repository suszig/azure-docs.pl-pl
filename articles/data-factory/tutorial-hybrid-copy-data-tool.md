---
title: "Kopiowanie danych lokalnych za pomocą narzędzia do kopiowania danych platformy Azure | Microsoft Docs"
description: "Utwórz fabrykę danych platformy Azure, a następnie za pomocą narzędzia do kopiowania danych skopiuj dane z lokalnej bazy danych programu SQL Server do usługi Azure Blob Storage."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/04/2018
ms.author: jingwang
ms.openlocfilehash: aba8b13d47b2b9236a0d5cc868e53780e894c406
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage-by-using-copy-data-tool"></a>Kopiowanie danych z lokalnej bazy danych programu SQL Server do usługi Azure Blob Storage za pomocą narzędzia do kopiowania danych
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Wersja 2 — wersja zapoznawcza](tutorial-hybrid-copy-data-tool.md)

W tym samouczku utworzysz fabrykę danych za pomocą witryny Azure Portal. Następnie za pomocą narzędzia do kopiowania danych utworzysz potok, który kopiuje dane z lokalnej bazy danych programu SQL Server do usługi Azure Blob Storage.

> [!NOTE]
> - Jeśli jesteś nowym użytkownikiem usługi Azure Data Factory, zobacz [Wprowadzenie do usługi Azure Data Factory](introduction.md).
>
> - Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz artykuł z [wprowadzeniem do usługi Data Factory w wersji 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Ten samouczek obejmuje następujące procedury:

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Tworzenie potoku za pomocą narzędzia do kopiowania danych
> * Monitorowanie uruchomień potoku i działań.

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

5. Aby utworzyć tabelę **emp** i wstawić do niej przykładowe dane, uruchom następujący skrypt zapytania w bazie danych: w widoku drzewa kliknij prawym przyciskiem myszy utworzoną bazę danych, a następnie wybierz pozycję **Nowe zapytanie**.

    ```sql
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

### <a name="azure-storage-account"></a>Konto usługi Azure Storage
W tym samouczku używasz konta usługi Azure Storage ogólnego przeznaczenia (a dokładniej usługi Azure Blob Storage) jako docelowego magazynu danych (ujścia). Jeśli nie masz konta usługi Azure Storage ogólnego przeznaczenia, zobacz [Tworzenie konta magazynu](../storage/common/storage-create-storage-account.md#create-a-storage-account), aby dowiedzieć się, jak je utworzyć. Potok usługi Data Factory tworzony w tym samouczku kopiuje dane z lokalnej bazy danych programu SQL Server (źródła) do tego konta usługi Azure Blob Storage (ujścia). 

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

1. Kliknij przycisk **Nowy** w lewym menu, kliknij pozycję **Dane + analiza**, a następnie kliknij pozycję **Data Factory**. 
   
   ![Nowy-> Fabryka danych](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory-menu.png)
2. Na stronie **Nowa fabryka danych** wprowadź wartość **ADFTutorialDataFactory** w polu **Nazwa**. 
      
     ![Strona Nowa fabryka danych](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory.png)
 
   Nazwa fabryki danych platformy Azure musi być **globalnie unikatowa**. Jeśli dla pola nazwy wystąpi poniższy błąd, zmień nazwę fabryki danych (np. twojanazwaADFTutorialDataFactory). Artykuł [Data Factory — Naming Rules (Usługa Data Factory — reguły nazewnictwa)](naming-rules.md) zawiera reguły nazewnictwa artefaktów usługi Data Factory.
  
     ![Strona Nowa fabryka danych](./media/tutorial-hybrid-copy-data-tool/name-not-available-error.png)
3. Wybierz **subskrypcję** Azure, w której chcesz utworzyć fabrykę danych. 
4. Dla opcji **Grupa zasobów** wykonaj jedną z następujących czynności:
     
      - Wybierz pozycję **Użyj istniejącej**, a następnie wybierz istniejącą grupę zasobów z listy rozwijanej. 
      - Wybierz pozycję **Utwórz nową**, a następnie wprowadź nazwę grupy zasobów.   
         
      Informacje na temat grup zasobów znajdują się w artykule [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md) (Używanie grup zasobów do zarządzania zasobami platformy Azure).  
4. Wybierz wartość **V2 (wersja zapoznawcza)** dla **wersji**.
5. Na liście **lokalizacja** wybierz lokalizację fabryki danych. Na liście rozwijanej są wyświetlane tylko obsługiwane lokalizacje. Magazyny danych (Azure Storage, Azure SQL Database itp.) i jednostki obliczeniowe (HDInsight itp.) używane przez fabrykę danych mogą mieścić się w innych lokalizacjach/regionach.
6. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**.     
7. Kliknij przycisk **Utwórz**.
8. Na pulpicie nawigacyjnym jest widoczny następujący kafelek ze stanem: **Wdrażanie fabryki danych**. 

    ![kafelek Wdrażanie fabryki danych](media/tutorial-hybrid-copy-data-tool/deploying-data-factory.png)
9. Po zakończeniu tworzenia zostanie wyświetlona strona **Fabryka danych**, jak pokazano na poniższej ilustracji.
   
   ![Strona główna fabryki danych](./media/tutorial-hybrid-copy-data-tool/data-factory-home-page.png)
10. Kliknij kafelek **Tworzenie i monitorowanie**, aby w osobnej karcie uruchomić interfejs użytkownika usługi Azure Data Factory. 

## <a name="use-copy-data-tool-to-create-a-pipeline"></a>Tworzenie potoku za pomocą narzędzia do kopiowania danych

1. Na stronie wprowadzenia kliknij kafelek **Kopiuj dane**, aby uruchomić narzędzie do kopiowania danych. 

   ![Kafelek narzędzia do kopiowania danych](./media/tutorial-hybrid-copy-data-tool/copy-data-tool-tile.png)
2. Na stronie **Właściwości** narzędzia do kopiowania danych wprowadź wartość **CopyFromOnPremSqlToAzureBlobPipeline** w polu **Nazwa zadania**, a następnie kliknij przycisk **Dalej**. Narzędzie do kopiowania danych tworzy potok o nazwie określonej w tym polu. 
    
   ![Strona właściwości](./media/tutorial-hybrid-copy-data-tool/properties-page.png)
3. Na stronie **Źródłowy magazyn danych** wybierz pozycję **SQL Server** i kliknij przycisk **Dalej**. Konieczne może być przewinięcie w dół w celu znalezienia pozycji **SQL Server** na liście. 

   ![Strona Źródłowy magazyn danych](./media/tutorial-hybrid-copy-data-tool/select-source-data-store.png)
4. Wprowadź wartość **SqlServerLinkedService** w polu **Nazwa połączenia** i kliknij link **Utwórz środowisko Integration Runtime**. Należy utworzyć środowisko Integration Runtime (Self-hosted), pobrać je na komputer i zarejestrować w usłudze Data Factory. Środowisko Integration Runtime (Self-hosted) kopiuje dane między środowiskiem lokalnym a chmurą platformy Azure.

   ![Link Utwórz środowisko Integration Runtime](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-link.png)
5. W oknie dialogowym **Tworzenie środowiska Integration Runtime** wprowadź wartość **TutorialIntegration Runtime** w polu **Nazwa**, a następnie kliknij przycisk **Utwórz**. 

   ![Okno dialogowe Tworzenie środowiska Integration Runtime](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-dialog.png)
6. Kliknij pozycję **Uruchom instalację ekspresową na tym komputerze**. Ta akcja instaluje na komputerze środowisko Integration Runtime i rejestruje je w usłudze Data Factory. Ewentualnie można użyć opcji instalacji ręcznej w celu pobrania pliku instalacyjnego, uruchomienia go i zarejestrowania środowiska Integration Runtime za pomocą klucza. 

    ![Link Utwórz środowisko Integration Runtime](./media/tutorial-hybrid-copy-data-tool/launch-express-setup-link.png)
7. Uruchom pobraną aplikację. W oknie zostanie wyświetlony **stan** instalacji ekspresowej. 

    ![Stan instalacji ekspresowej](./media/tutorial-hybrid-copy-data-tool/express-setup-status.png)
8. Upewnij się, że w polu **Integration Runtime** wybrano pozycję **TutorialIntegrationRuntime**.

    ![Wybrane środowisko Integration Runtime](./media/tutorial-hybrid-copy-data-tool/integration-runtime-selected.png)
9. Na stronie **Określanie lokalnej bazy danych programu SQL Server** wykonaj następujące czynności: 

    1. Wprowadź wartość **OnPremSqlLinkedService** w polu **Nazwa połączenia**.
    2. Wprowadź nazwę lokalnego programu SQL Server w polu **Nazwa serwera**.
    3. Wprowadź nazwę lokalnej bazy danych w polu **Nazwa bazy danych**. 
    4. Wybierz odpowiedni typ uwierzytelniania w polu **Typ uwierzytelniania**.
    5. Wprowadź nazwę użytkownika z dostępem do lokalnego programu SQL Server w polu **Nazwa użytkownika**.
    6. Wprowadź **hasło** dla użytkownika. 
10. Na stronie **Wybieranie tabel, z których mają być kopiowane dane, lub używanie zapytania niestandardowego** wybierz z listy tabelę **[dbo].[emp]**, a następnie kliknij przycisk **Dalej**. 

    ![Wybieranie tabeli emp](./media/tutorial-hybrid-copy-data-tool/select-emp-table.png)
11. Na stronie **Docelowy magazyn danych** wybierz pozycję **Azure Blob Storage**, a następnie kliknij przycisk **Dalej**.

    ![Wybieranie pozycji Azure Blob Storage](./media/tutorial-hybrid-copy-data-tool/select-destination-data-store.png)
12. Na stronie **Określanie konta usługi Azure Blob Storage** wykonaj następujące czynności: 

    1. Wprowadź wartość **AzureStorageLinkedService** w polu **Nazwa połączenia**.
    2. W polu **Nazwa konta magazynu** wybierz z listy rozwijanej konto usługi Azure Storage. 
    3. Kliknij przycisk **Dalej**.

        ![Wybieranie pozycji Azure Blob Storage](./media/tutorial-hybrid-copy-data-tool/specify-azure-blob-storage-account.png)
13. Na stronie **Wybieranie pliku lub folderu wyjściowego** wykonaj następujące czynności: 
    
    1. Wprowadź wartość **adftutorial/incrementalcopy** w polu **Ścieżka folderu**. Jako jedno z wymagań wstępnych został utworzony kontener **adftutorial**. Jeśli folder wyjściowy nie istnieje, usługa Data Factory automatycznie go utworzy. Można również za pomocą przycisku **Przeglądaj** przejść do magazynu obiektów blob i jego kontenerów/folderów. Zauważ, że domyślnie jako nazwa pliku wyjściowego jest ustawiona wartość **dbo.emp**.
        
        ![Wybieranie pliku lub folderu wyjściowego](./media/tutorial-hybrid-copy-data-tool/choose-output-file-folder.png)
14. Na stronie **Ustawienia formatu pliku** kliknij przycisk **Dalej**. 

    ![Strona Ustawienia formatu pliku](./media/tutorial-hybrid-copy-data-tool/file-format-settings-page.png)
15. Na stronie **Ustawienia** kliknij przycisk **Dalej**. 

    ![Strona Ustawienia](./media/tutorial-hybrid-copy-data-tool/settings-page.png)
16. Na stronie **Podsumowanie** sprawdź wartości wszystkich ustawień i kliknij przycisk **Dalej**. 

    ![Strona podsumowania](./media/tutorial-hybrid-copy-data-tool/summary-page.png)
17. Na stronie **Wdrażanie** kliknij pozycję **Monitorowanie**, aby monitorować utworzony potok lub zadanie.

    ![Strona Wdrażanie](./media/tutorial-hybrid-copy-data-tool/deployment-page.png)
18. Na karcie **Monitorowanie** można wyświetlić stan utworzonego potoku. Linki w kolumnie **Działania** umożliwiają wyświetlanie uruchomień działań skojarzonych z uruchomieniem potoku oraz ponowne uruchamianie potoku. 

    ![Uruchomienia potoków](./media/tutorial-hybrid-copy-data-tool/monitor-pipeline-runs.png)
19. Kliknij link **Wyświetl uruchomienia działania** w kolumnie **Działania**, aby wyświetlić uruchomienia działań skojarzone z uruchomieniem potoku. Aby wyświetlić szczegółowe informacje na temat operacji kopiowania, kliknij link **Szczegóły** (ikona okularów) w kolumnie **Działania**. Aby wrócić do widoku uruchomień potoków, kliknij pozycję **Potoki** u góry.

    ![Uruchomienia działania](./media/tutorial-hybrid-copy-data-tool/monitor-activity-runs.png)
20. Upewnij się, że w folderze **fromonprem** kontenera **adftutorial** znajduje się plik wyjściowy.   
 
    ![Wyjściowy obiekt blob](./media/tutorial-hybrid-copy-data-tool/output-blob.png)
21. Kliknij kartę **Edycja** po lewej stronie, aby przełączyć do trybu edytora. Za pomocą edytora można zaktualizować usługi połączone, zestawy danych i potoki utworzone przez narzędzie. Kliknij pozycję **Kod**, aby wyświetlić kod JSON skojarzony z jednostką otwartą w edytorze. Aby uzyskać szczegółowe informacje dotyczące edytowania tych jednostek w interfejsie użytkownika usługi Data Factory, zobacz [wersję witryny Azure Portal używaną w tym samouczku](tutorial-copy-data-portal.md).

    ![Karta Edycja](./media/tutorial-hybrid-copy-data-tool/edit-tab.png)


## <a name="next-steps"></a>Następne kroki
Potok w tym przykładzie kopiuje dane z lokalnej bazy danych programu SQL Server do usługi Azure Blob Storage. W tym samouczku omówiono: 

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Tworzenie potoku za pomocą narzędzia do kopiowania danych
> * Monitorowanie uruchomień potoku i działań.

Lista magazynów danych obsługiwanych przez usługę Data Factory znajduje się w artykule dotyczącym [obsługiwanych magazynów danych](copy-activity-overview.md#supported-data-stores-and-formats).

Przejdź do poniższego samouczka, aby dowiedzieć się o zbiorczym kopiowaniu danych z lokalizacji źródłowej do docelowej:

> [!div class="nextstepaction"]
>[Zbiorcze kopiowanie danych](tutorial-bulk-copy-portal.md)