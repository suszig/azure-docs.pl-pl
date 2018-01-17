---
title: "Wyzwalacze i wykonywanie potoku w usłudze Azure Data Factory | Microsoft Docs"
description: "Ten artykuł zawiera informacje na temat wykonywania potoku w usłudze Azure Data Factory, na żądanie lub przy użyciu wyzwalacza."
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
ms.date: 01/03/2018
ms.author: shlo
ms.openlocfilehash: 88ae5dfbf6246ecf92d6528ad3d9a8e5fb57e4b0
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/06/2018
---
# <a name="pipeline-execution-and-triggers-in-azure-data-factory"></a>Wyzwalacze i wykonywanie potoku w usłudze Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](v1/data-factory-scheduling-and-execution.md)
> * [Wersja 2 — wersja zapoznawcza](concepts-pipeline-execution-triggers.md)

**Uruchomienie potoku** to termin w usłudze Azure Data Factory Version 2, który definiuje wystąpienie wykonania potoku. Załóżmy na przykład istnienie potoku, który jest wykonywany o 8:00, 9:00 i 10:00. W takim przypadku występują trzy osobne uruchomienia potoku. Każde uruchomienie potoku ma unikatowy identyfikator, czyli identyfikator GUID stanowiący unikatową definicję tego konkretnego uruchomienia potoku. Uruchomienia potoku są tworzone zazwyczaj przez przekazanie argumentów do parametrów zdefiniowanych w potokach. Dostępne są dwie metody wykonywania potoku: **ręcznie** i przez **wyzwalacz**. Ten artykuł zawiera szczegółowe informacje o obu metodach wykonywania potoku.

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz [Scheduling and execution in Data Factory V1 (Planowanie i wykonywanie w usłudze Data Factory V1)](v1/data-factory-scheduling-and-execution.md).

## <a name="run-pipeline-on-demand"></a>Uruchamianie potoku na żądanie
W tej metodzie potok jest uruchamiany ręcznie. Jest ona również uważana za wykonywanie potoku na żądanie.

Na przykład, załóżmy, że występuje potok o nazwie **copyPipeline**, który chcesz wykonać. Potok ten jest prostym potokiem z jednym działaniem, który kopiuje elementy z folderu źródłowego w magazynie Azure Blob Storage do folderu docelowego w tym samym magazynie. Oto definicja przykładowego potoku:

```json
{
  "name": "copyPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "name": "CopyBlobtoBlob",
        "inputs": [
          {
            "referenceName": "sourceBlobDataset",
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "sinkBlobDataset",
            "type": "DatasetReference"
          }
        ]
      }
    ],
    "parameters": {
      "sourceBlobContainer": {
        "type": "String"
      },
      "sinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```
Potok przyjmuje dwa parametry: sourceBlobContainer i sinkBlobContainer, jak pokazano w definicji JSON. Wartości tych parametrów są przekazywane w czasie uruchomienia.

Aby ręcznie uruchomić potok, może użyć jednej z następujących metod: .NET, PowerShell, REST i Python.

### <a name="rest-api"></a>Interfejs API REST
Oto przykład polecenia REST:  

```
POST
https://management.azure.com/subscriptions/mySubId/resourceGroups/myResourceGroup/providers/Microsoft.DataFactory/factories/myDataFactory/pipelines/copyPipeline/createRun?api-version=2017-03-01-preview
```
Pełny przykład można znaleźć w artykule [Quickstart: create a data factory using REST API (Szybki start: tworzenie fabryki danych przy użyciu interfejsu API REST)](quickstart-create-data-factory-rest-api.md).

### <a name="powershell"></a>PowerShell
Oto przykład polecenia programu PowerShell:

```powershell
Invoke-AzureRmDataFactoryV2Pipeline -DataFactory $df -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
```

Parametry są przekazywane w treści ładunku żądania. W metodach wykorzystujących rozwiązania .NET, PowerShell i Python wartości przekazuje się w słowniku przekazywanym jako argument wywołania.

```json
{
  “sourceBlobContainer”: “MySourceFolder”,
  “sinkBlobCountainer”: “MySinkFolder”
}
```

Ładunek odpowiedzi to unikatowy identyfikator uruchomienia potoku:

```json
{
  "runId": "0448d45a-a0bd-23f3-90a5-bfeea9264aed"
}
```


Pełny przykład można znaleźć w artykule [Quickstart: create a data factory using PowerShell (Szybki start: tworzenie fabryki danych przy użyciu programu PowerShell)](quickstart-create-data-factory-powershell.md).

### <a name="net"></a>.NET
Oto przykładowe wywołanie .NET:

```csharp
client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, parameters)
```

Pełny przykład można znaleźć w artykule [Quickstart: create a data factory using .NET (Szybki start: tworzenie fabryki danych przy użyciu platformy .NET)](quickstart-create-data-factory-dot-net.md).

> [!NOTE]
> Interfejs API .NET umożliwia wywoływanie potoków usługi Data Factory z usługi Azure Functions, własnych usług internetowych itp.

## <a name="triggers"></a>Wyzwalacze
Wyzwalacze to druga metoda wykonywania potoków. Wyzwalacze reprezentują jednostki przetwarzania, które określają, kiedy należy rozpocząć wykonanie potoku. Obecnie usługa Data Factory obsługuje dwa rodzaje wyzwalaczy: 1)**Wyzwalacz harmonogramu** — wyzwalacz, który wywołuje potok zgodnie z harmonogramem zegarowym; 2)**Wyzwalacz okna wirowania** — dla wyzwalaczy działających w okresowych interwałach przy zachowaniu stanu. Obecnie usługa Data Factory nie obsługuje wyzwalaczy opartych na zdarzeniach, takich jak uruchomienie potoku w przypadku otrzymania pliku.

Między potokami i wyzwalaczami występuje relacja wiele-do-wielu. Wiele wyzwalaczy może uruchamiać jeden potok, a jeden wyzwalacz może uruchamiać wiele potoków. W poniższej definicji JSON wyzwalacza właściwość **pipelines** odnosi się do listy potoków wyzwalanych przez określony wyzwalacz oraz wartości parametrów potoku.

### <a name="basic-trigger-definition"></a>Podstawowa definicja wyzwalacza:
```json
    "properties": {
        "name": "MyTrigger",
        "type": "<type of trigger>",
        "typeProperties": {
            …
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
```

## <a name="schedule-trigger"></a>Wyzwalacz harmonogramu
Wyzwalacz harmonogramu uruchamia potoki zgodnie z harmonogramem zegarowym. Wyzwalacz obsługuje opcje okresowe i zaawansowane kalendarza (co tydzień, w poniedziałek o godzinie 17:00 i czwartek o godzinie 21: 00). Wyzwalacz jest elastyczny dzięki brakowi wrażliwości na wzorzec zestawu danych i brakowi rozróżnienia między danymi serii czasowych i innych.

Aby uzyskać bardziej szczegółowe informacje o wyzwalaczach harmonogramu i przykładach, zobacz [Poradnik: tworzenie wyzwalacza harmonogramu](how-to-create-schedule-trigger.md)

## <a name="tumbling-window-trigger"></a>Wyzwalacz okna wirowania
Wyzwalacze okna wirowania to rodzaj wyzwalaczy uruchamianych w określonych odstępach czasu od wskazanego czasu rozpoczęcia przy zachowaniu stanu. Okna wirowania to ciągłe, nienakładające się na siebie serie odstępów czasu o stałych rozmiarach.
Aby uzyskać więcej informacji o wyzwalaczach okna wirowania i przykładach, zobacz [Poradnik: tworzenie wyzwalacza okna wirowania](how-to-create-tumbling-window-trigger.md)

### <a name="scheduler-trigger-json-definition"></a>Definicja JSON wyzwalacza harmonogramu
W przypadku utworzenia wyzwalacza harmonogramu można określić planowanie i powtarzanie przy użyciu definicji JSON, jak pokazano na przykładzie w tej sekcji. 

Aby wyzwalacz harmonogramu uruchamiał potok, należy dołączyć odwołanie do konkretnego potoku do definicji wyzwalacza. Między potokami i wyzwalaczami występuje relacja wiele-do-wielu. Wiele wyzwalaczy może uruchomić jeden potok. Jeden wyzwalacz może uruchamiać wiele potoków.

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Year>>,
        "interval": <<int>>,             // how often to fire
        "startTime": <<datetime>>,
        "endTime": <<datetime>>,
        "timeZone": "UTC"
        "schedule": {                    // optional (advanced scheduling specifics)
          "hours": [<<0-24>>],
          "weekDays": ": [<<Monday-Sunday>>],
          "minutes": [<<0-60>>],
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


### <a name="overview-scheduler-trigger-schema"></a>Omówienie: schemat wyzwalacza harmonogramu
Poniższa tabela zawiera ogólne omówienie głównych elementów odnoszących się do powtarzania i planowania w wyzwalaczu:

Właściwość JSON |     Opis
------------- | -------------
startTime | Właściwość startTime ma format data-godzina. W przypadku prostych harmonogramów startTime to pierwsze wystąpienie. W przypadku harmonogramów złożonych wyzwalacz nie jest uruchamiany wcześniej niż startTime.
endTime | Określa datę i godzinę zakończenia wyzwalacza. Po upływie tego czasu wyzwalacz nie jest wykonywany. Wartość endTime nie może być przeszłą datą.
timeZone | Aktualnie obsługiwany jest tylko czas UTC. 
recurrence | Obiekt recurrence określa reguły powtarzania wyzwalacza. Obiekt recurrence obsługuje następujące elementy: frequency, interval, endTime, count i schedule. Zdefiniowanie powtarzalności wymaga zdefiniowania częstotliwości. Inne elementy obiektu recurrence są opcjonalne.
frequency | Reprezentuje jednostkę częstotliwości powtarzania wyzwalacza. Obsługiwane wartości to `minute`, `hour`, `day`, `week` i `month`.
interval | Obiekt interval jest dodatnią liczbą całkowitą. Oznacza on interwał częstotliwości, który określa częstotliwość uruchamiania wyzwalacza. Na przykład jeśli wartość interval to 3, a frequency to „week”, wyzwalacz powtarza się co 3 tygodnie.
schedule | Wyzwalacz z określoną częstotliwością zmienia swój cykl na podstawie harmonogramu cyklu. Harmonogram zawiera modyfikacje oparte na minutach, godzinach, dniach tygodnia, dniach miesiąca i numerze tygodnia.


## <a name="tumbling-window-trigger-vs-schedule-trigger"></a>Wyzwalacz okna wirowania oraz wyzwalacz harmonogramu
Jako że zarówno wyzwalacz okna wirowania, jak i wyzwalacz harmonogramu pracują w oparciu o pulsy czasu, w jaki sposób się różnią?
W przypadku wyzwalacza okna wirowania:
* **Scenariusze wypełniania**: wyzwalacze okna wirowania obsługują scenariusze wypełniania, dzięki czemu mogą planować uruchomienia okien w przeszłości. Wyzwalacz harmonogramu może obsługiwać wyłącznie okresy od teraźniejszości.
* **Niezawodność:** wyzwalacze okna wirowania będą planować uruchomienia potoków dla wszystkich okien od daty początkowej bez przerw z pełną niezawodnością.
* **Ponawianie próby**: wyzwalacze okna wirowania mają możliwość ponawiania próby. Uruchomienia potoków zakończone niepowodzeniem mają domyślne zasady ponawiania próby ustawione na 0 lub zasady określone przez użytkownika w ramach definicji wyzwalacza. Ponadto wyzwalacz będzie automatycznie ponawiać próby w wystąpieniach, gdy uruchomienia zakończą się niepowodzeniem ze względu na limity współbieżności/serwera/ograniczenia przepustowości, tj. przypadki obejmujące kod stanu 400 (błąd użytkownika), 429 (zbyt wiele żądań), 500 (wewnętrzny błąd serwera).
* **Współbieżność**: wyzwalacze okna wirowania umożliwiają użytkownikom jawne ustawienie limitów współbieżności dla wyzwalacza (maksymalnie 1–50 współbieżnych, wyzwolonych uruchomień potoków)
* **Zmienne początku i końca okna**: w przypadku wyzwalaczy okna wirowania użytkownicy mogą uzyskać dostęp do zmiennych triggerOutputs().windowStartTime i triggerOutputs().windowEndTime jako zmiennych systemu wyzwalacza w definicji wyzwalacza. Zmienne opisują odpowiednio czas rozpoczęcia okna i czas zakończenia okna. Przykładowo jeśli masz wyzwalacz okna wirowania uruchamiany co godzinę w przypadku okna od 1:00 do 2:00, triggerOutputs().WindowStartTime = 2017-09-01T01:00:00Z oraz triggerOutputs().WindowEndTime = 2017-09-01T02:00:00Z.
* **Relacja potoku i wyzwalacza**: wyzwalacze harmonogramu mają relację n:m z potokami. Wyzwalacz harmonogramu może wyzwolić wiele potoków. Wyzwalacze okna wirowania mają relację 1:1 z potokami. Wyzwalacz okna wirowania może wyzwalać tylko jeden potok.

### <a name="schedule-trigger-example"></a>Przykładowy wyzwalacz harmonogramu

```json
{
    "properties": {
        "name": "MyTrigger",
        "type": "ScheduleTrigger",
        "typeProperties": {
            "recurrence": {
                "frequency": "Hour",
                "interval": 1,
                "startTime": "2017-11-01T09:00:00-08:00",
                "endTime": "2017-11-02T22:00:00-08:00"
            }
        },
        "pipelines": [{
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "SQLServerToBlobPipeline"
                },
                "parameters": {}
            },
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "SQLServerToAzureSQLPipeline"
                },
                "parameters": {}
            }
        ]
    }
}
```

### <a name="overview-scheduler-trigger-schema-defaults-limits-and-examples"></a>Omówienie: wartości domyślne schematu wyzwalacza harmonogramu, limity i przykłady

Nazwa JSON | Typ wartości | Wymagana? | Wartość domyślna | Prawidłowe wartości | Przykład
--------- | ---------- | --------- | ------------- | ------------ | -------
startTime | Ciąg | Yes | None | Daty i godziny ISO-8601 | ```"startTime" : "2013-01-09T09:30:00-08:00"```
recurrence | Obiekt | Yes | Brak | Obiekt cyklu | ```"recurrence" : { "frequency" : "monthly", "interval" : 1 }```
interval | Liczba | Yes | None | 1 do 1000. | ```"interval":10```
endTime | Ciąg | Yes | None | Wartość daty i godziny reprezentująca godzinę w przyszłości | `"endTime" : "2013-02-09T09:30:00-08:00"`
schedule | Obiekt | Nie | Brak | Obiekt harmonogramu | `"schedule" : { "minute" : [30], "hour" : [8,17] }`

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
minutes | Minuty godziny, o których uruchamiany jest wyzwalacz. | <ul><li>Tablica liczb całkowitych</li></ul>
hours | Godziny dnia, o których uruchamiany jest wyzwalacz. | <ul><li>Tablica liczb całkowitych</li></ul>
weekDays | Dni tygodnia, w które uruchamiany jest wyzwalacz. Można określić tylko z częstotliwością tygodniową. | <ul><li>Tablica dowolnych wartości z poniższych (maksymalny rozmiar tablicy: 7)<ul><li>Poniedziałek</li><li>Wtorek</li><li>Środa</li><li>Czwartek</li><li>Piątek</li><li>Sobota</li><li>Niedziela</li></ul></li></p>Bez uwzględniania wielkości liter</p>
monthlyOccurrences | Określa dni miesiąca, w które uruchamiany jest wyzwalacz. Można określić tylko z częstotliwością miesięczną. | Tablica obiektów monthlyOccurence: `{ "day": day,  "occurrence": occurence }`. <p> Dzień to dzień tygodnia, w którym uruchamiany jest wyzwalacz, na przykład `{Sunday}` to każda niedziela miesiąca. Wymagany.<p>Wystąpienie to wystąpienie dnia w miesiącu, na przykład `{Sunday, -1}` to ostatnia niedziela miesiąca. Opcjonalny.
monthDays | Dzień miesiąca, w którym uruchamiany jest wyzwalacz. Można określić tylko z częstotliwością miesięczną. | <ul><li>Tablica poniższych wartości</li><ul><li>Dowolna wartość <= -1 i >= -31</li><li>Dowolna wartość >= 1 i <= 31</li></ul></ul> |


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




## <a name="next-steps"></a>Następne kroki
Zobacz następujące samouczki:

- [Quickstart: create a data factory using .NET (Szybki start: tworzenie fabryki danych przy użyciu platformy .NET)](quickstart-create-data-factory-dot-net.md)
- [Poradnik: tworzenie wyzwalacza harmonogramu](how-to-create-schedule-trigger.md)
- [Poradnik: tworzenie wyzwalacza okna wirowania](how-to-create-tumbling-window-trigger.md)
