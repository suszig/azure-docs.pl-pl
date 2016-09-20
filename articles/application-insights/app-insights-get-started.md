<properties
    pageTitle="Wprowadzenie do usługi Application Insights w programie Visual Studio | Microsoft Azure"
    description="Analizowanie użycia, dostępności i wydajności aplikacji sieci Web uruchomionej lokalnie lub na platformie Microsoft Azure za pomocą usługi Application Insights w programie Visual Studio."
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="06/06/2016"
    ms.author="awills"/>

# Wprowadzenie do usługi Application Insights w programie Visual Studio

*Usługa Application Insights jest dostępna w wersji zapoznawczej.*

Wykrywaj problemy, rozwiązuj je i stale udoskonalaj aplikacje. Szybko diagnozuj problemy w działającej aplikacji. Dowiedz się, do czego używają jej użytkownicy.

Konfiguracja jest bardzo łatwa, a wyniki zostaną wyświetlone w ciągu kilku minut.

Obecnie obsługujemy aplikacje w systemach iOS, Android i Windows, aplikacje sieci Web w technologiach J2EE i ASP.NET oraz usługi WCF. Aplikacje sieci Web można uruchamiać na platformie Azure lub na serwerach lokalnych. Nasz zestaw SDK języka JavaScript działa na dowolnej stronie sieci Web.

[Obejrzyj animację wprowadzającą](https://www.youtube.com/watch?v=fX2NtGrh-Y0).

## Wprowadzenie

Zacznij od dowolnej kombinacji i kolejności punktów wejściowych pokazanych na poniższym diagramie. Zrealizuj ścieżkę, która odpowiada Twoim potrzebom.

Usługa Application Insights działa dzięki dodaniu do aplikacji zestawu SDK, który umożliwia wysyłanie telemetrii do [portalu Azure](https://portal.azure.com). Istnieją różne zestawy SDK dla różnych obsługiwanych kombinacji platform, języków i środowisk IDE.

Będziesz potrzebować konta platformy [Microsoft Azure](http://azure.com). Możesz mieć już dostęp do konta grupy za pośrednictwem swojej organizacji lub możesz uzyskać konto opłacane zgodnie z rzeczywistym użyciem. Usługa Application Insights ma warstwę bezpłatną, więc nie trzeba płacić, dopóki aplikacja nie stanie się popularna. Zobacz [cennik](https://azure.microsoft.com/pricing/details/application-insights/).

Potrzeby | Realizacja | Efekty
---|---|---
 <a href="app-insights-asp-net.md">![ASP.NET](./media/app-insights-get-started/appinsights-gs-i-01-perf.png)</a> | <a href="app-insights-asp-net.md">Dodaj zestaw SDK usługi Application Insights do projektu sieci Web</a> <br/> ![efekty](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-asp-net.md">![Monitorowanie wydajności i użycia](./media/app-insights-get-started/appinsights-gs-r-01-perf.png)</a>
<a href="app-insights-monitor-performance-live-website-now.md">![Witryna ASP.NET, która już działa](./media/app-insights-get-started/appinsights-gs-i-04-red2.png)</a><br/><a href="app-insights-monitor-performance-live-website-now.md">![Monitorowanie zależności i wydajności](./media/app-insights-get-started/appinsights-gs-i-03-red.png)</a>|<a href="app-insights-monitor-performance-live-website-now.md">Zainstaluj monitor stanu na serwerze IIS</a> <br/> ![efekty](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-monitor-performance-live-website-now.md">![Monitorowanie zależności w technologii ASP.NET](./media/app-insights-get-started/appinsights-gs-r-03-red.png)</a>
<a href="insights-perf-analytics.md">![Aplikacja sieci Web lub maszyna wirtualna na platformie Azure](./media/app-insights-get-started/appinsights-gs-i-10-azure.png)</a>|<a href="insights-perf-analytics.md">Włącz usługę Insights w aplikacji sieci Web lub maszynie wirtualnej na platformie Azure</a> <br/> ![efekty](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="insights-perf-analytics.md">![Monitorowanie zależności i wydajności](./media/app-insights-get-started/appinsights-gs-r-03-red.png)</a>
<a href="app-insights-java-get-started.md">![Java](./media/app-insights-get-started/appinsights-gs-i-11-java.png)</a>|<a href="app-insights-java-get-started.md">Dodaj zestaw SDK do projektu języka Java</a><br/>![efekty](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-java-get-started.md">![Monitorowanie wydajności i użycia](./media/app-insights-get-started/appinsights-gs-r-10-java.png)</a>
<a href="app-insights-web-track-usage.md">![JavaScript](./media/app-insights-get-started/appinsights-gs-i-02-usage.png)</a>|<a href="app-insights-web-track-usage.md">Wstaw skrypt usługi Application Insights do stron sieci Web</a><br/>![efekty](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-web-track-usage.md">![liczba wyświetleń stron i wydajność przeglądarki](./media/app-insights-get-started/appinsights-gs-r-02-usage.png)</a>
<a href="app-insights-monitor-web-app-availability.md">![Dostępność](./media/app-insights-get-started/appinsights-gs-i-05-avail.png)</a>|<a href="app-insights-monitor-web-app-availability.md">Utwórz testy sieci Web</a><br/>![efekty](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-monitor-web-app-availability.md">![Dostępność](./media/app-insights-get-started/appinsights-gs-r-05-avail.png)</a>
<a href="app-insights-platforms.md">![Urządzenia z systemem iOS, Android i Windows](./media/app-insights-get-started/appinsights-gs-i-07-device.png)</a>|<a href="http://hockeyapp.net">Użyj aplikacji HockeyApp</a><br/>![efekty](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="http://hockeyapp.net">![Dane awarii i użycia](./media/app-insights-get-started/appinsights-gs-r-06-device.png)</a>

## Pomoc techniczna i opinie


* Pytania i problemy:
 * [Rozwiązywanie problemów][pytania_i_odpowiedzi].
 * [Forum MSDN](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)
 * [Witryna StackOverflow](http://stackoverflow.com/questions/tagged/ms-application-insights)
* Sugestie:
 * [UserVoice](https://visualstudio.uservoice.com/forums/357324)
* Przykłady kodu
 * [Przykłady kodu](app-insights-code-samples.md)



## <a name="video"></a>Film wideo

[![Aanimowane wprowadzenie](./media/app-insights-get-started/video-front-1.png)](https://www.youtube.com/watch?v=fX2NtGrh-Y0)

> [AZURE.VIDEO 218]

> [AZURE.VIDEO usage-monitoring-application-insights]

> [AZURE.VIDEO performance-monitoring-application-insights]



<!--Link references-->

[pytania_i_odpowiedzi]: app-insights-troubleshoot-faq.md



<!--HONumber=sep16_HO1-->


