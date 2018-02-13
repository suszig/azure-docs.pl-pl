---
title: "Pobliżu metryki alertów w czasie rzeczywistym w monitorze Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak korzystać z niemal w czasie rzeczywistym metryki alerty do monitorowania metryki zasobów platformy Azure z częstotliwością jak 1 minutę."
author: snehithm
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: snmuvva
ms.custom: 
ms.openlocfilehash: 6370f4596e6b20962c6de0dbcbd5f86c3b7777cc
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/13/2018
---
# <a name="near-real-time-metric-alerts-preview"></a>Niemal w czasie rzeczywistym alerty metryki (wersja zapoznawcza)
Azure Monitor obsługuje nowy typ alertu o nazwie niemal w czasie rzeczywistym alerty metryki (wersja zapoznawcza). Ta funkcja jest obecnie w wersji zapoznawczej.

Niemal w czasie rzeczywistym Metryka alerty różnią się od regularne alerty metryki na kilka sposobów:

- **Ulepszone opóźnienia**: niemal w czasie rzeczywistym Metryka alertów można monitorować zmiany w wartości metryki z częstotliwością jak 1 minutę.
- **Większa kontrola nad metryki warunków**: można zdefiniować bardziej zaawansowane funkcje reguły alertów w pobliżu metryki alertów w czasie rzeczywistym. Alerty obsługuje monitorowanie maksymalnej, minimalnej, średni i całkowitej wartości metryki.
- **Połączone monitorowanie wielu metryki**: niemal w czasie rzeczywistym Metryka alertów można monitorować wiele metryk (obecnie do dwóch metryk) przy użyciu jednej reguły. Alert zostanie wywołany, jeśli oba wskaźniki naruszenia ich odpowiednich progów w określonym przedziale czasu.
- **System powiadomień moduły**: niemal w czasie rzeczywistym metryki użycia alerty [grupy akcji](monitoring-action-groups.md). Grupy akcji służy do tworzenia moduły akcji. Można ponownie użyć grup akcji dla wielu reguł alertów.

> [!NOTE]
> Near w czasie rzeczywistym metryki funkcja alertu jest obecnie w wersji zapoznawczej. Funkcje i środowisko użytkownika może ulec zmianie.
>

## <a name="resources-you-can-use-with-near-real-time-metric-alerts"></a>Zasoby, których można używać z niemal w czasie rzeczywistym metryki alertów
Poniżej przedstawiono pełną listę typów zasobów, które są obsługiwane w pobliżu metryki alertów w czasie rzeczywistym:

* Microsoft.ApiManagement/service
* Microsoft.Automation/automationAccounts
* Microsoft.Batch/batchAccounts
* Microsoft.Cache/Redis
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.DataFactory/factories
* Microsoft.DBforMySQL/servers
* Microsoft.DBforPostgreSQL/servers
* Microsoft.EventHub/namespaces
* Microsoft.Logic/workflows
* Microsoft.Network/applicationGateways
* Microsoft.Network/publicipaddresses
* Microsoft.Search/searchServices
* Microsoft.ServiceBus/namespaces
* Microsoft.Storage/storageAccounts
* Microsoft.Storage/storageAccounts/services
* Microsoft.StreamAnalytics/streamingjobs
* Microsoft.CognitiveServices/accounts

## <a name="near-real-time-metric-alerts-for-metrics-that-use-dimensions"></a>Niemal w czasie rzeczywistym metryki alerty dotyczące metryk, które używają wymiarów
Niemal w czasie rzeczywistym alerty metryki obsługuje alerty dla metryki, które są używane wymiary. Wymiary służy do filtrowania Twoje metryki na odpowiedni poziom. Niemal w czasie rzeczywistym Metryka alerty dotyczące metryk, które są używane wymiary są obsługiwane dla następujących zasobów:

* Microsoft.ApiManagement/service
* Magazyn.Microsoft/kontamagazynu (obsługiwane tylko dla kont magazynu w regionach US)
* Microsoft.Storage/storageAccounts/services (obsługiwane tylko dla kont magazynu w regionach US)

## <a name="create-a-near-real-time-metric-alert"></a>Utwórz alert near metryki w czasie rzeczywistym
Obecnie można tworzyć niemal w czasie rzeczywistym metryki alerty tylko w portalu Azure. Obsługa konfigurowania niemal w czasie rzeczywistym alerty metryki przy użyciu programu PowerShell, interfejsu wiersza polecenia platformy Azure (Azure CLI) i interfejsów API REST Monitor Azure będzie dostępna wkrótce.

Proces tworzenia near alert metryki w czasie rzeczywistym została przeniesiona do nowego **alertów (wersja zapoznawcza)** strony. Nawet wtedy, gdy alerty bieżącej strony wyświetla **alert dodać niemal w czasie rzeczywistym Metryka**, nastąpi przekierowanie do **alertów (wersja zapoznawcza)** strony.

Aby dowiedzieć się, jak utworzyć near alertu metryki w czasie rzeczywistym, zobacz [Tworzenie reguły alertu w portalu Azure](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="manage-near-real-time-metric-alerts"></a>Zarządzanie niemal w czasie rzeczywistym metryki alertów
Po utworzeniu near w czasie rzeczywistym alert metryki alertu można zarządzać przy użyciu procedury opisanej w [Zarządzanie alertami w portalu Azure](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).

## <a name="payload-schema"></a>Schemat ładunku

Operację POST zawiera następujące ładunek JSON i schematu dla wszystkich niemal w czasie rzeczywistym alerty metryki:

```json
{
    "WebhookName": "Alert1510875839452",
    "RequestBody": {
        "status": "Activated",
        "context": {
            "condition": {
                "metricName": "Percentage CPU",
                "metricUnit": "Percent",
                "metricValue": "17.7654545454545",
                "threshold": "1",
                "windowSize": "10",
                "timeAggregation": "Average",
                "operator": "GreaterThan"
            },
            "resourceName": "ContosoVM1",
            "resourceType": "microsoft.compute/virtualmachines",
            "resourceRegion": "westus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/automationtest/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
            "timestamp": "2017-11-16T23:54:03.9517451Z",
            "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/microsoft.insights/alertrules/VMMetricAlert1",
            "name": "VMMetricAlert1",
            "description": "A metric alert for the VM Win2012R2",
            "conditionType": "Metric",
            "subscriptionId": "00000000-0000-0000-0000-000000000000",
            "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
            "resourceGroupName": "ContosoVM"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    },
    "RequestHeader": {
        "Connection": "Keep-Alive",
        "Host": "s1events.azure-automation.net",
        "User-Agent": "azure-insights/0.9",
        "x-ms-request-id": "00000000-0000-0000-0000-000000000000"
    }
}
```

## <a name="next-steps"></a>Kolejne kroki

* Dowiedz się więcej o nowe [alerty środowisko (wersja zapoznawcza)](monitoring-overview-unified-alerts.md).
* Dowiedz się więcej o [rejestrowania alertów w alertach Azure (wersja zapoznawcza)](monitor-alerts-unified-log.md).
* Dowiedz się więcej o [alertów w usłudze Azure](monitoring-overview-alerts.md).