---
title: Konfigurowanie analizy aplikacji sieci Web w technologii ASP.NET za pomocą usługi Application Insights
description: Konfigurowanie analizy wydajności, dostępności i użycia witryny sieci Web w technologii ASP.NET hostowanej lokalnie lub na platformie Azure.
services: application-insights
documentationcenter: .net
author: NumberByColors
manager: douge

ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2016
ms.author: daviste

---
# Konfigurowanie usługi Application Insights dla platformy ASP.NET
Usługa [Visual Studio Application Insights](app-insights-overview.md) służy do monitorowania działającej aplikacji, aby ułatwić [wykrywanie i diagnozowanie problemów z wydajnością oraz wyjątków](app-insights-detect-triage-diagnose.md) i [uzyskiwanie informacji na temat sposobu użycia aplikacji](app-insights-overview-usage.md).  Obsługuje aplikacje, które są hostowane na lokalnych serwerach usług IIS lub na maszynach wirtualnych w chmurze, jak również aplikacje sieci Web platformy Azure.

## Przed rozpoczęciem
Potrzebne są:

* Program Visual Studio 2013 Update 3 lub nowszy (im nowszy, tym lepiej).
* Subskrypcja platformy [Microsoft Azure](http://azure.com). Jeśli Twój zespół lub organizacja mają subskrypcję platformy Azure, właściciel subskrypcji może Cię do niej dodać, korzystając z Twojego [konta Microsoft](http://live.com). 

Istnieją jeszcze inne artykuły, które warto przejrzeć, jeśli interesują Cię następujące tematy:

* [Instrumentacja aplikacji sieci Web w czasie wykonywania](app-insights-monitor-performance-live-website-now.md)
* [Usługi Azure Cloud Services](app-insights-cloudservices.md)

## <a name="ide"></a> 1. Dodawanie zestawu SDK usługi Application Insights
### Nowy projekt
Podczas tworzenia nowego projektu w programie Visual Studio upewnij się, że jest zaznaczona opcja Application Insights. 

![Tworzenie projektu ASP.NET](./media/app-insights-asp-net/appinsights-01-vsnewp1.png)

### Istniejący projekt
Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, a następnie wybierz polecenie **Dodaj telemetrię usługi Application Insights** lub **Konfiguruj usługę Application Insights**.

![Wybieranie opcji Dodaj usługę Application Insights](./media/app-insights-asp-net/appinsights-03-addExisting.png)

* Projekt platformy ASP.NET Core? [Wykonaj następujące instrukcje, aby naprawić kilka wierszy kodu](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started#add-application-insights-instrumentation-code-to-startupcs). 

## <a name="run"></a> 2. Uruchamianie aplikacji
Użyj klawisza F5 do uruchomienia aplikacji i wypróbuj jej działanie: otwórz różne strony, aby wygenerować dane telemetryczne.

W programie Visual Studio zobaczysz liczbę zarejestrowanych zdarzeń. 

![Przycisk usługi Application Insights jest widoczny w programie Visual Studio podczas debugowania.](./media/app-insights-asp-net/54.png)

## 3. Wyświetlanie telemetrii...
### ... w programie Visual Studio
Otwórz okno Application Insights w programie Visual Studio: kliknij przycisk Application Insights albo kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań:

![W programie Visual Studio przycisk Application Insights jest widoczny podczas debugowania.](./media/app-insights-asp-net/55.png)

Ten widok przedstawia dane telemetryczne wygenerowane w aplikacji po stronie serwera. Poeksperymentuj z filtrami, a następnie kliknij dowolne zdarzenie, aby wyświetlić więcej szczegółów.

[Dowiedz się więcej o narzędziach usługi Application Insights w programie Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a> 

### ... w portalu
Jeśli nie wybrano opcji *Zainstaluj tylko zestaw SDK*, dane telemetryczne są widoczne także w portalu sieci Web usługi Application Insights. 

Portal udostępnia więcej wykresów, narzędzi analitycznych i pulpitów nawigacyjnych niż program Visual Studio. 

Otwórz zasób usługi Application Insights w [portalu Azure](https://portal.azure.com/).

![Kliknij projekt prawym przyciskiem myszy i otwórz portal Azure](./media/app-insights-asp-net/appinsights-04-openPortal.png)

W portalu zostanie otwarty widok danych telemetrycznych z Twojej aplikacji:
![](./media/app-insights-asp-net/66.png)

* Pierwsza telemetria zostanie wyświetlona w obszarze [transmisji strumieniowej metryk na żywo](app-insights-metrics-explorer.md#live-metrics-stream).
* Poszczególne zdarzenia są wyświetlane w obszarze **Wyszukiwanie** (1). Wyświetlenie danych może potrwać kilka minut. Kliknij dowolne zdarzenie, aby wyświetlić jego właściwości. 
* Agregowane metryki są wyświetlane na wykresach (2). Wyświetlenie danych w tym miejscu może zająć minutę lub dwie. Kliknij wykres, aby otworzyć blok z bardziej szczegółowymi informacjami.

[Dowiedz się więcej o korzystaniu z usługi Application Insights w portalu Azure](app-insights-dashboards.md).

## 4. Publikowanie aplikacji
Opublikuj aplikacje na serwerze IIS lub na platformie Azure. Obejrzyj [transmisję strumieniową metryk na żywo](app-insights-metrics-explorer.md#live-metrics-stream), aby upewnić się, że wszystko działa bez problemów.

Zobaczysz swoją telemetrię w trakcie budowania w portalu Application Insights, w którym można monitorować metryki, przeszukiwać telemetrię i konfigurować [pulpity nawigacyjne](app-insights-dashboards.md). Można także użyć zaawansowanego [języka zapytań analizy](app-insights-analytics.md) do przeanalizowania użycia i wydajności lub znalezienia określonych zdarzeń. 

Można również analizować telemetrię w programie [Visual Studio](app-insights-visual-studio.md) za pomocą narzędzi, takich jak wyszukiwanie diagnostyczne i [trendy](app-insights-visual-studio-trends.md).

> [!NOTE]
> Jeśli Twoja aplikacja wysyła taką ilość telemetrii, że bliskie jest osiągnięcie [limitów ograniczania przepustowości](app-insights-pricing.md#limits-summary), włączone zostanie [próbkowanie](app-insights-sampling.md) automatyczne. Próbkowania powoduje zmniejszenie ilości telemetrii wysyłanych z aplikacji przy jednoczesnym zachowaniu skorelowanych danych w celach diagnostycznych.
> 
> 

## <a name="land"></a> Do czego służy opcja „Dodaj usługę Application Insights”?
Usługa Application Insights wysyła dane telemetryczne z aplikacji do portalu usługi Application Insights (który jest hostowany na platformie Microsoft Azure):

![](./media/app-insights-asp-net/01-scheme.png)

To polecenie powoduje wykonanie trzech czynności:

1. Dodanie pakietu NuGet zestawu SDK sieci Web usługi Application Insights do projektu. Aby zobaczyć go w programie Visual Studio, kliknij projekt prawym przyciskiem myszy i wybierz polecenie Zarządzaj pakietami NuGet.
2. Utworzenie zasobu usługi Application Insights w [portalu Azure](https://portal.azure.com/). Dane będą widoczne dla tego zasobu. Pobierany jest *klucz instrumentacji*, który identyfikuje zasób.
3. Wstawienie klucza instrumentacji do pliku `ApplicationInsights.config`, dzięki czemu zestaw SDK może wysyłać telemetrię do portalu.

Jeśli chcesz, możesz wykonać te kroki ręcznie na potrzeby platformy [ASP.NET 4](app-insights-asp-net-manual.md) lub [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).

### Uaktualnianie do przyszłych wersji zestawu SDK
Aby przeprowadzić uaktualnienie do [nowej wersji zestawu SDK](app-insights-release-notes-dotnet.md), otwórz ponownie menedżera pakietów NuGet i przefiltruj zainstalowane pakiety. Wybierz Microsoft.ApplicationInsights.Web i kliknij pozycję Uaktualnij.

Jeśli plik ApplicationInsights.config został dostosowany, zapisz jego kopię przed uaktualnieniem, a następnie scal zmiany w nowej wersji.

## Co dalej?
|  |
| --- | --- |
| **[Praca z usługą Application Insights w programie Visual Studio](app-insights-visual-studio.md)**<br/>Debugowanie przy użyciu telemetrii, wyszukiwanie diagnostyczne, przechodzenie do szczegółów kodu. |
| **[Praca z portalem usługi Application Insights](app-insights-dashboards.md)**<br/>Pulpity nawigacyjne, zaawansowane narzędzia diagnostyczne i analityczne, alerty, mapa zależności aplikacji na żywo oraz eksportowanie telemetrii. |
| **[Dodawanie większej ilości danych](app-insights-asp-net-more.md)**<br/>Monitorowanie użycia, dostępności, zależności i wyjątków. Integrowanie śladów ze struktur rejestrowania. Zapisywanie niestandardowych danych telemetrycznych. |

<!--HONumber=sep16_HO1-->


