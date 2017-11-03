---
title: "Model danych Telemetrii Insights aplikacji Azure — dane telemetryczne zależności | Dokumentacja firmy Microsoft"
description: "Model danych usługi Insights aplikacji dla dane telemetryczne zależności"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/17/2017
ms.author: mbullwin
ms.openlocfilehash: aa305c30dc358997420be6802d43fa69e45f4a5f
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="dependency-telemetry-application-insights-data-model"></a>Dane telemetryczne zależności: model danych usługi Application Insights

Dane telemetryczne zależności (w [usługi Application Insights](app-insights-overview.md)) reprezentuje interakcji monitorowanego składnika zdalnego składnika, takiego jak SQL lub punkt końcowy HTTP.

## <a name="name"></a>Nazwa

Nazwa polecenia inicjowane z tego wywołania zależności. Wartość Kardynalność niski. Przykłady są nazwa procedury składowanej i szablon ścieżki adresu URL.

## <a name="id"></a>ID

Identyfikator wystąpienia wywołania zależności. Używane na potrzeby korelacji z elementem dane telemetryczne żądania odpowiadające wywołanie zależności. Aby uzyskać więcej informacji, zobacz [korelacji](application-insights-correlation.md) strony.

## <a name="data"></a>Dane

Polecenia inicjowane przez wywołanie zależności. Przykłady są instrukcji SQL i adres URL protokołu HTTP z wszystkie parametry zapytania.

## <a name="type"></a>Typ

Nazwa typu zależności. Kardynalność niskiej wartości logiczne grupowanie zależności i interpretacji innych pól, takich jak commandName i resultCode. Przykłady są SQL, tabeli platformy Azure i HTTP.

## <a name="target"></a>docelowy

Lokacji docelowej wywołania zależności. Przykłady to nazwa serwera, adres hosta. Aby uzyskać więcej informacji, zobacz [korelacji](application-insights-correlation.md) strony.

## <a name="duration"></a>Czas trwania

Żądaj czasu trwania w formacie: `DD.HH:MM:SS.MMMMMM`. Musi być mniejsza niż `1000` dni.

## <a name="result-code"></a>Kod wyniku

Kod wyniku wywołania zależności. Przykłady to kod błędu SQL oraz kod stanu HTTP.

## <a name="success"></a>Powodzenie

Oznaczenia wywołanie powiodła się czy nie.

## <a name="custom-properties"></a>Właściwości niestandardowe

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Miar niestandardowych

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a>Następne kroki

- Konfigurowanie śledzenia dla zależności [.NET](app-insights-asp-net-dependencies.md).
- Konfigurowanie śledzenia dla zależności [Java](app-insights-java-agent.md).
- [Zapisać dane telemetryczne zależności niestandardowych](app-insights-api-custom-events-metrics.md#trackdependency)
- Zobacz [modelu danych](application-insights-data-model.md) dla modelu danych i typów usługi Application Insights.
- Zapoznaj się z [platform](app-insights-platforms.md) obsługiwane przez usługę Application Insights.
