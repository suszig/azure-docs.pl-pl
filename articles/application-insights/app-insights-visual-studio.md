<properties 
    pageTitle="Praca z usługą Application Insights w programie Visual Studio" 
    description="Analiza wydajności i diagnostyka podczas debugowania i w środowisku produkcyjnym." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="05/25/2016" 
    ms.author="awills"/>


# Praca z usługą Application Insights w programie Visual Studio

W programie Visual Studio (w wersji 2015 i nowszych) można analizować wydajność i diagnozować problemy zarówno podczas debugowania, jak i w środowisku produkcyjnym, przy użyciu telemetrii z usługi [Visual Studio Application Insights](app-insights-overview.md).

Jeśli nie zostało to jeszcze zrobione, [zainstaluj usługę Application Insights w swojej aplikacji](app-insights-asp-net.md).

## <a name="run"></a> Debugowanie projektu

Użyj klawisza F5 do uruchomienia aplikacji i wypróbuj jej działanie: otwórz różne strony, aby wygenerować dane telemetryczne.

W programie Visual Studio zobaczysz liczbę zarejestrowanych zdarzeń.

![Przycisk usługi Application Insights jest widoczny w programie Visual Studio podczas debugowania.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

Kliknij go, aby otworzyć wyszukiwanie diagnostyczne. 



## Wyszukiwanie diagnostyczne

Okno wyszukiwania pokazuje zarejestrowane zdarzenia. (Jeśli zalogowano się do Azure podczas konfigurowania usługi Application Insights, można wyszukać te same zdarzenia w portalu).

![Kliknij prawym przyciskiem myszy projekt i wybierz kolejno opcje Application Insights, Wyszukiwanie](./media/app-insights-visual-studio/34.png)

Wyszukiwanie dowolnego tekstu działa we wszystkich polach zdarzeń. Można wyszukać na przykład część adresu URL strony, wartość właściwości (taką jak miasto klienta) lub określone słowa w dzienniku śledzenia.

Kliknij dowolne zdarzenie, aby wyświetlić jego szczegółowe właściwości.

Można również otworzyć kartę Elementy pokrewne, aby łatwiej diagnozować niepowodzenia żądań lub wyjątki.


![](./media/app-insights-visual-studio/41.png)



## Centrum diagnostyki

Centrum diagnostyki (w programie Visual Studio 2015 lub nowszym) pokazuje generowane dane telemetryczne serwera Application Insights. Działa to, nawet jeśli została wybrana opcja instalowania samego zestawu SDK, bez łączenia go z zasobem w portalu Azure.

![Otwórz okno Narzędzia diagnostyczne i sprawdź zdarzenia usługi Application Insights.](./media/app-insights-visual-studio/31.png)


## Wyjątki

Jeśli masz [skonfigurowane monitorowanie wyjątków](app-insights-asp-net-exceptions.md), raporty dotyczące wyjątków zostaną wyświetlone w oknie Wyszukiwanie. 

Kliknij wyjątek, aby uzyskać ślad stosu. Jeśli kod aplikacji jest otwarty w programie Visual Studio, można przejść przez ślad stosu do odpowiedniego wiersza kodu.


![](./media/app-insights-visual-studio/17.png)


## Monitorowanie lokalne



(Od programu Visual Studio 2015 z aktualizacją 2) Jeśli nie skonfigurowano zestawu SDK do wysyłania telemetrii do portalu Application Insights (czyli w pliku ApplicationInsights.config nie ma klucza instrumentacji) w oknie diagnostyki zostanie wyświetlona telemetria z ostatniej sesji debugowania. 

Jest to pożądane, jeśli poprzednia wersji aplikacji została już opublikowana. Lepiej, aby telemetria z sesji debugowania nie była mieszana z telemetrią z opublikowanej aplikacji w portalu Application Insights.

Jest to również przydatne, jeśli masz trochę [niestandardowej telemetrii](app-insights-api-custom-events-metrics.md), którą chcesz debugować przed wysłaniem telemetrii do portalu.


* *Na początku usługa Application Insights została skonfigurowana do wysyłania telemetrii do portalu, ale teraz chcę widzieć telemetrię tylko w programie Visual Studio.*

 * W ustawieniach okna wyszukiwania istnieje możliwość wyszukiwania lokalnych danych diagnostycznych, nawet jeśli aplikacja wysyła telemetrię do portalu.
 * Aby zatrzymać przesyłanie telemetrii do portalu, zakomentuj wiersz `<instrumentationkey>...` w pliku ApplicationInsights.config. Gdy wszystko będzie gotowe, odkomentuj wiersz, aby wznowić wysyłanie telemetrii do portalu.





## Uaktualnianie do przyszłych wersji zestawu SDK

Aby uaktualnić do [nowej wersji zestawu SDK](app-insights-release-notes-dotnet.md), otwórz ponownie menedżera pakietów NuGet i przefiltruj zainstalowane pakiety. Wybierz Microsoft.ApplicationInsights.Web i kliknij pozycję Uaktualnij.

Jeśli plik ApplicationInsights.config został dostosowany, zapisz jego kopię przed uaktualnieniem, a następnie scal zmiany w nowej wersji.



## Co dalej?

||
|---|---
|**[Dodawanie większej ilości danych](app-insights-asp-net-more.md)**<br/>Monitorowanie użycia, dostępności, zależności i wyjątków. Integrowanie śladów ze struktur rejestrowania. Zapisywanie niestandardowych danych telemetrycznych. | ![Visual Studio](./media/app-insights-asp-net/64.png)
|**[Praca z portalem Application Insights](app-insights-dashboards.md)**<br/>Pulpity nawigacyjne, zaawansowane narzędzia diagnostyczne i analityczne, alerty, mapa zależności aplikacji na żywo oraz eksportowanie telemetrii. |![Visual Studio](./media/app-insights-asp-net/62.png)


 



<!--HONumber=jun16_HO2-->


