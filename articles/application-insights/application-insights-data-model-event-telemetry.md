---
title: Model danych Telemetrii aplikacji Azure Insights - dane telemetryczne zdarzenia | Dokumentacja firmy Microsoft
description: "Model danych usługi Insights aplikacji dla dane telemetryczne zdarzenia"
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
ms.openlocfilehash: dc6ef1771f4f09b274f12606d1278e1f29c23103
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="event-telemetry-application-insights-data-model"></a>Dane telemetryczne zdarzenia: model danych usługi Application Insights

Można utworzyć zdarzenia telemetrii elementów (w [usługi Application Insights](app-insights-overview.md)) do reprezentowania zdarzenie w aplikacji. Zwykle jest on interakcji z użytkownikiem, takich jak przycisk kliknij lub kolejność wyewidencjonowania. Można także zdarzeń cyklu życia aplikacji, takich jak inicjowanie lub konfiguracji aktualizacji. 

Semantycznie zdarzenia może lub nie może zostać skorelowane z żądania. Jednak jeśli prawidłowo używana, dane telemetryczne zdarzenia jest ważniejsze niż żądań i śladów. Zdarzenia należy tematu, aby rozdzielić od siebie, mniej aktywnego i reprezentują firm telemetrii [próbkowania](app-insights-api-filtering-sampling.md).

## <a name="name"></a>Nazwa

Nazwa zdarzenia. Umożliwia grupowanie prawidłowego i przydatne metryki, należy ograniczyć aplikacji, tak aby generuje niewielką liczbę nazw oddzielne zdarzenie. Na przykład nie używaj osobnej nazwy dla każdego wystąpienia wygenerowane zdarzenia.

Maksymalna długość: 512 znaków

## <a name="custom-properties"></a>Właściwości niestandardowe

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Miar niestandardowych

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Następne kroki

- Zobacz [modelu danych](application-insights-data-model.md) dla modelu danych i typów usługi Application Insights.
- [Pisanie niestandardowych zdarzeń telemetrii](app-insights-api-custom-events-metrics.md#trackevent)
- Zapoznaj się z [platform](app-insights-platforms.md) obsługiwane przez usługę Application Insights.
