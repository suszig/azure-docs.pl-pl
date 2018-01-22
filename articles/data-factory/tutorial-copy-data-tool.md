---
title: "Kopiowanie danych przy użyciu narzędzia do kopiowania danych platformy Azure | Microsoft Docs"
description: "Utwórz fabrykę danych platformy Azure, a następnie za pomocą narzędzia do kopiowania danych skopiuj dane z usługi Azure Blob Storage do usługi Azure SQL Database."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: a8c7035ebf462bb168147e9d8eb60742333ce8b8
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="copy-data-from-azure-blob-to-azure-sql-database-using-copy-data-tool"></a>Kopiowanie danych z obiektu blob platformy Azure do bazy danych Azure SQL Database przy użyciu narzędzia do kopiowania danych
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Wersja 2 — wersja zapoznawcza](tutorial-copy-data-tool.md)

W tym samouczku utworzysz fabrykę danych za pomocą witryny Azure Portal. Następnie za pomocą narzędzia do kopiowania danych utworzysz potok, który kopiuje dane z usługi Azure Blob Storage do usługi Azure SQL Database. 

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

* **Subskrypcja platformy Azure**. Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.
* **Konto usługi Azure Storage**. Magazyn obiektów blob jest używany jako **źródłowy** magazyn danych. Jeśli nie masz konta usługi Azure Storage, utwórz je, wykonując czynności przedstawione w artykule [Tworzenie konta magazynu](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* **Usługa Azure SQL Database**. Baza danych jest używana jako magazyn danych **ujścia**. Jeśli nie masz bazy danych Azure SQL Database, utwórz ją, wykonując czynności przedstawione w artykule [Create an Azure SQL database (Tworzenie bazy danych Azure SQL Database)](../sql-database/sql-database-get-started-portal.md).

### <a name="create-a-blob-and-a-sql-table"></a>Tworzenie obiektu blob i tabeli SQL

Teraz przygotuj swój obiekt Blob platformy Azure i bazę danych Azure SQL Database w ramach tego samouczka, wykonując następujące kroki:

#### <a name="create-a-source-blob"></a>Tworzenie źródłowego obiektu Blob

1. Uruchom program Notatnik. Skopiuj poniższy tekst i zapisz go na dysku jako plik **inputEmp.txt**.

    ```
    John|Doe
    Jane|Doe
    ```

2. Użyj narzędzi, takich jak [Eksplorator usługi Azure Storage](http://storageexplorer.com/), aby utworzyć kontener **adfv2tutorial** i przekazać plik **inputEmp.txt** do kontenera.

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

2. Zezwól usługom platformy Azure na dostęp do serwera SQL. Upewnij się, że ustawienie **Zezwalaj na dostęp do usług Azure** jest włączone dla serwera SQL platformy Azure, aby usługa Data Factory mogła zapisywać dane na serwerze Azure SQL. W celu sprawdzenia i włączenia tego ustawienia wykonaj następujące kroki:

    1. Kliknij centrum **Więcej usług** po lewej stronie, a następnie kliknij przycisk **Serwery SQL**.
    2. Wybierz swój serwer, a następnie kliknij przycisk **Zapora** w obszarze **USTAWIENIA**.
    3. Na stronie **Ustawienia zapory** kliknij pozycję **WŁĄCZ** dla ustawienia **Zezwalaj na dostęp do usług platformy Azure**.

## <a name="create-a-data-factory"></a>Tworzenie fabryki danych

1. Kliknij przycisk **Nowy** w lewym menu, kliknij pozycję **Dane + analiza**, a następnie kliknij pozycję **Data Factory**. 
   
   ![Nowy-> Fabryka danych](./media/tutorial-copy-data-tool/new-azure-data-factory-menu.png)
2. Na stronie **Nowa fabryka danych** wprowadź wartość **ADFTutorialDataFactory** w polu **Nazwa**. 
      
     ![Strona Nowa fabryka danych](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
 
   Nazwa fabryki danych platformy Azure musi być **globalnie unikatowa**. Jeśli dla pola nazwy wystąpi poniższy błąd, zmień nazwę fabryki danych (np. twojanazwaADFTutorialDataFactory). Artykuł [Data Factory — Naming Rules (Usługa Data Factory — reguły nazewnictwa)](naming-rules.md) zawiera reguły nazewnictwa artefaktów usługi Data Factory.
  
     ![Strona Nowa fabryka danych](./media/tutorial-copy-data-tool/name-not-available-error.png)
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

    ![kafelek Wdrażanie fabryki danych](media/tutorial-copy-data-tool/deploying-data-factory.png)
9. Po zakończeniu tworzenia zostanie wyświetlona strona **Fabryka danych**, jak pokazano na poniższej ilustracji.
   
   ![Strona główna fabryki danych](./media/tutorial-copy-data-tool/data-factory-home-page.png)
10. Kliknij kafelek **Tworzenie i monitorowanie**, aby w osobnej karcie uruchomić interfejs użytkownika usługi Azure Data Factory. 

## <a name="use-copy-data-tool-to-create-a-pipeline"></a>Tworzenie potoku za pomocą narzędzia do kopiowania danych

1. Na stronie wprowadzenia kliknij kafelek **Kopiuj dane**, aby uruchomić narzędzie do kopiowania danych. 

   ![Kafelek narzędzia do kopiowania danych](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
2. Na stronie **Właściwości** narzędzia do kopiowania danych wprowadź jako **nazwę zadania** wartość **CopyFromBlobToSqlPipeline**, a następnie kliknij przycisk **Dalej**. Interfejs użytkownika usługi Data Factory tworzy potok o nazwie określonej dla nazwy zadania. 

    ![Narzędzie do kopiowania danych — strona Właściwości](./media/tutorial-copy-data-tool/copy-data-tool-properties-page.png)
3. Na stronie **Magazyn danych źródłowych** wybierz pozycję **Azure Blob Storage**, a następnie kliknij przycisk **Dalej**. Źródło danych znajduje się w usłudze Azure Blob Storage. 

    ![Strona Źródłowy magazyn danych](./media/tutorial-copy-data-tool/source-data-store-page.png)
4. Na stronie **Określanie konta usługi Azure Blob Storage** wykonaj następujące czynności: 
    1. Jako **nazwę połączenia** wprowadź wartość **AzureStorageLinkedService**.
    2. Wybierz **nazwę konta magazynu** z listy rozwijanej.
    3. Kliknij przycisk **Dalej**. 

        ![Określanie konta magazynu obiektów blob](./media/tutorial-copy-data-tool/specify-blob-storage-account.png)

        Połączona usługa łączy magazyn danych lub zasoby obliczeniowe z fabryką danych. W takim przypadku tworzysz połączoną usługę Azure Storage w celu połączenia konta usługi Azure Storage z magazynem danych. Połączona usługa ma informacje o połączeniu, których usługa Data Factory używa do nawiązywania połączenia z usługą Blob Storage w środowisku uruchomieniowym. Zestaw danych określa kontener, folder i plik (opcjonalnie) zawierający dane źródłowe. 
5. Na stronie **Wybieranie pliku lub folderu wejściowego** wykonaj następujące czynności:
    
    1. Przejdź do folderu **adfv2tutorial/input**.
    2. Wybierz plik **inputEmp.txt**
    3. Kliknij pozycję **Wybierz**. Możesz również kliknąć dwukrotnie plik **inputEmp.txt**. 
    4. Kliknij przycisk **Dalej**. 

    ![Wybieranie pliku lub folderu wejściowego](./media/tutorial-copy-data-tool/choose-input-file-folder.png)
6. Na stronie **Ustawienia formatu pliku** zwróć uwagę, że narzędzie automatycznie wykrywa ograniczniki kolumn i wierszy, a następnie kliknij przycisk **Dalej**. Na tej stronie możesz także wyświetlić podgląd danych i wyświetlić schemat danych wejściowych. 

    ![Strona Ustawienia formatu pliku](./media/tutorial-copy-data-tool/file-format-settings-page.png)
7. Na stronie **Docelowy magazyn danych** wybierz pozycję **Azure SQL Database** i kliknij przycisk **Dalej**. 

    ![Strona Docelowy magazyn danych](./media/tutorial-copy-data-tool/destination-data-storage-page.png)
8. Na stronie **Określanie bazy danych SQL Azure** wykonaj następujące czynności: 

    1. Jako **nazwę połączenia** wprowadź wartość **AzureSqlDatabaseLinkedService**. 
    2. W polu **Nazwa serwera** wybierz swój serwer usługi Azure SQL.
    3. W polu **Nazwa bazy danych** wybierz swoją bazę danych usługi Azure SQL.
    4. W polu **Nazwa użytkownika** podaj nazwę użytkownika.
    5. W polu **Hasło** podaj hasło użytkownika.
    6. Kliknij przycisk **Dalej**. 

        ![Określanie bazy danych Azure SQL Database](./media/tutorial-copy-data-tool/specify-azure-sql-database.png)

        Zestaw danych musi być skojarzony z połączoną usługą. Połączona usługa ma parametry połączenia, których usługa Data Factory używa do nawiązywania połączenia z usługą Azure SQL Database w środowisku uruchomieniowym. Zestaw danych określa kontener, folder i plik (opcjonalnie), do którego dane są kopiowane.
9.  Na stronie **Mapowanie tabeli** wybierz pozycję **[dbo].[emp]** i kliknij przycisk **Dalej**. 

    ![Strona Mapowanie tabeli](./media/tutorial-copy-data-tool/table-mapping-page.png)
10. Na stronie **Mapowanie schematu** zwróć uwagę, że pierwsza i druga kolumna w pliku wejściowym są mapowane na kolumny **FirstName** i **LastName** tabeli **emp**. 

    ![Strona Mapowanie schematu](./media/tutorial-copy-data-tool/schema-mapping-page.png)
11. Na stronie **Ustawienia** kliknij przycisk **Dalej**. 

    ![Strona Ustawienia](./media/tutorial-copy-data-tool/settings-page.png)
12. Sprawdź ustawienia w bloku **Podsumowanie** i kliknij przycisk **Dalej**.

    ![Strona podsumowania](./media/tutorial-copy-data-tool/summary-page.png)
13. Na **stronie wdrożenia** kliknij pozycję **Monitorowanie**, aby monitorować potok (zadanie).

    ![Strona Wdrażanie](./media/tutorial-copy-data-tool/deployment-page.png)
14. Zwróć uwagę, że karta **Monitor** po lewej stronie jest automatycznie wybrana. W kolumnie **Działania** znajdują się linki służące do wyświetlania szczegółów działań i ponownego uruchamiania potoku. Kliknij pozycję **Odśwież**, aby odświeżyć listę. 

    ![Monitorowanie uruchomień potoku](./media/tutorial-copy-data-tool/monitor-pipeline-runs.png)
15. Aby wyświetlić uruchomienia działań skojarzone z uruchomieniem potoku, kliknij link **Wyświetl uruchomienia działań** w kolumnie **Akcje**. W potoku jest tylko jedno działanie (działanie kopiowania), dlatego na liście jest wyświetlana tylko jedna pozycja. Aby uzyskać szczegółowe informacje na temat operacji kopiowania, kliknij link **Szczegóły** (ikona okularów) w kolumnie **Działania**. Aby powrócić do widoku Uruchomienia potoku, kliknij link **Potoki** u góry strony. Aby odświeżyć widok, kliknij przycisk **Odśwież**. 

    ![Monitorowanie uruchomień działania](./media/tutorial-copy-data-tool/monitor-activity-runs.png)
16. Kliknij kartę **Edycja** po lewej stronie, aby przełączyć do trybu edytora. Za pomocą edytora można zaktualizować usługi połączone, zestawy danych i potoki utworzone przez narzędzie. Kliknij pozycję **Kod**, aby wyświetlić kod JSON skojarzony z jednostką otwartą w edytorze. Aby uzyskać szczegółowe informacje dotyczące edytowania tych jednostek w interfejsie użytkownika usługi Data Factory, zobacz [wersję witryny Azure Portal używaną w tym samouczku](tutorial-copy-data-portal.md).

    ![Karta edytora](./media/tutorial-copy-data-tool/edit-tab.png)
17. Sprawdź, czy dane są wstawiane do tabeli **emp** w bazie danych Azure SQL Database. 

    ![Weryfikacja danych wyjściowych usługi SQL](./media/tutorial-copy-data-tool/verify-sql-output.png)

## <a name="next-steps"></a>Następne kroki
Potok w tym przykładzie kopiuje dane z usługi Azure Blob Storage do usługi Azure SQL Database. W tym samouczku omówiono: 

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Tworzenie potoku za pomocą narzędzia do kopiowania danych
> * Monitorowanie uruchomień potoku i działań.

Przejdź do następującego samouczka, aby dowiedzieć się więcej o kopiowaniu danych lokalnych do chmury: 

> [!div class="nextstepaction"]
>[Kopiowanie danych ze środowiska lokalnego do chmury](tutorial-hybrid-copy-data-tool.md)