---
title: "Dodawanie zestawu SDK usługi Application Insights w celu monitorowania aplikacji Node.js | Microsoft Docs"
description: "Analizowanie użycia, dostępności i wydajności aplikacji lokalnej lub aplikacji sieci Web na platformie Microsoft Azure za pomocą usługi Application Insights."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 2ec7f809-5e1a-41cf-9fcd-d0ed4bebd08c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/30/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fb80168b38be88ab18952569e6b6f9bcb53d473a


---
# <a name="add-application-insights-sdk-to-monitor-your-nodejs-app"></a>Dodawanie zestawu SDK usługi Application Insights w celu monitorowania aplikacji Node.js
*Usługa Application Insights jest dostępna w wersji zapoznawczej.*

Usługa [Visual Studio Application Insights](app-insights-overview.md) służy do monitorowania działającej aplikacji, aby ułatwić [wykrywanie i diagnozowanie problemów z wydajnością oraz wyjątków](app-insights-detect-triage-diagnose.md) i [uzyskiwanie informacji na temat sposobu użycia aplikacji](app-insights-overview-usage.md). Obsługuje aplikacje, które są hostowane na lokalnych serwerach usług IIS lub na maszynach wirtualnych platformy Azure, jak również aplikacje sieci Web platformy Azure.

Zestaw SDK zapewnia automatyczne zbieranie wskaźników przychodzących żądań HTTP i odpowiedzi, liczników wydajności (procesora, pamięci, żądań na sekundę) i nieobsługiwanych wyjątków. Można również dodać niestandardowe wywołania do śledzenia zależności, metryk i innych zdarzeń.

![Przykładowe wykresy monitorowania wydajności](./media/app-insights-nodejs/10-perf.png)

#### <a name="before-you-start"></a>Przed rozpoczęciem
Potrzebne elementy:

* Program Visual Studio 2013 lub nowszy. (Im nowszy, tym lepiej).
* Subskrypcja platformy [Microsoft Azure](http://azure.com). Jeśli Twój zespół lub organizacja mają subskrypcję platformy Azure, właściciel subskrypcji może Cię do niej dodać, korzystając z Twojego [konta Microsoft](http://live.com).

## <a name="a-nameaddacreate-an-application-insights-resource"></a><a name="add"></a>Tworzenie zasobu usługi Application Insights
Zaloguj się do [Azure Portal][portal] i utwórz nowy zasób usługi Application Insights. [Zasób][role] na platformie Azure to wystąpienie usługi. Ten zasób jest miejscem, gdzie odbywa się analizowane i wyświetlanie telemetrii z aplikacji.

![Kliknij kolejno polecenia Nowy, Application Insights](./media/app-insights-nodejs/01-new-asp.png)

Wybierz Inny jako typ aplikacji. Wybór typu aplikacji powoduje ustawienie domyślnej zawartości bloków zasobów i właściwości widocznych w [Eksploratorze metryk][metrics].

#### <a name="copy-the-instrumentation-key"></a>Kopiowanie klucza instrumentacji
Klucz identyfikuje zasób. Wkrótce zainstalujesz go w zestawie SDK w celu kierowania danych do tego zasobu.

![Kliknij opcję Właściwości, zaznacz klucz i naciśnij klawisze Ctrl+C](./media/app-insights-nodejs/02-props-asp.png)

## <a name="a-namesdka-install-the-sdk-in-your-application"></a><a name="sdk"></a> Instalowanie zestawu SDK w aplikacji
```
npm install applicationinsights --save
```

## <a name="usage"></a>Sposób użycia
Umożliwi to monitorowanie żądań, śledzenie nieobsługiwanych wyjątków oraz monitorowanie wydajności systemu (procesora CPU, pamięci, żądań na sekundę).

```javascript

var appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>").start();
```

Można również ustawić klucz instrumentacji w zmiennej środowiskowej APPINSIGHTS_INSTRUMENTATIONKEY. Żaden argument nie będzie wtedy wymagany podczas wywoływania metody `appInsights.setup()` lub `appInsights.getClient()`.

Możesz przetestować zestaw SDK bez wysyłania telemetrii — ustaw niepusty ciąg tekstowy jako klucz instrumentacji.

## <a name="a-nameruna-run-your-project"></a><a name="run"></a> Uruchamianie projektu
Uruchom aplikację i wypróbuj jej działanie. Otwieraj różne strony, aby wygenerować dane telemetryczne.

## <a name="a-namemonitora-view-your-telemetry"></a><a name="monitor"></a> Wyświetlanie telemetrii
Wróć do [Azure Portal](https://portal.azure.com) i przejdź do zasobu usługi Application Insights.

Poszukaj danych na stronie Przegląd. Na początku zobaczysz tylko jeden lub dwa punkty. Na przykład:

![Klikaj elementy, aby uzyskać więcej danych](./media/app-insights-nodejs/12-first-perf.png)

Klikaj elementy wykresów, aby wyświetlać bardziej szczegółowe metryki. [Dowiedz się więcej o metrykach.][wydajność]

#### <a name="no-data"></a>Brak danych?
* Otwieraj różne strony w aplikacji, aby wygenerować dane telemetryczne.
* Otwórz kafelek [Wyszukaj](app-insights-diagnostic-search.md), aby wyświetlić poszczególne zdarzenia. Niekiedy potrzeba nieco więcej czasu, zanim zdarzenia dotrą przez potok metryk.
* Odczekaj kilka sekund, a następnie kliknij przycisk **Odśwież**. Wykresy są odświeżane okresowo, ale można odświeżyć je ręcznie, jeśli oczekiwane jest pojawienie się pewnych danych.
* Zobacz [Rozwiązywanie problemów][qna].

## <a name="publish-your-app"></a>Publikowanie aplikacji
Przeprowadź teraz wdrożenie aplikacji w usługach IIS lub na platformie Azure. Rozpocznie się gromadzenie danych.

#### <a name="no-data-after-you-publish-to-your-server"></a>Brak danych po opublikowaniu na serwerze?
Otwórz następujące porty dla ruchu wychodzącego w zaporze serwera:

* `dc.services.visualstudio.com:443`
* `f5.services.visualstudio.com:443`

#### <a name="trouble-on-your-build-server"></a>Problem z serwerem kompilacji?
Zobacz [ten punkt rozwiązywania problemów](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

## <a name="customized-usage"></a>Niestandardowe użycie
### <a name="disabling-autocollection"></a>Wyłączanie automatycznego zbierania
```javascript
import appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoCollectRequests(false)
    .setAutoCollectPerformance(false)
    .setAutoCollectExceptions(false)
    // no telemetry will be sent until .start() is called
    .start();
```

### <a name="custom-monitoring"></a>Niestandardowe monitorowanie
```javascript
import appInsights = require("applicationinsights");
var client = appInsights.getClient();

client.trackEvent("custom event", {customProperty: "custom property value"});
client.trackException(new Error("handled exceptions can be logged with this method"));
client.trackMetric("custom metric", 3);
client.trackTrace("trace message");
```

[Dowiedz się więcej o interfejsie API telemetrii](app-insights-api-custom-events-metrics.md).

### <a name="using-multiple-instrumentation-keys"></a>Korzystanie z wielu kluczy instrumentacji
```javascript
import appInsights = require("applicationinsights");

// configure auto-collection with one instrumentation key
appInsights.setup("<instrumentation_key>").start();

// get a client for another instrumentation key
var otherClient = appInsights.getClient("<other_instrumentation_key>");
otherClient.trackEvent("custom event");
```

## <a name="examples"></a>Przykłady
### <a name="tracking-dependency"></a>Śledzenie zależności
```javascript
import appInsights = require("applicationinsights");
var client = appInsights.getClient();

var startTime = Date.now();
// execute dependency call
var endTime = Date.now();

var elapsedTime = endTime - startTime;
var success = true;
client.trackDependency("dependency name", "command name", elapsedTime, success);
```



### <a name="manual-request-tracking-of-all-get-requests"></a>Ręczne śledzenie wszystkich żądań „GET”
```javascript
var http = require("http");
var appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoCollectRequests(false) // disable auto-collection of requests for this example
    .start();

// assign common properties to all telemetry sent from the default client
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};

// track a system startup event
appInsights.client.trackEvent("server start");

// create server
var port = process.env.port || 1337
var server = http.createServer(function (req, res) {
    // track all "GET" requests
    if(req.method === "GET") {
        appInsights.client.trackRequest(req, res);
    }

    res.writeHead(200, { "Content-Type": "text/plain" });
    res.end("Hello World\n");
}).listen(port);

// track startup time of the server as a custom metric
var start = +new Date;
server.on("listening", () => {
    var end = +new Date;
    var duration = end - start;
    appInsights.client.trackMetric("StartupTime", duration);
});
```

## <a name="next-steps"></a>Następne kroki
* [Monitorowanie telemetrii w portalu](app-insights-dashboards.md)
* [Zapisywanie zapytań analizy za pośrednictwem danych telemetrycznych](app-insights-analytics-tour.md)

<!--Link references-->

[znani użytkownicy]: app-insights-overview-usage.md
[metrics]: app-insights-metrics-explorer.md
[wydajność]: app-insights-web-monitor-performance.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[role]: app-insights-resources-roles-access-control.md



<!--HONumber=Nov16_HO2-->


