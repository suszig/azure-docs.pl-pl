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
ms.date: 02/26/2018
ms.author: snmuvva, vinagara
ms.custom: 
ms.openlocfilehash: 88995b1f3350fe485e28efccc93779ae0a42eb97
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="near-real-time-metric-alerts-preview"></a>Niemal w czasie rzeczywistym alerty metryki (wersja zapoznawcza)
Azure Monitor obsługuje nowy typ alertu o nazwie niemal w czasie rzeczywistym alerty metryki (wersja zapoznawcza). Ta funkcja jest obecnie w wersji zapoznawczej.

Niemal w czasie rzeczywistym Metryka alerty różnią się od regularne alerty metryki na kilka sposobów:

- **Ulepszone opóźnienia**: niemal w czasie rzeczywistym Metryka alertów można monitorować zmiany w wartości metryki z częstotliwością około jednej minuty.
- **Większa kontrola nad metryki warunków**: można zdefiniować bardziej zaawansowane funkcje reguły alertów w pobliżu metryki alertów w czasie rzeczywistym. Alerty obsługuje monitorowanie maksymalnej, minimalnej, średni i całkowitej wartości metryki.
- **Metryki z dzienników**: Z danych dziennika popularnych wchodzących w [analizy dzienników](../log-analytics/log-analytics-overview.md), metryki można wyodrębnić do monitora Azure i otrzymywać alerty w niemal w czasie rzeczywistym
- **Połączone monitorowanie wielu metryki**: niemal w czasie rzeczywistym Metryka alertów można monitorować wiele metryk (obecnie do dwóch metryk) przy użyciu jednej reguły. Alert zostanie wywołany, jeśli oba wskaźniki naruszenia ich odpowiednich progów w określonym przedziale czasu.
- **System powiadomień moduły**: niemal w czasie rzeczywistym metryki użycia alerty [grupy akcji](monitoring-action-groups.md). Grupy akcji służy do tworzenia moduły akcji. Można ponownie użyć grup akcji dla wielu reguł alertów.

> [!NOTE]
> Near alert metryki w czasie rzeczywistym jest obecnie znajduje się w publicznej wersji zapoznawczej. I metryki z funkcji Dzienniki znajdują się w *ograniczone* publicznej wersji zapoznawczej. Funkcje i środowisko użytkownika może ulec zmianie.
>

## <a name="metrics-and-dimensions-supported"></a>Metryki i wymiary obsługiwane
Niemal w czasie rzeczywistym alerty metryki obsługuje alerty dla metryki, które są używane wymiary. Wymiary służy do filtrowania Twoje metryki na odpowiedni poziom. Wszystkie metryki obsługiwanych wraz z odpowiednich wymiary mogą być zbadane i wizualizowane z [Monitor Azure — Eksploratora metryk (wersja zapoznawcza)](monitoring-metric-charts.md).

Poniżej przedstawiono pełną listę Azure monitora na podstawie źródeł metryki, które są obsługiwane w pobliżu metryki alertów w czasie rzeczywistym:

|Metryki szczegóły  |Wymiary obsługiwane  |
|---------|---------|
|Microsoft.ApiManagement/service     | Yes        |
|Microsoft.Automation/automationAccounts     |     ND    |
|Microsoft.Automation/automationAccounts     |   ND      |
|Microsoft.Cache/Redis     |    ND     |
|Microsoft.Compute/virtualMachines     |    ND     |
|Microsoft.Compute/virtualMachineScaleSets     |   ND      |
|Microsoft.DataFactory/factories     |   ND      |
|Microsoft.DBforMySQL/servers     |   ND      |
|Microsoft.DBforPostgreSQL/servers     |    ND     |
|Microsoft.EventHub/namespaces     |   ND      |
|Microsoft.Logic/workflows     |     ND    |
|Microsoft.Network/applicationGateways     |    ND     |
|Microsoft.Network/publicipaddresses     |  ND       |
|Microsoft.Search/searchServices     |   ND      |
|Microsoft.ServiceBus/namespaces     |  ND       |
|Microsoft.Storage/storageAccounts     |    Yes     |
|Microsoft.Storage/storageAccounts/services     |     Yes    |
|Microsoft.StreamAnalytics/streamingjobs     |  ND       |
|Microsoft.CognitiveServices/accounts     |    ND     |


Metryki z dzienników, obsługuje obecnie następujące popularnych dzienniki pakietu OMS:
- [Liczniki wydajności](../log-analytics/log-analytics-data-sources-performance-counters.md) dla komputerów z systemem Windows i Linux
- Rekordy pulsu dla komputerów
- [Zarządzanie aktualizacjami](../operations-management-suite/oms-solution-update-management.md) rekordów

Poniżej przedstawiono pełną listę OMS opartych na dzienniku metryki źródeł, które są obsługiwane w pobliżu metryki alertów w czasie rzeczywistym:

Metryki szczegóły  |Wymiary obsługiwane  | Typ dziennika  |
|---------|---------|---------|
|Average_Avg. Czas dysku w s/Odczyt     |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Windows      |
| Average_Avg. Dysku w s/Zapis     |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Windows      |
| Długość kolejki dysku Average_Current   |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Windows      |
| Average_Disk/s    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Windows      |
| Transfery Average_Disk na sekundę    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Windows      |
|   Average_ % wolnego miejsca    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Windows      |
| Average_Available (MB)     |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Windows      |
| Average_ % Zadeklarowane bajty w użyciu    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Windows      |
| Average_Bytes odebrane/s    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Windows      |
|  Average_Bytes wysłane/s    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Windows      |
|  Całkowita liczba Average_Bytes/s    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Windows      |
|  Average_ czas procesora (%)    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Windows      |
|   Długość kolejki Average_Processor    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Windows      |
|   Average_ % wolnych węzłów i   |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_ % wolnego miejsca   |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_ % używanych węzłów i  |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Używany obszar Average_ %   |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Disk odczytu bajtów na sekundę    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Disk/s |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Transfery Average_Disk na sekundę |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Bajty zapisu Average_Disk/s   |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Disk/s    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Free megabajtów |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Logical Bajty dysku/s |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_ % dostępnej pamięci |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_ % dostępnego obszaru wymiany |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_ procent wykorzystania pamięci  |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Używany obszar wymiany (MB) Average_ %  |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Pamięć (MB) Average_Available    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Available wymiany (MB)  |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Page/s |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Page/s    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Pages/sec  |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Used obszar wymiany (MB) |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Used pamięć (MB) |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Total bajtów przesłanych    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Odebrane bajty Average_Total   |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Total bajtów    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Pakiety Average_Total  |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Odebrane pakiety Average_Total |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Błędy odbioru Average_Total    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Błędy wysyłania Average_Total    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Total kolizji   |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Avg. Czas dysku w s/Odczyt |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Avg. Dysku w s/Transfer |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Avg. Dysku w s/Zapis    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Physical Bajty dysku/s    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Czas uprzywilejowany Average_Pct    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Czas użytkownika Average_Pct  |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    KB pamięci Average_Used |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Virtual współużytkowana pamięć  |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_ czas DPC (%) |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Czas bezczynności (%) Average_    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_ czas przerwań (%)   |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Czas oczekiwania Average_ % we/wy |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Czas nieuprzywilejowany % Average_    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Czas uprzywilejowany Average_ %  |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_ czas procesora (%)   |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Czas użytkownika % Average_    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Pamięć fizyczna Average_Free   |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Free miejsce w plikach stronicowania |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Free pamięci wirtualnej    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Processes  |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Size przechowywane w plikach stronicowania    |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Uptime |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Average_Users  |     Tak — komputera, nazwa obiektu, InstanceName, Ścieżka_licznika i SourceSystem    |   Licznik wydajności systemu Linux      |
|    Pulsu  |     Tak — komputer, OSType, wersji i SourceComputerId    |   Rekordy pulsu |
|    Aktualizacja |     Tak — komputer, produktów, klasyfikacji, UpdateState, opcjonalne & zatwierdzone    |   Zarządzanie aktualizacjami |

> [!NOTE]
> Określonej metryki i/lub wymiaru będą wyświetlane tylko jeśli danych dla niego istnieje w wybranym okresie.

## <a name="create-a-near-real-time-metric-alert"></a>Utwórz alert near metryki w czasie rzeczywistym
Obecnie można tworzyć niemal w czasie rzeczywistym metryki alerty tylko w portalu Azure. Obsługa konfigurowania niemal w czasie rzeczywistym alerty metryki przy użyciu programu PowerShell, interfejsu wiersza polecenia platformy Azure (Azure CLI) i interfejsów API REST Monitor Azure będzie dostępna wkrótce.

Proces tworzenia near alert metryki w czasie rzeczywistym została przeniesiona do nowego **alertów (wersja zapoznawcza)** strony. Nawet wtedy, gdy alerty bieżącej strony wyświetla **alert dodać niemal w czasie rzeczywistym Metryka**, nastąpi przekierowanie do **alertów (wersja zapoznawcza)** strony.

Aby dowiedzieć się, jak utworzyć near alertu metryki w czasie rzeczywistym, zobacz [Tworzenie reguły alertu w portalu Azure](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="manage-near-real-time-metric-alerts"></a>Zarządzanie niemal w czasie rzeczywistym metryki alertów
Po utworzeniu near w czasie rzeczywistym alert metryki alertu można zarządzać przy użyciu procedury opisanej w [Zarządzanie alertami w portalu Azure](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).

## <a name="payload-schema"></a>Schemat ładunku

Operację POST zawiera następujące ładunek JSON i schematu dla wszystkich alertów w czasie rzeczywistym metryki odpowiednio skonfigurowane w pobliżu [grupy akcji](monitoring-action-groups.md) jest używany:

```json
{"schemaId":"AzureMonitorMetricAlert","data":
    {
    "version": "2.0",
    "status": "Activated",
    "context": {
    "timestamp": "2018-02-28T10:44:10.1714014Z",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
    "name": "StorageCheck",
    "description": "",
    "conditionType": "SingleResourceMultipleMetricCriteria",
    "condition": {
      "windowSize": "PT5M",
      "allOf": [
        {
          "metricName": "Transactions",
          "dimensions": [
            {
              "name": "AccountResourceId",
              "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
            },
            {
              "name": "GeoType",
              "value": "Primary"
            }
          ],
          "operator": "GreaterThan",
          "threshold": "0",
          "timeAggregation": "PT5M",
          "metricValue": 1.0
        },
      ]
    },
    "subscriptionId": "00000000-0000-0000-0000-000000000000",
    "resourceGroupName": "Contoso",
    "resourceName": "diag500",
    "resourceType": "Microsoft.Storage/storageAccounts",
    "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
    "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
  },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    }
}
```

## <a name="next-steps"></a>Kolejne kroki

* Dowiedz się więcej o nowe [alerty środowisko (wersja zapoznawcza)](monitoring-overview-unified-alerts.md).
* Dowiedz się więcej o [rejestrowania alertów w alertach Azure (wersja zapoznawcza)](monitor-alerts-unified-log.md).
* Dowiedz się więcej o [alertów w usłudze Azure](monitoring-overview-alerts.md).
