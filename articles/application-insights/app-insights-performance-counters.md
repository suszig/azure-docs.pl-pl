---
title: "Liczniki wydajności w usłudze Application Insights | Dokumentacja firmy Microsoft"
description: "Monitorowanie systemu i niestandardowe liczniki wydajności .NET w usłudze Application Insights."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 5b816f4c-a77a-4674-ae36-802ee3a2f56d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/11/2016
ms.author: mbullwin
ms.openlocfilehash: 26837a72dd4539cd5b32e5b49a127a714f3a1426
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="system-performance-counters-in-application-insights"></a>Liczniki wydajności systemu w usłudze Application Insights
System Windows udostępnia wiele różnych [liczniki wydajności](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) takich jak zajęte Procesora, pamięci, dysku i użycia sieci. Możesz również definiować własnych. [Usługa Application Insights](app-insights-overview.md) można wyświetlić te liczniki wydajności, jeśli aplikacja działa w środowisku usług IIS na lokalnym hosta lub maszyny wirtualnej, do których masz dostęp administracyjny. Wykresy określają zasoby dostępne dla działającej aplikacji, a może pomóc w identyfikacji obciążenia równowagi między wystąpieniami serwera.

Liczniki wydajności są wyświetlane w bloku serwerów zawiera tabelę tego segmentów przez wystąpienie serwera.

![Liczniki wydajności zgłoszonych w usłudze Application Insights](./media/app-insights-performance-counters/counters-by-server-instance.png)

(Liczniki wydajności nie są dostępne dla aplikacji sieci Web Azure. Ale możesz [wysyłanie danych diagnostycznych platformy Azure do usługi Application Insights](app-insights-azure-diagnostics.md).)

## <a name="view-counters"></a>Wyświetlanie liczników
Blok serwerów Pokazuje domyślny zbiór liczników wydajności. 

Aby wyświetlić liczniki, edytowanie wykresów na blok serwerów albo otwórz nowe [Eksploratora metryk](app-insights-metrics-explorer.md) bloku i dodawanie nowych schematów. 

Dostępne liczniki są wyświetlane jako metryki podczas edytowania wykresu.

![Liczniki wydajności zgłoszonych w usłudze Application Insights](./media/app-insights-performance-counters/choose-performance-counters.png)

Aby wyświetlić wszystkie wykresy najbardziej przydatne w jednym miejscu, należy utworzyć [pulpitu nawigacyjnego](app-insights-dashboards.md) i przypiąć je do niego.

## <a name="add-counters"></a>Dodawanie liczników
Licznik wydajności, które mają nie jest widoczne na liście metryki, która jest ponieważ zestaw SDK usługi Application Insights nie jest zbieranie go na serwerze sieci web. Można skonfigurować go, aby to zrobić.

1. Sprawdzić, jakie liczniki są dostępne na serwerze za pomocą tego polecenia programu PowerShell na serwerze:
   
    `Get-Counter -ListSet *`
   
    (Zobacz [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)
2. Otwórz ApplicationInsights.config.
   
   * Jeśli dodano usługę Application Insights do aplikacji podczas tworzenia, edycji ApplicationInsights.config w projekcie, a następnie ponownie wdrożyć go na serwerach.
   * Jeśli Monitor stanu są używane do agregowania aplikacji sieci web w czasie wykonywania, można znaleźć pliku ApplicationInsights.config w katalogu głównym aplikacji w usługach IIS. Zaktualizuj ją w tym miejscu w każdym wystąpieniu serwera.
3. Edytuj dyrektywy wydajności modułu zbierającego:
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

Można przechwycić zarówno liczniki standard, jak i te zostały zaimplementowane samodzielnie. `\Objects\Processes`Przykładem standardowe licznika, dostępne we wszystkich systemach Windows. `\Sales(photo)\# Items Sold`jest to przykład liczników niestandardowych, które można zaimplementować w usłudze sieci web. 

Format jest `\Category(instance)\Counter"`, lub dla kategorii, które nie mają wystąpień, po prostu `\Category\Counter`.

`ReportAs`jest wymagany dla nazwy liczników, które nie są zgodne `[a-zA-Z()/-_ \.]+` -oznacza to, zawiera znaki, które nie znajdują się w następujących zestawach: litery round nawiasy, ukośnik, łącznik, podkreślenie, miejsca, kropka.

Jeśli określisz wystąpienia będą zbierane jako "CounterInstanceName" metryki zgłoszone wymiaru.

### <a name="collecting-performance-counters-in-code"></a>Zbierania liczników wydajności w kodzie
Aby zebrać liczników wydajności systemu i wysyłać je do usługi Application Insights, można dostosować poniższy fragment:


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

Lub możesz zrobić to samo z metryki niestandardowe utworzone:

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

## <a name="performance-counters-in-analytics"></a>Liczniki wydajności w module analiz
Można wyszukiwać i wyświetlać raporty licznika wydajności w [Analytics](app-insights-analytics.md).

**Liczniki wydajności** schemat przedstawia `category`, `counter` nazwa, i `instance` nazwę każdego licznika wydajności.  W danych telemetrycznych dla każdej aplikacji zostanie wyświetlony tylko liczniki dla tej aplikacji. Na przykład aby zobaczyć, jakie liczniki są dostępne: 

![Liczniki wydajności w module analiz usługi Application Insights](./media/app-insights-performance-counters/analytics-performance-counters.png)

(W tym miejscu "Instance" odwołuje się do wystąpienia licznika wydajności nie wystąpienie maszyny roli lub serwera. Nazwa wystąpienia liczników wydajności zazwyczaj segmenty liczników, takie jak czas procesora według nazwy procesu lub aplikacji.)

Aby wyświetlić wykres dostępnej pamięci w ostatnim okresie: 

![Timechart pamięci w module analiz usługi Application Insights](./media/app-insights-performance-counters/analytics-available-memory.png)

Inne telemetrii, takich jak **liczniki wydajności** również zawiera kolumnę `cloud_RoleInstance` wskazujące tożsamości wystąpienia serwera hosta, na którym jest uruchomiona aplikacja. Na przykład, aby porównać wydajności aplikacji na różnych komputerach: 

![Wydajność segmentowanych przez wystąpienie roli w usłudze Application Insights analityka](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a>ASP.NET i liczby usługi Application Insights
*Jaka jest różnica między szybkość wyjątków i wyjątków metryk?*

* *Szybkość wyjątek* jest licznika wydajności systemu. Środowisko CLR zlicza wszystkich obsłużonych i nieobsłużonych wyjątków, które są generowane i podzielenie sumy w interwale próbkowania przez długość interwału. Zestaw SDK usługi Application Insights zbiera wynik i wysyła go do portalu.
* *Wyjątki* to liczba raportów TrackException odebranych przez portal w interwale próbkowania wykresu. Obejmuje on tylko wyjątki obsłużone których napisano TrackException wywołuje w kodzie i nie zawiera wszystkich [nieobsługiwane wyjątki](app-insights-asp-net-exceptions.md). 

## <a name="alerts"></a>Alerty
Podobnie jak inne metryki, możesz [ustawić alert](app-insights-alerts.md) celu ostrzeżenia, jeśli licznika wydajności wykracza poza możesz określić limit. Otwiera blok alerty, a następnie kliknij przycisk Dodaj alertu.

## <a name="next"></a>Następne kroki
* [Śledzenia zależności](app-insights-asp-net-dependencies.md)
* [Śledzenie wyjątków](app-insights-asp-net-exceptions.md)

