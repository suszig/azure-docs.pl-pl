<properties
    pageTitle="Dodawanie zestawu SDK usługi Application Insights w celu monitorowania aplikacji ASP.NET | Microsoft Azure"
    description="Analizowanie użycia, dostępności i wydajności aplikacji lokalnej lub aplikacji sieci Web na platformie Microsoft Azure za pomocą usługi Application Insights."
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
    ms.date="02/04/2016"
    ms.author="awills"/>


# Dodawanie zestawu SDK usługi Application Insights w celu monitorowania aplikacji ASP.NET

*Usługa Application Insights jest w wersji zapoznawczej.*

[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]


Usługa Application Insights w programie Visual Studio służy do monitorowania działającej aplikacji, co ułatwia [wykrywanie i diagnozowanie problemów z wydajnością oraz wyjątków][detect] oraz [uzyskiwanie informacji na temat sposobu użycia aplikacji][knowUsers]. Może być używana z wieloma różnorodnymi typami aplikacji. Obsługuje aplikacje, które są hostowane na lokalnych serwerach usług IIS lub na maszynach wirtualnych platformy Azure, jak również aplikacje sieci Web platformy Azure.



![Przykładowe wykresy monitorowania wydajności](./media/app-insights-start-monitoring-app-health-usage/10-perf.png)

*Zobacz też:*

* [ASP.NET 5](app-insights-asp-net-five.md)
* [Aplikacje urządzeń i serwery Java][platformy]

#### Przed rozpoczęciem

W przypadku wielu typów aplikacji program [Visual Studio może dodać usługę Application Insights do aplikacji](#ide) w sposób prawie niezauważalny. Jednak ponieważ czytasz ten artykuł, aby lepiej zrozumieć działanie, przeprowadzimy Cię przez te kroki w sposób ręczny.


Potrzebne elementy:

* Subskrypcja platformy [Microsoft Azure](http://azure.com). Jeśli Twój zespół lub organizacja mają subskrypcję platformy Azure, właściciel subskrypcji może Cię do niej dodać, korzystając z Twojego [konta Microsoft](http://live.com).
* Program Visual Studio 2013 lub nowszy.

## <a name="add"></a> 1. Tworzenie zasobu usługi Application Insights

Zaloguj się do [portalu Azure][portal] i utwórz nowy zasób usługi Application Insights. Wybierz ASP.NET jako typ aplikacji.

![Kliknij kolejno polecenia Nowy, Application Insights](./media/app-insights-start-monitoring-app-health-usage/01-new-asp.png)

[Zasób][role] na platformie Azure to wystąpienie usługi. Ten zasób jest miejscem, gdzie odbywa się analizowane i wyświetlanie telemetrii z aplikacji.

Wybór typu aplikacji powoduje ustawienie domyślnej zawartości bloków zasobów i właściwości widocznych w [Eksploratorze metryk][metrics].

#### Kopiowanie klucza instrumentacji

Klucz identyfikuje zasób. Wkrótce zainstalujesz go w zestawie SDK w celu kierowania danych do tego zasobu.

![Kliknij opcję Właściwości, zaznacz klucz i naciśnij Ctrl+C](./media/app-insights-start-monitoring-app-health-usage/02-props-asp.png)

Wykonane przed chwilą kroki służące do utworzenia nowego zasobu są dobrym sposobem na rozpoczęcie monitorowania dowolnej aplikacji. Teraz możesz wysyłać dane do tego zasobu.

## <a name="sdk"></a> 2. Instalowanie zestawu SDK w aplikacji

Instalowanie i konfigurowanie zestawu SDK usługi Application Insights różni się w zależności od platformy, na której pracujesz. W przypadku aplikacji ASP.NET jest łatwe.

1. W programie Visual Studio edytuj pakiety NuGet projektu aplikacji sieci Web.

    ![Kliknij prawym przyciskiem myszy projekt i wybierz polecenie Zarządzaj pakietami NuGet](./media/app-insights-start-monitoring-app-health-usage/03-nuget.png)

2. Zainstaluj zestaw SDK usługi Application Insights dla aplikacji sieci Web.

    ![Wyszukaj „Application Insights”](./media/app-insights-start-monitoring-app-health-usage/04-ai-nuget.png)

3. Edytuj plik ApplicationInsights.config (który został dodany w ramach instalacji NuGet). Wstaw ten kod tuż przed tagiem zamykającym:

    `<InstrumentationKey>` *skopiowany klucz instrumentacji* `</InstrumentationKey>`

    (Możesz też [ustawić klucz za pomocą kodu][apikey] napisanego w aplikacji).

#### Uaktualnianie do przyszłych wersji zestawu SDK

Od czasu do czasu wydajemy nową wersję zestawu SDK.

Aby uaktualnić do [nowej wersji zestawu SDK](app-insights-release-notes-dotnet.md), otwórz ponownie menedżera pakietów NuGet i przefiltruj zainstalowane pakiety. Wybierz **Microsoft.ApplicationInsights.Web** i kliknij pozycję **Uaktualnij**.

Jeśli plik ApplicationInsights.config został dostosowany, zapisz jego kopię przed uaktualnieniem, a następnie scal zmiany w nowej wersji.


## <a name="run"></a> 3. Uruchamianie projektu

Użyj klawisza **F5** do uruchomienia aplikacji i wypróbuj jej działanie: otwórz różne strony, aby wygenerować dane telemetryczne.

W programie Visual Studio zobaczysz liczbę zdarzeń, które zostały wysłane.

![](./media/app-insights-start-monitoring-app-health-usage/appinsights-09eventcount.png)

## <a name="monitor"></a> 4. Wyświetlanie telemetrii

Wróć do [portalu Azure][portal] i przejdź do zasobu usługi Application Insights.


Poszukaj danych na wykresach w bloku Przegląd. Na początku zobaczysz tylko jeden lub dwa punkty. Na przykład:

![Klikaj elementy, aby uzyskać więcej danych](./media/app-insights-start-monitoring-app-health-usage/12-first-perf.png)

Klikaj elementy wykresów, aby wyświetlać bardziej szczegółowe metryki. [Dowiedz się więcej o metrykach.][wydajność]

#### Brak danych?

* Otwieraj różne strony w aplikacji, aby wygenerować dane telemetryczne.
* Otwórz kafelek [Wyszukaj][diagnostyka], aby wyświetlić poszczególne zdarzenia. Niekiedy potrzeba nieco więcej czasu, zanim zdarzenia dotrą przez potok metryk.
* Odczekaj kilka sekund, a następnie kliknij przycisk **Odśwież**. Wykresy są odświeżane okresowo, ale można odświeżyć je ręcznie, jeśli oczekiwane jest pojawienie się pewnych danych.
* Zobacz [Rozwiązywanie problemów][qna].

## Publikowanie aplikacji

Przeprowadź teraz wdrożenie aplikacji w usługach IIS lub na platformie Azure. Rozpocznie się gromadzenie danych.

![Użycie programu Visual Studio do publikowania aplikacji](./media/app-insights-start-monitoring-app-health-usage/15-publish.png)

Po uruchomieniu w trybie debugowania telemetria jest przyspieszana za pośrednictwem potoku, dzięki czemu dane powinny być widoczne w ciągu kilku sekund. Dane są gromadzone wolniej po wdrożeniu aplikacji w konfiguracji wydania.

#### Brak danych po opublikowaniu na serwerze?

Otwórz następujące porty dla ruchu wychodzącego w zaporze serwera:

+ `dc.services.visualstudio.com:443`
+ `f5.services.visualstudio.com:443`


#### Problem z serwerem kompilacji?

Zobacz [ten punkt rozwiązywania problemów](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

> [AZURE.NOTE] Jeśli aplikacja generuje wiele danych telemetrycznych (i używasz wersji 2.0.0-beta3 lub nowszej zestawu SDK platformy ASP.NET), moduł próbkowania adaptacyjnego będzie automatycznie zmniejszać ilość danych wysyłanych do portalu, przesyłając tylko reprezentatywny ułamek zdarzeń. Jednak zdarzenia, które są związane z tym samym żądaniem, zostaną wybrane lub pominięte jako grupa, dzięki czemu można nawigować między powiązanymi zdarzeniami. 
> [Więcej informacji na temat próbkowania](app-insights-sampling.md).


## 5. Dodawanie śledzenia zależności (i liczników wydajności usług IIS)

Zestaw SDK wymaga niewielkiej pomocy, aby uzyskać dostęp do niektórych danych. Ten dodatkowy krok należy wykonać zwłaszcza w celu automatycznego pomiaru wywołań między aplikacją a bazami danych, interfejsami API REST i innymi składnikami zewnętrznymi. Te metryki zależności mogą być nieocenione, ponieważ ułatwiają diagnozowanie problemów z wydajnością.

Jeśli pracujesz na własnym serwerze IIS, ten krok umożliwia również pokazywanie liczników wydajności systemu w [eksploratorze metryk](app-insights-metrics-explorer.md).

#### Jeśli aplikacja działa na Twoim serwerze IIS

Zaloguj się do serwera przy użyciu uprawnień administratora, a następnie zainstaluj [monitor stanu usługi Application Insights](http://go.microsoft.com/fwlink/?LinkId=506648).

Konieczne może być [otwarcie dodatkowych portów wychodzących w zaporze](app-insights-monitor-performance-live-website-now.md#troubleshooting).

Ten krok umożliwia również [raportowanie liczników wydajności](app-insights-web-monitor-performance.md#system-performance-counters) np. procesora CPU, pamięci, zajętości sieci.

#### Jeśli aplikacja jest aplikacją sieci Web platformy Azure

W panelu sterowania aplikacji sieci Web platformy Azure dodaj rozszerzenie Application Insights.

![W aplikacji sieci Web wybierz kolejno opcje Ustawienia, Rozszerzenia, Dodaj, Application Insights](./media/app-insights-start-monitoring-app-health-usage/05-extend.png)


#### Jeśli jest to projekt usług w chmurze platformy Azure

[Dodaj skrypty do ról sieci Web i procesu roboczego](app-insights-cloudservices.md).



## 6. Dodawanie monitorowania po stronie klienta

Właśnie udało Ci się przeprowadzić instalację zestawu SDK, który wysyła dane telemetryczne z serwera (zaplecza) aplikacji. Teraz możesz dodać monitorowanie po stronie klienta. Dostarcza ono dane dotyczące użytkowników, sesji, wyświetleń stron i ewentualnych wyjątków lub awarii występujących w przeglądarce. Możesz także napisać własny kod, aby śledzić, jak użytkownicy korzystają z aplikacji, schodząc do szczegółowego poziomu kliknięć i naciśnięć klawiszy.


Dodaj fragment kodu JavaScript na każdej stronie. Pobierz kod ze swojego zasobu Application Insights:

![W aplikacji sieci Web otwórz Szybki start, a następnie kliknij polecenie „Pobierz kod umożliwiający monitorowanie stron sieci Web”.](./media/app-insights-start-monitoring-app-health-usage/02-monitor-web-page.png)

Zauważ, że kod zawiera klucz instrumentacji, który identyfikuje dany zasób aplikacji.

[Dowiedz się więcej na temat śledzenia stron sieci Web.](app-insights-web-track-usage.md)


## Śledzenie wersji aplikacji

Upewnij się, że plik `buildinfo.config` jest generowany przez proces MSBuild. W pliku .csproj dodaj ten kod:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup> 
```

Jeśli plik zawiera informację o kompilacji, moduł sieci Web usługi Application Insights automatycznie dodaje **wersję aplikacji** jako właściwość do każdego elementu telemetrii. Pozwala to na filtrowanie według wersji podczas przeprowadzania [wyszukiwania diagnostycznego][diagnostyka] lub [eksplorowania metryk][metrics]. 

Numer wersji kompilacji jest jednak generowany tylko przez program MS Build, a nie podczas kompilowania przez deweloperów w programie Visual Studio.

## 7. Kończenie instalacji

Aby uzyskać pełny wgląd w dane aplikacji, można zrobić jeszcze kilka rzeczy:

* [Konfigurowanie testów sieci Web][availability], aby upewnić się, że aplikacja stale działa i odpowiada.
* [Przechwytywanie danych śledzenia dziennika][netlogs] z preferowanej struktury rejestrowania
* [Śledzenie niestandardowych zdarzeń i metryk][api] po stronie klienta, serwera lub po obu stronach, aby dowiedzieć się więcej o sposobie korzystania z aplikacji.

## <a name="ide"></a> Procedura zautomatyzowana

Na początku tego artykułu napisaliśmy, że pokażemy ręczny sposób tworzenia zasobu usługi Application Insights, a następnie instalacji zestawu SDK. Uważamy, że warto rozumieć obie części tej procedury. Jednak w przypadku aplikacji ASP.NET (i wielu innych) jest jeszcze szybszy, zautomatyzowany sposób.

Potrzebne będą: program [Visual Studio](http://go.microsoft.com/fwlink/?linkid=397827&clcid=0x409) (2013 Update 3 lub nowszy) oraz konto w na platformie [Microsoft Azure](http://azure.com).

#### Jeśli to nowy projekt...

Podczas tworzenia nowego projektu w programie Visual Studio upewnij się, że jest zaznaczona opcja **Dodaj usługę Application Insights**.


![Tworzenie projektu ASP.NET](./media/app-insights-start-monitoring-app-health-usage/appinsights-01-vsnewp1.png)

Program Visual Studio tworzy zasób w usłudze Application Insights, dodaje zestaw SDK do projektu, a następnie umieszcza klucz w pliku `.config`.

Jeśli projekt zawiera strony sieci Web, program dodaje również [Zestaw SDK JavaScript][klient] do wzorcowej strony sieci Web.

#### A jeśli to istniejący projekt...

Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, a następnie wybierz polecenie **Dodaj usługę Application Insights**.

![Wybieranie opcji Dodaj usługę Application Insights](./media/app-insights-start-monitoring-app-health-usage/appinsights-03-addExisting.png)

Program Visual Studio tworzy zasób w usłudze Application Insights, dodaje zestaw SDK do projektu, a następnie umieszcza klucz w pliku `.config`.

W takim przypadku [Zestaw SDK JavaScript][klient] nie jest dodawany do stron sieci Web — zalecamy, aby zrobić to w następnym kroku.

#### Opcje konfiguracji

Jeśli robisz to po raz pierwszy, pojawi się monit o zalogowanie lub zarejestrowanie się na platformie Microsoft Azure w wersji zapoznawczej. 

Jeśli ta aplikacja jest częścią większej aplikacji, warto użyć **Konfigurowania ustawień**, aby umieścić ją w tej samej grupie zasobów co inne składniki.

*Nie ma opcji Application Insights? Sprawdź, czy używasz programu Visual Studio w wersji 2013 Update 3 lub nowszej, a narzędzia Application Insights Tools są włączone w obszarze Rozszerzenia i aktualizacje.*

#### Otwieranie usługi Application Insights z poziomu projektu

![Kliknij prawym przyciskiem myszy projekt i otwórz portal Azure](./media/app-insights-start-monitoring-app-health-usage/appinsights-04-openPortal.png)




## <a name="video"></a>Film wideo

> [AZURE.VIDEO getting-started-with-application-insights]


<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apikey]: app-insights-api-custom-events-metrics.md#ikey
[availability]: app-insights-monitor-web-app-availability.md
[Azure]: ../insights-perf-analytics.md
[klient]: app-insights-javascript.md
[detect]: app-insights-detect-triage-diagnose.md
[diagnostyka]: app-insights-diagnostic-search.md
[knowUsers]: app-insights-overview-usage.md
[metrics]: app-insights-metrics-explorer.md
[netlogs]: app-insights-asp-net-trace-logs.md
[wydajność]: app-insights-web-monitor-performance.md
[platformy]: app-insights-platforms.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[role]: app-insights-resources-roles-access-control.md
[rozpoczynanie]: app-insights-overview.md



<!--HONumber=jun16_HO2-->


