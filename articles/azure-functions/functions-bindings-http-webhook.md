---
title: "Azure powiązania HTTP funkcje i elementu webhook | Dokumentacja firmy Microsoft"
description: "Zrozumienie, jak używać protokołu HTTP i elementu webhook wyzwalaczy i powiązań w usługi Azure Functions."
services: functions
documentationcenter: na
author: mattchenderson
manager: cfowler
editor: 
tags: 
keywords: "funkcje platformy Azure, funkcje, zdarzenia przetwarzania elementów webhook, dynamicznych zasobów obliczeniowych, API niekorzystającą architektury, protokołu HTTP, REST"
ms.assetid: 2b12200d-63d8-4ec1-9da8-39831d5a51b1
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/26/2017
ms.author: mahender
ms.openlocfilehash: 3c3247592cbe2bc382d220264b0c646ee566b8a7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-http-and-webhook-bindings"></a>Azure powiązania HTTP funkcje i elementu webhook
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule opisano sposób konfigurowania i pracować z HTTP wyzwalaczy i powiązań w usługi Azure Functions.
Z tych opcji możesz użyć funkcji Azure kompilacji niekorzystającą interfejsów API i reagowanie na elementów webhook.

Środowisko Azure Functions zapewnia następujące powiązania:
- [Wyzwalacza HTTP](#httptrigger) umożliwia wywołanie funkcji z żądania HTTP. To można dostosować, aby odpowiadać na [elementów webhook](#hooktrigger).
- [Powiązania wyjściowego HTTP](#output) pozwala odpowiedzieć na żądanie.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a>Wyzwalacz HTTP
Wyzwalacza HTTP wykonuje funkcji w odpowiedzi na żądania HTTP. Można dostosować go do odpowiedzi z określonego adresu URL lub zestaw metod HTTP. Wyzwalacz HTTP można skonfigurować w taki sposób, aby odpowiedzieć na elementów webhook. 

Jeśli za pomocą portalu funkcji, można również rozpocząć od razu przy użyciu predefiniowanych szablonów. Wybierz **nową funkcję** i wybierz polecenie "Interfejsu API i elementów Webhook" z **scenariusza** listy rozwijanej. Wybierz jeden z szablonów i kliknij przycisk **Utwórz**.

Domyślnie wyzwalacza HTTP odpowiada na żądania z kodem stanu HTTP 200 OK i pustej treści. Aby zmodyfikować odpowiedzi, należy skonfigurować [powiązania wyjściowego HTTP](#output)

### <a name="configuring-an-http-trigger"></a>Konfigurowanie wyzwalacza HTTP
Wyzwalacz protokołu HTTP jest zdefiniowane przez obiekt JSON w `bindings` tablicy function.json, jak pokazano w poniższym przykładzie:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function",
    "methods": [ "get" ],
    "route": "values/{id}"
},
```
Wiązanie obsługuje następujące właściwości:

|Właściwość  |Opis  |
|---------|---------|
| **Nazwa** | Wymagana — nazwa zmiennej używane w kodzie funkcji żądania lub treści żądania. Zobacz [Praca z wyzwalacza HTTP z kodu](#httptriggerusage). |
| **Typ** | Wymagana — musi być ustawiona `httpTrigger`. |
| **Kierunek** | Wymagana — musi być ustawiona `in`. |
| **authLevel** | Określa, jakie klucze, jeśli taki występuje, musi być obecny na żądanie, aby można było wywołać funkcję. Wartość może być jedną z następujących wartości: <ul><li><code>anonymous</code>&mdash;Brak klucza interfejsu API jest wymagana.</li><li><code>function</code>&mdash;Wymagany jest klucz interfejsu API właściwe dla funkcji. Jeśli nie zostanie podana jest wartość domyślna.</li><li><code>admin</code>&mdash;Klucz główny jest wymagany.</li></ul> Aby uzyskać więcej informacji, zobacz [Praca z kluczami](#keys). |
| **metody** | Tablica metod HTTP, na które odpowiada funkcji. Jeśli nie zostanie określony, funkcja odpowiada na wszystkich metod HTTP. Zobacz [Dostosowywanie punkt końcowy HTTP](#url). |
| **trasy** | Określa szablon trasy, kontrolowanie, do której żądanie odpowiada funkcji adresów URL. Jeśli nie zostanie podana wartość domyślna to `<functionname>`. Aby uzyskać więcej informacji, zobacz [Dostosowywanie punkt końcowy HTTP](#url). |
| **webHookType** | Konfiguruje wyzwalacz protokołu HTTP do działania jako odbiornik elementu webhook dla określonego dostawcy. Nie używaj _metody_ właściwości przy użyciu tego ustawienia. Wartość może być jedną z następujących wartości:<ul><li><code>genericJson</code>&mdash;Punkt końcowy elementu webhook ogólnego przeznaczenia bez logiki dla określonego dostawcy.</li><li><code>github</code>&mdash;Funkcja odpowiada elementów webhook GitHub. Nie używaj _authLevel_ właściwości przy użyciu tej wartości.</li><li><code>slack</code>&mdash;Funkcja odpowiada elementów webhook Slack. Nie używaj _authLevel_ właściwości przy użyciu tej wartości.</li></ul> Aby uzyskać więcej informacji, zobacz [odpowiada na żądania elementów webhook](#hooktrigger). |

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a>Praca z wyzwalacza HTTP z kodu
Dla funkcji języka C# i F #, mogą zadeklarować typ danych wejściowych musi mieć przypisany wyzwalacz `HttpRequestMessage` lub niestandardowy typ architektury .NET. Jeśli wybierzesz `HttpRequestMessage`, możesz uzyskać dostęp do obiektu żądania. Niestandardowy typ architektury .NET funkcje próbuje przeanalizować treść żądania JSON można ustawić właściwości obiektu. 

W przypadku funkcji Node.js środowisko uruchomieniowe Functions zapewnia treści żądania, zamiast obiektu żądania. Aby uzyskać więcej informacji, zobacz [przykłady wyzwalacza HTTP](#httptriggersample).


<a name="output"></a>
## <a name="http-response-output-binding"></a>Powiązania wyjściowego odpowiedzi HTTP
Za pomocą raportu HTTP powiązanie odpowiedź do nadawcy żądania HTTP. To powiązanie wymaga wyzwalacza HTTP i umożliwia dostosowanie odpowiedzi skojarzony z żądaniem tego wyzwalacza. Jeśli powiązania wyjściowego HTTP nie zostanie podany, wyzwalacz HTTP zwraca HTTP 200 OK o pustej treści. 

### <a name="configuring-an-http-output-binding"></a>Konfigurowanie protokołu HTTP powiązania wyjściowego
Powiązania wyjściowego HTTP jest zdefiniowany w obiekcie JSON w `bindings` tablicy function.json, jak pokazano w poniższym przykładzie:

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
Wiązanie obsługuje następujących wymaganych właściwości:

|Właściwość  |Opis  |
|---------|---------|
|**Nazwa** | Nazwa zmiennej używane w kodzie funkcji dla odpowiedzi. Zobacz [powiązania z kodu wyjściowego roboczych protokołu HTTP z](#outputusage). |
| **Typ** |należy wybrać opcję `http`. |
| **Kierunek** | należy wybrać opcję `out`. |

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a>Praca z HTTP output powiązania z kodu
Można użyć parametru wyjściowego odpowiadać obiektowi wywołującemu, http lub elementu webhook. Można również użyć wzorców odpowiedzi standard języka. Na przykład odpowiedzi, zobacz [przykłady wyzwalacza HTTP](#httptriggersample) i [przykłady wyzwalacza elementu Webhook](#hooktriggersample).


<a name="hooktrigger"></a>
## <a name="responding-to-webhooks"></a>Odpowiada na żądania elementów webhook
Wyzwalacz protokołu HTTP z _webHookType_ właściwości jest skonfigurowane do odpowiedzi na [elementów webhook](https://en.wikipedia.org/wiki/Webhook). Konfiguracja podstawowa używana do określania "genericJson". To ogranicza żądania tylko do tych przy użyciu protokołu HTTP POST i z `application/json` typ zawartości.

Wyzwalacz umożliwia również dostosowanie dostawcy określonego elementu webhook, takich jak [GitHub](https://developer.github.com/webhooks/) lub [Slack](https://api.slack.com/outgoing-webhooks). Jeśli nie określono dostawcy, środowisko uruchomieniowe Functions może obsługiwać logikę weryfikacji dostawcy dla Ciebie.  

### <a name="configuring-github-as-a-webhook-provider"></a>Konfigurowanie dostawcy elementu webhook GitHub
Aby odpowiedzieć elementów webhook GitHub, najpierw utwórz funkcji z wyzwalaczem HTTP i ustaw **webHookType** właściwości `github`. Następnie skopiuj jej [adres URL](#url) i [klucz interfejsu API](#keys) do **Dodawanie elementu webhook** strony repozytorium GitHub. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

Na przykład zobacz [Utwórz funkcję wyzwalane przez GitHub webhook](functions-create-github-webhook-triggered-function.md).

### <a name="configuring-slack-as-a-webhook-provider"></a>Konfigurowanie zapas czasu jako dostawca elementu webhook
Slack elementu webhook generuje token dla Ciebie, zamiast czekać na określeniu, dlatego należy skonfigurować klucz właściwe dla funkcji przy użyciu tokenu z zapas czasu. Zobacz [Praca z kluczami](#keys).

<a name="url"></a>
## <a name="customizing-the-http-endpoint"></a>Dostosowywanie punkt końcowy HTTP
Domyślnie podczas tworzenia funkcja wyzwalacza HTTP lub elementu WebHook, funkcja jest adresowanego trasa formularza:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Można dostosować tej trasy przy użyciu opcjonalnego `route` wejściowych powiązania właściwości wyzwalacza HTTP. Na przykład następujące *function.json* plik definiuje `route` właściwości dla wyzwalacza HTTP:

```json
{
    "bindings": [
    {
        "type": "httpTrigger",
        "name": "req",
        "direction": "in",
        "methods": [ "get" ],
        "route": "products/{category:alpha}/{id:int?}"
    },
    {
        "type": "http",
        "name": "res",
        "direction": "out"
    }
    ]
}
```

Za pomocą tej konfiguracji, funkcja jest teraz adresowanego trasa następujących zamiast oryginalnej trasy.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

Dzięki temu kod funkcji do obsługi dwóch parametrów w adresie _kategorii_ i _identyfikator_. Można użyć dowolnego [ograniczenia trasy interfejsu API sieci Web](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) z parametrami. Poniższy kod funkcji języka C# korzysta z obu tych parametrów.

```csharp
public static Task<HttpResponseMessage> Run(HttpRequestMessage req, string category, int? id, 
                                                TraceWriter log)
{
    if (id == null)
        return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
    else
        return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
}
```

Oto kod funkcji Node.js do korzystania z tego samego parametrów trasy.

```javascript
module.exports = function (context, req) {

    var category = context.bindingData.category;
    var id = context.bindingData.id;

    if (!id) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "All " + category + " items were requested."
        };
    }
    else {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: category + " item with id = " + id + " was requested."
        };
    }

    context.done();
} 
```

Domyślnie wszystkie trasy funkcji są poprzedzane prefiksem *interfejsu api*. Można również dostosować lub usunąć przy użyciu prefiksu `http.routePrefix` właściwości w Twojej *host.json* pliku. Poniższy przykład umożliwia usunięcie *interfejsu api* prefiks trasy przy użyciu pustego ciągu dla prefiksu w *host.json* pliku.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

Aby uzyskać szczegółowe informacje na temat aktualizowania *host.json* plików dla funkcji, zobacz [jak zaktualizować pliki aplikacji funkcji](functions-reference.md#fileupdate). 

Aby uzyskać informacje na temat innych właściwości można skonfigurować w sieci *host.json* plików, zobacz [odwołania host.json](functions-host-json.md).


<a name="keys"></a>
## <a name="working-with-keys"></a>Praca z kluczy
Wyzwalacze HTTP pozwalają używać kluczy w celu zwiększenia bezpieczeństwa. Standardowa wyzwalacza HTTP można użyć je jako klucz interfejsu API wymagające klucza, który ma być obecne w żądaniu. Elementów Webhook można używać kluczy do autoryzowania żądań na różne sposoby, w zależności od tego, czy dostawca obsługuje.

Klucze są przechowywane w ramach funkcji aplikacji na platformie Azure i są szyfrowane, gdy. Aby wyświetlić klucze, tworzenie nowych, lub Przywróć do nowych wartości kluczy, przejdź do jednej z funkcji w portalu i wybierz "Manage". 

Istnieją dwa typy kluczy:
- **Klucze hosta**: klucze te są współużytkowane przez wszystkie funkcje w ramach funkcji aplikacji. Gdy jest używany jako klucz interfejsu API, umożliwiają one dostęp do dowolnej funkcji w funkcji aplikacji.
- **Klawisze funkcyjne**: te klucze mają zastosowanie tylko do określonych funkcji, w których są zdefiniowane. Gdy jest używany jako klucz interfejsu API, te tylko zezwolić na dostęp do tej funkcji.

Każdy klucz nosi nazwę odwołania, a jest domyślny klucz (o nazwie "domyślne") na poziomie funkcji i hosta. **Klucza głównego** jest domyślny klucz hosta o nazwie "_master", który jest zdefiniowany dla każdej funkcji aplikacji. Ten klucz nie może zostać odwołany. Zapewnia dostęp administracyjny do interfejsów API środowiska wykonawczego. Przy użyciu `"authLevel": "admin"` w powiązaniu JSON wymaga tego klucza, aby być prezentowana na żądanie; innego klucza powoduje błąd autoryzacji.

> [!IMPORTANT]  
> Z powodu podwyższonym poziomem uprawnień przyznanych przez klucz główny nie należy udostępnić ten klucz osobom trzecim ani rozpowszechnienie go w aplikacjach klienckich natywnego. Należy zachować ostrożność, wybierając poziom dostępu administratora.

### <a name="api-key-authorization"></a>Autoryzacji klucza interfejsu API
Domyślnie wyzwalacza HTTP wymaga klucza interfejsu API w żądaniu HTTP. Dlatego żądanie HTTP zwykle wygląda następująco:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

Klucz może być uwzględniony w zmiennej ciągu zapytania o nazwie `code`, zgodnie z powyższym, lub można je uwzględnić w `x-functions-key` nagłówka HTTP. Wartość klucza może być dowolny klawisz funkcji zdefiniowany dla funkcji lub dowolnego klucz hosta.

Można zezwolić na anonimowe żądania, które nie wymagają kluczy. Możesz również wymagać użytą klucza głównego. Zmiany domyślnego poziomu autoryzacji przy użyciu `authLevel` właściwości w powiązaniu JSON. Aby uzyskać więcej informacji, zobacz [wyzwalacza HTTP](#httptrigger).

### <a name="keys-and-webhooks"></a>Klucze i elementów webhook
Autoryzacji elementu Webhook jest obsługiwany przez element webhook składnika odbiornika, część wyzwalacza HTTP i mechanizmu w zależności od typu elementu webhook. Nie mechanizmu, jednak zależne od klucza. Domyślnie jest używana funkcja klucz o nazwie "domyślne". Aby użyć innego klucza, należy skonfigurować dostawcę elementu webhook można wysłać nazwę klucza z żądaniem w jednym z następujących sposobów:

- **Długość ciągu zapytania**: dostawca przekazuje nazwę klucza w `clientid` parametr ciągu, zapytania, takie jak `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`.
- **Nagłówek żądania**: dostawca przekazuje nazwę klucza w `x-functions-clientid` nagłówka.

> [!NOTE]
> Klawisze funkcyjne mają pierwszeństwo przed klucze hosta. Po zdefiniowaniu dwa klucze o takiej samej nazwie jest zawsze używana klawisze funkcyjne.


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a>Przykłady wyzwalacza HTTP
Załóżmy, że masz następujące wyzwalacza HTTP `bindings` tablicy function.json:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

Zobacz przykład specyficzny dla języka, która sprawdza, czy `name` parametru w ciągu zapytania lub w treści żądania HTTP.

* [C#](#httptriggercsharp)
* [F#](#httptriggerfsharp)
* [Node.js](#httptriggernodejs)


<a name="httptriggercsharp"></a>
### <a name="http-trigger-sample-in-c"></a>Przykładowe wyzwalacza HTTP w języku C# #
```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

Można także powiązać obiektu .NET. zamiast **HttpRequestMessage**. Ten obiekt jest tworzony z treści żądania jako JSON. Podobnie typu mogą być przekazywane do odpowiedzi HTTP, dane wyjściowe powiązania i zwrócony jako treść odpowiedzi, wraz z kodem stanu 200.

```csharp
using System.Net;
using System.Threading.Tasks;

public static string Run(CustomObject req, TraceWriter log)
{
    return "Hello " + req?.name;
}

public class CustomObject {
     public String name {get; set;}
}
}
```

<a name="httptriggerfsharp"></a>
### <a name="http-trigger-sample-in-f"></a>Przykładowe wyzwalacza HTTP w języku F # #
```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

Należy `project.json` pliku, który używa NuGet, aby odwołać `FSharp.Interop.Dynamic` i `Dynamitey` zestawy, takie jak następujące:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

To używa NuGet na pobieranie zależności i odwołuje się do nich w skrypcie.

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a>Przykładowe wyzwalacza HTTP w środowisku Node.JS
```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

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
     
<a name="hooktriggersample"></a>
## <a name="webhook-samples"></a>Przykłady elementu Webhook
Załóżmy, że masz następujące wyzwalacza elementu webhook `bindings` tablicy function.json:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

Patrz zaloguje się komentarze problem GitHub próbki specyficzny dla języka.

* [C#](#hooktriggercsharp)
* [F#](#hooktriggerfsharp)
* [Node.js](#hooktriggernodejs)

<a name="hooktriggercsharp"></a>

### <a name="webhook-sample-in-c"></a>Przykład elementu Webhook w języku C# #
```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

<a name="hooktriggerfsharp"></a>

### <a name="webhook-sample-in-f"></a>Przykład elementu Webhook w języku F # #
```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

<a name="hooktriggernodejs"></a>

### <a name="webhook-sample-in-nodejs"></a>Przykład elementu Webhook w środowisku Node.JS
```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```


## <a name="next-steps"></a>Następne kroki
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

