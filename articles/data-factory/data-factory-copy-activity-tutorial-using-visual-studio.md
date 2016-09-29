<properties 
    pageTitle="Samouczek: tworzenie potoku za pomocą działania kopiowania przy użyciu programu Visual Studio |Microsoft Azure" 
    description="Ten samouczek zawiera instrukcje tworzenia potoku usługi Azure Data Factory za pomocą działania kopiowania przy użyciu programu Visual Studio." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/01/2016" 
    ms.author="spelluru"/>


# Samouczek: tworzenie potoku za pomocą działania kopiowania przy użyciu programu Visual Studio
> [AZURE.SELECTOR]
- [Przegląd i wymagania wstępne](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Interfejs API REST](data-factory-copy-activity-tutorial-using-rest-api.md)
- [Interfejs API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Kreator kopiowania](data-factory-copy-data-wizard-tutorial.md)

Ten samouczek pokazuje, jak utworzyć i monitorować fabrykę danych Azure przy użyciu programu Visual Studio. Potok w fabryce danych używa działania kopiowania w celu kopiowania danych z usługi Azure Blob Storage do usługi Azure SQL Database.

Poniżej przedstawiono czynności do wykonania w ramach tego samouczka:

1. Utworzysz dwie połączone usługi: **AzureStorageLinkedService1** i **AzureSqlLinkedService1**. Usługa AzureStorageLinkedService1 łączy z fabryką danych **ADFTutorialDataFactoryVS** konto magazynu Azure, a usługa AzureSqlLinkedService1 — bazę danych SQL Azure. Dane wejściowe dla potoku znajdują się w kontenerze obiektów blob w magazynie obiektów blob platformy Azure, a dane wyjściowe są przechowywane w tabeli w bazie danych SQL Azure. Te dwa magazyny danych są więc dodawane jako połączone usługi do fabryki danych.
2. Utworzysz dwie tabele fabryki danych — **EmpTableFromBlob** i **EmpSQLTable** — zawierające dane wejściowe/wyjściowe przechowywane w magazynach danych. Dla obiektu EmpTableFromBlob należy określić kontener obiektów blob zawierający obiekt blob z danymi źródłowymi. Dla tabeli EmpSQLTable należy określić tabelę SQL, która przechowuje dane wyjściowe. Należy określić również inne właściwości, takie jak struktura, dostępność i zasady.
3. Utworzysz potok o nazwie **ADFTutorialPipeline** w fabryce danych ADFTutorialDataFactoryVS. Potok obejmuje **działanie kopiowania**, które będzie kopiować dane wejściowe z obiektu blob platformy Azure do tabeli wyjściowej SQL Azure. Działanie kopiowania wykonuje operację przenoszenia danych w usłudze Azure Data Factory. Działanie jest obsługiwane przez globalnie dostępną usługę, która może kopiować dane między różnymi magazynami danych w sposób bezpieczny, niezawodny i skalowalny. Szczegółowe informacje dotyczące działania kopiowania znajdują się w artykule [Data Movement Activities](data-factory-data-movement-activities.md) (Działania przenoszenia danych). 
4. Utworzysz fabrykę danych i wdrożysz połączone usługi, tabele i potok.    

## Wymagania wstępne

1. Przeczytaj artykuł [Omówienie samouczka](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) oraz wykonaj kroki **wymagań wstępnych**. 
2. Aby publikować jednostki fabryki danych w usłudze Azure Data Factory, musisz być **administratorem subskrypcji platformy Azure**.  
3. Na komputerze muszą być zainstalowane następujące elementy: 
    - Visual Studio 2013 lub Visual Studio 2015
    - Pobierz zestaw Azure SDK dla programu Visual Studio 2013 lub Visual Studio 2015. Przejdź do [strony plików do pobrania Azure](https://azure.microsoft.com/downloads/) i kliknij pozycję **VS 2013** lub **VS 2015** w sekcji **.NET**.
    - Pobierz najnowszą wtyczkę usługi Azure Data Factory dla programu Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) lub [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Jeśli używasz programu Visual Studio 2013, możesz także zaktualizować wtyczkę, wykonując następujące czynności: w menu kliknij kolejno opcje **Narzędzia** -> **Rozszerzenia i aktualizacje** -> **Online** -> **Galeria Visual Studio** -> **Narzędzia usługi Fabryka danych Microsoft Azure dla programu Visual Studio** -> **Aktualizuj**. 
 


## Tworzenie projektu programu Visual Studio 
1. Uruchom program **Visual Studio 2013**. Kliknij pozycję **Plik**, wskaż polecenie **Nowy** i kliknij pozycję **Projekt**. Powinno zostać wyświetlone okno dialogowe **Nowy projekt**.  
2. W oknie dialogowym **Nowy projekt** wybierz szablon **DataFactory** i kliknij pozycję **Pusty projekt usługi Fabryka danych**. Jeśli nie widzisz szablonu DataFactory, zamknij program Visual Studio, zainstaluj zestaw Azure SDK dla programu Visual Studio 2013 i otwórz ponownie program Visual Studio.  

    ![Okno dialogowe Nowy projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)

3. Wprowadź **nazwę** dla projektu, **lokalizację** oraz nazwę **rozwiązania** i kliknij przycisk **OK**.

    ![Eksplorator rozwiązań](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png) 

## Tworzenie połączonych usług
Połączone usługi łączą magazyny danych lub usługi obliczeniowe z fabryką danych Azure. Magazynem danych może być magazyn Azure, baza danych SQL Azure lub lokalna baza danych programu SQL Server.

W tym kroku opisano tworzenie dwóch połączonych usług: **AzureStorageLinkedService1** i **AzureSqlLinkedService1**. Połączona usługa AzureStorageLinkedService1 łączy z fabryką danych **ADFTutorialDataFactory** konto usługi Azure Storage, a usługa AzureSqlLinkedService — bazę danych SQL Azure. 

### Tworzenie połączonej usługi Azure Storage

4. Kliknij prawym przyciskiem myszy pozycję **Połączone usługi** w Eksploratorze rozwiązań, wskaż polecenie **Dodaj** i kliknij pozycję **Nowy element**.      
5. W oknie dialogowym **Dodawanie nowego elementu** wybierz z listy pozycję **Połączona usługa Azure Storage** i kliknij przycisk **Dodaj**. 

    ![Nowa połączona usługa](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
 
3. Zastąp wartości **accountname** i **accountkey** nazwą konta magazynu Azure oraz jego kluczem. 

    ![Połączona usługa Azure Storage](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)

4. Zapisz plik **AzureStorageLinkedService1.json**.

### Tworzenie połączonej usługi SQL Azure

5. Kliknij ponownie prawym przyciskiem myszy węzeł **Połączone usługi** w **Eksploratorze rozwiązań**, wskaż polecenie **Dodaj** i kliknij opcję **Nowy element**. 
6. Tym razem wybierz pozycję **Połączona usługa SQL Azure** i kliknij przycisk **Dodaj**. 
7. W **pliku AzureSqlLinkedService1.json** zastąp wartości **servername**, **databasename**, **username@servername** oraz **password** nazwą serwera SQL Azure, nazwą bazy danych, kontem użytkownika i hasłem.    
8.  Zapisz plik **AzureSqlLinkedService1.json**. 


## Tworzenie zestawów danych
W poprzednim kroku utworzono połączone usługi **AzureStorageLinkedService1** i **AzureSqlLinkedService1** w celu powiązania konta usługi Azure Storage i bazy danych SQL Azure z fabryką danych **ADFTutorialDataFactory**. W tym kroku opisano definiowanie dwóch tabel fabryki danych — **EmpTableFromBlob** i **EmpSQLTable**, które zawierają dane wejściowe/wyjściowe przechowywane w magazynach danych przywoływanych przez usługę, odpowiednio, AzureStorageLinkedService1 i AzureSqlLinkedService1. Dla obiektu EmpTableFromBlob należy określić kontener obiektów blob zawierający obiekt blob z danymi źródłowymi. Dla tabeli EmpSQLTable należy określić tabelę SQL, która przechowuje dane wyjściowe.

### Tworzenie wejściowego zestawu danych

9. Kliknij prawym przyciskiem myszy pozycję **Tabele** w **Eksploratorze rozwiązań**, wskaż polecenie **Dodaj** i kliknij pozycję **Nowy element**.
10. W oknie dialogowym **Dodawanie nowego elementu** wybierz pozycję **Obiekt blob platformy Azure** i kliknij przycisk **Dodaj**.   
10. Zastąp tekst kodu JSON następującym tekstem i zapisz plik **AzureBlobLocation1.json**. 

        {
          "name": "EmpTableFromBlob",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

### Tworzenie wyjściowego zestawu danych

11. Ponownie kliknij prawym przyciskiem myszy pozycję **Tabele** w **Eksploratorze rozwiązań**, wskaż polecenie **Dodaj** i kliknij pozycję **Nowy element**.
12. W oknie dialogowym **Dodawanie nowego elementu** wybierz pozycję **Usługa SQL Azure** i kliknij przycisk **Dodaj**. 
13. Zastąp tekst kodu JSON następującym kodem JSON i zapisz plik **AzureSqlTableLocation1.json**.

        {
          "name": "EmpSQLTable",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService1",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

## Tworzenie potoku 
Do tej pory nastąpiło utworzenie połączonych usług i tabel dotyczących danych wejściowych/wyjściowych. Teraz utworzysz potok za pomocą **działania kopiowania**, aby skopiować dane z obiektu blob platformy Azure do bazy danych SQL Azure. 


1. Kliknij prawym przyciskiem myszy pozycję **Potoki** w **Eksploratorze rozwiązań**, wskaż polecenie **Dodaj** i kliknij pozycję **Nowy element**.  
15. Wybierz pozycję **Potok kopiowania danych** w oknie dialogowym **Dodawanie nowego elementu** i kliknij przycisk **Dodaj**. 
16. Zastąp kod JSON poniższym kodem JSON i zapisz plik **CopyActivity1.json**.
            
        {
          "name": "ADFTutorialPipeline",
          "properties": {
            "description": "Copy data from a blob to Azure SQL table",
            "activities": [
              {
                "name": "CopyFromBlobToSQL",
                "description": "Push Regional Effectiveness Campaign data to Azure SQL database",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "EmpTableFromBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "EmpSQLTable"
                  }
                ],
                "typeProperties": {
                  "source": {
                    "type": "BlobSource"
                  },
                  "sink": {
                    "type": "SqlSink",
                    "writeBatchSize": 10000,
                    "writeBatchTimeout": "60:00:00"
                  }
                },
                "Policy": {
                  "concurrency": 1,
                  "executionPriorityOrder": "NewestFirst",
                  "style": "StartOfInterval",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z",
            "isPaused": false
          }
        }

## Publikowanie/wdrażanie jednostek usługi Fabryka danych
  
18. Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, a następnie kliknij polecenie **Publikuj**. 
19. Jeśli zostanie wyświetlone okno dialogowe **Logowanie na koncie Microsoft** wprowadź poświadczenia dla konta z subskrypcją Azure i kliknij przycisk **Zaloguj**.
20. Powinno zostać wyświetlone następujące okno dialogowe:

    ![Okno dialogowe publikowania](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)

21. Na stronie konfigurowania fabryki danych wykonaj następujące czynności: 
    1. Zaznacz opcję **Utwórz nową fabrykę danych**.
    2. Wprowadź wartość **VSTutorialFactory** w polu **Nazwa**.  
    
        > [AZURE.NOTE]  
        > Nazwa fabryki danych Azure musi być globalnie unikatowa. Jeśli podczas publikowania wystąpi błąd dotyczący nazwy fabryki danych, zmień nazwę fabryki danych (np. twojanazwaVSTutorialFactory) i spróbuj opublikować ją ponownie. Artykuł [Data Factory — Naming Rules](data-factory-naming-rules.md) (Fabryka danych — zasady nazewnictwa) zawiera zasady nazewnictwa artefaktów usługi Fabryka danych.
        
    3. Wybierz odpowiednią subskrypcję w polu **Subskrypcja**. 
    4. Wybierz **grupę zasobów** dla fabryki danych, która ma być utworzona. 
    5. Wybierz **region** dla fabryki danych. 
    6. Kliknij przycisk **Dalej**, aby przejść na stronę **Publikowanie elementów**. 
23. Na stronie **Publikowanie elementów** upewnij się, że wszystkie jednostki usługi Fabryka danych zostały wybrane, i kliknij przycisk **Dalej**, aby przejść na stronę **Podsumowanie**.     
24. Przejrzyj podsumowanie i kliknij przycisk **Dalej**, aby rozpocząć proces wdrożenia oraz wyświetlić stronę **Stan wdrożenia**.
25. Na stronie **Stan wdrożenia** powinien zostać wyświetlony stan procesu wdrożenia. Po zakończeniu wdrożenia kliknij przycisk Zakończ. 

Pamiętaj o następujących kwestiach: 

- Jeśli zostanie wyświetlony komunikat o błędzie: „**Subskrypcja nie jest zarejestrowana w celu używania przestrzeni nazw Microsoft.DataFactory**”, wykonaj jedną z następujących czynności i spróbuj opublikować ponownie: 

    - W programie Azure PowerShell uruchom następujące polecenie, aby zarejestrować dostawcę usługi Fabryka danych. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Można uruchomić następujące polecenie, aby potwierdzić, że dostawca usługi Data Factory jest zarejestrowany. 
    
            Get-AzureRmResourceProvider
    - Zaloguj się przy użyciu subskrypcji Azure do [portalu Azure](https://portal.azure.com) i przejdź do bloku Fabryka danych lub utwórz fabrykę danych w portalu Azure. Ta akcja powoduje automatyczne zarejestrowanie dostawcy.
-   W przyszłości nazwa fabryki danych może zostać zarejestrowana jako nazwa DNS, a wówczas stanie się widoczna publicznie.
-   Aby tworzyć wystąpienia usługi Fabryka danych, musisz być współautorem/administratorem subskrypcji Azure

## Podsumowanie
W tym samouczku opisano tworzenie fabryki danych Azure w celu kopiowania danych z obiektu blob Azure do bazy danych SQL Azure. Program Visual Studio został użyty do utworzenia fabryki danych, połączonych usług, zestawów danych oraz potoku. Główne kroki opisane w tym samouczku:  

1.  Tworzenie **fabryki danych** Azure.
2.  Tworzenie **połączonych usług**:
    1. Połączona usługa **Azure Storage** w celu połączenia z kontem usługi Azure Storage, na którym przechowywane są dane wejściowe.    
    2. Połączona usługa **SQL Azure** w celu połączenia z bazą danych SQL Azure, w której przechowywane są dane wyjściowe. 
3.  Tworzenie **zestawów danych** opisujących dane wejściowe i wyjściowe dla potoków.
4.  Tworzenie **potoku** za pomocą **działania kopiowania**, w którym źródłem jest element **BlobSource**, a ujściem element **SqlSink**. 


## Korzystanie z Eksploratora serwera w celu wyświetlania fabryk danych

1. W programie **Visual Studio** kliknij w menu pozycję **Widok**, a następnie kliknij pozycję **Eksplorator serwera**.
2. W oknie Eksploratora serwera rozwiń węzeł **Azure**, a następnie węzeł **Fabryka danych**. Jeśli zostanie wyświetlony monit **Zaloguj się do programu Visual Studio**, wprowadź **konto** skojarzone z subskrypcją Azure i kliknij przycisk **Kontynuuj**. Wprowadź **hasło** i kliknij przycisk **Zaloguj**. Program Visual Studio podejmie próbę uzyskania informacji na temat wszystkich fabryk danych Azure w ramach danej subskrypcji. Stan tej operacji zostanie wyświetlony w oknie **Lista zadań usługi Data Factory**.
    ![Eksplorator serwera](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. Kliknij prawym przyciskiem myszy fabrykę danych i wybierz opcję Eksportuj fabrykę danych do nowego projektu, aby utworzyć projekt w programie Visual Studio na podstawie istniejącej fabryki danych.
    ![Eksportowanie fabryki danych do projektu programu VS](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## Aktualizacja narzędzi usługi Fabryka danych dla programu Visual Studio
Aby zaktualizować narzędzia usługi Fabryka danych Azure dla programu Visual Studio, wykonaj następujące czynności:

1. W menu kliknij pozycję **Narzędzia** i wybierz pozycję **Rozszerzenia i aktualizacje**. 
2. Wybierz pozycję **Aktualizacje** w lewym okienku, a następnie wybierz pozycję **Galeria Visual Studio**.
4. Wybierz pozycję **Narzędzia usługi Fabryka danych Azure dla programu Visual Studio** i kliknij przycisk **Aktualizuj**. Jeśli ta pozycja nie jest wyświetlana, masz już najnowszą wersję narzędzi. 

Zobacz artykuł [Monitor datasets and pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) (Monitorowanie zestawów danych i potoku), aby uzyskać instrukcje dotyczące korzystania z witryny Azure Portal do monitorowania potoku i zestawów danych utworzonych przez siebie w ramach tego samouczka.

## Zobacz też
| Temat | Opis |
| :---- | :---- |
| [Działania przenoszenia danych](data-factory-data-movement-activities.md) | Ten artykuł zawiera szczegółowe informacje dotyczące działania kopiowania używanego w tym samouczku. |
| [Planowanie i wykonywanie](data-factory-scheduling-and-execution.md) | W tym artykule wyjaśniono aspekty planowania i wykonywania modelu aplikacji usługi Fabryka danych Azure. |
| [Potoki](data-factory-create-pipelines.md) | Ten artykuł ułatwia zapoznanie się z potokami i działaniami w usłudze Azure Data Factory |
| [Zestawy danych](data-factory-create-datasets.md) | Ten artykuł ułatwia zapoznanie się z zestawami danych w usłudze Azure Data Factory.
| [Monitorowanie potoków i zarządzanie nimi za pomocą aplikacji do monitorowania](data-factory-monitor-manage-app.md) | Ten artykuł zawiera instrukcje dotyczące monitorowania i debugowania potoków oraz zarządzania nimi przy użyciu aplikacji do monitorowania i zarządzania. 



<!--HONumber=Sep16_HO3-->


