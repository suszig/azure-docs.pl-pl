---
title: "Samouczek: tworzenie potoku za pomocą działania kopiowania przy użyciu programu Visual Studio | Microsoft Docs"
description: "Ten samouczek zawiera instrukcje tworzenia potoku usługi Azure Data Factory za pomocą działania kopiowania przy użyciu programu Visual Studio."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1751185b-ce0a-4ab2-a9c3-e37b4d149ca3
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/02/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 4b29fd1c188c76a7c65c4dcff02dc9efdf3ebaee
ms.openlocfilehash: 01e1728d9658ac69659e31aca584cb574338a238


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>Samouczek: tworzenie potoku za pomocą działania kopiowania przy użyciu programu Visual Studio
> [!div class="op_single_selector"]
> * [Przegląd i wymagania wstępne](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Kreator kopiowania](data-factory-copy-data-wizard-tutorial.md)
> * [Witryna Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Program Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [Program PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Szablon usługi Azure Resource Manager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [Interfejs API REST](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [Interfejs API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

Ten samouczek pokazuje, jak utworzyć i monitorować fabrykę danych Azure przy użyciu programu Visual Studio. Potok w fabryce danych używa działania kopiowania w celu kopiowania danych z usługi Azure Blob Storage do usługi Azure SQL Database.

> [!NOTE]
> Potok danych przedstawiony w tym samouczku kopiuje dane ze źródłowego do docelowego magazynu danych. Nie przekształca on danych wejściowych w celu wygenerowania danych wyjściowych. Aby zapoznać się z samouczkiem dotyczącym przekształcania danych za pomocą usługi Azure Data Factory, zobacz [Tutorial: Build your first pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md) (Samouczek: Tworzenie pierwszego potoku przekształcającego dane przy użyciu klastra Hadoop).
> 
> Dwa działania można połączyć w łańcuch (uruchomić jedno działanie po drugim), ustawiając wyjściowy zestaw danych jednego działania jako zestaw wejściowy drugiego. Szczegółowe informacje znajdują się w artykule [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) (Planowanie i wykonywanie w usłudze Data Factory).

Poniżej przedstawiono kroki do wykonania w ramach tego samouczka:

1. Utworzysz dwie połączone usługi: **AzureStorageLinkedService1** i **AzureSqlLinkedService1**. 
   
    Usługa AzureStorageLinkedService1 łączy z fabryką danych **ADFTutorialDataFactoryVS** konto magazynu Azure, a usługa AzureSqlLinkedService1 — bazę danych SQL Azure. Dane wejściowe dla potoku znajdują się w kontenerze obiektów blob w magazynie obiektów blob platformy Azure, a dane wyjściowe są przechowywane w tabeli w bazie danych SQL Azure. Te dwa magazyny danych są więc dodawane jako połączone usługi do fabryki danych.
2. Utwórz dwa zestawy danych — **InputDataset** i **OutputDataset** — zawierające dane wejściowe i wyjściowe przechowywane w magazynach danych. 
   
    Dla zestawu InputDataset należy określić kontener obiektów blob zawierający obiekt blob z danymi źródłowymi. Dla zestawu OutputDataset należy określić tabelę SQL, która przechowuje dane wyjściowe. Należy określić również inne właściwości, takie jak struktura, dostępność i zasady.
3. Utworzysz potok o nazwie **ADFTutorialPipeline** w fabryce danych ADFTutorialDataFactoryVS. 
   
    Potok obejmuje **działanie kopiowania**, które będzie kopiować dane wejściowe z obiektu blob platformy Azure do tabeli wyjściowej SQL Azure. Działanie kopiowania wykonuje operację przenoszenia danych w usłudze Azure Data Factory. Działanie jest obsługiwane przez globalnie dostępną usługę, która może kopiować dane między różnymi magazynami danych w sposób bezpieczny, niezawodny i skalowalny. Szczegółowe informacje dotyczące działania kopiowania znajdują się w artykule [Data Movement Activities](data-factory-data-movement-activities.md) (Działania przenoszenia danych). 
4. Utwórz fabrykę danych o nazwie **VSTutorialFactory**. Wdróż fabrykę danych i wszystkie obiekty usługi Data Factory (połączone usługi, tabele i potok).    

## <a name="prerequisites"></a>Wymagania wstępne
1. Przeczytaj artykuł [Omówienie samouczka](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) oraz wykonaj kroki **wymagań wstępnych**. 
2. Aby publikować jednostki fabryki danych w usłudze Azure Data Factory, musisz być **administratorem subskrypcji platformy Azure**.  
3. Na komputerze muszą być zainstalowane następujące elementy: 
   * Visual Studio 2013 lub Visual Studio 2015
   * Pobierz zestaw Azure SDK dla programu Visual Studio 2013 lub Visual Studio 2015. Przejdź do [strony plików do pobrania Azure](https://azure.microsoft.com/downloads/) i kliknij pozycję **VS 2013** lub **VS 2015** w sekcji **.NET**.
   * Pobierz najnowszą wtyczkę usługi Azure Data Factory dla programu Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) lub [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Możesz także zaktualizować wtyczkę, wykonując następujące czynności: w menu kliknij kolejno opcje **Narzędzia** -> **Rozszerzenia i aktualizacje** -> **Online** -> **Galeria Visual Studio** -> **Narzędzia usługi Fabryka danych Microsoft Azure dla programu Visual Studio** -> **Aktualizuj**.

## <a name="create-visual-studio-project"></a>Tworzenie projektu programu Visual Studio
1. Uruchom program **Visual Studio 2013**. Kliknij pozycję **Plik**, wskaż polecenie **Nowy** i kliknij pozycję **Projekt**. Powinno zostać wyświetlone okno dialogowe **Nowy projekt**.  
2. W oknie dialogowym **Nowy projekt** wybierz szablon **DataFactory** i kliknij pozycję **Pusty projekt usługi Fabryka danych**. Jeśli nie widzisz szablonu DataFactory, zamknij program Visual Studio, zainstaluj zestaw Azure SDK dla programu Visual Studio 2013 i otwórz ponownie program Visual Studio.  
   
    ![Okno dialogowe Nowy projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)
3. Wprowadź **nazwę** dla projektu, **lokalizację** oraz nazwę **rozwiązania** i kliknij przycisk **OK**.
   
    ![Eksplorator rozwiązań](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png)    

## <a name="create-linked-services"></a>Tworzenie połączonych usług
Połączone usługi łączą magazyny danych lub usługi obliczeniowe z fabryką danych Azure. Artykuł [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) (Obsługiwane magazyny danych) zawiera listę wszystkich źródeł i ujść obsługiwanych przez działanie kopiowania. Artykuł [compute linked services](data-factory-compute-linked-services.md) (Obliczanie połączonych usług) zawiera listę usług obliczeniowych obsługiwanych przez usługę Data Factory. Ten samouczek nie obejmuje używania żadnej usługi obliczeniowej. 

W tym kroku opisano tworzenie dwóch połączonych usług: **AzureStorageLinkedService1** i **AzureSqlLinkedService1**. Połączona usługa AzureStorageLinkedService1 łączy z fabryką danych **ADFTutorialDataFactory** konto usługi Azure Storage, a usługa AzureSqlLinkedService — bazę danych SQL Azure. 

### <a name="create-the-azure-storage-linked-service"></a>Tworzenie połączonej usługi Azure Storage
1. Kliknij prawym przyciskiem myszy pozycję **Połączone usługi** w Eksploratorze rozwiązań, wskaż polecenie **Dodaj** i kliknij pozycję **Nowy element**.      
2. W oknie dialogowym **Dodawanie nowego elementu** wybierz z listy pozycję **Połączona usługa Azure Storage** i kliknij przycisk **Dodaj**. 
   
    ![Nowa połączona usługa](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
3. Zastąp wartości `<accountname>` i `<accountkey>`* nazwą konta magazynu Azure oraz jego kluczem. 
   
    ![Połączona usługa Azure Storage](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)
4. Zapisz plik **AzureStorageLinkedService1.json**.

> Szczegółowe informacje na temat właściwości JSON zawiera artykuł [Move data from/to Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) (Przenoszenie danych do i z usługi Azure Blob).
> 
> 

### <a name="create-the-azure-sql-linked-service"></a>Tworzenie połączonej usługi SQL Azure
1. Kliknij ponownie prawym przyciskiem myszy węzeł **Połączone usługi** w **Eksploratorze rozwiązań**, wskaż polecenie **Dodaj** i kliknij opcję **Nowy element**. 
2. Tym razem wybierz pozycję **Połączona usługa SQL Azure** i kliknij przycisk **Dodaj**. 
3. W pliku **AzureSqlLinkedService1.json** zastąp wartości `<servername>``<databasename>``<username@servername>``<password>`, wpisując nazwy serwera SQL Azure, bazy danych i konta użytkownika oraz hasło.    
4. Zapisz plik **AzureSqlLinkedService1.json**. 

> [!NOTE]
> Szczegółowe informacje na temat właściwości JSON zawiera artykuł [Move data from/to Azure SQL Database](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) (Przenoszenie danych do i z usługi Azure SQL Database).
> 
> 

## <a name="create-datasets"></a>Tworzenie zestawów danych
W poprzednim kroku utworzono połączone usługi **AzureStorageLinkedService1** i **AzureSqlLinkedService1** w celu powiązania konta usługi Azure Storage i bazy danych SQL Azure z fabryką danych **ADFTutorialDataFactory**. W tym kroku opisano definiowanie dwóch zestawów danych — **InputDataset** i **OutputDataset** — zawierających dane wejściowe i wyjściowe przechowywane w magazynach danych, do których odwołują się usługi AzureStorageLinkedService1 i AzureSqlLinkedService1. Dla zestawu InputDataset należy określić kontener obiektów blob zawierający obiekt blob z danymi źródłowymi. Dla zestawu OutputDataset należy określić tabelę SQL, która przechowuje dane wyjściowe.

### <a name="create-input-dataset"></a>Tworzenie wejściowego zestawu danych
W tym kroku opisano tworzenie zestawu danych o nazwie **InputDataset** wskazującego na kontener obiektów blob w usłudze Azure Storage reprezentowany przez połączoną usługę **AzureStorageLinkedService1**. Tabela jest prostokątnym zestawem danych i jest jedynym obsługiwanym obecnie typem zestawu danych. 

1. Kliknij prawym przyciskiem myszy pozycję **Tabele** w **Eksploratorze rozwiązań**, wskaż polecenie **Dodaj** i kliknij pozycję **Nowy element**.
2. W oknie dialogowym **Dodawanie nowego elementu** wybierz pozycję **Obiekt blob platformy Azure** i kliknij przycisk **Dodaj**.   
3. Zastąp tekst kodu JSON następującym tekstem i zapisz plik **AzureBlobLocation1.json**. 

  ```json   
  {
    "name": "InputDataset",
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
  ``` 
    Pamiętaj o następujących kwestiach: 
   
   * parametr **type** zestawu danych został ustawiony na **AzureBlob**.
   * parametr **linkedServiceName** został ustawiony na **AzureStorageLinkedService**. Ta połączona usługa została utworzona w kroku 2.
   * Parametr **folderPath** został ustawiony na kontener **adftutorial**. Możesz również określić nazwę obiektu blob znajdującego się w folderze przy użyciu właściwości **fileName**. Ponieważ nie określasz nazwy obiektu blob, dane z wszystkich obiektów blob w kontenerze są traktowane jako dane wejściowe.  
   * parametr **type** formatu został ustawiony na **TextFormat**
   * W pliku tekstowym znajdują się dwa pola — **FirstName** i **LastName** — oddzielone przecinkiem (**columnDelimiter**)    
   * Parametr **availability** został ustawiony na wartość **hourly** (parametr **frequency** ma wartość **hour**, a **interval** — **1**). W związku z tym usługa Data Factory szuka danych wejściowych co godzinę w folderze głównym określonego kontenera obiektów blob (**adftutorial**). 
   
   Jeśli nie określisz parametru **fileName** dla **wejściowego** zestawu danych, wszystkie pliki/obiekty blob z folderu danych wejściowych (**folderPath**) będą traktowane jako dane wejściowe. Jeśli określisz parametr fileName w kodzie JSON, tylko określony plik/obiekt blob będzie traktowany jako dane wejściowe.
   
   Jeśli nie określisz parametru **fileName** dla **tabeli wyjściowej**, wygenerowane pliki w ścieżce **folderPath** są nazywane według następującego formatu: Data.&lt;Guid\&gt;.txt (przykład: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).
   
   Aby ustawić parametry **folderPath** i **fileName** dynamicznie w oparciu o czas **SliceStart**, użyj właściwości **partitionedBy**. W poniższym przykładzie parametr folderPath używa elementów Year, Month i Day z parametru SliceStart (czas rozpoczęcia przetwarzania wycinka), a parametr fileName używa elementu Hour z parametru SliceStart. Na przykład jeśli wycinek jest generowany dla czasu 2016-09-20T08:00:00, parametr folderName zostaje ustawiony na wartość wikidatagateway/wikisampledataout/2016/09/20, a parametr fileName zostaje ustawiony na wartość 08.csv. 
  
    ```json   
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
    [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ```
            
> [!NOTE]
> Szczegółowe informacje na temat właściwości JSON zawiera artykuł [Move data from/to Azure Blob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) (Przenoszenie danych do i z usługi Azure Blob).
> 
> 

### <a name="create-output-dataset"></a>Tworzenie wyjściowego zestawu danych
W tym kroku tworzony jest wyjściowy zestaw danych o nazwie **OutputDataset**. Ten zestaw danych wskazuje tabelę SQL w bazie danych SQL Azure reprezentowanej przez usługę **AzureSqlLinkedService1**. 

1. Ponownie kliknij prawym przyciskiem myszy pozycję **Tabele** w **Eksploratorze rozwiązań**, wskaż polecenie **Dodaj** i kliknij pozycję **Nowy element**.
2. W oknie dialogowym **Dodawanie nowego elementu** wybierz pozycję **Usługa SQL Azure** i kliknij przycisk **Dodaj**. 
3. Zastąp tekst kodu JSON następującym kodem JSON i zapisz plik **AzureSqlTableLocation1.json**.

    ```json
    {
     "name": "OutputDataset",
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
    ```
   
    Pamiętaj o następujących kwestiach: 
   
   * parametr **type** zestawu danych został ustawiony na wartość **AzureSQLTable**.
   * Parametr **linkedServiceName** został ustawiony na wartość **AzureSqlLinkedService** (ta połączona usługa została utworzona w kroku 2).
   * Parametr **tablename** został ustawiony na wartość **emp**.
   * Tabela emp bazy danych zawiera trzy kolumny — **ID**, **FirstName** i **LastName**. ID to kolumna tożsamości, więc należy określić tylko wartości **FirstName** i **LastName**.
   * Parametr **availability** (dostępność) został ustawiony na wartość **hourly** (co godzinę) (parametr **frequency** [częstotliwość] został ustawiony na **hour** [godzinę], a **interval** [interwał] został ustawiony na wartość **1**).  Usługa Data Factory co godzinę generuje wycinek danych wyjściowych w tabeli **emp** w bazie danych Azure SQL Database.

> [!NOTE]
> Szczegółowe informacje na temat właściwości JSON zawiera artykuł [Move data from/to Azure SQL Database](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) (Przenoszenie danych do i z usługi Azure SQL Database).
> 
> 

## <a name="create-pipeline"></a>Tworzenie potoku
Do tej pory nastąpiło utworzenie połączonych usług i tabel dotyczących danych wejściowych/wyjściowych. Teraz utworzysz potok za pomocą **działania kopiowania**, aby skopiować dane z obiektu blob platformy Azure do bazy danych SQL Azure. 

1. Kliknij prawym przyciskiem myszy pozycję **Potoki** w **Eksploratorze rozwiązań**, wskaż polecenie **Dodaj** i kliknij pozycję **Nowy element**.  
2. Wybierz pozycję **Potok kopiowania danych** w oknie dialogowym **Dodawanie nowego elementu** i kliknij przycisk **Dodaj**. 
3. Zastąp kod JSON poniższym kodem JSON i zapisz plik **CopyActivity1.json**.

    ```json   
    {
     "name": "ADFTutorialPipeline",
     "properties": {
       "description": "Copy data from a blob to Azure SQL table",
       "activities": [
         {
           "name": "CopyFromBlobToSQL",
           "type": "Copy",
           "inputs": [
             {
               "name": "InputDataset"
             }
           ],
           "outputs": [
             {
               "name": "OutputDataset"
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
    ```   
   Pamiętaj o następujących kwestiach:
   
   * W sekcji działań jest tylko jedno działanie, którego parametr **type** (typ) został ustawiony na wartość **Copy**.
   * Dane wejściowe dla działania mają ustawienie **InputDataset**, a dane wyjściowe — **OutputDataset**.
   * W sekcji **typeProperties** parametr **BlobSource** został określony jako typ źródłowy, a parametr **SqlSink** został określony jako typ ujścia.
   
   Zastąp wartość właściwości **start** datą bieżącą, a wartość **end** datą jutrzejszą. Możesz określić tylko część daty i pominąć część godziny parametru data/godzina. Na przykład „2016-02-03” jest odpowiednikiem „2016-02-03T00:00:00Z”.
   
   Zarówno data/godzina rozpoczęcia, jak i data/godzina zakończenia muszą być w [formacie ISO](http://en.wikipedia.org/wiki/ISO_8601). Przykładowo: 2016-10-14T16:32:41Z. Czas **end** jest opcjonalny, ale w tym samouczku zostanie użyty. 
   
   Jeśli nie określisz wartości dla właściwości **end**, zostanie ona obliczona jako „**czas rozpoczęcia + 48 godzin**”. Aby uruchomić potok bezterminowo, określ **9999-09-09** jako wartość właściwości **end**.
   
   W powyższym przykładzie występują 24 wycinki danych, gdyż poszczególne wycinki są generowane co godzinę.

## <a name="publishdeploy-data-factory-entities"></a>Publikowanie/wdrażanie jednostek usługi Fabryka danych
W tym kroku publikowane są utworzone wcześniej obiekty usługi Data Factory (połączone usługi, zestawy danych i potok). Należy również określić nazwę nowej fabryki danych, która zostanie utworzona w celu przechowywania tych obiektów.  

1. Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, a następnie kliknij polecenie **Publikuj**. 
2. Jeśli zostanie wyświetlone okno dialogowe **Logowanie na koncie Microsoft** wprowadź poświadczenia dla konta z subskrypcją Azure i kliknij przycisk **Zaloguj**.
3. Powinno zostać wyświetlone następujące okno dialogowe:
   
   ![Okno dialogowe publikowania](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
4. Na stronie konfigurowania fabryki danych wykonaj następujące czynności: 
   
   1. Zaznacz opcję **Utwórz nową fabrykę danych**.
   2. Wprowadź wartość **VSTutorialFactory** w polu **Nazwa**.  
      
      > [!IMPORTANT]
      > Nazwa fabryki danych Azure musi być globalnie unikatowa. Jeśli podczas publikowania wystąpi błąd dotyczący nazwy fabryki danych, zmień nazwę fabryki danych (np. twojanazwaVSTutorialFactory) i spróbuj opublikować ją ponownie. Artykuł [Data Factory — Naming Rules](data-factory-naming-rules.md) (Fabryka danych — zasady nazewnictwa) zawiera zasady nazewnictwa artefaktów usługi Fabryka danych.        
      > 
      > 
   3. Wybierz swoją subskrypcję platformy Azure w polu **Subskrypcja**.
      
      > [!IMPORTANT]
      > Jeśli nie jest widoczna żadna subskrypcja, upewnij się, że do logowania zostało użyte konto o uprawnieniach administratora lub współadministratora subskrypcji.  
      > 
      > 
   4. Wybierz **grupę zasobów** dla fabryki danych, która ma być utworzona. 
   5. Wybierz **region** dla fabryki danych. Na liście rozwijanej są wyświetlane tylko regiony obsługiwane przez usługę Data Factory.
   6. Kliknij przycisk **Dalej**, aby przejść na stronę **Publikowanie elementów**.
      
       ![Strona konfiguracji fabryki danych](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
5. Na stronie **Publikowanie elementów** upewnij się, że wszystkie jednostki usługi Fabryka danych zostały wybrane, i kliknij przycisk **Dalej**, aby przejść na stronę **Podsumowanie**.
   
   ![Strona publikowania elementów](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
6. Przejrzyj podsumowanie i kliknij przycisk **Dalej**, aby rozpocząć proces wdrożenia oraz wyświetlić stronę **Stan wdrożenia**.
   
   ![Strona publikowania podsumowania](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
7. Na stronie **Stan wdrożenia** powinien zostać wyświetlony stan procesu wdrożenia. Po zakończeniu wdrożenia kliknij przycisk Zakończ.
 
   ![Strona stanu wdrożenia](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png)

Pamiętaj o następujących kwestiach: 

* Jeśli zostanie wyświetlony komunikat o błędzie: „**Subskrypcja nie jest zarejestrowana w celu używania przestrzeni nazw Microsoft.DataFactory**”, wykonaj jedną z następujących czynności i spróbuj opublikować ponownie: 
  
  * W programie Azure PowerShell uruchom następujące polecenie, aby zarejestrować dostawcę usługi Fabryka danych. 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    Można uruchomić następujące polecenie, aby potwierdzić, że dostawca usługi Data Factory jest zarejestrowany. 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Zaloguj się przy użyciu subskrypcji Azure do [portalu Azure](https://portal.azure.com) i przejdź do bloku Fabryka danych lub utwórz fabrykę danych w portalu Azure. Ta akcja powoduje automatyczne zarejestrowanie dostawcy.
* W przyszłości nazwa fabryki danych może zostać zarejestrowana jako nazwa DNS, a wówczas stanie się widoczna publicznie.

> [!IMPORTANT]
> Aby tworzyć wystąpienia usługi Data Factory, musisz być administratorem lub współadministratorem subskrypcji platformy Azure
> 
> 

## <a name="summary"></a>Podsumowanie
W tym samouczku opisano tworzenie fabryki danych Azure w celu kopiowania danych z obiektu blob Azure do bazy danych SQL Azure. Program Visual Studio został użyty do utworzenia fabryki danych, połączonych usług, zestawów danych oraz potoku. Główne kroki opisane w tym samouczku:  

1. Tworzenie **fabryki danych** Azure.
2. Tworzenie **połączonych usług**:
   1. Połączona usługa **Azure Storage** w celu połączenia z kontem usługi Azure Storage, na którym przechowywane są dane wejściowe.     
   2. Połączona usługa **SQL Azure** w celu połączenia z bazą danych SQL Azure, w której przechowywane są dane wyjściowe. 
3. Tworzenie **zestawów danych** opisujących dane wejściowe i wyjściowe dla potoków.
4. Tworzenie **potoku** za pomocą **działania kopiowania**, w którym źródłem jest element **BlobSource**, a ujściem element **SqlSink**. 

## <a name="use-server-explorer-to-view-data-factories"></a>Korzystanie z Eksploratora serwera w celu wyświetlania fabryk danych
1. W programie **Visual Studio** kliknij w menu pozycję **Widok**, a następnie kliknij pozycję **Eksplorator serwera**.
2. W oknie Eksploratora serwera rozwiń węzeł **Azure**, a następnie węzeł **Fabryka danych**. Jeśli zostanie wyświetlony monit **Zaloguj się do programu Visual Studio**, wprowadź **konto** skojarzone z subskrypcją Azure i kliknij przycisk **Kontynuuj**. Wprowadź **hasło** i kliknij przycisk **Zaloguj**. Program Visual Studio podejmie próbę uzyskania informacji na temat wszystkich fabryk danych Azure w ramach danej subskrypcji. Stan tej operacji zostanie wyświetlony w oknie **Lista zadań usługi Data Factory**.

    ![Eksplorator serwera](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. Kliknij prawym przyciskiem myszy fabrykę danych i wybierz opcję Eksportuj fabrykę danych do nowego projektu, aby utworzyć projekt w programie Visual Studio na podstawie istniejącej fabryki danych.

    ![Eksportowanie fabryki danych do projektu programu VS](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a>Aktualizacja narzędzi usługi Fabryka danych dla programu Visual Studio
Aby zaktualizować narzędzia usługi Fabryka danych Azure dla programu Visual Studio, wykonaj następujące czynności:

1. W menu kliknij pozycję **Narzędzia** i wybierz pozycję **Rozszerzenia i aktualizacje**. 
2. Wybierz pozycję **Aktualizacje** w lewym okienku, a następnie wybierz pozycję **Galeria Visual Studio**.
3. Wybierz pozycję **Narzędzia usługi Fabryka danych Azure dla programu Visual Studio** i kliknij przycisk **Aktualizuj**. Jeśli ta pozycja nie jest wyświetlana, masz już najnowszą wersję narzędzi. 

Zobacz artykuł [Monitor datasets and pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) (Monitorowanie zestawów danych i potoku), aby uzyskać instrukcje dotyczące korzystania z witryny Azure Portal do monitorowania potoku i zestawów danych utworzonych przez siebie w ramach tego samouczka.

## <a name="see-also"></a>Zobacz też
| Temat | Opis |
|:--- |:--- |
| [Potoki](data-factory-create-pipelines.md) |Ten artykuł ułatwia zapoznanie się z potokami i działaniami w usłudze Azure Data Factory |
| [Zestawy danych](data-factory-create-datasets.md) |Ten artykuł ułatwia zapoznanie się z zestawami danych w usłudze Azure Data Factory. |
| [Planowanie i wykonywanie](data-factory-scheduling-and-execution.md) |W tym artykule wyjaśniono aspekty planowania i wykonywania modelu aplikacji usługi Fabryka danych Azure. |
| [Monitorowanie potoków i zarządzanie nimi za pomocą aplikacji do monitorowania](data-factory-monitor-manage-app.md) |Ten artykuł zawiera instrukcje dotyczące monitorowania i debugowania potoków oraz zarządzania nimi przy użyciu aplikacji do monitorowania i zarządzania. |




<!--HONumber=Feb17_HO1-->


