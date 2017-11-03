---
title: Model danych Telemetrycznych Insights aplikacji Azure | Dokumentacja firmy Microsoft
description: "Omówienie modelu danych usługi Insights aplikacji"
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
ms.author: mbullwin
ms.openlocfilehash: b14eea46e773a4b92ba20cd3121cd258f86307c9
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="application-insights-telemetry-data-model"></a>Model danych telemetrii Insights aplikacji

[Azure Application Insights](app-insights-overview.md) wysyła dane telemetryczne z aplikacji sieci web w portalu Azure, dzięki czemu można analizować wydajności i użycia aplikacji. Model danych telemetrycznych jest standardowym, dzięki czemu można tworzyć platformy i monitorowania niezależny od języka. 

Dane zebrane przez usługę Application Insights modele tego wzorca wykonywania typowych aplikacji:

![Model Application Insights aplikacji](./media/application-insights-data-model/application-insights-data-model.png)

Następujące typy telemetrii są używane do monitorowania wykonywania aplikacji. Trzy następujące typy są zazwyczaj automatycznie gromadzone przez zestaw SDK usługi Application Insights z platforma aplikacji sieci web:

* [**Żądanie** ](application-insights-data-model-request-telemetry.md) — wygenerowanego logowania żądanie odebrane przez aplikację. Na przykład w sieci web usługi Application Insights SDK automatycznie generuje element dane telemetryczne żądania dla każdego żądania HTTP, odbierająca aplikacja sieci web. 

    **Operacji** to wątków wykonywania, który przetwarza żądania. Możesz również [napisać kod](app-insights-api-custom-events-metrics.md#trackrequest) do monitorowania innych operacji, takich jak "wznawiania" w sieci web zadania lub funkcji, które okresowo przetwarza dane.  Każda operacja wymaga identyfikatora. Ten identyfikator, który może być używane do [grupy](application-insights-correlation.md) wszystkie dane telemetryczne generowane podczas przetwarzania żądania aplikacji. Każdej operacji albo zakończy się pomyślnie lub nie powiedzie się i został czasu.
* [**Wyjątek** ](application-insights-data-model-exception-telemetry.md) — zazwyczaj reprezentuje wyjątek, który powoduje, że operacja nie powiodła się.
* [**Zależności** ](application-insights-data-model-dependency-telemetry.md) -reprezentuje wywołanie z aplikacji do zewnętrznej usługi lub pamięci masowej np. interfejsu API REST lub SQL. W programie ASP.NET, wywołań zależności SQL są definiowane przez `System.Data`. Wywołania do punktów końcowych HTTP są definiowane przez `System.Net`. 

Telemetria niestandardowa usługi Application Insights zawiera trzy typy dodatkowe dane:

* [Śledzenia](application-insights-data-model-trace-telemetry.md) — używany bezpośrednio lub za pośrednictwem karty do zaimplementowania rejestrowania diagnostyki za pomocą Instrumentacji platforma, która jest bardzo podobne do, takich jak `Log4Net` lub `System.Diagnostics`.
* [Zdarzenie](application-insights-data-model-event-telemetry.md) — zwykle używane do przechwytywania interakcji z usługą, aby analizować wzorce użycia.
* [Metryka](application-insights-data-model-metric-telemetry.md) — używanych do pomiarów okresowych skalarne raportu.

Każdy element danych telemetrycznych można zdefiniować [informacje o kontekście](application-insights-data-model-context.md) identyfikator sesji aplikacji w wersji lub użytkownika. Kontekst jest zestaw pól jednoznacznie odblokowuje niektórych scenariuszy. Wersja aplikacji został poprawnie zainicjowany, usługi Application Insights może sprawdzić wzorce nowe zachowanie aplikacji skorelowane z ponownego wdrażania. Identyfikator sesji może służyć do obliczenia awarii lub problem wpływ na użytkowników. Dla niektórych obliczanie liczności unikatowych wartości wartości identyfikatora sesji nie może zależności, śledzenie błąd lub wyjątek krytyczny zapewnia dobrą znajomość wpływ.

Model danych telemetrycznych Insights aplikacji definiuje sposób [skorelowania](application-insights-correlation.md) telemetrii, do którego jest częścią operacji. Na przykład żądanie może wywoływać bazy danych SQL i zarejestrowane informacje diagnostyczne. Można ustawić kontekstu korelacji dla tych elementów dane telemetryczne, które powiązanie jej dane telemetryczne żądania.

## <a name="schema-improvements"></a>Ulepszenia schematu

Model danych usługi Insights aplikacji to proste i podstawowa, ale sposób modelu dane telemetryczne aplikacji. Staramy się zachować modelu proste i obsługiwane na potrzeby obsługi scenariuszy podstawowych i umożliwiają rozszerzanie schematu dla zaawansowanych użytkowników.

Do raportowania danych model lub schematu problemy i sugestie używają GitHub [ApplicationInsights głównej](https://github.com/Microsoft/ApplicationInsights-Home/labels/schema) repozytorium.

## <a name="next-steps"></a>Następne kroki

- [Telemetria niestandardowa zapisu](app-insights-api-custom-events-metrics.md)
- Dowiedz się, jak [rozszerzanie i filtrować dane telemetryczne](app-insights-api-filtering-sampling.md).
- Użyj [próbkowania](app-insights-sampling.md) aby zminimalizować ilość danych telemetrycznych oparte na modelu danych.
- Zapoznaj się z [platform](app-insights-platforms.md) obsługiwane przez usługę Application Insights.
