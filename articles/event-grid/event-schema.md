---
title: "Azure schematu zdarzeń siatki zdarzeń"
description: "Opisuje właściwości, które są dostępne dla zdarzeń o Azure zdarzeń siatki"
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/07/2017
ms.author: babanisa
ms.openlocfilehash: caa709fdc2a59472ee812bde91f7300396aa5755
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2017
---
# <a name="azure-event-grid-event-schema"></a>Azure schematu zdarzeń siatki zdarzeń

W tym artykule opisano właściwości i schematu, które znajdują się dla wszystkich zdarzeń. Zdarzenia składają się z zbiór właściwości pięć wymaganych parametrów obiektu wymaganych danych. Właściwości są wspólne dla wszystkich zdarzeń z dowolnego wydawcę. Obiekt danych zawiera właściwości, które są specyficzne dla każdego wydawcy. Tematy systemu te właściwości są specyficzne dla dostawcy zasobu, takiego jak magazyn Azure lub usługi Azure Event Hubs.

Zdarzenia są wysyłane do usługi Azure Event siatki w tablicy, która może zawierać wiele obiektów zdarzeń. W przypadku pojedynczego zdarzenia tablicy ma długość 1. Tablica może mieć całkowity rozmiar maksymalnie 1 MB. Każde zdarzenie w tablicy jest ograniczony do 64 KB.

## <a name="event-schema"></a>Schemat zdarzeń

W poniższym przykładzie przedstawiono właściwości, które są używane przez wszystkich wydawców zdarzeń:

```json
[
  {
    "topic": string,
    "subject": string,    
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    }
  }
]
```

Na przykład schemat opublikowana na potrzeby zdarzenia magazynu obiektów Blob platformy Azure jest:

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
 
## <a name="event-properties"></a>Właściwości zdarzenia

Wszystkie zdarzenia zawierają tych samych danych najwyższego poziomu następujące:

| Właściwość | Typ | Opis |
| -------- | ---- | ----------- |
| Temat | Ciąg | Zasobów Pełna ścieżka do źródła zdarzeń. To pole nie jest zapisywalny. |
| Temat | Ciąg | Ścieżka zdefiniowana wydawcy podmiotem zdarzeń. |
| Typ zdarzenia | Ciąg | Jeden z typów zdarzeń zarejestrowane dla tego źródła zdarzenia. |
| eventTime | Ciąg | Czas jest generowane zdarzenie oparte na czas UTC dostawcy. |
| id | Ciąg | Unikatowy identyfikator zdarzenia. |
| Dane | Obiekt | Dane zdarzenia specyficzne dla dostawcy zasobów. |

Aby dowiedzieć się więcej na temat właściwości w obiekcie danych, zobacz źródło zdarzenia:

* [Subskrypcje platformy Azure (operacje zarządzania)](event-schema-subscriptions.md)
* [Blob Storage](event-schema-blob-storage.md)
* [Centra zdarzeń](event-schema-event-hubs.md)
* [Grupy zasobów (operacje zarządzania)](event-schema-resource-groups.md)

W przypadku niestandardowych tematów wydawca zdarzeń określa obiekt danych. Danych najwyższego poziomu powinien zawierać te same pola jako standardowych zdarzeń zdefiniowanych zasobów. Podczas publikowania zdarzeń w niestandardowych tematów, należy rozważyć modelowania przedmiotem zdarzeń ułatwiających routingu i filtrowania.

## <a name="next-steps"></a>Następne kroki

* Aby obejrzeć wprowadzenie do usługi Azure Event siatki, zobacz [co to jest zdarzenie siatki?](overview.md)
* Aby uzyskać więcej informacji o tworzeniu subskrypcji platformy Azure zdarzeń siatki, zobacz [schematu subskrypcji zdarzeń siatki](subscription-creation-schema.md).
