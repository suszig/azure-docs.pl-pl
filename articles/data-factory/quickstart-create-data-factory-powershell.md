---
title: "Tworzenie usługi Azure Data Factory przy użyciu programu PowerShell | Microsoft Docs"
description: "Tworzenie usługi Azure Data Factory w celu skopiowania danych z jednej lokalizacji w usłudze Azure Blob Storage do innej lokalizacji w tej samej usłudze Blob Storage."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: hero-article
ms.date: 11/14/2017
ms.author: jingwang
ms.openlocfilehash: 63e4c654409651f6655da1bed6ab2f544cf024dd
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="create-an-azure-data-factory-and-pipeline-using-powershell"></a>Tworzenie potoku i fabryki danych Azure Data Factory przy użyciu programu PowerShell
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Wersja 2 — wersja zapoznawcza](quickstart-create-data-factory-powershell.md)

Ten samouczek szybki start opisuje sposób używania programu PowerShell w celu utworzenia usługi Azure Data Factory. Potok tworzony w tej fabryce danych kopiuje dane z jednej lokalizacji do innej lokalizacji w usłudze Azure Blob Storage. Aby zapoznać się z samouczkiem dotyczącym przekształcania danych za pomocą usługi Azure Data Factory, zobacz [Tutorial: Transform data using Spark (Samouczek: Przekształcanie danych przy użyciu usługi Spark)](transform-data-using-spark.md). 

Ten artykuł nie zawiera szczegółowego wprowadzenia do usługi Data Factory. Aby zapoznać się z wprowadzeniem do usługi Azure Data Factory, zobacz [Wprowadzenie do usługi Azure Data Factory](introduction.md).

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz artykuł z [wprowadzeniem do usługi Data Factory w wersji 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="prerequisites"></a>Wymagania wstępne

### <a name="azure-subscription"></a>Subskrypcja platformy Azure
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

### <a name="azure-storage-account"></a>Konto usługi Azure Storage
W tym samouczku używasz konta usługi Azure Storage ogólnego przeznaczenia (w szczególności usługi Blob Storage) jako magazynu danych: **źródłowego** oraz **docelowego/ujścia**. Jeśli nie masz konta usługi Azure Storage ogólnego przeznaczenia, zobacz [Tworzenie konta magazynu](../storage/common/storage-create-storage-account.md#create-a-storage-account), aby uzyskać informacje o jego tworzeniu. 

#### <a name="get-storage-account-name-and-account-key"></a>Pobieranie nazwy konta i klucza konta magazynu
W tym samouczku Szybki start używasz nazwy i klucza konta magazynu platformy Azure. Poniższa procedura obejmuje kroki pobierania nazwy i konta klucza magazynu. 

1. Otwórz przeglądarkę internetową i przejdź do witryny [Azure Portal](https://portal.azure.com). Zaloguj się za pomocą nazwy użytkownika i hasła platformy Azure. 
2. Kliknij pozycję **Więcej usług >** w menu po lewej stronie, odfiltruj przy użyciu słowa kluczowego **Storage** i wybierz pozycję **Konta usługi Storage**.

    ![Wyszukiwanie konta magazynu](media/quickstart-create-data-factory-powershell/search-storage-account.png)
3. Na liście kont magazynu odfiltruj konto magazynu (w razie potrzeby), a następnie wybierz **swoje konto magazynu**. 
4. Na stronie **Konto magazynu** wybierz pozycję **Klucze dostępu**.

    ![Pobieranie nazwy i klucza konta magazynu](media/quickstart-create-data-factory-powershell/storage-account-name-key.png)
5. Skopiuj wartości pól **Nazwa konta magazynu** i **klucz1** do schowka. Wklej je do Notatnika lub innego edytora i zapisz plik.  

#### <a name="create-input-folder-and-files"></a>Tworzenie plików i folderu wejściowego
W tej sekcji utworzysz kontener obiektów blob o nazwie adftutorial w magazynie obiektów blob Azure Blob Storage. Następnie utwórz folder o nazwie: input w kontenerze i przekaż przykładowy plik do folderu input. 

1. Zainstaluj [Eksploratora usługi Azure Storage](https://azure.microsoft.com/features/storage-explorer/), jeśli nie ma go na maszynie. 
2. Uruchom **Eksploratora usługi Microsoft Azure Storage** na swojej maszynie.   
3. W oknie **łączenia z usługą Azure Storage** wybierz pozycję **Użyj nazwy i klucza konta magazynu** i kliknij pozycję **Dalej**. Jeśli nie widzisz okna **łączenia z usługą Azure Storage**, kliknij prawym przyciskiem myszy pozycję **Konta magazynu** w widoku drzewa, a następnie kliknij pozycję **Połącz z usługą Azure Storage**. 

    ![Łączenie z usługą Azure Storage](media/quickstart-create-data-factory-powershell/storage-explorer-connect-azure-storage.png)
4. W oknie **dołączania przy użyciu nazwy i klucza** wklej **nazwę konta** i **klucz konta** zapisane w poprzednim kroku. Następnie kliknij przycisk **Dalej**. 
5. W oknie **Podsumowanie połączenia** kliknij pozycję **Połącz**.
6. Sprawdź, czy widzisz swoje konto magazynu w widoku drzewa w obszarze **(Lokalny i dołączony)** -> **Konta magazynu**. 
7. Rozwiń węzeł **Kontenery obiektów blob** i upewnij się, że kontener obiektów blob **adftutorial** nie istnieje. Jeśli już istnieje, możesz pominąć kolejne kroki tworzenia kontenera. 
8. Kliknij prawym przyciskiem myszy pozycję **Kontenery obiektów blob** i wybierz pozycję **Utwórz kontener obiektów blob**.

    ![Tworzenie kontenera obiektów blob](media/quickstart-create-data-factory-powershell/stroage-explorer-create-blob-container-menu.png)
9. Wprowadź ciąg **adftutorial** jako nazwę i naciśnij klawisz **ENTER**. 
10. Upewnij się, że kontener **adftutorial** został wybrany w widoku drzewa. 
11. Kliknij pozycję **Nowy folder** na pasku narzędzi. 

    ![Przycisk tworzenia folderu](media/quickstart-create-data-factory-powershell/stroage-explorer-new-folder-button.png)
12. W oknie **Tworzenie nowego katalogu wirtualnego** wprowadź ciąg **input** jako **nazwę** i kliknij przycisk **OK**. 

    ![Okno dialogowe tworzenia katalogu](media/quickstart-create-data-factory-powershell/storage-explorer-create-new-directory-dialog.png)
13. Uruchom **Notatnik** i utwórz plik o nazwie **emp.txt** z następującą zawartością: 
    
    ```
    John, Doe
    Jane, Doe
    ```    
    Zapisz go w folderze **c:\ADFv2QuickStartPSH**: (utwórz folder **ADFv2QuickStartPSH**, jeśli jeszcze nie istnieje). 
14. Kliknij przycisk **Przekaż** na pasku narzędzi i wybierz pozycję **Przekaż pliki**. 

    ![Przycisk Przekaż](media/quickstart-create-data-factory-powershell/storage-explorer-upload-button.png)
15. W oknie **Przekazywanie plików** dla obszaru **Pliki** wybierz pozycję `...`. 
16. W oknie **wybierania folderu do przekazania** przejdź do folderu z plikiem **emp.txt** i wybierz ten plik. 

    ![Okno dialogowe Przekazywanie plików](media/quickstart-create-data-factory-powershell/storage-explorer-upload-files-dialog.png)
17. W oknie **Przekazywanie plików** kliknij pozycję **Przekaż**. 

### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-azure-powershell"></a>Instalowanie programu Azure PowerShell
Zainstaluj najnowszy program Azure PowerShell, jeśli nie masz go na swojej maszynie. 

1. W przeglądarce internetowej przejdź do strony [plików do pobrania zestawu Azure SDK i innych zestawów SDK](https://azure.microsoft.com/downloads/). 
2. Kliknij pozycję **Wersja instalacyjna dla systemu Windows** w sekcji **Narzędzia wiersza polecenia** -> **PowerShell**. 
3. Aby zainstalować program Azure PowerShell, uruchom plik **MSI**. 

Aby uzyskać szczegółowe informacje, zobacz [How to install and configure Azure PowerShell (Jak zainstalować i skonfigurować program Azure PowerShell)](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-azure-powershell"></a>Logowanie do programu Azure PowerShell
Uruchom program **PowerShell** na maszynie. Nie zamykaj programu Azure PowerShell aż do końca tego samouczka Szybki start. Jeśli go zamkniesz i otworzysz ponownie, musisz uruchomić te polecenia jeszcze raz.

1. Uruchom poniższe polecenie i wprowadź nazwę użytkownika platformy Azure oraz hasło, których używasz do logowania się w witrynie Azure Portal:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Jeśli masz wiele subskrypcji platformy Azure, uruchom poniższe polecenie, aby wyświetlić wszystkie subskrypcje dla tego konta:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Uruchom poniższe polecenie, aby wybrać subskrypcję, z którą chcesz pracować. Zastąp parametr **SubscriptionId** identyfikatorem Twojej subskrypcji platformy Azure:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Tworzenie fabryki danych
1. Zdefiniuj zmienną nazwy grupy zasobów, której użyjesz później w poleceniach programu PowerShell. Skopiuj poniższy tekst polecenia do programu PowerShell, podaj nazwę [grupy zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) w podwójnych cudzysłowach, a następnie uruchom polecenie. 
   
     ```powershell
    $resourceGroupName = "<Specify a name for the Azure resource group>";
    ```
2. Zdefiniuj zmienną nazwy fabryki danych, której możesz użyć później w poleceniach programu PowerShell. 

    ```powershell
    $dataFactoryName = "<Specify a name for the data factory. It must be globally unique.>";
    ```
1. Zdefiniuj zmienną lokalizacji fabryki danych: 

    ```powershell
    $location = "East US"
    ```
4. Aby utworzyć grupę zasobów platformy Azure, uruchom następujące polecenie: 

    ```powershell
    New-AzureRmResourceGroup $resourceGroupName $location
    ``` 
    Jeśli grupa zasobów już istnieje, możesz zrezygnować z jej zastąpienia. Przypisz inną wartość do zmiennej `$resourceGroupName` i spróbuj ponownie. Jeśli chcesz udostępnić grupę zasobów innym użytkownikom, przejdź do następnego kroku. 
5. Aby utworzyć fabrykę danych, uruchom następujące polecenie cmdlet **Set-AzureRmDataFactoryV2**: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

Pamiętaj o następujących kwestiach:

* Nazwa fabryki danych Azure musi być globalnie unikatowa. Jeśli zostanie wyświetlony następujący błąd, zmień nazwę i spróbuj ponownie.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```

* Aby tworzyć wystąpienia usługi Data Factory, musisz być **współautorem** lub **administratorem** subskrypcji platformy Azure.
* Obecnie usługa Data Factory w wersji 2 umożliwia tworzenie fabryk danych tylko w regionach Wschodnie stany USA, Wschodnie stany USA 2 i Europa Zachodnia. Magazyny danych (Azure Storage, Azure SQL Database itp.) i jednostki obliczeniowe (HDInsight itp.) używane przez fabrykę danych mogą mieścić się w innych regionach.

## <a name="create-a-linked-service"></a>Tworzenie usługi połączonej

Utwórz połączone usługi w fabryce danych w celu połączenia swoich magazynów danych i usług obliczeniowych z fabryką danych. W tym samouczku Szybki start musisz utworzyć tylko jedną połączoną usługę Azure Storage, która będzie używana zarówno jako źródło, jak i ujście magazynu o nazwie „AzureStorageLinkedService” w tym przykładzie.

1. Utwórz plik JSON o nazwie **AzureStorageLinkedService.json** w folderze **C:\ADFv2QuiclStartPSH** o następującej zawartości: (Utwórz folder ADFv2QuickStartPSH, jeśli jeszcze nie istnieje). 

    > [!IMPORTANT]
    > Przed zapisaniem pliku zastąp wartości &lt;accountName&gt; i &lt;accountKey&gt; nazwą i kluczem konta usługi Azure Storage.

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

2. W programie **Azure PowerShell** przejdź do folderu **ADFv2QuickStartPSH**.

3. Uruchom polecenie cmdlet **Set-AzureRmDataFactoryV2LinkedService**, aby utworzyć połączoną usługę: **AzureStorageLinkedService**. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureStorageLinkedService" -DefinitionFile ".\AzureStorageLinkedService.json"
    ```

    Oto przykładowe dane wyjściowe:

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

## <a name="create-a-dataset"></a>Tworzenie zestawu danych

Zdefiniuj zestaw danych, który reprezentuje dane do skopiowania ze źródła do ujścia. W tym przykładzie ten zestaw danych obiektu blob odwołuje się do połączonej usługi Azure Storage utworzonej w poprzednim kroku. Zestaw danych przyjmuje parametr, którego wartość jest ustawiana w działaniu wykorzystującym zestaw danych. Parametr służy do tworzenia wartości **folderPath** wskazującej miejsce, gdzie znajdują się/są przechowywane dane.

1. Utwórz plik JSON o nazwie **BlobDataset.json** w folderze **C:\ADFv2QuickStartPSH** o następującej zawartości:

    ```json
    {
        "name": "BlobDataset",
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": {
                    "value": "@{dataset().path}",
                    "type": "Expression"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            },
            "parameters": {
                "path": {
                    "type": "String"
                }
            }
        }
    }
    ```

2. Aby utworzyć zestaw danych **BlobDataset**, uruchom polecenie cmdlet **Set-AzureRmDataFactoryV2Dataset**.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "BlobDataset" -DefinitionFile ".\BlobDataset.json"
    ```

    Oto przykładowe dane wyjściowe:

    ```
    DatasetName       : BlobDataset
    ResourceGroupName : <resourceGroupname>
    DataFactoryName   : <dataFactoryName>
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-a-pipeline"></a>Tworzenie potoku
  
W niniejszym przykładzie ten potok zawiera jedno działanie i pobiera dwa parametry — ścieżkę wejściowego obiektu blob i ścieżkę wyjściowego obiektu blob. Wartości tych parametrów są ustawiane w chwili wyzwolenia/uruchomienia potoku. Działanie kopiowania używa tego samego zestawu danych obiektu blob, który został utworzony w poprzednim kroku jako wejście i wyjście. W przypadku użycia zestawu danych jako zestawu danych wejściowych określana jest ścieżka wejściowa. Natomiast w przypadku użycia zestawu danych jako zestawu danych wyjściowych określana jest ścieżka wyjściowa. 

1. Utwórz plik JSON o nazwie **Adfv2QuickStartPipeline.json** w folderze **C:\Adfv2QuickStartPSH** o następującej zawartości:

    ```json
    {
        "name": "Adfv2QuickStartPipeline",
        "properties": {
            "activities": [
                {
                    "name": "CopyFromBlobToBlob",
                    "type": "Copy",
                    "inputs": [
                        {
                            "referenceName": "BlobDataset",
                            "parameters": {
                                "path": "@pipeline().parameters.inputPath"
                            },
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "BlobDataset",
                            "parameters": {
                                "path": "@pipeline().parameters.outputPath"
                            },
                            "type": "DatasetReference"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    }
                }
            ],
            "parameters": {
                "inputPath": {
                    "type": "String"
                },
                "outputPath": {
                    "type": "String"
                }
            }
        }
    }
    ```

2. W celu utworzenia potoku: **Adfv2QuickStartPipeline** uruchom polecenie cmdlet **Set-AzureRmDataFactoryV2Pipeline**.

    ```powershell
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "Adfv2QuickStartPipeline" -DefinitionFile ".\Adfv2QuickStartPipeline.json"
    ```

    Oto przykładowe dane wyjściowe:

    ```
    PipelineName      : Adfv2QuickStartPipeline
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Activities        : {CopyFromBlobToBlob}
    Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
    ```

## <a name="create-a-pipeline-run"></a>Tworzenie uruchomienia potoku

W tym kroku ustawisz wartości parametrów potoku: **inputPath** i **outputPath** przy użyciu rzeczywistych wartości ścieżki źródła i ujścia obiektu blob. Następnie utworzysz uruchomienie potoku za pomocą tych argumentów. 

1. Utwórz plik JSON o nazwie **PipelineParameters.json** w folderze **C:\ADFv2QuickStartPSH** o następującej zawartości:

    Zastąp wartość **inputPath** i **outputPath** swoją ścieżką źródła i ujścia obiektu blob, jeśli używasz innych kontenerów i folderów.

    ```json
    {
        "inputPath": "adftutorial/input",
        "outputPath": "adftutorial/output"
    }
    ```

2. Uruchom polecenie cmdlet **Invoke-AzureRmDataFactoryV2Pipeline**, aby utworzyć uruchomienie potoku i przekazać wartości parametrów. Umożliwia to również przechwycenie identyfikatora uruchomienia potoku w celu monitorowania w przyszłości.

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
    ```

## <a name="monitor-a-pipeline-run"></a>Monitorowanie uruchomienia potoku

1. Uruchom następujący skrypt, aby stale sprawdzać stan uruchomienia potoku do momentu zakończenia kopiowania danych.

    ```powershell
    while ($True) {
        $run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $resourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $runId

        if ($run) {
            if ($run.Status -ne 'InProgress') {
                Write-Host "Pipeline run finished. The status is: " $run.Status -foregroundcolor "Yellow"
                $run
                break
            }
            Write-Host  "Pipeline is running...status: InProgress" -foregroundcolor "Yellow"
        }

        Start-Sleep -Seconds 30
    }
    ```

    Oto przykładowe dane wyjściowe uruchomienia potoku:

    ```
    Pipeline is running...status: InProgress
    Pipeline run finished. The status is:  Succeeded
    
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : SPTestFactory0928
    RunId             : 0000000000-0000-0000-0000-0000000000000
    PipelineName      : Adfv2QuickStartPipeline
    LastUpdated       : 9/28/2017 8:28:38 PM
    Parameters        : {[inputPath, adftutorial/input], [outputPath, adftutorial/output]}
    RunStart          : 9/28/2017 8:28:14 PM
    RunEnd            : 9/28/2017 8:28:38 PM
    DurationInMs      : 24151
    Status            : Succeeded
    Message           :
    ```

2. Uruchom następujący skrypt, aby pobrać szczegóły uruchomienia działania kopiowania, na przykład rozmiar odczytanych/zapisanych danych.

    ```powershell
    Write-Host "Activity run details:" -foregroundcolor "Yellow"
    $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $result
    
    Write-Host "Activity 'Output' section:" -foregroundcolor "Yellow"
    $result.Output -join "`r`n"
    
    Write-Host "\nActivity 'Error' section:" -foregroundcolor "Yellow"
    $result.Error -join "`r`n"
    ```
3. Upewnij się, że wyświetlone dane wyjściowe są podobne do następujących przykładowych danych wyjściowych uruchomienia działania:

    ```json
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : SPTestFactory0928
    ActivityName      : CopyFromBlobToBlob
    PipelineRunId     : 00000000000-0000-0000-0000-000000000000
    PipelineName      : Adfv2QuickStartPipeline
    Input             : {source, sink}
    Output            : {dataRead, dataWritten, copyDuration, throughput...}
    LinkedServiceName :
    ActivityRunStart  : 9/28/2017 8:28:18 PM
    ActivityRunEnd    : 9/28/2017 8:28:36 PM
    DurationInMs      : 18095
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    Activity 'Output' section:
    "dataRead": 38
    "dataWritten": 38
    "copyDuration": 7
    "throughput": 0.01
    "errors": []
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)"
    "usedCloudDataMovementUnits": 2
    "billedDuration": 14
    ```

## <a name="verify-the-output"></a>Sprawdzanie danych wyjściowych
Potok automatycznie tworzy folder wyjściowy w kontenerze obiektów blob adftutorial. Następnie kopiuje plik emp.txt z folderu wejściowego do folderu wyjściowego. Za pomocą [Eksploratora usługi Azure Storage](https://azure.microsoft.com/features/storage-explorer/) sprawdź, czy obiekty blob ze ścieżki inputBlobPath zostały skopiowane do ścieżki outputBlobPath. 

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Zasoby, które zostały utworzone w ramach tego przewodnika Szybki start, możesz wyczyścić na dwa sposoby. Możesz usunąć [grupę zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) zawierającą wszystkie zasoby w tej grupie. Jeśli chcesz zachować inne zasoby bez zmian, usuń tylko fabrykę danych utworzoną w tym samouczku.

Uruchom poniższe polecenie, aby usunąć całą grupę zasobów: 
```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

Uruchom następujące polecenie, aby usunąć tylko fabrykę danych: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="next-steps"></a>Następne kroki
Potok w tym przykładzie kopiuje dane z jednej lokalizacji do innej lokalizacji w usłudze Azure Blob Storage. Zapoznaj się z [samouczkami](tutorial-copy-data-dot-net.md), aby dowiedzieć się więcej o korzystaniu z usługi Data Factory w dalszych scenariuszach. 
