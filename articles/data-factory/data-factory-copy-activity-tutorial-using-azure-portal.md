---
title: "Samouczek: tworzenie potoku za pomocą działania kopiowania przy użyciu witryny Azure Portal | Microsoft Docs"
description: "Ten samouczek zawiera instrukcje tworzenia potoku usługi Azure Data Factory za pomocą działania kopiowania przy użyciu Edytora fabryki danych w witrynie Azure Portal."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d9317652-0170-4fd3-b9b2-37711272162b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/06/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 3205077236dd44253b3fa36d6eace36fb307871e
ms.openlocfilehash: 2fe52756ea5522e0d9d763afc1c89d45bf830877


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-portal"></a>Samouczek: tworzenie potoku za pomocą działania kopiowania przy użyciu witryny Azure Portal
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

Ten samouczek pokazuje, jak utworzyć i monitorować fabrykę danych Azure przy użyciu witryny Azure Portal. Potok w fabryce danych używa działania kopiowania w celu kopiowania danych z usługi Azure Blob Storage do usługi Azure SQL Database.

Poniżej przedstawiono kroki do wykonania w ramach tego samouczka:

| Krok | Opis |
| --- | --- |
| [Tworzenie fabryki danych Azure Data Factory](#create-data-factory) |W tym kroku opisano tworzenie fabryki danych Azure o nazwie **ADFTutorialDataFactory**. |
| [Tworzenie połączonych usług](#create-linked-services) |W tym kroku opisano tworzenie dwóch połączonych usług: **AzureStorageLinkedService** i **AzureSqlLinkedService**. <br/><br/>Usługa AzureStorageLinkedService łączy magazyn Azure, a usługa AzureSqlLinkedService — bazę danych Azure SQL z parametrem ADFTutorialDataFactory. Dane wejściowe dla potoku znajdują się w kontenerze obiektów blob w usłudze magazynie obiektów blob Azure, a dane wyjściowe będą przechowywane w tabeli w bazie danych SQL Azure. Te dwa magazyny danych są więc dodawane jako połączone usługi do fabryki danych. |
| [Tworzenie wejściowych i wyjściowych zestawów danych](#create-datasets) |W poprzednim kroku utworzono połączone usługi, które odwołują się do magazynów danych zawierających dane wejściowe i wyjściowe. W tym kroku opisano definiowanie dwóch zestawów danych — **InputDataset** i **OutputDataset** — zawierających dane wejściowe i wyjściowe przechowywane w magazynach danych. <br/><br/>Dla zestawu InputDataset zostanie określony kontener obiektów blob zawierający obiekt blob z danymi źródłowymi, a dla zestawu OutputDataset zostanie określona tabela SQL, w której będą przechowywane dane wyjściowe. Należy określić również inne właściwości, takie jak struktura, dostępność i zasady. |
| [Tworzenie potoku](#create-pipeline) |W tym kroku opisano tworzenie potoku o nazwie **ADFTutorialPipeline** w fabryce ADFTutorialDataFactory. <br/><br/>Do potoku zostanie dodane **działanie kopiowania**, które będzie kopiować dane wejściowe z obiektu blob platformy Azure do tabeli wyjściowej SQL Azure. Działanie kopiowania wykonuje operację przenoszenia danych w usłudze Azure Data Factory. Jest obsługiwane przez globalnie dostępną usługę, która może kopiować dane między różnymi magazynami danych w sposób bezpieczny, niezawodny i skalowalny. Szczegółowe informacje dotyczące działania kopiowania znajdują się w artykule [Data Movement Activities](data-factory-data-movement-activities.md) (Działania przenoszenia danych). |
| [Monitorowanie potoku](#monitor-pipeline) |W tym kroku opisano monitorowanie wycinków tabel wejściowych i wyjściowych przy użyciu witryny Azure Portal. |

## <a name="prerequisites"></a>Wymagania wstępne
Przed wykonaniem instrukcji z tego samouczka wykonaj działania dotyczące wymagań wstępnych podane w artykule [Omówienie samouczka](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="create-data-factory"></a>Tworzenie fabryki danych
W tym kroku opisano tworzenie fabryki danych Azure o nazwie **ADFTutorialDataFactory** przy użyciu witryny Azure Portal.

1. Po zalogowaniu się w witrynie [Azure Portal](https://portal.azure.com/) kliknij pozycję **Nowy**, wybierz pozycję **Zbieranie danych i analiza** i kliknij pozycję **Fabryka danych**. 
   
   ![Nowy->Fabryka danych](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)    
2. W bloku **Nowa fabryka danych**:
   
   1. Wprowadź wartość **ADFTutorialDataFactory** dla opcji **Nazwa**. 
      
         ![Blok Nowa fabryka danych](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)
      
       Nazwa fabryki danych platformy Azure musi być **globalnie unikatowa**. Jeśli wystąpi poniższy błąd, zmień nazwę fabryki danych (np. twojanazwaADFTutorialDataFactory) i spróbuj utworzyć ją ponownie. Artykuł [Data Factory — Naming Rules](data-factory-naming-rules.md) (Fabryka danych — zasady nazewnictwa) zawiera zasady nazewnictwa artefaktów usługi Fabryka danych.
      
           Data factory name “ADFTutorialDataFactory” is not available  
      
       ![Nazwa fabryki danych jest niedostępna](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
   2. Wybierz swoją **subskrypcję** platformy Azure.
   3. Wykonaj jedną z następujących czynności dotyczącą grupy zasobów:
      
      - Wybierz pozycję **Użyj istniejącej**, a następnie wybierz istniejącą grupę zasobów z listy rozwijanej. 
      - Wybierz pozycję **Utwórz nową**, a następnie wprowadź nazwę grupy zasobów.   
         
          W niektórych krokach w tym samouczku zakłada się, że nazwa grupy zasobów to **ADFTutorialResourceGroup**. Informacje na temat grup zasobów znajdują się w artykule [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md) (Używanie grup zasobów do zarządzania zasobami platformy Azure).  
   4. Na liście **lokalizacja** wybierz lokalizację fabryki danych. Na liście rozwijanej są wyświetlane tylko regiony obsługiwane przez usługę Data Factory.
   5. Wybierz pozycję **Przypnij do tablicy startowej**.     
   6. Kliknij przycisk **Utwórz**.
      
      > [!IMPORTANT]
      > Aby utworzyć wystąpienia usługi Data Factory, użytkownik musi być członkiem roli [współautora usługi Data Factory](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) na poziomie subskrypcji/grupy zasobów.
      > 
      > W przyszłości nazwa fabryki danych może zostać zarejestrowana jako nazwa DNS, a wówczas stanie się widoczna publicznie.                
      > 
      > 
3. Aby wyświetlić powiadomienia i komunikaty dotyczące stanu, kliknij ikonę dzwonka na pasku narzędzi. 
   
   ![Powiadomienia](./media/data-factory-copy-activity-tutorial-using-azure-portal/Notifications.png) 
4. Po zakończeniu tworzenia zostanie wyświetlony blok **Fabryka danych**, jak pokazano na poniższej ilustracji.
   
   ![Strona główna fabryki danych](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## <a name="create-linked-services"></a>Tworzenie połączonych usług
Połączone usługi łączą magazyny danych lub usługi obliczeniowe z fabryką danych Azure. Artykuł [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) (Obsługiwane magazyny danych) zawiera listę wszystkich źródeł i ujść obsługiwanych przez działanie kopiowania. Artykuł [compute linked services](data-factory-compute-linked-services.md) (Obliczanie połączonych usług) zawiera listę usług obliczeniowych obsługiwanych przez usługę Data Factory. Ten samouczek nie obejmuje używania żadnej usługi obliczeniowej. 

W tym kroku opisano tworzenie dwóch połączonych usług: **AzureStorageLinkedService** i **AzureSqlLinkedService**. Połączona usługa AzureStorageLinkedService łączy konto usługi Magazyn Azure, a AzureSqlLinkedService — bazę danych SQL Azure z parametrem **ADFTutorialDataFactory**. W dalszej części tego samouczka opisano tworzenie potoku, który kopiuje dane z kontenera obiektów blob w usłudze AzureStorageLinkedService do tabeli SQL w usłudze AzureSqlLinkedService.

### <a name="create-a-linked-service-for-the-azure-storage-account"></a>Tworzenie połączonej usługi dla konta magazynu Azure
1. W bloku **Fabryka danych** kliknij kafelek **Utwórz i wdróż**, aby uruchomić **edytor** dla fabryki danych.
   
   ![Kafelek Utwórz i wdróż](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
2. W **edytorze** kliknij przycisk **Nowy magazyn danych** na pasku narzędzi i wybierz opcję **Magazyn Azure** z menu rozwijanego. W okienku po prawej stronie powinien zostać wyświetlony szablon JSON do tworzenia połączonej usługi magazynu Azure. 
   
    ![Przycisk Nowy magazyn danych w edytorze](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
3. Zastąp parametry `<accountname>` i `<accountkey>` wartościami nazwy konta i klucza konta magazynu Azure. 
   
    ![Skrypt JSON w edytorze usługi Blob Storage](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png)    
4. Kliknij pozycję **Wdróż** na pasku narzędzi. W widoku drzewa powinna być widoczna wdrożona usługa **AzureStorageLinkedService**. 
   
    ![Przycisk Wdróż w edytorze usługi Blob Storage](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

> [!NOTE]
> Szczegółowe informacje na temat właściwości JSON zawiera artykuł [Move data from/to Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) (Przenoszenie danych do i z usługi Azure Blob).
> 
> 

### <a name="create-a-linked-service-for-the-azure-sql-database"></a>Tworzenie połączonej usługi dla bazy danych SQL Azure
1. W **Edytorze fabryki danych** kliknij przycisk **Nowy magazyn danych** na pasku narzędzi i wybierz opcję **Baza danych SQL Azure** z menu rozwijanego. W okienku po prawej stronie powinien zostać wyświetlony szablon JSON do tworzenia połączonej usługi serwera SQL Azure.
2. Zastąp parametry `<servername>`, `<databasename>`, `<username>@<servername>` i `<password>`, wpisując nazwy serwera SQL Azure, bazy danych i konta użytkownika oraz hasło. 
3. Kliknij przycisk **Wdróż** na pasku narzędzi, aby utworzyć i wdrożyć usługę **AzureSqlLinkedService**.
4. Upewnij się, że w widoku drzewa jest wyświetlana nazwa usługi **AzureSqlLinkedService**. 

> [!NOTE]
> Szczegółowe informacje na temat właściwości JSON zawiera artykuł [Move data from/to Azure SQL Database](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) (Przenoszenie danych do i z usługi Azure SQL Database).
> 
> 

## <a name="create-datasets"></a>Tworzenie zestawów danych
W poprzednim kroku utworzono połączone usługi **AzureStorageLinkedService** i **AzureSqlLinkedService** w celu powiązania konta usługi Azure Storage i bazy danych SQL Azure z fabryką danych **ADFTutorialDataFactory**. W tym kroku opisano definiowanie dwóch zestawów danych — **InputDataset** i **OutputDataset** — zawierających dane wejściowe i wyjściowe przechowywane w magazynach danych, do których odwołują się usługi AzureStorageLinkedService i AzureSqlLinkedService. Dla zestawu InputDataset zostanie określony kontener obiektów blob zawierający obiekt blob z danymi źródłowymi, a dla zestawu OutputDataset zostanie określona tabela SQL, w której będą przechowywane dane wyjściowe. 

### <a name="create-input-dataset"></a>Tworzenie wejściowego zestawu danych
W tym kroku opisano tworzenie zestawu danych o nazwie **InputDataset** wskazującego na kontener obiektów blob w usłudze Azure Storage reprezentowany przez połączoną usługę **AzureStorageLinkedService**.

1. W **edytorze** fabryki danych kliknij kolejno polecenia ** Więcej**, **Nowy zestaw danych** i **Azure Blob Storage** w menu rozwijanym. 
   
    ![Menu Nowy zestaw danych](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. Zastąp kod JSON w prawym okienku następującym fragmentem kodu JSON: 
   
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
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "fileName": "emp.txt",
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
     
           "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
           "fileName": "{Hour}.csv",
           "partitionedBy": 
           [
               { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
               { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
               { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
               { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
           ],
3. Kliknij przycisk **Wdróż** na pasku narzędzi, aby utworzyć i wdrożyć zestaw danych **InputDataset**. Upewnij się, że zestaw **InputDataset** jest wyświetlany w widoku drzewa.

> [!NOTE]
> Szczegółowe informacje na temat właściwości JSON zawiera artykuł [Move data from/to Azure Blob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) (Przenoszenie danych do i z usługi Azure Blob).
> 
> 

### <a name="create-output-dataset"></a>Tworzenie wyjściowego zestawu danych
W tej części kroku tworzony jest wyjściowy zestaw danych o nazwie **OutputDataset**. Ten zestaw danych wskazuje tabelę SQL w bazie danych SQL Azure reprezentowanej przez usługę **AzureSqlLinkedService**. 

1. W **edytorze** fabryki danych kliknij kolejno polecenia ** Więcej**, **Nowy zestaw danych** i **SQL Azure** w menu rozwijanym. 
2. Zastąp kod JSON w prawym okienku następującym fragmentem kodu JSON:
   
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
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }
   
     Pamiętaj o następujących kwestiach: 
   
   * parametr **type** zestawu danych został ustawiony na wartość **AzureSQLTable**.
   * Parametr **linkedServiceName** został ustawiony na wartość **AzureSqlLinkedService** (ta połączona usługa została utworzona w kroku 2).
   * Parametr **tablename** został ustawiony na wartość **emp**.
   * Tabela emp bazy danych zawiera trzy kolumny — **ID**, **FirstName** i **LastName**. ID to kolumna tożsamości, więc należy określić tylko wartości **FirstName** i **LastName**.
   * Parametr **availability** (dostępność) został ustawiony na wartość **hourly** (co godzinę) (parametr **frequency** [częstotliwość] został ustawiony na **hour** [godzinę], a **interval** [interwał] został ustawiony na wartość **1**).  Usługa Data Factory co godzinę generuje wycinek danych wyjściowych w tabeli **emp** w bazie danych Azure SQL Database.
3. Kliknij przycisk **Wdróż** na pasku narzędzi, aby utworzyć i wdrożyć zestaw danych **OutputDataset**. Upewnij się, że zestaw **OutputDataset** jest wyświetlany w widoku drzewa. 

> [!NOTE]
> Szczegółowe informacje na temat właściwości JSON zawiera artykuł [Move data from/to Azure SQL Database](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) (Przenoszenie danych do i z usługi Azure SQL Database).
> 
> 

## <a name="create-pipeline"></a>Tworzenie potoku
W tym kroku opisano tworzenie potoku za pomocą **działania kopiowania**, w którym parametr **InputDataset** jest używany jako dane wejściowe, a parametr **OutputDataset** jako dane wyjściowe.

1. W **edytorze** fabryki danych kliknij kolejno polecenia ** Więcej** i **Nowy potok**. Możesz również kliknąć prawym przyciskiem myszy opcję **Potoki** w widoku drzewa i kliknąć opcję **Nowy potok**.
2. Zastąp kod JSON w prawym okienku następującym fragmentem kodu JSON: 
   
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
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2016-07-12T00:00:00Z",
            "end": "2016-07-13T00:00:00Z"
          }
        } 
   
    Pamiętaj o następujących kwestiach:
   
   * W sekcji działań jest tylko jedno działanie, którego parametr **type** (typ) został ustawiony na wartość **Copy**.
   * Dane wejściowe dla działania mają ustawienie **InputDataset**, a dane wyjściowe — **OutputDataset**.
   * W sekcji **typeProperties** parametr **BlobSource** został określony jako typ źródłowy, a parametr **SqlSink** został określony jako typ ujścia.
     
     Zastąp wartość właściwości **start** datą bieżącą, a wartość **end** datą jutrzejszą. Możesz określić tylko część daty i pominąć część godziny parametru data/godzina. Na przykład „2016-02-03” jest odpowiednikiem „2016-02-03T00:00:00Z”.
     
     Zarówno data/godzina rozpoczęcia, jak i data/godzina zakończenia muszą być w [formacie ISO](http://en.wikipedia.org/wiki/ISO_8601). Przykładowo: 2016-10-14T16:32:41Z. Czas **end** jest opcjonalny, ale w tym samouczku zostanie użyty. 
     
     Jeśli nie określisz wartości dla właściwości **end**, zostanie ona obliczona jako „**czas rozpoczęcia + 48 godzin**”. Aby uruchomić potok bezterminowo, określ **9999-09-09** jako wartość właściwości **end**.
     
     W powyższym przykładzie występują 24 wycinki danych, gdyż poszczególne wycinki są generowane co godzinę.
3. Kliknij przycisk **Wdróż** na pasku narzędzi, aby utworzyć i wdrożyć element **ADFTutorialPipeline**. Upewnij się, że potok jest wyświetlany w widoku drzewa. 
4. Teraz zamknij blok **Edytor**, klikając przycisk **X**. Kliknij ponownie przycisk **X**, aby wyświetlić stronę główną **fabryki danych** dla fabryki **ADFTutorialDataFactory**.

**Gratulacje!** Udało Ci się utworzyć fabrykę danych Azure, połączone usługi, tabele i potok oraz zaplanować potok.   

### <a name="view-the-data-factory-in-a-diagram-view"></a>Wyświetlanie fabryki danych w widoku diagramu
1. W bloku **Fabryka danych** kliknij opcję **Diagram**.
   
    ![Blok Fabryka danych — kafelek Diagram](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. Powinien zostać wyświetlony diagram podobny do tego na poniższej ilustracji: 
   
    ![Widok diagramu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)
   
    Możesz powiększyć, pomniejszyć, powiększyć do 100%, powiększyć do dopasowania, automatycznie rozmieścić potoki i tabele oraz wyświetlić informacje dotyczące elementów powiązanych (powoduje wyróżnienie elementów nadrzędnych i podrzędnych wybranych elementów).  Możesz kliknąć dwukrotnie obiekt (tabelę wejściową/wyjściową lub potok), aby wyświetlić jego właściwości. 
3. Kliknij prawym przyciskiem myszy potok **ADFTutorialPipeline** w widoku diagramu i kliknij przycisk **Otwórz potok**. 
   
    ![Otwieranie potoku](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenPipeline.png)
4. Powinny zostać wyświetlone działania w potoku razem ze swoimi wejściowymi i wyjściowymi zestawami danych. W tym samouczku w potoku znajduje się tylko jedno działanie (działanie kopiowania) obejmujące obiekt InputDataset jako wejściowy zestaw danych i obiekt OutputDataset jako wyjściowy zestaw danych.   
   
    ![Widok otwartego potoku](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenedPipeline.png)
5. Kliknij przycisk **Fabryka danych** w obszarze nawigacji w lewym górnym rogu, aby wrócić do widoku diagramu. W widoku diagramu są wyświetlane wszystkie potoki. W tym przykładzie utworzono tylko jeden potok.   

## <a name="monitor-pipeline"></a>Monitorowanie potoku
W tym kroku opisano użycie witryny Azure Portal do monitorowania tego, co dzieje się w fabryce danych Azure. 

### <a name="monitor-pipeline-using-diagram-view"></a>Monitorowanie potoku przy użyciu widoku diagramu
1. Kliknij przycisk **X**, aby zamknąć widok **Diagram** i wyświetlić stronę główną usługi Data Factory dla fabryki danych. Jeśli zamknięto przeglądarkę sieci Web, wykonaj następujące czynności: 
   1. Przejdź do witryny [Azure Portal](https://portal.azure.com/). 
   2. Kliknij dwukrotnie opcję **ADFTutorialDataFactory** na **tablicy startowej** lub kliknij pozycję **Fabryki danych** w menu po lewej stronie, a następnie wyszukaj nazwę ADFTutorialDataFactory. 
2. Powinna zostać wyświetlona liczba tabel i potoków utworzonych w tym bloku oraz ich nazwy.
   
    ![strona główna z nazwami](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactory-home-page-pipeline-tables.png)
3. Kliknij kafelek **Zestawy danych**.
4. W bloku **Zestawy danych** kliknij zestaw **InputDataset**. Ten zestaw danych to wejściowy zestaw danych dla potoku **ADFTutorialPipeline**.
   
    ![Zestawy danych z wybranym zestawem InputDataset](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. Kliknij przycisk **... (wielokropek)**, aby wyświetlić wszystkie wycinki danych.
   
    ![Wszystkie wycinki danych wejściowych](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  
   
    Zwróć uwagę, że wszystkie wycinki danych do bieżącego czasu są w stanie **Gotowe**, ponieważ plik **emp.txt** znajduje się cały czas w kontenerze obiektów blob **adftutorial\input**. Upewnij się, że żadne wycinki nie są wyświetlane w sekcji **Ostatnie wycinki z błędami** na dole.
   
    Obie listy — **Ostatnio zaktualizowane wycinki** i **Ostatnie wycinki z błędami** — są posortowane według parametru **CZAS OSTATNIEJ AKTUALIZACJI**. 
   
    Kliknij przycisk **Filtruj** na pasku narzędzi, aby filtrować wycinki.  
   
    ![Filtrowanie wycinków wejściowych](./media/data-factory-copy-activity-tutorial-using-azure-portal/filter-input-slices.png)
6. Zamknij bloki, pozostawiając blok **Zestawy danych**. Kliknij zestaw **OutputDataset**. Ten zestaw danych to wyjściowy zestaw danych dla potoku **ADFTutorialPipeline**.
   
    ![blok zestawów danych](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datasets-blade.png)
7. Powinien zostać wyświetlony blok **OutputDataset**, jak pokazano na poniższej ilustracji:
   
    ![blok tabeli](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-table-blade.png) 
8. Zwróć uwagę, że wycinki danych do bieżącego czasu zostały już wygenerowane i są w stanie **Gotowe**. W sekcji **Wycinki z problemami** na dole nie są wyświetlane żadne wycinki.
9. Kliknij przycisk **... (wielokropek)**, aby wyświetlić wszystkie wycinki.
   
    ![blok Wycinki danych](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png)
10. Po kliknięciu dowolnego wycinka danych na liście powinien zostać wyświetlony blok **Wycinek danych**.
    
     ![blok Wycinek danych](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
    
     Jeśli wycinek nie znajduje się w stanie **Gotowe**, możesz wyświetlić wycinki strumienia wychodzącego, które nie są w stanie Gotowe i blokują wykonanie bieżącego wycinka na liście **Niegotowe wycinki strumienia wychodzącego**.
11. W bloku **WYCINEK DANYCH** na liście u dołu powinny zostać wyświetlone wszystkie uruchomienia działania. Kliknij pozycję **uruchomienie działania**, aby wyświetlić blok **Szczegóły uruchamiania działania**. 
    
    ![Szczegóły uruchamiania działania](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)
12. Kliknij przycisk **X**, aby zamknąć wszystkie bloki przed powrotem do bloku głównego fabryki **ADFTutorialDataFactory**.
13. (opcjonalnie) Kliknij przycisk **Potoki** na stronie głównej fabryki **ADFTutorialDataFactory**, kliknij potok **ADFTutorialPipeline** w bloku **Potoki** i przeglądaj tabele wejściowe (**Wykorzystane**) lub wyjściowe (**Utworzone**).
14. Uruchom program **SQL Server Management Studio**, połącz się z usługą Azure SQL Database i upewnij się, że wiersze zostały wstawione do tabeli **emp** w bazie danych.
    
    ![wyniki zapytania sql](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

### <a name="monitor-pipeline-using-monitor-manage-app"></a>Monitorowanie potoku przy użyciu aplikacji Monitorowanie i zarządzanie
Do monitorowania potoków danych możesz też użyć aplikacji Monitorowanie i zarządzanie. Szczegółowe informacje dotyczące korzystania z aplikacji znajdują się w artykule [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md) (Monitorowanie potoków usługi Azure Data Factory oraz zarządzanie nimi za pomocą aplikacji Monitorowanie i zarządzanie).

1. Kliknij kafelek **Monitorowanie i zarządzanie** na stronie głównej fabryki danych.
   
    ![Kafelek Monitorowanie i zarządzanie](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. Powinna zostać wyświetlona **aplikacja Monitorowanie i zarządzanie**. Zmień **godzinę rozpoczęcia** i **godzinę zakończenia** na godzinę rozpoczęcia (2016-07-12) i godzinę zakończenia (2016-07-13) potoku i kliknij przycisk **Zastosuj**. 
   
    ![Aplikacja Monitorowanie i zarządzanie](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png) 
3. Wybierz okno działania z listy **okien działania**, aby zobaczyć szczegółowe informacje o nim. 
    ![Szczegóły okna działania](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)

## <a name="summary"></a>Podsumowanie
W tym samouczku opisano tworzenie fabryki danych Azure w celu kopiowania danych z obiektu blob Azure do bazy danych SQL Azure. Portal Azure został użyty do utworzenia fabryki danych, połączonych usług, zestawów danych oraz potoku. Główne kroki opisane w tym samouczku:  

1. Tworzenie **fabryki danych** Azure.
2. Tworzenie **połączonych usług**:
   1. Połączona usługa **Azure Storage** w celu połączenia z kontem usługi Azure Storage, na którym przechowywane są dane wejściowe.     
   2. Połączona usługa **SQL Azure** w celu połączenia z bazą danych SQL Azure, w której przechowywane są dane wyjściowe. 
3. Tworzenie **zestawów danych** opisujących dane wejściowe i wyjściowe dla potoków.
4. Tworzenie **potoku** za pomocą **działania kopiowania**, w którym źródłem jest element **BlobSource**, a ujściem element **SqlSink**.  

## <a name="see-also"></a>Zobacz też
| Temat | Opis |
|:--- |:--- |
| [Działania przenoszenia danych](data-factory-data-movement-activities.md) |Ten artykuł zawiera szczegółowe informacje dotyczące działania kopiowania używanego w tym samouczku. |
| [Planowanie i wykonywanie](data-factory-scheduling-and-execution.md) |W tym artykule wyjaśniono aspekty planowania i wykonywania modelu aplikacji usługi Fabryka danych Azure. |
| [Potoki](data-factory-create-pipelines.md) |Ten artykuł ułatwia zapoznanie się z potokami i działaniami w usłudze Azure Data Factory. |
| [Zestawy danych](data-factory-create-datasets.md) |Ten artykuł ułatwia zapoznanie się z zestawami danych w usłudze Azure Data Factory. |
| [Monitorowanie potoków i zarządzanie nimi za pomocą aplikacji do monitorowania](data-factory-monitor-manage-app.md) |Ten artykuł zawiera instrukcje dotyczące monitorowania i debugowania potoków oraz zarządzania nimi przy użyciu aplikacji do monitorowania i zarządzania. |




<!--HONumber=Dec16_HO1-->


