---
title: "Wywołanie pakietów SSIS przy użyciu fabryki danych Azure - działania dotyczącego procedury składowanej | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak można wywołać z potoku fabryki danych Azure za pomocą działania dotyczącego procedury składowanej pakiet SQL Server Integration Services (SSIS)."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: article
ms.date: 12/07/2017
ms.author: jingwang
ms.openlocfilehash: 8aabe45a1627b1a897ca9fe4bda581c7a3f6bc03
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="invoke-an-ssis-package-using-stored-procedure-activity-in-azure-data-factory"></a>Wywołanie pakietów SSIS za pomocą działania procedury składowanej w fabryce danych Azure
W tym artykule opisano sposób wywołania pakietów SSIS z potoku fabryki danych Azure za pomocą działania procedury składowanej. 

> [!NOTE]
> Ten artykuł dotyczy wersji 1 usługi fabryka danych, która jest ogólnie dostępna. Jeśli używasz wersji 2 usługi fabryka danych, który znajduje się w publicznej wersji zapoznawczej, zobacz [pakietów SSIS wywołać przy użyciu działania procedury składowanej w wersji 2](../how-to-invoke-ssis-package-stored-procedure-activity.md).

## <a name="prerequisites"></a>Wymagania wstępne

### <a name="azure-sql-database"></a>Azure SQL Database 
Wskazówki w tym artykule używa bazy danych Azure SQL katalogiem usług SSIS. Umożliwia także Azure zarządzane wystąpienia SQL (wersja zapoznawcza prywatnych).

### <a name="create-an-azure-ssis-integration-runtime"></a>Tworzenie środowiska Azure SSIS Integration Runtime
Tworzenie środowiska uruchomieniowego integracji usług SSIS Azure, jeśli nie masz, wykonując instrukcje krok po kroku w [samouczek: pakiety wdrażania usług SSIS](../tutorial-deploy-ssis-packages-azure.md). Należy utworzyć fabryki danych w wersji 2, aby utworzyć środowiska uruchomieniowego integracji usług SSIS Azure. 

### <a name="azure-powershell"></a>Azure PowerShell
Zainstaluj najnowsze moduły programu Azure PowerShell, wykonując instrukcje podane w temacie [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="create-a-data-factory"></a>Tworzenie fabryki danych
Poniższa procedura zawiera kroki, aby utworzyć fabryki danych. Możesz utworzyć potok z działaniem procedury składowanej w tej fabryce danych. Działania procedury składowanej wykonuje procedurę przechowywaną w bazie danych usług SSIS do uruchamiania pakietu SSIS.

1. Zdefiniuj zmienną nazwy grupy zasobów, której użyjesz później w poleceniach programu PowerShell. Skopiuj poniższy tekst polecenia do programu PowerShell, podaj nazwę [grupy zasobów platformy Azure](../../azure-resource-manager/resource-group-overview.md) w podwójnych cudzysłowach, a następnie uruchom polecenie. Na przykład: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup";
    ```

    Jeśli grupa zasobów już istnieje, możesz zrezygnować z jej zastąpienia. Przypisz inną wartość do zmiennej `$ResourceGroupName` i ponownie uruchom polecenie.
2. Aby utworzyć grupę zasobów platformy Azure, uruchom następujące polecenie: 

    ```powershell
    $ResGrp = New-AzureRmResourceGroup $resourceGroupName -location 'eastus'
    ``` 
    Jeśli grupa zasobów już istnieje, możesz zrezygnować z jej zastąpienia. Przypisz inną wartość do zmiennej `$ResourceGroupName` i ponownie uruchom polecenie. 
3. Zdefiniuj zmienną nazwy fabryki danych. 

    > [!IMPORTANT]
    >  Zaktualizuj nazwę fabryki danych, aby była unikatowa w skali globalnej, 

    ```powershell
    $DataFactoryName = "ADFTutorialFactory";
    ```

5. Można utworzyć fabryki danych, uruchom następujące polecenie **AzureRmDataFactory nowy** polecenia cmdlet, za pomocą właściwości lokalizacji i grupy zasobów o nazwie zmiennej $ResGrp: 
    
    ```powershell       
    $df = New-AzureRmDataFactory -ResourceGroupName $ResourceGroupName -Name $dataFactoryName -Location "East US"
    ```

Pamiętaj o następujących kwestiach:

* Nazwa fabryki danych Azure musi być globalnie unikatowa. Jeśli zostanie wyświetlony następujący błąd, zmień nazwę i spróbuj ponownie.

    ```
    The specified Data Factory name 'ADFTutorialFactory' is already in use. Data Factory names must be globally unique.
    ```
* Aby utworzyć wystąpienia usługi Data Factory, konto użytkownika używane do logowania się na platformie Azure musi być członkiem roli **współautora** lub **właściciela** albo **administratorem** subskrypcji platformy Azure.

### <a name="create-an-azure-sql-database-linked-service"></a>Tworzenie połączonej usługi Azure SQL Database
Tworzenie połączonej usługi, aby połączyć bazy danych Azure SQL obsługującego katalogu SSIS z fabryką danych. Fabryka danych używa informacji dostępnych w tej połączonej usługi do łączenia z bazą danych usług SSIS i wykonuje procedurę składowaną do uruchamiania pakietów SSIS. 

1. Utwórz plik JSON o nazwie **AzureSqlDatabaseLinkedService.json** w **C:\ADF\RunSSISPackage** folderu o następującej treści: 

    > [!IMPORTANT]
    > Zastąp &lt;servername&gt;, &lt;username&gt;@&lt;servername&gt; i &lt;hasło&gt; z wartościami bazy danych SQL Azure przed Zapisywanie pliku.

    ```json
    {
        "name": "AzureSqlDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=SSISDB;User ID=<username>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
        }
    ```
2. W **programu Azure PowerShell**, przełącz się do **C:\ADF\RunSSISPackage** folderu.
3. Uruchom **AzureRmDataFactoryLinkedService nowy** , aby utworzyć połączonej usługi: **AzureSqlDatabaseLinkedService**. 

    ```powershell
    New-AzureRmDataFactoryLinkedService $df -File ".\AzureSqlDatabaseLinkedService.json"
    ```

## <a name="create-an-output-dataset"></a>Tworzenie wyjściowego zestawu danych
Ten zestaw danych wyjściowych jest fikcyjny zestawu danych, który dyski harmonogram potoku. Należy zauważyć, że ustawiono częstotliwość godzinę i interwał jest ustawiona na 1. W związku z tym potoku uruchomieniu godzinę w potoku godziny rozpoczęcia i zakończenia. 

1. Utwórz plik OuputDataset.json o następującej treści: 
    
    ```json
    {
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": { },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
2. Uruchom **AzureRmDataFactoryDataset nowy** polecenia cmdlet można utworzyć zestawu danych. 

    ```powershell
    New-AzureRmDataFactoryDataset $df -File ".\OutputDataset.json"
    ```

## <a name="create-a-pipeline-with-stored-procedure-activity"></a>Utworzyć potok z działania procedury składowanej 
W tym kroku możesz utworzyć potok z działania procedury składowanej. Działanie wywołuje procedury przechowywanej sp_executesql, aby uruchomić pakiet SSIS. 

1. Utwórz plik JSON o nazwie **MyPipeline.json** w **C:\ADF\RunSSISPackage** folderu o następującej treści:

    > [!IMPORTANT]
    > Zastąp &lt;nazwa folderu&gt;, &lt;Nazwa projektu&gt;, &lt;nazwy pakietu&gt; z nazwami folderu projektu i pakietu w katalogu usług SSIS przed zapisaniem pliku.

    ```json
    {
        "name": "MyPipeline",
        "properties": {
            "activities": [{
                "name": "SprocActivitySample",
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_executesql",
                    "storedProcedureParameters": {
                        "stmt": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<folder name>', @project_name=N'<project name>', @package_name=N'<package name>', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
                    }
                },
                "outputs": [{
                    "name": "sprocsampleout"
                }],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }],
            "start": "2017-10-01T00:00:00Z",
            "end": "2017-10-01T05:00:00Z",
            "isPaused": false
        }
    }    
    ```

2. Można utworzyć potoku: **RunSSISPackagePipeline**Uruchom **AzureRmDataFactoryPipeline nowy** polecenia cmdlet.

    ```powershell
    $DFPipeLine = New-AzureRmDataFactoryPipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

## <a name="monitor-the-pipeline-run"></a>Monitorowanie działania potoku

2. Uruchom **Get AzureRmDataFactorySlice** Aby uzyskać szczegółowe informacje o wszystkich wycinków wyjściowego zestawu danych **, czyli tabeli wyjściowej potoku.

    ```PowerShell
    Get-AzureRmDataFactorySlice $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```
    Zauważ, że określana tutaj właściwość StartDateTime oznacza taki sam czas rozpoczęcia jak określony w potoku JSON. 
3. Uruchom polecenie **Get-AzureRmDataFactoryRun**, aby uzyskać szczegółowe informacje o uruchomieniach działania dla określonego wycinka.

    ```PowerShell
    Get-AzureRmDataFactoryRun $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```

    Możesz kontynuować uruchamianie tego polecenia cmdlet do momentu, gdy wycinek będzie widoczny w stanie **Gotowe** lub **Niepowodzenie**. 

    Można uruchomić następujące zapytanie w bazie danych usług SSIS znajdujących się na serwerze Azure SQL, aby sprawdzić, czy pakiet wykonywane. 

    ```sql
    select * from catalog.executions
    ```

## <a name="next-steps"></a>Kolejne kroki
Aby uzyskać szczegółowe informacje dotyczące działania procedury składowanej, zobacz [działania procedura składowana](data-factory-stored-proc-activity.md) artykułu.

