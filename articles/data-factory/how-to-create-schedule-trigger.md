---
title: "Jak utworzyć wyzwalaczy w fabryce danych Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć wyzwalacza w fabryce danych Azure z systemem potoku zgodnie z harmonogramem."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: shlo
ms.openlocfilehash: eed286c01604ab0e9ac2113d56cbce268503668d
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2017
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Jak utworzyć wyzwalacz uruchamia potoku zgodnie z harmonogramem
Ten artykuł zawiera kroki, aby utworzyć, uruchomić i monitorować wyzwalacz. Aby uzyskać informacje o pojęciach dotyczących wyzwalaczy, zobacz [potoku wykonywania i wyzwalaczy](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz artykuł z [wprowadzeniem do usługi Data Factory w wersji 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="use-azure-powershell"></a>Korzystanie z programu Azure PowerShell
W tej sekcji przedstawiono sposób tworzenia, uruchomić i monitorować wyzwalacz przy użyciu programu Azure PowerShell. Jeśli chcesz wyświetlić pracy tej próbki, najpierw przejść przez [Szybki Start: tworzenie fabryki danych przy użyciu programu Azure PowerShell](quickstart-create-data-factory-powershell.md). Następnie dodaj następujący kod do metody main, która tworzy i uruchamia wyzwalacz harmonogram, który jest uruchamiane co 15 minut. Wyzwalacz jest skojarzony z potokiem (**Adfv2QuickStartPipeline**) utworzonego w ramach szybkiego startu.

1. Utwórz plik JSON o nazwie MyTrigger.json w folderze C:\ADFv2QuickStartPSH\ o następującej treści: 

    > [!IMPORTANT]
    > Ustaw **startTime** bieżący czas UTC i **endTime** aby godzinę poza bieżący czas UTC razem przed zapisaniem pliku JSON.

    ```json   
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Minute",
                    "interval": 15,
                    "startTime": "2017-12-08T00:00:00",
                    "endTime": "2017-12-08T01:00:00"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "Adfv2QuickStartPipeline"
                    },
                    "parameters": {
                        "inputPath": "adftutorial/input",
                        "outputPath": "adftutorial/output"
                    }
                }
            ]
        }
    }
    ```
    
    We fragmencie JSON: 
    - **Typu** wyzwalacza jest ustawiony na wartość **ScheduleTrigger**. 
    - **Częstotliwość** ustawiono **minutę** i **interwał** ustawiono **15**. W związku z tym wyzwalacz uruchamia potoku co 15 minut od godziny rozpoczęcia i zakończenia. 
    - **EndTime** to jedna godzina po **startTime**, więc wyzwalacz uruchamia potoku 15 minut, 30 minut, a 45 minut po rozpoczęcia. Pamiętaj zaktualizować wartości startTime bieżący czas UTC i endTime na godzinę poza startTime.  
    - Wyzwalacz skojarzony z **Adfv2QuickStartPipeline** potoku. Aby skojarzyć wiele potoki z wyzwalaczem, Dodaj więcej **pipelineReference** sekcje. 
    - Potok w szybkiego startu przyjmuje dwa **parametry**. W związku z tym należy przekazać wartości tych parametrów z wyzwalacza. 
2. Utwórz wyzwalacz za pomocą **AzureRmDataFactoryV2Trigger zestaw** polecenia cmdlet.

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```
3. Upewnij się, że jest w stanie wyzwalacza **zatrzymane** za pomocą **Get-AzureRmDataFactoryV2Trigger** polecenia cmdlet. 

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" 
    ```
4. Uruchomić wyzwalacz za pomocą **Start AzureRmDataFactoryV2Trigger** polecenia cmdlet: 

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" 
    ```
5. Upewnij się, że jest w stanie wyzwalacza **uruchomiono** za pomocą **Get-AzureRmDataFactoryV2Trigger** polecenia cmdlet.

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" 
    ```
6.  Pobierz uruchamia wyzwalacz za pomocą programu PowerShell przy użyciu **Get-AzureRmDataFactoryV2TriggerRun** polecenia cmdlet. Aby uzyskać informacje na temat uruchamia wyzwalacz, uruchom następujące polecenie okresowo: aktualizacja **TriggerRunStartedAfter** i **TriggerRunStartedBefore** wartości są zgodne z wartościami w definicji wyzwalacza . 

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```

    Aby monitorować wyzwalacz uruchamia/potoku działa w portalu Azure, zobacz [uruchamia Monitor potoku](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="use-net-sdk"></a>Korzystanie z zestawu SDK dla platformy .NET
W tej sekcji przedstawiono sposób użycia zestawu .NET SDK do tworzenia, uruchomić i monitorować wyzwalacz. Jeśli chcesz wyświetlić ten kod działa, najpierw go za pośrednictwem [szybkiego startu dla tworzenie fabryki danych przy użyciu zestawu .NET SDK](quickstart-create-data-factory-dot-net.md). Następnie dodaj następujący kod do metody main, która tworzy i uruchamia wyzwalacz harmonogram, który jest uruchamiane co 15 minut. Wyzwalacz jest skojarzony z potokiem (**Adfv2QuickStartPipeline**) utworzonego w ramach szybkiego startu. 

```csharp
            //create the trigger
            Console.WriteLine("Creating the trigger");

            // set the start time to the current UTC time
            DateTime startTime = DateTime.UtcNow;

            // specify values for the inputPath and outputPath parameters
            Dictionary<string, object> pipelineParameters = new Dictionary<string, object>();
            pipelineParameters.Add("inputPath", "adftutorial/input");
            pipelineParameters.Add("outputPath", "adftutorial/output");

            // create a schedule trigger
            string triggerName = "MyTrigger";
            ScheduleTrigger myTrigger = new ScheduleTrigger()
            {
                Pipelines = new List<TriggerPipelineReference>()
                {
                    // associate the Adfv2QuickStartPipeline pipeline with the trigger
                    new TriggerPipelineReference()
                    {
                        PipelineReference = new PipelineReference(pipelineName),
                        Parameters = pipelineParameters,
                    }
                },
                Recurrence = new ScheduleTriggerRecurrence()
                {
                    // set the start time to current UTC time and the end time to be one hour after.
                    StartTime = startTime,
                    TimeZone = "UTC",
                    EndTime = startTime.AddHours(1),
                    Frequency = RecurrenceFrequency.Minute,
                    Interval = 15,
                }
            };

            // now, create the trigger by invoking CreateOrUpdate method. 
            TriggerResource triggerResource = new TriggerResource()
            {
                Properties = myTrigger
            };
            client.Triggers.CreateOrUpdate(resourceGroup, dataFactoryName, triggerName, triggerResource);

            //start the trigger
            Console.WriteLine("Starting the trigger");
            client.Triggers.Start(resourceGroup, dataFactoryName, triggerName);

```

Aby monitorować wyzwalacz uruchomienia, Dodaj następujący kod przed ostatniego `Console.WriteLine` instrukcji: 

```csharp
            // Check the trigger runs every 15 minutes
            Console.WriteLine("Trigger runs. You see the output every 15 minutes");

            for (int i = 0; i < 3; i++)
            {
                System.Threading.Thread.Sleep(TimeSpan.FromMinutes(15));
                List<TriggerRun> triggerRuns = client.Triggers.ListRuns(resourceGroup, dataFactoryName, triggerName, DateTime.UtcNow.AddMinutes(-15 * (i + 1)), DateTime.UtcNow.AddMinutes(2)).ToList();
                Console.WriteLine("{0} trigger runs found", triggerRuns.Count);

                foreach (TriggerRun run in triggerRuns)
                {
                    foreach (KeyValuePair<string, string> triggeredPipeline in run.TriggeredPipelines)
                    {
                        PipelineRun triggeredPipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, triggeredPipeline.Value);
                        Console.WriteLine("Pipeline run ID: {0}, Status: {1}", triggeredPipelineRun.RunId, triggeredPipelineRun.Status);
                        List<ActivityRun> runs = client.ActivityRuns.ListByPipelineRun(resourceGroup, dataFactoryName, triggeredPipelineRun.RunId, run.TriggerRunTimestamp.Value, run.TriggerRunTimestamp.Value.AddMinutes(20)).ToList();
                    }
                }
            }
```

Aby monitorować wyzwalacz uruchamia/potoku działa w portalu Azure, zobacz [uruchamia Monitor potoku](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="use-python-sdk"></a>Użyj zestawu SDK Python
W tej sekcji przedstawiono sposób użycia języka Python SDK do tworzenia, uruchomić i monitorować wyzwalacz. Jeśli chcesz wyświetlić ten kod działa, najpierw go za pośrednictwem [szybkiego startu dla tworzenie fabryki danych przy użyciu zestawu SDK Python](quickstart-create-data-factory-python.md). Następnie należy dodać następujący blok kodu po bloku kodu "Monitoruj potoku uruchamiania" w skrypcie języka python. Ten kod tworzy wyzwalacz harmonogram uruchamiany co 15 minut od określonego czasu rozpoczęcia i zakończenia. Zaktualizuj godzina_rozpoczęcia bieżący czas UTC i end_time na godzinę poza bieżący czas UTC. 

```python
    # Create a trigger
    tr_name = 'mytrigger'
    scheduler_recurrence = ScheduleTriggerRecurrence(frequency='Minute', interval='15',start_time='2017-12-12T04:00:00', end_time='2017-12-12T05:00:00', time_zone='UTC') 
    pipeline_parameters = {'inputPath':'adftutorial/input', 'outputPath':'adftutorial/output'}
    pipelines_to_run = []
    pipeline_reference = PipelineReference('copyPipeline')
    pipelines_to_run.append(TriggerPipelineReference(pipeline_reference, pipeline_parameters))
    tr_properties = ScheduleTrigger(description='My scheduler trigger', pipelines = pipelines_to_run, recurrence=scheduler_recurrence)    
    adf_client.triggers.create_or_update(rg_name, df_name, tr_name, tr_properties)

    # start the trigger
    adf_client.triggers.start(rg_name, df_name, tr_name)
```
Aby monitorować wyzwalacz uruchamia/potoku działa w portalu Azure, zobacz [uruchamia Monitor potoku](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="use-resource-manager-template"></a>Użyj Menedżera zasobów szablonu
Szablon usługi Azure Resource Manager służy do tworzenia wyzwalacza. Aby uzyskać instrukcje, zobacz [tworzenie fabryki danych Azure przy użyciu szablonu usługi Resource Manager](quickstart-create-data-factory-resource-manager-template.md).  

## <a name="pass-the-trigger-start-time-to-a-pipeline"></a>Przekaż wyzwalacza godziny rozpoczęcia dla potoku
W wersji 1 usługi fabryka danych Azure obsługiwane odczytu lub zapisu danych podzielonej na partycje przy użyciu SliceStart/SliceEnd/WindowStart/WindowEnd zmienne systemowe. W wersji 2 to zachowanie można osiągnąć za pomocą parametru potoku i czas/zaplanowana godzina rozpoczęcia tego wyzwalacza jako wartość parametru. W poniższym przykładzie zaplanowanego czasu wyzwalacza jest przekazywany jako wartość do potoku scheduledRunTime parametru. 

```json
"parameters": {
    "scheduledRunTime": "@trigger().scheduledTime"
}
```    
Aby uzyskać więcej informacji, zobacz [jak do odczytu lub zapisu na partycje danych](how-to-read-write-partitioned-data.md).

## <a name="next-steps"></a>Następne kroki
Aby uzyskać szczegółowe informacje dotyczące wyzwalaczy, zobacz [potoku wykonywania i wyzwalaczy](concepts-pipeline-execution-triggers.md#triggers).