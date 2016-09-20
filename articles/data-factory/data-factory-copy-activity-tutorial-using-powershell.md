<properties 
    pageTitle="Samouczek: tworzenie potoku za pomocą działania kopiowania przy użyciu programu Azure PowerShell | Microsoft Azure" 
    description="Ten samouczek zawiera instrukcje tworzenia potoku usługi Azure Data Factory za pomocą działania kopiowania przy użyciu programu Azure PowerShell." 
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

# Samouczek: tworzenie potoku za pomocą działania kopiowania przy użyciu programu Azure PowerShell
> [AZURE.SELECTOR]
- [Omówienie samouczka](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Korzystanie z Edytora fabryki danych](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Korzystanie z programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Korzystanie z programu Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Korzystanie z interfejsu API REST](data-factory-copy-activity-tutorial-using-rest-api.md)
- [Korzystanie z interfejsu API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Korzystanie z Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md)

W samouczku [Kopiowanie danych z usługi Blob Storage do usługi SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) przedstawiono sposób tworzenia i monitorowania fabryki danych platformy Azure przy użyciu witryny [Azure Portal][azure-portal]. Ten samouczek zawiera instrukcje tworzenia i monitorowania fabryki danych Azure przy użyciu poleceń cmdlet programu Azure PowerShell. Potok w fabryce danych tworzonej w tym samouczku używa działania kopiowania w celu kopiowania danych z obiektu blob platformy Azure do bazy danych SQL Azure.

Działanie kopiowania wykonuje operację przenoszenia danych w usłudze Azure Data Factory. Działanie jest obsługiwane przez globalnie dostępną usługę, która może kopiować dane między różnymi magazynami danych w sposób bezpieczny, niezawodny i skalowalny. Szczegółowe informacje dotyczące działania kopiowania znajdują się w artykule [Data Movement Activities](data-factory-data-movement-activities.md) (Działania przenoszenia danych).   

> [AZURE.IMPORTANT] 
> Zapoznaj się z artykułem [Omówienie samouczka](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) i wykonaj przedstawione tam działania dotyczące wymagań wstępnych przed wykonaniem instrukcji z tego samouczka.
>   
> Ten artykuł nie obejmuje wszystkich poleceń cmdlet dla usługi Fabryka danych. Pełna dokumentacja dotycząca poleceń cmdlet dla usługi Fabryka danych znajduje się w artykule [Data Factory Cmdlet Reference](https://msdn.microsoft.com/library/dn820234.aspx) (Dokumentacja dotycząca poleceń cmdlet dla usługi Fabryka danych).
  

##Wymagania wstępne
Oprócz wymagań wstępnych wymienionych w artykule Omówienie samouczka wymagane jest:

- Zainstalowanie programu **Azure PowerShell**. Postępuj zgodnie z instrukcjami w artykule [How to install and configure Azure PowerShell](../powershell-install-configure.md) (Instalowanie i konfigurowanie programu Azure PowerShell), aby zainstalować program Azure PowerShell na komputerze.

##Informacje o tym samouczku
W poniższej tabeli wymieniono kroki, które należy wykonać w ramach samouczka oraz ich opisy. 

Krok | Opis
-----| -----------
[Tworzenie fabryki danych Azure](#create-data-factory) | W tym kroku opisano tworzenie fabryki danych Azure o nazwie **ADFTutorialDataFactory**. 
[Tworzenie połączonych usług](#create-linked-services) | W tym kroku opisano tworzenie dwóch połączonych usług: **StorageLinkedService** i **AzureSqlLinkedService**. Usługa StorageLinkedService łączy z fabryką danych ADFTutorialDataFactoryPSH magazyn Azure, a usługa AzureSqlLinkedService — bazę danych SQL Azure.
[Tworzenie wejściowych i wyjściowych zestawów danych](#create-datasets) | W tym kroku przedstawiono definiowanie dwóch zestawów danych (**EmpTableFromBlob** i **EmpSQLTable**). Te zestawy danych zostaną użyte jako tabele wejściowe i wyjściowe dla **działania kopiowania** w potoku ADFTutorialPipeline, który zostanie utworzony w następnym kroku.
[Tworzenie i uruchamianie potoku](#create-pipeline) | W tym kroku opisano tworzenie potoku o nazwie **ADFTutorialPipeline** w fabryce danych **ADFTutorialDataFactoryPSH**. Potok obejmuje **działanie kopiowania**, które kopiuje dane z obiektu blob Azure do tabeli wyjściowej bazy danych Azure.
[Monitorowanie zestawów danych i potoku](#monitor-pipeline) | W tym kroku opisano monitorowanie zestawów danych oraz potoku przy użyciu programu Azure PowerShell.

## Tworzenie fabryki danych
W tym kroku opisano użycie programu Azure PowerShell do utworzenia fabryki danych Azure o nazwie **ADFTutorialDataFactoryPSH**.

1. Uruchom program **PowerShell** i uruchom następujące polecenie. Nie zamykaj programu Azure PowerShell, zanim nie wykonasz wszystkich instrukcji z tego samouczka. Jeśli go zamkniesz i otworzysz ponownie, musisz uruchomić te polecenia jeszcze raz.
    1. Uruchom poniższe polecenie i wprowadź nazwę użytkownika oraz hasło, których używasz do logowania się w witrynie Azure Portal.
    
            Login-AzureRmAccount   
    2. Uruchom poniższe polecenie, aby wyświetlić wszystkie subskrypcje dla tego konta.

            Get-AzureRmSubscription 
    3. Uruchom poniższe polecenie, aby wybrać subskrypcję, z którą chcesz pracować. Zastąp ciąg **&lt;NameOfAzureSubscription**&gt; nazwą subskrypcji platformy Azure. 

            Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

3. Utwórz grupę zasobów Azure o nazwie **ADFTutorialResourceGroup** przez uruchomienie następującego polecenia.
   
        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    W niektórych krokach w tym samouczku zakłada się, że używana jest grupa zasobów o nazwie **ADFTutorialResourceGroup**. Jeśli używasz innej grupy zasobów, podczas wykonywania instrukcji w tym samouczku trzeba będzie wstawić jej nazwę zamiast nazwy ADFTutorialResourceGroup. 
4. Uruchom polecenie cmdlet **New-AzureRmDataFactory**, aby utworzyć fabrykę danych o nazwie **ADFTutorialDataFactoryPSH**.  

        New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"

    
Pamiętaj o następujących kwestiach:
 
- Nazwa fabryki danych Azure musi być globalnie unikatowa. Jeśli wystąpi poniższy błąd, zmień nazwę (np. twojanazwaADFTutorialDataFactoryPSH). Użyj tej nazwy zamiast ADFTutorialFactoryPSH podczas wykonywania kroków w tym samouczku. Artykuł [Data Factory — Naming Rules](data-factory-naming-rules.md) (Fabryka danych — zasady nazewnictwa) zawiera zasady nazewnictwa artefaktów usługi Fabryka danych.
    
        Data factory name “ADFTutorialDataFactoryPSH” is not available
- Aby tworzyć wystąpienia usługi Fabryka danych, musisz być współautorem/administratorem subskrypcji Azure
- W przyszłości nazwa fabryki danych może zostać zarejestrowana jako nazwa DNS, a wówczas stanie się widoczna publicznie.
- Jeśli zostanie wyświetlony komunikat o błędzie: „**Subskrypcja nie jest zarejestrowana w celu używania przestrzeni nazw Microsoft.DataFactory**”, wykonaj jedną z następujących czynności i spróbuj opublikować ponownie: 

    - W programie Azure PowerShell uruchom następujące polecenie, aby zarejestrować dostawcę usługi Fabryka danych. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Można uruchomić następujące polecenie, aby potwierdzić, że dostawca usługi Data Factory jest zarejestrowany. 
    
            Get-AzureRmResourceProvider
    - Zaloguj się przy użyciu subskrypcji Azure do [portalu Azure](https://portal.azure.com) i przejdź do bloku Fabryka danych lub utwórz fabrykę danych w portalu Azure. Ta akcja powoduje automatyczne zarejestrowanie dostawcy.

## Tworzenie połączonych usług
Połączone usługi łączą magazyny danych lub usługi obliczeniowe z fabryką danych Azure. Magazynem danych może być usługa Azure Storage, usługa Azure SQL Database lub lokalna baza danych SQL Server, która zawiera dane wejściowe lub przechowuje dane wyjściowe dla potoku usługi Data Factory. Usługą obliczeniową jest usługa, która przetwarza dane wejściowe i generuje dane wyjściowe. 

W tym kroku opisano tworzenie dwóch połączonych usług: **StorageLinkedService** i **AzureSqlLinkedService**. Połączona usługa StorageLinkedService łączy z fabryką danych **ADFTutorialDataFactoryPSH** konto usługi Azure Storage, a usługa AzureSqlLinkedService — bazę danych SQL Azure. W dalszej części tego samouczka opisano tworzenie potoku, który kopiuje dane z kontenera obiektów blob w usłudze StorageLinkedService do tabeli SQL w usłudze AzureSqlLinkedService.

### Tworzenie połączonej usługi dla konta magazynu Azure
1.  W folderze **C:\ADFGetStartedPSH** utwórz plik JSON o nazwie **StorageLinkedService.json** o następującej zawartości. Utwórz folder ADFGetStartedPSH, jeśli jeszcze nie istnieje.

            {
                "name": "StorageLinkedService",
                "properties": {
                    "type": "AzureStorage",
                    "typeProperties": {
                        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                    }
                }
            }

    Zastąp wartości **accountname** i **accountkey** nazwą konta usługi Azure Storage oraz jego kluczem.
2.  W programie **Azure PowerShell** przejdź do folderu **ADFGetStartedPSH**. 
3.  Możesz użyć polecenia cmdlet **New-AzureRmDataFactoryLinkedService**, aby utworzyć połączoną usługę. To polecenie cmdlet i inne polecenia cmdlet usługi Data Factory używane w tym samouczku wymagają przekazania wartości dla parametrów **ResourceGroupName** i **DataFactoryName**. Możesz też użyć polecenia **Get-AzureRmDataFactory**, aby pobrać obiekt DataFactory i przekazać obiekt bez wpisywania parametrów ResourceGroupName i DataFactoryName za każdym razem, gdy uruchamiasz polecenie cmdlet. Uruchom następujące polecenie, aby przypisać dane wyjściowe polecenia cmdlet **Get-AzureRmDataFactory** do zmiennej **$df**. 

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH

4.  Teraz uruchom polecenie cmdlet **New-AzureRmDataFactoryLinkedService**, aby utworzyć połączoną usługę **StorageLinkedService**. 

        New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json

    Jeśli polecenie cmdlet **Get-AzureRmDataFactory** nie zostało uruchomione i nie przypisano danych wyjściowych do zmiennej **$df**, trzeba określić wartości dla parametrów ResourceGroupName i DataFactoryName w następujący sposób.   
        
        New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName ADFTutorialDataFactoryPSH -File .\StorageLinkedService.json

    Jeśli zamkniesz program Azure PowerShell w trakcie wykonywania samouczka, w celu dokończenia samouczka trzeba uruchomić polecenie cmdlet Get-AzureRmDataFactory po następnym uruchomieniu programu Azure PowerShell.

### Tworzenie połączonej usługi dla bazy danych SQL Azure
1.  Utwórz plik JSON o nazwie AzureSqlLinkedService.json o następującej zawartości.

            {
                "name": "AzureSqlLinkedService",
                "properties": {
                    "type": "AzureSqlDatabase",
                    "typeProperties": {
                        "connectionString": "Server=tcp:<server>.database.windows.net,1433;Database=<databasename>;User ID=user@server;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                    }
                }
            }

    Zastąp wartości **servername**, **databasename**, **username@servername** oraz **password** nazwą serwera SQL Azure, nazwą bazy danych, kontem użytkownika i hasłem.

2.  Uruchom poniższe polecenie, aby utworzyć połączoną usługę. 
    
        New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json

    Upewnij się, że ustawienie **Zezwalaj na dostęp do usług Azure** dla serwera SQL Azure jest WŁĄCZONE. W celu sprawdzenia i włączenia wykonaj następujące czynności:

    1. Kliknij centrum **PRZEGLĄDAJ** po lewej stronie i kliknij przycisk **Serwery SQL**.
    2. Wybierz serwer i kliknij pozycję **USTAWIENIA** w bloku SERWER SQL.
    3. W bloku **USTAWIENIA** kliknij opcję **Zapora**.
    4. W bloku **Ustawienia zapory** kliknij pozycję **WŁĄCZ** dla ustawienia **Zezwalaj na dostęp do usług Azure**.
    5. Kliknij centrum **AKTYWNE** po lewej stronie, aby przejść do wcześniej używanego bloku **Fabryka danych**.
    

## Tworzenie zestawów danych

W poprzednim kroku utworzono połączone usługi **StorageLinkedService** i **AzureSqlLinkedService** w celu powiązania konta usługi Azure Storage i bazy danych SQL Azure z fabryką danych **ADFTutorialDataFactoryPSH**. W tym kroku przedstawiono tworzenie zestawów danych reprezentujących dane wejściowe i wyjściowe dla działania kopiowania w potoku, który zostanie utworzony w następnym kroku. 

Tabela jest prostokątnym zestawem danych i jest jedynym obsługiwanym obecnie typem zestawu danych. Tabela wejściowa w tym samouczku odnosi się do kontenera obiektu blob w magazynie Azure, na który wskazuje element StorageLinkedService. Tabela danych wyjściowych odwołuje się do tabeli SQL w bazie danych Azure SQL Database, na którą wskazuje element AzureSqlLinkedService.  

### Przygotowanie magazynu obiektów blob Azure i bazy danych SQL Azure na potrzeby samouczka
Jeśli wykonano już instrukcje z samouczka w artykule [Kopiowanie danych z usługi Blob Storage](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md), pomiń ten krok. 

Aby przygotować magazyn obiektów blob Azure i bazę danych SQL Azure, wykonaj następujące kroki. 
 
* Utwórz kontener obiektów blob o nazwie **adftutorial** w magazynie obiektów blob Azure, który wskazuje usługa **StorageLinkedService**. 
* Utwórz i przekaż plik tekstowy **emp.txt** jako obiekt blob do kontenera **adftutorial**. 
* Utwórz tabelę o nazwie **emp** w bazie danych SQL Azure, którą wskazuje usługa **AzureSqlLinkedService**.


1. Uruchom program Notatnik, wklej następujący tekst i zapisz go jako plik **emp.txt** w folderze **C:\ADFGetStartedPSH** na dysku twardym. 

        John, Doe
        Jane, Doe
                
2. Użyj narzędzi takich jak [Eksplorator magazynu Azure](https://azurestorageexplorer.codeplex.com/) do utworzenia kontenera **adftutorial** i przekazania pliku **emp.txt** do kontenera.

    ![Eksplorator magazynu Azure](media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png)
3. Poniższy skrypt SQL umożliwia utworzenie tabeli **emp** w bazie danych SQL Azure.  


        CREATE TABLE dbo.emp 
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID); 

    Jeśli masz zainstalowany na komputerze program SQL Server 2014, wykonaj instrukcje z sekcji [Step 2: Connect to SQL Database of the Managing Azure SQL Database using SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md) (Krok 2: Łączenie z bazą danych SQL w artykule Zarządzanie bazą danych Azure SQL Database przy użyciu programu SQL Server Management Studio), aby połączyć się z serwerem SQL Azure i uruchomić skrypt SQL.

    Jeśli klient nie ma dostępu do serwera SQL Azure, musisz skonfigurować zaporę serwera SQL Azure tak, aby dostęp z Twojego komputera (adresu IP) był dozwolony. W [tym artykule](../sql-database/sql-database-configure-firewall-settings.md) opisano kroki konfigurowania zapory dla serwera SQL Azure.
        
### Tworzenie wejściowego zestawu danych 
Tabela jest prostokątnym zestawem danych i obejmuje schemat. W tym kroku opisano tworzenie tabeli o nazwie **EmpBlobTable** wskazującej kontener obiektów blob w usłudze Azure Storage reprezentowany przez połączoną usługę **StorageLinkedService**. Ten kontener obiektów blob (**adftutorial**) zawiera dane wejściowe w pliku **emp.txt**. 

1.  Utwórz plik JSON o nazwie **EmpBlobTable.json** w folderze **C:\ADFGetStartedPSH** o następującej zawartości:

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
                "linkedServiceName": "StorageLinkedService",
                "typeProperties": {
                  "fileName": "emp.txt",
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
    
    Pamiętaj o następujących kwestiach: 
    
    - Parametr **type** zestawu danych został ustawiony na **AzureBlob**.
    - Parametr **linkedServiceName** został ustawiony na **StorageLinkedService**. 
    - Parametr **folderPath** został ustawiony na kontener **adftutorial**. 
    - Parametr **fileName** został ustawiony na **emp.txt**. Jeśli nie określisz nazwy obiektu blob, dane ze wszystkich obiektów blob w kontenerze będą traktowane jak dane wejściowe.  
    - Parametr **type** formatu został ustawiony na **TextFormat**
    - W pliku tekstowym znajdują się dwa pola — **FirstName** i **LastName** — oddzielone przecinkiem (**columnDelimiter**) 
    - Parametr **availability** został ustawiony na wartość **hourly** (parametr **frequency** ma wartość **hour**, a **interval** — **1**). W związku z tym usługa Data Factory szuka danych wejściowych co godzinę w folderze głównym w określonym kontenerze obiektów blob (**adftutorial**).

    Jeśli nie określisz parametru **fileName** dla **tabeli** **wejściowej**, wszystkie pliki/obiekty blob z folderu danych wejściowych (**folderPath**) będą traktowane jako dane wejściowe. Jeśli określisz parametr fileName w kodzie JSON, tylko określony plik/obiekt blob będzie traktowany jako dane wejściowe. 
 
    Jeśli nie określisz parametru **fileName** dla **tabeli wyjściowej**, wygenerowane pliki w ścieżce **folderPath** otrzymają nazwy w następującym formacie: Data.<Guid\>.txt (np. Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt).

    Aby ustawić parametry **folderPath** i **fileName** dynamicznie w oparciu o czas **SliceStart**, użyj właściwości **partitionedBy**. W poniższym przykładzie parametr folderPath używa elementów Year, Month i Day z parametru SliceStart (czas rozpoczęcia przetwarzania wycinka), a parametr fileName używa elementu Hour z parametru SliceStart. Na przykład jeśli wycinek jest generowany dla czasu 2014-10-20T08:00:00, parametr folderName zostaje ustawiony na wikidatagateway/wikisampledataout/2014/10/20, a parametr fileName zostaje ustawiony na wartość 08.csv. 

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

2.  Uruchom następujące polecenie, aby utworzyć zestaw danych usługi Fabryka danych.

        New-AzureRmDataFactoryDataset $df -File .\EmpBlobTable.json

### Tworzenie wyjściowego zestawu danych
W tym kroku tworzony jest wyjściowy zestaw danych o nazwie **EmpSQLTable**. Ten zestaw danych wskazuje tabelę SQL (**emp**) w bazie danych Azure SQL Database reprezentowanej przez usługę **AzureSqlLinkedService**. Potok kopiuje dane z wejściowego obiektu blob do tabeli **emp**. 

1.  Utwórz plik JSON o nazwie **EmpSQLTable.json** w folderze **C:\ADFGetStartedPSH** o następującej zawartości.
        
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
    
    * Parametr **type** zestawu danych został ustawiony na wartość **AzureSqlTable**.
    * Parametr **linkedServiceName** został ustawiony na wartość **AzureSqlLinkedService**.
    * Parametr **tablename** został ustawiony na wartość **emp**.
    * Tabela emp bazy danych zawiera trzy kolumny — **ID**, **FirstName** i **LastName**. ID to kolumna tożsamości, więc należy określić tylko wartości **FirstName** i **LastName**.
    * Parametr **availability** (dostępność) został ustawiony na wartość **hourly** (co godzinę) (parametr **frequency** [częstotliwość] został ustawiony na **hour** [godzinę], a **interval** [interwał] został ustawiony na wartość **1**).  Usługa Data Factory co godzinę generuje wycinek danych wyjściowych w tabeli **emp** w bazie danych Azure SQL Database.

2.  Uruchom następujące polecenie, aby utworzyć zestaw danych usługi Fabryka danych. 
    
        New-AzureRmDataFactoryDataset $df -File .\EmpSQLTable.json


## Tworzenie potoku
W tym kroku opisano tworzenie potoku za pomocą **działania kopiowania**, w którym parametr **EmpTableFromBlob** jest używany jako dane wejściowe, a parametr **EmpSQLTable** jako dane wyjściowe.

1.  Utwórz plik JSON o nazwie **ADFTutorialPipeline.json** w folderze **C:\ADFGetStartedPSH** o następującej zawartości: 
    
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
                        "type": "SqlSink"
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
                "start": "2016-08-09T00:00:00Z",
                "end": "2016-08-10T00:00:00Z",
                "isPaused": false
              }
            }

    Pamiętaj o następujących kwestiach:

    - W sekcji działań jest tylko jedno działanie, którego parametr **type** (typ) został ustawiony na wartość **Copy**.
    - Dane wejściowe dla działania zostały ustawione na wartość **EmpTableFromBlob**, a dane wyjściowe dla działania zostały ustawione na wartość **EmpSQLTable**.
    - W sekcji **przekształcania** parametr **BlobSource** został określony jako typ źródłowy, a parametr **SqlSink** został określony jako typ ujścia.

    Zastąp wartość właściwości **start** bieżącym dniem, a wartość właściwości **end** następnym dniem. Zarówno data/godzina rozpoczęcia, jak i data/godzina zakończenia muszą być w [formacie ISO](http://en.wikipedia.org/wiki/ISO_8601). Na przykład: 2014-10-14T16:32:41Z. Czas **end** jest opcjonalny, ale w tym samouczku zostanie użyty. 
    
    Jeśli nie określisz wartości dla właściwości **end**, zostanie ona obliczona jako „**czas rozpoczęcia + 48 godzin**”. Aby uruchomić potok bezterminowo, określ **9/9/9999** jako wartość dla właściwości **end**.
    
    W tym przykładzie występują 24 wycinki danych, ponieważ poszczególne wycinki są generowane co godzinę.
    
    Szczegółowe informacje dotyczące właściwości kodu JSON znajdują się w artykule [JSON Scripting Reference](http://go.microsoft.com/fwlink/?LinkId=516971) (Dokumentacja dotycząca skryptów JSON).
2.  Uruchom następujące polecenie, aby utworzyć tabelę usługi Fabryka danych. 
        
        New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json

**Gratulacje!** Udało Ci się utworzyć fabrykę danych Azure, połączone usługi, tabele i potok oraz zaplanować potok.

## Monitorowanie potoku
W tym kroku opisano monitorowanie tego, co dzieje się w fabryce danych Azure za pomocą programu Azure PowerShell.

1.  Uruchom polecenie **Get-AzureRmDataFactory** i przypisz dane wyjściowe do zmiennej $df.

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH
 
2.  Uruchom polecenie **Get-AzureRmDataFactorySlice**, aby uzyskać szczegółowe informacje na temat wszystkich wycinków elementu **EmpSQLTable**, który stanowi tabelę wyjściową potoku.  

        Get-AzureRmDataFactorySlice $df -DatasetName EmpSQLTable -StartDateTime 2015-03-03T00:00:00

    Zastąp rok, miesiąc i datę w parametrze **StartDateTime** bieżącym rokiem, miesiącem i datą. To ustawienie powinno być zgodne z wartością właściwości **Start** w kodzie JSON potoku. 

    Powinny zostać wyświetlone 24 wycinki, jeden dla każdej godziny od godz. 12: 00 bieżącego dnia do godz. 12: 00 dnia następnego. 
    
    **Pierwszy wycinek:**

        ResourceGroupName : ADFTutorialResourceGroup
        DataFactoryName   : ADFTutorialDataFactoryPSH
        TableName         : EmpSQLTable
        Start             : 3/3/2015 12:00:00 AM
        End               : 3/3/2015 1:00:00 AM
        RetryCount        : 0
        Status            : Waiting
        LatencyStatus     :
        LongRetryCount    : 0

    **Ostatni wycinek:**

        ResourceGroupName : ADFTutorialResourceGroup
        DataFactoryName   : ADFTutorialDataFactoryPSH
        TableName         : EmpSQLTable
        Start             : 3/4/2015 11:00:00 PM
        End               : 3/4/2015 12:00:00 AM
        RetryCount        : 0
        Status            : Waiting
        LatencyStatus     : 
        LongRetryCount    : 0

3.  Uruchom polecenie **Get-AzureRmDataFactoryRun**, aby uzyskać szczegółowe informacje o uruchomieniach działania dla **określonego** wycinka. Zmień wartość parametru **StartDateTime**, aby była zgodna z czasem **rozpoczęcia** wycinka z danych wyjściowych. Wartość **StartDateTime** musi być w [formacie ISO](http://en.wikipedia.org/wiki/ISO_8601). Na przykład: 2014-03-03T22:00:00Z.

        Get-AzureRmDataFactoryRun $df -DatasetName EmpSQLTable -StartDateTime 2015-03-03T22:00:00

    Powinny zostać wyświetlone dane wyjściowe podobne do następujących:

        Id                  : 3404c187-c889-4f88-933b-2a2f5cd84e90_635614488000000000_635614524000000000_EmpSQLTable
        ResourceGroupName   : ADFTutorialResourceGroup
        DataFactoryName     : ADFTutorialDataFactoryPSH
        TableName           : EmpSQLTable
        ProcessingStartTime : 3/3/2015 11:03:28 PM
        ProcessingEndTime   : 3/3/2015 11:04:36 PM
        PercentComplete     : 100
        DataSliceStart      : 3/8/2015 10:00:00 PM
        DataSliceEnd        : 3/8/2015 11:00:00 PM
        Status              : Succeeded
        Timestamp           : 3/8/2015 11:03:28 PM
        RetryAttempt        : 0
        Properties          : {}
        ErrorMessage        :
        ActivityName        : CopyFromBlobToSQL
        PipelineName        : ADFTutorialPipeline
        Type                : Copy

Pełna dokumentacja dotycząca poleceń cmdlet dla usługi Fabryka danych znajduje się w artykule [Data Factory Cmdlet Reference] (Dokumentacja dotycząca poleceń cmdlet dla usługi Fabryka zadań) [cmdlet-reference]. 

## Podsumowanie
W tym samouczku opisano tworzenie fabryki danych Azure w celu kopiowania danych z obiektu blob Azure do bazy danych SQL Azure. Program PowerShell został użyty do utworzenia fabryki danych, połączonych usług, zestawów danych oraz potoku. Główne kroki opisane w tym samouczku:  

1.  Tworzenie **fabryki danych** Azure.
2.  Tworzenie **połączonych usług**:
    1. Połączona usługa **Azure Storage** w celu połączenia z kontem usługi Azure Storage, na którym przechowywane są dane wejściowe.    
    2. Połączona usługa **SQL Azure** w celu połączenia z bazą danych SQL Azure, w której przechowywane są dane wyjściowe. 
3.  Tworzenie **zestawów danych** opisujących dane wejściowe i wyjściowe dla potoków.
4.  Tworzenie **potoku** za pomocą **działania kopiowania**, w którym źródłem jest element **BlobSource**, a ujściem element **SqlSink**. 

## Zobacz też
| Temat | Opis |
| :---- | :---- |
| [Działania przenoszenia danych](data-factory-data-movement-activities.md) | Ten artykuł zawiera szczegółowe informacje dotyczące działania kopiowania używanego w tym samouczku. |
| [Planowanie i wykonywanie](data-factory-scheduling-and-execution.md) | W tym artykule wyjaśniono aspekty planowania i wykonywania modelu aplikacji usługi Fabryka danych Azure. |
| [Potoki](data-factory-create-pipelines.md) | Ten artykuł ułatwia zapoznanie się z potokami i działaniami w usłudze Azure Data Factory |
| [Zestawy danych](data-factory-create-datasets.md) | Ten artykuł ułatwia zapoznanie się z zestawami danych w usłudze Azure Data Factory.
| [Monitorowanie potoków i zarządzanie nimi za pomocą aplikacji do monitorowania](data-factory-monitor-manage-app.md) | Ten artykuł zawiera instrukcje dotyczące monitorowania i debugowania potoków oraz zarządzania nimi przy użyciu aplikacji do monitorowania i zarządzania. 



[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908

[cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234.aspx
[old-cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234(v=azure.98).aspx
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[azure-portal]: http://portal.azure.com
[download-azure-powershell]: ../powershell-install-configure.md
[data-factory-introduction]: data-factory-introduction.md

[image-data-factory-get-started-storage-explorer]: ./media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png

[sql-management-studio]: ../sql-database/sql-database-manage-azure-ssms.md
 


<!--HONumber=sep16_HO1-->


