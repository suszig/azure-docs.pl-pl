---
title: "Korzystanie z działań niestandardowych w potoku usługi Azure Data Factory"
description: "Informacje o sposobie tworzenia niestandardowych działań i używać ich w potoku fabryki danych Azure."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: shengc
ms.openlocfilehash: 9673c5ad3ae48f9f2b8a47165b739cc2431060ae
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Korzystanie z działań niestandardowych w potoku usługi Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](v1/data-factory-use-custom-activities.md)
> * [Wersja 2 — wersja zapoznawcza](transform-data-using-dotnet-custom-activity.md)

Istnieją dwa typy działań, które można używać w potoku fabryki danych Azure.

- [Działania przepływu danych](copy-activity-overview.md) do przenoszenia danych między [obsługiwane magazyny danych źródłowy i odbiorczy](copy-activity-overview.md#supported-data-stores-and-formats).
- [Działania przekształcania danych](transform-data.md) do przekształcania danych przy użyciu usług, takich jak Azure HDInsight, partii zadań Azure i usługi Azure Machine Learning obliczeniowe. 

Aby przenieść czy fabryki danych nie obsługuje lub aby proces/transformacji danych w taki sposób, który nie jest obsługiwany przez fabrykę danych, można utworzyć magazynu danych do/z danych **działania niestandardowe** z własnych przenoszenia danych lub logiki transformacji i użyj działania w potoku. Niestandardowe działanie jest uruchomione logiki niestandardowy kod **partii zadań Azure** puli maszyn wirtualnych.

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [działania DotNet (niestandardowy) w wersji 1](v1/data-factory-use-custom-activities.md).
 

Zobacz następujące tematy, jeśli jesteś nowym użytkownikiem usługi partia zadań Azure:

* [Podstawy usługi partia zadań Azure](../batch/batch-technical-overview.md) Omówienie usługi partia zadań Azure.
* [Nowy AzureRmBatchAccount](/powershell/module/azurerm.batch/New-AzureRmBatchAccount?view=azurermps-4.3.1) polecenia cmdlet, aby utworzyć konto partii zadań Azure (lub) [portalu Azure](../batch/batch-account-create-portal.md) Aby utworzyć konto partii zadań Azure za pomocą portalu Azure. Zobacz [przy użyciu programu PowerShell do zarządzania konta usługi partia zadań Azure](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) tematu, aby uzyskać szczegółowe instrukcje na temat używania polecenia cmdlet.
* [Nowy AzureBatchPool](/powershell/module/azurerm.batch/New-AzureBatchPool?view=azurermps-4.3.1) polecenia cmdlet, aby utworzyć pulę partii zadań Azure.

## <a name="azure-batch-linked-service"></a>Usługa Azure partii połączone 
Następujące JSON definiuje próbkę usługi partia zadań Azure połączone. Aby uzyskać więcej informacji, zobacz [obliczeniowe środowiskach obsługiwanych przez usługi fabryka danych Azure](compute-linked-services.md)

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
        "type": "AzureBatch",
        "typeProperties": {
            "accountName": "batchaccount",
            "accessKey": {
                "type": "SecureString",
                "value": "access key"
            },
            "batchUri": "https://batchaccount.region.batch.azure.com",
            "poolName": "poolname",
            "linkedServiceName": {
                "referenceName": "StorageLinkedService",
                "type": "LinkedServiceReference"
            }
        }
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

 Aby dowiedzieć się więcej na temat usługi partia zadań Azure połączone, zobacz [obliczeniowe połączonych usług](compute-linked-services.md) artykułu. 

## <a name="custom-activity"></a>Działanie niestandardowe

Poniższy fragment kodu JSON definiuje potoku z prostego działania niestandardowego. Definicja działania zawiera odwołanie do usługi partia zadań Azure połączone. 

```json
{
    "name": "MyCustomActivityPipeline",
    "properties": {
      "description": "Custom activity sample",
      "activities": [{
        "type": "Custom",
        "name": "MyCustomActivity",
        "linkedServiceName": {
          "referenceName": "AzureBatchLinkedService",
          "type": "LinkedServiceReference"
        },
        "typeProperties": {
          "command": "helloworld.exe",
          "folderPath": "customactv2/helloworld",
          "resourceLinkedService": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
          }
        }
      }]
    }
  }
```

W tym przykładzie helloworld.exe to aplikacja niestandardowe przechowywane w folderze customactv2/helloworld konta magazynu Azure używanego w resourceLinkedService. Działania niestandardowe przesyła ten niestandardową aplikację do wykonania w partii zadań Azure. Można zastąpić polecenie inną aplikację, która może być wykonane na elemencie docelowym System operacyjny węzłów puli usługi partia zadań Azure. 

W poniższej tabeli opisano nazwy i opisy właściwości, które są specyficzne dla tego działania. 

| Właściwość              | Opis                              | Wymagane |
| :-------------------- | :--------------------------------------- | :------- |
| name                  | Nazwa działania w potoku     | Tak      |
| description           | Tekst opisujący działanie robi.  | Nie       |
| type                  | Dla działania niestandardowego typu działania jest **niestandardowy**. | Tak      |
| linkedServiceName     | Połączonej usługi partia zadań Azure. Aby dowiedzieć się więcej na temat tej połączonej usługi, zobacz [obliczeniowe połączonych usług](compute-linked-services.md) artykułu.  | Tak      |
| polecenie               | Polecenia niestandardowych aplikacji do wykonania. Jeśli aplikacja jest już dostępne w węźle puli usługi partia zadań Azure, resourceLinkedService i folderPath można pominięte. Na przykład można określić polecenie, aby być `cmd /c dir`, które jest obsługiwane przez węzeł puli partii systemu Windows. | Tak      |
| resourceLinkedService | Azure połączonej usługi magazynu do konta magazynu, w którym przechowywana jest aplikacja niestandardowych | Nie       |
| folderPath            | Ścieżka do folderu niestandardowych aplikacji i wszystkich jego zależności | Nie       |
| referenceObjects      | Tablica istniejących połączonych usług i zestawów danych. Przywoływany połączonych usług i zestawy danych są przekazywane do niestandardowych aplikacji w formacie JSON, więc niestandardowy kod może odwoływać się zasobów z fabryką danych | Nie       |
| właściwości rozszerzone    | Właściwości zdefiniowane przez użytkownika, które mogą zostać przekazane do niestandardowych aplikacji w formacie JSON, więc niestandardowy kod może odwoływać się dodatkowe właściwości | Nie       |

## <a name="executing-commands"></a>Wykonywanie polecenia

Polecenie za pomocą działania niestandardowego można wykonać bezpośrednio. W poniższym przykładzie mamy Uruchom polecenie "echo hello world" w puli partii Azure węzły docelowe i wyświetla dane wyjściowe do strumienia wyjściowego stdout. 

  ```json
  {
    "name": "MyCustomActivity",
    "properties": {
      "description": "Custom activity sample",
      "activities": [{
        "type": "Custom",
        "name": "MyCustomActivity",
        "linkedServiceName": {
          "referenceName": "AzureBatchLinkedService",
          "type": "LinkedServiceReference"
        },
        "typeProperties": {
          "command": "cmd /c echo hello world"
        }
      }]
    }
  } 
  ```

## <a name="passing-objects-and-properties"></a>Przekazywanie obiektów i właściwości

W tym przykładzie pokazano, jak można użyć referenceObjects i właściwości rozszerzone do przekazania do aplikacji niestandardowych obiektów fabryki danych i właściwości zdefiniowane przez użytkownika. 


```json
{
  "name": "MyCustomActivityPipeline",
  "properties": {
    "description": "Custom activity sample",
    "activities": [{
      "type": "Custom",
      "name": "MyCustomActivity",
      "linkedServiceName": {
        "referenceName": "AzureBatchLinkedService",
        "type": "LinkedServiceReference"
      },
      "typeProperties": {
        "command": "SampleApp.exe",
        "folderPath": "customactv2/SampleApp",
        "resourceLinkedService": {
          "referenceName": "StorageLinkedService",
          "type": "LinkedServiceReference"
        },
        "referenceObjects": {
          "linkedServices": [{
            "referenceName": "AzureBatchLinkedService",
            "type": "LinkedServiceReference"
          }]
        },
        "extendedProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "aSampleSecureString"
            },
            "PropertyBagPropertyName1": "PropertyBagValue1",
            "propertyBagPropertyName2": "PropertyBagValue2",
            "dateTime1": "2015-04-12T12:13:14Z"              
        }
      }
    }]
  }
}
```

Po wykonaniu działania referenceObjects i właściwości rozszerzone są przechowywane w następujących plików, które są wdrażane do tego samego folderu wykonywania SampleApp.exe: 

- Activity.JSON

  Przechowuje właściwości rozszerzone i właściwości działania niestandardowego. 

- linkedServices.json

  Magazyny tablicę połączonych usług zdefiniowany we właściwości referenceObjects. 

- DataSets.JSON

  Magazyny tablicę zestawów danych zdefiniowany we właściwości referenceObjects. 

Następujący przykładowy kod pokazują, jak SampleApp.exe można uzyskać dostępu do wymaganych informacji z pliki w formacie JSON: 

```C#
using Newtonsoft.Json;
using System;
using System.IO;

namespace SampleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            //From Extend Properties
            dynamic activity = JsonConvert.DeserializeObject(File.ReadAllText("activity.json"));
            Console.WriteLine(activity.typeProperties.extendedProperties.connectionString.value);

            // From LinkedServices
            dynamic linkedServices = JsonConvert.DeserializeObject(File.ReadAllText("linkedServices.json"));
            Console.WriteLine(linkedServices[0].properties.typeProperties.connectionString.value);
        }
    }
}
```

## <a name="retrieve-execution-outputs"></a>Pobieranie danych wyjściowych wykonywania

  Można uruchomić potoku Uruchom za pomocą następującego polecenia programu PowerShell: 

  ```.powershell
  $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName $pipelineName
  ```
  Potok jest uruchomiona, można sprawdzić dane wyjściowe wykonania przy użyciu następujących poleceń: 

  ```.powershell
  while ($True) {
      $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

      if(!$result) {
          Write-Host "Waiting for pipeline to start..." -foregroundcolor "Yellow"
      }
      elseif (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
          Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
      }
      else {
          Write-Host "Pipeline '"$pipelineName"' run finished. Result:" -foregroundcolor "Yellow"
          $result
          break
      }
      ($result | Format-List | Out-String)
      Start-Sleep -Seconds 15
  }

  Write-Host "Activity `Output` section:" -foregroundcolor "Yellow"
  $result.Output -join "`r`n"

  Write-Host "Activity `Error` section:" -foregroundcolor "Yellow"
  $result.Error -join "`r`n"
  ```

  **Stdout** i **stderr** niestandardowych aplikacji są zapisywane w **adfjobs** kontenera w usługi połączonej magazynu Azure, określone podczas tworzenia połączonej usługi partia zadań Azure Usługa o identyfikatorze GUID zadania. Jak pokazano w poniższy fragment kodu, można uzyskać szczegółowe ścieżki z działania Uruchom danych wyjściowych: 

  ```shell
  Pipeline ' MyCustomActivity' run finished. Result:

  ResourceGroupName : resourcegroupname
  DataFactoryName   : datafactoryname
  ActivityName      : MyCustomActivity
  PipelineRunId     : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
  PipelineName      : MyCustomActivity
  Input             : {command}
  Output            : {exitcode, outputs, effectiveIntegrationRuntime}
  LinkedServiceName : 
  ActivityRunStart  : 10/5/2017 3:33:06 PM
  ActivityRunEnd    : 10/5/2017 3:33:28 PM
  DurationInMs      : 21203
  Status            : Succeeded
  Error             : {errorCode, message, failureType, target}

  Activity Output section:
  "exitcode": 0
  "outputs": [
    "https://shengcstorbatch.blob.core.windows.net/adfjobs/<GUID>/output/stdout.txt",
    "https://shengcstorbatch.blob.core.windows.net/adfjobs/<GUID>/output/stderr.txt"
  ]
  "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)"
  Activity Error section:
  "errorCode": ""
  "message": ""
  "failureType": ""
  "target": "MyCustomActivity"
  ```
Jeśli chcesz korzystać z zawartości stdout.txt działania podrzędne, można uzyskać ścieżki do pliku stdout.txt w wyrażeniu "@activity.output.outputs (MyCustomActivity) [0]". 

  > [!IMPORTANT]
  > - Activity.json linkedServices.json i datasets.json są przechowywane w folderze czasu wykonywania zadania łaźni. Na przykład activity.json, linkedServices.json i datasets.json są przechowywane w "https://adfv2storage.blob.core.windows.net/adfjobs/<GUID>/runtime/" ścieżki. Jeśli to konieczne, należy wyczyścić oddzielnie. 
  > - Do celów połączone usługi, środowiska uruchomieniowego integracji Self-Hosted, poufne informacje, takie jak klucze lub haseł, są szyfrowane przez środowisko uruchomieniowe integracji Self-Hosted do zapewnienia poświadczeń pozostaje klientów zdefiniowane prywatnego środowiska sieciowego. Gdy odwołuje się kod aplikacji niestandardowej w ten sposób można brakuje niektórych pól. W właściwości rozszerzone zamiast odwołanie do połączonej usługi, jeśli to konieczne, należy użyć elementu SecureString. 

## <a name="difference-between-custom-activity-in-azure-data-factory-v2-and-custom-dotnet-activity-in-azure-data-factory-v1"></a>Różnica między działań niestandardowych w fabryki danych Azure w wersji 2 i działania DotNet (niestandardowy) w wersji 1 z fabryki danych Azure 

  W wersji 1 fabryki danych Azure, zaimplementować kod działania DotNet (niestandardowy) przez utworzenie .net projektu biblioteki klas z klasy, która implementuje metody Execute interfejsu IDotNetActivity. Połączone usługi, zestawy danych i właściwości rozszerzone w ładunku JSON działania DotNet (niestandardowy) są przekazywane do metody wykonywania jako silne obiektów określonego typu. Aby uzyskać więcej informacji, zapoznaj się [DotNet (niestandardowy) w wersji 1](v1/data-factory-use-custom-activities.md). Z tego powodu, niestandardowy kod musi być napisana w .net Framework 4.5.2 i można wykonać na węzłach opartych na systemie Windows Azure puli partii. 

  W danych fabryki V2 niestandardowe działanie usługi Azure nie należy do implementacji interfejsu .net. Można teraz bezpośrednio uruchamianie poleceń i skryptów i uruchomić własny kod spełnione jako pliku wykonywalnego. To osiągnąć, określając właściwość polecenia razem z właściwością folderPath. Niestandardowe działania przekazuje plik wykonywalny i zależności w folderpath i wykonuje polecenie. 

  Połączone usługi, zestawów danych (zdefiniowany w referenceObjects) i rozszerzone właściwości zdefiniowane w ładunku JSON działania niestandardowe są dostępne w pliku wykonywalnego jako pliki w formacie JSON. Można uzyskać dostępu do wymaganych właściwości przy użyciu serializator JSON, jak pokazano w poprzednim przykładzie kodu SampleApp.exe. 

  Zmiany wprowadzone w działania niestandardowego V2 fabryki danych Azure są należy zająć się zapisanie logiki niestandardowego kodu w języku preferowanym i wykonać ich w systemach Windows i systemy operacyjne Linux obsługiwane przez partii zadań Azure. 

  W poniższej tabeli opisano różnice między działania niestandardowe V2 fabryki danych i V1 fabryki danych (niestandardowy) DotNet działania: 


|Różnice      |ADFv2 niestandardowego działania      |ADFv1 działania DotNet (niestandardowy)      |
| ---- | ---- | ---- |
|Jak zdefiniowano niestandardowej logiki      |Uruchamiając każdego pliku wykonywalnego (istniejące lub wdrażanie własnego pliku wykonywalnego)      |Zaimplementowanie .net biblioteki DLL      |
|Enviornment wykonania niestandardowej logiki      |Systemu Windows lub Linux      |Systemu Windows (.Net Framework 4.5.2)      |
|Wykonywanie skryptów      |Obsługuje bezpośrednio wykonywanie skryptów (na przykład "cmd /c echo hello world" na maszynie Wirtualnej z systemem Windows)      |Wymaga wdrożenia w środowisku .net biblioteki DLL      |
|Zestaw danych jest wymagane      |Optional (Opcjonalność)      |Wymagane do łańcucha działań i przekazywania informacji      |
|Przekazywania informacji z działania do niestandardowej logiki      |Za pomocą ReferenceObjects (LinkedServices i zbiory danych) i właściwości rozszerzone (właściwości niestandardowych) i      |Za pomocą właściwości rozszerzone (właściwości niestandardowych), dane wejściowe i wyjściowe zestawy danych      |
|Pobieranie informacji w niestandardowej logiki      |Przeanalizować activity.json, linkedServices.json i datasets.json przechowywane w folderze tego samego pliku wykonywalnego      |Za pomocą .net SDK (ramki 4.5.2 .net)      |
|Rejestrowanie      |Zapisuje dane bezpośrednio na STDOUT      |Rejestrator Implemeting w .net biblioteki DLL      |


  Jeśli masz istniejący kod .net napisane dla V1 działania DotNet (niestandardowy), należy zmodyfikować kod ich do pracy z V2 niestandardowe działanie z następującymi wytycznymi wysokiego poziomu:  

   - Zmień projekt z .net biblioteki klas w aplikacji konsoli. 
   - Uruchom aplikację za pomocą metody Main, metody Execute interfejsu IDotNetActivity nie jest już wymagane. 
   - Przeczytaj i analizy połączone usługi, zestawy danych i działania z serializator JSON, a nie jako silne obiektów określonego typu, a przekazania wartości właściwości wymaganych do logiki główny kodu niestandardowego. Zapoznaj się z powyższymi kodu SampleApp.exe jako próbka. 
   - Obiekt rejestratora nie jest już obsługiwana, executeable wyniki mogą być wydruku do konsoli i jest zapisywany w stdout.txt. 
   - Pakiet Microsoft.Azure.Management.DataFactories NuGet nie jest już wymagane. 
   - Kompilowanie kodu, Przekaż plik wykonywalny i zależności do magazynu Azure i określenia ścieżki we właściwości folderPath. 

Dla kompletnego przykładu sposób pełnego biblioteki DLL i potoku próbka opisanych w dokumencie V1 fabryki danych [skorzystać z działań niestandardowych w potoku fabryki danych Azure](https://docs.microsoft.com/en-us/azure/data-factory/v1/data-factory-use-custom-activities) można zmodyfikować w stylu działania niestandardowego V2 fabryki danych. Zapoznaj się [działania niestandardowe V2 fabryki danych przykładowych](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFv2CustomActivitySample). 

## <a name="auto-scaling-of-azure-batch"></a>Automatyczne skalowanie partii zadań Azure
Można również utworzyć puli partii zadań Azure z **skalowania automatycznego** funkcji. Na przykład można utworzyć puli partii zadań azure 0 dedykowanych maszyn wirtualnych i formuły skalowania automatycznego na podstawie liczby oczekujących zadań. 

Przykładowa formuła tutaj realizuje następujące zachowanie: podczas tworzenia puli, rozpoczyna się od 1 maszyny Wirtualnej. Metryka $PendingTasks definiuje liczbę zadań uruchomiona + aktywny (w kolejce) stanu.  Formuła znajduje średnią liczbę oczekujących zadań w ciągu ostatnich 180 sekund i odpowiednio ustawia TargetDedicated. Gwarantuje, że TargetDedicated nigdy nie wykracza poza 25 maszyn wirtualnych. Tak, jak nowe zadania są przesyłane, automatycznie zwiększa rozmiar puli i jako zakończenie zadania, maszyn wirtualnych stają się wolnego jeden po drugim i skalowanie automatyczne zmniejsza tych maszyn wirtualnych. startingNumberOfVMs i maxNumberofVMs można dostosować do własnych potrzeb.

Formuła skalowania automatycznego:

``` 
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

Zobacz [automatycznie skali obliczeniowe węzłów w puli partii zadań Azure](../batch/batch-automatic-scaling.md) szczegółowe informacje.

Jeśli w puli jest przy użyciu domyślnego [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), usługa partia zadań może zająć 15 do 30 minut, aby przygotować maszyny Wirtualnej przed uruchomieniem działania niestandardowego.  Jeśli pula używa innego autoScaleEvaluationInterval, usługa partia zadań może zająć autoScaleEvaluationInterval + 10 minut.


## <a name="next-steps"></a>Następne kroki
Zobacz następujące artykuły, które opisują sposób przekształcania danych w inny sposób: 

* [Działanie U-SQL](transform-data-using-data-lake-analytics.md)
* [Działanie gałęzi](transform-data-using-hadoop-hive.md)
* [Działanie pig](transform-data-using-hadoop-pig.md)
* [Działania MapReduce](transform-data-using-hadoop-map-reduce.md)
* [Działaniu przesyłania strumieniowego usługi Hadoop](transform-data-using-hadoop-streaming.md)
* [Działanie Spark](transform-data-using-spark.md)
* [Działanie wykonywania wsadowego usługi uczenie maszyny](transform-data-using-machine-learning.md)
* [Działania procedury składowanej](transform-data-using-stored-procedure.md)
