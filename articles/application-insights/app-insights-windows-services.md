---
title: Usługa Application Insights dla usług systemu Windows i ról procesów roboczych | Microsoft Docs
description: Ręczne dodawanie zestawu SDK usługi Application Insights do aplikacji platformy ASP.NET w celu dokonania analizy użycia, dostępności i wydajności.
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: douge

ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/30/2016
ms.author: awills

---
# Ręczne konfigurowanie aplikacji Application Insights dla aplikacji platformy ASP.NET 4
*Usługa Application Insights jest dostępna w wersji zapoznawczej.*

[!INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]

Usługę [Visual Studio Application Insights](app-insights-overview.md) można ręcznie skonfigurować tak, aby monitorowała usługi systemu Windows, role procesów roboczych i inne aplikacje platformy ASP.NET. W przypadku aplikacji sieci Web konfiguracja ręczna stanowi alternatywę dla [automatycznej konfiguracji](app-insights-asp-net.md) oferowanej przez program Visual Studio.

Usługa Application Insights pomaga diagnozować problemy oraz monitorować wydajność i użycie działającej aplikacji.

![Przykładowe wykresy monitorowania wydajności](./media/app-insights-windows-services/10-perf.png)

#### Przed rozpoczęciem
Potrzebne elementy:

* Subskrypcja platformy [Microsoft Azure](http://azure.com). Jeśli Twój zespół lub organizacja mają subskrypcję platformy Azure, właściciel subskrypcji może Cię do niej dodać, korzystając z Twojego [konta Microsoft](http://live.com).
* Program Visual Studio 2013 lub nowszy.

## <a name="add"></a>1. Tworzenie zasobu usługi Application Insights
Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/) i utwórz nowy zasób usługi Application Insights. Wybierz ASP.NET jako typ aplikacji.

![Kliknij kolejno polecenia Nowy, Application Insights](./media/app-insights-windows-services/01-new-asp.png)

[Zasób](app-insights-resources-roles-access-control.md) na platformie Azure to wystąpienie usługi. Ten zasób jest miejscem, gdzie odbywa się analizowane i wyświetlanie telemetrii z aplikacji.

Wybór typu aplikacji powoduje ustawienie domyślnej zawartości bloków zasobów i właściwości widocznych w [Eksploratorze metryk](app-insights-metrics-explorer.md).

#### Kopiowanie klucza instrumentacji
Klucz identyfikuje zasób. Wkrótce zainstalujesz go w zestawie SDK w celu kierowania danych do tego zasobu.

![Kliknij opcję Właściwości, zaznacz klucz i naciśnij klawisze Ctrl+C](./media/app-insights-windows-services/02-props-asp.png)

Wykonane przed chwilą kroki służące do utworzenia nowego zasobu są dobrym sposobem na rozpoczęcie monitorowania dowolnej aplikacji. Teraz możesz wysyłać dane do tego zasobu.

## <a name="sdk"></a>2. Instalowanie zestawu SDK w aplikacji
Instalowanie i konfigurowanie zestawu SDK usługi Application Insights różni się w zależności od platformy, na której pracujesz. W przypadku aplikacji ASP.NET jest to łatwe.

1. W programie Visual Studio edytuj pakiety NuGet projektu aplikacji sieci Web.
   
    ![Kliknij projekt prawym przyciskiem myszy i wybierz polecenie Zarządzaj pakietami NuGet](./media/app-insights-windows-services/03-nuget.png)
2. Zainstaluj zestaw SDK usługi Application Insights dla aplikacji sieci Web.
   
    ![Wyszukaj „Application Insights”](./media/app-insights-windows-services/04-ai-nuget.png)
   
    *Czy można użyć innych pakietów?*
   
    Tak. Wybierz podstawowy interfejs API (Microsoft.ApplicationInsights), jeśli chcesz użyć interfejsu API do wysyłania własnej telemetrii. Pakiet systemu Windows Server automatycznie dołącza podstawowy interfejs API oraz kilka innych pakietów, takich jak zbieranie danych licznika wydajności i monitorowanie zależności. 

#### Uaktualnianie do przyszłych wersji zestawu SDK
Od czasu do czasu wydajemy nową wersję zestawu SDK.

Aby uaktualnić do [nowej wersji zestawu SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/), otwórz ponownie menedżera pakietów NuGet i przefiltruj zainstalowane pakiety. Wybierz **Microsoft.ApplicationInsights.Web** i kliknij pozycję **Uaktualnij**.

Jeśli plik ApplicationInsights.config został dostosowany, zapisz jego kopię przed uaktualnieniem, a następnie scal zmiany w nowej wersji.

## 3. Wysyłanie danych telemetrycznych
**Jeśli zainstalowano tylko pakiet podstawowego interfejsu API:**

* Ustaw klucz instrumentacji w kodzie, na przykład w funkcji `main()`: 
  
    `TelemetryConfiguration.Active.InstrumentationKey = "` *twój klucz* `";` 
* [Zapisz własne dane telemetryczne przy użyciu interfejsu API](app-insights-api-custom-events-metrics.md#ikey).

**Jeśli zainstalowano inne pakiety usługi Application Insights**, można użyć pliku konfiguracji w celu ustawienia klucza instrumentacji:

* Edytuj plik ApplicationInsights.config (który został dodany w ramach instalacji NuGet). Wstaw poniższy kod tuż przed tagiem zamykającym:
  
    `<InstrumentationKey>` *skopiowany klucz instrumentacji* `</InstrumentationKey>`
* Upewnij się, że właściwości pliku ApplicationInsights.config w Eksploratorze rozwiązań są ustawione na **Akcja kompilacji = Zawartość, Kopiuj do katalogu wyjściowego = Kopiuj**.

## <a name="run"></a> Uruchamianie projektu
Użyj klawisza **F5** do uruchomienia aplikacji i wypróbuj jej działanie: otwórz różne strony, aby wygenerować dane telemetryczne.

W programie Visual Studio zobaczysz liczbę zdarzeń, które zostały wysłane.

![Liczba zdarzeń w programie Visual Studio](./media/app-insights-windows-services/appinsights-09eventcount.png)

## <a name="monitor"></a> Wyświetlanie telemetrii
Wróć do [portalu Azure](https://portal.azure.com/) i przejdź do zasobu usługi Application Insights.

Poszukaj danych na wykresach w bloku Przegląd. Na początku zobaczysz tylko jeden lub dwa punkty. Na przykład:

![Klikaj elementy, aby uzyskać więcej danych](./media/app-insights-windows-services/12-first-perf.png)

Klikaj elementy wykresów, aby wyświetlać bardziej szczegółowe metryki. [Dowiedz się więcej o metrykach.](app-insights-web-monitor-performance.md)

#### Brak danych?
* Otwieraj różne strony w aplikacji, aby wygenerować dane telemetryczne.
* Otwórz kafelek [Wyszukaj](app-insights-diagnostic-search.md), aby wyświetlić poszczególne zdarzenia. Niekiedy potrzeba nieco więcej czasu, zanim zdarzenia dotrą przez potok metryk.
* Odczekaj kilka sekund, a następnie kliknij przycisk **Odśwież**. Wykresy są odświeżane okresowo, ale można odświeżyć je ręcznie, jeśli oczekiwane jest pojawienie się pewnych danych.
* Zobacz [Rozwiązywanie problemów](app-insights-troubleshoot-faq.md).

## Publikowanie aplikacji
Przeprowadź teraz wdrożenie aplikacji na swoim serwerze lub na platformie Azure i obserwuj gromadzenie się danych.

![Użycie programu Visual Studio do publikowania aplikacji](./media/app-insights-windows-services/15-publish.png)

Po uruchomieniu w trybie debugowania telemetria jest przyspieszana w potoku, dzięki czemu dane powinny być widoczne w ciągu kilku sekund. Po wdrożeniu aplikacji w konfiguracji wydania dane są gromadzone wolniej.

#### Brak danych po opublikowaniu na serwerze?
Otwórz następujące porty dla ruchu wychodzącego w zaporze serwera:

* `dc.services.visualstudio.com:443`
* `f5.services.visualstudio.com:443`

#### Problem z serwerem kompilacji?
Zobacz [ten punkt rozwiązywania problemów](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

> [!NOTE]
> Jeśli aplikacja generuje wiele danych telemetrycznych (i używasz wersji 2.0.0-beta3 lub nowszej zestawu SDK platformy ASP.NET), moduł próbkowania adaptacyjnego automatycznie będzie redukować ilość danych, które są wysyłane do portalu, wysyłając tylko reprezentatywny ułamek zdarzeń. Jednak zdarzenia związane z tym samym żądaniem zostaną wybrane lub pominięte jako grupa, dzięki czemu możesz nawigować między powiązanymi zdarzeniami. 
> [Więcej informacji na temat próbkowania](app-insights-sampling.md).
> 
> 

## Następne kroki
* [Dodaj więcej danych telemetrycznych](app-insights-asp-net-more.md), aby uzyskać pełny obraz swojej aplikacji.

<!--HONumber=Sep16_HO3-->


