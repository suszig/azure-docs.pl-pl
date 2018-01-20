---
title: "Metryki alertów w czasie rzeczywistym w Monitorze systemu Azure w pobliżu | Dokumentacja firmy Microsoft"
description: "Niemal w czasie rzeczywistym Metryka alerty pozwalają monitorować nawet 1 min metryki zasobów platformy Azure."
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
ms.openlocfilehash: d3e88a98e0ba93a630d131c25ca4dd5cb16f1b1a
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="near-real-time-metric-alerts-preview"></a>Niemal w czasie rzeczywistym alerty metryki (wersja zapoznawcza)
Azure Monitor obsługuje obecnie nowy typ metryki alertów o nazwie niemal w czasie rzeczywistym Metryka alerty (wersja zapoznawcza). Ta funkcja jest obecnie w wersji zapoznawczej.
Te alerty różnią się od regularne alerty metryki na kilka sposobów

- **Ulepszone opóźnienia** -niemal w czasie rzeczywistym Metryka alertów można monitorować zmiany w wartości metryki tak szybko, jak 1 min.
- **Większa kontrola nad metryki warunki** -niemal w czasie rzeczywistym Metryka alerty Zezwalaj użytkownikom na definiowanie bardziej rozbudowane reguły alertów. Alerty są teraz obsługiwane monitorowanie maksymalnej, minimalnej, średni i całkowitej wartości metryki.
- **Połączone monitorowanie wielu metryki** -niemal w czasie rzeczywistym Metryka alertów można monitorować metryki wielu (obecnie dwa) przy użyciu jednej reguły. Alert pobiera uruchomiony, jeśli obie metryki naruszyć ich odpowiednich progów w określonym przedziale czasu.
- **System powiadomień moduły** — niemal w czasie rzeczywistym alerty metryki użycia [grupy akcji](monitoring-action-groups.md). Ta funkcja umożliwia użytkownikom tworzenie akcji w sposób moduły i użyj ich ponownie dla wielu reguł alertów.

> [!NOTE]
> Niemal w czasie rzeczywistym alerty metryki funkcja jest obecnie w wersji zapoznawczej. Funkcje i środowisko użytkownika może ulec zmianie.
>

## <a name="what-resources-can-i-create-near-real-time-metric-alerts-for"></a>Jakie zasoby można tworzyć niemal w czasie rzeczywistym alerty metryki dla?
Pełną listę typów zasobów, które są obsługiwane przez niemal w czasie rzeczywistym alerty metryki:

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

## <a name="near-real-time-metric-alerts-on-metrics-with-dimensions"></a>NEAR w czasie rzeczywistym Metryka alerty dotyczące metryk o wymiarach
Niemal w czasie rzeczywistym Metryka alerty obsługuje alerty na metryki z wymiarów. Wymiary są odfiltrować Twoje metryki na odpowiedni poziom. Niemal w czasie rzeczywistym Metryka alerty dotyczące metryk z wymiarów są obsługiwane dla następujących typów zasobów

* Microsoft.ApiManagement/service
* Magazyn.Microsoft/kontamagazynu (obsługiwane tylko w przypadku kont magazynu w regionach US)
* Microsoft.Storage/storageAccounts/services (obsługiwane tylko w przypadku kont magazynu w regionach US)


## <a name="create-a-near-real-time-metric-alert"></a>Utwórz Alert Near metryki w czasie rzeczywistym
Obecnie niemal w czasie rzeczywistym Metryka alertów można tworzyć tylko za pośrednictwem portalu Azure. Obsługa konfigurowania niemal metryki alertów w czasie rzeczywistym za pomocą programu PowerShell, interfejsu wiersza polecenia (CLI) i interfejsu API REST Monitor Azure będzie dostępna wkrótce.

Środowisko tworzenia alertu dla niemal w czasie rzeczywistym Metryka Alert został przeniesiony do nowej **Alerts(Preview)** wystąpić. Mimo że alerty bieżącej strony pokazuje **alert dodać niemal w czasie rzeczywistym Metryka**, nastąpi przekierowanie do nowego środowiska.

Możesz utworzyć near w czasie rzeczywistym alert metryki korzystając z procedury opisanej [tutaj](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="managing-near-real-time-metric-alerts"></a>Zarządzanie niemal w czasie rzeczywistym metryki alertów
Po utworzeniu **alertu w pobliżu metryki w czasie rzeczywistym**, można zarządzać, korzystając z procedury opisanej [tutaj](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).

## <a name="payload-schema"></a>Schemat ładunku

Operację POST zawiera następujące ładunek JSON i schematu dla wszystkich niemal w czasie rzeczywistym alerty metryki.

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

* [Dowiedz się więcej na temat nowego środowiska alerty (wersja zapoznawcza)](monitoring-overview-unified-alerts.md)
* [Dowiedz się więcej o alertach dziennika w alertach Azure (wersja zapoznawcza)](monitor-alerts-unified-log.md)
* [Więcej informacji na temat alertów na platformie Azure](monitoring-overview-alerts.md)