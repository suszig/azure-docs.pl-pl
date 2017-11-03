---
title: "Automatyczne skalowanie w górę jednostek przepływności usługi Azure Event Hubs | Dokumentacja firmy Microsoft"
description: "Włącz zwiększyć automatycznie w przestrzeni nazw, aby automatycznie skalować jednostki przepływności"
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: 1cd31e0866ee6088483f88e8f80d01f75764c771
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Automatyczne skalowanie w górę jednostek przepływności usługi Azure Event Hubs

Usługa Azure Event Hubs to wysoce skalowalna danych przesyłania strumieniowego platformy. Tak klienci usługi Event Hubs często zwiększyć ich użycie po dołączania do usługi. Takie zwiększenie wymaga, zwiększenie jednostki przepływności ustalonej skalować centra zdarzeń i obsługi większych szybkości transferu. *Zwiększyć automatycznie* funkcji usługi Event hubs jest automatycznie skalowany liczby jednostek przepływności, aby spełnić potrzeby użycia. Zwiększenie jednostek przepływności uniemożliwia ograniczania scenariuszy, w którym:

* Szybkości transferu danych przekracza zestaw jednostek przepływności.
* Szybkości żądania wyjście danych przekracza zestaw jednostek przepływności.

## <a name="how-auto-inflate-works"></a>Jak zwiększyć automatycznie działa

Ruch centra zdarzeń jest kontrolowana przez jednostki przepływności. Pojedyncza jednostka przepływności umożliwia 1 MB na sekundę przychodzące i dwa razy ilość wyjście. Standardowa usługa Event Hubs można skonfigurować za pomocą 1-20 jednostek przepływności. Podniesienie automatycznie pozwala na rozpoczęcie od czegoś małego z jednostkami minimalna wymagana przepustowość. Funkcja następnie może obsłużyć automatycznie limit maksymalnej liczby jednostek przepływności, które są potrzebne, w zależności od wzrost ruchu. Podniesienie automatycznie zapewnia następujące korzyści:

- Wydajny mechanizm skalowania rozpoczęcie od czegoś małego i skalowanie w górę można powiększać.
- Automatycznie skalować górny limit określony bez ograniczania problemy.
- Większa kontrola nad skalowania, możesz kontrolować, kiedy i ile do skalowania.

## <a name="enable-auto-inflate-on-a-namespace"></a>Włącz zwiększyć automatycznie w przestrzeni nazw

Można włączyć lub wyłączyć zwiększyć automatycznie w przestrzeni nazw przy użyciu jednej z następujących metod:

1. [Portalu Azure](https://portal.azure.com).
2. Szablon usługi Azure Resource Manager.

### <a name="enable-auto-inflate-through-the-portal"></a>Włącz zwiększyć automatycznie za pośrednictwem portalu

Podczas tworzenia przestrzeni nazw usługi Event Hubs, mogą włączyć funkcję zwiększyć automatycznie w przestrzeni nazw:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

Po włączeniu tej opcji możesz uruchomić małych na jednostek przepływności i skalować w miarę wzrostu użycie. Górny limit inflacji nie wpływa na cennika, która jest zależna od liczby jednostek przepływności na godzinę.

Można również włączyć funkcję automatycznego zwiększyć za pomocą **skali** w bloku ustawień w portalu:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a>Włącz automatyczne-zwiększyć przy użyciu szablonu usługi Azure Resource Manager

Podniesienie automatycznej można włączyć podczas wdrażania szablonu usługi Azure Resource Manager. Na przykład ustawić `isAutoInflateEnabled` właściwości **true** i ustaw `maximumThroughputUnits` do 10.

```json
"resources": [
        {
            "apiVersion": "2017-04-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.EventHub/Namespaces",
            "location": "[variables('location')]",
            "sku": {
                "name": "Standard",
                "tier": "Standard"
            },
            "properties": {
                "isAutoInflateEnabled": true,
                "maximumThroughputUnits": 10
            },
            "resources": [
                {
                    "apiVersion": "2017-04-01",
                    "name": "[parameters('eventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
                    ],
                    "properties": {},
                    "resources": [
                        {
                            "apiVersion": "2017-04-01",
                            "name": "[parameters('consumerGroupName')]",
                            "type": "ConsumerGroups",
                            "dependsOn": [
                                "[parameters('eventHubName')]"
                            ],
                            "properties": {}
                        }
                    ]
                }
            ]
        }
    ]
```

Zakończenie szablonu, zobacz [utworzyć centra zdarzeń w przestrzeni nazw i Włącz zwiększyć](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) szablonu w witrynie GitHub.

## <a name="next-steps"></a>Następne kroki

Następujące linki pozwalają dowiedzieć się więcej na temat usługi Event Hubs:

* [Omówienie usługi Event Hubs](event-hubs-what-is-event-hubs.md)
* [Tworzenie centrum zdarzeń](event-hubs-create.md)
