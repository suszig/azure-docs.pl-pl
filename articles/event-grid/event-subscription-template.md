---
title: "Usługi Azure event siatki subskrypcji z szablonu"
description: "Utwórz subskrypcję siatki zdarzeń za pomocą szablonu usługi Resource Manager."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/05/2018
ms.author: tomfitz
ms.openlocfilehash: 2b9f55f8e944d688b622e30a773e1a34698f22ec
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/05/2018
---
# <a name="use-resource-manager-template-for-event-grid-subscription"></a>Użyj Menedżera zasobów szablon dla subskrypcji zdarzeń siatki

W tym artykule pokazano, jak szablon Menedżera zasobów Azure umożliwia tworzenie subskrypcji zdarzeń siatki. Format, którego używasz różni się w zależności od tego, czy są subskrybowanie zdarzeń grupy zasobów lub zdarzenia dla określonego typu zasobu. W tym artykule przedstawiono obu formatów.

## <a name="subscribe-to-resource-group-events"></a>Subskrybowanie zdarzeń grupy zasobów

Gdy subskrybowanie zdarzeń grupy zasobów, użyj `Microsoft.EventGrid/eventSubscriptions` dla typu zasobu. Dla zdarzenia typu punktu, należy użyć jednego `WebHook` lub `EventHub`.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "type": "Microsoft.EventGrid/eventSubscriptions",
            "name": "mySubscription",
            "apiVersion": "2017-09-15-preview",
            "properties": {
                "destination": {
                    "endpointType": "WebHook",
                    "properties": {
                        "endpointUrl": "https://requestb.in/ynboxiyn"
                    }
                },
                "filter": {
                    "subjectBeginsWith": "",
                    "subjectEndsWith": "",
                    "isSubjectCaseSensitive": false,
                    "includedEventTypes": ["All"]
                }
            }
        }
    ]
}
```

Podczas wdrażania tego szablonu do grupy zasobów, subskrypcji zdarzeń dla danej grupy zasobów.

## <a name="subscribe-to-resource-events"></a>Subskrybowanie zdarzeń zasobów

Subskrybowanie zdarzeń zasobu, należy skojarzyć subskrypcję poprawne zasobów przez uwzględnienie typ zasobu i nazwę w definicji subskrypcji. Dla typu zasobu, należy użyć `<provider-namespace>/<resource-type>/providers/eventSubscriptions`. W nazwie, użyj `<resource-name>/Microsoft.EventGrid/<subscription-name>`. Dla zdarzenia typu punktu, należy użyć jednego `WebHook` lub `EventHub`.

Poniższy przykład przedstawia sposób subskrybowania zdarzenia magazynu obiektów Blob.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageName": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts/providers/eventSubscriptions",
            "name": "[concat(parameters('storageName'), '/Microsoft.EventGrid/myStorageSubscription')]",
            "apiVersion": "2017-09-15-preview",
            "properties": {
                "destination": {
                    "endpointType": "WebHook",
                    "properties": {
                        "endpointUrl": "https://requestb.in/ynboxiyn"
                    }
                },
                "filter": {
                    "subjectBeginsWith": "",
                    "subjectEndsWith": "",
                    "isSubjectCaseSensitive": false
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Kolejne kroki

* Aby obejrzeć wprowadzenie do siatki zdarzeń, zobacz [o siatki zdarzeń](overview.md).
* Aby obejrzeć wprowadzenie do Menedżera zasobów, zobacz [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
* Aby rozpocząć pracę z siatki zdarzeń, zobacz [tworzenie i tras niestandardowych zdarzeń siatki zdarzeń Azure](custom-event-quickstart.md).