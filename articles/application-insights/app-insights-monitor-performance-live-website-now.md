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
    ms.date="08/24/2016"
    ms.author="awills"/>


# Instrumentowanie aplikacji sieci Web w czasie wykonywania za pomocą usługi Application Insights

*Usługa Application Insights jest dostępna w wersji zapoznawczej.*

Możliwe jest instrumentowanie działającej aplikacji sieci Web za pomocą usługi Application Insights w programie Visual Studio bez konieczności modyfikowania kodu ani jego ponownego wdrażania. W aplikacjach hostowanych przez lokalny serwer IIS instalowany jest monitor stanu. Jeśli natomiast aplikacje to aplikacje sieci Web platformy Azure lub aplikacje działające w ramach maszyny wirtualnej platformy Azure, można zainstalować rozszerzenie Application Insights. Istnieją także osobne artykuły na temat instrumentacji [działających aplikacji sieci Web w technologii J2EE](app-insights-java-live.md) i [usług Azure Cloud Services](app-insights-cloudservices.md).

![przykładowe wykresy](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

Dostępne są trzy sposoby stosowania usługi Application Insights do aplikacji sieci Web platformy .NET:

* **W czasie kompilacji:** [dodaj zestaw Application Insights SDK][greenbrown] do kodu aplikacji sieci Web. 
* **W czasie wykonywania:** przeprowadź instrumentację aplikacji sieci Web na serwerze, jak opisano poniżej, bez konieczności ponownego kompilowania lub wdrażania kodu.
* **W czasie kompilacji i w czasie wykonywania:** skompiluj zestaw SDK w kodzie aplikacji sieci Web, a także zastosuj rozszerzenia czasu wykonywania. Połącz korzyści z obu opcji. 

Poniżej przedstawiono podsumowanie tego, co można uzyskać, korzystając z danej opcji:

||W czasie kompilacji|W czasie wykonywania|
|---|---|---|
|Żądania i wyjątki|Tak|Tak|
|[Bardziej szczegółowe wyjątki](app-insights-asp-net-exceptions.md)||Tak|
|[Diagnostyka zależności](app-insights-asp-net-dependencies.md)|Na platformie .NET 4.6+|Tak|
|[Liczniki wydajności sytemu](app-insights-web-monitor-performance.md#system-performance-counters)||Usługa IIS lub Usługa w chmurze platformy Azure, nie aplikacja sieci Web platformy Azure|
|[Interfejs API dla telemetrii niestandardowej][api]|Tak||
|[Integracja dziennika śledzenia](app-insights-asp-net-trace-logs.md)|Tak||
|[Widok strony i dane użytkownika](app-insights-javascript.md)|Tak||
|Nie ma potrzeby ponownej kompilacji kodu|Nie||




## Instrumentacja aplikacji sieci Web w czasie wykonywania

Potrzebna jest subskrypcja platformy [Microsoft Azure](http://azure.com).

### Jeśli aplikacja jest hostowana na Twoim serwerze IIS

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

1. W witrynie [Azure Portal](https://portal.azure.com) utwórz zasób usługi Application Insights z typem platformy ASP.NET. Jest to miejsce, w którym telemetria Twojej aplikacji będzie przechowywana, analizowana i wyświetlana.

    ![Dodawanie, usługa Application Insights. Wybierz typ platformy ASP.NET.](./media/app-insights-monitor-performance-live-website-now/01-new.png)
     
2. Otwórz teraz blok kontroli aplikacji sieci Web platformy Azure, otwórz pozycję **Narzędzia > Monitorowanie wydajności**, a następnie dodaj rozszerzenie Application Insights.

    ![W aplikacji sieci Web wybierz pozycję Narzędzia, Rozszerzenia, Dodaj, Application Insights](./media/app-insights-monitor-performance-live-website-now/05-extend.png)

    Wybierz właśnie utworzony zasób usługi Application Insights.


### Jeśli jest to projekt usług w chmurze platformy Azure

[Dodaj skrypty do ról sieci Web i procesu roboczego](app-insights-cloudservices.md).


## Wyświetlanie telemetrii wydajności

Zaloguj się w [portalu Azure](https://portal.azure.com), przejdź do usługi Application Insights i otwórz utworzony zasób.

![Wybierz kolejno opcje Przeglądaj, Application Insights, a następnie wybierz aplikację](./media/app-insights-monitor-performance-live-website-now/appinsights-08openApp.png)

Otwórz blok Wydajność, aby zobaczyć żądanie, czas odpowiedzi, zależności i inne dane.

![Wydajność](./media/app-insights-monitor-performance-live-website-now/21-perf.png)

Kliknij dowolny wykres, aby otworzyć widok z bardziej szczegółowymi informacjami.

W przypadku wykresów możliwa jest ich [edycja, zmienianie kolejności i zapisywanie](app-insights-metrics-explorer.md). Można również przypinać wykresy lub cały blok do [pulpitu nawigacyjnego](app-insights-dashboards.md).

## Zależności

Wykres Czas trwania zależności pokazuje czas trwania wywołań z aplikacji do składników zewnętrznych, takich jak bazy danych, interfejsy API REST lub magazyn obiektów blob platformy Azure.

Aby podzielić wykres na segmenty według wywołań różnych składników zależnych: edytuj wykres, włącz grupowanie, a następnie grupuj według pozycji Zależność, Typ zależności lub Zależność — wydajność.

![Zależność](./media/app-insights-monitor-performance-live-website-now/23-dep.png)

## Liczniki wydajności 

(Nie dotyczy aplikacji sieci Web platformy Azure). Kliknij opcję Serwery w bloku przeglądu, aby zobaczyć wykresy liczników wydajności serwera, takich jak zajętość procesora CPU i użycie pamięci.

Jeśli masz kilka wystąpień serwera, możesz edytować wykresy w celu grupowania według wystąpienia roli.

![Serwery](./media/app-insights-monitor-performance-live-website-now/22-servers.png)

Możesz również [zmienić zestaw liczników wydajności, które są zgłaszane przez zestaw SDK](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3). 


## Wyjątki

![Klikaj elementy wykresu wyjątków serwera](./media/app-insights-monitor-performance-live-website-now/appinsights-039-1exceptions.png)

Możesz przejść do określonych wyjątków (z ostatnich siedmiu dni) i odczytać ślady stosu i dane kontekstowe.

## Próbkowanie

Jeśli aplikacja wysyła dużo danych, a używasz zestawu SDK usługi Application Insights dla technologii ASP.NET w wersji 2.0.0-beta3 lub nowszej, może działać funkcja adaptacyjnego próbkowania, powodując wysyłanie tylko ułamka telemetrii. [Dowiedz się więcej na temat próbkowania.](app-insights-sampling.md)


## Rozwiązywanie problemów

### Błędy połączenia

Aby umożliwić działanie Monitora stanu, na zaporze serwera należy otworzyć [niektóre porty wychodzące](app-insights-ip-addresses.md#outgoing-ports).

### Brak telemetrii?

  * Użyj swojej witryny, aby wygenerować trochę danych.
  * Odczekaj kilka minut, aby dane zostały dostarczone, a następnie kliknij przycisk **Odśwież**.
  * Otwórz blok Wyszukiwanie diagnostyczne (kafelek Wyszukaj), aby wyświetlić poszczególne zdarzenia. Zdarzenia są często widoczne w bloku Wyszukiwanie diagnostyczne, zanim zagregowane dane zostaną wyświetlone na wykresach.
  * Otwórz monitor stanu i wybierz swoją aplikację w lewym okienku. Sprawdź, czy w sekcji „Powiadomienia konfiguracyjne” występują komunikaty diagnostyczne dotyczące tej aplikacji:

  ![Otwórz blok Wydajność, aby zobaczyć żądanie, czas odpowiedzi, zależności i inne dane](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)

  * Upewnij się, że zapora serwera umożliwia ruch wychodzący na portach wymienionych powyżej.
  * Jeśli na serwerze zostanie wyświetlony komunikat o „niewystarczających uprawnieniach”, spróbuj wykonać następujące kroki:
    * W Menedżerze usług IIS wybierz pulę aplikacji, otwórz **Ustawienia zaawansowane** i zapamiętaj tożsamość w obszarze **Model procesu**.
    * W panelu sterowania Zarządzanie komputerem dodaj tę tożsamość do grupy Użytkownicy monitora wydajności.
  * Jeśli na serwerze jest zainstalowany agent MMA/SCOM, niektóre wersje mogą powodować konflikt. Odinstaluj oprogramowanie SCOM i monitor stanu, a następnie ponownie zainstaluj najnowsze wersje.
  * Zobacz [Rozwiązywanie problemów][pytania i odpowiedzi].

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

Najpierw zaimportuj moduł Application Insights:

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

Dowiedz się, które aplikacje są monitorowane:

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
[pytania i odpowiedzi]: app-insights-troubleshoot-faq.md
[role]: app-insights-resources-roles-access-control.md
[użycie]: app-insights-web-track-usage.md



<!--HONumber=sep16_HO1-->


