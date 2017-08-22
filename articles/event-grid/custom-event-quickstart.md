---
title: "Zdarzenia niestandardowe dla usługi Azure Event Grid | Microsoft Docs"
description: "Za pomocą usługi Azure Event Grid można opublikować temat i subskrybować dane zdarzenie."
services: event-grid
keywords: 
author: djrosanova
ms.author: darosa
ms.date: 08/15/2017
ms.topic: hero-article
ms.service: event-grid
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: 5bfe08c249d01f5e01cd4cd82f609201da7eccbc
ms.contentlocale: pl-pl
ms.lasthandoff: 08/17/2017

---

# <a name="create-and-route-custom-events-with-azure-event-grid"></a>Tworzenie i kierowanie zdarzeń niestandardowych za pomocą usługi Azure Event Grid

Azure Event Grid to usługa obsługi zdarzeń dla chmury. W tym artykule omówiono tworzenie tematu niestandardowego, subskrybowanie go i wyzwalanie zdarzenia w celu wyświetlenia wyniku za pomocą interfejsu wiersza polecenia platformy Azure. Zazwyczaj wysyła się zdarzenia do punktu końcowego, który na nie reaguje, takiego jak element webhook lub funkcja platformy Azure. Jednak aby uprościć ten artykuł, omówimy wysłanie zdarzenia na adres URL, który tylko zbiera komunikaty. Utworzysz ten adres URL przy użyciu narzędzia open source innego producenta o nazwie [RequestBin](https://requestb.in/).

Po zakończeniu przekonasz się, że dane zdarzenia zostały wysłane do punktu końcowego.

![Dane zdarzenia](./media/custom-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten artykuł będzie wymagał interfejsu wiersza polecenia platformy Azure w najnowszej wersji (2.0.14 lub nowszej). Aby dowiedzieć się, jaka wersja jest używana, uruchom polecenie `az --version`. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Tematy usługi Event Grid to zasoby platformy Azure i muszą być umieszczone w grupie zasobów platformy Azure. Grupa zasobów to kolekcja logiczna przeznaczona do wdrażania zasobów platformy Azure i zarządzania nimi.

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). 

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *gridResourceGroup* w lokalizacji *westus2*.

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a>Tworzenie tematu niestandardowego

Temat udostępnia zdefiniowany przez użytkownika punkt końcowy, do którego wysyłane są zdarzenia. Poniższy przykład obejmuje utworzenie tematu w grupie zasobów. Zamień `<topic_name>` na unikatową nazwę tematu. Nazwa tematu musi być unikatowa, ponieważ jest reprezentowana przez wpis DNS. W wersji zapoznawczej usługa Event Grid obsługuje lokalizacje **westus2** i **westcentralus**.

```azurecli-interactive
az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="create-a-message-endpoint"></a>Tworzenie punktu końcowego komunikatów

Przed zasubskrybowaniem tematu utwórzmy punkt końcowy dla komunikatów o zdarzeniach. Zamiast pisać kod w celu zareagowania na zdarzenie, utwórzmy punkt końcowy, który będzie zbierać komunikaty, aby można było je wyświetlić. RequestBin to narzędzie open source innego producenta, które umożliwia utworzenie punktu końcowego i wyświetlanie wysyłanych do niego żądań. Przejdź do narzędzia [RequestBin](https://requestb.in/) i kliknij pozycję **Create a RequestBin** (Utwórz pojemnik na żądania).  Skopiuj adres URL pojemnika, ponieważ będzie on potrzebny podczas subskrybowania tematu.

## <a name="subscribe-to-a-topic"></a>Subskrybowanie tematu

Subskrybowanie tematu ma poinformować usługę Event Grid o tym, które zdarzenia chcesz śledzić. Poniższy przykład ilustruje subskrybowanie utworzonego tematu i przekazanie adresu URL z narzędzia RequestBin jako punktu końcowego dla powiadomień o zdarzeniach. Zamień `<event_subscription_name>` na unikatową nazwę subskrypcji, a `<URL_from_RequestBin>` wartością z poprzedniej sekcji. Dzięki określeniu punktu końcowego podczas subskrybowania usługa Event Grid obsługuje kierowanie zdarzeń do tego punktu końcowego. Jako `<topic_name>` użyj wartości utworzonej wcześniej. 

```azurecli-interactive
az eventgrid topic event-subscription create --name <event_subscription_name> \
  --endpoint <URL_from_RequestBin> \
  -g gridResourceGroup \
  --topic-name <topic_name>
```

## <a name="send-an-event-to-your-topic"></a>Wysyłanie zdarzenia do tematu

Teraz wyzwólmy zdarzenie, aby zobaczyć, jak usługa Event Grid dystrybuuje komunikat do punktu końcowego. Po pierwsze uzyskajmy adres URL i klucz dla tematu. Ponownie jako `<topic_name>` użyj nazwy tematu.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

Aby uprościć ten artykuł, skonfigurowaliśmy przykładowe dane zdarzenia do wysłania do tematu. Zwykle dane zdarzenia są wysyłane przez aplikację lub usługę platformy Azure. Poniższy przykład pobiera dane zdarzenia:

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
Jeśli zamierzasz kontynuować pracę z tym zdarzeniem, nie usuwaj zasobów utworzonych w tym artykule. Jeśli nie planujesz kontynuowania pracy, użyj poniższego polecenia, aby usunąć zasoby utworzone w ramach tego artykułu.

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a>Następne kroki

Wiesz już, jak tworzyć tematy i subskrypcje zdarzeń. Dowiedz się więcej na temat tego, co może Ci ułatwić usługa Event Grid:

- [Event Grid — informacje](overview.md)
- [Monitorowanie zmian maszyn wirtualnych za pomocą usług Azure Event Grid i Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md)
