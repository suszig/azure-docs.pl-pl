---
title: Zaplanowane zdarzenia dla maszyn wirtualnych systemu Windows na platformie Azure | Dokumentacja firmy Microsoft
description: "Zaplanowane zdarzenia przy użyciu usługi Azure metadanych dla na maszynach wirtualnych systemu Windows."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 28d8e1f2-8e61-4fbe-bfe8-80a68443baba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2017
ms.author: zivr
ms.openlocfilehash: 7198fa8d1a512d10ca7022078aa2ea7bde3a4c02
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-metadata-service-scheduled-events-preview-for-windows-vms"></a>Usługa Azure metadanych: Zaplanowanego zdarzenia (wersja zapoznawcza) dla maszyn wirtualnych systemu Windows

> [!NOTE] 
> Podglądy są udostępniane użytkownikowi, pod warunkiem że wyrażasz zgodę na warunki użytkowania. Aby uzyskać więcej informacji, zobacz [Dodatkowe warunki użytkowania dotyczące wersji zapoznawczych platformy Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

Zaplanowane zdarzenia jest jednym z subservices w usłudze Azure metadanych. Jest odpowiedzialny za udostępniając informacje dotyczące zdarzeń nadchodzących (na przykład ponowny rozruch), aplikacja może przygotować je i ograniczyć przerw w działaniu. Jest ona dostępna dla wszystkich typów maszyny wirtualnej platformy Azure, w tym PaaS i IaaS. Zaplanowane zdarzenia daje czasu maszyny wirtualnej do wykonywania zadań zapobiegawcze, aby zminimalizować wpływ zdarzeń. 

Zaplanowane zdarzenia jest dostępna dla maszyn wirtualnych systemu Windows i Linux. Informacje o zdarzeniach zaplanowane w systemie Linux, zobacz [zaplanowane zdarzenia dla maszyn wirtualnych systemu Linux](../windows/scheduled-events.md).

## <a name="why-scheduled-events"></a>Dlaczego zaplanowane zdarzenia?

Bez zaplanowane zdarzenia należy wykonać kroki, aby ograniczyć wpływ intiated platformy konserwacji lub akcje inicjowane przez użytkownika w usłudze. 

Mająca wiele wystąpień obciążeń, które są używane do zarządzania stanem technik replikacji, mogą być narażone na awarie wykonywane w wielu wystąpieniach. Takie jak awarie może spowodować kosztowne zadania (na przykład odbudowywania indeksów) lub nawet utraty repliki. 

W wielu innych przypadkach ogólnych dostępności usługi mogą być ulepszane wykonując sekwencji łagodne zamykanie, takie jak pula modułu równoważenia obciążenia Kończenie (lub anulowanie) locie transakcji, ponowne przypisywanie zadań do innych maszyn wirtualnych w klastrze (ręcznego przełączania trybu failover) lub usuwanie maszyny wirtualnej z sieci. 

Istnieją przypadki, w którym powiadamianie o tym administratora o nadchodzących zdarzenia lub rejestrowanie takie zdarzenia pomocy poprawy użytkowanie aplikacji hostowanych w chmurze.

Usługa Azure metadanych udostępnia zaplanowane zdarzenia w następujących przypadkach użycia:
-   Platforma zainicjował konserwacji (na przykład wdrożenie systemu operacyjnego hosta)
-   Wywołania zainicjowane przez użytkownika (na przykład użytkownik zostanie ponownie uruchomiony lub wdraża ponownie maszyny Wirtualnej)


## <a name="the-basics"></a>Podstawy  

Usługa Azure metadanych przedstawia informacje o uruchamianiu maszyn wirtualnych przy użyciu punkt końcowy REST jest dostępny z poziomu maszyny Wirtualnej. Informacje są dostępne za pośrednictwem IP bez obsługi routingu, dzięki czemu nie jest uwidaczniana poza maszyny Wirtualnej.

### <a name="scope"></a>Zakres
Zaplanowane zdarzenia są udostępniane dla wszystkich maszyn wirtualnych w usłudze w chmurze lub dla wszystkich maszyn wirtualnych w zestawie dostępności. W związku z tym należy sprawdzić `Resources` w zdarzenia w celu określenia, na które będzie to mieć wpływ na maszynach wirtualnych. 

### <a name="discovering-the-endpoint"></a>Odnajdywanie punktu końcowego
W przypadku, gdy utworzono maszynę wirtualną w sieć wirtualną (VNet), usługa metadanych jest dostępne ze statycznego adresu IP bez obsługi routingu, `169.254.169.254`.
Jeśli maszyna wirtualna nie została utworzona w ramach sieci wirtualnej, a domyślnym czcionkom dla usługi w chmurze i klasycznych maszyn wirtualnych, dodatkową logikę jest potrzebny do odnajdywania punktu końcowego do użycia. Odwołuje się do poniższego przykładu, aby dowiedzieć się, jak [odnajdywanie punktu końcowego hosta](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).

### <a name="versioning"></a>Przechowywanie wersji 
Wystąpienie usługi metadanych jest kontrolą wersji. Wersje są obowiązkowe i bieżąca wersja to `2017-03-01`.

> [!NOTE] 
> Poprzednie wersje Podgląd zdarzeń zaplanowane {najnowszych} obsługiwana jako wersja interfejsu api. Ten format jest już obsługiwane i zostaną wycofane w przyszłości.

### <a name="using-headers"></a>Korzystanie z nagłówków
Kwerenda usługi metadanych, należy określić nagłówek `Metadata: true` aby upewnić się, żądanie nie zostało przekierowane przypadkowo.

### <a name="enabling-scheduled-events"></a>Włączanie zaplanowanego zdarzenia
Utwórz żądanie zaplanowanego zdarzenia po raz pierwszy Azure niejawnie włączy tę funkcję na maszynie wirtualnej. W związku z tym spodziewać opóźnione odpowiedzi w Twoje pierwsze wywołanie do dwóch minut.

### <a name="user-initiated-maintenance"></a>Konserwacja zainicjowanej przez użytkownika
Użytkownik zainicjował konserwacji maszyny wirtualnej za pośrednictwem portalu Azure, interfejsu API, interfejsu wiersza polecenia lub środowiska PowerShell powoduje zaplanowane zdarzenie. To umożliwia przetestowanie logiki przygotowania konserwacji w aplikacji oraz umożliwia aplikacji w taki sposób przygotować się do obsługi inicjowanych przez użytkownika.

Ponowne uruchomienie maszyny wirtualnej planuje zdarzenia z typem `Reboot`. Ponownego wdrażania maszyny wirtualnej planuje zdarzenia z typem `Redeploy`.

> [!NOTE] 
> Obecnie maksymalnie 10 operacji konserwacji zainicjowanej przez użytkownika można jednocześnie zaplanować. Ten limit zostanie złagodzony przed zaplanowane zdarzenia ogólnej dostępności.

> [!NOTE] 
> Wynikiem zaplanowane zdarzenia konserwacji zainicjowanej przez użytkownika nie jest obecnie można konfigurować. Konfigurowalne jest planowane w przyszłości.

## <a name="using-the-api"></a>Korzystanie z interfejsu API

### <a name="query-for-events"></a>Zapytania do zdarzenia
Mogą wykonywać kwerendę o zaplanowane zdarzenia po prostu, wprowadzając następujące wywołanie:

```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

Odpowiedź zawiera tablicę zaplanowanego zdarzenia. Pusta tablica oznacza, że obecnie nie istnieją żadne zdarzenia według harmonogramu.
W przypadku w przypadku zaplanowanego zdarzenia odpowiedzi zawiera tablicę zdarzeń: 
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},              
        }
    ]
}
```

### <a name="event-properties"></a>Właściwości zdarzenia
|Właściwość  |  Opis |
| - | - |
| Identyfikator zdarzenia | Globalnie unikatowy identyfikator dla tego zdarzenia. <br><br> Przykład: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| Typ zdarzenia | Wpływ powoduje, że to zdarzenie. <br><br> Wartości: <br><ul><li> `Freeze`: Planowane maszyny wirtualnej jest wstrzymywać w kilka sekund. Procesor jest wstrzymana, ale nie ma żadnego wpływu na pamięć, otwartych plików lub połączeń sieciowych. <li>`Reboot`: Ponowne uruchomienie zaplanowano maszyny wirtualnej (z systemem innym niż trwałej pamięci jest utracone). <li>`Redeploy`: Zaplanowano maszynę wirtualną można przenieść do innego węzła (tymczasowych dyski zostaną utracone). |
| ResourceType | Typ zasobu, który ma wpływ na to zdarzenie. <br><br> Wartości: <ul><li>`VirtualMachine`|
| Zasoby| Lista zasobów, które ma wpływ na to zdarzenie. Gwarantuje zawiera maszyny z co najwyżej jeden [domeny aktualizacji](manage-availability.md), ale nie może zawierać wszystkie maszyny w UD. <br><br> Przykład: <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| Stan zdarzenia | Stan tego zdarzenia. <br><br> Wartości: <ul><li>`Scheduled`: To zdarzenie jest zaplanowane do uruchomienia po upływie czasu określonego w `NotBefore` właściwości.<li>`Started`: To zdarzenie zostało rozpoczęte.</ul> Nie `Completed` lub podobne stan kiedykolwiek jest dostarczana, zdarzenie nie zostanie zwrócony po zakończeniu zdarzenia.
| Nie wcześniej niż| Czas, po którym to zdarzenie może zostać uruchomiony. <br><br> Przykład: <br><ul><li> 2016-09-19T18:29:47Z  |

### <a name="event-scheduling"></a>Planowanie zdarzeń
Zaplanowano każdego zdarzenia minimalna ilość czasu w przyszłości oparty na typie zdarzenia. Ten czas jest odzwierciedlone w zdarzeniu `NotBefore` właściwości. 

|Typ zdarzenia  | Minimalna powiadomień |
| - | - |
| Blokowanie| 15 minut |
| Ponowne uruchamianie | 15 minut |
| Ponowne wdrożenie | 10 minut |

### <a name="starting-an-event"></a>Uruchamianie zdarzenia 

Po rozpoznane nadchodzących zdarzenia i ukończyć logiki dla łagodne zamykanie należy zatwierdzić zaległych zdarzeń dokonując `POST` wywołanie usługi metadanych z `EventId`. To wskazuje na platformie Azure skrócić minimalna powiadomień czasu (jeśli jest to możliwe). 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> Potwierdzeniem zdarzenia umożliwia zdarzeń w przypadku wszystkich `Resources` w przypadku, nie tylko w maszynie wirtualnej, który potwierdza zdarzenia. W związku z tym można wybrać opcję wybiera wiodące do koordynowania potwierdzenia, które mogą być prosta jako maszynę pierwszej w `Resources` pola.


## <a name="powershell-sample"></a>Przykładowe programu PowerShell 

Poniższy przykład korzysta z usługi metadanych dla zaplanowanych zdarzeń i zatwierdza każdego zdarzenia oczekujące.

```PowerShell
# How to get scheduled events 
function GetScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How to approve a scheduled event
function ApproveScheduledEvent($eventId, $docIncarnation, $uri)
{    
    # Create the Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests; "DocumentIncarnation" = $docIncarnation} 
    
    # Convert to JSON string
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with the following: `n" $approvalString

    # Post approval string to scheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

function HandleScheduledEvents($scheduledEvents)
{
    # Add logic for handling events here
}

######### Sample Scheduled Events Interaction #########

# Set up the scheduled events URI for a VNET-enabled VM
$localHostIP = "169.254.169.254"
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2017-03-01' -f $localHostIP 

# Get events
$scheduledEvents = GetScheduledEvents $scheduledEventURI

# Handle events however is best for your service
HandleScheduledEvents $scheduledEvents

# Approve events when ready (optional)
foreach($event in $scheduledEvents.Events)
{
    Write-Host "Current Event: `n" $event
    $entry = Read-Host "`nApprove event? Y/N"
    if($entry -eq "Y" -or $entry -eq "y")
    {
        ApproveScheduledEvent $event.EventId $scheduledEvents.DocumentIncarnation $scheduledEventURI 
    }
}
``` 


## <a name="c-sample"></a>C\# próbki 

Poniższy przykład jest proste klienta, który komunikuje się z usługą metadanych.

```csharp
public class ScheduledEventsClient
{
    private readonly string scheduledEventsEndpoint;
    private readonly string defaultIpAddress = "169.254.169.254"; 

    // Set up the scheduled events URI for a VNET-enabled VM
    public ScheduledEventsClient()
    {
        scheduledEventsEndpoint = string.Format("http://{0}/metadata/scheduledevents?api-version=2017-03-01", defaultIpAddress);
    }

    // Get events
    public string GetScheduledEvents()
    {
        Uri cloudControlUri = new Uri(scheduledEventsEndpoint);
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Metadata", "true");
            return webClient.DownloadString(cloudControlUri);
        }   
    }

    // Approve events
    public void ApproveScheduledEvents(string jsonPost)
    {
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Content-Type", "application/json");
            webClient.UploadString(scheduledEventsEndpoint, jsonPost);
        }
    }
}
```

Zaplanowane zdarzenia mogą być przedstawiane za pomocą następujących struktur danych:

```csharp
public class ScheduledEventsDocument
{
    public string DocumentIncarnation;
    public List<CloudControlEvent> Events { get; set; }
}

public class CloudControlEvent
{
    public string EventId { get; set; }
    public string EventStatus { get; set; }
    public string EventType { get; set; }
    public string ResourceType { get; set; }
    public List<string> Resources { get; set; }
    public DateTime? NotBefore { get; set; }
}

public class ScheduledEventsApproval
{
    public string DocumentIncarnation;
    public List<StartRequest> StartRequests = new List<StartRequest>();
}

public class StartRequest
{
    [JsonProperty("EventId")]
    private string eventId;

    public StartRequest(string eventId)
    {
        this.eventId = eventId;
    }
}
```

Poniższy przykład korzysta z usługi metadanych dla zaplanowanych zdarzeń i zatwierdza każdego zdarzenia oczekujące.

```csharp
public class Program
{
    static ScheduledEventsClient client;

    static void Main(string[] args)
    {
        client = new ScheduledEventsClient();

        while (true)
        {
            string json = client.GetDocument();
            ScheduledEventsDocument scheduledEventsDocument = JsonConvert.DeserializeObject<ScheduledEventsDocument>(json);

            HandleEvents(scheduledEventsDocument.Events);

            // Wait for user response
            Console.WriteLine("Press Enter to approve executing events\n");
            Console.ReadLine();

            // Approve events
            ScheduledEventsApproval scheduledEventsApprovalDocument = new ScheduledEventsApproval()
            {
                DocumentIncarnation = scheduledEventsDocument.DocumentIncarnation
            };
        
            foreach (CloudControlEvent event in scheduledEventsDocument.Events)
            {
                scheduledEventsApprovalDocument.StartRequests.Add(new StartRequest(event.EventId));
            }

            if (scheduledEventsApprovalDocument.StartRequests.Count > 0)
            {
                // Serialize using Newtonsoft.Json
                string approveEventsJsonDocument =
                    JsonConvert.SerializeObject(scheduledEventsApprovalDocument);

                Console.WriteLine($"Approving events with json: {approveEventsJsonDocument}\n");
                client.ApproveScheduledEvents(approveEventsJsonDocument);
            }

            Console.WriteLine("Complete. Press enter to repeat\n\n");
            Console.ReadLine();
            Console.Clear();
        }
    }

    private static void HandleEvents(List<CloudControlEvent> events)
    {
        // Add logic for handling events here
    }
}
```

## <a name="python-sample"></a>Przykładowe Python 

Poniższy przykład korzysta z usługi metadanych dla zaplanowanych zdarzeń i zatwierdza każdego zdarzenia oczekujące.

```python
#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01"
headers = "{Metadata:true}"
this_host = socket.gethostname()

def get_scheduled_events():
   req = urllib2.Request(metadata_url)
   req.add_header('Metadata', 'true')
   resp = urllib2.urlopen(req)
   data = json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid = evt['EventId']
        status = evt['EventStatus']
        resources = evt['Resources']
        eventtype = evt['EventType']
        resourcetype = evt['ResourceType']
        notbefore = evt['NotBefore'].replace(" ","_")
        if this_host in resources:
            print "+ Scheduled Event. This host is scheduled for " + eventype + " not before " + notbefore
            # Add logic for handling events here

def main():
   data = get_scheduled_events()
   handle_scheduled_events(data)
   
if __name__ == '__main__':
  main()
  sys.exit(0)
```

## <a name="next-steps"></a>Następne kroki 

- Dowiedz się więcej o interfejsami API dostępnymi w [metadanych wystąpienia usługi](instance-metadata-service.md).
- Dowiedz się więcej o [zaplanowanej konserwacji dla maszyn wirtualnych systemu Windows Azure](planned-maintenance.md).

