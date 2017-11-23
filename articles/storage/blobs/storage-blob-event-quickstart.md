---
title: "Kierowanie zdarzeń usługi Azure Blob Storage do niestandardowego internetowego punktu końcowego (wersja zapoznawcza) | Microsoft Docs"
description: "Zasubskrybuj zdarzenia usługi Blob Storage przy użyciu usługi Azure Event Grid."
services: storage,event-grid
keywords: 
author: cbrooksmsft
ms.author: cbrooks
ms.date: 08/18/2017
ms.topic: article
ms.service: storage
ms.openlocfilehash: 67f262913333fb69f5b862fa3d862c0d773e4172
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/23/2017
---
# <a name="route-blob-storage-events-to-a-custom-web-endpoint-preview"></a>Kierowanie zdarzeń usługi Blob Storage do niestandardowego internetowego punktu końcowego (wersja zapoznawcza)

Azure Event Grid to usługa obsługi zdarzeń dla chmury. W tym artykule omówiono subskrybowanie zdarzeń usługi Blob Storage i wyzwalanie zdarzenia w celu wyświetlenia wyniku za pomocą interfejsu wiersza polecenia platformy Azure. 

Zazwyczaj wysyła się zdarzenia do punktu końcowego, który na nie reaguje, takiego jak element webhook lub funkcja platformy Azure. Aby uprościć przykład w tym artykule, omówimy wysłanie zdarzeń na adres URL, który tylko zbiera komunikaty. Utworzysz ten adres URL przy użyciu narzędzia open source innego producenta o nazwie [RequestBin](https://requestb.in/).

> [!NOTE]
> **RequestBin** to narzędzie typu „open source”, które nie jest przeznaczone do użycia przy wysokiej przepływności. To narzędzie zostało tutaj użyte wyłącznie w celach pokazowych. W przypadku wypchnięcia więcej niż jednego zdarzenia w tym samym czasie w narzędziu mogą nie być widoczne wszystkie zdarzenia.

Po wykonaniu czynności opisanych w tym artykule dane powinny zostać wysłane do punktu końcowego.

![Dane zdarzenia](./media/storage-blob-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten artykuł będzie wymagał interfejsu wiersza polecenia platformy Azure w najnowszej wersji (2.0.14 lub nowszej). Aby dowiedzieć się, jaka wersja jest używana, uruchom polecenie `az --version`. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli).

Jeśli nie korzystasz z usługi Cloud Shell, musisz się najpierw zalogować za pomocą polecenia `az login`.

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Tematy usługi Event Grid to zasoby platformy Azure i muszą być umieszczone w grupie zasobów platformy Azure. Grupa zasobów to kolekcja logiczna przeznaczona do wdrażania zasobów platformy Azure i zarządzania nimi.

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). 

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie `<resource_group_name>` w lokalizacji *westcentralus*.  Zamień `<resource_group_name>` na unikatową nazwę grupy zasobów.

```azurecli-interactive
az group create --name <resource_group_name> --location westcentralus
```

## <a name="create-a-blob-storage-account"></a>Tworzenie konta usługi Blob Storage

Aby móc użyć usługi Azure Storage, musisz mieć konto magazynu.  Zdarzenia usługi Blob Storage są obecnie dostępne tylko w ramach kont usługi Blob Storage.

Konto usługi Blob Storage to specjalne konto magazynu służące do przechowywania danych niestrukturalnych w formie obiektów blob w usłudze Azure Storage. Konta usługi Blob Storage są podobne do istniejących kont magazynu ogólnego przeznaczenia i udostępniają wszystkie używane obecnie funkcje doskonałej trwałości, dostępności, skalowalności i wydajności, łącznie z pełną spójnością interfejsu API na potrzeby blokowych i uzupełnialnych obiektów blob. W przypadku aplikacji wymagających tylko magazynu obiektów blokowych lub uzupełnialnych obiektów blob zalecamy używanie kont usługi Blob Storage.

> [!NOTE]
> Siatka zdarzenie jest obecnie w wersji zapoznawczej i jest dostępna tylko w przypadku kont magazynu w **westcentralus** i **westus2** regionów.

Zamień `<storage_account_name>` na unikatową nazwę konta magazynu oraz `<resource_group_name>` na wcześniej utworzoną grupę zasobów.

```azurecli-interactive
az storage account create \
  --name <storage_account_name> \
  --location westcentralus \
  --resource-group <resource_group_name> \
  --sku Standard_LRS \
  --kind BlobStorage \
  --access-tier Hot
```

## <a name="create-a-message-endpoint"></a>Tworzenie punktu końcowego komunikatów

Przed zasubskrybowaniem zdarzeń w ramach konta usługi Blob Storage utwórzmy punkt końcowy dla komunikatów o zdarzeniach. Zamiast pisać kod w celu zareagowania na zdarzenie, utworzymy punkt końcowy, który będzie zbierać komunikaty, aby można było je wyświetlić. RequestBin to narzędzie open source innego producenta, które umożliwia utworzenie punktu końcowego i wyświetlanie wysyłanych do niego żądań. Przejdź do narzędzia [RequestBin](https://requestb.in/) i kliknij pozycję **Create a RequestBin** (Utwórz pojemnik na żądania).  Skopiuj adres URL pojemnika, ponieważ będzie on potrzebny podczas subskrybowania tematu.

## <a name="subscribe-to-your-blob-storage-account"></a>Subskrybowanie konta usługi Blob Storage

Subskrybowanie tematu ma poinformować usługę Event Grid o tym, które zdarzenia chcesz śledzić. Poniższy przykład ilustruje subskrybowanie utworzonego konta usługi Blob Storage i przekazanie adresu URL z narzędzia RequestBin jako punktu końcowego dla powiadomień o zdarzeniach. Zamień `<event_subscription_name>` na unikatową nazwę subskrypcji zdarzeń oraz `<URL_from_RequestBin>` na wartość z poprzedniej sekcji. Dzięki określeniu punktu końcowego podczas subskrybowania usługa Event Grid obsługuje kierowanie zdarzeń do tego punktu końcowego. Jako parametrów `<resource_group_name>` i `<storage_account_name>` użyj utworzonych wcześniej wartości. 

```azurecli-interactive
az eventgrid resource event-subscription create \
--endpoint <URL_from_RequestBin> \
--name <event_subscription_name> \
--provider-namespace Microsoft.Storage \
--resource-type storageAccounts \
--resource-group <resource_group_name> \
--resource-name <storage_account_name>
```

## <a name="trigger-an-event-from-blob-storage"></a>Wyzwalanie zdarzenia z usługi Blob Storage

Teraz wyzwólmy zdarzenie, aby zobaczyć, jak usługa Event Grid dystrybuuje komunikat do punktu końcowego. Po pierwsze umożliwia konfigurowanie nazwy i klucza dla konta magazynu, firma Microsoft będzie utworzyć kontener, a następnie utworzyć i przekazać plik. Jako parametrów `<storage_account_name>` i `<resource_group_name>` ponownie użyj utworzonych wcześniej wartości.

```azurecli-interactive
export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_ACCESS_KEY="$(az storage account keys list --account-name <storage_account_name> --resource-group <resource_group_name> --query "[0].value" --output tsv)"

az storage container create --name testcontainer

touch testfile.txt
az storage blob upload --file testfile.txt --container-name testcontainer --name testfile.txt
```

Zdarzenie zostało wyzwolone, a usługa Event Grid wysłała komunikat do punktu końcowego skonfigurowanego podczas subskrybowania. Przejdź do adresu URL pojemnika RequestBin utworzonego wcześniej. Ewentualnie kliknij przycisk Refresh (Odśwież) w otwartej przeglądarce narzędzia RequestBin. Zobaczysz właśnie wysłane zdarzenie. 

```json
[{
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Storage/storageAccounts/myblobstorageaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-08-16T20:33:51.0595757Z",
  "id": "4d96b1d4-0001-00b3-58ce-16568c064fab",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "d65ca2e2-a168-4155-b7a4-2c925c18902f",
    "requestId": "4d96b1d4-0001-00b3-58ce-16568c000000",
    "eTag": "0x8D4E4E61AE038AD",
    "contentType": "text/plain",
    "contentLength": 0,
    "blobType": "BlockBlob",
    "url": "https://myblobstorageaccount.blob.core.windows.net/testcontainer/testblob1.txt",
    "sequencer": "00000000000000EB0000000000046199",
    "storageDiagnostics": {
      "batchId": "dffea416-b46e-4613-ac19-0371c0c5e352"
    }
  }
}]

```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Jeśli zamierzasz kontynuować pracę z tym kontem magazynu i tą subskrypcją zdarzeń, nie usuwaj zasobów utworzonych w tym artykule. Jeśli nie planujesz kontynuowania pracy, użyj poniższego polecenia, aby usunąć zasoby utworzone w ramach tego artykułu.

Zamień `<resource_group_name>` na utworzoną powyżej grupę zasobów.

```azurecli-interactive
az group delete --name <resource_group_name>
```

## <a name="next-steps"></a>Następne kroki

Wiesz już, jak tworzyć tematy i subskrypcje zdarzeń. Dowiedz się więcej na temat tego, co mogą Ci ułatwić zdarzenia usługi Blob Storage i usługa Event Grid:

- [Reagowanie na zdarzenia usługi Blob Storage](storage-blob-event-overview.md)
- [Event Grid — informacje](../../event-grid/overview.md)
