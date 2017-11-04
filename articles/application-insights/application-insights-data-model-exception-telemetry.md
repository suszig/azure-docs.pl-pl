---
title: "Model danych Telemetrii aplikacji Azure Insights — dane telemetryczne dotyczące wyjątków | Dokumentacja firmy Microsoft"
description: "Model danych usługi Insights aplikacji dla dane telemetryczne dotyczące wyjątków"
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
ms.openlocfilehash: 3c3c3a39c7986cc771fe4baf60ad9b316888f6ed
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="exception-telemetry-application-insights-data-model"></a>Dane telemetryczne wyjątku: model danych usługi Application Insights

W [usługi Application Insights](app-insights-overview.md), wystąpienie wyjątku reprezentuje obsłużonych i nieobsłużonych wyjątków, który wystąpił podczas wykonywania monitorowanej aplikacji.

## <a name="problem-id"></a>Identyfikator problemu

Identyfikator, gdy wyjątek został zgłoszony w kodzie. Używane do grupowania wyjątków. Zwykle połączenie typ wyjątku i funkcję ze stosu wywołań.

Maksymalna długość: 1024 znaki

## <a name="severity-level"></a>Poziom ważności

Poziom ważności śledzenia. Wartość może być `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Szczegóły wyjątku

(Aby rozszerzony)

## <a name="custom-properties"></a>Właściwości niestandardowe

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Miar niestandardowych

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Następne kroki

- Zobacz [modelu danych](application-insights-data-model.md) dla modelu danych i typów usługi Application Insights.
- Dowiedz się, jak [diagnozowanie wyjątków w aplikacjach sieci web za pomocą usługi Application Insights](app-insights-asp-net-exceptions.md).
- Zapoznaj się z [platform](app-insights-platforms.md) obsługiwane przez usługę Application Insights.
