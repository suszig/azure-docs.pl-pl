---
title: "Wysyłanie zdarzeń do środowiska usługi Azure Time Series Insights | Microsoft Docs"
description: "Ten samouczek przedstawia kroki wypychania zdarzeń do środowiska usługi Time Series Insights"
keywords: 
services: tsi
documentationcenter: 
author: venkatgct
manager: jhubbard
editor: 
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/21/2017
ms.author: venkatja
ms.openlocfilehash: b4ef96a045393f28b3cd750068fe82a5a8411afa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="send-events-to-a-time-series-insights-environment-using-event-hub"></a>Wysyłanie zdarzeń do środowiska usługi Time Series Insights za pomocą centrum zdarzeń

W tym samouczku wyjaśniono, jak utworzyć i skonfigurować centrum zdarzeń oraz jak uruchomić przykładową aplikację do wypychania zdarzeń. Jeśli masz już centrum zdarzeń, które zawiera zdarzenia w formacie JSON, możesz pominąć ten samouczek i wyświetlić swoje środowisko, korzystając z [wglądu w dane szeregów czasowych](https://insights.timeseries.azure.com).

## <a name="configure-an-event-hub"></a>Konfigurowanie centrum zdarzeń
1. Aby utworzyć centrum zdarzeń, wykonaj instrukcje zawarte w [dokumentacji](https://docs.microsoft.com/azure/event-hubs/event-hubs-create) centrum zdarzeń.

2. Upewnij się, że utworzona grupa odbiorców jest używana wyłącznie przez źródło zdarzeń usługi Time Series Insights.

  > [!IMPORTANT]
  > Sprawdź, czy ta grupa odbiorców nie jest używana przez żadną inną usługę (np. zadanie usługi Stream Analytics lub drugie środowisko usługi Time Series Insights). Używanie grupy odbiorców przez inne usługi ma negatywny wpływ na operacje odczytu w bieżącym środowisku i tych usługach. Użycie grupy odbiorców „$Default” może potencjalnie spowodować jej ponowne użycie przez inne czytniki.

  ![Wybieranie grupy odbiorców centrum zdarzeń](media/send-events/consumer-group.png)

3. W centrum zdarzeń utwórz zasady „Moje_zasady_wysyłania”, które w poniższym przykładzie w języku csharp będą używane do wysyłania zdarzeń.

  ![Wybieranie zasad dostępu współdzielonego i klikanie przycisku Dodaj](media/send-events/shared-access-policy.png)  

  ![Dodawanie nowej zasady dostępu współdzielonego](media/send-events/shared-access-policy-2.png)  

## <a name="create-time-series-insights-event-source"></a>Tworzenie źródła zdarzeń usługi Time Series Insights
1. Jeśli nie utworzono źródła zdarzeń, postępuj zgodnie z [tymi instrukcjami](time-series-insights-add-event-source.md), aby je utworzyć.

2. Wpisz „deviceTimestamp” jako nazwę właściwości sygnatury czasowej — właściwość ta jest używana w przykładzie w języku csharp jako rzeczywista sygnatura czasowa. W nazwie właściwości sygnatury czasowej jest uwzględniana wielkość liter. Wartości wysyłane do centrum zdarzeń jako dane JSON muszą mieć format __rrrr-MM-ddTGG:mm:ss.FFFFFFFK__. Jeśli w zdarzeniu nie ma tej właściwości, używany jest czas umieszczenia zdarzenia w kolejce w centrum zdarzeń.

  ![Tworzenie źródła zdarzeń](media/send-events/event-source-1.png)

## <a name="sample-code-to-push-events"></a>Przykładowy kod umożliwiający wypychanie zdarzeń
1. Przejdź do zasad centrum zdarzeń „Moje_zasady_wysyłania” i skopiuj parametry połączenia z kluczem zasad.

  ![Kopiowanie parametrów połączenia Moja_zasada_wysyłania](media/send-events/sample-code-connection-string.png)

2. Uruchom poniższy kod, który umożliwia wysyłanie 600 zdarzeń do każdego z trzech urządzeń. Zaktualizuj zmienną `eventHubConnectionString` przy użyciu parametrów połączenia.

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.Rdx.DataGenerator
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            var from = new DateTime(2017, 4, 20, 15, 0, 0, DateTimeKind.Utc);
            Random r = new Random();
            const int numberOfEvents = 600;

            var deviceIds = new[] { "device1", "device2", "device3" };

            var events = new List<string>();
            for (int i = 0; i < numberOfEvents; ++i)
            {
                for (int device = 0; device < deviceIds.Length; ++device)
                {
                    // Generate event and serialize as JSON object:
                    // { "deviceTimestamp": "utc timestamp", "deviceId": "guid", "value": 123.456 }
                    events.Add(
                        String.Format(
                            CultureInfo.InvariantCulture,
                            @"{{ ""deviceTimestamp"": ""{0}"", ""deviceId"": ""{1}"", ""value"": {2} }}",
                            (from + TimeSpan.FromSeconds(i * 30)).ToString("o"),
                            deviceIds[device],
                            r.NextDouble()));
                }
            }

            // Create event hub connection.
            var eventHubConnectionString = @"Endpoint=sb://...";
            var eventHubClient = EventHubClient.CreateFromConnectionString(eventHubConnectionString);

            using (var ms = new MemoryStream())
            using (var sw = new StreamWriter(ms))
            {
                // Wrap events into JSON array:
                sw.Write("[");
                for (int i = 0; i < events.Count; ++i)
                {
                    if (i > 0)
                    {
                        sw.Write(',');
                    }
                    sw.Write(events[i]);
                }
                sw.Write("]");

                sw.Flush();
                ms.Position = 0;

                // Send JSON to event hub.
                EventData eventData = new EventData(ms);
                eventHubClient.Send(eventData);
            }
        }
    }
}

```
## <a name="supported-json-shapes"></a>Obsługiwane kształty JSON
### <a name="sample-1"></a>Przykład 1

#### <a name="input"></a>Dane wejściowe

Prosty obiekt JSON.

```json
{
    "id":"device1",
    "timestamp":"2016-01-08T01:08:00Z"
}
```
#### <a name="output---1-event"></a>Dane wyjściowe — jedno zdarzenie

|id|sygnatura czasowa|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|

### <a name="sample-2"></a>Przykład 2

#### <a name="input"></a>Dane wejściowe
Tablica JSON z dwoma obiektami JSON. Każdy obiekt JSON zostanie przekształcony na zdarzenie.
```json
[
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    },
    {
        "id":"device2",
        "timestamp":"2016-01-17T01:17:00Z"
    }
]
```
#### <a name="output---2-events"></a>Dane wyjściowe — dwa zdarzenia

|id|sygnatura czasowa|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|
|device2|2016-01-08T01:17:00Z|
### <a name="sample-3"></a>Przykład 3

#### <a name="input"></a>Dane wejściowe

Obiekt JSON z zagnieżdżoną tablicą JSON zawierającą dwa obiekty JSON.
```json
{
    "location":"WestUs",
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z"
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z"
        }
    ]
}

```
#### <a name="output---2-events"></a>Dane wyjściowe — dwa zdarzenia
Zwróć uwagę na to, że właściwość „location” jest kopiowana do każdego zdarzenia.

|location|events.id|events.timestamp|
|--------|---------------|----------------------|
|WestUs|device1|2016-01-08T01:08:00Z|
|WestUs|device2|2016-01-08T01:17:00Z|

### <a name="sample-4"></a>Przykład 4

#### <a name="input"></a>Dane wejściowe

Obiekt JSON z zagnieżdżoną tablicą JSON zawierającą dwa obiekty JSON. Te dane wejściowe pokazują, że globalne właściwości mogą być reprezentowane przez złożony obiekt JSON.

```json
{
    "location":"WestUs",
    "manufacturer":{
        "name":"manufacturer1",
        "location":"EastUs"
    },
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z",
            "data":{
                "type":"pressure",
                "units":"psi",
                "value":108.09
            }
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z",
            "data":{
                "type":"vibration",
                "units":"abs G",
                "value":217.09
            }
        }
    ]
}
```
#### <a name="output---2-events"></a>Dane wyjściowe — dwa zdarzenia

|location|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
|---|---|---|---|---|---|---|---|
|WestUs|manufacturer1|EastUs|device1|2016-01-08T01:08:00Z|pressure|psi|108.09|
|WestUs|manufacturer1|EastUs|device2|2016-01-08T01:17:00Z|vibration|abs G|217.09|

## <a name="next-steps"></a>Następne kroki

* Wyświetlanie środowiska w [Portalu usługi Time Series Insights](https://insights.timeseries.azure.com)
