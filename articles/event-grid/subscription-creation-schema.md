---
title: "Azure schematu subskrypcji zdarzeń siatki"
description: "Opisuje właściwości subskrybowanie zdarzenie Azure zdarzeń siatki."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 03/09/2018
ms.author: babanisa
ms.openlocfilehash: 888196225ec5998405113842344469d02a2cf5c7
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="event-grid-subscription-schema"></a>Schematu subskrypcji siatki zdarzeń

Aby utworzyć subskrypcję zdarzeń siatki, Wyślij żądanie do operacji tworzenia zdarzenia subskrypcji. Użyj następującego formatu:

```
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Na przykład można utworzyć subskrypcji zdarzeń dla konta magazynu o nazwie `examplestorage` w grupie zasobów o nazwie `examplegroup`, użyj następującego formatu:

```
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Artykuł opisuje właściwości i schematu dla treści żądania.
 
## <a name="event-subscription-properties"></a>Właściwości subskrypcji zdarzeń

| Właściwość | Typ | Opis |
| -------- | ---- | ----------- |
| Miejsce docelowe | obiekt | Obiekt, który definiuje punktu końcowego. |
| filtr | obiekt | Pole opcjonalne do filtrowania typów zdarzeń. |

### <a name="destination-object"></a>obiekt docelowy

| Właściwość | Typ | Opis |
| -------- | ---- | ----------- |
| endpointType | ciąg | Typ punktu końcowego dla subskrypcji (webhook/HTTP Centrum zdarzeń i kolejki). | 
| endpointUrl | ciąg | Docelowy adres URL zdarzenia w tej subskrypcji zdarzeń. | 

### <a name="filter-object"></a>obiekt filtru

| Właściwość | Typ | Opis |
| -------- | ---- | ----------- |
| includedEventTypes | tablica | Dopasowania, gdy typ zdarzenia w komunikacie zdarzenia jest dokładnym odpowiednikiem do jednej z następujących nazw typów zdarzeń. Zgłasza błąd, jeśli nazwa zdarzenia nie są zgodne z nazwy typu zdarzenia zarejestrowane dla źródła zdarzenia. Domyślne dopasowuje wszystkie typy zdarzeń. |
| subjectBeginsWith | ciąg | Dopasowanie prefiksu filtrowanie zdarzeń do pola temat wiadomości. Domyślne lub pusty ciąg pasuje do wszystkich. | 
| subjectEndsWith | ciąg | Sufiks match filtrowane w celu pole tematu zdarzeń komunikatów. Domyślne lub pusty ciąg pasuje do wszystkich. |
| subjectIsCaseSensitive | ciąg | Formanty z uwzględnieniem wielkości liter dopasowanie filtrów. |


## <a name="example-subscription-schema"></a>Przykład subskrypcji schematu

```json
{
  "properties": {
    "destination": {
      "endpointType": "webhook",
      "properties": {
          "endpointUrl": "https://example.azurewebsites.net/api/HttpTriggerCSharp1?code=VXbGWce53l48Mt8wuotr0GPmyJ/nDT4hgdFj9DpBiRt38qqnnm5OFg=="
      }
    },
    "filter": {
      "includedEventTypes": [ "Microsoft.Storage.BlobCreated", "Microsoft.Storage.BlobDeleted" ],
      "subjectBeginsWith": "blobServices/default/containers/mycontainer/log",
      "subjectEndsWith": ".jpg",
      "subjectIsCaseSensitive": "true"
    }
  }
}
```

## <a name="next-steps"></a>Kolejne kroki

* Aby obejrzeć wprowadzenie do siatki zdarzeń, zobacz [co to jest zdarzenie siatki?](overview.md)