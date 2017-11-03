---
title: "Azure schematu subskrypcji zdarzeń siatki"
description: "Opisuje właściwości subskrybowanie zdarzenie Azure zdarzeń siatki."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/17/2017
ms.author: babanisa
ms.openlocfilehash: eff2352066a76010d6d882a7b7e1961870cd2d46
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="event-grid-subscription-schema"></a>Schematu subskrypcji siatki zdarzeń

Aby utworzyć subskrypcję zdarzeń siatki, Wyślij żądanie do operacji tworzenia zdarzenia subskrypcji. Użyj następującego formatu:

```
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

Na przykład można utworzyć subskrypcji zdarzeń dla konta magazynu o nazwie `examplestorage` w grupie zasobów o nazwie `examplegroup`, użyj następującego formatu:

```
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

Artykuł opisuje właściwości i schematu dla treści żądania.
 
## <a name="event-subscription-properties"></a>Właściwości subskrypcji zdarzeń

| Właściwość | Typ | Opis |
| -------- | ---- | ----------- |
| Miejsce docelowe | Obiekt | Obiekt, który definiuje punktu końcowego. |
| Filtr | Obiekt | Pole opcjonalne do filtrowania typów zdarzeń. |

### <a name="destination-object"></a>obiekt docelowy

| Właściwość | Typ | Opis |
| -------- | ---- | ----------- |
| endpointType | Ciąg | Typ punktu końcowego dla subskrypcji (webhook/HTTP Centrum zdarzeń i kolejki). | 
| adres URL punktu końcowego | Ciąg |  | 

### <a name="filter-object"></a>obiekt filtru

| Właściwość | Typ | Opis |
| -------- | ---- | ----------- |
| includedEventTypes | Tablica | Dopasowania, gdy typ zdarzenia w komunikacie zdarzenia jest dokładnym odpowiednikiem do jednej z następujących nazw typów zdarzeń. Zgłasza błąd, jeśli nazwa zdarzenia nie są zgodne z nazwy typu zdarzenia zarejestrowane dla źródła zdarzenia. Domyślne dopasowuje wszystkie typy zdarzeń. |
| subjectBeginsWith | Ciąg | Dopasowanie prefiksu filtrowanie zdarzeń do pola temat wiadomości. Domyślne lub pusty ciąg pasuje do wszystkich. | 
| subjectEndsWith | Ciąg | Sufiks match filtrowane w celu pole tematu zdarzeń komunikatów. Domyślne lub pusty ciąg pasuje do wszystkich. |
| subjectIsCaseSensitive | Ciąg | Formanty z uwzględnieniem wielkości liter dopasowanie filtrów. |


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
      "includedEventTypes": [ "blobCreated", "blobDeleted" ],
      "subjectBeginsWith": "blobServices/default/containers/mycontainer/log",
      "subjectEndsWith": ".jpg",
      "subjectIsCaseSensitive": "true"
    }
  }
}
```

## <a name="next-steps"></a>Następne kroki

* Aby obejrzeć wprowadzenie do siatki zdarzeń, zobacz [co to jest zdarzenie siatki?](overview.md)