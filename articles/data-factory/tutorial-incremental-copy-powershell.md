---
title: "Przyrostowe kopiowanie danych przy użyciu usługi Azure Data Factory | Microsoft Docs"
description: "W tym samouczku utworzysz potok usługi Azure Data Factory, który przyrostowo kopiuje dane z bazy danych Azure SQL Database do magazynu Azure Blob Storage."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/06/2017
ms.author: shlo
ms.openlocfilehash: f6f90f35fc4d26c0cdb021e85a159d4984d48399
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="incrementally-load-data-from-azure-sql-database-to-azure-blob-storage"></a>Przyrostowe ładowanie danych z bazy danych Azure SQL Database do magazynu Azure Blob Storage

[!INCLUDE [data-factory-what-is-include-md](../../includes/data-factory-what-is-include.md)]

#### <a name="this-tutorial"></a>Ten samouczek

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz [dokumentację dotyczącą usługi Data Factory w wersji 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Podczas procedury integracji danych jednym z powszechnie używanych scenariuszy jest okresowe przyrostowe ładowanie danych w celu odświeżenia uaktualnionego wyniku analizy po początkowym załadowaniu i analizie danych. W tym samouczku skupisz się na ładowaniu tylko nowych lub zaktualizowanych rekordów ze źródeł danych do ujść danych. Jest to procedura bardziej efektywna niż pełne ładowanie, zwłaszcza w przypadku dużych zestawów danych.    

Usługa Data Factory umożliwia tworzenie rozwiązań z górnym limitem w celu osiągnięcia przyrostowego ładowania danych przy użyciu działań procedur wyszukiwania, kopiowania i składowania w potoku.  

Ten samouczek obejmuje następujące procedury:

> [!div class="checklist"]
> * Przygotowywanie magazynu danych do przechowywania wartości limitu.   
> * Tworzenie fabryki danych.
> * Tworzenie połączonych usług. 
> * Tworzenie zestawu danych źródła, ujścia, limitu.
> * Tworzenie potoku.
> * Uruchamianie potoku.
> * Monitorowanie uruchomienia potoku. 

## <a name="overview"></a>Omówienie
Diagram ogólny rozwiązania wygląda następująco: 

![Przyrostowe ładowanie danych](media\tutorial-Incrementally-load-data-from-azure-sql-to-blob\incrementally-load.png)

Poniżej przedstawiono ważne czynności związane z tworzeniem tego rozwiązania: 

1. **Wybierz kolumnę limitu**.
    Wybierz jedną kolumnę w magazynie danych źródłowych, która może służyć do tworzenia wycinków nowych lub zaktualizowanych rekordów dla każdego przebiegu. Zazwyczaj dane w tej wybranej kolumnie (na przykład last_modify_time lub ID) rosną wraz z tworzeniem i aktualizacją wierszy. Maksymalna wartość w tej kolumnie jest używana jako limit.
2. **Przygotuj magazyn danych do przechowywania wartości limitu**.   
    W tym samouczku wartość limitu jest przechowywana w bazie danych Azure SQL.
3. **Utwórz potok z następującym przepływem pracy:** 
    
    Potok w tym rozwiązaniu obejmuje następujące działania:
  
    1. Utwórz dwa działania **wyszukiwania**. Użyj pierwszego działania wyszukiwania do pobrania ostatniej wartości limitu. Użyj drugiego działania wyszukiwania do pobrania nowej wartości limitu. Te wartości limitu są przekazywane do działania kopiowania. 
    2. Utwórz **działanie kopiowania**, które kopiuje wiersze z magazynu danych źródłowych o wartości kolumny limitu większej niż poprzednia wartość limitu i mniejszej niż nowa wartość limitu. Następnie kopiuje dane różnicowe z magazynu danych źródłowych do magazynu obiektów blob jako nowy plik. 
    3. Utwórz **działanie procedury składowania**, które aktualizuje wartość limitu dla potoku przy następnym uruchomieniu. 


Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="prerequisites"></a>Wymagania wstępne
* **Usługa Azure SQL Database**. Baza danych jest używana jako magazyn danych **źródłowych**. Jeśli nie masz bazy danych Azure SQL Database, utwórz ją, wykonując czynności przedstawione w artykule [Create an Azure SQL database (Tworzenie bazy danych Azure SQL Database)](../sql-database/sql-database-get-started-portal.md).
* **Konto usługi Azure Storage**. Magazyn obiektów blob jest używany jako magazyn danych **źródłowych**. Jeśli nie masz konta usługi Azure Storage, utwórz je, wykonując czynności przedstawione w artykule [Tworzenie konta magazynu](../storage/common/storage-create-storage-account.md#create-a-storage-account). Utwórz kontener o nazwie **adftutorial**. 
* Zainstalowanie programu **Azure PowerShell**. Wykonaj instrukcje podane w temacie [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/install-azurerm-ps).

### <a name="create-a-data-source-table-in-your-azure-sql-database"></a>Tworzenie tabeli danych źródłowych w bazie danych Azure SQL
1. Otwórz program **SQL Server Management Studio**, w **Eksploratorze serwera** kliknij bazę danych prawym przyciskiem myszy i wybierz pozycję **Nowe zapytanie**.
2. Uruchom poniższe polecenie SQL dla bazy danych Azure SQL, aby utworzyć tabelę o nazwie `data_source_table` jako magazyn danych źródłowych.  
    
    ```sql
    create table data_source_table
    (
        PersonID int,
        Name varchar(255),
        LastModifytime datetime
    );

    INSERT INTO data_source_table
    (PersonID, Name, LastModifytime)
    VALUES
    (1, 'aaaa','9/1/2017 12:56:00 AM'),
    (2, 'bbbb','9/2/2017 5:23:00 AM'),
    (3, 'cccc','9/3/2017 2:36:00 AM'),
    (4, 'dddd','9/4/2017 3:21:00 AM'),
    (5, 'eeee','9/5/2017 8:06:00 AM');
    ```
    W tym samouczku użyjesz **LastModifytime** jako kolumny **limitu**.  Dane w magazynie danych źródłowych zostały przedstawione w poniższej tabeli:

    ```
    PersonID | Name | LastModifytime
    -------- | ---- | --------------
    1 | aaaa | 2017-09-01 00:56:00.000
    2 | bbbb | 2017-09-02 05:23:00.000
    3 | cccc | 2017-09-03 02:36:00.000
    4 | dddd | 2017-09-04 03:21:00.000
    5 | eeee | 2017-09-05 08:06:00.000
    ```

### <a name="create-another-table-in-sql-database-to-store-the-high-watermark-value"></a>Utwórz inną tabelę w bazie danych SQL do przechowywania wartości górnego limitu
1. Uruchom poniższe polecenie SQL dla bazy danych Azure SQL, aby utworzyć tabelę o nazwie `watermarktable` w celu przechowywania wartości limitu.  
    
    ```sql
    create table watermarktable
    (
    
    TableName varchar(255),
    WatermarkValue datetime,
    );
    ```
3. Ustaw domyślną **wartość** górnego limitu z nazwą tabeli magazynu danych źródłowych.  (W tym samouczku nazwa tabeli to: **data_source_table**)

    ```sql
    INSERT INTO watermarktable
    VALUES ('data_source_table','1/1/2010 12:00:00 AM')    
    ```
4. Sprawdź dane w tabeli: `watermarktable`.
    
    ```sql
    Select * from watermarktable
    ```
    Dane wyjściowe: 

    ```
    TableName  | WatermarkValue
    ----------  | --------------
    data_source_table | 2010-01-01 00:00:00.000
    ```

### <a name="create-a-stored-procedure-in-azure-sql-database"></a>Tworzenie procedur składowanych w bazie danych Azure SQL 

Uruchom następujące polecenie, aby utworzyć procedurę składowaną w bazie danych Azure SQL.

```sql
CREATE PROCEDURE sp_write_watermark @LastModifiedtime datetime, @TableName varchar(50)
AS

BEGIN
    
    UPDATE watermarktable
    SET [WatermarkValue] = @LastModifiedtime 
WHERE [TableName] = @TableName
    
END
```

## <a name="create-a-data-factory"></a>Tworzenie fabryki danych

1. Uruchom program **PowerShell**. Nie zamykaj programu Azure PowerShell, zanim nie wykonasz wszystkich instrukcji z tego samouczka. Jeśli go zamkniesz i otworzysz ponownie, musisz uruchomić te polecenia jeszcze raz.

    Uruchom poniższe polecenie i wprowadź nazwę użytkownika oraz hasło, których używasz do logowania się w witrynie Azure Portal:
        
    ```powershell
    Login-AzureRmAccount
    ```        
    Uruchom poniższe polecenie, aby wyświetlić wszystkie subskrypcje dla tego konta:

    ```powershell
    Get-AzureRmSubscription
    ```
    Uruchom poniższe polecenie, aby wybrać subskrypcję, z którą chcesz pracować. Zastąp parametr **SubscriptionId** identyfikatorem Twojej subskrypcji platformy Azure:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```
2. Uruchom polecenie cmdlet **Set-AzureRmDataFactoryV2**, aby utworzyć fabrykę danych. Przed uruchomieniem polecenia zastąp symbol zastępczy własnymi wartościami.

    ```powershell
    Set-AzureRmDataFactoryV2 -ResourceGroupName "<your resource group to create the factory>" -Location "East US" -Name "<specify the name of data factory to create. It must be globally unique.>" 
    ```

    Pamiętaj o następujących kwestiach:

    * Nazwa fabryki danych Azure musi być globalnie unikatowa. Jeśli zostanie wyświetlony następujący błąd, zmień nazwę i spróbuj ponownie.

        ```
        The specified Data Factory name '<data factory name>' is already in use. Data Factory names must be globally unique.
        ```

    * Aby tworzyć wystąpienia usługi Data Factory, musisz być współautorem lub administratorem subskrypcji platformy Azure.
    * Obecnie usługa danych Data Factory V2 umożliwia tworzenie fabryk danych tylko w regionie Wschodnie stany USA. Magazyny danych (Azure Storage, Azure SQL Database itp.) i jednostki obliczeniowe (HDInsight itp.) używane przez fabrykę danych mogą mieścić się w innych regionach.


## <a name="create-linked-services"></a>Tworzenie połączonych usług
Połączone usługi tworzy się w fabryce danych w celu połączenia magazynów danych i usług obliczeniowych z fabryką danych. W tej sekcji utworzysz usługi połączone ze swoim kontem usługi Azure Storage i bazą danych Azure SQL. 

### <a name="create-azure-storage-linked-service"></a>Utwórz połączoną usługę Azure Storage.
1. Utwórz plik JSON o nazwie **AzureStorageLinkedService.json** w folderze **C:\ADF** o następującej zawartości: (utwórz folder ADF, jeśli jeszcze nie istnieje). Przed zapisaniem pliku zastąp wartości `<accountName>` i `<accountKey>` nazwą i kluczem konta usługi Azure Storage.

    ```json
    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountName>;AccountKey=<accountKey>",
                    "type": "SecureString"
                }
            }
        }
    }
    ```
2. W programie **Azure PowerShell** przejdź do folderu **ADF**.
3. Uruchom polecenie cmdlet **Set-AzureRmDataFactoryV2LinkedService**, aby utworzyć połączoną usługę: **AzureStorageLinkedService**. W poniższym przykładzie przekazujesz wartości dla parametrów **ResourceGroupName** i **DataFactoryName**. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureStorageLinkedService" -File ".\AzureStorageLinkedService.json"
    ```

    Oto przykładowe dane wyjściowe:

    ```json
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

### <a name="create-azure-sql-database-linked-service"></a>Utwórz połączoną usługę Azure SQL Database.
1. Utwórz plik JSON o nazwie **AzureSQLDatabaseLinkedService.json** w folderze **C:\ADF** o następującej zawartości: (utwórz folder ADF, jeśli jeszcze nie istnieje). Zastąp parametry **&lt;server&gt;, &lt;user id&gt; i &lt;password&gt;** nazwą swojego serwera Azure SQL, identyfikatorem użytkownika i hasłem przed zapisaniem pliku. 

    ```json
    {
        "name": "AzureSQLDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": {
                    "value": "Server = tcp:<server>.database.windows.net,1433;Initial Catalog=<database name>; Persist Security Info=False; User ID=<user name> ; Password=<password>; MultipleActiveResultSets = False; Encrypt = True; TrustServerCertificate = False; Connection Timeout = 30;",
                    "type": "SecureString"
                }
            }
        }
    }
    ```
2. W programie **Azure PowerShell** przejdź do folderu **ADF**.
3. Uruchom polecenie cmdlet **Set-AzureRmDataFactoryV2LinkedService**, aby utworzyć połączoną usługę: **AzureSQLDatabaseLinkedService**. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureSQLDatabaseLinkedService" -File ".\AzureSQLDatabaseLinkedService.json"
    ```

    Oto przykładowe dane wyjściowe:

    ```json
    LinkedServiceName : AzureSQLDatabaseLinkedService
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlDatabaseLinkedService
    ProvisioningState :
    ```

## <a name="create-datasets"></a>Tworzenie zestawów danych
W tym kroku utworzysz zestawy danych reprezentujące dane źródłowe i ujścia. 

### <a name="create-a-source-dataset"></a>Tworzenie zestawu danych źródłowych

1. Utwórz plik JSON o nazwie SourceDataset.json w tym samym folderze, o następującej zawartości: 

    ```json
    {
        "name": "SourceDataset",
        "properties": {
            "type": "AzureSqlTable",
            "typeProperties": {
                "tableName": "data_source_table"
            },
            "linkedServiceName": {
                "referenceName": "AzureSQLDatabaseLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }
   
    ```
    W tym samouczku używamy nazwy tabeli **data_source_table**. Zastąp ją, jeśli używasz tabeli o innej nazwie. 
2.  Uruchom polecenie cmdlet Set-AzureRmDataFactoryV2Dataset, aby utworzyć zestaw danych: SourceDataset
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SourceDataset" -File ".\SourceDataset.json"
    ```

    Oto przykładowe dane wyjściowe polecenia cmdlet:
    
    ```json
    DatasetName       : SourceDataset
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlTableDataset
    ```

### <a name="create-a-sink-dataset"></a>Tworzenie ujścia zestawu danych

1. Utwórz plik JSON o nazwie SinkDataset.json w tym samym folderze, o następującej zawartości: 

    ```json
    {
        "name": "SinkDataset",
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "adftutorial/incrementalcopy",
                "fileName": "@CONCAT('Incremental-', pipeline().RunId, '.txt')", 
                "format": {
                    "type": "TextFormat"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }   
    ```

    > [!IMPORTANT]
    > W tym fragmencie kodu założono, że w masz kontener obiektów blob **adftutorial** w magazynie obiektów Azure Blob Storage. Utwórz kontener, jeśli nie istnieje, lub zmień nazwę istniejącego kontenera. Folder wyjściowy `incrementalcopy` jest tworzony automatycznie, jeśli nie występuje w kontenerze. W tym samouczku nazwa pliku jest generowana dynamicznie przy użyciu wyrażenia: `@CONCAT('Incremental-', pipeline().RunId, '.txt')`.
2.  Uruchom polecenie cmdlet Set-AzureRmDataFactoryV2Dataset, aby utworzyć zestaw danych: SinkDataset
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SinkDataset" -File ".\SinkDataset.json"
    ```

    Oto przykładowe dane wyjściowe polecenia cmdlet:
    
    ```json
    DatasetName       : SinkDataset
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset    
    ```

## <a name="create-a-dataset-for-watermark"></a>Tworzenie zestawu danych dla limitu
W tym kroku utworzysz zestaw danych do przechowywania wartości górnego limitu. 

1. Utwórz plik JSON o nazwie WatermarkDataset.json w tym samym folderze, o następującej zawartości: 

    ```json
    {
        "name": " WatermarkDataset ",
        "properties": {
            "type": "AzureSqlTable",
            "typeProperties": {
                "tableName": "watermarktable"
            },
            "linkedServiceName": {
                "referenceName": "AzureSQLDatabaseLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }    
    ```
2.  Uruchom polecenie cmdlet Set-AzureRmDataFactoryV2Dataset, aby utworzyć zestaw danych: SinkDataset
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "WatermarkDataset" -File ".\WatermarkDataset.json"
    ```

    Oto przykładowe dane wyjściowe polecenia cmdlet:
    
    ```json
    DatasetName       : WatermarkDataset
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlTableDataset    
    ```

## <a name="create-a-pipeline"></a>Tworzenie potoku
W tym samouczku utworzysz potok z dwoma działaniami wyszukiwania, jednym działaniem kopiowania i jednym działaniem procedury składowanej, połączonymi w jednym potoku. 


1. Utwórz plik JSON: IncrementalCopyPipeline.json w tym samym folderze o następującej zawartości. 

    ```json
    {
        "name": "IncrementalCopyPipeline",
        "properties": {
            "activities": [
                {
                    "name": "LookupOldWaterMarkActivity",
                    "type": "Lookup",
                    "typeProperties": {
                        "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from watermarktable"
                        },
    
                        "dataset": {
                        "referenceName": "WatermarkDataset",
                        "type": "DatasetReference"
                        }
                    }
                },
                {
                    "name": "LookupNewWaterMarkActivity",
                    "type": "Lookup",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "select MAX(LastModifytime) as NewWatermarkvalue from data_source_table"
                        },
    
                        "dataset": {
                        "referenceName": "SourceDataset",
                        "type": "DatasetReference"
                        }
                    }
                },
                
                {
                    "name": "IncrementalCopyActivity",
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "select * from data_source_table where LastModifytime > '@{activity('LookupOldWaterMarkActivity').output.firstRow.WatermarkValue}' and LastModifytime <= '@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}'"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "dependsOn": [
                        {
                            "activity": "LookupNewWaterMarkActivity",
                            "dependencyConditions": [
                                "Succeeded"
                            ]
                        },
                        {
                            "activity": "LookupOldWaterMarkActivity",
                            "dependencyConditions": [
                                "Succeeded"
                            ]
                        }
                    ],
    
                    "inputs": [
                        {
                            "referenceName": "SourceDataset",
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "SinkDataset",
                            "type": "DatasetReference"
                        }
                    ]
                },
    
                {
                    "name": "StoredProceduretoWriteWatermarkActivity",
                    "type": "SqlServerStoredProcedure",
                    "typeProperties": {
    
                        "storedProcedureName": "sp_write_watermark",
                        "storedProcedureParameters": {
                            "LastModifiedtime": {"value": "@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}", "type": "datetime" },
                            "TableName":  { "value":"@{activity('LookupOldWaterMarkActivity').output.firstRow.TableName}", "type":"String"}
                        }
                    },
    
                    "linkedServiceName": {
                        "referenceName": "AzureSQLDatabaseLinkedService",
                        "type": "LinkedServiceReference"
                    },
    
                    "dependsOn": [
                        {
                            "activity": "IncrementalCopyActivity",
                            "dependencyConditions": [
                                "Succeeded"
                            ]
                        }
                    ]
                }
            ]
            
        }
    }
    ```
    

2. Uruchom polecenie cmdlet Set-AzureRmDataFactoryV2Pipeline, aby utworzyć potok: IncrementalCopyPipeline.
    
   ```powershell
   Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "IncrementalCopyPipeline" -File ".\IncrementalCopyPipeline.json"
   ``` 

   Oto przykładowe dane wyjściowe: 

   ```json
    PipelineName      : IncrementalCopyPipeline
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Activities        : {LookupOldWaterMarkActivity, LookupNewWaterMarkActivity, IncrementalCopyActivity, StoredProceduretoWriteWatermarkActivity}
    Parameters        :
   ```
 
## <a name="run-the-pipeline"></a>Uruchamianie potoku

1. Uruchom potok: **IncrementalCopyPipeline** za pomocą polecenia cmdlet **Invoke-AzureRmDataFactoryV2Pipeline**. Zastąp symbole zastępcze własną nazwą grupy zasobów i fabryki danych.

    ```powershell
    $RunId = Invoke-AzureRmDataFactoryV2Pipeline -PipelineName "IncrementalCopyPipeline" -ResourceGroup "<your resource group>" -dataFactoryName "<your data factory name>"
    ``` 
2. Sprawdź stan potoku, uruchamiając polecenie cmdlet Get-AzureRmDataFactoryV2ActivityRun, aż do wyświetlenia wszystkich uruchomionych pomyślnie działań. Zastąp symbole zastępcze własnym odpowiednim czasem dla parametrów RunStartedAfter i RunStartedBefore.  W tym samouczku użyto parametrów -RunStartedAfter "2017/09/14" -RunStartedBefore "2017/09/15"

    ```powershell
    Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $RunId -RunStartedAfter "<start time>" -RunStartedBefore "<end time>"
    ```

    Oto przykładowe dane wyjściowe:
 
    ```json
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupNewWaterMarkActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {NewWatermarkvalue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:42:42 AM
    ActivityRunEnd    : 9/14/2017 7:42:50 AM
    DurationInMs      : 7777
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupOldWaterMarkActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {TableName, WatermarkValue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:42:42 AM
    ActivityRunEnd    : 9/14/2017 7:43:07 AM
    DurationInMs      : 25437
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : IncrementalCopyActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, sink}
    Output            : {dataRead, dataWritten, rowsCopied, copyDuration...}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:43:10 AM
    ActivityRunEnd    : 9/14/2017 7:43:29 AM
    DurationInMs      : 19769
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : StoredProceduretoWriteWatermarkActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {storedProcedureName, storedProcedureParameters}
    Output            : {}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:43:32 AM
    ActivityRunEnd    : 9/14/2017 7:43:47 AM
    DurationInMs      : 14467
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}

    ```

## <a name="review-the-results"></a>Sprawdzanie wyników

1. W magazynie obiektów blob Azure (magazynie ujścia) powinien być widoczny fakt skopiowania danych do pliku zdefiniowanego w parametrze SinkDataset.  W tym samouczku nazwą pliku jest `Incremental- d4bf3ce2-5d60-43f3-9318-923155f61037.txt`.  Otwórz plik. Powinny zostać wyświetlone rekordy w pliku identyczne z danymi w bazie danych Azure SQL.

    ```
    1,aaaa,2017-09-01 00:56:00.0000000
    2,bbbb,2017-09-02 05:23:00.0000000
    3,cccc,2017-09-03 02:36:00.0000000
    4,dddd,2017-09-04 03:21:00.0000000
    5,eeee,2017-09-05 08:06:00.0000000
    ``` 
2. Sprawdź najnowszą wartość z `watermarktable`, zobaczysz, że wartość limitu została zaktualizowana.

    ```sql
    Select * from watermarktable
    ```
    
    Oto przykładowe dane wyjściowe:
 
    TableName | WatermarkValue
    --------- | --------------
    data_source_table   2017-09-05  8:06:00.000

### <a name="insert-data-into-data-source-store-to-verify-delta-data-loading"></a>Wstawianie danych ze źródła danych w celu sprawdzenia ładowania danych różnicowych

1. Wstaw nowe dane do bazy danych Azure SQL (magazynu danych źródłowych):

    ```sql
    INSERT INTO data_source_table
    VALUES (6, 'newdata','9/6/2017 2:23:00 AM')
    
    INSERT INTO data_source_table
    VALUES (7, 'newdata','9/7/2017 9:01:00 AM')
    ``` 

    Zaktualizowane dane w bazie danych Azure SQL są następujące:

    ```
    PersonID | Name | LastModifytime
    -------- | ---- | --------------
    1 | aaaa | 2017-09-01 00:56:00.000
    2 | bbbb | 2017-09-02 05:23:00.000
    3 | cccc | 2017-09-03 02:36:00.000
    4 | dddd | 2017-09-04 03:21:00.000
    5 | eeee | 2017-09-05 08:06:00.000
    6 | newdata | 2017-09-06 02:23:00.000
    7 | newdata | 2017-09-07 09:01:00.000
    ```
2. Uruchom potok **IncrementalCopyPipeline** ponownie za pomocą polecenia cmdlet **Invoke-AzureRmDataFactoryV2Pipeline**. Zastąp symbole zastępcze własną nazwą grupy zasobów i fabryki danych.

    ```powershell
    $RunId = Invoke-AzureRmDataFactoryV2Pipeline -PipelineName "IncrementalCopyPipeline" -ResourceGroup "<your resource group>" -dataFactoryName "<your data factory name>"
    ```
3. Sprawdź stan potoku, uruchamiając polecenie cmdlet **Get-AzureRmDataFactoryV2ActivityRun**, aż do wyświetlenia wszystkich uruchomionych pomyślnie działań. Zastąp symbole zastępcze własnym odpowiednim czasem dla parametrów RunStartedAfter i RunStartedBefore.  W tym samouczku użyto parametrów -RunStartedAfter "2017/09/14" -RunStartedBefore "2017/09/15"

    ```powershell
    Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $RunId -RunStartedAfter "<start time>" -RunStartedBefore "<end time>"
    ```

    Oto przykładowe dane wyjściowe:
 
    ```json
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupNewWaterMarkActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {NewWatermarkvalue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:52:26 AM
    ActivityRunEnd    : 9/14/2017 8:52:58 AM
    DurationInMs      : 31758
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupOldWaterMarkActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {TableName, WatermarkValue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:52:26 AM
    ActivityRunEnd    : 9/14/2017 8:52:52 AM
    DurationInMs      : 25497
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : IncrementalCopyActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, sink}
    Output            : {dataRead, dataWritten, rowsCopied, copyDuration...}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:53:00 AM
    ActivityRunEnd    : 9/14/2017 8:53:20 AM
    DurationInMs      : 20194
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : StoredProceduretoWriteWatermarkActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {storedProcedureName, storedProcedureParameters}
    Output            : {}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:53:23 AM
    ActivityRunEnd    : 9/14/2017 8:53:41 AM
    DurationInMs      : 18502
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}

    ```
4.  W magazynie Azure Blob Storage powinien być widoczny kolejny plik utworzony w magazynie. W tym samouczku nazwa nowego pliku to `Incremental-2fc90ab8-d42c-4583-aa64-755dba9925d7.txt`.  Otwórz ten plik, zobaczysz w nim 2 wiersze rekordów:
5.  Sprawdź najnowszą wartość z `watermarktable`, zobaczysz, że wartość limitu została ponownie zaktualizowana

    ```sql
    Select * from watermarktable
    ```
    Przykładowe dane wyjściowe: 
    
    TableName | WatermarkValue
    --------- | ---------------
    data_source_table | 2017-09-07 09:01:00.000

     
## <a name="next-steps"></a>Następne kroki
W ramach tego samouczka wykonano następujące procedury: 

> [!div class="checklist"]
> * Definiowanie kolumny **limitu** i zapisanie jej w bazie danych Azure SQL Database.  
> * Tworzenie fabryki danych.
> * Tworzenie połączonych usług dla bazy danych SQL i magazynu obiektów blob. 
> * Tworzenie zestawu danych źródła i ujścia.
> * Tworzenie potoku.
> * Uruchamianie potoku.
> * Monitorowanie uruchomienia potoku. 

Przejdź do następującego samouczka, aby dowiedzieć się więcej o przekształcaniu danych za pomocą klastra Spark na platformie Azure:

> [!div class="nextstepaction"]
>[Przekształcanie danych przy użyciu klastra Spark w chmurze](tutorial-transform-data-spark-powershell.md)



