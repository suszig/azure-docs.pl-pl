---
title: "Konfigurowanie analizy aplikacji sieci Web w technologii ASP.NET za pomocą usługi Application Insights | Microsoft Docs"
description: "Konfigurowanie analizy wydajności, dostępności i użycia witryny sieci Web w technologii ASP.NET hostowanej lokalnie lub na platformie Azure."
services: application-insights
documentationcenter: .net
author: NumberByColors
manager: douge
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/13/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: dc95c922b71d18cf791ea98f4ab1a02d2bac2c3b
ms.openlocfilehash: 5103c28047e6d5e7be5f4f3b7933196de7045eeb


---
# <a name="set-up-application-insights-for-aspnet"></a>Konfigurowanie usługi Application Insights dla platformy ASP.NET
Usługa [Azure Application Insights](app-insights-overview.md) monitoruje działającą aplikację, aby ułatwić [wykrywanie i diagnozowanie problemów z wydajnością oraz wyjątków](app-insights-detect-triage-diagnose.md) i [uzyskiwanie informacji na temat sposobu użycia aplikacji](app-insights-overview-usage.md).  Obsługuje aplikacje, które są hostowane na lokalnych serwerach usług IIS lub na maszynach wirtualnych w chmurze, jak również aplikacje sieci Web platformy Azure.

## <a name="before-you-start"></a>Przed rozpoczęciem
Potrzebne są:

* Program Visual Studio 2013 Update 3 lub nowszy (im nowszy, tym lepiej).
* Subskrypcja platformy [Microsoft Azure](http://azure.com). Jeśli Twój zespół lub organizacja mają subskrypcję platformy Azure, właściciel subskrypcji może Cię do niej dodać, korzystając z Twojego [konta Microsoft](http://live.com). 

Istnieją jeszcze inne artykuły, które warto przejrzeć, jeśli interesują Cię następujące tematy:

* [Instrumentacja aplikacji sieci Web w czasie wykonywania](app-insights-monitor-performance-live-website-now.md)
* [Usługi Azure Cloud Services](app-insights-cloudservices.md)

## <a name="a-nameidea-1-add-application-insights-sdk"></a><a name="ide"></a> 1. Dodawanie zestawu SDK usługi Application Insights
### <a name="if-its-a-new-project"></a>Nowy projekt
Podczas tworzenia nowego projektu w programie Visual Studio upewnij się, że jest zaznaczona opcja Application Insights. 

![Tworzenie projektu ASP.NET](./media/app-insights-asp-net/appinsights-01-vsnewp1.png)

### <a name="-or-if-its-an-existing-project"></a>Istniejący projekt
Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, a następnie wybierz polecenie **Dodaj telemetrię usługi Application Insights** lub **Konfiguruj usługę Application Insights**.

![Wybieranie opcji Dodaj usługę Application Insights](./media/app-insights-asp-net/appinsights-03-addExisting.png)

* Projekt platformy ASP.NET Core? [Wykonaj następujące instrukcje, aby naprawić kilka wierszy kodu](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started#add-application-insights-instrumentation-code-to-startupcs). 

## <a name="a-nameruna-2-run-your-app"></a><a name="run"></a> 2. Uruchamianie aplikacji
Użyj klawisza F5 do uruchomienia aplikacji i wypróbuj jej działanie: otwórz różne strony, aby wygenerować dane telemetryczne.

W programie Visual Studio zobaczysz liczbę zarejestrowanych zdarzeń. 

![Przycisk usługi Application Insights jest widoczny w programie Visual Studio podczas debugowania.](./media/app-insights-asp-net/54.png)

## <a name="3-see-your-telemetry"></a>3. Wyświetlanie telemetrii...
### <a name="-in-visual-studio"></a>... w programie Visual Studio
Otwórz okno Application Insights w programie Visual Studio: kliknij przycisk Application Insights albo kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań:

![Przycisk usługi Application Insights jest widoczny w programie Visual Studio podczas debugowania.](./media/app-insights-asp-net/55.png)

Ten widok („Dane z sesji debugowania”) przedstawia dane telemetryczne wygenerowane w aplikacji po stronie serwera. Poeksperymentuj z filtrami, a następnie kliknij dowolne zdarzenie, aby wyświetlić więcej szczegółów.

* *Brak danych? Upewnij się, że zakres czasu jest poprawny, a następnie kliknij ikonę wyszukiwania.*

[Dowiedz się więcej o narzędziach usługi Application Insights w programie Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a> 

### <a name="-in-the-portal"></a>... w portalu
Jeśli nie wybrano opcji *Zainstaluj tylko zestaw SDK*, dane telemetryczne są widoczne także w portalu sieci Web usługi Application Insights. 

Portal udostępnia więcej wykresów, narzędzi analitycznych i pulpitów nawigacyjnych niż program Visual Studio. 

Otwórz zasób usługi Application Insights — zaloguj się do witryny [Azure Portalu](https://portal.azure.com/) i go tam znajdź, lub kliknij prawym przyciskiem myszy projekt w programie Visual Studio i przejdź do niego.

![Kliknij projekt prawym przyciskiem myszy i otwórz portal Azure](./media/app-insights-asp-net/appinsights-04-openPortal.png)

* *Błąd dostępu? Jeśli masz więcej niż jeden zestaw poświadczeń firmy Microsoft, możesz być zalogowany z nieprawidłowym zestawem. W portalu wyloguj się i zaloguj się ponownie.*

W portalu zostanie otwarty widok danych telemetrycznych z Twojej aplikacji: ![strona Omówienie usługi Application Insights](./media/app-insights-asp-net/66.png)

Kliknij dowolny kafelek lub wykres, aby wyświetlić więcej szczegółów.

### <a name="more-detail-in-the-portal"></a>Więcej szczegółów w portalu

* W obszarze [**transmisji strumieniowej metryk na żywo**](app-insights-metrics-explorer.md#live-metrics-stream) dane telemetryczne zostaną wyświetlone niemal natychmiast.

    ![Z poziomu bloku Przegląd kliknij pozycję Transmisja strumieniowa na żywo](./media/app-insights-asp-net/livestream.png)

    Otwórz transmisję strumieniową na żywo, gdy uruchomiona jest aplikacja, aby umożliwić im nawiązanie połączenia.

    W transmisji strumieniowej na żywo dane telemetryczne są wyświetlane tylko przez minutę po wysłaniu. W celu przeprowadzenia badania bardziej pod kątem danych historycznych, użyj funkcji Wyszukiwanie, Eksplorator metryk i Analiza. Wyświetlenie danych w tych miejscach może potrwać kilka minut.

* W funkcji [**Wyszukiwanie** ](app-insights-diagnostic-search.md) wyświetlane są poszczególne zdarzenia, takie jak żądania, wyjątki i wyświetlenia stron. Można filtrować według typu zdarzenia, dopasowania terminu i wartości właściwości. Kliknij dowolne zdarzenie, aby wyświetlić jego właściwości i powiązane zdarzenia. 

    ![Z poziomu bloku Przegląd kliknij pozycję Wyszukaj](./media/app-insights-asp-net/search.png)

 * W trybie projektowania może być wyświetlanych wiele zdarzeń zależności (AJAX). Są to synchronizacje między przeglądarką a emulatorem serwera. Aby je ukryć, kliknij pozycję Filtr zależności.
* [**Zagregowane metryki **](app-insights-metrics-explorer.md), takie jak żądania i współczynniki błędów, są wyświetlane na wykresach. Kliknij wykres, aby otworzyć blok z bardziej szczegółowymi informacjami. Kliknij tag **Edytuj** na dowolnym wykresie, aby ustawić filtry, rozmiar i inne elementy.
    
    ![Z poziomu bloku Przegląd kliknij dowolny wykres](./media/app-insights-asp-net/metrics.png)

[Dowiedz się więcej o korzystaniu z usługi Application Insights w portalu Azure](app-insights-dashboards.md).

## <a name="4-publish-your-app"></a>4. Publikowanie aplikacji
Opublikuj aplikacje na serwerze IIS lub na platformie Azure. Obejrzyj [transmisję strumieniową metryk na żywo](app-insights-metrics-explorer.md#live-metrics-stream), aby upewnić się, że wszystko działa bez problemów.

Zobaczysz swoją telemetrię w trakcie budowania w portalu Application Insights, w którym można monitorować metryki, przeszukiwać telemetrię i konfigurować [pulpity nawigacyjne](app-insights-dashboards.md). Można także użyć zaawansowanego [języka zapytań analizy](app-insights-analytics.md) do przeanalizowania użycia i wydajności lub znalezienia określonych zdarzeń. 

Można również analizować telemetrię w programie [Visual Studio](app-insights-visual-studio.md) za pomocą narzędzi, takich jak wyszukiwanie diagnostyczne i [trendy](app-insights-visual-studio-trends.md).

> [!NOTE]
> Jeśli Twoja aplikacja wysyła taką ilość telemetrii, że bliskie jest osiągnięcie [limitów ograniczania przepustowości](app-insights-pricing.md#limits-summary), włączone zostanie [próbkowanie](app-insights-sampling.md) automatyczne. Próbkowania powoduje zmniejszenie ilości telemetrii wysyłanych z aplikacji przy jednoczesnym zachowaniu skorelowanych danych w celach diagnostycznych.
> 
> 

## <a name="a-namelanda-what-did-add-application-insights-do"></a><a name="land"></a> Do czego służy opcja „Dodaj usługę Application Insights”?
Usługa Application Insights wysyła dane telemetryczne z aplikacji do portalu usługi Application Insights (który jest hostowany na platformie Microsoft Azure):

![](./media/app-insights-asp-net/01-scheme.png)

To polecenie powoduje wykonanie trzech czynności:

1. Dodanie pakietu NuGet zestawu SDK sieci Web usługi Application Insights do projektu. Aby zobaczyć go w programie Visual Studio, kliknij projekt prawym przyciskiem myszy i wybierz polecenie Zarządzaj pakietami NuGet.
2. Utworzenie zasobu usługi Application Insights w [portalu Azure](https://portal.azure.com/). Dane będą widoczne dla tego zasobu. Pobierany jest *klucz instrumentacji*, który identyfikuje zasób.
3. Wstawienie klucza instrumentacji do pliku `ApplicationInsights.config`, dzięki czemu zestaw SDK może wysyłać telemetrię do portalu.

Jeśli chcesz, możesz wykonać te kroki ręcznie na potrzeby platformy [ASP.NET 4](app-insights-windows-services.md) lub [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).

### <a name="to-upgrade-to-future-sdk-versions"></a>Uaktualnianie do przyszłych wersji zestawu SDK
Aby przeprowadzić uaktualnienie do [nowej wersji zestawu SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), otwórz ponownie menedżera pakietów NuGet i przefiltruj zainstalowane pakiety. Wybierz Microsoft.ApplicationInsights.Web i kliknij pozycję Uaktualnij.

Jeśli plik ApplicationInsights.config został dostosowany, zapisz jego kopię przed uaktualnieniem, a następnie scal zmiany w nowej wersji.

## <a name="add-more-telemetry"></a>Dodawanie kolejnych funkcji telemetrii
### <a name="web-pages-and-single-page-apps"></a>Strony sieci Web i aplikacje jednostronicowe
1. [Dodaj fragment kodu JavaScript](app-insights-javascript.md) do stron sieci Web, aby wzbogacić zawartość bloków Przeglądarka i Użycie o dane dotyczące wyświetleń strony, czasów ładowania, wyjątków przeglądarki, wydajności wywołań AJAX oraz liczby użytkowników i sesji.
2. [Zakoduj zdarzenia niestandardowe](app-insights-api-custom-events-metrics.md), aby zliczać lub mierzyć akcje użytkownika bądź sprawdzać ich czas.

### <a name="dependencies-exceptions-and-performance-counters"></a>Zależności, wyjątki i liczniki wydajności
[Zainstaluj monitor stanu](app-insights-monitor-performance-live-website-now.md) na każdym komputerze serwera, aby uzyskać dodatkową telemetrię dotyczącą swojej aplikacji. Oto, co otrzymujesz:

* [Liczniki wydajności](app-insights-performance-counters.md) - 
  procesora, pamięci, dysku i inne liczniki wydajności dotyczące Twojej aplikacji. 
* [Wyjątki](app-insights-asp-net-exceptions.md) — bardziej szczegółowa telemetria dla niektórych wyjątków.
* [Zależności](app-insights-asp-net-dependencies.md) — wywołania interfejsu API REST lub usług SQL. Dowiedz się, czy powolne odpowiedzi składników zewnętrznych są przyczyną problemów z wydajnością aplikacji. (Jeśli aplikacja działa na platformie .NET 4.6, nie potrzebujesz monitora stanu, aby uzyskać tę telemetrię).

### <a name="diagnostic-code"></a>Kod diagnostyczny
Masz problem? Jeśli chcesz wstawić w aplikacji kod, który pomoże w zdiagnozowaniu problemu, masz kilka opcji:

* [Przechwytywanie danych śledzenia dziennika](app-insights-asp-net-trace-logs.md): Jeśli korzystasz już z narzędzia Log4N, NLog lub System.Diagnostics.Trace, aby rejestrować zdarzenia śledzenia, to możesz wysyłać dane wyjściowe do usługi Application Insights, aby móc skorelować te dane z żądaniami, przeszukać je oraz przeanalizować. 
* [Niestandardowe zdarzenia i metryki](app-insights-api-custom-events-metrics.md): Użyj poleceń TrackEvent() i TrackMetric() na serwerze lub w kodzie strony sieci Web.
* [Tagowanie telemetrii za pomocą dodatkowych właściwości](app-insights-api-filtering-sampling.md#add-properties)

Użyj opcji [Wyszukaj](app-insights-diagnostic-search.md), aby znaleźć i skorelować określone zdarzenia, oraz opcji [Analiza](app-insights-analytics.md), aby wykonywać bardziej zaawansowane kwerendy.

## <a name="alerts"></a>Alerty
Bądź pierwszą osobą, która się dowie, że w aplikacji występują problemy. (Nie czekaj na informacje od użytkowników!) 

* [Utwórz testy sieci Web](app-insights-monitor-web-app-availability.md), aby upewnić się, że Twoja witryna jest widoczna w sieci Web.
* [Proaktywna diagnostyka](app-insights-proactive-diagnostics.md) uruchamiana automatycznie (jeśli aplikacja generuje określony minimalny ruch). Nie musisz nic robić, aby ją skonfigurować. Ta funkcja powiadomi Cię, jeśli w aplikacji występuje nietypowa liczba nieudanych żądań.
* [Ustaw alerty metryki](app-insights-alerts.md) w celu otrzymania ostrzeżenia, gdy metryka przekroczy próg. Możesz je ustawić dla metryk niestandardowych, które zakodujesz w aplikacji.

Domyślnie powiadomienia o alertach są wysyłane do właściciela subskrypcji platformy Azure. 

![przykładowa wiadomość e-mail z alertem](./media/app-insights-asp-net/alert-email.png)

## <a name="version-and-release-tracking"></a>Śledzenie wersji i wydania
### <a name="track-application-version"></a>Śledzenie wersji aplikacji
Upewnij się, że plik `buildinfo.config` jest generowany przez proces MSBuild. W pliku .csproj dodaj ten kod:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup> 
```

Jeśli plik zawiera informację o kompilacji, moduł sieci Web usługi Application Insights automatycznie dodaje **wersję aplikacji** jako właściwość do każdego elementu telemetrii. Pozwala to na filtrowanie według wersji podczas przeprowadzania [wyszukiwania diagnostycznego](app-insights-diagnostic-search.md) lub [eksplorowania metryk](app-insights-metrics-explorer.md). 

Numer wersji kompilacji jest jednak generowany tylko przez program MS Build, a nie podczas kompilowania przez deweloperów w programie Visual Studio.

### <a name="release-annotations"></a>Adnotacje dotyczące wersji
Jeśli korzystasz z usługi Visual Studio Team Services, możesz [uzyskać znacznik adnotacji](app-insights-annotations.md) dodawany do Twoich wykresów, za każdym razem, gdy wydasz nową wersję.

![przykładowa adnotacja dotycząca wersji](./media/app-insights-asp-net/release-annotation.png)

## <a name="next-steps"></a>Następne kroki
|  |
| --- | --- |
| **[Praca z usługą Application Insights w programie Visual Studio](app-insights-visual-studio.md)**<br/>Debugowanie przy użyciu telemetrii, wyszukiwanie diagnostyczne, przechodzenie do szczegółów kodu. |
| **[Praca z portalem usługi Application Insights](app-insights-dashboards.md)**<br/>Pulpity nawigacyjne, zaawansowane narzędzia diagnostyczne i analityczne, alerty, mapa zależności aplikacji na żywo oraz eksportowanie telemetrii. |
| **[Dodawanie większej ilości danych](app-insights-asp-net-more.md)**<br/>Monitorowanie użycia, dostępności, zależności i wyjątków. Integrowanie śladów ze struktur rejestrowania. Zapisywanie niestandardowych danych telemetrycznych. |




<!--HONumber=Dec16_HO3-->


