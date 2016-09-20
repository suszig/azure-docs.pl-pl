<properties
    pageTitle="Analiza aplikacji systemu iOS | Microsoft Azure"
    description="Analizowanie użycia i wydajności aplikacji systemu iOS."
    services="application-insights"
    documentationCenter="ios"
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

# Analiza aplikacji systemu iOS

Firma Microsoft udostępnia dwa rozwiązania metodyki DevOps dotyczące urządzeń: [HockeyApp](http://hockeyapp.net/) do urządzeń klienckich oraz [Application Insights](app-insights-overview.md) do analiz po stronie serwera oraz stron sieci Web klienta.

[HockeyApp](http://hockeyapp.net/) to nasze rozwiązanie metodyki Mobile DevOps do tworzenia aplikacji dla urządzeń z systemami iOS, OS X, Android lub Windows, a także aplikacji międzyplatformowych opartych na technologiach Xamarin, Cordova i Unity. Dzięki niemu można dystrybuować kompilacje do testerów wersji beta, zbierać dane dotyczące awarii i uzyskiwać opinie użytkowników. Jest zintegrowane z usługami Visual Studio Team Services, co pozwala na łatwe wdrażanie kompilacji i integrację elementów pracy. 


Przejdź do strony:

* [HockeyApp](http://support.hockeyapp.net/kb)
* [HockeyApp dla systemu iOS](http://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [Blog HockeyApp](http://hockeyapp.net/blog/)
* Dołącz do społeczności [Hockeyapp Preseason](http://hockeyapp.net/preseason/), aby uzyskać wczesne wersje.

Jeśli aplikacja działa po stronie serwera, użyj usługi [Application Insights](app-insights-overview.md) do monitorowania aplikacji w technologii [ASP.NET](app-insights-asp-net.md) lub [J2EE](app-insights-java-get-started.md) po stronie serwera sieci Web. 


## Analiza, eksportowanie i dostęp interfejsu API do danych platformy HockeyApp 

[Skonfiguruj mostek platformy HockeyApp](app-insights-hockeyapp-bridge-app.md) w usłudze Application Insights. Umożliwia to:

* Użycie zaawansowanego języka zapytań [analizy](app-insights-analytics.md) w ramach telemetrii. 
* [Wyeksportowanie telemetrii](app-insights-export-telemetry.md) do magazynu obiektów blob platformy Azure.

## Następne kroki

* [Wprowadzenie do platformy HockeyApp dla systemu iOS](http://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)




<!--HONumber=sep16_HO1-->


