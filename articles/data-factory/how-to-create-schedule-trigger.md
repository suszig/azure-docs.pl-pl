---
title: "Sposób tworzenia harmonogramu Wyzwalacze w fabryce danych Azure | Dokumentacja firmy Microsoft"
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
ms.openlocfilehash: 6b92e8402d372e29e264dc70b128124973d66bb9
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/05/2018
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Jak utworzyć wyzwalacz uruchamia potoku zgodnie z harmonogramem
Ten artykuł zawiera informacje o wyzwalacz harmonogram i kroki, aby utworzyć, uruchomić i monitorować wyzwalacz. Dla innych typów wyzwalaczy, zobacz [potoku wykonywania i wyzwalaczy](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz artykuł z [wprowadzeniem do usługi Data Factory w wersji 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

### <a name="schedule-trigger-json-definition"></a>Harmonogram wyzwalacza definicji JSON
Podczas tworzenia harmonogramu wyzwalacza, można określić planowania i cyklu przy użyciu formatu JSON, jak pokazano w przykładzie w tej sekcji.

Aby wyzwalacz harmonogram zaczną działać poza potoku uruchamiania, zawiera odwołanie potoku potoku określonego w definicji wyzwalacza. Między potokami i wyzwalaczami występuje relacja wiele-do-wielu. Wiele wyzwalaczy może uruchomić jeden potok. Jeden wyzwalacz może uruchamiać wiele potoków.

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Month>>,
        "interval": <<int>>,             // optional, how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime - optional>>,
        "timeZone": "UTC"
        "schedule": {                    // optional (advanced scheduling specifics)
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
>  Właściwość **parameters** jest obowiązkowa wewnątrz sekcji **pipelines**. Nawet gdy potok nie przyjmuje żadnych parametrów, należy dodać pusty kod json dla właściwości parameters, ponieważ ta właściwość musi istnieć.


### <a name="overview-schedule-trigger-schema"></a>Omówienie: Harmonogram wyzwalacza schematu
Poniższa tabela zawiera ogólne omówienie głównych elementów odnoszących się do powtarzania i planowania w wyzwalaczu:

Właściwość JSON |     Opis
------------- | -------------
startTime | Właściwość startTime ma format data-godzina. W przypadku prostych harmonogramów startTime to pierwsze wystąpienie. W przypadku harmonogramów złożonych wyzwalacz nie jest uruchamiany wcześniej niż startTime.
endTime | Określa datę i godzinę zakończenia wyzwalacza. Po upływie tego czasu wyzwalacz nie jest wykonywany. Wartość endTime nie może być przeszłą datą. Jest to właściwość opcjonalna.
timeZone | Aktualnie obsługiwany jest tylko czas UTC.
recurrence | Obiekt recurrence określa reguły powtarzania wyzwalacza. Obiekt recurrence obsługuje następujące elementy: frequency, interval, endTime, count i schedule. Zdefiniowanie powtarzalności wymaga zdefiniowania częstotliwości. Inne elementy obiektu recurrence są opcjonalne.
frequency | Reprezentuje jednostkę częstotliwości powtarzania wyzwalacza. Obsługiwane wartości to `minute`, `hour`, `day`, `week` i `month`.
interval | Obiekt interval jest dodatnią liczbą całkowitą. Oznacza on interwał częstotliwości, który określa częstotliwość uruchamiania wyzwalacza. Na przykład jeśli wartość interval to 3, a frequency to „week”, wyzwalacz powtarza się co 3 tygodnie.
schedule | Wyzwalacz z określoną częstotliwością zmienia swój cykl na podstawie harmonogramu cyklu. Harmonogram zawiera modyfikacje oparte na minutach, godzinach, dniach tygodnia, dniach miesiąca i numerze tygodnia.


### <a name="overview-schedule-trigger-schema-defaults-limits-and-examples"></a>Omówienie: Planowanie wartości domyślne schematu wyzwalacza, ograniczenia i przykłady

Nazwa JSON | Typ wartości | Wymagana? | Wartość domyślna | Prawidłowe wartości | Przykład
--------- | ---------- | --------- | ------------- | ------------ | -------
startTime | Ciąg | Yes | None | Daty i godziny ISO-8601 | ```"startTime" : "2013-01-09T09:30:00-08:00"```
recurrence | Obiekt | Yes | None | Obiekt cyklu | ```"recurrence" : { "frequency" : "monthly", "interval" : 1 }```
interval | Liczba | Nie | 1 | 1 do 1000. | ```"interval":10```
endTime | Ciąg | Yes | None | Wartość daty i godziny reprezentująca godzinę w przyszłości | `"endTime" : "2013-02-09T09:30:00-08:00"`
schedule | Obiekt | Nie | None | Obiekt harmonogramu | `"schedule" : { "minute" : [30], "hour" : [8,17] }`

### <a name="deep-dive-starttime"></a>Szczegółowe informacje: startTime
Poniższa tabela przedstawia metodę kontrolowania sposobu uruchomienia wyzwalacza przez właściwość startTime:

Wartość startTime | Cykl bez harmonogramu | Cykl z harmonogramem
--------------- | --------------------------- | ------------------------
Godzina rozpoczęcia w przeszłości | Oblicza pierwszy przyszły czas wykonania po czasie rozpoczęcia i uruchamia wyzwalacz w tym czasie.<p>Uruchamia kolejne wykonania na podstawie obliczeń z ostatniego czasu wykonania.</p><p>Zobacz przykład znajdujący się pod tabelą.</p> | Wyzwalacz jest uruchamiany _nie wcześniej niż_ w określonym czasie rozpoczęcia. Pierwsze wystąpienie opiera się na harmonogramie obliczonym na podstawie czasu rozpoczęcia. <p>Kolejne wykonania opierają na harmonogramie cyklu</p>
Godzina rozpoczęcia w przyszłości lub obecna | Uruchamiany raz o określonej godzinie rozpoczęcia. <p>Uruchamia kolejne wykonania na podstawie obliczeń z ostatniego czasu wykonania.</p> | Wyzwalacz jest uruchamiany _nie wcześniej niż_ w określonym czasie rozpoczęcia. Pierwsze wystąpienie opiera się na harmonogramie obliczonym na podstawie czasu rozpoczęcia.<p>Kolejne wykonania opierają na harmonogramie cyklu.</p>

Zobaczmy przykład zdarzeń w przypadku, gdy właściwość startTime określa godzinę w przeszłości, z cyklem, ale bez harmonogramu. Załóżmy, że aktualna godzina to `2017-04-08 13:00`, właściwość startTime ma wartość `2017-04-07 14:00`, a cykl jest określony na dwa dni (zdefiniowany parametrami : day i interval: 2.) Zwróć uwagę, że właściwość startTime dotyczy przeszłej daty i następuje przed aktualną godziną.

W tych warunkach pierwsze wykonanie następuje o `2017-04-09 at 14:00`. Aparat harmonogramu oblicza wystąpienia wykonania od czasu rozpoczęcia. Wszystkie wystąpienia w przeszłości są odrzucane. Aparat wykorzystuje następne wystąpienie, które ma miejsce w przyszłości. Tak więc w tym przypadku właściwość startTime ma wartość `2017-04-07 at 2:00pm`, w związku z czym kolejne wystąpienie ma miejsce dwa dni po tym czasie, czyli o `2017-04-09 at 2:00pm`.

Czas pierwszego wykonania jest identyczny, nawet jeśli właściwość startTime ma wartość `2017-04-05 14:00` lub `2017-04-01 14:00`. Po pierwszym wykonaniu kolejne wykonania są obliczane przy użyciu harmonogramu. W związku z tym wystąpią o `2017-04-11 at 2:00pm`, następnie o `2017-04-13 at 2:00pm`, `2017-04-15 at 2:00pm` itp.

Z kolei jeśli wyzwalacz ma harmonogram, jeśli w harmonogramie nie ustawiono godzin i/lub minut, wartością domyślną są odpowiednio godziny i/lub minuty pierwszego wykonania.

### <a name="deep-dive-schedule"></a>Szczegółowe informacje: schedule
Z jednej strony, właściwość schedule może ograniczyć liczbę wykonań wyzwalacza. Na przykład, jeśli wyzwalacz z częstotliwością „month” ma harmonogram uruchamiany tylko w 31 dniu, wyzwalacz jest wykonywany tylko w miesiącach, której mają 31 dni.

Harmonogram może także zwiększyć liczbę wykonań wyzwalacza. Na przykład, jeśli wyzwalacz z częstotliwością „month”ma harmonogram uruchamiany w 1 i 2 dniu miesiąca, wyzwalacz jest wykonywany w 1 i 2dniu miesiąca, nie tylko raz w miesiącu.

W przypadku określenia wielu elementów harmonogramu ocena następuje od największego do najmniejszego elementu: numer tygodnia, dzień miesiąca, dzień tygodnia, godzina i minuta.

W poniższej tabeli opisano szczegółowo elementy harmonogramu:


Nazwa JSON | Opis | Prawidłowe wartości
--------- | ----------- | ------------
minutes | Minuty godziny, o których uruchamiany jest wyzwalacz. | <ul><li>Liczba całkowita</li><li>Tablica liczb całkowitych</li></ul>
hours | Godziny dnia, o których uruchamiany jest wyzwalacz. | <ul><li>Liczba całkowita</li><li>Tablica liczb całkowitych</li></ul>
weekDays | Dni tygodnia, w które uruchamiany jest wyzwalacz. Można określić tylko z częstotliwością tygodniową. | <ul><li>Monday, Tuesday, Wednesday, Thursday, Friday, Saturday lub Sunday</li><li>Tablica dowolnych wartości (maksymalny rozmiar tablicy: 7)</li></p>Bez uwzględniania wielkości liter</p>
monthlyOccurrences | Określa dni miesiąca, w które uruchamiany jest wyzwalacz. Można określić tylko z częstotliwością miesięczną. | Tablica obiektów monthlyOccurence: `{ "day": day,  "occurrence": occurence }`. <p> Dzień to dzień tygodnia, w którym uruchamiany jest wyzwalacz, na przykład `{Sunday}` to każda niedziela miesiąca. Wymagany.<p>Wystąpienie to wystąpienie dnia w miesiącu, na przykład `{Sunday, -1}` to ostatnia niedziela miesiąca. Opcjonalny.
monthDays | Dzień miesiąca, w którym uruchamiany jest wyzwalacz. Można określić tylko z częstotliwością miesięczną. | <ul><li>Dowolna wartość <= -1 i >= -31</li><li>Dowolna wartość >= 1 i <= 31</li><li>Tablica wartości</li>


## <a name="examples-recurrence-schedules"></a>Przykłady: harmonogramy cyklu
Ta sekcja zawiera przykłady harmonogramów cyklu — koncentrujące się na obiekcie harmonogramu i jego podelementach.

W przykładowych harmonogramach założono, że właściwość interval ma wartość 1. Założono także, że właściwość frequency ma wartość zgodną z harmonogramem, na przykład nie można użyć w harmonogramie częstotliwości „day” i modyfikacji „monthDays”. Ograniczenia te są wymienione w tabeli w poprzedniej sekcji.

Przykład | Opis
------- | -----------
`{"hours":[5]}` | Uruchamiany o 5:00 każdego dnia
`{"minutes":[15], "hours":[5]}` | Uruchamiany o 5:15 każdego dnia
`{"minutes":[15], "hours":[5,17]}` | Uruchamiany o 5:15 i 17:15 każdego dnia
`{"minutes":[15,45], "hours":[5,17]}` | Uruchamiany o 5:15, 5:45, 17:15 i 17:45 każdego dnia
`{"minutes":[0,15,30,45]}` | Uruchamiany co 15 minut
`{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Uruchamiany co godzinę. Ten wyzwalacz jest uruchamiany co godzinę. Minuta jest kontrolowana przez właściwość startTime, jeśli ją określono lub przez godzinę utworzenia, jeśli nie określono właściwości. Na przykład, jeśli godzina rozpoczęcia lub godzina utworzenia (zależnie od sytuacji) to 12:25, wyzwalacz jest uruchamiany o godzinie 0:25, 1:25, 2:25, ..., 23:25. Harmonogram jest odpowiednikiem wyzwalacza z właściwością frequency o wartości „hour”, interval o wartości 1 i bez harmonogramu. Różnica polega na tym, że ten harmonogram może być również używany z innymi wartościami frequency i interval do tworzenia innych wyzwalaczy. Na przykład, gdyby właściwość frequency miała wartość „month”, harmonogram byłby uruchamiany tylko raz w miesiącu, a nie codziennie, jak w przypadku wartości „day”.
`{"minutes":[0]}` | Uruchamiany co godzinę, o pełnej godzinie. Ten wyzwalacz jest również uruchamiany co godzinę, ale o pełnej godzinie (na przykład 0:00, 1:00, 2:00 itp.). To ustawienie jest równoważnikiem wyzwalacza o właściwości frequency z wartością „hour”, startTime wynoszącym zero minut oraz braku harmonogramu, gdyby właściwość frequency miała wartość „day”, ale jeśli właściwość frequency miałaby wartość „week” lub „month”, harmonogram byłby uruchamiany odpowiednio tylko raz w tygodniu lub raz w miesiącu.
`{"minutes":[15]}` | Uruchamiany 15 minut po pełnej godzinie. Uruchamiany co godzinę, począwszy od 0:15, 1:15 i 2:15, aż do 22:15 i 23:15.
`{"hours":[17], "weekDays":["saturday"]}` | Uruchamiany o 17:00 w każdą sobotę
`{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Uruchamiany o 17:00 w każdy poniedziałek, środę i piątek
`{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Uruchamiany o 17:15 i 17:45 w każdy poniedziałek, środę i piątek
`{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Uruchamiany co 15 minut w dni robocze
`{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Uruchamiany co 15 minut w dni robocze, między 9:00 i 16:45
`{"weekDays":["tuesday", "thursday"]}` | Uruchamiany we wtorki i czwartki o określonej godzinie.
`{"minutes":[0], "hours":[6], "monthDays":[28]}` | Uruchamiany o 6:00, 28. dnia każdego miesiąca (przy częstotliwości miesięcznej)
`{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Uruchamiany o 6:00 ostatniego dnia miesiąca. Jeśli chcesz uruchamiać wyzwalacz ostatniego dnia miesiąca, użyj wartości -1 zamiast 28, 29, 30 lub 31.
`{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Uruchamiany o 6:00 pierwszego i ostatniego dnia każdego miesiąca
`{monthDays":[1,14]}` | Uruchamiany pierwszego i czternastego dnia każdego miesiąca o określonej godzinie.
`{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Uruchamiany w pierwszy piątek każdego miesiąca o 5:00
`{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Uruchamiany w pierwszy piątek każdego miesiąca o określonej godzinie.
`{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Uruchamiany w trzeci piątek od końca miesiąca, co miesiąc, o określonej godzinie
`{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Uruchamiany w pierwszy i ostatni piątek każdego miesiąca o 5:15
`{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Uruchamiany w pierwszy i ostatni piątek każdego miesiąca o określonej godzinie
`{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Uruchamiany w piąty piątek każdego miesiąca o określonej godzinie. Jeśli miesiąc nie ma piątego piątku, potok nie jest uruchamiany, ponieważ jego harmonogram określa uruchamianie tylko w piąty piątek.  Jeśli chcesz uruchamiać wyzwalacz w ostatni piątek miesiąca, użyj dla wystąpienia wartości -1 zamiast 5.
`{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Uruchamiany co 15 minut w ostatni piątek miesiąca.
`{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Uruchamiany o 5:15, 5:45, 17:15 i 17:45 w trzecią środę każdego miesiąca.


## <a name="use-azure-powershell"></a>Korzystanie z programu Azure PowerShell
W tej sekcji przedstawiono sposób tworzenia, uruchomić i monitorować wyzwalacz harmonogramu przy użyciu programu Azure PowerShell. Jeśli chcesz wyświetlić pracy tej próbki, najpierw przejść przez [Szybki Start: tworzenie fabryki danych przy użyciu programu Azure PowerShell](quickstart-create-data-factory-powershell.md). Następnie dodaj następujący kod do metody main, która tworzy i uruchamia wyzwalacz harmonogram, który jest uruchamiane co 15 minut. Wyzwalacz jest skojarzony z potokiem (**Adfv2QuickStartPipeline**) utworzonego w ramach szybkiego startu.

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

## <a name="next-steps"></a>Kolejne kroki
Aby uzyskać szczegółowe informacje dotyczące wyzwalaczy, zobacz [potoku wykonywania i wyzwalaczy](concepts-pipeline-execution-triggers.md#triggers).
