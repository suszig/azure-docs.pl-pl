---
title: "Monitoruj usługę Azure Functions"
description: "Dowiedz się, jak używać usługi Azure Application Insights w środowisku Azure Functions do wykonywania funkcji monitorowania."
services: functions
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: "usługa Azure Functions, funkcje, przetwarzanie zdarzeń, elementy webhook, obliczanie dynamiczne, architektura bez serwera"
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/15/2017
ms.author: tdykstra
ms.openlocfilehash: 355cb2cef52b5dfecddae228d0cc24a069d3b695
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-azure-functions"></a>Monitoruj usługę Azure Functions

## <a name="overview"></a>Omówienie 

[Środowisko Azure Functions](functions-overview.md) oferuje wbudowane funkcje integracji z [Azure Application Insights](../application-insights/app-insights-overview.md) dla funkcji monitorowania. W tym artykule przedstawiono sposób konfigurowania funkcji do wysyłania danych telemetrycznych do usługi Application Insights.

![Eksplorator metryk usługi Application Insights](media/functions-monitoring/metrics-explorer.png)

Funkcje ma wbudowaną funkcję monitorowania, który nie korzysta z usługi Application Insights. Firma Microsoft zaleca usługi Application Insights, ponieważ oferuje więcej danych i lepsze metody do analizowania danych. Informacje o monitorowaniu wbudowanych, zobacz [ostatniej sekcji tego artykułu](#monitoring-without-application-insights).

## <a name="enable-application-insights-integration"></a>Włącz integrację usługi Application Insights

Dla aplikacji funkcja do wysyłania danych do usługi Application Insights należy znać klucza Instrumentacji wystąpienia usługi Application Insights. Istnieją dwa sposoby dokonanie takiego połączenia w [portalu Azure](https://portal.azure.com):

* [Utwórz połączony wystąpienie usługi Application Insights, podczas tworzenia aplikacji funkcji](#new-function-app).
* [Połącz wystąpienie usługi Application Insights do istniejącej aplikacji funkcji](#existing-function-app).
 
### <a name="new-function-app"></a>Nowa aplikacja — funkcja

Włącz usługę Application Insights w aplikacji funkcji **Utwórz** strony:

1. Ustaw **usługi Application Insights** przełącznika **na**.

2. Wybierz **Application Insights lokalizacji**.

   ![Włącz usługę Application Insights podczas tworzenia aplikacji funkcji](media/functions-monitoring/enable-ai-new-function-app.png)

### <a name="existing-function-app"></a>Istniejących aplikacji — funkcja

Pobierz klucz Instrumentacji i zapisz go w aplikacji funkcji:

1. Utwórz wystąpienie usługi Application Insights. Ustaw typ aplikacji **ogólne**.

   ![Utwórz wystąpienie usługi Application Insights, wpisz ogólne](media/functions-monitoring/ai-general.png)

2. Kopiowanie klucza Instrumentacji z **Essentials** strony wystąpienia usługi Application Insights. Umieść kursor nad koniec wyświetlane wartości klucza do pobrania **kliknij, aby skopiować** przycisku.

   ![Skopiuj klucz Instrumentacji usługi Application Insights](media/functions-monitoring/copy-ai-key.png)

1. W aplikacji funkcji **ustawienia aplikacji** strony, [Dodaj ustawienie aplikacji](functions-how-to-use-azure-function-app-settings.md#settings) o nazwie APPINSIGHTS_INSTRUMENTATIONKEY i Wklej klucz instrumentacji.

   ![Dodaj klucz Instrumentacji do ustawień aplikacji](media/functions-monitoring/add-ai-key.png)

1. Kliknij pozycję **Zapisz**.

## <a name="view-telemetry-data"></a>Wyświetlanie danych telemetrii

Aby przejść do usługi Application Insights z funkcji aplikacji w portalu, wybierz **usługi Application Insights** łącze w aplikacji funkcji **omówienie** strony.

Aby uzyskać informacje o sposobie używania usługi Application Insights, zobacz [dokumentacji usługi Application Insights](https://docs.microsoft.com/azure/application-insights/). W tej sekcji przedstawiono kilka przykładów sposobu wyświetlania danych w usłudze Application Insights. Jeśli już znasz usługi Application Insights, można przejść bezpośrednio do [sekcje zawierają informacje dotyczące konfigurowania i dostosowywania danych telemetrycznych](#configure-categories-and-log-levels).

W [Eksploratora metryk](../application-insights/app-insights-metrics-explorer.md), można utworzyć wykresów i alerty na podstawie metryk takie jak liczba wywołań funkcji, czas wykonywania i Częstotliwość powodzeń.

![Eksploratora metryk](media/functions-monitoring/metrics-explorer.png)

Na [błędów](../application-insights/app-insights-asp-net-exceptions.md) kartę, można utworzyć wykresów i alerty na podstawie błędy funkcji i serwera wyjątki. **Nazwy operacji** jest nazwą funkcji. Błędy w zależności nie są wyświetlane, chyba że zaimplementowaniem [telemetria niestandardowa](#custom-telemetry-in-c-functions) zależności.

![błędy](media/functions-monitoring/failures.png)

Na [wydajności](../application-insights/app-insights-performance-counters.md) kartę, można analizować problemy z wydajnością.

![Wydajność](media/functions-monitoring/performance.png)

**Serwerów** karcie są wyświetlane wykorzystania zasobów i przepływności na serwer. Może to być przydatne w przypadku debugowania scenariuszy, w którym funkcje są bogging zasobami podstawowej w dół. Serwery są określane jako *wystąpień roli w chmurze*. 

![Serwery](media/functions-monitoring/servers.png)

[Strumień na żywo metryki](../application-insights/app-insights-live-stream.md) karcie są wyświetlane dane metryk jako jest tworzony w czasie rzeczywistym.

![Transmisja strumieniowa na żywo](media/functions-monitoring/live-stream.png)

## <a name="query-telemetry-data"></a>Dane telemetryczne zapytania

[Application Insights Analytics](../application-insights/app-insights-analytics.md) zapewnia dostęp do wszystkich danych telemetrycznych w formie tabel w bazie danych. Analytics zapewnia język kwerendy do wyodrębniania i manipulowania danymi.

![Wybierz analityka](media/functions-monitoring/select-analytics.png)

![Przykład analityka](media/functions-monitoring/analytics-traces.png)

Oto przykład kwerendy. Ta pokazuje rozkład żądania na proces roboczy w ciągu ostatnich 30 minut.

```
requests
| where timestamp > ago(30m) 
| summarize count() by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```

Tabele, które są dostępne są wyświetlane w **schematu** okienka po lewej stronie. Można znaleźć dane generowane przez wywołania funkcji w poniższych tabelach:

* **ślady** — Dzienniki tworzone przez środowisko uruchomieniowe i kodu funkcji.
* **żądania** — po jednej dla każdego wywołania funkcji.
* **Wyjątki** — wszelkie wyjątki zgłaszane przez środowisko uruchomieniowe.
* **customMetrics** — liczba pomyślnych i niepowodzenia wywołań, Częstotliwość powodzeń, czas trwania.
* **customEvents** -zdarzeń śledzenia w czasie wykonywania, na przykład: żądania HTTP, które mogą powodować funkcji.
* **liczniki wydajności** — informacje o wydajności serwerów, które funkcje są uruchomione na.

Inne tabele są przeznaczone dla testów dostępności i dane telemetryczne/w przeglądarce klienta. Można zaimplementować telemetria niestandardowa umożliwiające dodawanie danych do nich.

W każdej tabeli, niektóre dane specyficzne dla funkcji znajduje się w `customDimensions` pola.  Na przykład poniższe zapytanie pobiera wszystkie dane śledzenia, które mają poziom dziennika `Error`.

```
traces 
| where customDimensions.LogLevel == "Error"
```

Udostępnia środowisko uruchomieniowe `customDimensions.LogLevel` i `customDimensions.Category`. Możesz podać dodatkowe pola w dziennikach, zostanie zapisany w kodzie funkcji. Zobacz [strukturę rejestrowania](#structured-logging) dalszej części tego artykułu.

## <a name="configure-categories-and-log-levels"></a>Konfigurowanie kategorii i poziomu dziennika

Można użyć usługi Application Insights bez żadnych niestandardowych konfiguracji, ale konfiguracja domyślna może powodować dużej ilości danych. Jeśli używasz subskrypcji programu Visual Studio Azure może osiągnęła zakończenia Twoje dane aplikacji szczegółowe informacje o. W dalszej części tego artykułu pokazano, jak skonfigurować i dostosować dane funkcji wysyłania do usługi Application Insights.

### <a name="categories"></a>Kategorie

Rejestrator usługi Azure Functions zawiera *kategorii* dla każdego dziennika. Kategoria wskazuje część kod środowiska uruchomieniowego lub kodu funkcji zapisano dziennika. 

Środowisko uruchomieniowe Functions tworzy dzienniki, które mają kategorii począwszy "Host". Na przykład "Funkcja uruchomiona," "wykonać funkcji" i "Funkcja zakończona" dzienniki ma kategorii "Host.Executor". 

Jeśli zapisują dzienniki w kodzie funkcji, ich kategorii jest "Function".

### <a name="log-levels"></a>Poziomy dziennika

Zawiera również usługę Azure functions rejestratora *poziom dziennika* przy każdym logowaniu. [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel#Microsoft_Extensions_Logging_LogLevel) jest wyliczeniem i kod całkowitą wskazuje znaczenie:

|LogLevel    |Kod|
|------------|---|
|Ślad       | 0 |
|Debugowanie       | 1 |
|Informacje | 2 |
|Ostrzeżenie     | 3 |
|Błąd       | 4 |
|Krytyczne    | 5 |
|Brak        | 6 |

Poziom dziennika `None` znajduje się w następnej sekcji. 

### <a name="configure-logging-in-hostjson"></a>Konfiguruj rejestrowanie w host.json

*Host.json* pliku określa poziom rejestrowania aplikacji funkcji wysyła do usługi Application Insights. Dla każdej kategorii możesz zaznaczyć poziom dziennika minimalna do wysłania. Oto przykład:

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host.Results": "Error",
        "Function": "Error",
        "Host.Aggregator": "Information"
      }
    }
  }
}
```

Ten przykład konfiguruje następujące reguły:

1. W przypadku dzienników z kategorii "Host.Results" lub "Function" Wyślij tylko `Error` poziom i powyżej do usługi Application Insights. W dziennikach `Information` poziomie i poniżej są ignorowane.
2. W dziennikach kategorii hosta. Agregator, Wyślij tylko `Information` poziom lub nowszego z usługą Application Insights. W dziennikach `Debug` poziomie i poniżej są ignorowane.
3. W przypadku innych dzienników wysłać tylko `Information` poziom i powyżej do usługi Application Insights.

Wartość kategorii *host.json* kontroluje rejestrowanie dla wszystkich kategorii, które zaczynają się taką samą wartość. Na przykład "Host" w *host.json* kontroluje rejestrowanie "Host.General", "Host.Executor", "Host.Results" i tak dalej.

Jeśli *host.json* zawiera wiele kategorii rozpoczynających się od tych samych parametrach dłużej te są stosowane. Na przykład, załóżmy, że ma od środowiska uruchomieniowego, z wyjątkiem "Host.Aggregator", aby zalogować się na `Error` poziomu podczas "Host.Aggregator" dzienniki na `Information` poziomu:

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host": "Error",
        "Function": "Error",
        "Host.Aggregator": "Information"
      }
    }
  }
}
```

Aby pominąć wszystkie dzienniki dla kategorii, można użyć poziom dziennika `None`. Dzienniki nie są zapisywane z tej kategorii i nie ma żadnych poziom dziennika powyżej.

W poniższych sekcjach opisano głównych kategorii dzienników, które tworzy środowiska uruchomieniowego. 

### <a name="category-hostresults"></a>Host.Results kategorii

Te dzienniki są wyświetlane jako "żądań" w usłudze Application Insights. Wskazują one powodzenie lub Niepowodzenie funkcji.

![Wykres żądań](media/functions-monitoring/requests-chart.png)

Wszystkie te dzienniki są zapisywane w `Information` poziomu, tak więc jeśli filtru u `Warning` lub nowszym, nie będą widzieć żadnego z tych danych.

### <a name="category-hostaggregator"></a>Host.Aggregator kategorii

Dzienniki te zapewniają liczby i średnie wywołania funkcji za pośrednictwem [można skonfigurować](#configure-the-aggregator) okresu czasu. Domyślny okres to 30 sekund lub wyników 1000, zależnie od zostanie osiągnięty jako pierwszy. 

Dzienniki są wyświetlane jako "customMetrics" w usłudze Application Insights. Przykłady to liczba uruchomień, Częstotliwość powodzeń i czasu trwania.

![customMetrics zapytania](media/functions-monitoring/custom-metrics-query.png)

Wszystkie te dzienniki są zapisywane w `Information` poziomu, tak więc jeśli filtru u `Warning` lub nowszym, nie będą widzieć żadnego z tych danych.

### <a name="other-categories"></a>Inne kategorie

Wszystkie dzienniki dla kategorii innych niż już na liście Pokaż "śladów" w usłudze Application Insights.

![ślady zapytania](media/functions-monitoring/analytics-traces.png)

Wszystkie dzienniki z kategorii, które zaczynają się od "Host" są zapisywane w czasie wykonywania funkcji. "Funkcja uruchomić" i "Funkcja zakończona" dzienniki ma kategorii "Host.Executor". Uruchamia się pomyślnie, te dzienniki są `Information` poziomu; wyjątków są rejestrowane w `Error` poziom. Środowisko uruchomieniowe tworzy także `Warning` poziomu dzienników, na przykład: wysłanych do kolejki skażone wiadomości w kolejce.

Dzienniki zapisywane w kodzie funkcji ma kategorii "Funkcji" i może być dowolny poziom dziennika.

## <a name="configure-the-aggregator"></a>Skonfiguruj agregatora

Jak wspomniano w poprzedniej sekcji, środowisko uruchomieniowe agreguje dane dotyczące wykonania funkcji w danym okresie czasu. Domyślny okres to 30 sekund lub uruchamia 1000, zależnie od miało miejsce wcześniej. Można skonfigurować tego ustawienia w *host.json* pliku.  Oto przykład:

```json
{
    "aggregator": {
      "batchSize": 1000,
      "flushTimeout": "00:00:30"
    }
}
```

## <a name="configure-sampling"></a>Skonfiguruj pobierania próbek

Usługa Application Insights ma [próbkowania](../application-insights/app-insights-sampling.md) funkcja, która może chronić od wytwarzania zbyt dużej ilości danych telemetrycznych w czasie z obciążenia szczytowego. Liczba elementów danych telemetrycznych przekracza określonej wartości, usługi Application Insights uruchamia losowo zignorowanie niektórych elementów przychodzących. Można skonfigurować próbkowania w *host.json*.  Oto przykład:

```json
{
  "applicationInsights": {
    "sampling": {
      "isEnabled": true,
      "maxTelemetryItemsPerSecond" : 5
    }
  }
}
```

## <a name="write-logs-in-c-functions"></a>Zapisywanie dzienników w funkcjach języka C#

Dzienniki można pisać w kodzie funkcji, które są wyświetlane jako dane śledzenia w usłudze Application Insights.

### <a name="ilogger"></a>ILogger

Użyj [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) parametru w funkcji zamiast `TraceWriter` parametru. Dzienniki utworzone za pomocą `TraceWriter` przejdź do usługi Application Insights, ale `ILogger` umożliwia [strukturę rejestrowania](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Z `ILogger` obiekt, należy wywołać `Log<level>` [metody rozszerzenia na ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions#Methods_) tworzyć dzienniki. Na przykład poniższy kod zapisy `Information` dzienniki z kategorią "Function".

```cs
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger logger)
{
    logger.LogInformation("Request for item with key={itemKey}.", id);
```

### <a name="structured-logging"></a>Rejestrowanie strukturalnych

Kolejność symboli zastępczych, a nie ich nazw, określa, które parametry są używane w komunikacie dziennika. Załóżmy na przykład następujący kod:

```csharp
string partitionKey = "partitionKey";
string rowKey = "rowKey";
logger.LogInformation("partitionKey={partitionKey}, rowKey={rowKey}", partitionKey, rowKey);
```

Jeśli zachować ten sam ciąg komunikatu i odwrotna kolejność parametrów, wynikowy tekst komunikatu musi wartości w niewłaściwej miejscach.

Symbole zastępcze są obsługiwane w ten sposób, dzięki czemu można wykonać rejestrowania strukturalnych. Usługa Application Insights przechowuje pary nazwa wartość parametru oprócz ciąg z komunikatem. Wynik jest argumenty komunikatów pola, które można wykonać zapytanie na.

Na przykład, jeśli wywołania metody Rejestrator wygląda tak jak w poprzednim przykładzie, możesz zapytania dotyczącego pola `customDimensions.prop__rowKey`. Prefiks jest dodawany do zapewnienia, że nie istnieją żadne kolizji między pola, które dodaje środowiska uruchomieniowego i pola, które dodaje kodu funkcji.

Możesz także zbadać na oryginalny ciąg z komunikatem, umieszczając odwołanie do pola `customDimensions.prop__{OriginalFormat}`.  

Oto przykładowe JSON reprezentację `customDimensions` danych:

```json
{
  customDimensions: {
    "prop__{OriginalFormat}":"C# Queue trigger function processed: {message}",
    "Category":"Function",
    "LogLevel":"Information",
    "prop__message":"c9519cbf-b1e6-4b9b-bf24-cb7d10b1bb89"
  }
}
```

### <a name="logging-custom-metrics"></a>Rejestrowanie metryki niestandardowe  

W języku C# skrypt funkcji, można użyć `LogMetric` — metoda rozszerzenia na `ILogger` do tworzenia metryki niestandardowe w usłudze Application Insights. Oto przykładowe wywołanie metody:

```csharp
logger.LogMetric("TestMetric", 1234); 
```

Ten kod jest to alternatywa dla wywołania `TrackMetric` przy użyciu [aplikacji API Insights dla platformy .NET](#custom-telemetry-in-c-functions).

## <a name="write-logs-in-javascript-functions"></a>Zapisywanie dzienników w funkcji JavaScript

W przypadku funkcji Node.js użyj `context.log` zapisywanie dzienników. Nie włączono rejestrowania strukturalnych.

```
context.log('JavaScript HTTP trigger function processed a request.' + context.invocationId);
```

### <a name="logging-custom-metrics"></a>Rejestrowanie metryki niestandardowe  

W funkcji Node.js, można użyć `context.log.metric` metodę w celu utworzenia metryki niestandardowe w usłudze Application Insights. Oto przykładowe wywołanie metody:

```javascript
context.log.metric("TestMetric", 1234); 
```

Ten kod jest to alternatywa dla wywołania `trackMetric` przy użyciu [zestaw Node.js SDK dla usługi Application Insights](#custom-telemetry-in-javascript-functions).

## <a name="custom-telemetry-in-c-functions"></a>Telemetria niestandardowa w funkcjach języka C#

Można użyć [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) pakiet NuGet w celu wysyłania danych telemetrycznych niestandardowych do usługi Application Insights.

Poniżej przedstawiono przykładowy kod C#, który używa [niestandardowego interfejsu API telemetrii](../application-insights/app-insights-api-custom-events-metrics.md). Przykładem jest biblioteki klasy .NET, ale kodu usługi Application Insights jest taki sam dla skryptu C#.

```cs
using System;
using System.Net;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.Azure.WebJobs;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;
using System.Linq;

namespace functionapp0915
{
    public static class HttpTrigger2
    {
        private static string key = TelemetryConfiguration.Active.InstrumentationKey = 
            System.Environment.GetEnvironmentVariable(
                "APPINSIGHTS_INSTRUMENTATIONKEY", EnvironmentVariableTarget.Process);

        private static TelemetryClient telemetry = 
            new TelemetryClient() { InstrumentationKey = key };

        [FunctionName("HttpTrigger2")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
            HttpRequestMessage req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // parse query parameter
            string name = req.GetQueryNameValuePairs()
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Get request body
            dynamic data = await req.Content.ReadAsAsync<object>();

            // Set name to query string or body data
            name = name ?? data?.name;

            telemetry.Context.Operation.Id = context.InvocationId.ToString();
            telemetry.Context.Operation.Name = "cs-http";
            if (!String.IsNullOrEmpty(name))
            {
                telemetry.Context.User.Id = name;
            }
            telemetry.TrackEvent("Function called");
            telemetry.TrackMetric("Test Metric", DateTime.Now.Millisecond);
            telemetry.TrackDependency("Test Dependency", 
                "swapi.co/api/planets/1/", 
                start, DateTime.UtcNow - start, true);

            return name == null
                ? req.CreateResponse(HttpStatusCode.BadRequest, 
                    "Please pass a name on the query string or in the request body")
                : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
        }
    }
}
```

Nie wywołuj `TrackRequest` lub `StartOperation<RequestTelemetry>`, ponieważ zobaczysz zduplikowane żądania dla wywołania funkcji.  Środowisko uruchomieniowe Functions automatyczne śledzenie żądań.

Ustaw `telemetry.Context.Operation.Id` aby można było wywołać identyfikator każdego uruchomienia funkcji. Umożliwia służące do skorelowania wszystkie elementy danych telemetrycznych dla wywołania danej funkcji.

```cs
telemetry.Context.Operation.Id = context.InvocationId.ToString();
```

## <a name="custom-telemetry-in-javascript-functions"></a>Telemetria niestandardowa w funkcji JavaScript

[Zestaw Node.js SDK usługi Application Insights](https://www.npmjs.com/package/applicationinsights) jest obecnie w wersji beta. Oto niektóre przykładowy kod, który wysyła dane telemetryczne niestandardowych do usługi Application Insights:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup();
const client = appInsights.defaultClient;

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    client.trackEvent({name: "my custom event", tagOverrides:{"ai.operation.id": context.invocationId}, properties: {customProperty2: "custom property value"}});
    client.trackException({exception: new Error("handled exceptions can be logged with this method"), tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackMetric({name: "custom metric", value: 3, tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackTrace({message: "trace message", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackDependency({target:"http://dbname", name:"select customers proc", data:"SELECT * FROM Customers", duration:231, resultCode:0, success: true, dependencyTypeName: "ZSQL", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackRequest({name:"GET /customers", url:"http://myserver/customers", duration:309, resultCode:200, success:true, tagOverrides:{"ai.operation.id": context.invocationId}});

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

`tagOverrides` Zestawów parametrów `operation_Id` identyfikatora wywołania funkcji To ustawienie umożliwia korelowanie wszystkie dane telemetryczne generowane automatycznie i niestandardowych dla wywołania funkcji danego.

## <a name="known-issues"></a>Znane problemy

### <a name="dependencies"></a>Zależności

Zależności nie pojawiają się automatycznie, ale można napisać kod niestandardowy w celu wyświetlania zależności. Przykładowy kod [C# telemetria niestandardowa sekcji](#custom-telemetry-in-c-functions) przedstawia sposób. Przykładowy kod powoduje *mapowanie aplikacji* w usłudze Application Insights, że wygląda podobnie do następującej:

![Mapa aplikacji](media/functions-monitoring/app-map.png)

### <a name="report-issues"></a>Raport problemów

Aby zgłosić problem w integracji usługi Application Insights w funkcjach lub sugestię lub żądania, [Utwórz problem w usłudze GitHub](https://github.com/Azure/Azure-Functions/issues/new).

## <a name="monitoring-without-application-insights"></a>Monitorowanie bez usługi Application Insights

Zalecamy usługi Application Insights dla funkcji monitorowania, ponieważ oferuje więcej danych i lepsze metody do analizowania danych. Jednak możesz także znaleźć dane telemetryczne i rejestrowanie danych na stronach portalu platformy Azure dla aplikacji funkcja. 

Wybierz **Monitor** kartę dla funkcji i można uzyskać listę wykonaniami funkcji. Wybierz wykonywania funkcji, aby sprawdzić czas trwania, danych wejściowych, błędy i skojarzone pliki dziennika.

> [!IMPORTANT]
> Korzystając z [zużycie plan hostingu](functions-overview.md#pricing) dla usługi Azure Functions **monitorowanie** kafelka w aplikacji funkcji nie są wyświetlane wszystkie dane. Jest to spowodowane platformę dynamicznie skaluje i zarządza wystąpienia obliczeniowe. Te metryki nie jest zrozumiały dla planu zużycia.

### <a name="real-time-monitoring"></a>Monitorowanie w czasie rzeczywistym

Monitorowanie w czasie rzeczywistym będą dostępne po kliknięciu **strumień na żywo zdarzeń** w funkcji **Monitor** kartę. Strumień na żywo zdarzeń jest wyświetlane na wykresie na nowej karcie w przeglądarce

> [!NOTE]
> Jest to znany problem, który może powodować danych nie można wypełnić. Konieczne może być zamknięcie karty przeglądarki zawierające strumień na żywo zdarzeń, a następnie kliknij przycisk **strumień na żywo zdarzeń** ponownie, aby zezwalała na poprawnie wypełnić danych strumienia zdarzeń. 

Te statystyki są w czasie rzeczywistym, ale rzeczywisty Tworzenie wykresów danych wykonawczych mogą mieć około 10 sekund opóźnienia.

### <a name="monitor-log-files-from-a-command-line"></a>Monitor plików dziennika z wiersza polecenia

Można przesłać strumieniowo pliki dziennika do sesji wiersza polecenia na lokalnej stacji roboczej za pomocą interfejsu wiersza polecenia platformy Azure (CLI) 1.0 lub programu PowerShell.

### <a name="monitor-function-app-log-files-with-the-azure-cli-10"></a>Monitorowanie plików dziennika aplikacji funkcji z interfejsu wiersza polecenia platformy Azure w wersji 1.0

Aby rozpocząć, [zainstalować 1.0 interfejsu wiersza polecenia Azure](../cli-install-nodejs.md) i [logowanie do platformy Azure](../xplat-cli-connect.md).

Aby włączyć klasycznym trybie zarządzania usługami, wybierz subskrypcję i przesyłanie strumieniowe plików dziennika, użyj następujących poleceń:

```
azure config mode asm
azure account list
azure account set <subscriptionNameOrId>
azure site log tail -v <function app name>
```

### <a name="monitor-function-app-log-files-with-powershell"></a>Monitorowanie plików dziennika aplikacji funkcji przy użyciu programu PowerShell

Aby rozpocząć, [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview).

Aby dodać konta platformy Azure, wybierz subskrypcję i przesyłanie strumieniowe plików dziennika, użyj następujących poleceń:

```
PS C:\> Add-AzureAccount
PS C:\> Get-AzureSubscription
PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription
PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail
```

Aby uzyskać więcej informacji, zobacz [porady: strumienia dzienników aplikacji sieci web](../app-service/web-sites-enable-diagnostic-log.md#streamlogs). 

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Dowiedz się więcej na temat usługi Application Insights](https://docs.microsoft.com/azure/application-insights/)

> [!div class="nextstepaction"]
> [Dowiedz się więcej na temat struktury rejestrowania, który używa funkcji](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)
