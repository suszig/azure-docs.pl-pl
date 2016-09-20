<properties
    pageTitle="Dodawanie zestawu SDK usługi Application Insights w celu monitorowania aplikacji Node.js | Microsoft Azure"
    description="Analizowanie użycia, dostępności i wydajności aplikacji lokalnej lub aplikacji sieci Web na platformie Microsoft Azure za pomocą usługi Application Insights."
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/25/2016"
    ms.author="awills"/>


# Dodawanie zestawu SDK usługi Application Insights w celu monitorowania aplikacji Node.js

*Usługa Application Insights jest w wersji zapoznawczej.*

Usługa [Visual Studio Application Insights](app-insights-overview.md) służy do monitorowania działającej aplikacji, aby ułatwić [wykrywanie i diagnozowanie problemów z wydajnością oraz wyjątków](app-insights-detect-triage-diagnose.md) i [uzyskiwanie informacji na temat sposobu użycia aplikacji](app-insights-overview-usage.md). Obsługuje aplikacje, które są hostowane na lokalnych serwerach usług IIS lub na maszynach wirtualnych platformy Azure, jak również aplikacje sieci Web platformy Azure.



Zestaw SDK zapewnia automatyczne zbieranie wskaźników przychodzących żądań HTTP i odpowiedzi, liczników wydajności (procesora, pamięci, żądań na sekundę) i nieobsługiwanych wyjątków. Można również dodać niestandardowe wywołania do śledzenia zależności, metryk i innych zdarzeń.

![Przykładowe wykresy monitorowania wydajności](./media/app-insights-asp-net-manual/10-perf.png)


#### Przed rozpoczęciem

Potrzebne elementy:

* Program Visual Studio 2013 lub nowszy. (Im nowszy, tym lepiej).
* Subskrypcja platformy [Microsoft Azure](http://azure.com). Jeśli Twój zespół lub organizacja mają subskrypcję platformy Azure, właściciel subskrypcji może Cię do niej dodać, korzystając z Twojego [konta Microsoft](http://live.com).

## <a name="add"></a>Tworzenie zasobu usługi Application Insights

Zaloguj się do [portalu Azure][portal] i utwórz nowy zasób usługi Application Insights. [Zasób][role] na platformie Azure to wystąpienie usługi. Ten zasób jest miejscem, gdzie odbywa się analizowane i wyświetlanie telemetrii z aplikacji.

![Kliknij kolejno polecenia Nowy, Application Insights](./media/app-insights-asp-net-manual/01-new-asp.png)

Wybierz Inny jako typ aplikacji. Wybór typu aplikacji powoduje ustawienie domyślnej zawartości bloków zasobów i właściwości widocznych w [Eksploratorze metryk][metrics].

#### Kopiowanie klucza instrumentacji

Klucz identyfikuje zasób. Wkrótce zainstalujesz go w zestawie SDK w celu kierowania danych do tego zasobu.

![Kliknij opcję Właściwości, zaznacz klucz i naciśnij Ctrl+C](./media/app-insights-asp-net-manual/02-props-asp.png)


## <a name="sdk"></a> Instalowanie zestawu SDK w aplikacji

```
npm install applicationinsights --save
```

## Sposób użycia

Umożliwi to monitorowanie żądań, śledzenie nieobsługiwanych wyjątków oraz monitorowanie wydajności systemu (procesora CPU, pamięci, żądań na sekundę).

```javascript

var appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>").start();
```

Można również ustawić klucz instrumentacji w zmiennej środowiskowej APPINSIGHTS_INSTRUMENTATIONKEY. Żaden argument nie będzie wtedy wymagany podczas wywoływania metody `appInsights.setup()` lub `appInsights.getClient()`.

Możesz przetestować zestaw SDK bez wysyłania telemetrii — ustaw niepusty ciąg tekstowy jako klucz instrumentacji.


## <a name="run"></a> Uruchamianie projektu

Uruchom aplikację i wypróbuj jej działanie. Otwieraj różne strony, aby wygenerować dane telemetryczne.


## <a name="monitor"></a> Wyświetlanie telemetrii

Wróć do [portalu Azure](https://portal.azure.com) i przejdź do zasobu usługi Application Insights.


Poszukaj danych na stronie Przegląd. Na początku zobaczysz tylko jeden lub dwa punkty. Na przykład:

![Klikaj elementy, aby uzyskać więcej danych](./media/app-insights-asp-net-manual/12-first-perf.png)

Klikaj elementy wykresów, aby wyświetlać bardziej szczegółowe metryki. [Dowiedz się więcej o metrykach.][wydajność]

#### Brak danych?

* Otwieraj różne strony w aplikacji, aby wygenerować dane telemetryczne.
* Otwórz kafelek [Wyszukaj](app-insights-diagnostic-search.md), aby wyświetlić poszczególne zdarzenia. Niekiedy potrzeba nieco więcej czasu, zanim zdarzenia dotrą przez potok metryk.
* Odczekaj kilka sekund, a następnie kliknij przycisk **Odśwież**. Wykresy są odświeżane okresowo, ale można odświeżyć je ręcznie, jeśli oczekiwane jest pojawienie się pewnych danych.
* Zobacz [Rozwiązywanie problemów][qna].

## Publikowanie aplikacji

Przeprowadź teraz wdrożenie aplikacji w usługach IIS lub na platformie Azure. Rozpocznie się gromadzenie danych.


#### Brak danych po opublikowaniu na serwerze?

Otwórz następujące porty dla ruchu wychodzącego w zaporze serwera:

+ `dc.services.visualstudio.com:443`
+ `f5.services.visualstudio.com:443`


#### Problem z serwerem kompilacji?

Zobacz [ten punkt rozwiązywania problemów](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).



## Niestandardowe użycie 

### Wyłączanie automatycznego zbierania

```javascript
import appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoCollectRequests(false)
    .setAutoCollectPerformance(false)
    .setAutoCollectExceptions(false)
    // no telemetry will be sent until .start() is called
    .start();
```

### Niestandardowe monitorowanie

```javascript
import appInsights = require("applicationinsights");
var client = appInsights.getClient();

client.trackEvent("custom event", {customProperty: "custom property value"});
client.trackException(new Error("handled exceptions can be logged with this method"));
client.trackMetric("custom metric", 3);
client.trackTrace("trace message");
```

[Dowiedz się więcej o interfejsie API telemetrii](app-insights-api-custom-events-metrics.md).

### Korzystanie z wielu kluczy instrumentacji

```javascript
import appInsights = require("applicationinsights");

// configure auto-collection with one instrumentation key
appInsights.setup("<instrumentation_key>").start();

// get a client for another instrumentation key
var otherClient = appInsights.getClient("<other_instrumentation_key>");
otherClient.trackEvent("custom event");
```

## Przykłady

### Śledzenie zależności

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



### Ręczne śledzenie wszystkich żądań „GET”

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




<!--Link references-->

[knowUsers]: app-insights-overview-usage.md
[metrics]: app-insights-metrics-explorer.md
[wydajność]: app-insights-web-monitor-performance.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[role]: app-insights-resources-roles-access-control.md



<!--HONumber=sep16_HO1-->


