---
title: "Model danych Telemetrii Insights aplikacji Azure — dane telemetryczne metryki | Dokumentacja firmy Microsoft"
description: "Model danych usługi Insights aplikacji dla dane telemetryczne metryki"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: bwren
ms.openlocfilehash: 42e55db4f932de85ee1a71c408b889e0ff9fe3e1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="metric-telemetry-application-insights-data-model"></a>Dane telemetryczne metryki: model danych usługi Application Insights

Istnieją dwa typy dane telemetryczne metryki obsługiwane przez [usługi Application Insights](app-insights-overview.md): pojedyncze pomiaru i wstępnie zagregowane metryki. Pojedynczego pomiaru jest po prostu nazw i wartości. Metryka wstępnie zagregowane określa minimalną i maksymalną wartość metryki interwał agregacji i odchylenie standardowe go.

Wstępnie zagregowane dane telemetryczne metryki zakłada tego okresu agregacji został jednej minuty.

Istnieje kilka dobrze znane nazwy metryki obsługiwane przez usługę Application Insights. 

Metryka reprezentujący liczniki systemu i procesu:

| **Nazwa platformy .NET**             | **Nazwa o niesprecyzowanym platformy** | **Nazwa interfejsu API REST** | **Opis**
| ------------------------- | -------------------------- | ----------------- | ---------------- 
| `\Processor(_Total)\% Processor Time` | Praca w toku... | [processorCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessorCpuPercentage) | Łączna liczba procesorów maszyny
| `\Memory\Available Bytes`                 | Praca w toku... | [memoryAvailableBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FmemoryAvailableBytes) | ilość pamięci dostępnej na dysku
| `\Process(??APP_WIN32_PROC??)\% Processor Time` | Praca w toku... | [processCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessCpuPercentage) | Procesor CPU procesu hostingu aplikacji
| `\Process(??APP_WIN32_PROC??)\Private Bytes`      | Praca w toku... | [processPrivateBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessPrivateBytes) | pamięci używanej przez proces obsługujący aplikacji
| `\Process(??APP_WIN32_PROC??)\IO Data Bytes/sec` | Praca w toku... | [processIOBytesPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessIOBytesPerSecond) | Liczba operacji We/Wy jest uruchamiana za procesu hostingu aplikacji
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec`             | Praca w toku... | [requestsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsPerSecond) | Liczba żądań przetworzonych przez aplikację 
| `\.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec`    | Praca w toku... | [exceptionsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FexceptionsPerSecond) | Liczba wyjątków zgłoszonych przez aplikację
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time`   | Praca w toku... | [requestExecutionTime](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestExecutionTime) | Średnia liczba żądań czasu wykonywania
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue` | Praca w toku... | [requestsInQueue](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsInQueue) | Liczba żądań oczekujących do przetworzenia w kolejce

## <a name="name"></a>Nazwa

Nazwa metryki, które chcesz zobaczyć w portalu usługi Application Insights i interfejsu użytkownika. 

## <a name="value"></a>Wartość

Pojedynczej wartości miary. Suma poszczególnych pomiarów podczas agregacji.

## <a name="count"></a>Licznik

Waga metryki zagregowane metryki. Nie powinien mieć ustawionej dla miary.

## <a name="min"></a>Min

Minimalna wartość zagregowane metryki. Nie powinien mieć ustawionej dla miary.

## <a name="max"></a>Maks.

Maksymalna wartość zagregowane metryki. Nie powinien mieć ustawionej dla miary.

## <a name="standard-deviation"></a>Odchylenie standardowe

Odchylenie standardowe zagregowane metryki. Nie powinien mieć ustawionej dla miary.

## <a name="custom-properties"></a>Właściwości niestandardowe

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak używać [aplikacji interfejsu API Insights dla niestandardowych zdarzeń i metryk](app-insights-api-custom-events-metrics.md#trackmetric).
- Zobacz [modelu danych](application-insights-data-model.md) dla modelu danych i typów usługi Application Insights.
- Zapoznaj się z [platform](app-insights-platforms.md) obsługiwane przez usługę Application Insights.
