<properties
    pageTitle="Dodawanie zestawu SDK usługi Application Insights do monitorowania aplikacji ASP.NET | Microsoft Azure"
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
    ms.date="05/19/2016"
    ms.author="awills"/>


# Dodawanie zestawu SDK usługi Application Insights do monitorowania aplikacji ASP.NET

*Usługa Application Insights jest dostępna w wersji zapoznawczej.*

[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]


Usługa Application Insights w programie Visual Studio służy do monitorowania działającej aplikacji, co ułatwia [wykrywanie i diagnozowanie problemów z wydajnością oraz wyjątków][detect] oraz [uzyskiwanie informacji na temat sposobu użycia aplikacji][knowUsers]. Może być używana z wieloma różnorodnymi typami aplikacji. Obsługuje aplikacje, które są hostowane na lokalnych serwerach usług IIS lub na maszynach wirtualnych platformy Azure, jak również aplikacje sieci Web platformy Azure.



![Przykładowe wykresy monitorowania wydajności](./media/app-insights-asp-net-manual/10-perf.png)

*Zobacz też:*

* [ASP.NET 5](app-insights-asp-net-five.md)
* [Aplikacje urządzeń i serwery Java][platformy]

#### Przed rozpoczęciem

W przypadku wielu typów aplikacji program [Visual Studio może dodać usługę Application Insights do aplikacji](#ide) w sposób prawie niezauważalny. Jednak ponieważ czytasz ten artykuł, aby lepiej zrozumieć działanie, przeprowadzimy Cię przez te kroki w sposób ręczny.


Potrzebne są:

* Subskrypcja platformy [Microsoft Azure](http://azure.com). Jeśli Twój zespół lub organizacja mają subskrypcję platformy Azure, właściciel subskrypcji może Cię do niej dodać, korzystając z Twojego [konta Microsoft](http://live.com).
* Program Visual Studio w wersji 2013 lub nowszej.

## <a name="add"></a>Tworzenie zasobu usługi Application Insights

Zaloguj się do [portalu Azure][portal] i utwórz nowy zasób Application Insights. Wybierz ASP.NET jako typ aplikacji.

![Kliknij kolejno polecenia Nowy, Application Insights](./media/app-insights-asp-net-manual/01-new-asp.png)

[Zasób][role] w systemie Azure jest wystąpieniem usługi. Na poziomie zasobu odbywa się analizowane i wyświetlanie telemetrii z aplikacji.

Wybór typu aplikacji powoduje ustawienie domyślnej zawartości bloków zasobów i właściwości widocznych w [Eksploratorze metryk][metrics].

#### Kopiowanie klucza instrumentacji

Klucz identyfikuje zasób. Wkrótce zainstalujesz go w zestawie SDK w celu kierowania danych do tego zasobu.

![Kliknij opcję Właściwości, zaznacz klucz i naciśnij klawisze Ctrl+C](./media/app-insights-asp-net-manual/02-props-asp.png)

Wykonane przed chwilą kroki służące do utworzenia nowego zasobu są dobrym sposobem na rozpoczęcie monitorowania dowolnej aplikacji. Teraz możesz wysyłać dane do tego zasobu.

## <a name="sdk"></a> Instalowanie zestawu SDK w aplikacji

Instalowanie i konfigurowanie zestawu SDK usługi Application Insights różni się w zależności od platformy, na której pracujesz. W przypadku aplikacji ASP.NET jest to łatwe.

1. W programie Visual Studio edytuj pakiety NuGet projektu aplikacji sieci Web.

    ![Kliknij projekt prawym przyciskiem myszy i wybierz polecenie Zarządzaj pakietami NuGet](./media/app-insights-asp-net-manual/03-nuget.png)

2. Zainstaluj zestaw SDK usługi Application Insights dla aplikacji sieci Web.

    ![Wyszukaj „Application Insights”](./media/app-insights-asp-net-manual/04-ai-nuget.png)

3. Edytuj plik ApplicationInsights.config (który został dodany w ramach instalacji NuGet). Wstaw poniższy kod tuż przed tagiem zamykającym:

    `<InstrumentationKey>` *skopiowany klucz instrumentacji* `</InstrumentationKey>`

    (Możesz też [ustawić klucz za pomocą kodu][apikey] napisanego w aplikacji).

#### Uaktualnienia do przyszłych wersji zestawu SDK

Od czasu do czasu wydajemy nową wersję zestawu SDK.

Aby uaktualnić do [nowej wersji zestawu SDK](app-insights-release-notes-dotnet.md), ponownie otwórz menedżera pakietów NuGet i filtruj zainstalowane pakiety. Wybierz pakiet **Microsoft.ApplicationInsights.Web** i wybierz polecenie **Uaktualnij**.

Jeśli plik ApplicationInsights.config został wcześniej dostosowany, zapisz jego kopię przed uaktualnieniem, a następnie scal zmiany w nowej wersji.


## <a name="run"></a> Uruchamianie projektu

Użyj klawisza **F5** do uruchomienia aplikacji i wypróbuj jej działanie: otwórz różne strony, aby wygenerować dane telemetryczne.

W programie Visual Studio zobaczysz liczbę zdarzeń, które zostały wysłane.

![](./media/app-insights-asp-net-manual/appinsights-09eventcount.png)

## <a name="monitor"></a> Wyświetlanie telemetrii

Wróć do [portalu Azure][portal] i przeglądaj, aby znaleźć odpowiedni zasób usługi Application Insights.


Poszukaj danych na wykresach w bloku Przegląd. Na początku zobaczysz tylko jeden lub dwa punkty. Na przykład:

![Klikaj elementy, aby uzyskać więcej danych](./media/app-insights-asp-net-manual/12-first-perf.png)

Klikaj elementy wykresów, aby wyświetlać bardziej szczegółowe metryki. [Dowiedz się więcej o metrykach.][wydajność]

#### Brak danych?

* Używaj aplikacji, otwierając różne strony, aby wygenerować dane telemetryczne.
* Otwórz kafelek [Wyszukaj][diagnostyka], aby wyświetlić poszczególne zdarzenia. Niekiedy potrzeba nieco więcej czasu, zanim zdarzenia dotrą przez potok metryk.
* Odczekaj kilka sekund, a następnie kliknij przycisk **Odśwież**. Wykresy są odświeżane okresowo, ale można odświeżyć je ręcznie w razie oczekiwania na pojawienie się pewnych danych.
* Zobacz [Rozwiązywanie problemów][qna].

## Publikowanie aplikacji

Przeprowadź teraz wdrożenie aplikacji w usługach IIS lub na platformie Azure. Rozpocznie się gromadzenie danych.

![Użycie programu Visual Studio do publikowania aplikacji](./media/app-insights-asp-net-manual/15-publish.png)

Po uruchomieniu w trybie debugowania telemetria jest przyspieszana w potoku, dzięki czemu dane powinny być widoczne w ciągu kilku sekund. Po wdrożeniu aplikacji w konfiguracji wydania dane są gromadzone wolniej.

#### Brak danych po opublikowaniu na serwerze?

Otwórz następujące porty dla ruchu wychodzącego w zaporze serwera:

+ `dc.services.visualstudio.com:443`
+ `f5.services.visualstudio.com:443`


#### Problem z serwerem kompilacji?

Zobacz [ten punkt rozwiązywania problemów](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

> [AZURE.NOTE] Jeśli aplikacja generuje wiele danych telemetrycznych (i używasz wersji 2.0.0-beta3 lub nowszej zestawu SDK platformy ASP.NET), moduł próbkowania adaptacyjnego automatycznie będzie redukować ilość danych, które są wysyłane do portalu, wysyłając tylko reprezentatywny ułamek zdarzeń. Jednak zdarzenia związane z tym samym żądaniem zostaną wybrane lub pominięte jako grupa, dzięki czemu możesz nawigować między powiązanymi zdarzeniami. 
> [Więcej informacji na temat próbkowania](app-insights-sampling.md).


## Dodawanie śledzenia zależności (i liczników wydajności usług IIS)

Zestaw SDK wymaga niewielkiej pomocy, aby uzyskać dostęp do niektórych danych. Ten dodatkowy krok należy wykonać zwłaszcza w celu automatycznego pomiaru wywołań między aplikacją a bazami danych, interfejsami API REST i innymi składnikami zewnętrznymi. Te metryki zależności mogą być cenne, ponieważ ułatwiają diagnozowanie problemów z wydajnością.

Jeśli pracujesz na własnym serwerze IIS, ten krok umożliwia również pokazywanie liczników wydajności systemu w [eksploratorze metryk](app-insights-metrics-explorer.md).

#### Jeśli aplikacja działa na Twoim serwerze IIS

Zaloguj się do serwera przy użyciu uprawnień administratora, a następnie zainstaluj [Monitor stanu usługi Application Insights](http://go.microsoft.com/fwlink/?LinkId=506648).

Konieczne może być [otwarcie dodatkowych portów wychodzących w zaporze](app-insights-monitor-performance-live-website-now.md#troubleshooting).

Ten krok umożliwia również [raportowanie liczników wydajności](app-insights-web-monitor-performance.md#system-performance-counters) np. procesora, pamięci, zajętości sieci.

#### Jeśli aplikacja jest aplikacją sieci Web platformy Azure

W panelu sterowania aplikacji sieci Web platformy Azure dodaj rozszerzenie Application Insights.

![W aplikacji sieci Web wybierz kolejno opcje Ustawienia, Rozszerzenia, Dodaj, Application Insights](./media/app-insights-asp-net-manual/05-extend.png)


#### Jeśli jest to projekt usług w chmurze Azure

[Dodaj skrypty do ról sieci Web i procesu roboczego](app-insights-cloudservices.md).



## Dodawanie monitorowania po stronie klienta

Udało Ci się przeprowadzić instalację zestawu SDK, który wysyła dane telemetryczne z serwera (zaplecza) aplikacji. Teraz możesz dodać monitorowanie po stronie klienta. Dostarcza ono dane dotyczące użytkowników, sesji, wyświetleń stron i ewentualnych wyjątków lub awarii występujących w przeglądarce. Możesz także napisać własny kod, aby śledzić, jak użytkownicy korzystają z aplikacji, schodząc do szczegółowego poziomu kliknięć i naciśnięć klawiszy.


Dodaj fragment kodu JavaScript na każdej stronie. Pobierz kod ze swojego zasobu usługi Application Insights:

![W aplikacji sieci Web otwórz Szybki start, a następnie kliknij polecenie „Pobierz kod umożliwiający monitorowanie stron sieci Web”.](./media/app-insights-asp-net-manual/02-monitor-web-page.png)

Zauważ, że kod zawiera klucz instrumentacji, który identyfikuje dany zasób aplikacji.

[Dowiedz się więcej na temat śledzenia stron sieci Web.](app-insights-web-track-usage.md)


## Śledzenie wersji aplikacji

Upewnij się, że plik `buildinfo.config` jest generowany przez proces MSBuild. W pliku .csproj dodaj:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup> 
```

Jeśli plik zawiera informację o kompilacji, moduł sieci Web usługi Application Insights automatycznie dodaje **wersję aplikacji** jako właściwość do każdego elementu telemetrii. Pozwala to na filtrowanie według wersji podczas przeprowadzania [wyszukiwania diagnostycznego][diagnostyka] lub [eksplorowania metryk][metrics]. 

Numer wersji kompilacji jest jednak generowany tylko przez program MS Build, a nie podczas kompilowania przez deweloperów w programie Visual Studio.

## Kończenie instalacji

Aby uzyskać pełny wgląd w dane aplikacji, można zrobić jeszcze kilka rzeczy:

* [Konfigurowanie testów sieci Web][availability], aby upewnić się, że aplikacja stale działa i odpowiada.
* [Przechwytywanie danych śledzenia dziennika][netlogs] z preferowanej struktury rejestrowania
* [Śledzenie niestandardowych zdarzeń i metryk][api] po stronie klienta, serwera lub po obu stronach, aby dowiedzieć się więcej o sposobie korzystania z aplikacji.

## <a name="ide"></a> Procedura zautomatyzowana

Na początku tego artykułu napisaliśmy, że pokażemy ręczną procedurę tworzenia zasobu usługi Application Insights, a następnie instalacji zestawu SDK. Uważamy, że warto rozumieć obie części tej procedury. Jednak w przypadku aplikacji ASP.NET (i wielu innych) jest jeszcze szybszy, zautomatyzowany sposób.

Potrzebne będą: program [Visual Studio](http://go.microsoft.com/fwlink/?linkid=397827&clcid=0x409) (2013 Update 3 lub nowszy) oraz konto w na platformie [Microsoft Azure](http://azure.com).

#### Nowy projekt

Podczas tworzenia nowego projektu w programie Visual Studio upewnij się, że jest zaznaczona opcja **Dodaj usługę Application Insights**.


![Tworzenie projektu ASP.NET](./media/app-insights-asp-net-manual/appinsights-01-vsnewp1.png)

Program Visual Studio tworzy zasób w usłudze Application Insights, dodaje zestaw SDK do projektu i umieszcza klucz w pliku `.config`.

Jeśli projekt zawiera strony sieci Web, program dodaje również [Zestaw SDK JavaScript][klient] do wzorcowej strony sieci Web.

#### Istniejący projekt

Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, a następnie wybierz polecenie **Dodaj usługę Application Insights**.

![Wybieranie opcji Dodaj usługę Application Insights](./media/app-insights-asp-net-manual/appinsights-03-addExisting.png)

Program Visual Studio tworzy zasób w usłudze Application Insights, dodaje zestaw SDK do projektu i umieszcza klucz w pliku `.config`.

W takim przypadku [Zestaw SDK JavaScript][klient] nie jest dodawany do stron sieci Web — zalecamy, aby zrobić to w następnym kroku.

#### Opcje konfiguracji

Jeśli robisz to po raz pierwszy, pojawi się monit o zalogowanie lub zarejestrowanie się na platformie Microsoft Azure w wersji zapoznawczej. 

Jeśli ta aplikacja jest częścią większej aplikacji, warto użyć opcji **Konfigurowanie ustawień**, aby umieścić ją w tej samej grupie zasobów co inne składniki.

*Nie ma opcji Application Insights? Sprawdź, czy używasz programu Visual Studio w wersji 2013 Update 3 lub nowszej, a narzędzia Application Insights Tools są włączone w obszarze Rozszerzenia i aktualizacje.*

#### Otwieranie usługi Application Insights z poziomu projektu

![Kliknij projekt prawym przyciskiem myszy i otwórz portal Azure](./media/app-insights-asp-net-manual/appinsights-04-openPortal.png)




## <a name="video"></a>Wideo

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


