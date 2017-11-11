---
title: "Azure schematu zdarzeń centra zdarzeń siatki zdarzeń"
description: "Opisuje właściwości, które są dostępne dla zdarzeń centra zdarzeń Azure zdarzeń siatki"
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/07/2017
ms.author: tomfitz
ms.openlocfilehash: 80959ee589a1cfcf317a98c3bafd7f92c796fc2d
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2017
---
# <a name="azure-event-grid-event-schema-for-event-hubs"></a>Azure schematu zdarzeń siatki zdarzeń usługi event hubs

Ten artykuł zawiera właściwości i schematu dla zdarzeń centrów zdarzeń. Aby obejrzeć wprowadzenie do schematów zdarzeń, zobacz [schematu zdarzeń siatki zdarzeń Azure](event-schema.md).

### <a name="available-event-types"></a>Typy dostępnych zdarzeń

Centra zdarzeń emituje **Microsoft.EventHub.CaptureFileCreated** typ zdarzenia podczas tworzenia pliku przechwytywania.

## <a name="example-event"></a>Przykład zdarzeń

To zdarzenie próbkowania zawiera schemat centra zdarzeń zdarzenia wywoływane, gdy funkcja przechwytywania są przechowywane w pliku: 

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        }
    }
]
```

## <a name="event-properties"></a>Właściwości zdarzenia

Zdarzenie ma następujące dane najwyższego poziomu:

| Właściwość | Typ | Opis |
| -------- | ---- | ----------- |
| Temat | Ciąg | Zasobów Pełna ścieżka do źródła zdarzeń. To pole nie jest zapisywalny. |
| Temat | Ciąg | Ścieżka zdefiniowana wydawcy podmiotem zdarzeń. |
| Typ zdarzenia | Ciąg | Jeden z typów zdarzeń zarejestrowane dla tego źródła zdarzenia. |
| eventTime | Ciąg | Czas jest generowane zdarzenie oparte na czas UTC dostawcy. |
| id | Ciąg | Unikatowy identyfikator zdarzenia. |
| Dane | Obiekt | Dane zdarzenia Centrum zdarzeń. |

Obiekt danych ma następujące właściwości:

| Właściwość | Typ | Opis |
| -------- | ---- | ----------- |
| fileUrl | Ciąg | Ścieżka do pliku przechwytywania. |
| Typ pliku | Ciąg | Typ pliku przechwytywania. |
| partitionId | Ciąg | Identyfikator niezależnego fragmentu. |
| sizeInBytes | Liczba całkowita | Rozmiar pliku. |
| eventCount | Liczba całkowita | Liczba zdarzeń w pliku. |
| firstSequenceNumber | Liczba całkowita | Najmniejsza liczba sekwencji z kolejki. |
| lastSequenceNumber | Liczba całkowita | Ostatni numer sekwencji z kolejki. |
| firstEnqueueTime | Ciąg | Przy pierwszym z kolejki. |
| lastEnqueueTime | Ciąg | Czas ostatniego z kolejki. |

## <a name="next-steps"></a>Następne kroki

* Aby obejrzeć wprowadzenie do usługi Azure Event siatki, zobacz [co to jest zdarzenie siatki?](overview.md)
* Aby uzyskać więcej informacji o tworzeniu subskrypcji platformy Azure zdarzeń siatki, zobacz [schematu subskrypcji zdarzeń siatki](subscription-creation-schema.md).
* Aby uzyskać informacje na temat obsługi zdarzeń centra zdarzeń, zobacz [strumienia danych big data do magazynu danych](event-grid-event-hubs-integration.md).