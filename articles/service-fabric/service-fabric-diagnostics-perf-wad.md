---
title: "Azure Service Fabric — wydajności monitorowania za pomocą rozszerzenia Windows Azure Diagnostics | Dokumentacja firmy Microsoft"
description: "Użyj diagnostyki Windows Azure można zebrać liczników wydajności dla klastrów sieci szkieletowej usług Azure."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/05/2017
ms.author: dekapur
ms.openlocfilehash: f71c483eba201eae91c15b84a2ed42fcb68ae575
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="performance-monitoring-with-windows-azure-diagnostics-extension"></a>Monitorowanie wydajności z rozszerzeniem diagnostyki Windows Azure

W tym dokumencie opisano kroki wymagane do skonfigurowania kolekcji liczników wydajności za pośrednictwem rozszerzenia systemu Windows Azure Diagnostics (WAD) w przypadku klastrów systemu Windows. W przypadku klastrów systemu Linux, skonfiguruj [Agent pakietu OMS](service-fabric-diagnostics-oms-agent.md) można zebrać liczników wydajności dla węzłów. 

 > [!NOTE]
> Rozszerzenie WAD powinny zostać wdrożone w klastrze dla tej procedury do działania. Jeśli go nie ustawiono, przejdź do [agregacji zdarzeń i kolekcji przy użyciu systemu Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) i postępuj zgodnie z instrukcjami *wdrażanie rozszerzenia diagnostyki*.

## <a name="collect-performance-counters-via-the-wadcfg"></a>Zbieraj liczniki wydajności za pośrednictwem WadCfg

Można zebrać liczników wydajności za pośrednictwem WAD, należy zmodyfikować konfigurację odpowiednio w szablonie usługi Resource Manager z klastra. Wykonaj następujące kroki, aby dodać licznika wydajności, które chcesz zebrać do szablonu, a następnie uruchom uaktualnianie zasobów Menedżera zasobów.

1. Znajdź konfiguracji WAD w szablonie klastra — Znajdź `WadCfg`. Dodasz liczniki wydajności do w obszarze `DiagnosticMonitorConfiguration`.

2. Konfigurowanie konfiguracji można zebrać liczników wydajności, dodając w poniższej sekcji, aby Twoje `DiagnosticMonitorConfiguration`. 

    ```json
    "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": []
    }
    ```

    `scheduledTransferPeriod` Definiuje konfiguracji frquently wartości liczników, które są zbierane są przekazywane do tabeli magazynu systemu Azure i dla każdego obiektu sink. 

3. Dodaj liczniki wydajności, które chcesz zebrać na potrzeby `PerformanceCounterConfiguration` który został zadeklarowany w poprzednim kroku. Każdego licznika chcesz zbierać jest zdefiniowana z `counterSpecifier`, `sampleRate`, `unit`, `annotation`, a wszelkie odpowiednie `sinks`. Oto przykład licznika wydajności dla *łączny czas procesora* (ilość czasu procesora CPU był używany w operacjach przetwarzania):

    ```json
    "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": [
            {
                "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                "sampleRate": "PT1M",
                "unit": "Percent",
                "annotation": [
                ],
                "sinks": ""
            }
        ]
    }
    ```

    Częstotliwość próbkowania licznika można zmodyfikować zgodnie z potrzebami. Format dla niego `PT<time><unit>`, tak aby licznik zbierane w ciągu sekundy, następnie należy ustawić `"sampleRate": "PT15S"`.

    Podobnie można zebrać kilka innych liczników wydajności, dodając je do listy w `PerformanceCounterConfiguration`. Chociaż można użyć `*` do określania grup liczniki wydajności, które są nazywane podobnie, wysyłanie liczników za pośrednictwem obiektu sink (do usługi Application Insights) wymaga indywidualnie była zadeklarowana. 

4. Po było dodać liczniki wydajności odpowiednie, które mają być zbierane, musisz uaktualnić zasobu klastra, aby te zmiany zostaną odzwierciedlone w uruchomionej klastra. Zapisz z zmodyfikowanych `template.json` i otwórz programu PowerShell. Można uaktualnić za pomocą klastra `New-AzureRmResourceGroupDeployment`. Wywołanie wymaga nazwy grupy zasobów, plik zaktualizowany szablon i plik parametrów i wyświetla monit o Resource Manager, aby wprowadzić zmiany odpowiednie zasoby, które można zaktualizować. Po zalogowano do konta i w prawo subskrypcji, użyj następującego polecenia w celu przeprowadzenia uaktualnienia:

    ```sh
    New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroup> -TemplateFile <PathToTemplateFile> -TemplateParameterFile <PathToParametersFile> -Verbose
    ```

5. Po zakończeniu uaktualniania zetknie (trwa od 15 do 45 minut), WAD powinny być zbierania liczników wydajności i wysyłania ich do tabeli na koncie magazynu zadeklarowany w `WadCfg`.

## <a name="next-steps"></a>Następne kroki
* Zobacz sink liczników wydajności w usłudze Application Insights przez dodanie usługi Application Insights do Twojej `WadCfg`. Odczytu *Dodaj obiekt Sink AI do szablonu usługi Resource Manager* sekcji [analiza zdarzeń i wizualizacji z usługą Application Insights ](service-fabric-diagnostics-event-analysis-appinsights.md) skonfigurować zbiornika usługi Application Insights.
* Zbieranie większej liczby liczników wydajności dla klastra. Zobacz [metryki wydajności](service-fabric-diagnostics-event-generation-perf.md) listę liczników, które należy zebrać.
* [Użyj monitorowania i diagnostyki z szablonów maszyny Wirtualnej systemu Windows i usługi Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md) na dalsze zmiany w Twojej `WadCfg`, łącznie z konfigurowaniem dodatkowych kont magazynu do wysyłania danych diagnostycznych.