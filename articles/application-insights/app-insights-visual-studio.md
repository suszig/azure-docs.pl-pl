---
title: "Praca z usługą Application Insights w programie Visual Studio"
description: "Analiza wydajności i diagnostyka podczas debugowania i w środowisku produkcyjnym."
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: douge
ms.assetid: 2059802b-1131-477e-a7b4-5f70fb53f974
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/21/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 8886107f96f31f668ce55ebb4c76e58ad1f21268


---
# <a name="working-with-application-insights-in-visual-studio"></a>Praca z usługą Application Insights w programie Visual Studio
W programie Visual Studio (w wersji 2015 i nowszych) można analizować wydajność i diagnozować problemy zarówno podczas debugowania, jak i w środowisku produkcyjnym, przy użyciu telemetrii z usługi [Visual Studio Application Insights](app-insights-overview.md).

Jeśli nie zostało to jeszcze zrobione, [zainstaluj usługę Application Insights w swojej aplikacji](app-insights-asp-net.md).

## <a name="a-nameruna-debug-your-project"></a><a name="run"></a> Debugowanie projektu
Użyj klawisza F5 do uruchomienia aplikacji i wypróbuj jej działanie: otwórz różne strony, aby wygenerować dane telemetryczne.

W programie Visual Studio zobaczysz liczbę zarejestrowanych zdarzeń.

![Przycisk usługi Application Insights jest widoczny w programie Visual Studio podczas debugowania.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

Kliknij go, aby otworzyć wyszukiwanie diagnostyczne. 

## <a name="diagnostic-search"></a>Wyszukiwanie diagnostyczne
Okno wyszukiwania pokazuje zarejestrowane zdarzenia. (Jeśli zalogowano się do Azure podczas konfigurowania usługi Application Insights, można wyszukać te same zdarzenia w portalu).

![Kliknij prawym przyciskiem myszy projekt i wybierz kolejno opcje Application Insights, Wyszukiwanie](./media/app-insights-visual-studio/34.png)

Wyszukiwanie dowolnego tekstu działa we wszystkich polach zdarzeń. Można wyszukać na przykład część adresu URL strony, wartość właściwości (taką jak miasto klienta) lub określone słowa w dzienniku śledzenia.

Kliknij dowolne zdarzenie, aby wyświetlić jego szczegółowe właściwości.

Można również otworzyć kartę Elementy pokrewne, aby łatwiej diagnozować niepowodzenia żądań lub wyjątki.

![](./media/app-insights-visual-studio/41.png)

## <a name="diagnostics-hub"></a>Centrum diagnostyki
Centrum diagnostyki (w programie Visual Studio 2015 lub nowszym) pokazuje generowane dane telemetryczne serwera Application Insights. Działa to, nawet jeśli została wybrana opcja instalowania samego zestawu SDK, bez łączenia go z zasobem w portalu Azure.

![Otwórz okno Narzędzia diagnostyczne i sprawdź zdarzenia usługi Application Insights.](./media/app-insights-visual-studio/31.png)

## <a name="exceptions"></a>Wyjątki
Jeśli masz [skonfigurowane monitorowanie wyjątków](app-insights-asp-net-exceptions.md), raporty dotyczące wyjątków zostaną wyświetlone w oknie Wyszukiwanie. 

Kliknij wyjątek, aby uzyskać ślad stosu. Jeśli kod aplikacji jest otwarty w programie Visual Studio, można przejść przez ślad stosu do odpowiedniego wiersza kodu.

![Ślad stosu wyjątków](./media/app-insights-visual-studio/17.png)

Ponadto w wierszu Code Lens powyżej każdej metody będzie wyświetlana liczba wyjątków zarejestrowanych przez usługę Application Insights w ciągu ostatnich 24 godzin.

![Ślad stosu wyjątków](./media/app-insights-visual-studio/21.png)

## <a name="local-monitoring"></a>Monitorowanie lokalne
(Od programu Visual Studio 2015 z aktualizacją 2) Jeśli nie skonfigurowano zestawu SDK do wysyłania telemetrii do portalu Application Insights (czyli w pliku ApplicationInsights.config nie ma klucza instrumentacji) w oknie diagnostyki zostanie wyświetlona telemetria z ostatniej sesji debugowania. 

Jest to pożądane, jeśli poprzednia wersji aplikacji została już opublikowana. Lepiej, aby telemetria z sesji debugowania nie była mieszana z telemetrią z opublikowanej aplikacji w portalu Application Insights.

Jest to również przydatne, jeśli masz trochę [niestandardowej telemetrii](app-insights-api-custom-events-metrics.md), którą chcesz debugować przed wysłaniem telemetrii do portalu.

* *Na początku usługa Application Insights została skonfigurowana do wysyłania telemetrii do portalu, ale teraz chcę widzieć telemetrię tylko w programie Visual Studio.*
  
  * W ustawieniach okna wyszukiwania istnieje możliwość wyszukiwania lokalnych danych diagnostycznych, nawet jeśli aplikacja wysyła telemetrię do portalu.
  * Aby zatrzymać przesyłanie telemetrii do portalu, zakomentuj wiersz `<instrumentationkey>...` w pliku ApplicationInsights.config. Gdy wszystko będzie gotowe, odkomentuj wiersz, aby wznowić wysyłanie telemetrii do portalu.

## <a name="trends"></a>Trends
Trends to narzędzie do wizualizacji zachowania aplikacji wraz z upływem czasu. 

Na przycisku paska narzędzi Application Insights lub w oknie Application Insights Search wybierz pozycję **Eksploruj trendy telemetryczne**. Wybierz jedno z pięciu typowych zapytań, aby rozpocząć. W zależności od typów telemetrii, zakresów czasu i innych właściwości możesz analizować różne zestawy danych . 

Aby znaleźć anomalie w danych, wybierz jedną z opcji anomalii w menu rozwijanym „Typ widoku”. Opcje filtrowania u dołu okna ułatwiają sprecyzowanie konkretnych podzestawów w telemetrii.

![Trends](./media/app-insights-visual-studio/51.png)

[Więcej informacji na temat narzędzia Trends](app-insights-visual-studio-trends.md).

## <a name="whats-next"></a>Co dalej?
|  |  |
| --- | --- |
| **[Dodawanie większej ilości danych](app-insights-asp-net-more.md)**<br/>Monitorowanie użycia, dostępności, zależności i wyjątków. Integrowanie śladów ze struktur rejestrowania. Zapisywanie niestandardowych danych telemetrycznych. |![Visual Studio](./media/app-insights-visual-studio/64.png) |
| **[Praca z portalem usługi Application Insights](app-insights-dashboards.md)**<br/>Pulpity nawigacyjne, zaawansowane narzędzia diagnostyczne i analityczne, alerty, mapa zależności aplikacji na żywo oraz eksportowanie telemetrii. |![Visual Studio](./media/app-insights-visual-studio/62.png) |




<!--HONumber=Nov16_HO2-->


