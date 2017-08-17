---
title: "Monitorowanie usług Node.js za pomocą usługi Azure Application Insights | Microsoft Docs"
description: "Monitoruj wydajność i diagnozuj problemy w usługach Node.js za pomocą usługi Application Insights."
services: application-insights
documentationcenter: nodejs
author: joshgav
manager: carmonm
ms.assetid: 2ec7f809-5e1a-41cf-9fcd-d0ed4bebd08c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/01/2017
ms.author: bwren
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 76a8025cd2a67533beb321c88e924517c1977dfc
ms.contentlocale: pl-pl
ms.lasthandoff: 05/02/2017

---

# <a name="monitor-your-nodejs-services-and-apps-with-application-insights"></a>Monitorowanie usług i aplikacji Node.js za pomocą usługi Application Insights

Usługa [Azure Application Insights](app-insights-overview.md) monitoruje usługi i składniki zaplecza po ich wdrożeniu w celu ułatwienia [odnajdywania i szybkiego diagnozowania różnych problemów, na przykład z wydajnością](app-insights-detect-triage-diagnose.md). Usługi tej można używać z usługami Node.js hostowanymi w dowolnym miejscu: w centrum danych, na maszynach wirtualnych i aplikacjach Web Apps platformy Azure, a nawet w innych chmurach publicznych.

Aby móc odbierać, przechowywać i eksplorować dane monitorowania, należy wykonać następujące instrukcje w celu uwzględnienia agenta w kodzie, a następnie skonfigurowania odpowiedniego zasobu usługi Application Insights na platformie Azure. Agent wysyła dane do zasobu na potrzeby przyszłej analizy i eksploracji.

Agent Node.js może automatycznie monitorować przychodzące i wychodzące żądania HTTP, kilka metryk systemu i wyjątki. Począwszy od wersji v0.20 agent może także monitorować niektóre popularne pakiety innych firm, takie jak `mongodb`, `mysql`, i `redis`. Wszystkie zdarzenia związane z przychodzącym żądaniem HTTP są skorelowane, aby zapewnić szybsze rozwiązanie problemów.

Inne aspekty aplikacji i systemu można monitorować po ich ręcznej instrumentacji za pomocą opisanego dalej interfejsu API agenta.

![Przykładowe wykresy monitorowania wydajności](./media/app-insights-nodejs/10-perf.png)

## <a name="getting-started"></a>Wprowadzenie

Rozważmy kroki konfigurowania na potrzeby aplikacji lub usługi.

### <a name="resource"></a> Konfigurowanie zasobu usługi App Insights

**Przed rozpoczęciem** upewnij się, że masz subskrypcję platformy Azure, lub [bezpłatnie uzyskaj nową][azure-free-offer]. Jeśli Twoja organizacja ma już subskrypcję platformy Azure, administrator może dodać Cię do niej, wykonując [te instrukcje][add-aad-user].

[azure-free-offer]: https://azure.microsoft.com/en-us/free/
[add-aad-user]: https://docs.microsoft.com/en-us/azure/active-directory/active-directory-users-create-azure-portal

Zaloguj się w witrynie [Azure Portal][portal] i utwórz zasób usługi Application Insights, jak pokazano na następującej ilustracji. Kliknij przycisk „Nowy” > „Narzędzia programistyczne” > „Application Insights”. Zasób zawiera punkt końcowy do odbierania danych telemetrycznych, magazyn danych, zapisane raporty oraz pulpity nawigacyjne, konfigurację reguły i alertu oraz inne elementy.

![Tworzenie zasobu usługi App Insights](./media/app-insights-nodejs/03-new_appinsights_resource.png)

Na stronie tworzenia zasobu z listy rozwijanej typu aplikacji wybierz pozycję „Aplikacja Node.js”. Typ aplikacji określa domyślny zestaw tworzonych pulpitów nawigacyjnych i raportów. Nie należy się jednak martwić, ponieważ dowolny zasób usługi App Insights może w rzeczywistości zbierać dane dla dowolnego języka i platformy.

![Formularz nowego zasobu usługi App Insights](./media/app-insights-nodejs/04-create_appinsights_resource.png)

### <a name="agent"></a> Konfigurowanie agenta Node.js

Teraz można włączyć agenta do aplikacji, aby mógł zbierać dane.
Zacznij od skopiowania klucza instrumentacji Twojego zasobu (określanego dalej jako `ikey`) z portalu, jak pokazano poniżej. System App Insights używa tego klucza do mapowania danych na zasób platformy Azure, dlatego należy określić go w zmiennej środowiskowej lub w kodzie używanym przez agenta.  

![Kopiowanie klucza instrumentacji](./media/app-insights-nodejs/05-appinsights_ikey_portal.png)

Następnie dodaj bibliotekę agenta Node.js do zależności aplikacji przy użyciu pliku package.json. W folderze głównym aplikacji uruchom polecenie:

```bash
npm install applicationinsights --save
```

Teraz należy jawnie załadować bibliotekę w kodzie. Ponieważ agent wprowadza instrumentację do wielu innych bibliotek, należy załadować go możliwie jak najszybciej, nawet przed innymi instrukcjami `require`. Aby rozpocząć, na początku pierwszego pliku js dodaj następujący kod:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>");
appInsights.start();
```

Metoda `setup` konfiguruje klucz instrumentacji (a tym samym zasób platformy Azure) w taki sposób, aby był używany domyślnie dla wszystkich śledzonych elementów. Po zakończeniu konfiguracji wywołaj metodę `start`, aby rozpocząć zbieranie i wysyłanie danych telemetrycznych.

Klucz ikey można również udostępnić za pośrednictwem zmiennej środowiskowej APPINSIGHTS\_INSTRUMENTATIONKEY, zamiast przekazywać go ręcznie do metody `setup()` lub `getClient()`. Takie podejście umożliwia oddzielenie kluczy ikey od zatwierdzonego kodu źródłowego i określenie różnych kluczy ikey dla różnych środowisk.

Dodatkowe opcje konfiguracji opisano poniżej.

Agenta można przetestować bez wysyłania telemetrii. Wystarczy ustawić dla klucza instrumentacji dowolny niepusty ciąg tekstowy.

### <a name="monitor"></a> Monitorowanie aplikacji

Agent automatycznie zbiera dane telemetryczne ze środowiska uruchomieniowego Node.js i niektórych popularnych modułów innych firm. Użyj swojej aplikacji, aby teraz wygenerować niektóre z tych danych.

Następnie w witrynie [Azure Portal][portal] przejdź do utworzonego wcześniej zasobu usługi Application Insights i poszukaj kilku pierwszych punktów danych na osi czasu Przegląd, tak jak pokazano na poniższej ilustracji. Kliknij wykresy, aby uzyskać więcej szczegółów.

![Pierwsze punkty danych](./media/app-insights-nodejs/12-first-perf.png)

Kliknij przycisk Mapa aplikacji, aby wyświetlić wykrytą topologię aplikacji, tak jak to pokazano na poniższej ilustracji. Klikaj składniki na mapie, aby uzyskać więcej szczegółów.

![Mapa prostej aplikacji](./media/app-insights-nodejs/06-appinsights_appmap.png)

Użyj innych widoków dostępnych w sekcji „Zbadaj”, aby uzyskać więcej informacji o swojej aplikacji i rozwiązać ewentualne problemy.

![Sekcja Zbadaj](./media/app-insights-nodejs/07-appinsights_investigate_blades.png)

#### <a name="no-data"></a>Brak danych?

Agent przesyła dane partiami, dlatego niektóre elementy w portalu mogą być wyświetlane z opóźnieniem. Jeśli nie widzisz danych w swoim zasobie, wypróbuj następujące sposoby:

* Używaj aplikacji przez dłuższy czas. Wykonaj więcej akcji w celu wygenerowania większej ilości danych telemetrycznych.
* Kliknij przycisk **Odśwież** w widoku zasobu portalu. Wykresy są okresowo automatycznie odświeżane, ale kliknięcie tego przycisku wymusza natychmiastowe odświeżenie.
* Upewnij się, że [potrzebne porty wychodzące](app-insights-ip-addresses.md) są otwarte.
* Otwórz kafelek [Wyszukaj](app-insights-diagnostic-search.md) i poszukaj konkretnych zdarzeń.
* Zapoznaj się z sekcją [Często zadawane pytania][].


## <a name="agent-configuration"></a>Konfiguracja agenta

Poniżej wymieniono metody konfiguracji agenta i ich wartości domyślne.

Aby w pełni skorelować zdarzenia w usłudze, należy ustawić parametr `.setAutoDependencyCorrelation(true)`. Umożliwi to agentowi śledzenie kontekstu między asynchronicznymi wywołaniami zwrotnymi w środowisku Node.js.

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoDependencyCorrelation(false)
    .setAutoCollectRequests(true)
    .setAutoCollectPerformance(true)
    .setAutoCollectExceptions(true)
    .setAutoCollectDependencies(true)
    .start();
```

## <a name="agent-api"></a>Interfejs API agenta

<!-- TODO: Fully document agent API. -->

Interfejs API .NET agenta opisano szczegółowo [tutaj](app-insights-api-custom-events-metrics.md).

Klient Node.js usługi Application Insights umożliwia śledzenie dowolnego żądania, zdarzenia, metryki lub wyjątku. W poniższym przykładzie pokazano niektóre z dostępnych interfejsów API.

```javascript
let appInsights = require("applicationinsights");
appInsights.setup().start(); // assuming ikey in env var
let client = appInsights.getClient();

client.trackEvent("my custom event", {customProperty: "custom property value"});
client.trackException(new Error("handled exceptions can be logged with this method"));
client.trackMetric("custom metric", 3);
client.trackTrace("trace message");

let http = require("http");
http.createServer( (req, res) => {
  client.trackRequest(req, res); // Place at the beginning of your request handler
});
```

### <a name="track-your-dependencies"></a>Śledzenie zależności

```javascript
let appInsights = require("applicationinsights");
let client = appInsights.getClient();

var success = false;
let startTime = Date.now();
// execute dependency call here....
let duration = Date.now() - startTime;
success = true;

client.trackDependency("dependency name", "command name", duration, success);
```

### <a name="add-a-custom-property-to-all-events"></a>Dodawanie właściwości niestandardowych do wszystkich zdarzeń

```javascript
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};
```

### <a name="track-http-get-requests"></a>Śledzenie żądań HTTP GET

```javascript
var server = http.createServer((req, res) => {
    if ( req.method === "GET" ) {
            appInsights.client.trackRequest(req, res);
    }
    // other work here....
    res.end();
});
```

### <a name="track-server-startup-time"></a>Śledzenie czasu uruchamiania serwera

```javascript
let start = Date.now();
server.on("listening", () => {
    let duration = Date.now() - start;
    appInsights.client.trackMetric("server startup time", duration);
});
```

## <a name="more-resources"></a>Więcej zasobów

* [Monitorowanie telemetrii w portalu](app-insights-dashboards.md)
* [Zapisywanie zapytań analizy za pośrednictwem danych telemetrycznych](app-insights-analytics-tour.md)

<!--references-->

[portal]: https://portal.azure.com/
[Często zadawane pytania]: app-insights-troubleshoot-faq.md

