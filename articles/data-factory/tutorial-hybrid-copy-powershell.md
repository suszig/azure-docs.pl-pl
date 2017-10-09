---
title: "Kopiowanie danych lokalnych do chmury przy użyciu usługi Azure Data Factory | Microsoft Docs"
description: "Dowiedz się, jak skopiować dane z lokalnego magazynu danych do chmury na platformie Azure przy użyciu własnego środowiska Integration Runtime w usłudze Azure Data Factory."
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
ms.date: 09/14/2017
ms.author: jingwang
ms.translationtype: HT
ms.sourcegitcommit: cb9130243bdc94ce58d6dfec3b96eb963cdaafb0
ms.openlocfilehash: 60641ddfef7846f0e8b5d850e716b2652bf62367
ms.contentlocale: pl-pl
ms.lasthandoff: 09/26/2017

---

# <a name="copy-data-between-on-premises-and-cloud"></a>Kopiowanie danych między środowiskiem lokalnym i chmurą
Azure Data Factory to oparta na chmurze usługa integracji danych, za pomocą której możesz tworzyć oparte na danych przepływy pracy w chmurze służące do organizowania oraz automatyzowania przenoszenia i przekształcania danych. Za pomocą usługi Azure Data Factory można tworzyć oparte na danych przepływy pracy (nazywane potokami) i ustalać ich harmonogram. Te przepływy mogą pozyskiwać dane z różnych magazynów danych, przetwarzać/przekształcać je za pomocą usług obliczeniowych, takich jak Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics i Azure Machine Learning, a następnie publikować dane wyjściowe w magazynach danych, np. Azure SQL Data Warehouse, do użycia przez aplikacje analizy biznesowej. 

W tym samouczku użyjesz programu Azure PowerShell, aby utworzyć potok fabryki danych, który kopiuje dane z lokalnej bazy danych programu SQL Server do usługi Azure Blob Storage. Utworzysz i użyjesz własnego środowiska Integration Runtime (IR) usługi Azure Data Factory, co umożliwi integrację lokalnych magazynów danych oraz magazynów danych w chmurze.  Aby dowiedzieć się więcej o zastosowaniu innych narzędzi/zestawów SDK do tworzenia fabryki danych, zapoznaj się z samouczkami [Szybki start](quickstart-create-data-factory-dot-net.md). 

Ten samouczek obejmuje następujące procedury:

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Tworzenie własnego środowiska Integration Runtime.
> * Tworzenie i szyfrowanie lokalnej usługi połączonej serwera SQL Server we własnym środowisku Integration Runtime.
> * Tworzenie połączonej usługi Azure Storage.
> * Tworzenie zestawów danych programu SQL Server i usługi Azure Storage.
> * Tworzenie potoku z działaniem kopiowania do przenoszenia danych.
> * Uruchamianie działania potoku.
> * Monitorowanie potoku i przebiegu działania.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="prerequisites"></a>Wymagania wstępne

* **SQL Server**. Użyj lokalnej bazy danych programu SQL Server jako **źródła** magazynu danych w tym samouczku. 
* **Konto usługi Azure Storage**. Użyj usługi Azure Blob Storage jako miejsca **docelowego/ujścia** magazynu danych w tym samouczku. Jeśli nie masz konta usługi Azure Storage, utwórz je, wykonując czynności przedstawione w artykule [Tworzenie konta magazynu](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* Zainstalowanie programu **Azure PowerShell**. Wykonaj instrukcje podane w temacie [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/install-azurerm-ps).

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
    $resourceGroupName = "<your resource group to create the factory>"
    $dataFactoryName = "<specify the name of data factory to create. It must be globally unique.>"
    $df = Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName
    ```

    Pamiętaj o następujących kwestiach:

    * Nazwa fabryki danych Azure musi być globalnie unikatowa. Jeśli zostanie wyświetlony następujący błąd, zmień nazwę i spróbuj ponownie.

        ```
        Data factory name "<data factory name>" is not available.
        ```

    * Aby tworzyć wystąpienia usługi Data Factory, musisz być współautorem lub administratorem subskrypcji platformy Azure.
    * Obecnie usługa Data Factory umożliwia tworzenie fabryk danych tylko w regionie Wschodnie stany USA i Wschodnie stany USA 2. Magazyny danych (Azure Storage, Azure SQL Database itp.) i jednostki obliczeniowe (HDInsight itp.) używane przez fabrykę danych mogą mieścić się w innych regionach.

## <a name="create-a-self-hosted-ir"></a>Tworzenie własnego środowiska IR

W tej sekcji możesz utworzyć własne środowisko Integration Runtime i skojarzyć je z węzłem lokalnym (maszyną).

1. Utwórz własne środowisko Integration Runtime. Użyj unikatowej nazwy w przypadku, gdy istnieje inne środowisko Integration Runtime o takiej samej nazwie.

   ```powershell
   $integrationRuntimeName = "<your integration runtime name>"
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Oto przykładowe dane wyjściowe:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Uruchom następujące polecenie, aby pobrać stan utworzonego środowiska Integration Runtime.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Oto przykładowe dane wyjściowe:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption : 
   Version                   : 
   Capabilities              : {}
   ScheduledUpdateDate       : 
   UpdateDelayOffset         : 
   LocalTimeZoneOffset       : 
   AutoUpdate                : 
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Uruchom następujące polecenie, aby pobrać klucze uwierzytelniania w celu zarejestrowania własnego środowiska Integration Runtime za pomocą usługi Data Factory w chmurze. Skopiuj jeden z kluczy w celu zarejestrowania środowiska Integration Runtime (Self-hosted).

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Oto przykładowe dane wyjściowe: 

   ```json
   {
       "AuthKey1":  "IR@8437c862-d6a9-4fb3-87dd-7d4865a9e845@ab1@eu@VDnzgySwUfaj3pfSUxpvfsXXXXXXx4GHiyF4wboad0Y=",
       "AuthKey2":  "IR@8437c862-d6a9-4fb3-85dd-7d4865a9e845@ab1@eu@sh+k/QNJGBltXL46vXXXXXXXXOf/M1Gne5aVqPtbweI="
   }
   ```

4. [Pobierz](https://www.microsoft.com/download/details.aspx?id=39717) własne środowisko Integration Runtime na komputer lokalny z systemem Windows, a następnie użyj klucza uwierzytelniania uzyskanego w poprzednim kroku, aby ręcznie zarejestrować własne środowisko Integration Runtime. 

   ![Rejestrowanie środowiska Integration Runtime](media/tutorial-hybrid-copy-powershell/register-integration-runtime.png)

   Gdy własne środowisko Integration Runtime zostanie pomyślnie zarejestrowane, zostanie wyświetlony następujący komunikat: 

   ![Pomyślnie zarejestrowano](media/tutorial-hybrid-copy-powershell/registered-successfully.png)

   Gdy węzeł zostanie połączony z usługą w chmurze, zostanie wyświetlona następująca strona: 
    
   ![Węzeł jest połączony](media/tutorial-hybrid-copy-powershell/node-is-connected.png)

## <a name="create-linked-services"></a>Tworzenie połączonych usług 

### <a name="create-an-azure-storage-linked-service-destinationsink"></a>Tworzenie połączonej usługi Azure Storage (obiektu docelowego/ujścia)

1. W folderze **C:\ADFv2Tutorial** utwórz plik JSON o nazwie **AzureStorageLinkedService.json** z następującą zawartością. Utwórz folder ADFv2Tutorial, jeśli jeszcze nie istnieje.  Zastąp wartości &lt;accountname&gt; i &lt;accountkey&gt; nazwą konta usługi Azure Storage oraz jej kluczem.

   ```json
    {
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": { 
                    "type": "SecureString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            }
        },
        "name": "AzureStorageLinkedService"
    }
   ```

2. W programie **Azure PowerShell** przejdź do folderu **ADFv2Tutorial**.

   Uruchom polecenie cmdlet **Set-AzureRmDataFactoryV2LinkedService**, aby utworzyć połączoną usługę: **AzureStorageLinkedService**. Polecenia cmdlet użyte w tym samouczku pobierają wartości dla parametrów **ResourceGroupName** i **DataFactoryName**. Możesz też przekazać obiekt **DataFactory** zwrócony przez polecenie cmdlet Set-AzureRmDataFactoryV2 bez wpisywania parametrów ResourceGroupName i DataFactoryName za każdym razem, gdy uruchamiasz polecenie cmdlet.

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "AzureStorageLinkedService" -File ".\AzureStorageLinkedService.json"
   ```

   Oto przykładowe dane wyjściowe:

    ```json
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

### <a name="create-and-encrypt-a-sql-server-linked-service-source"></a>Tworzenie i szyfrowanie połączonej usługi SQL Server (źródło)

1. Utwórz plik JSON o nazwie **SqlServerLinkedService.json** w folderze **C:\ADFv2Tutorial** o następującej zawartości: Zastąp zmienne **&lt;servername >**, **&lt;databasename>**, **&lt;username>**, **&lt;servername >** i **&lt;password>** wartościami Twojego programu SQL Server przed zapisaniem pliku. Zastąp zmienną **&lt;integration** **runtime** **name>** nazwą Twojego środowiska Integration Runtime. 

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }
   ```
2. Do szyfrowania poufnych danych z ładunku w formacie JSON we własnym lokalnym środowisku Integration Runtime można uruchomić polecenie **New-AzureRmDataFactoryV2LinkedServiceEncryptCredential** i przekazać powyższy ładunek w formacie JSON. To szyfrowanie zapewnia szyfrowanie poświadczeń za pomocą interfejsu API ochrony danych (DPAPI) i zapisanie ich lokalnie we własnym węźle środowiska Integration Runtime. Ładunek danych wyjściowych może zostać przekierowany do innego pliku JSON (w tym przypadku „encryptedLinkedService.json”), który zawiera zaszyfrowane poświadczenia. 

    Zastąp zmienną **&lt;integration runtime name>&gt;** nazwą Twojego środowiska Integration Runtime przed uruchomieniem polecenia.

   ```powershell
   New-AzureRmDataFactoryV2LinkedServiceEncryptCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -IntegrationRuntimeName <integration runtime name> -File ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
   ```

3. Uruchom następujące polecenie przy użyciu pliku JSON z poprzedniego kroku, aby utworzyć usługę **SqlServerLinkedService**:

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -File ".\encryptedSqlServerLinkedService.json" 
   ```


## <a name="create-datasets"></a>Tworzenie zestawów danych

### <a name="prepare-an-on-premises-sql-server-for-the-tutorial"></a>Przygotowanie lokalnego programu SQL Server dla tego samouczka

W tym kroku utworzysz zestawy danych wejściowych i wyjściowych, które reprezentują dane wejściowe i wyjściowe operacji kopiowania (lokalna baza danych programu SQL Server => usługa Azure Blob Storage). Przed utworzeniem zestawów danych wykonaj następujące czynności (szczegółowy opis kroków znajduje się po liście):

- Utwórz tabelę o nazwie **emp** w bazie danych programu SQL Server dodanej jako połączona usługa do fabryki danych i wstaw kilka przykładowych wpisów do tabeli.
- Utwórz kontener obiektów blob o nazwie **adftutorial** na koncie usługi Azure Blob Storage dodanym jako połączona usługa do fabryki danych.


1. W bazie danych określonej dla lokalnej połączonej usługi SQL Server (**SqlServerLinkedService**) użyj następującego skryptu SQL w celu utworzenia tabeli **emp** w bazie danych.

   ```sql   
     CREATE TABLE dbo.emp
     (
         ID int IDENTITY(1,1) NOT NULL,
         FirstName varchar(50),
         LastName varchar(50),
         CONSTRAINT PK_emp PRIMARY KEY (ID)
     )
     GO
   ```

2. Wstaw do tabeli kilka przykładów:

   ```sql
     INSERT INTO emp VALUES ('John', 'Doe')
     INSERT INTO emp VALUES ('Jane', 'Doe')
   ```



### <a name="create-a-dataset-for-source-sql-database"></a>Tworzenie zestawu danych źródłowej bazy danych SQL Database

1. Utwórz plik JSON o nazwie **SqlServerDataset.json** w folderze **C:\ADFv2Tutorial** o następującej zawartości:  

    ```json
    {
       "properties": {
            "type": "SqlServerTable",
            "typeProperties": {
                "tableName": "dbo.emp"
            },
            "structure": [
                 {
                    "name": "ID",
                    "type": "String"
                },
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "linkedServiceName": {
                "referenceName": "EncryptedSqlServerLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "SqlServerDataset"
    }
    ```

2. Aby utworzyć zestaw danych **SqlServerDataset**, uruchom polecenie cmdlet **Set-AzureRmDataFactoryV2Dataset**.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SqlServerDataset" -File ".\SqlServerDataset.json"
    ```

    Oto przykładowe dane wyjściowe:

    ```json
    DatasetName       : SqlServerDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         : {"name": "ID" "type": "String", "name": "FirstName" "type": "String", "name": "LastName" "type": "String"}
    Properties        : Microsoft.Azure.Management.DataFactory.Models.SqlServerTableDataset
    ```

### <a name="create-a-dataset-for-sink-azure-blob-storage"></a>Tworzenie zestawu danych ujścia usługi Azure Blob Storage

1. Utwórz plik JSON o nazwie **AzureBlobDataset.json** w folderze **C:\ADFv2Tutorial** o następującej zawartości: 

    > [!IMPORTANT]
    > W tym przykładzie kodu założono, że masz kontener o nazwie **adftutorial** w usłudze Azure Blob Storage.

    ```json
    {
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "adftutorial/fromonprem",
                "format": {
                    "type": "TextFormat"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "AzureBlobDataset"
    }
    ```

2. Aby utworzyć zestaw danych **AzureBlobDataset**, uruchom polecenie cmdlet **Set-AzureRmDataFactoryV2Dataset**.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureBlobDataset" -File ".\AzureBlobDataset.json"
    ```

    Oto przykładowe dane wyjściowe:

    ```json
    DatasetName       : AzureBlobDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-pipelines"></a>Tworzenie potoków

### <a name="create-the-pipeline-sqlservertoblobpipeline"></a>Tworzenie potoku „SqlServerToBlobPipeline”

1. Utwórz plik JSON o nazwie **SqlServerToBlobPipeline.json** w folderze **C:\ADFv2Tutorial** o następującej zawartości:

    ```json
    {
       "name": "SQLServerToBlobPipeline",
        "properties": {
            "activities": [       
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource"
                        },
                        "sink": {
                            "type":"BlobSink"
                        }
                    },
                    "name": "CopySqlServerToAzureBlobActivity",
                    "inputs": [
                        {
                            "referenceName": "SqlServerDataset",
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "AzureBlobDataset",
                            "type": "DatasetReference"
                        }
                    ]
                }
            ]
        }
    }
    ```

2. W celu utworzenia potoku **SQLServerToBlobPipeline** uruchom polecenie cmdlet **Set-AzureRmDataFactoryV2Pipeline**.

    ```powershell
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SQLServerToBlobPipeline" -File ".\SQLServerToBlobPipeline.json"
    ```

    Oto przykładowe dane wyjściowe:

    ```json
    PipelineName      : SQLServerToBlobPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Activities        : {CopySqlServerToAzureBlobActivity}
    Parameters        :  
    ```



## <a name="start-and-monitor-a-pipeline-run"></a>Uruchamianie i monitorowanie działania potoku

1. Uruchom działanie potoku dla potoku „SQLServerToBlobPipeline” i zarejestruj identyfikator działania potoku do monitorowania w przyszłości.  

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName 'SQLServerToBlobPipeline'
    ```

2. Uruchom następujący skrypt, aby stale sprawdzać stan działania potoku „**SQLServerToBlobPipeline**” i wydrukować wynik końcowy.

    ```powershell
    while ($True) {
        $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

        if (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
            Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
            Start-Sleep -Seconds 30
        }
        else {
            Write-Host "Pipeline 'SQLServerToBlobPipeline' run finished. Result:" -foregroundcolor "Yellow"
            $result
            break
        }
    } 
    ```

    Oto dane wyjściowe przykładowego uruchomienia:

    ```jdon
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    ActivityName      : copy
    PipelineRunId     : 4ec8980c-62f6-466f-92fa-e69b10f33640
    PipelineName      : SQLServerToBlobPipeline
    Input             :  
    Output            :  
    LinkedServiceName : 
    ActivityRunStart  : 9/13/2017 1:35:22 PM
    ActivityRunEnd    : 9/13/2017 1:35:42 PM
    DurationInMs      : 20824
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    ```

3. Aby uzyskać identyfikator działania potoku „**SQLServerToBlobPipeline**” i sprawdzić szczegółowe wyniki działania, wykonaj poniższe instrukcje.

    ```powershell
    Write-Host "Pipeline 'SQLServerToBlobPipeline' run result:" -foregroundcolor "Yellow"
    ($result | Where-Object {$_.ActivityName -eq "CopySqlServerToAzureBlobActivity"}).Output.ToString()
    ```

    Oto dane wyjściowe przykładowego uruchomienia:

    ```json
    {
      "dataRead": 36,
      "dataWritten": 24,
      "rowsCopied": 2,
      "copyDuration": 4,
      "throughput": 0.01,
      "errors": []
    }
    ```
4. Połącz się z ujściem Twojej usługi Azure Blob Storage i upewnij się, że dane zostały właściwie skopiowane z bazy danych Azure SQL.

## <a name="next-steps"></a>Następne kroki
Potok w tym przykładzie kopiuje dane z jednej lokalizacji do innej lokalizacji w usłudze Azure Blob Storage. W tym samouczku omówiono: 

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Tworzenie własnego środowiska Integration Runtime
> * Tworzenie i szyfrowanie lokalnej usługi połączonej programu SQL Server we własnym środowisku Integration Runtime
> * Tworzenie połączonej usługi Azure Storage.
> * Tworzenie zestawów danych programu SQL Server i usługi Azure Storage.
> * Tworzenie potoku z działaniem kopiowania do przenoszenia danych.
> * Uruchamianie działania potoku.
> * Monitorowanie potoku i przebiegu działania.

Aby uzyskać listę magazynów danych obsługiwanych przez usługę Azure Data Factory jako źródła i ujścia, zobacz artykuł dotyczący [obsługiwanych magazynów danych](copy-activity-overview.md#supported-data-stores-and-formats).

Przejdź do poniższego samouczka, aby dowiedzieć się o zbiorczym kopiowaniu danych z lokalizacji źródłowej do docelowej:

> [!div class="nextstepaction"]
>[Zbiorcze kopiowanie danych](tutorial-bulk-copy.md)
