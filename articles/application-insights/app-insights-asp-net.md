---
title: "Konfigurowanie analizy aplikacji sieci Web w technologii ASP.NET za pomocą usługi Azure Application Insights | Microsoft Docs"
description: "Konfigurowanie analizy wydajności, dostępności i użycia witryny sieci Web w technologii ASP.NET hostowanej lokalnie lub na platformie Azure."
services: application-insights
documentationcenter: .net
author: NumberByColors
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: bcd8b3b7fb44a28d7184d14969eb998ce07c8e53
ms.lasthandoff: 03/31/2017


---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>Konfigurowanie usługi Application Insights dla witryny sieci Web ASP.NET.
Usługa [Azure Application Insights](app-insights-overview.md) monitoruje działającą aplikację, aby ułatwić [wykrywanie i diagnozowanie problemów z wydajnością oraz wyjątków](app-insights-detect-triage-diagnose.md). Ułatwia ona także [uzyskiwanie informacji na temat sposobu użycia aplikacji](app-insights-overview-usage.md). Obsługuje ona funkcję Web Apps usługi Azure App Service, a także aplikacje, które są hostowane na lokalnych serwerach usług IIS lub na maszynach wirtualnych w chmurze.

## <a name="before-you-start"></a>Przed rozpoczęciem
Potrzebne są:

* Program Visual Studio 2013 Update 3 lub nowszy (im nowszy, tym lepiej).
* Subskrypcja platformy [Microsoft Azure](http://azure.com). Jeśli Twój zespół lub organizacja mają subskrypcję platformy Azure, właściciel subskrypcji może Cię do niej dodać, korzystając z Twojego [konta Microsoft](http://live.com).

Istnieją jeszcze inne tematy, które warto przejrzeć, jeśli interesują Cię następujące kwestie:

* [Instrumentacja aplikacji sieci Web w czasie wykonywania](app-insights-monitor-performance-live-website-now.md)
* [Azure Cloud Services](app-insights-cloudservices.md)

## <a name="ide"></a> Krok 1. Dodawanie zestawu SDK usługi Application Insights

Kliknij prawym przyciskiem myszy projekt aplikacji sieci Web w Eksploratorze rozwiązań, a następnie wybierz polecenie **Dodaj**, **Telemetria usługi Application Insights...** lub **Konfiguruj usługę Application Insights**.

![Zrzut ekranu Eksploratora rozwiązań z wyróżnioną opcją Dodaj telemetrię usługi Application Insights](./media/app-insights-asp-net/appinsights-03-addExisting.png)

(W programie Visual Studio 2015 istnieje również opcja dodania usługi Application Insights w oknie dialogowym Nowy projekt).

Przejdź do strony konfiguracji usługi Application Insights:

![Zrzut ekranu strony Zarejestruj swoją aplikację w usłudze Application Insights](./media/app-insights-asp-net/visual-studio-register-dialog.png)

1. Wybierz konto i subskrypcję używane do uzyskiwania dostępu do platformy Azure.
2. Wybierz zasób na platformie Azure, w którym mają być wyświetlane dane z aplikacji. Zazwyczaj należy utworzyć osobny zasób dla każdej aplikacji. Jeśli chcesz ustawić grupę zasobów lub lokalizację, w której dane są przechowywane, kliknij pozycję **Konfiguruj ustawienia**. Grupy zasobów są używane do kontrolowania dostępu do danych. Jeśli na przykład masz kilka aplikacji, które stanowią część tego samego systemu, możesz umieścić ich dane usługi Application Insights w tej samej grupie zasobów.
3. Usługa Application Insights jest bezpłatna do pewnej ilości danych telemetrycznych. Aby uniknąć naliczania opłat, możesz ustawić limit dla tej ilości. Po utworzeniu zasobu możesz zmienić wybór w portalu, wybierając pozycję **Funkcje i cennik**, **Zarządzanie ilością danych**, **Dzienny limit ilości**.
4. Kliknij przycisk **Zarejestruj**, aby skonfigurować usługę Application Insights dla aplikacji sieci Web. Dane telemetryczne będą wysyłane do witryny [Azure Portal](https://portal.azure.com) zarówno podczas debugowania, jak i po opublikowaniu aplikacji.
5. Alternatywnie możesz po prostu dodać zestaw SDK usługi Application Insights do swojej aplikacji. W takim przypadku dane telemetryczne będzie można wyświetlać w programie Visual Studio podczas debugowania. Później możesz powrócić do tej strony konfiguracji lub po wdrożeniu aplikacji [włączyć telemetrię w czasie wykonywania](app-insights-monitor-performance-live-website-now.md).


## <a name="run"></a> Krok 2. Uruchamianie aplikacji
Uruchom aplikację, naciskając klawisz F5. Otwórz różne strony w celu wygenerowania telemetrii.

W programie Visual Studio zobaczysz liczbę zarejestrowanych zdarzeń.

![Zrzut ekranu programu Visual Studio. Przycisk usługi Application Insights jest widoczny podczas debugowania.](./media/app-insights-asp-net/54.png)

## <a name="step-3-see-your-telemetry-in-visual-studio-or-application-insights"></a>Krok 3. Wyświetlanie telemetrii w programie Visual Studio lub w usłudze Application Insights
Telemetrię można wyświetlić w programie Visual Studio lub w portalu sieci Web usługi Application Insights.

**W programie Visual Studio** otwórz okno Application Insights. Kliknij przycisk **Application Insights** albo kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, wybierz pozycję **Application Insights**, a następnie kliknij pozycję **Wyszukaj telemetrię na żywo**.

W oknie wyszukiwania usługi Visual Studio Application Insights wyświetl widok **Dane z sesji debugowania** dla telemetrii wygenerowanej po stronie serwera aplikacji. Poeksperymentuj z filtrami, a następnie kliknij dowolne zdarzenie, aby wyświetlić więcej szczegółów.

![Zrzut ekranu przedstawiający widok Dane z sesji debugowania w oknie Application Insights.](./media/app-insights-asp-net/55.png)

> [!NOTE]
> Jeśli nie są wyświetlane żadne dane, upewnij się, że zakres czasu jest poprawny, a następnie kliknij ikonę wyszukiwania.

[Dowiedz się więcej o narzędziach usługi Application Insights w programie Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a>
### <a name="the-application-insights-web-portal"></a>Portal sieci Web usługi Application Insights
Telemetrię można również wyświetlić **w portalu sieci Web usługi Application Insights**, chyba że wybrano opcję zainstalowania tylko zestawu SDK. Portal udostępnia więcej wykresów, narzędzi analitycznych i pulpitów nawigacyjnych niż program Visual Studio.

Otwórz zasób usługi Application Insights. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/) i go tam znajdź lub kliknij prawym przyciskiem myszy projekt w programie Visual Studio i przejdź do niego.

![Zrzut ekranu programu Visual Studio przedstawiający sposób otwierania portalu Application Insights](./media/app-insights-asp-net/appinsights-04-openPortal.png)

> [!NOTE]
> Jeśli wystąpi błąd dostępu: czy masz więcej niż jeden zestaw poświadczeń firmy Microsoft i czy logowanie nastąpiło za pomocą nieprawidłowego zestawu? W portalu wyloguj się i zaloguj się ponownie.

W portalu zostanie otwarty widok danych telemetrycznych z Twojej aplikacji.

![Zrzut ekranu przedstawiający stronę przeglądu usługi Application Insights](./media/app-insights-asp-net/66.png)

Kliknij dowolny kafelek lub wykres, aby wyświetlić więcej szczegółów.

### <a name="more-details-in-the-application-insights-web-portal"></a>Więcej szczegółów w portalu sieci Web usługi Application Insights
Oto kilka przykładów sposobu udostępniania w portalu większej liczby szczegółów.

* W obszarze [**transmisji strumieniowej metryk na żywo**](app-insights-live-stream.md) dane telemetryczne zostaną wyświetlone niemal natychmiast.

    ![Zrzut ekranu przedstawiający portal. Z poziomu bloku Przegląd kliknij pozycję Live Stream.](./media/app-insights-asp-net/livestream.png)

    Otwórz transmisję Live Metrics Stream, gdy uruchomiona jest aplikacja, aby umożliwić im nawiązanie połączenia.

    W transmisji Live Metrics Stream dane telemetryczne są wyświetlane tylko przez minutę po wysłaniu. W celu przeprowadzenia badania bardziej pod kątem danych historycznych, użyj funkcji Wyszukiwanie, Eksplorator metryk i Analiza. Wyświetlenie danych w tych miejscach może potrwać kilka minut.

* W funkcji [**Wyszukiwanie**](app-insights-diagnostic-search.md) wyświetlane są poszczególne zdarzenia, takie jak żądania, wyjątki i wyświetlenia stron. Można filtrować według typu zdarzenia, dopasowania terminu i wartości właściwości. Kliknij dowolne zdarzenie, aby wyświetlić jego właściwości i powiązane zdarzenia.

    ![Zrzut ekranu przedstawiający portal. Z poziomu bloku Przegląd kliknij pozycję Wyszukaj.](./media/app-insights-asp-net/search.png)

 * W trybie projektowania może być wyświetlanych wiele zdarzeń zależności (AJAX). Są to synchronizacje między przeglądarką a emulatorem serwera. Aby je ukryć, kliknij filtr **Zależność**.
* [**Zagregowane metryki**](app-insights-metrics-explorer.md), takie jak żądania i współczynniki błędów, są wyświetlane na wykresach. Kliknij wykres, aby otworzyć blok z bardziej szczegółowymi informacjami. Kliknij tag **Edytuj** na dowolnym wykresie, aby ustawić filtry i rozmiar.

    ![Zrzut ekranu przedstawiający blok zagregowanych metryk dostępnych w portalu](./media/app-insights-asp-net/metrics.png)

[Dowiedz się więcej o korzystaniu z usługi Application Insights w portalu Azure](app-insights-dashboards.md).

## <a name="step-4-publish-your-app"></a>Krok 4. Publikowanie aplikacji
Opublikuj aplikacje na serwerze IIS lub na platformie Azure. Obejrzyj [transmisję strumieniową metryk na żywo](app-insights-metrics-explorer.md#live-metrics-stream), aby upewnić się, że wszystko działa bez problemów.

Dane telemetryczne są gromadzone w portalu Application Insights, w którym można monitorować metryki, przeszukiwać telemetrię i konfigurować [pulpity nawigacyjne](app-insights-dashboards.md). Można także użyć zaawansowanego [języka zapytań analizy](app-insights-analytics.md) do przeanalizowania użycia i wydajności lub znalezienia określonych zdarzeń.

Można również analizować telemetrię w programie [Visual Studio](app-insights-visual-studio.md) za pomocą narzędzi, takich jak wyszukiwanie diagnostyczne i [trendy](app-insights-visual-studio-trends.md).

> [!NOTE]
> Jeśli Twoja aplikacja wysyła taką ilość telemetrii, że bliskie jest osiągnięcie [limitów ograniczania przepustowości](app-insights-pricing.md#limits-summary), włączone zostanie [próbkowanie](app-insights-sampling.md) automatyczne. Próbkowania powoduje zmniejszenie ilości telemetrii wysyłanych z aplikacji przy jednoczesnym zachowaniu skorelowanych danych w celach diagnostycznych.
>
>

## <a name="land"></a> Do czego służy polecenie Dodaj usługę Application Insights?
Usługa Application Insights wysyła dane telemetryczne z aplikacji do portalu usługi Application Insights (który jest hostowany na platformie Azure).

![Diagram przepływu danych telemetrycznych](./media/app-insights-asp-net/01-scheme.png)

To polecenie powoduje wykonanie trzech czynności:

1. Dodanie pakietu NuGet zestawu SDK sieci Web usługi Application Insights do projektu. Aby zobaczyć go w programie Visual Studio, kliknij projekt prawym przyciskiem myszy i wybierz polecenie **Zarządzaj pakietami NuGet**.
2. Utworzenie zasobu usługi Application Insights w witrynie [Azure Portal](https://portal.azure.com/). Dane będą widoczne dla tego zasobu. Pobierany jest *klucz instrumentacji*, który identyfikuje zasób.
3. Wstawienie klucza instrumentacji do pliku `ApplicationInsights.config`, dzięki czemu zestaw SDK może wysyłać telemetrię do portalu.

Jeśli chcesz, możesz wykonać te kroki ręcznie na potrzeby platformy [ASP.NET 4](app-insights-windows-services.md) lub [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).

### <a name="upgrade-to-future-sdk-versions"></a>Uaktualnianie do przyszłych wersji zestawu SDK
Aby przeprowadzić uaktualnienie do [nowej wersji zestawu SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), otwórz ponownie **menedżera pakietów NuGet** i przefiltruj zainstalowane pakiety. Wybierz **Microsoft.ApplicationInsights.Web** i kliknij pozycję **Uaktualnij**.

Jeśli plik ApplicationInsights.config został dostosowany, zapisz jego kopię przed uaktualnieniem. Następnie scal zmiany w nowej wersji.

## <a name="add-more-telemetry"></a>Dodawanie kolejnych funkcji telemetrii
Poniżej przedstawiono inne rodzaje danych telemetrycznych, jakie można dodać.
### <a name="dependencies-exceptions-and-performance-counters"></a>Zależności, wyjątki i liczniki wydajności

[Zainstaluj monitor stanu](http://go.microsoft.com/fwlink/?LinkId=506648) na każdym komputerze serwera usług IIS, aby uzyskać dodatkową telemetrię dotyczącą aplikacji sieci Web. Jeśli monitor został już zainstalowany, nie trzeba wykonywać żadnych czynności. (Możliwe, że używasz już monitora stanu do monitorowania aplikacji w czasie wykonywania).

Używając monitora stanu wraz z zestawem SDK w czasie kompilacji, otrzymujesz bardziej kompletny zestaw danych telemetrycznych obejmujący:

* [Liczniki wydajności](app-insights-performance-counters.md): liczniki procesora CPU, pamięci, dysku i inne liczniki wydajności dotyczące Twojej aplikacji.
* [Wyjątki](app-insights-asp-net-exceptions.md): bardziej szczegółowa telemetria dla niektórych wyjątków.
* [Zależności](app-insights-asp-net-dependencies.md): w tym wartości zwracane.

### <a name="webpages-and-single-page-apps"></a>Strony sieci Web i aplikacje jednostronicowe
1. [Dodaj fragment kodu JavaScript](app-insights-javascript.md) do stron sieci Web, aby wyświetlić dane dotyczące wyświetleń strony, czasów ładowania, wyjątków przeglądarki, wydajności wywołań AJAX oraz liczby użytkowników i sesji. Te dane są wyświetlane w blokach Przeglądarka i Użycie.
2. [Zakoduj zdarzenia niestandardowe](app-insights-api-custom-events-metrics.md), aby zliczać lub mierzyć akcje użytkownika bądź sprawdzać ich czas.


### <a name="diagnostic-code"></a>Kod diagnostyczny
Masz problem? Jeśli chcesz wstawić w aplikacji kod, który pomoże w zdiagnozowaniu problemu, masz kilka opcji:

* [Przechwytywanie danych śledzenia dziennika](app-insights-asp-net-trace-logs.md): Jeśli korzystasz już z narzędzia Log4N, NLog lub System.Diagnostics.Trace, aby rejestrować zdarzenia śledzenia, to możesz wysyłać dane wyjściowe do usługi Application Insights. Dzięki temu można skorelować te dane wyjściowe z żądaniami, przeszukać je oraz przeanalizować.
* [Niestandardowe zdarzenia i metryki](app-insights-api-custom-events-metrics.md): Użyj poleceń TrackEvent() i TrackMetric() na serwerze lub w kodzie strony sieci Web.
* [Oznacz dane telemetryczne za pomocą dodatkowych właściwości](app-insights-api-filtering-sampling.md#add-properties).

Użyj opcji [Wyszukaj](app-insights-diagnostic-search.md), aby znaleźć i skorelować określone zdarzenia, oraz opcji [Analiza](app-insights-analytics.md), aby wykonywać bardziej zaawansowane kwerendy.

## <a name="alerts"></a>Alerty
Bądź pierwszą osobą, która się dowie, że w aplikacji występują problemy.

* [Testy dostępności](app-insights-monitor-web-app-availability.md): Utwórz testy, aby upewnić się, że Twoja witryna jest widoczna w sieci Web.
* [Inteligentne diagnostyki](app-insights-proactive-diagnostics.md): Te testy są uruchamiane automatycznie, więc nie trzeba wykonywać żadnych czynności, aby je skonfigurować. Ta funkcja powiadomi Cię, jeśli w aplikacji występuje nietypowa liczba nieudanych żądań.
* [Alerty metryki](app-insights-alerts.md): Ustaw je w celu otrzymania ostrzeżenia, gdy metryka przekroczy próg. Możesz je ustawić dla metryk niestandardowych, które zakodujesz w aplikacji.

Domyślnie powiadomienia o alertach są wysyłane do właściciela subskrypcji platformy Azure.

![Zrzut ekranu przedstawiający przykładową wiadomości e-mail z alertem](./media/app-insights-asp-net/alert-email.png)

## <a name="version-and-release-tracking"></a>Śledzenie wersji i wydania
Aby śledzić wersje aplikacji, upewnij się, że plik `buildinfo.config` jest generowany przez proces aparatu Microsoft Build Engine. W pliku .csproj dodaj ten kod:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Jeśli plik zawiera informację o kompilacji, moduł sieci Web usługi Application Insights automatycznie dodaje **wersję aplikacji** jako właściwość do każdego elementu telemetrii. Pozwala to na filtrowanie według wersji podczas przeprowadzania [wyszukiwania diagnostycznego](app-insights-diagnostic-search.md) lub [eksplorowania metryk](app-insights-metrics-explorer.md).

Numer wersji kompilacji jest jednak generowany tylko przez aparat Microsoft Build Engine, a nie podczas kompilowania przez deweloperów w programie Visual Studio.

### <a name="release-annotations"></a>Adnotacje dotyczące wersji
Jeśli korzystasz z usługi Visual Studio Team Services, możesz [uzyskać znacznik adnotacji](app-insights-annotations.md) dodawany do Twoich wykresów, za każdym razem, gdy wydasz nową wersję. Na następującej ilustracji pokazano sposób wyświetlania tego znacznika.

![Zrzut ekranu przedstawiający przykładową adnotację dotyczącą wersji widoczną na wykresie](./media/app-insights-asp-net/release-annotation.png)

## <a name="video"></a>Połączenia wideo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Następne kroki
**[Praca z usługą Application Insights w programie Visual Studio](app-insights-visual-studio.md)**<br/>Zawiera informacje o debugowaniu przy użyciu telemetrii, wyszukiwaniu diagnostycznym i przechodzeniu do szczegółów kodu.

**[Praca z portalem usługi Application Insights](app-insights-dashboards.md)**<br/> Zawiera informacje o pulpitach nawigacyjnych, zaawansowanych narzędziach diagnostycznych i analitycznych, alertach, mapie zależności aplikacji na żywo oraz eksportowaniu telemetrii.

