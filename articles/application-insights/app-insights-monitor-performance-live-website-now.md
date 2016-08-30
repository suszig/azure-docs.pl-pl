<properties
    pageTitle="Diagnozowanie problemów z wydajnością witryny sieci Web działającej na serwerze usług IIS | Microsoft Azure"
    description="Monitorowanie wydajności witryny sieci Web bez jej ponownego wdrażania. Uzyskiwanie telemetrii zależności za pomocą autonomicznego monitora stanu lub zestawu SDK usługi Application Insights."
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
    ms.date="03/09/2016"
    ms.author="awills"/>


# Instalowanie monitora stanu usługi Application Insights w celu monitorowania wydajności witryny sieci Web

*Usługa Application Insights jest w wersji zapoznawczej.*

Monitor stanu usługi Application Insights w programie Visual Studio pozwala diagnozować problemy z wyjątkami i wydajnością w aplikacjach ASP.NET. 

![przykładowe wykresy](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

> [AZURE.TIP] Istnieją osobne artykuły na temat instrumentacji [działających aplikacji sieci Web w technologii J2EE](app-insights-java-live.md) i [usług Azure Cloud Services](app-insights-cloudservices.md).


Dostępne są trzy sposoby zastosowania usługi Application Insights do aplikacji sieci Web usług IIS:

* **W czasie kompilacji:** [dodaj zestaw Application Insights SDK][greenbrown] do kodu aplikacji sieci Web. Zapewnia to:
 * Zakres standardowej telemetrii diagnostycznej i użycia.
 * [Interfejs API usługi Application Insights][api] pozwala programować własną telemetrię, aby śledzić szczegółowe użycie lub diagnozować problemy.
* **W czasie wykonywania:** użyj Monitora stanu do instrumentacji aplikacji sieci Web na serwerze.
 * Monitorowanie aplikacji sieci Web, które są już uruchomione: nie ma potrzeby ponownej kompilacji ani ponownego publikowania.
 * Zakres standardowej telemetrii diagnostycznej i użycia.
 * Diagnostyka zależności &#151; lokalizowanie występowania błędów lub słabej wydajności w przypadku używania przez aplikację innych składników, takich jak bazy danych, interfejsy API REST lub inne usługi.
 * Rozwiązywanie problemów z telemetrią.
* **Połączenie obu sposobów:** wkompiluj zestaw SDK do kodu aplikacji sieci Web, a następnie uruchom monitor stanu na serwerze sieci Web.  Najlepsze aspekty obu rozwiązań:
 * Standardowa telemetria diagnostyczna i użycia.
 * Diagnostyka zależności.
 * Interfejs API umożliwiający zapisywanie niestandardowych danych telemetrycznych.
 * Rozwiązywanie dowolnych problemów za pomocą zestawu SDK i telemetrii.


## Instalowanie monitora stanu usługi Application Insights

Potrzebna jest subskrypcja platformy [Microsoft Azure](http://azure.com).

### Jeśli aplikacja działa na Twoim serwerze IIS

1. Zaloguj się na serwerze sieci Web usług IIS, używając poświadczeń administratora.
2. Pobierz i uruchom [instalator monitora stanu](http://go.microsoft.com/fwlink/?LinkId=506648).
4. W kreatorze instalacji zaloguj się do platformy Microsoft Azure.

    ![Logowanie do platformy Azure przy użyciu poświadczeń konta Microsoft](./media/app-insights-monitor-performance-live-website-now/appinsights-035-signin.png)

    *Błędy podczas połączenia? Zobacz [Rozwiązywanie problemów](#troubleshooting).*

5. Wybierz zainstalowaną aplikację sieci Web lub witrynę, którą chcesz monitorować, a następnie skonfiguruj zasób, w którym chcesz oglądać wyniki w portalu usługi Application Insights.

    ![Wybór aplikacji i zasobu.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

    Zwykle należy wybrać opcję skonfigurowania nowego zasobu i [grupy zasobów][role].

    Możesz zamiast tego użyć istniejącego zasobu, jeśli masz już skonfigurowane [testy sieci Web][availability] dla danej witryny lub [monitorowanie klienta sieci Web][klient].

6. Uruchom ponownie usługi IIS.

    ![Wybór opcji Uruchom ponownie w górnej części okna dialogowego.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    Działanie usługi sieci Web zostanie na krótko przerwane.

6. Zauważ, że do aplikacji sieci Web, którą chcesz monitorować, został wstawiony plik ApplicationInsights.config.

    ![Plik .config obok plików kodu aplikacji sieci Web.](./media/app-insights-monitor-performance-live-website-now/appinsights-034-aiconfig.png)

   Pewne zmiany zostały wprowadzone również w pliku web.config.

#### Czy chcesz dokonać później (ponownej) konfiguracji?

Po ukończeniu działania kreatora możesz ponownie skonfigurować agenta w dowolnym czasie. Możesz także użyć tego sposobu, jeśli agent został zainstalowany, ale wystąpiły problemy z konfiguracją początkową.

![Kliknij ikonę Application Insights na pasku zadań](./media/app-insights-monitor-performance-live-website-now/appinsights-033-aicRunning.png)


### Jeśli aplikacja działa jako aplikacja sieci Web platformy Azure

W panelu sterowania aplikacji sieci Web platformy Azure dodaj rozszerzenie Application Insights.

![W aplikacji sieci Web wybierz kolejno opcje Ustawienia, Rozszerzenia, Dodaj, Application Insights](./media/app-insights-monitor-performance-live-website-now/05-extend.png)


### Jeśli jest to projekt usług w chmurze platformy Azure

[Dodaj skrypty do ról sieci Web i procesu roboczego](app-insights-cloudservices.md).


## Wyświetlanie telemetrii wydajności

Zaloguj się w [portalu Azure](https://portal.azure.com), przejdź do usługi Application Insights i otwórz utworzony zasób.

![Wybierz kolejno opcje Przeglądaj, Application Insights, a następnie wybierz aplikację](./media/app-insights-monitor-performance-live-website-now/appinsights-08openApp.png)

Otwórz blok Wydajność, aby zobaczyć żądanie, czas odpowiedzi, zależności i inne dane.

![Wydajność](./media/app-insights-monitor-performance-live-website-now/21-perf.png)

Kliknij, aby dopasować szczegóły wyświetlania lub dodać nowy wykres.


![](./media/app-insights-monitor-performance-live-website-now/appinsights-038-dependencies.png)

## Zależności

Wykres Czas trwania zależności pokazuje czas trwania wywołań z aplikacji do składników zewnętrznych, takich jak bazy danych, interfejsy API REST lub magazyn obiektów blob platformy Azure.

Aby podzielić wykres na segmenty według wywołań różnych składników zależnych, wybierz wykres, włącz Grupowanie, a następnie wybierz opcje Zależność, Typ zależności lub Zależność — wydajność.

Możesz również filtrować wykres, aby przyjrzeć się określonej zależności, typowi lub zasobnikowi wydajności. Kliknij opcję Filtry.

## Liczniki wydajności

(Nie dotyczy aplikacji sieci Web platformy Azure). Kliknij opcję Serwery w bloku przeglądu, aby zobaczyć wykresy liczników wydajności serwera, takich jak zajętość procesora CPU i użycie pamięci.

Dodaj nowy wykres lub kliknij dowolny wykres, aby zmienić jego zawartość. 

Możesz również [zmienić zestaw liczników wydajności, które są zgłaszane przez zestaw SDK](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3). 

## Wyjątki

![Klikaj elementy wykresu wyjątków serwera](./media/app-insights-monitor-performance-live-website-now/appinsights-039-1exceptions.png)

Możesz przejść do określonych wyjątków (z ostatnich siedmiu dni) i odczytać ślady stosu i dane kontekstowe.

## Próbkowanie

Jeśli aplikacja wysyła dużo danych, a używasz zestawu SDK usługi Application Insights dla technologii ASP.NET w wersji 2.0.0-beta3 lub nowszej, może działać funkcja adaptacyjnego próbkowania, powodując wysyłanie tylko ułamka telemetrii. [Dowiedz się więcej na temat próbkowania.](app-insights-sampling.md)


## Rozwiązywanie problemów

### Błędy połączenia

Aby umożliwić działanie Monitora stanu, na zaporze serwera należy otworzyć niektóre porty wychodzące:

+ Telemetria — potrzebne cały czas:
 +  `dc.services.visualstudio.com:80`
 +  `dc.services.visualstudio.com:443`
 +  `dc.applicationinsights.microsoft.com`
+ Konfiguracja — wymagane tylko podczas wprowadzania zmian:
 -  `management.core.windows.net:443`
 -  `management.azure.com:443`
 -  `login.windows.net:443`
 -  `login.microsoftonline.com:443`
 -  `secure.aadcdn.microsoftonline-p.com:443`
 -  `auth.gfx.ms:443`
 -  `login.live.com:443`
+ Instalacja:
 +  `packages.nuget.org:443`

Ta lista może zmieniać się od czasu do czasu.

### Brak telemetrii?

  * Użyj swojej witryny, aby wygenerować trochę danych.
  * Odczekaj kilka minut, aby dane zostały dostarczone, a następnie kliknij przycisk **Odśwież**.
  * Otwórz blok Wyszukiwanie diagnostyczne (kafelek Wyszukaj), aby wyświetlić poszczególne zdarzenia. Zdarzenia są często widoczne w bloku Wyszukiwanie diagnostyczne, zanim zagregowane dane zostaną wyświetlone na wykresach.
  * Otwórz monitor stanu i wybierz swoją aplikację w lewym okienku. Sprawdź, czy w sekcji „Powiadomienia konfiguracyjne” występują komunikaty diagnostyczne dotyczące tej aplikacji:

  ![](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)

  * Upewnij się, że zapora serwera umożliwia ruch wychodzący na portach wymienionych powyżej.
  * Jeśli na serwerze zostanie wyświetlony komunikat o „niewystarczających uprawnieniach”, spróbuj wykonać następujące kroki:
    * W Menedżerze usług IIS wybierz pulę aplikacji, otwórz **Ustawienia zaawansowane** i zapamiętaj tożsamość w obszarze **Model procesu**.
    * W panelu sterowania Zarządzanie komputerem dodaj tę tożsamość do grupy Użytkownicy monitora wydajności.
  * Jeśli na serwerze jest zainstalowany agent MMA/SCOM, niektóre wersje mogą powodować konflikt. Odinstaluj oprogramowanie SCOM i monitor stanu, a następnie ponownie zainstaluj najnowsze wersje.
  * Zobacz [Rozwiązywanie problemów][qna].

## Wymagania systemu

Serwerowe systemy operacyjne obsługiwane przez monitor stanu usługi Application Insights:

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2

z najnowszym dodatkiem SP oraz oprogramowaniem .NET Framework 4.0 i 4.5

Po stronie klienta systemy Windows 7, 8 i 8.1, również z oprogramowaniem .NET Framework 4.0 i 4.5

Obsługiwane wersje usług IIS: 7, 7.5, 8, 8.5 (usługi IIS są wymagane)

## Automatyzacja przy użyciu programu PowerShell

Monitorowanie można uruchomić i zatrzymać przy użyciu programu PowerShell.

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* `-Name` Nazwa aplikacji sieci Web (opcjonalnie).
* Wyświetla stan monitorowania usługi Application Insights dla każdej (lub wskazanej z nazwy) aplikacji sieci Web na tym serwerze usług IIS.

* Zwraca obiekt `ApplicationInsightsApplication` dla każdej aplikacji:
 * `SdkState==EnabledAfterDeployment`: aplikacja jest monitorowana, a instrumentacja została dokonana w czasie wykonywania za pomocą narzędzia Monitor stanu lub polecenia `Start-ApplicationInsightsMonitoring`.
 * `SdkState==Disabled`: nie ma instrumentacji aplikacji dla usługi Application Insights. Instrumentacja nie została nigdy przeprowadzona albo monitorowanie czasu wykonywania zostało wyłączone za pomocą monitora stanu lub polecenia `Stop-ApplicationInsightsMonitoring`.
 * `SdkState==EnabledByCodeInstrumentation`: dokonano instrumentacji aplikacji przez dodanie zestawu SDK do kodu źródłowego. Nie można zaktualizować ani zatrzymać tego zestawu SDK.
 * `SdkVersion` wyświetla wersję używaną do monitorowania tej aplikacji.
 * `LatestAvailableSdkVersion`wyświetla wersję aktualnie dostępną w galerii NuGet. Aby uaktualnić aplikację do tej wersji, użyj polecenia `Update-ApplicationInsightsMonitoring`.

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* `-Name` Nazwa aplikacji w usługach IIS
* `-InstrumentationKey` Klucz instrumentacji zasobu usługi Application Insights, w którym mają być wyświetlane wyniki.

* To polecenie cmdlet dotyczy tylko aplikacji, dla których nie przeprowadzono jeszcze instrumentacji (czyli SdkState == NotInstrumented).

    Polecenie cmdlet nie wpływa na aplikację, dla której dokonano już instrumentacji — zarówno w czasie kompilacji przez dodanie zestawu SDK do kodu, jaki i w czasie wykonywania przez wcześniejsze użycie tego polecenia cmdlet.

    Wersja zestawu SDK użyta do instrumentacji aplikacji to wersja, która została ostatnio pobrana na ten serwer.

    Aby pobrać najnowszą wersję, użyj polecenia cmdlet Update-ApplicationInsightsVersion.

* Zwraca obiekt `ApplicationInsightsApplication` w przypadku powodzenia. W razie niepowodzenia rejestruje ślad do stderr.

    
          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* `-Name` Nazwa aplikacji w usługach IIS
* `-All` Zatrzymuje monitorowanie wszystkich aplikacji na tym serwerze usług IIS, dla których `SdkState==EnabledAfterDeployment`

* Zatrzymuje monitorowanie określonych aplikacji i usuwa instrumentację. Działa to tylko w przypadku aplikacji, które zostały zinstrumentowane w czasie wykonywania za pomocą monitora stanu lub polecenia cmdlet Start-ApplicationInsightsApplication. (`SdkState==EnabledAfterDeployment`)

* Zwraca obiekt ApplicationInsightsApplication.

`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]

* `-Name`: nazwa aplikacji sieci Web na serwerze usług IIS.
* `-InstrumentationKey` (opcjonalnie) umożliwia zmianę zasobu, do którego wysyłana jest telemetria aplikacji.
* To polecenie cmdlet:
 * Uaktualnia wskazaną aplikację do ostatniej wersji zestawu SDK pobranej na ten komputer. (Działa tylko wtedy, gdy `SdkState==EnabledAfterDeployment`)
 * Jeśli zostanie wprowadzony klucz instrumentacji, wskazana aplikacja jest konfigurowana ponownie do wysłania telemetrii do zasobu dotyczącego tego klucza. (Działa, jeśli `SdkState != Disabled`)

`Update-ApplicationInsightsVersion`

* Pobiera na serwer najnowszy zestaw SDK usługi Application Insights.

## Szablon Azure

Jeśli aplikacja sieci Web działa na platformie Azure, a zasoby zostały utworzone przy użyciu szablonu usługi Azure Resource Manager, można skonfigurować usługę Application Insights, dodając następujący kod do węzła zasobów:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource` — nazwa zasobu usługi Application Insights.
* `myWebAppName` — identyfikator aplikacji sieci Web.

## <a name="next"></a>Następne kroki

* [Tworzenie testów sieci Web][availability], aby upewnić się, że witryna pozostaje aktywna.
* [Wyszukiwanie zdarzeń i dzienników][diagnostyka], aby łatwiej diagnozować problemy.
* [Dodawanie telemetrii klienta sieci Web][użycie], aby zobaczyć wyjątki pochodzące z kodu strony sieci Web i umożliwić wstawianie wywołań śladu.
* [Dodawanie zestawu SDK usługi Application Insights do kodu usługi sieci Web][greenbrown], aby móc wstawić ślad i rejestrować wywołania w kodzie serwera.

## Film wideo

#### Monitorowanie wydajności

[AZURE.VIDEO app-insights-performance-monitoring]

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[klient]: app-insights-javascript.md
[diagnostyka]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[role]: app-insights-resources-roles-access-control.md
[użycie]: app-insights-web-track-usage.md



<!--HONumber=jun16_HO2-->


