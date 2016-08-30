<properties 
    pageTitle="Samouczek: tworzenie potoku za pomocą działania kopiowania przy użyciu edytora fabryki danych" 
    description="Ten samouczek zawiera instrukcje tworzenia potoku usługi Fabryka danych Azure za pomocą działania kopiowania przy użyciu edytora fabryki danych w portalu Azure." 
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
    ms.date="05/16/2016" 
    ms.author="spelluru"/>

# Samouczek: tworzenie potoku za pomocą działania kopiowania przy użyciu edytora fabryki danych
> [AZURE.SELECTOR]
- [Omówienie samouczka](data-factory-get-started.md)
- [Korzystanie z edytora fabryki danych](data-factory-get-started-using-editor.md)
- [Korzystanie z programu Visual Studio](data-factory-get-started-using-vs.md)
- [Korzystanie z programu PowerShell](data-factory-monitor-manage-using-powershell.md)
- [Korzystanie z Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md)


W tym samouczku opisano następujące kroki:

Krok | Opis
-----| -----------
[Tworzenie fabryki danych Azure](#create-data-factory) | W tym kroku opisano tworzenie fabryki danych Azure o nazwie **ADFTutorialDataFactory**.  
[Tworzenie połączonych usług](#create-linked-services) | W tym kroku opisano tworzenie dwóch połączonych usług: **AzureStorageLinkedService** i **AzureSqlLinkedService**. Usługa AzureStorageLinkedService łączy magazyn Azure, a usługa AzureSqlLinkedService — bazę danych Azure SQL z parametrem ADFTutorialDataFactory. Dane wejściowe dla potoku znajdują się w kontenerze obiektów blob w magazynie obiektów blob platformy Azure, a dane wyjściowe będą przechowywane w tabeli w bazie danych SQL Azure. Te dwa magazyny danych są więc dodawane jako połączone usługi do fabryki danych.      
[Tworzenie wejściowych i wyjściowych zestawów danych](#create-datasets) | W poprzednim kroku utworzono połączone usługi, które odwołują się do magazynów danych zawierających dane wejściowe i wyjściowe. W tym kroku opisano definiowanie dwóch tabel fabryki danych — **EmpTableFromBlob** i **EmpSQLTable** — zawierających dane wejściowe i wyjściowe przechowywane w magazynach danych. Dla tabeli EmpTableFromBlob zostanie określony kontener obiektów blob zawierający obiekt blob z danymi źródłowymi, a dla tabeli EmpSQLTable zostanie określona tabela SQL, w której będą przechowywane dane wyjściowe. Zostaną również określone inne właściwości, takie jak struktura danych, ich dostępność itd. 
[Tworzenie potoku](#create-pipeline) | W tym kroku opisano tworzenie potoku o nazwie **ADFTutorialPipeline** w fabryce ADFTutorialDataFactory. Potok będzie obejmował **działanie kopiowania**, które będzie kopiować dane wejściowe z obiektu blob platformy Azure do tabeli wyjściowej SQL Azure. Działanie kopiowania wykonuje operację przenoszenia danych w usłudze Fabryka danych Azure i jest obsługiwane przez dostępną globalnie usługę, która może kopiować dane między różnymi magazynami danych w sposób bezpieczny, niezawodny i skalowalny. Szczegółowe informacje dotyczące działania kopiowania znajdują się w artykule [Data Movement Activities](data-factory-data-movement-activities.md) (Działania przenoszenia danych). 
[Monitorowanie potoku](#monitor-pipeline) | W tym kroku opisano monitorowanie wycinków tabel wejściowych i wyjściowych przy użyciu portalu Azure.

> [AZURE.IMPORTANT] Zapoznaj się z artykułem [Tutorial Overview](data-factory-get-started.md) (Omówienie samouczka) i wykonaj kroki dotyczące wymagań wstępnych przed wykonaniem instrukcji w tym samouczku.

## Tworzenie fabryki danych
W tym kroku opisano tworzenie fabryki danych Azure o nazwie **ADFTutorialDataFactory** przy użyciu portalu Azure.

1.  Po zalogowaniu się w [portalu Azure][azure-portal] kliknij przycisk **NOWE** w lewym dolnym rogu, wybierz opcję **Analiza danych** w bloku **Utwórz** i kliknij opcję **Fabryka danych** w bloku **Analiza danych**. 

    ![Nowy->Fabryka danych][image-data-factory-new-datafactory-menu]    

6. W bloku **Nowa fabryka danych**:
    1. Wprowadź wartość **ADFTutorialDataFactory** dla opcji **nazwa**. 
    
        ![Blok Nowa fabryka danych][image-data-factory-getstarted-new-data-factory-blade]
    2. Kliknij opcję **NAZWA GRUPY ZASOBÓW** i wykonaj następujące czynności:
        1. Kliknij opcję **Utwórz nową grupę zasobów**.
        2. W bloku **Tworzenie grupy zasobów** wprowadź wartość **ADFTutorialResourceGroup** dla opcji **nazwa** grupy zasobów, a następnie kliknij przycisk **OK**. 

            ![Tworzenie grupy zasobów][image-data-factory-create-resource-group]

        W niektórych krokach w tym samouczku zakłada się, że nazwa grupy zasobów to **ADFTutorialResourceGroup**. Informacje na temat grup zasobów znajdują się w artykule [Using resource groups to manage your Azure resources](../resource-group-overview.md) (Używanie grup zasobów do zarządzania zasobami platformy Azure).  
7. Zwróć uwagę, że w bloku **Nowa fabryka danych** została wybrana opcja **Dodaj do tablicy startowej**.
8. Kliknij przycisk **Utwórz** w bloku **Nowa fabryka danych**.

    Nazwa fabryki danych Azure musi być globalnie unikatowa. Jeśli wystąpi błąd **Nazwa fabryki danych „ADFTutorialDataFactory” nie jest dostępna**, zmień nazwę fabryki danych (np. na Twoja_nazwaADFTutorialDataFactory) i spróbuj utworzyć fabrykę ponownie. Artykuł [Data Factory — Naming Rules](data-factory-naming-rules.md) (Fabryka danych — zasady nazewnictwa) zawiera zasady nazewnictwa artefaktów usługi Fabryka danych.  
     
    ![Nazwa fabryki danych nie jest dostępna][image-data-factory-name-not-available]
    
    > [AZURE.NOTE] W przyszłości nazwa fabryki danych może zostać zarejestrowana jako nazwa DNS, a wówczas stanie się widoczna publicznie.  
    > 
    > Aby tworzyć wystąpienia usługi Fabryka danych, musisz być współautorem/administratorem subskrypcji Azure

9. Kliknij centrum **POWIADOMIENIA** po lewej stronie i poszukaj powiadomień z procesu tworzenia. Kliknij przycisk **X**, aby zamknąć blok **POWIADOMIENIA**, jeśli jest otwarty. 
10. Po zakończeniu tworzenia zostanie wyświetlony blok **FABRYKA DANYCH**, jak pokazano poniżej.

    ![Strona główna fabryki danych][image-data-factory-get-stated-factory-home-page]

## Tworzenie połączonych usług
Połączone usługi łączą magazyny danych lub usługi obliczeniowe z fabryką danych Azure. Magazynem danych może być magazyn Azure, baza danych SQL Azure lub lokalna baza danych serwera SQL Server.

W tym kroku opisano tworzenie dwóch połączonych usług: **AzureStorageLinkedService** i **AzureSqlLinkedService**. Połączona usługa AzureStorageLinkedService łączy konto usługi Magazyn Azure, a AzureSqlLinkedService — bazę danych SQL Azure z parametrem **ADFTutorialDataFactory**. W dalszej części tego samouczka opisano tworzenie potoku, który kopiuje dane z kontenera obiektów blob w usłudze AzureStorageLinkedService do tabeli SQL w usłudze AzureSqlLinkedService.

### Tworzenie połączonej usługi dla konta magazynu Azure
1.  W bloku **FABRYKA DANYCH** kliknij kafelek **Utwórz i wdróż**, aby uruchomić **edytor** dla fabryki danych.

    ![Kafelek Utwórz i wdróż][image-author-deploy-tile] 

     
5. W **edytorze** kliknij przycisk **Nowy magazyn danych** na pasku narzędzi i wybierz opcję **Magazyn Azure** z menu rozwijanego. W okienku po prawej stronie powinien zostać wyświetlony szablon JSON do tworzenia połączonej usługi magazynu Azure. 

    ![Przycisk Nowy magazyn danych w edytorze][image-editor-newdatastore-button]
    
6. Zastąp parametry **accountname** i **accountkey** wartościami nazwy konta i klucza konta magazynu Azure. 

    ![Skrypt JSON w edytorze usługi Blob Storage](./media/data-factory-get-started-using-editor/getstarted-editor-blob-storage-json.png)    
    
    Szczegółowe informacje dotyczące właściwości kodu JSON znajdują się w artykule [JSON Scripting Reference](http://go.microsoft.com/fwlink/?LinkId=516971) (Dokumentacja dotycząca skryptów JSON).

6. Kliknij przycisk **Wdróż** na pasku narzędzi, aby wdrożyć usługę AzureStorageLinkedService. Upewnij się, że na pasku tytułu jest wyświetlany komunikat **POŁĄCZONA USŁUGA ZOSTAŁA UTWORZONA POMYŚLNIE**.

    ![Przycisk Wdróż w edytorze usługi Blob Storage][image-editor-blob-storage-deploy]

### Tworzenie połączonej usługi dla bazy danych SQL Azure
1. W **edytorze fabryki danych** kliknij przycisk **Nowy magazyn danych** na pasku narzędzi i wybierz opcję **Baza danych SQL Azure** z menu rozwijanego. W okienku po prawej stronie powinien zostać wyświetlony szablon JSON do tworzenia połączonej usługi serwera SQL Azure.

    ![Edytor ustawień serwera SQL Azure][image-editor-azure-sql-settings]

2. Zastąp parametry **servername**, **databasename**, **username@servername** oraz **password** nazwą serwera SQL Azure, bazy danych, konta użytkownika i hasłem. 
3. Kliknij przycisk **Wdróż** na pasku narzędzi, aby utworzyć i wdrożyć usługę AzureSqlLinkedService. 
   

## Tworzenie zestawów danych
W poprzednim kroku utworzono połączone usługi **AzureStorageLinkedService** i **AzureSqlLinkedService** w celu powiązania konta usługi Azure Storage i bazy danych SQL Azure z fabryką danych **ADFTutorialDataFactory**. W tym kroku opisano definiowanie dwóch tabel fabryki danych — **EmpTableFromBlob** i **EmpSQLTable** — zawierających dane wejściowe i wyjściowe przechowywane w magazynach danych, do których odwołują się odpowiednio usługi AzureStorageLinkedService i AzureSqlLinkedService. Dla tabeli EmpTableFromBlob zostanie określony kontener obiektów blob zawierający obiekt blob z danymi źródłowymi, a dla tabeli EmpSQLTable zostanie określona tabela SQL, w której będą przechowywane dane wyjściowe. 

### Tworzenie wejściowego zestawu danych 
Tabela jest prostokątnym zestawem danych i obejmuje schemat. W tym kroku opisano tworzenie tabeli o nazwie **EmpBlobTable** wskazującej na kontener obiektów blob w usłudze Azure Storage reprezentowany przez połączoną usługę **AzureStorageLinkedService**.

1. W **edytorze** usługi Fabryka danych kliknij przycisk **Nowy zestaw danych** na pasku narzędzi i w menu rozwijanym kliknij opcję **Tabela obiektów blob**. 
2. Zastąp kod JSON w prawym okienku następującym fragmentem kodu JSON: 

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
    
    - parametr **type** zestawu danych został ustawiony na **AzureBlob**.
    - parametr **linkedServiceName** został ustawiony na **AzureStorageLinkedService**. Ta połączona usługa została utworzona w kroku 2.
    - parametr **folderPath** został ustawiony na kontener **adftutorial**. Można również określić nazwę obiektu blob znajdującego się w folderze. Ponieważ nie określasz nazwy obiektu blob, dane z wszystkich obiektów blob w kontenerze są traktowane jako dane wejściowe.  
    - parametr **type** formatu został ustawiony na **TextFormat**
    - W pliku tekstowym znajdują się dwa pola — **FirstName** i **LastName** — oddzielone przecinkiem (**columnDelimiter**) 
    - Parametr **availability** (dostępność) został ustawiony na wartość **hourly** (co godzinę) (parametr **frequency** [częstotliwość] został ustawiony na wartość **hour** [godzina], a parametr **interval** [interwał] został ustawiony na wartość **1**), więc usługa Fabryka danych będzie wyszukiwać dane wejściowe co godzinę w folderze głównym w określonym przez użytkownika kontenerze obiektów blob (**adftutorial**). 
    

    Jeśli nie określisz parametru **fileName** dla **tabeli** **wejściowej**, wszystkie pliki/obiekty blob z folderu danych wejściowych (**folderPath**) będą traktowane jako dane wejściowe. Jeśli określisz parametr fileName w kodzie JSON, tylko określony plik/obiekt blob będzie traktowany jako dane wejściowe.
 
    Jeśli nie określisz parametru **fileName** dla **tabeli wyjściowej**, wygenerowane pliki w ścieżce **folderPath** są nazywane według następującego formatu: Data.&lt;Guid\&gt;.txt (przykład: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    Aby ustawić parametry **folderPath** i **fileName** dynamicznie w oparciu o czas **SliceStart**, użyj właściwości **partitionedBy**. W poniższym przykładzie parametr folderPath używa elementów Year (Rok), Month (Miesiąc) i Day (Dzień) z parametru SliceStart (czas rozpoczęcia przetwarzania wycinka), a parametr fileName używa elementu Hour (Godzina) z parametru SliceStart. Na przykład jeśli wycinek jest generowany dla czasu 2014-10-20T08:00:00, parametr folderName zostaje ustawiony na wikidatagateway/wikisampledataout/2014/10/20, a parametr fileName zostaje ustawiony na wartość 08.csv. 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

    Szczegółowe informacje dotyczące właściwości kodu JSON znajdują się w artykule [JSON Scripting Reference](http://go.microsoft.com/fwlink/?LinkId=516971) (Dokumentacja dotycząca skryptów JSON).

2. Kliknij przycisk **Wdróż** na pasku narzędzi, aby utworzyć i wdrożyć tabelę **EmpTableFromBlob**. Upewnij się, że na pasku tytułu w edytorze wyświetlany jest komunikat **TABELA ZOSTAŁA UTWORZONA POMYŚLNIE**.

### Tworzenie wyjściowego zestawu danych
W tej części kroku opisano tworzenie tabeli wyjściowej o nazwie **EmpSQLTable**, która wskazuje na tabelę SQL w bazie danych SQL Azure reprezentowaną przez połączoną usługę **AzureSqlLinkedService**. 

1. W **edytorze** usługi Fabryka danych kliknij przycisk **Nowy zestaw danych** na pasku narzędzi i w menu rozwijanym kliknij opcję **Tabela SQL Azure**. 
2. Zastąp kod JSON w prawym okienku następującym fragmentem kodu JSON:

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
    * parametr **linkedServiceName** został ustawiony na wartość **AzureSqlLinkedService** (ta połączona usługa została utworzona w kroku 2).
    * parametr **tablename** został ustawiony na wartość **emp**.
    * W tabeli emp w bazie danych istnieją trzy kolumny – **ID**, **FirstName** i **LastName**, ale kolumna ID oznacza tożsamość, więc wystarczy określić tylko parametry **FirstName** i **LastName**.
    * Parametr **availability** (dostępność) został ustawiony na wartość **hourly** (co godzinę) (parametr **frequency** [częstotliwość] został ustawiony na **hour** [godzinę], a **interval** [interwał] został ustawiony na wartość **1**).  Usługa Fabryka danych będzie co godzinę generować wycinek danych wyjściowych w tabeli **emp** w bazie danych SQL Azure.


3. Kliknij przycisk **Wdróż** na pasku narzędzi, aby utworzyć i wdrożyć tabelę **EmpSQLTable**.


## Tworzenie potoku
W tym kroku opisano tworzenie potoku za pomocą **działania kopiowania**, w którym parametr **EmpTableFromBlob** jest używany jako dane wejściowe, a parametr **EmpSQLTable** jako dane wyjściowe.

1. W **edytorze** usługi Fabryka danych kliknij przycisk **Nowy potok** na pasku narzędzi. Kliknij przycisk **... (wielokropek)** na pasku narzędzi, jeśli nie widać przycisku. Możesz również kliknąć prawym przyciskiem myszy opcję **Potoki** w widoku drzewa i kliknąć opcję **Nowy potok**.

    ![Przycisk Nowy potok w edytorze][image-editor-newpipeline-button]
 
2. Zastąp kod JSON w prawym okienku następującym fragmentem kodu JSON: 
        
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
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z"
          }
        } 

    Pamiętaj o następujących kwestiach:

    - W sekcji działań jest tylko jedno działanie, którego parametr **type** (typ) został ustawiony na wartość **CopyActivity**.
    - Dane wejściowe dla działania zostały ustawione na wartość **EmpTableFromBlob**, a dane wyjściowe dla działania zostały ustawione na wartość **EmpSQLTable**.
    - W sekcji **przekształcania** parametr **BlobSource** został określony jako typ źródłowy, a parametr **SqlSink** został określony jako typ ujścia.

    Zastąp wartość właściwości **start** datą bieżącą, a wartość **end** datą jutrzejszą. Możesz określić tylko część daty i pominąć część godziny parametru data/godzina. Na przykład „2015-02-03” jest odpowiednikiem „2015-02-03T00:00:00Z”
    
    Zarówno data/godzina rozpoczęcia, jak i data/godzina zakończenia muszą być w [formacie ISO](http://en.wikipedia.org/wiki/ISO_8601). Na przykład: 2014-10-14T16:32:41Z. Godzina **zakończenia** jest opcjonalna, ale w tym samouczku zostanie użyta. 
    
    Jeśli nie określisz wartości właściwości **end**, zostanie ona obliczona zgodnie z wzorem „**start + 48 godzin**”. Aby uruchomić potok bezterminowo, określ **9999-09-09** jako wartość właściwości **end**.
    
    W powyższym przykładzie będą 24 wycinki danych, gdyż poszczególne wycinki są generowane co godzinę.
    
    Szczegółowe informacje dotyczące właściwości kodu JSON znajdują się w artykule [JSON Scripting Reference](http://go.microsoft.com/fwlink/?LinkId=516971) (Dokumentacja dotycząca skryptów JSON).

4. Kliknij przycisk **Wdróż** na pasku narzędzi, aby utworzyć i wdrożyć element **ADFTutorialPipeline**. Upewnij się, że został wyświetlony komunikat **POTOK UTWORZONY POMYŚLNIE**.
5. Teraz zamknij blok **Edytor**, klikając przycisk **X**. Kliknij przycisk **X** ponownie, aby zamknąć blok ADFTutorialDataFactory z paskiem narzędzi i widokiem drzewa. Jeśli zostanie wyświetlony komunikat **Niezapisane zmiany zostaną odrzucone**, kliknij przycisk **OK**.
6. Powinien nastąpić powrót do bloku **FABRYKA DANYCH** dla elementu **ADFTutorialDataFactory**.

**Gratulacje!** Pomyślnie utworzono fabrykę danych Azure, połączone usługi, tabele i potoki oraz zaplanowano potok.   
 
### Wyświetlanie fabryki danych w widoku diagramu 
1. W bloku **FABRYKA DANYCH** kliknij opcję **Diagram**.

    ![Blok Fabryka danych — kafelek Diagram][image-datafactoryblade-diagramtile]

2. Powinien zostać wyświetlony diagram podobny do poniższego: 

    ![Widok diagramu][image-data-factory-get-started-diagram-blade]

    Możesz powiększyć, pomniejszyć, powiększyć do 100%, powiększyć do dopasowania, automatycznie rozmieścić potoki i tabele oraz wyświetlić informacje dotyczące elementów powiązanych (powoduje wyróżnienie elementów nadrzędnych i podrzędnych wybranych elementów).  Możesz kliknąć dwukrotnie obiekt (tabelę wejściową/wyjściową lub potok), by wyświetlić jego właściwości. 
3. Kliknij prawym przyciskiem myszy potok **ADFTutorialPipeline** w widoku diagramu i kliknij przycisk **Otwórz potok**. Powinny zostać wyświetlone działania w potoku razem ze swoimi wejściowymi i wyjściowymi zestawami danych. W tym samouczku w potoku znajduje się tylko jedno działanie (działanie kopiowania) obejmujące obiekt EmpTableBlob jako wejściowy zestaw danych i tabelę EmpSQLTable jako wyjściowy zestaw danych.   

    ![Otwieranie potoku](./media/data-factory-get-started-using-editor/DiagramView-OpenPipeline.png)

4. Kliknij przycisk **Fabryka danych** w obszarze nawigacji w lewym górnym rogu, aby wrócić do widoku diagramu. W widoku diagramu są wyświetlane wszystkie potoki. W tym przykładzie utworzono tylko jeden potok.   
 

## Monitorowanie potoku
W tym kroku opisano użycie portalu Azure do monitorowania tego, co dzieje się w fabryce danych Azure. Do monitorowania zestawów danych i potoków można również używać poleceń cmdlet programu PowerShell. Szczegółowe informacje dotyczące korzystania z poleceń cmdlet w celu monitorowania znajdują się w artykule [Monitor and Manage Data Factory using PowerShell Cmdlets] (Monitorowanie usługi Fabryka danych oraz zarządzanie nią przy użyciu poleceń cmdlet programu PowerShell)[monitor-manage-using-powershell].

1. Przejdź do [portalu Azure (w wersji zapoznawczej)][azure-portal], jeśli jeszcze nie został on otwarty. 
2. Jeśli blok dla parametru **ADFTutorialDataFactory** nie jest otwarty, otwórz go, klikając przycisk **ADFTutorialDataFactory** na **tablicy startowej**. 
3. Powinna zostać wyświetlona liczba tabel i potoków utworzonych w tym bloku oraz ich nazwy.

    ![strona główna z nazwami][image-data-factory-get-started-home-page-pipeline-tables]

4. Kliknij kafelek **Zestawy danych**.
5. W bloku **Zestawy danych** kliknij tabelę **EmpTableFromBlob**. Jest to tabela wejściowa potoku **ADFTutorialPipeline**.

    ![Wybrane zestawy danych z tabelą EmpTableFromBlob][image-data-factory-get-started-datasets-emptable-selected]   
5. Zwróć uwagę, że wycinki danych do bieżącego czasu zostały już wygenerowane i są w stanie **Gotowe**, ponieważ plik **emp.txt** znajduje się cały czas w kontenerze obiektów blob **adftutorial\input**. Upewnij się, że żadne wycinki nie są wyświetlane w sekcji **Ostatnie wycinki z błędami** na dole.

    Obie listy — **Ostatnio zaktualizowane wycinki** i **Ostatnie wycinki z błędami** — są posortowane według parametru **CZAS OSTATNIEJ AKTUALIZACJI**. Czas aktualizacji wycinka zostaje zmieniony w następujących sytuacjach. 
    

    -  Musisz zaktualizować stan wycinka ręcznie, na przykład przy użyciu polecenia **Set-AzureRmDataFactorySliceStatus** lub klikając polecenie **URUCHOM** w bloku **WYCINEK** danego wycinka.
    -  Wycinek zmienia stan w zależności od wykonywanych operacji (np. uruchamianie rozpoczęło się, uruchamianie zakończyło się i nie powiodło się, uruchamianie zakończyło się pomyślnie itp.).
 
    Kliknij tytuł list lub **... (wielokropek)**, aby wyświetlić większą listę wycinków. Kliknij przycisk **Filtruj** na pasku narzędzi, aby filtrować wycinki.  
    
    Aby zamiast tego wyświetlić wycinki danych według godzin rozpoczęcia/zakończenia, kliknij kafelek **Wycinki danych (według godziny wycinka)**.   

    ![Wycinki danych według godziny wycinka][DataSlicesBySliceTime]   

6. W bloku **Zestawy danych** kliknij tabelę **EmpSQLTable**. Jest to tabela wyjściowa potoku **ADFTutorialPipeline**.

    ![blok zestawów danych][image-data-factory-get-started-datasets-blade]



     
6. Powinien zostać wyświetlony blok tabeli **EmpSQLTable**, jak pokazano poniżej:

    ![blok tabeli][image-data-factory-get-started-table-blade]
 
7. Zwróć uwagę, że wycinki danych do bieżącego czasu zostały już wygenerowane i są w stanie **Gotowe**. W sekcji **Wycinki z problemami** na dole nie są wyświetlane żadne wycinki.
8. Kliknij przycisk **... (wielokropek)**, aby wyświetlić wszystkie wycinki.

    ![blok Wycinki danych][image-data-factory-get-started-dataslices-blade]

9. Po kliknięciu dowolnego wycinka danych na liście powinien zostać wyświetlony blok **WYCINEK DANYCH**.

    ![blok Wycinek danych][image-data-factory-get-started-dataslice-blade]
  
    Jeśli wycinek nie znajduje się w stanie **Gotowe**, możesz wyświetlić wycinki strumienia wychodzącego, które nie są w stanie Gotowe i blokują wykonanie bieżącego wycinka na liście **Niegotowe wycinki strumienia wychodzącego**. 

11. W bloku **WYCINEK DANYCH** na liście u dołu powinny zostać wyświetlone wszystkie uruchomienia działania. Kliknij **uruchomienie działania**, aby wyświetlić blok **SZCZEGÓŁY URUCHAMIANIA DZIAŁANIA**. 

    ![Szczegóły uruchamiania działania][image-data-factory-get-started-activity-run-details]

    
12. Kliknij przycisk **X**, aby zamknąć wszystkie bloki przed powrotem do bloku głównego fabryki **ADFTutorialDataFactory**.
14. (opcjonalnie) Kliknij przycisk **Potoki** na stronie głównej fabryki **ADFTutorialDataFactory**, kliknij potok **ADFTutorialPipeline** w bloku **Potoki** i przeglądaj tabele wejściowe (**Wykorzystane**) lub wyjściowe (**Utworzone**).
15. Uruchom program **SQL Server Management Studio**, połącz się z usługą Azure SQL Database i upewnij się, że wiersze zostały wstawione do tabeli **emp** w bazie danych.

    ![wyniki zapytania sql][image-data-factory-get-started-sql-query-results]


## Podsumowanie 
W tym samouczku opisano tworzenie fabryki danych Azure w celu kopiowania danych z obiektu blob platformy Azure do bazy danych SQL Azure. Portal Azure został użyty do utworzenia fabryki danych, połączonych usług, zestawów danych oraz potoku. Główne kroki opisane w tym samouczku:  

1.  Tworzenie **fabryki danych** Azure.
2.  Tworzenie **połączonych usług**:
    1. Połączona usługa **Azure Storage** w celu połączenia z kontem usługi Azure Storage, na którym przechowywane są dane wejściowe.    
    2. Połączona usługa **SQL Azure** w celu połączenia z bazą danych SQL Azure, w której przechowywane są dane wyjściowe. 
3.  Tworzenie **zestawów danych** opisujących dane wejściowe i wyjściowe dla potoków.
4.  Tworzenie **potoku** za pomocą **działania kopiowania**, gdy źródłem jest parametr **BlobSource**, a ujściem parametr **SqlSink**.  


## Zobacz też
| Temat | Opis |
| :---- | :---- |
| [Działania przenoszenia danych](data-factory-data-movement-activities.md) | Ten artykuł zawiera szczegółowe informacje dotyczące działania kopiowania używanego w tym samouczku. |
| [Planowanie i wykonywanie](data-factory-scheduling-and-execution.md) | W tym artykule wyjaśniono aspekty planowania i wykonywania modelu aplikacji usługi Fabryka danych Azure. |
| [Potoki](data-factory-create-pipelines.md) | Ten artykuł ułatwi zapoznanie się z potokami i działaniami w usłudze Fabryka danych Azure oraz ze sposobem ich użycia do konstruowania przepływów pracy typu end-to-end opartych na danych na potrzeby scenariusza lub firmy. |
| [Zestawy danych](data-factory-create-datasets.md) | Ten artykuł ułatwi zapoznanie się z zestawami danych w usłudze Fabryka danych Azure.
| [Monitorowanie potoków i zarządzanie nimi za pomocą aplikacji do monitorowania](data-factory-monitor-manage-app.md) | Ten artykuł zawiera instrukcje dotyczące monitorowania i debugowania potoków oraz zarządzania nimi przy użyciu aplikacji do monitorowania i zarządzania. 

<!--Link references-->
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[msdn-activities]: https://msdn.microsoft.com/library/dn834988.aspx
[msdn-linkedservices]: https://msdn.microsoft.com/library/dn834986.aspx
[data-factory-naming-rules]: https://msdn.microsoft.com/library/azure/dn835027.aspx

[azure-portal]: https://portal.azure.com/
[download-azure-powershell]: http://azure.microsoft.com/documentation/articles/install-configure-powershell
[sql-management-studio]: http://azure.microsoft.com/documentation/articles/sql-database-manage-azure-ssms/#Step2
[sql-cmd-exe]: https://msdn.microsoft.com/library/azure/ee336280.aspx

[monitor-manage-using-powershell]: data-factory-monitor-manage-using-powershell.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[data-factory-create-storage]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account



[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

<!--Image references-->

[DataSlicesBySliceTime]: ./media/data-factory-get-started-using-editor/DataSlicesBySliceTime.png

[image-data-factory-getstarted-new-data-factory-blade]: ./media/data-factory-get-started-using-editor/getstarted-new-data-factory.png

[image-data-factory-get-stated-factory-home-page]: ./media/data-factory-get-started-using-editor/getstarted-data-factory-home-page.png

[image-author-deploy-tile]: ./media/data-factory-get-started-using-editor/getstarted-author-deploy-tile.png

[image-editor-newdatastore-button]: ./media/data-factory-get-started-using-editor/getstarted-editor-newdatastore-button.png

[image-editor-blob-storage-deploy]: ./media/data-factory-get-started-using-editor/getstarted-editor-blob-storage-deploy.png

[image-editor-azure-sql-settings]: ./media/data-factory-get-started-using-editor/getstarted-editor-azure-sql-settings.png

[image-editor-newpipeline-button]: ./media/data-factory-get-started-using-editor/getstarted-editor-newpipeline-button.png

[image-datafactoryblade-diagramtile]: ./media/data-factory-get-started-using-editor/getstarted-datafactoryblade-diagramtile.png


[image-data-factory-get-started-diagram-blade]: ./media/data-factory-get-started-using-editor/getstarted-diagram-blade.png

[image-data-factory-get-started-home-page-pipeline-tables]: ./media/data-factory-get-started-using-editor/getstarted-datafactory-home-page-pipeline-tables.png

[image-data-factory-get-started-datasets-blade]: ./media/data-factory-get-started-using-editor/getstarted-datasets-blade.png

[image-data-factory-get-started-table-blade]: ./media/data-factory-get-started-using-editor/getstarted-table-blade.png

[image-data-factory-get-started-dataslices-blade]: ./media/data-factory-get-started-using-editor/getstarted-dataslices-blade.png

[image-data-factory-get-started-dataslice-blade]: ./media/data-factory-get-started-using-editor/getstarted-dataslice-blade.png

[image-data-factory-get-started-sql-query-results]: ./media/data-factory-get-started-using-editor/getstarted-sql-query-results.png

[image-data-factory-get-started-datasets-emptable-selected]: ./media/data-factory-get-started-using-editor/DataSetsWithEmpTableFromBlobSelected.png

[image-data-factory-get-started-activity-run-details]: ./media/data-factory-get-started-using-editor/ActivityRunDetails.png

[image-data-factory-create-resource-group]: ./media/data-factory-get-started-using-editor/CreateNewResourceGroup.png


[image-data-factory-new-datafactory-menu]: ./media/data-factory-get-started-using-editor/NewDataFactoryMenu.png


[image-data-factory-name-not-available]: ./media/data-factory-get-started-using-editor/getstarted-data-factory-not-available.png
 


<!--HONumber=jun16_HO2-->


