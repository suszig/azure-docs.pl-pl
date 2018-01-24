---
title: Tworzenie harmonogramu wyzwalaczy w fabryce danych Azure | Dokumentacja firmy Microsoft
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
ms.date: 01/23/2018
ms.author: shlo
ms.openlocfilehash: 51e2dddbe66ca372d89fc8efeb24bdab9fe6a442
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Utwórz wyzwalacz uruchamia potoku zgodnie z harmonogramem
Ten artykuł zawiera informacje o wyzwalacza harmonogramu oraz instrukcje umożliwiające tworzenie, uruchomić i monitorować wyzwalacz harmonogramu. Dla innych typów wyzwalaczy, zobacz [potoku wykonywania i wyzwalaczy](concepts-pipeline-execution-triggers.md).

Podczas tworzenia harmonogramu wyzwalacza należy określić harmonogram (rozpoczęcie cyklu, końcowa data itp.) dla wyzwalacza i skojarz z potokiem. Między potokami i wyzwalaczami występuje relacja wiele-do-wielu. Wiele wyzwalaczy może uruchomić jeden potok. Jeden wyzwalacz może uruchamiać wiele potoków.

> [!NOTE]
> Ten artykuł dotyczy usługi fabryka danych Azure w wersji 2, który jest obecnie w wersji zapoznawczej. Jeśli używasz usługi fabryka danych Azure w wersji 1, która jest ogólnie dostępna (GA), zobacz [Rozpoczynanie pracy z fabryką danych Azure w wersji 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Poniższe sekcje zawierają kroki, aby utworzyć harmonogram wyzwalacza w inny sposób. 

## <a name="data-factory-ui"></a>Fabryka danych interfejsu użytkownika
Można utworzyć **wyzwalacza harmonogram** można zaplanować potoku do uruchamiania okresowo (co godzinę, codziennie, itp.). 

> [!NOTE]
> Aby uzyskać pełny Przewodnik tworzenia potoku i wyzwalacz harmonogram kojarzenie wyzwalacza potoku i uruchamiania i monitorowania potoku, zobacz [Szybki Start: tworzenie fabryki danych za pomocą interfejsu użytkownika z fabryki danych](quickstart-create-data-factory-portal.md).

1. Przejdź do karty **Edycja**. 

    ![Przechodzenie do karty Edycja](./media/how-to-create-schedule-trigger/switch-edit-tab.png)
1. Kliknij pozycję **Wyzwalacz** w menu, a następnie kliknij pozycję **Nowy/Edytuj**. 

    ![Menu Nowy wyzwalacz](./media/how-to-create-schedule-trigger/new-trigger-menu.png)
2. Na stronie **Dodawanie wyzwalaczy** kliknij pozycję **Wybierz wyzwalacz...** , a następnie kliknij przycisk **Nowy**. 

    ![Dodawanie wyzwalaczy — nowy wyzwalacz](./media/how-to-create-schedule-trigger/add-trigger-new-button.png)
3. W **nowego wyzwalacza** wykonaj następujące czynności: 

    1. Upewnij się, że **harmonogram** został wybrany do **typu**. 
    2. Określ datę i godzinę rozpoczęcia wyzwalacza dla **Start Data (UTC)**. Domyślnie jest ustawiona do bieżącej daty/godziny. 
    3. Określ **cyklu** wyzwalacza. Wybierz jedną z wartości z listy rozwijanej (co minutę, co godzinę, codziennie, co tydzień i miesięczne). Mnożnik należy wprowadzić w polu tekstowym. Na przykład, jeśli chcesz trigger, aby uruchomić jeden raz dla co 15 minut, wybraniu **co minutę**, a następnie wprowadź **15** w polu tekstowym. 
    4. Dla **zakończenia** pola, jeśli nie chcesz określać daty/godziny zakończenia dla wyzwalacza, wybierz opcję **End nie**. Aby określić Data i godzina zakończenia, zaznacz **na daty**i określ daty/godziny zakończenia i kliknij **Zastosuj**. Z każdym uruchomieniem potoku wiąże się koszt. Podczas testowania, może zajść potrzeba upewnij się, że potok zostanie wywołany tylko kilka razy. Upewnij się również, że okres między czasem publikowania i czasem zakończenia będzie wystarczający do uruchomienia potoku. Wyzwalacz zaczyna obowiązywać dopiero po opublikowaniu rozwiązania w fabryce Data Factory, a nie po zapisaniu go w interfejsie użytkownika.

        ![Ustawienia wyzwalacza](./media/how-to-create-schedule-trigger/trigger-settings.png)
4. W **nowego wyzwalacza** okno wyboru **Activated** opcji, a następnie kliknij przycisk **dalej**. To pole wyboru służy do dezaktywowania wyzwalacza później. 

    ![Ustawienia wyzwalacza — przycisk Dalej](./media/how-to-create-schedule-trigger/trigger-settings-next.png)
5. Na stronie **Nowy wyzwalacz** zapoznaj się z komunikatem ostrzegawczym, a następnie kliknij przycisk **Zakończ**.

    ![Ustawienia wyzwalacza — przycisk Zakończ](./media/how-to-create-schedule-trigger/new-trigger-finish.png)
6. Kliknij pozycję **Opublikuj**, aby opublikować zmiany w fabryce Data Factory. Dopóki Opublikuj zmiany z fabryką danych, wyzwalacz nie można uruchomić wyzwalania uruchamia potoku. 

    ![Przycisk Opublikuj](./media/how-to-create-schedule-trigger/publish-2.png)
8. Przejdź do karty **Monitorowanie** po lewej stronie. Kliknij pozycję **Odśwież**, aby odświeżyć listę. Zobaczysz wyzwalanych przez zaplanowane wyzwalacz uruchamia potoku. Zwróć uwagę na wartości w kolumnie **Wyzwolone przez**. Jeśli używasz **wyzwalacza teraz** opcji, zobacz wyzwalacza ręcznego uruchamiania na liście. 

    ![Monitorowanie wyzwolonych uruchomień](./media/how-to-create-schedule-trigger/monitor-triggered-runs.png)
9. Kliknij strzałkę w dół obok pozycji **Uruchomienia potoków**, aby przejść do widoku **Uruchomienia wyzwalacza**. 

    ![Monitorowanie uruchomień wyzwalacza](./media/how-to-create-schedule-trigger/monitor-trigger-runs.png)

## <a name="azure-powershell"></a>Azure PowerShell
W tej sekcji przedstawiono sposób tworzenia, uruchomić i monitorować wyzwalacz harmonogramu przy użyciu programu Azure PowerShell. Aby wyświetlić pracy tej próbki, najpierw przejść przez [Szybki Start: tworzenie fabryki danych przy użyciu programu Azure PowerShell](quickstart-create-data-factory-powershell.md). Następnie dodaj następujący kod do metody main, która tworzy i uruchamia wyzwalacz harmonogram, który jest uruchamiane co 15 minut. Wyzwalacz jest skojarzony z potokiem o nazwie **Adfv2QuickStartPipeline** utworzonego w ramach szybkiego startu.

1. Utwórz plik JSON o nazwie **MyTrigger.json** w folderze C:\ADFv2QuickStartPSH\ o następującej treści:

    > [!IMPORTANT]
    > Przed zapisaniem pliku JSON, ustaw wartość **startTime** element bieżący czas UTC. Ustaw wartość **endTime** elementu godzinę poza bieżący czas UTC.

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
    - **Typu** element wyzwalacza jest ustawiony na wartość "ScheduleTrigger."
    - **Częstotliwość** element jest ustawiony na wartość "Min" i **interwał** element jest ustawiony na wartość 15. W związku z tym wyzwalacz uruchamia potoku co 15 minut od godziny rozpoczęcia i zakończenia.
    - **EndTime** element jest jednej godziny po wartości **startTime** elementu. W związku z tym wyzwalacz uruchamia potoku 15 minut, 30 minut, a 45 minut po czasie rozpoczęcia. Należy pamiętać zaktualizować czas rozpoczęcia bieżącego czasu UTC, a czas zakończenia godzinę późniejsza niż godzina rozpoczęcia. 
    - Wyzwalacz skojarzony z **Adfv2QuickStartPipeline** potoku. Aby skojarzyć wiele potoki z wyzwalaczem, Dodaj więcej **pipelineReference** sekcje.
    - Potok w szybkiego startu przyjmuje dwa **parametry** wartości: **inputPath** i **outputPath**. W związku z tym należy przekazać wartości tych parametrów z wyzwalacza.

2. Utwórz wyzwalacz za pomocą **AzureRmDataFactoryV2Trigger zestaw** polecenia cmdlet:

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```

3. Upewnij się, że jest w stanie wyzwalacza **zatrzymane** za pomocą **Get-AzureRmDataFactoryV2Trigger** polecenia cmdlet:

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

4. Uruchomić wyzwalacz za pomocą **Start AzureRmDataFactoryV2Trigger** polecenia cmdlet:

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

5. Upewnij się, że jest w stanie wyzwalacza **uruchomiono** za pomocą **Get-AzureRmDataFactoryV2Trigger** polecenia cmdlet:

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

6.  GET, wyzwalacz uruchamia w programie Azure PowerShell przy użyciu **Get-AzureRmDataFactoryV2TriggerRun** polecenia cmdlet. Aby uzyskać informacje na temat uruchamia wyzwalacz, uruchom następujące polecenie okresowo. Aktualizacja **TriggerRunStartedAfter** i **TriggerRunStartedBefore** wartości są zgodne z wartościami w definicji wyzwalacz:

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
    Aby monitorować wyzwalacz uruchamia i potoku jest uruchamiana w portalu Azure, zobacz [uruchamia Monitor potoku](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).


## <a name="net-sdk"></a>Zestaw SDK .NET
W tej sekcji przedstawiono sposób użycia zestawu .NET SDK do tworzenia, uruchomić i monitorować wyzwalacz. Aby wyświetlić pracy tej próbki, najpierw przejść przez [Szybki Start: tworzenie fabryki danych przy użyciu zestawu .NET SDK](quickstart-create-data-factory-dot-net.md). Następnie dodaj następujący kod do metody main, która tworzy i uruchamia wyzwalacz harmonogram, który jest uruchamiane co 15 minut. Wyzwalacz jest skojarzony z potokiem o nazwie **Adfv2QuickStartPipeline** utworzonego w ramach szybkiego startu.

Aby utworzyć i uruchomić wyzwalacz harmonogram, który jest uruchamiane co 15 minut, Dodaj następujący kod do metody głównej:

```csharp
            // Create the trigger
            Console.WriteLine("Creating the trigger");

            // Set the start time to the current UTC time
            DateTime startTime = DateTime.UtcNow;

            // Specify values for the inputPath and outputPath parameters
            Dictionary<string, object> pipelineParameters = new Dictionary<string, object>();
            pipelineParameters.Add("inputPath", "adftutorial/input");
            pipelineParameters.Add("outputPath", "adftutorial/output");

            // Create a schedule trigger
            string triggerName = "MyTrigger";
            ScheduleTrigger myTrigger = new ScheduleTrigger()
            {
                Pipelines = new List<TriggerPipelineReference>()
                {
                    // Associate the Adfv2QuickStartPipeline pipeline with the trigger
                    new TriggerPipelineReference()
                    {
                        PipelineReference = new PipelineReference(pipelineName),
                        Parameters = pipelineParameters,
                    }
                },
                Recurrence = new ScheduleTriggerRecurrence()
                {
                    // Set the start time to the current UTC time and the end time to one hour after the start time
                    StartTime = startTime,
                    TimeZone = "UTC",
                    EndTime = startTime.AddHours(1),
                    Frequency = RecurrenceFrequency.Minute,
                    Interval = 15,
                }
            };

            // Now, create the trigger by invoking the CreateOrUpdate method
            TriggerResource triggerResource = new TriggerResource()
            {
                Properties = myTrigger
            };
            client.Triggers.CreateOrUpdate(resourceGroup, dataFactoryName, triggerName, triggerResource);

            // Start the trigger
            Console.WriteLine("Starting the trigger");
            client.Triggers.Start(resourceGroup, dataFactoryName, triggerName);
```

Aby monitorować wyzwalacz uruchomienia, Dodaj następujący kod przed ostatniego `Console.WriteLine` instrukcji w przykładzie:

```csharp
            // Check that the trigger runs every 15 minutes
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

Aby monitorować wyzwalacz uruchamia i potoku jest uruchamiana w portalu Azure, zobacz [uruchamia Monitor potoku](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).


## <a name="python-sdk"></a>Zestaw SDK dla języka Python
W tej sekcji przedstawiono sposób użycia zestawu SDK Python do tworzenia, uruchomić i monitorować wyzwalacz. Aby wyświetlić pracy tej próbki, najpierw przejść przez [Szybki Start: tworzenie fabryki danych przy użyciu zestawu SDK Python](quickstart-create-data-factory-python.md). Następnie należy dodać następujący blok kodu po bloku kodu "Monitoruj potoku uruchamiania" w skrypcie języka Python. Ten kod tworzy wyzwalacz harmonogram uruchamiany co 15 minut od określonego czasu rozpoczęcia i zakończenia. Aktualizacja **godzina_rozpoczęcia** zmiennej bieżący czas UTC i **end_time** zmienną godzinę poza bieżący czas UTC.

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

    # Start the trigger
    adf_client.triggers.start(rg_name, df_name, tr_name)
```

Aby monitorować wyzwalacz uruchamia i potoku jest uruchamiana w portalu Azure, zobacz [uruchamia Monitor potoku](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="azure-resource-manager-template"></a>Szablon usługi Azure Resource Manager
Szablon usługi Azure Resource Manager służy do tworzenia wyzwalacza. Aby uzyskać instrukcje, zobacz [tworzenie fabryki danych Azure przy użyciu szablonu usługi Resource Manager](quickstart-create-data-factory-resource-manager-template.md).  

## <a name="pass-the-trigger-start-time-to-a-pipeline"></a>Przekaż wyzwalacza godziny rozpoczęcia dla potoku
Fabryka danych Azure w wersji 1 obsługuje odczytu lub zapisu danych podzielonej na partycje przy użyciu zmiennych systemowych: **SliceStart**, **SliceEnd**, **WindowStart**i **WindowEnd**. W fabryce danych Azure w wersji 2 to zachowanie można osiągnąć za pomocą parametru potoku. Czas rozpoczęcia i zaplanowanym terminie wyzwalacza są ustawione jako wartość parametru potoku. W poniższym przykładzie zaplanowanego czasu dla wyzwalacza jest przekazywany jako wartość do potoku **scheduledRunTime** parametru:

```json
"parameters": {
    "scheduledRunTime": "@trigger().scheduledTime"
}
```    

Aby uzyskać więcej informacji, zobacz instrukcje w [jak do odczytu lub zapisu na partycje danych](how-to-read-write-partitioned-data.md).

## <a name="json-schema"></a>Schematu JSON
Poniższe definicji JSON przedstawia sposób tworzenia harmonogramu wyzwalacza z planowania i cyklu:

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Month>>,
        "interval": <<int>>,             // Optional, specifies how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime - optional>>,
        "timeZone": "UTC"
        "schedule": {                    // Optional (advanced scheduling specifics)
          "hours": [<<0-23>>],
          "weekDays": : [<<Monday-Sunday>>],
          "minutes": [<<0-59>>],
          "monthDays": [<<1-31>>],
          "monthlyOccurences": [
               {
                    "day": <<Monday-Sunday>>,
                    "occurrence": <<1-5>>
               }
           ]
        }
      }
    },
   "pipelines": [
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "<Name of your pipeline>"
                },
                "parameters": {
                    "<parameter 1 Name>": {
                        "type": "Expression",
                        "value": "<parameter 1 Value>"
                    },
                    "<parameter 2 Name>" : "<parameter 2 Value>"
                }
           }
      ]
  }
}
```

> [!IMPORTANT]
>  **Parametry** właściwość jest właściwością obowiązkowe **potoki** elementu. Planowaną nie przyjmuje żadnych parametrów, należy dołączyć pusta definicja JSON **parametry** właściwości.


### <a name="schema-overview"></a>Przegląd schematu
Poniższa tabela zawiera omówienie elementów głównych schematu, które są powiązane z cyklu i planowania wyzwalacza:

| Właściwość JSON | Opis |
|:--- |:--- |
| **startTime** | Wartość daty i godziny. Proste harmonogramów, wartość **startTime** właściwość jest stosowana do pierwszego wystąpienia. Złożone harmonogramów wyzwalacz uruchamia nie wcześniej niż określona **startTime** wartość. |
| **endTime** | Data i godzina zakończenia dla wyzwalacza. Wyzwalacz nie jest wykonywana po określonej daty i godziny zakończenia. Wartość właściwości nie może przypadać w przeszłości. Ta właściwość jest opcjonalna. |
| **timeZone** | Strefa czasowa. Aktualnie obsługiwana jest tylko strefę czasową UTC. |
| **cyklu** | Obiekt cyklu, który określa cyklu reguły dla wyzwalacza. Obiekt cyklu obsługuje **częstotliwość**, **interva**l, **endTime**, **liczba**, i **harmonogram**elementów. Po zdefiniowaniu obiektu cyklu **częstotliwość** element jest wymagany. Inne elementy obiektu cyklu są opcjonalne. |
| **częstotliwość** | Jednostka częstotliwość powtarzania wyzwalacza. Obsługiwane wartości to m.in. "min", "Godzina", "day", "tydzień" i "miesiąc". |
| **interval** | Dodatnia liczba całkowita, która określa interwał **częstotliwość** wartość, która określa, jak często uruchamia wyzwalacz. Na przykład jeśli **interwał** 3 i **częstotliwość** jest "tydzień" wyzwalacza wystąpi co 3 tygodni. |
| **schedule** | Harmonogram cyklu dla wyzwalacza. Wyzwalacz z określonym **częstotliwość** wartość zmienia jego cyklu na podstawie harmonogramu cyklu. **Harmonogram** właściwość zawiera zmiany cyklu, które są oparte na minuty, godziny, dni tygodnia, dni miesiąca i numer tygodnia.


### <a name="schema-defaults-limits-and-examples"></a>Wartości domyślne schematu, ograniczenia i przykłady

| Właściwość JSON | Typ | Wymagane | Wartość domyślna | Prawidłowe wartości | Przykład |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** | Ciąg | Yes | None | Daty i godziny ISO-8601 | `"startTime" : "2013-01-09T09:30:00-08:00"` |
| **cyklu** | Obiekt | Yes | None | Obiekt cyklu | `"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **interval** | Liczba | Nie | 1 | 1 do 1000. | `"interval":10` |
| **endTime** | Ciąg | Yes | Brak | Wartość daty i godziny, który reprezentuje czas w przyszłości. | `"endTime" : "2013-02-09T09:30:00-08:00"` |
| **schedule** | Obiekt | Nie | Brak | Obiekt harmonogramu | `"schedule" : { "minute" : [30], "hour" : [8,17] }` |

### <a name="starttime-property"></a>Właściwość startTime
Poniższej tabeli przedstawiono sposób **startTime** właściwość określa wyzwalacz uruchomienia:

| Wartość startTime | Cykl bez harmonogramu | Cykl z harmonogramem |
|:--- |:--- |:--- |
| Godzina rozpoczęcia w przeszłości | Oblicza przyszłych wykonywania po raz pierwszy po czasie rozpoczęcia i działa w tym czasie.<br/><br/>Uruchamiany podczas kolejnych wykonań kodu oparte na obliczanie od czasu ostatniego wykonania.<br/><br/>Zobacz przykład znajdujący się pod tabelą. | Wyzwalacz uruchamia _nie sooner niż_ określonym czasie rozpoczęcia. Pierwsze wystąpienie opiera się na harmonogram, który jest obliczana na podstawie czasu rozpoczęcia.<br/><br/>Uruchamiany podczas kolejnych wykonań kodu na podstawie harmonogramu cyklu. |
| Godzina rozpoczęcia w przyszłości lub obecna | Uruchamia jeden raz o określonym czasie rozpoczęcia.<br/><br/>Uruchamiany podczas kolejnych wykonań kodu oparte na obliczanie od czasu ostatniego wykonania. | Wyzwalacz uruchamia _nie wcześniej_ niż określony czas rozpoczęcia. Pierwsze wystąpienie opiera się na harmonogram, który jest obliczana na podstawie czasu rozpoczęcia.<br/><br/>Uruchamiany podczas kolejnych wykonań kodu na podstawie harmonogramu cyklu. |

Zobacz przykład co się stanie, jeśli czas rozpoczęcia przypada w przeszłości, cyklu, ale bez harmonogramu. Załóżmy, że bieżący czas jest `2017-04-08 13:00`, godzina rozpoczęcia jest `2017-04-07 14:00`, i cykl to dwa dni. ( **Cyklu** wartość jest definiowana przez ustawienie **częstotliwość** dla właściwości "day" i **interwał** właściwości do 2.) Zwróć uwagę, że **startTime** wartość przypada w przeszłości i występuje przed bieżącym czasem.

W tych warunkach pierwsze wykonanie następuje o `2017-04-09 at 14:00`. Aparat harmonogramu oblicza wystąpienia wykonania od czasu rozpoczęcia. Wszystkie wystąpienia w przeszłości są odrzucane. Aparat wykorzystuje następne wystąpienie, które ma miejsce w przyszłości. W tym scenariuszu, godzina rozpoczęcia jest `2017-04-07 at 2:00pm`, więc następne wystąpienie jest dwa dni po tym czasie, czyli `2017-04-09 at 2:00pm`.

Wykonanie po raz pierwszy jest tym samym, nawet jeśli **startTime** wartość jest `2017-04-05 14:00` lub `2017-04-01 14:00`. Po wykonaniu pierwszej podczas kolejnych wykonań kodu są obliczane na podstawie harmonogramu. W związku z tym podczas kolejnych wykonań kodu są `2017-04-11 at 2:00pm`, następnie `2017-04-13 at 2:00pm`, następnie `2017-04-15 at 2:00pm`i tak dalej.

Na koniec po godzinach i minutach nie są ustawione w harmonogramie wyzwalaczy, godzinach i minutach pierwszy wykonywania są używane jako ustawienia domyślne.

### <a name="schedule-property"></a>Właściwość harmonogramu
Z jednej strony Użyj harmonogramu można ograniczyć Liczba wykonań wyzwalacza. Na przykład jeśli wyzwalacz z miesięcznej częstotliwości jest zaplanowane do uruchomienia tylko na dzień 31, wyzwalacz działa tylko w tych miesięcy, przez które mają 31 dni.

Harmonogram może także zwiększyć liczbę wykonań wyzwalacza. Na przykład wyzwalacz z miesięcznej częstotliwości, który zostało zaplanowane do uruchomienia na dni miesiąca 1 i 2 jest uruchamiane na 1 i 2 dni miesiąca, zamiast raz w miesiącu.

Jeśli wiele **harmonogram** elementy są określone, kolejności oceny od największej do najmniejszej ustawienia harmonogramu. Oceny rozpoczyna się od tygodnia numer, a następnie miesiąc, dzień, dzień tygodnia, godzinę i na koniec minutę.

W poniższej tabeli opisano **harmonogram** elementy szczegółowo:


| JSON element | Opis | Prawidłowe wartości |
|:--- |:--- |:--- |
| **minut** | Minuty godziny, o których uruchamiany jest wyzwalacz. | <ul><li>Liczba całkowita</li><li>Tablica liczb całkowitych</li></ul>
| **godziny** | Godziny dnia, o których uruchamiany jest wyzwalacz. | <ul><li>Liczba całkowita</li><li>Tablica liczb całkowitych</li></ul> |
| **dni tygodnia** | Dni tygodnia, w którym uruchamia wyzwalacz. Wartość można określić tylko co tydzień częstotliwość. | <ul><li>Poniedziałek, Wtorek, środę, czwartek, piątek, sobota, niedziela</li><li>Tablica wartości dzień (rozmiar maksymalny tablicy wynosi 7)</li><li>Wartości dzień nie jest rozróżniana wielkość liter</li></ul> |
| **monthlyOccurrences** | Dni miesiąca, na którym jest uruchomiony wyzwalacza. Można określić wartość z tylko miesięcznej częstotliwości. | <ul><li>Tablica **monthlyOccurence** obiektów: `{ "day": day,  "occurrence": occurence }`.</li><li>**Dzień** atrybut jest dzień tygodnia, na którym jest uruchomiony wyzwalacza. Na przykład **monthlyOccurrences** właściwości o **dzień** wartość `{Sunday}` oznacza niedzielę każdego miesiąca. **Dzień** atrybut jest wymagany.</li><li>**Wystąpienie** atrybutu jest wystąpienie określonego **dzień** w miesiącu. Na przykład **monthlyOccurrences** właściwości o **dzień** i **wystąpienie** wartości `{Sunday, -1}` oznacza niedzielę ostatniego dnia miesiąca. **Wystąpienie** atrybutu jest opcjonalny.</li></ul> |
| **monthDays** | Dzień miesiąca, na którym jest uruchomiony wyzwalacza. Można określić wartość z tylko miesięcznej częstotliwości. | <ul><li>Dowolna wartość <= -1 i >= -31</li><li>Dowolna wartość >= 1 i <= 31</li><li>Tablica wartości</li></ul> |


## <a name="examples-of-trigger-recurrence-schedules"></a>Przykłady harmonogramy cyklu wyzwalacza
Ta sekcja zawiera przykłady cyklu harmonogramów i koncentruje się na **harmonogram** obiekt i jego elementów.

Przykłady przyjęto założenie, że **interwał** wartość to 1 i który **częstotliwość** wartość jest prawidłowa zgodnie z definicją harmonogramu. Na przykład nie może mieć **częstotliwość** wartość "day", a także mieć modyfikację "monthDays" w **harmonogram** obiektu. Ograniczenia, takie jak te są wymienione w tabeli w poprzedniej sekcji.

| Przykład | Opis |
|:--- |:--- |
| `{"hours":[5]}` | Uruchom o 5:00 każdego dnia. |
| `{"minutes":[15], "hours":[5]}` | Jednocześnie 5:15:00 każdego dnia. |
| `{"minutes":[15], "hours":[5,17]}` | Uruchom w 5:15:00 a 17:15:00 każdego dnia. |
| `{"minutes":[15,45], "hours":[5,17]}` | Uruchom na 5:15:00, 5:45:00, 17:15:00 i 5:45 PM każdego dnia. |
| `{"minutes":[0,15,30,45]}` | Uruchom co 15 minut. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Uruchom co godzinę. Ten wyzwalacz jest uruchamiany co godzinę. Protokoły są kontrolowane przez **startTime** wartość, gdy określona jest wartość. Jeśli nie określono wartości, protokoły są kontrolowane przez czas utworzenia. Na przykład, jeśli czas rozpoczęcia i godzina utworzenia (którekolwiek ma zastosowanie) jest godzina 12:25, wyzwalacz uruchamia się na 00:25, 01:25 02:25,... i 23:25.<br/><br/>Ten harmonogram jest odpowiednikiem o wyzwalacza z **częstotliwość** wartość "godzin," **interwał** wartość 1, a nie **harmonogram**.  Ten harmonogram może być używany z różnych **częstotliwość** i **interwał** wartości, aby utworzyć inne wyzwalaczy. Na przykład, jeśli **częstotliwość** wartość to "miesiąc", uruchamia harmonogram każdego dnia, a nie tylko raz w miesiącu po **częstotliwość** wartość to "dzień". |
| `{"minutes":[0]}` | Uruchom co godzinę w ciągu godziny. Tego wyzwalacza jest uruchamiana co godzinę, począwszy od 00:00:00, 1:00:00, 2:00 AM, godzinę i tak dalej.<br/><br/>Ten harmonogram jest odpowiednikiem wyzwalacza z **częstotliwość** wartość "Godzina" i **startTime** wartość zero minut lub nie **harmonogram** , ale **częstotliwości**  wartość "day". Jeśli **częstotliwość** wartość to "week" lub "month", harmonogram wykonuje jeden dzień tygodnia lub jeden dzień w miesiącu tylko, odpowiednio. |
| `{"minutes":[15]}` | Uruchom 15 minut po co godzinę. Tego wyzwalacza jest uruchamiana co godzinę na 15 minut po pełnej godzinie, zaczynając od 00:15 AM, 1:15:00, 2:15 AM i tak dalej i kończący się na 23:15:00. |
| `{"hours":[17], "weekDays":["saturday"]}` | Uruchom na 5:00 PM soboty co tydzień. |
| `{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Jednocześnie 17:00:00 w poniedziałek, środę i piątek co tydzień. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Jednocześnie 17:15:00 a: 45 17 w poniedziałek, środę i piątek co tydzień. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Uruchom co 15 minut na dni tygodnia. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Uruchom co 15 minut w dni robocze między 9:00 AM a 4:45 PM. |
| `{"weekDays":["tuesday", "thursday"]}` | Uruchom we wtorki i czwartki o określonym czasie rozpoczęcia. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` | Uruchom o 6:00 w dniu 28 co miesiąc (przy założeniu **częstotliwość** wartość "miesiąc"). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Uruchom o 6:00:00 ostatniego dnia miesiąca. Aby uruchomić wyzwalacz ostatniego dnia miesiąca, należy użyć zamiast dnia 28, 29, 30 i 31 -1. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Uruchom godzinie 6:00 pierwszy i ostatni dzień każdego miesiąca. |
| `{monthDays":[1,14]}` | Uruchamianie pierwszej i 14 dnia każdego miesiąca w określonym czasie rozpoczęcia. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Uruchom w pierwszym piątek każdego miesiąca o godzinie 5:00:00. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Uruchom w pierwszym piątek miesiąca w określonym czasie rozpoczęcia. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Uruchom na trzeci poniedziałek od końca miesiąca, co miesiąc, w określonym czasie rozpoczęcia. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Uruchom na pierwszy i ostatni piątek miesiąca w 5:15:00. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Uruchom na pierwszy i ostatni piątek miesiąca w określonym czasie rozpoczęcia. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Uruchom w piątym piątek miesiąca w określonym czasie rozpoczęcia. Gdy w miesiącu nie ma żadnych piątej piątek, potoku nie działa, ponieważ zostało zaplanowane do uruchomienia tylko na piątki piątego. Aby uruchomić wyzwalacz ostatniego wystąpienia piątek miesiąca, rozważ użycie -1 zamiast 5 dla **wystąpienie** wartość. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Uruchom co 15 minut na ostatni piątek miesiąca. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Uruchom na 5:15:00, 5:45:00, 17:15:00 i 5:45 PM trzecią środę każdego miesiąca. |


## <a name="next-steps"></a>Kolejne kroki
Aby uzyskać szczegółowe informacje dotyczące wyzwalaczy, zobacz [potoku wykonywania i wyzwalaczy](concepts-pipeline-execution-triggers.md#triggers).
