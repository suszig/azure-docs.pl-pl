---
title: "Azure schematu zdarzeń siatki zdarzeń"
description: "Opisuje właściwości, które są dostępne dla zdarzeń o Azure zdarzeń siatki"
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 10/20/2017
ms.author: babanisa
ms.openlocfilehash: e251cbfe7c4d8dfbd492817a8fa7af48e6b379df
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2017
---
# <a name="azure-event-grid-event-schema"></a>Azure schematu zdarzeń siatki zdarzeń

Ten artykuł zawiera właściwości i schematu dla zdarzeń. Zdarzenia składają się z zbiór właściwości pięć wymaganych parametrów obiektu wymaganych danych. Właściwości są wspólne dla wszystkich zdarzeń z dowolnego wydawcę. Obiekt danych zawiera właściwości, które są specyficzne dla każdego wydawcy. Tematy systemu te właściwości są specyficzne dla dostawcy zasobu, takiego jak magazyn Azure lub usługi Azure Event Hubs.

Zdarzenia są wysyłane do usługi Azure Event siatki w tablicy, która może zawierać wiele obiektów zdarzeń. W przypadku pojedynczego zdarzenia tablicy ma długość 1. Tablica może mieć całkowity rozmiar maksymalnie 1 MB. Każde zdarzenie w tablicy jest ograniczony do 64 KB.
 
## <a name="event-properties"></a>Właściwości zdarzenia

Wszystkie zdarzenia będzie zawierać te same dane najwyższego poziomu następujące:

| Właściwość | Typ | Opis |
| -------- | ---- | ----------- |
| Temat | Ciąg | Zasobów Pełna ścieżka do źródła zdarzeń. To pole nie jest zapisywalny. |
| Temat | Ciąg | Ścieżka zdefiniowana wydawcy podmiotem zdarzeń. |
| Typ zdarzenia | Ciąg | Jeden z typów zdarzeń zarejestrowane dla tego źródła zdarzenia. |
| eventTime | Ciąg | Czas jest generowane zdarzenie oparte na czas UTC dostawcy. |
| id | Ciąg | Unikatowy identyfikator zdarzenia. |
| Dane | Obiekt | Dane zdarzenia specyficzne dla dostawcy zasobów. |

## <a name="available-event-sources"></a>Źródła zdarzeń dostępne

Następujące źródła zdarzeń publikowanie zdarzeń zużycia za pośrednictwem siatki zdarzeń:

* Grupy zasobów (operacje zarządzania)
* Subskrypcje platformy Azure (operacje zarządzania)
* Centra zdarzeń
* Niestandardowe — tematy

## <a name="azure-subscriptions"></a>Subskrypcje platformy Azure

Subskrypcji platformy Azure można teraz Emituj zdarzeń zarządzania z usługi Azure Resource Manager, takie jak podczas tworzenia maszyny Wirtualnej lub na konto magazynu zostało usunięte.

### <a name="available-event-types"></a>Typy dostępnych zdarzeń

- **Microsoft.Resources.ResourceWriteSuccess**: wywoływane, gdy zasób utworzyć ani zaktualizować operacji zakończy się pomyślnie.  
- **Microsoft.Resources.ResourceWriteFailure**: wywoływane, gdy tworzenie zasobu lub operacja aktualizacji nie powiedzie się.  
- **Microsoft.Resources.ResourceWriteCancel**: wywoływane, gdy zasób utworzyć ani zaktualizować operacji zostało anulowane.  
- **Microsoft.Resources.ResourceDeleteSuccess**: zgłaszane w przypadku powodzenia operacji usuwania zasobów.  
- **Microsoft.Resources.ResourceDeleteFailure**: wywoływane, gdy operacja usuwania zasobu nie powiedzie się.  
- **Microsoft.Resources.ResourceDeleteCancel**: wywoływane, gdy operacja usuwania zasobów została anulowana. Dzieje się tak podczas wdrażania szablonu zostaną anulowane.

### <a name="example-event-schema"></a>Przykład schematu zdarzeń

```json
[
    {
    "topic":"/subscriptions/{subscription-id}",
    "subject":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "eventType":"Microsoft.Resources.ResourceWriteSuccess",
    "eventTime":"2017-08-16T03:54:38.2696833Z",
    "id":"25b3b0d0-d79b-44d5-9963-440d4e6a9bba",
    "data": {
        "authorization":"{azure_resource_manager_authorizations}",
        "claims":"{azure_resource_manager_claims}",
        "correlationId":"54ef1e39-6a82-44b3-abc1-bdeb6ce4d3c6",
        "httpRequest":"",
        "resourceProvider":"Microsoft.EventGrid",
        "resourceUri":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
        "operationName":"Microsoft.EventGrid/eventSubscriptions/write",
        "status":"Succeeded",
        "subscriptionId":"{subscription-id}",
        "tenantId":"72f988bf-86f1-41af-91ab-2d7cd011db47"
        },
    }
]
```



## <a name="resource-groups"></a>Grupy zasobów

Grup zasobów można teraz Emituj zdarzeń zarządzania z usługi Azure Resource Manager, takie jak podczas tworzenia maszyny Wirtualnej lub na konto magazynu zostało usunięte.

### <a name="available-event-types"></a>Typy dostępnych zdarzeń

- **Microsoft.Resources.ResourceWriteSuccess**: wywoływane, gdy zasób utworzyć ani zaktualizować operacji zakończy się pomyślnie.  
- **Microsoft.Resources.ResourceWriteFailure**: wywoływane, gdy tworzenie zasobu lub operacja aktualizacji nie powiedzie się.  
- **Microsoft.Resources.ResourceWriteCancel**: wywoływane, gdy zasób utworzyć ani zaktualizować operacji zostało anulowane.  
- **Microsoft.Resources.ResourceDeleteSuccess**: zgłaszane w przypadku powodzenia operacji usuwania zasobów.  
- **Microsoft.Resources.ResourceDeleteFailure**: wywoływane, gdy operacja usuwania zasobu nie powiedzie się.  
- **Microsoft.Resources.ResourceDeleteCancel**: wywoływane, gdy operacja usuwania zasobów została anulowana. Dzieje się tak podczas wdrażania szablonu zostaną anulowane.

### <a name="example-event"></a>Przykład zdarzeń

```json
[
    {
    "topic":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}",
    "subject":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "eventType":"Microsoft.Resources.ResourceWriteSuccess",
    "eventTime":"2017-08-16T03:54:38.2696833Z",
    "id":"25b3b0d0-d79b-44d5-9963-440d4e6a9bba",
    "data": {
        "authorization":"{azure_resource_manager_authorizations}",
        "claims":"{azure_resource_manager_claims}",
        "correlationId":"54ef1e39-6a82-44b3-abc1-bdeb6ce4d3c6",
        "httpRequest":"",
        "resourceProvider":"Microsoft.EventGrid",
        "resourceUri":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
        "operationName":"Microsoft.EventGrid/eventSubscriptions/write",
        "status":"Succeeded",
        "subscriptionId":"{subscription-id}",
        "tenantId":"72f988bf-86f1-41af-91ab-2d7cd011db47"
        },
    }
]
```



## <a name="event-hubs"></a>Usługa Event Hubs

Zdarzenia centra zdarzeń obecnie są emitowane tylko wtedy, gdy plik jest automatycznie przesyłany do magazynu za pomocą funkcji przechwytywania.

### <a name="available-event-types"></a>Typy dostępnych zdarzeń

- **Microsoft.EventHub.CaptureFileCreated**: wywoływane po utworzeniu pliku przechwytywania.

### <a name="example-event"></a>Przykład zdarzeń

To zdarzenie próbkowania zawiera schemat usługi Event Hubs zdarzenia wywoływane, gdy funkcja przechwytywania są przechowywane w pliku: 

```json
[
    {
        "topic": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{event-hubs-ns}",
        "subject": "eventhubs/eh1",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-07-11T00:55:55.0120485Z",
        "id": "bd440490-a65e-4c97-8298-ef1eb325673c",
        "data": {
            "fileUrl": "https://gridtest1.blob.core.windows.net/acontainer/eventgridtest1/eh1/1/2017/07/11/00/54/54.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 0,
            "eventCount": 0,
            "firstSequenceNumber": -1,
            "lastSequenceNumber": -1,
            "firstEnqueueTime": "0001-01-01T00:00:00",
            "lastEnqueueTime": "0001-01-01T00:00:00"
        },
    }
]

```


## <a name="azure-blob-storage"></a>Azure Blob Storage

### <a name="available-event-types"></a>Typy dostępnych zdarzeń

- **Microsoft.Storage.BlobCreated**: wywoływane po utworzeniu obiektu blob.
- **Microsoft.Storage.BlobDeleted**: wywoływane po usunięciu obiektu blob.

### <a name="example-event"></a>Przykład zdarzeń

To zdarzenie próbkowania zawiera schemat magazynu zdarzenia wywoływane po utworzeniu obiektu blob: 

```json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
    "subject": "/blobServices/default/containers/oc2d2817345i200097container/blobs/oc2d2817345i20002296blob",
    "eventType": "Microsoft.Storage.BlobCreated",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
      "api": "PutBlockList",
      "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
      "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
      "eTag": "0x8D4BCC2E4835CD0",
      "contentType": "application/octet-stream",
      "contentLength": 524288,
      "blobType": "BlockBlob",
      "url": "https://oc2d2817345i60006.blob.core.windows.net/oc2d2817345i200097container/oc2d2817345i20002296blob",
      "sequencer": "00000000000004420000000000028963",
      "storageDiagnostics": {
        "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
      }
    }
  }
]
```




## <a name="custom-topics"></a>Niestandardowe — tematy

Ładunek danych zdarzenia niestandardowe jest zdefiniowane przez użytkownika i może być poprawnie sformatowany obiekt JSON. Danych najwyższego poziomu powinien zawierać te same pola jako standardowych zdarzeń zdefiniowanych zasobów. Podczas publikowania zdarzeń w niestandardowych tematów, należy rozważyć modelowania przedmiotem zdarzeń ułatwiających routingu i filtrowania.

### <a name="example-event"></a>Przykład zdarzeń

W poniższym przykładzie przedstawiono zdarzenie dla niestandardowego tematu:
````json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.EventGrid/topics/myeventgridtopic",
    "subject": "/myapp/vehicles/motorcycles",    
    "id": "b68529f3-68cd-4744-baa4-3c0498ec19e2",
    "eventType": "recordInserted",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "data":{
      "make": "Ducati",
      "model": "Monster"
    }
  }
]

````

## <a name="next-steps"></a>Następne kroki

* Aby obejrzeć wprowadzenie do usługi Azure Event siatki, zobacz [co to jest zdarzenie siatki?](overview.md).
* Aby uzyskać więcej informacji o tworzeniu subskrypcji platformy Azure zdarzeń siatki, zobacz [schematu subskrypcji zdarzeń siatki](subscription-creation-schema.md).
