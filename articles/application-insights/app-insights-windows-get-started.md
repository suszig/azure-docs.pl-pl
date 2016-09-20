<properties
    pageTitle="Analizy dotyczące aplikacji dla systemu Windows Phone i Sklepu Windows | Microsoft Azure"
    description="Analizowanie użycia i awarii aplikacji na urządzeniach z systemem Windows."
    services="application-insights"
    documentationCenter="windows"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/26/2016"
    ms.author="awills"/>

# Analizy dotyczące aplikacji dla systemu Windows Phone i Sklepu Windows

Firma Microsoft udostępnia dwa rozwiązania metodyki DevOps dotyczące urządzeń: [HockeyApp](http://hockeyapp.net/) (analizy po stronie klienta) oraz [Application Insights](app-insights-overview.md) (analizy po stronie serwera).

[HockeyApp](http://hockeyapp.net/) to nasze rozwiązanie do metodyki Mobile DevOps dotyczące aplikacji na urządzeniach z systemami iOS, OS X, Android oraz Windows, a także aplikacji międzyplatformowych opartych na technologiach Xamarin, Cordova i Unity. Dzięki niemu można dystrybuować kompilacje do testerów wersji beta, zbierać dane dotyczące awarii oraz gromadzić metryki i opinie użytkowników. Rozwiązanie to jest zintegrowane z usługami Visual Studio Team Services, co pozwala na łatwe wdrażanie kompilacji i integrację elementów pracy. 

## Wprowadzenie do platformy HockeyApp

Przejdź do strony:

* [HockeyApp](http://support.hockeyapp.net/kb)
* [HockeyApp dla systemu Windows](http://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone)
* [Blog HockeyApp](http://hockeyapp.net/blog/)
* Dołącz do społeczności [Hockeyapp Preseason](http://hockeyapp.net/preseason/), aby uzyskać wczesne wersje.

Jeśli aplikacja działa po stronie serwera, użyj usługi [Application Insights](app-insights-overview.md) do monitorowania aplikacji w technologii [ASP.NET](app-insights-asp-net.md) lub [J2EE](app-insights-java-get-started.md) po stronie serwera sieci Web. 


Możesz również użyć usługi [Application Insights dla aplikacji klasycznych systemu Windows](app-insights-windows-desktop.md).

## Analiza, eksportowanie i dostęp interfejsu API do danych platformy HockeyApp 

[Skonfiguruj mostek platformy HockeyApp](app-insights-hockeyapp-bridge-app.md) w usłudze Application Insights. Umożliwia to:

* Użycie zaawansowanego języka zapytań [analizy](app-insights-analytics.md) w ramach telemetrii. 
* [Wyeksportowanie telemetrii](app-insights-export-telemetry.md) do magazynu obiektów blob platformy Azure.

## Następne kroki

* [Wprowadzenie do rozwiązania HockeyApp dla systemu Windows](http://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone)



<!--HONumber=sep16_HO1-->


