---
title: "Zdarzenia niestandardowe dla usługi Azure Event Grid z programem PowerShell | Microsoft Docs"
description: "Za pomocą usługi Azure Event Grid i programu PowerShell można opublikować temat i subskrybować dane zdarzenie."
services: event-grid
keywords: 
author: tfitzmac
ms.author: tomfitz
ms.date: 01/30/2018
ms.topic: hero-article
ms.service: event-grid
ms.openlocfilehash: 24366df54fa4fc32ebbff7c1303183707dea17c6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="create-and-route-custom-events-with-azure-powershell-and-event-grid"></a>Tworzenie i kierowanie zdarzeń niestandardowych za pomocą programu Azure PowerShell i usługi Event Grid

Azure Event Grid to usługa obsługi zdarzeń dla chmury. W tym artykule omówiono tworzenie tematu niestandardowego, subskrybowanie go i wyzwalanie zdarzenia w celu wyświetlenia wyniku za pomocą programu Azure PowerShell. Zazwyczaj wysyła się zdarzenia do punktu końcowego, który na nie reaguje, takiego jak element webhook lub funkcja platformy Azure. Jednak aby uprościć ten artykuł, omówimy wysłanie zdarzenia na adres URL, który tylko zbiera komunikaty. Do utworzenia tego adresu URL użyte zostaną narzędzia innych producentów: [RequestBin](https://requestb.in/) lub [Hookbin](https://hookbin.com/).

>[!NOTE]
>Narzędzia **RequestBin** i **Hookbin** nie są przeznaczone do używania przy wysokiej przepływności. Te narzędzia zostały tutaj użyte wyłącznie w celach pokazowych. W przypadku wypchnięcia więcej niż jednego zdarzenia w tym samym czasie w narzędziu mogą nie być widoczne wszystkie zdarzenia.

Po zakończeniu przekonasz się, że dane zdarzenia zostały wysłane do punktu końcowego.

![Dane zdarzenia](./media/custom-event-quickstart-powershell/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

Ten artykuł wymaga używania najnowszej wersji programu Azure PowerShell. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Tematy usługi Event Grid to zasoby platformy Azure i muszą być umieszczone w grupie zasobów platformy Azure. Grupa zasobów to kolekcja logiczna przeznaczona do wdrażania zasobów platformy Azure i zarządzania nimi.

Utwórz grupę zasobów na pomocą polecenia [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *gridResourceGroup* w lokalizacji *westus2*.

```powershell
New-AzureRmResourceGroup -Name gridResourceGroup -Location westus2
```

## <a name="create-a-custom-topic"></a>Tworzenie tematu niestandardowego

Temat udostępnia zdefiniowany przez użytkownika punkt końcowy, do którego wysyłane są zdarzenia. Poniższy przykład obejmuje utworzenie tematu w grupie zasobów. Zamień `<topic_name>` na unikatową nazwę tematu. Nazwa tematu musi być unikatowa, ponieważ jest reprezentowana przez wpis DNS.

```powershell
New-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Location westus2 -Name <topic_name>
```

## <a name="create-a-message-endpoint"></a>Tworzenie punktu końcowego komunikatów

Przed zasubskrybowaniem tematu utwórzmy punkt końcowy dla komunikatów o zdarzeniach. Zamiast pisać kod w celu zareagowania na zdarzenie, utwórzmy punkt końcowy, który będzie zbierać komunikaty, aby można było je wyświetlić. RequestBin i Hookbin to narzędzia innych producentów, które umożliwiają utworzenie punktu końcowego i wyświetlanie wysyłanych do niego żądań. Przejdź do narzędzia [RequestBin](https://requestb.in/) i kliknij pozycję **Create a RequestBin** (Utwórz pojemnik na żądania) lub przejdź do narzędzia [Hookbin](https://hookbin.com/) i kliknij pozycję **Create New Endpoint** (Utwórz nowy punkt końcowy).  Skopiuj adres URL pojemnika, ponieważ będzie on potrzebny podczas subskrybowania tematu.

## <a name="subscribe-to-a-topic"></a>Subskrybowanie tematu

Subskrybowanie tematu ma poinformować usługę Event Grid o tym, które zdarzenia chcesz śledzić. Poniższy przykład ilustruje subskrybowanie utworzonego tematu i przekazanie adresu URL z narzędzia RequestBin lub Hookbin jako punktu końcowego dla powiadomień o zdarzeniach. Zamień `<event_subscription_name>` na unikatową nazwę subskrypcji, a `<endpoint_URL>` wartością z poprzedniej sekcji. Dzięki określeniu punktu końcowego podczas subskrybowania usługa Event Grid obsługuje kierowanie zdarzeń do tego punktu końcowego. Jako `<topic_name>` użyj wartości utworzonej wcześniej.

```powershell
New-AzureRmEventGridSubscription -EventSubscriptionName <event_subscription_name> -Endpoint <endpoint_URL> -ResourceGroupName gridResourceGroup -TopicName <topic_name>
```

## <a name="send-an-event-to-your-topic"></a>Wysyłanie zdarzenia do tematu

Teraz wyzwólmy zdarzenie, aby zobaczyć, jak usługa Event Grid dystrybuuje komunikat do punktu końcowego. Po pierwsze uzyskajmy adres URL i klucz dla tematu. Ponownie jako `<topic_name>` użyj nazwy tematu.

```powershell
$endpoint = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name <topic-name>).Endpoint
$keys = Get-AzureRmEventGridTopicKey -ResourceGroupName gridResourceGroup -Name <topic-name>
```

Aby uprościć ten artykuł, skonfiguruj przykładowe dane zdarzenia do wysłania do tematu. Zwykle dane zdarzenia są wysyłane przez aplikację lub usługę platformy Azure. W poniższym przykładzie użyto tabeli skrótów (Hashtable) do skonstruowania danych zdarzenia `htbody`, a następnie przekonwertowano je na poprawnie sformułowany obiekt ładunku JSON `$body`:

```powershell
$eventID = Get-Random 99999

#Date format should be SortableDateTimePattern (ISO 8601)
$eventDate = Get-Date -Format s

#Construct body using Hashtable
$htbody = @{
    id= $eventID
    eventType="recordInserted"
    subject="myapp/vehicles/motorcycles"
    eventTime= $eventDate   
    data= @{
        make="Ducati"
        model="Monster"
    }
    dataVersion="1.0"
}

#Use ConvertTo-Json to convert event body from Hashtable to JSON Object
#Append square brackets to the converted JSON payload since they are expected in the event's JSON payload syntax
$body = "["+(ConvertTo-Json $htbody)+"]"
```

Jeśli wyświetlisz `$body`, zobaczysz pełne zdarzenie. Element `data` danych JSON to ładunek zdarzenia. W tym polu można umieścić dowolną poprawnie sformułowaną zawartość JSON. Można też używać pola tematu do zaawansowanego routingu i filtrowania.

Teraz wyślij zdarzenie do swojego tematu.

```powershell
Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
```

Zdarzenie zostało wyzwolone, a usługa Event Grid wysłała komunikat do punktu końcowego skonfigurowanego podczas subskrybowania. Przejdź do adresu URL punktu końcowego utworzonego wcześniej. Ewentualnie kliknij przycisk Refresh (Odśwież) w otwartej przeglądarce. Zobaczysz właśnie wysłane zdarzenie.

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2018-01-25T15:58:13",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/topics/{topic}"
}]
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli zamierzasz kontynuować pracę z tym zdarzeniem, nie usuwaj zasobów utworzonych w tym artykule. Jeśli nie planujesz kontynuowania pracy, użyj poniższego polecenia, aby usunąć zasoby utworzone w ramach tego artykułu.

```powershell
Remove-AzureRmResourceGroup -Name gridResourceGroup
```

## <a name="next-steps"></a>Następne kroki

Wiesz już, jak tworzyć tematy i subskrypcje zdarzeń. Dowiedz się więcej na temat tego, co może Ci ułatwić usługa Event Grid:

- [Event Grid — informacje](overview.md)
- [Kierowanie zdarzeń usługi Blob Storage do niestandardowego internetowego punktu końcowego](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Monitorowanie zmian maszyn wirtualnych za pomocą usług Azure Event Grid i Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md)
- [Przesyłanie strumieniowe danych Big Data do magazynu danych](event-grid-event-hubs-integration.md)
