---
title: "Zdarzenia niestandardowe dla usługi Azure Event Grid przy użyciu witryny Azure Portal | Microsoft Docs"
description: "Za pomocą usługi Azure Event Grid i programu PowerShell można opublikować temat i subskrybować dane zdarzenie."
services: event-grid
keywords: 
author: djrosanova
ms.author: darosa
ms.date: 10/11/2017
ms.topic: hero-article
ms.service: event-grid
ms.openlocfilehash: 0fe498b7b6dcf59bc5caef8ff5a40053e0498f85
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2017
---
# <a name="create-and-route-custom-events-with-the-azure-portal-and-event-grid"></a>Tworzenie i kierowanie zdarzeń niestandardowych za pomocą witryny Azure Portal i usługi Event Grid

Azure Event Grid to usługa obsługi zdarzeń dla chmury. W tym artykule omówiono tworzenie tematu niestandardowego, subskrybowanie go i wyzwalanie zdarzenia w celu wyświetlenia wyniku za pomocą witryny Azure Portal. Zazwyczaj wysyła się zdarzenia do punktu końcowego, który na nie reaguje, takiego jak element webhook lub funkcja platformy Azure. Jednak aby uprościć ten artykuł, omówimy wysłanie zdarzenia na adres URL, który tylko zbiera komunikaty. Utworzysz ten adres URL przy użyciu narzędzia open source innego producenta o nazwie [RequestBin](https://requestb.in/).

>[!NOTE]
>**RequestBin** to narzędzie typu „open source”, które nie jest przeznaczone do użycia przy wysokiej przepływności. To narzędzie zostało tutaj użyte wyłącznie w celach pokazowych. W przypadku wypchnięcia więcej niż jednego zdarzenia w tym samym czasie w narzędziu mogą nie być widoczne wszystkie zdarzenia.

Po zakończeniu przekonasz się, że dane zdarzenia zostały wysłane do punktu końcowego.

![Dane zdarzenia](./media/custom-event-quickstart-portal/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Tematy usługi Event Grid to zasoby platformy Azure i muszą być umieszczone w grupie zasobów platformy Azure. Grupa zasobów to kolekcja logiczna przeznaczona do wdrażania zasobów platformy Azure i zarządzania nimi.

1. W lewym obszarze nawigacji wybierz pozycję **Grupy zasobów**. Następnie wybierz pozycję **Dodaj**.

   ![Tworzenie grupy zasobów](./media/custom-event-quickstart-portal/create-resource-group.png)

1. Ustaw nazwę grupy zasobów na *gridResourceGroup* i lokalizację na *westus2*. Wybierz pozycję **Utwórz**.

   ![Podawanie wartości grupy zasobów](./media/custom-event-quickstart-portal/provide-resource-group-values.png)

## <a name="create-a-custom-topic"></a>Tworzenie tematu niestandardowego

Temat udostępnia zdefiniowany przez użytkownika punkt końcowy, do którego wysyłane są zdarzenia. 

1. Aby utworzyć temat w swojej grupie zasobów, wybierz pozycję **Więcej usług** i wyszukaj frazę *event grid*. Z dostępnych opcji wybierz opcję **Tematy usługi Event Grid**.

   ![Tworzenie tematu usługi Event Grid](./media/custom-event-quickstart-portal/create-event-grid-topic.png)

1. Wybierz pozycję **Dodaj**.

   ![Dodawanie tematu usługi Event Grid](./media/custom-event-quickstart-portal/add-topic.png)

1. Podaj nazwę tematu. Nazwa tematu musi być unikatowa, ponieważ jest reprezentowana przez wpis DNS. W wersji zapoznawczej usługa Event Grid obsługuje lokalizacje **westus2** i **westcentralus**. Wybierz grupę zasobów, która została utworzona wcześniej. Wybierz pozycję **Utwórz**.

   ![Podawanie wartości tematu usługi Event Grid](./media/custom-event-quickstart-portal/provide-topic-values.png)

1. Po utworzeniu tematu wybierz pozycję **Odśwież**, aby wyświetlić temat.

   ![Wyświetlanie tematu usługi Event Grid](./media/custom-event-quickstart-portal/see-topic.png)

## <a name="create-a-message-endpoint"></a>Tworzenie punktu końcowego komunikatów

Przed zasubskrybowaniem tematu utwórzmy punkt końcowy dla komunikatów o zdarzeniach. Zamiast pisać kod w celu zareagowania na zdarzenie, utwórzmy punkt końcowy, który będzie zbierać komunikaty, aby można było je wyświetlić. RequestBin to narzędzie open source innego producenta, które umożliwia utworzenie punktu końcowego i wyświetlanie wysyłanych do niego żądań. Przejdź do narzędzia [RequestBin](https://requestb.in/) i kliknij pozycję **Create a RequestBin** (Utwórz pojemnik na żądania).  Skopiuj adres URL pojemnika, ponieważ będzie on potrzebny podczas subskrybowania tematu.

## <a name="subscribe-to-a-topic"></a>Subskrybowanie tematu

Subskrybowanie tematu ma poinformować usługę Event Grid o tym, które zdarzenia chcesz śledzić. 

1. Aby utworzyć subskrypcję usługi Event Grid, wybierz ponownie pozycję **Więcej usług** i wyszukaj frazę *event grid*. Z dostępnych opcji wybierz opcję **Subskrypcje usługi Event Grid**.

   ![Tworzenie subskrypcji usługi Event Grid](./media/custom-event-quickstart-portal/create-subscription.png)

1. Wybierz pozycję **+ Subskrypcja zdarzeń**.

   ![Dodawanie subskrypcji usługi Event Grid](./media/custom-event-quickstart-portal/add-subscription.png)

1. Podaj unikatową nazwę dla Twojej subskrypcji zdarzeń. Jako typ tematu wybierz **Tematy usługi Event Grid**. Dla wystąpienia wybierz niestandardowy temat, który został utworzony. Podaj adres URL z narzędzia RequestBin jako punkt końcowy powiadomienia o zdarzeniu. Po zakończeniu podawania wartości wybierz pozycję **Utwórz**.

   ![Podawanie wartości subskrypcji usługi Event Grid](./media/custom-event-quickstart-portal/provide-subscription-values.png)

Teraz wyzwólmy zdarzenie, aby zobaczyć, jak usługa Event Grid dystrybuuje komunikat do punktu końcowego. Aby uprościć ten artykuł, użyliśmy usługi Cloud Shell do wysłania przykładowych danych zdarzenia do tematu. Zwykle dane zdarzenia są wysyłane przez aplikację lub usługę platformy Azure.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="send-an-event-to-your-topic"></a>Wysyłanie zdarzenia do tematu

Po pierwsze uzyskajmy adres URL i klucz dla tematu. Użyj nazwy tematu dla `<topic_name>`.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

Poniższy przykład pobiera przykładowe dane zdarzenia:

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
```

Użycie opcji `echo "$body"` pozwala wyświetlić pełne zdarzenie. Element `data` danych JSON to ładunek zdarzenia. W tym polu można umieścić dowolną poprawnie sformułowaną zawartość JSON. Można też używać pola tematu do zaawansowanego routingu i filtrowania.

CURL to narzędzie, które wykonuje żądania HTTP. W tym artykule używamy narzędzia CURL do wysłania zdarzenia do tematu. 

```azurecli-interactive
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

Zdarzenie zostało wyzwolone, a usługa Event Grid wysłała komunikat do punktu końcowego skonfigurowanego podczas subskrybowania. Przejdź do adresu URL pojemnika RequestBin utworzonego wcześniej. Ewentualnie kliknij przycisk Refresh (Odśwież) w otwartej przeglądarce narzędzia RequestBin. Zobaczysz właśnie wysłane zdarzenie.

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2017-08-10T21:03:07+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/topics/{topic}"
}]
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli zamierzasz kontynuować pracę z tym zdarzeniem, nie usuwaj zasobów utworzonych w tym artykule. Jeśli nie planujesz kontynuowania pracy, usuń zasoby utworzone w ramach tego artykułu.

Wybierz grupę zasobów, a następnie wybierz pozycję **Usuń grupę zasobów**.

## <a name="next-steps"></a>Następne kroki

Wiesz już, jak tworzyć tematy i subskrypcje zdarzeń. Dowiedz się więcej na temat tego, co może Ci ułatwić usługa Event Grid:

- [Event Grid — informacje](overview.md)
- [Kierowanie zdarzeń usługi Blob Storage do niestandardowego internetowego punktu końcowego](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Monitorowanie zmian maszyn wirtualnych za pomocą usług Azure Event Grid i Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md)
- [Przesyłanie strumieniowe danych Big Data do magazynu danych](event-grid-event-hubs-integration.md)
