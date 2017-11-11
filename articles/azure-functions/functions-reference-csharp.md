---
title: "Dokumentacja dla deweloperów usługi Azure funkcje skryptu C# | Dokumentacja firmy Microsoft"
description: "Zrozumienie sposobu tworzenia usługi Azure Functions przy użyciu języka C#."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "usługa Azure Functions, funkcje, przetwarzanie zdarzeń, elementy webhook, obliczanie dynamiczne, architektura bez serwera"
ms.assetid: f28cda01-15f3-4047-83f3-e89d5728301c
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/07/2017
ms.author: glenga
ms.openlocfilehash: 10943769e46adf05642e19d6be297820171bb9f6
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="azure-functions-c-script-developer-reference"></a>Azure funkcje skryptu developer odwołanie w C#
[!INCLUDE [functions-selector-languages](../../includes/functions-selector-languages.md)]

Środowisko skryptu C# dla usługi Azure Functions opiera się na zestaw SDK zadań Webjob Azure. Przepływy danych w funkcji języka C# za pomocą argumenty metody. Argument nazwy zostały określone w `function.json`, i jest wstępnie zdefiniowanych nazw do uzyskiwania dostępu do elementów, jak funkcja tokenów rejestratora i anulowania.

W tym artykule przyjęto założenie, że został już przeczytany [dokumentacja dla deweloperów usługi Azure Functions](functions-reference.md).

Aby uzyskać informacje o użyciu języka C# klasy biblioteki, zobacz [.NET przy użyciu biblioteki klas z usługi Azure Functions](functions-dotnet-class-library.md).

## <a name="how-csx-works"></a>Jak działa csx
`.csx` Format umożliwia pisanie mniej "standardowy" i skoncentrować się na zapisywanie tylko C# funkcję. Jak zwykle obejmują wszelkie odwołania do zestawów i przestrzeni nazw na początku pliku. Zamiast zawijania wszystkie elementy w przestrzeni nazw i klasy, wystarczy zdefiniować `Run` metody. Jeśli musisz obejmują wszystkie klasy, na przykład aby zdefiniować obiekty zwykłego obiektu CLR stary (POCO), można dołączyć klasy wewnątrz tego samego pliku.   

## <a name="binding-to-arguments"></a>Powiązanie z argumentów
Różne powiązania są powiązane z funkcją języka C# za pomocą `name` właściwości w *function.json* konfiguracji. Każdego powiązania ma własną obsługiwanych typów; na przykład wyzwalacza obiektu blob może obsługiwać ciąg, POCO lub CloudBlockBlob. Obsługiwane typy są udokumentowane w odwołaniu dla każdego powiązania. Obiekt POCO musi mieć określonej metody pobierającej i ustawiającej zdefiniowane dla każdej właściwości.

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="using-method-return-value-for-output-binding"></a>Dla powiązania danych wyjściowych przy użyciu wartości zwracanej — metoda

Można użyć wartość zwracaną metody dla powiązania danych wyjściowych przy użyciu nazwy `$return` w *function.json*:

```json
{
    "type": "queue",
    "direction": "out",
    "name": "$return",
    "queueName": "outqueue",
    "connection": "MyStorageConnectionString",
}
```

```csharp
public static string Run(string input, TraceWriter log)
{
    return input;
}
```

## <a name="writing-multiple-output-values"></a>Trwa zapisywanie wielu wartości danych wyjściowych

Aby napisać wiele wartości do powiązania danych wyjściowych, użyj [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) lub [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) typów. Te typy są kolekcji tylko do zapisu, które są zapisywane w powiązaniu danych wyjściowych, po zakończeniu metody.

W tym przykładzie zapisuje wiele wiadomości w kolejce do tej samej kolejki przy użyciu `ICollector`:

```csharp
public static void Run(ICollector<string> myQueueItem, TraceWriter log)
{
    myQueueItem.Add("Hello");
    myQueueItem.Add("World!");
}
```

## <a name="logging"></a>Rejestrowanie
Aby rejestrować dane wyjściowe do dzienników przesyłania strumieniowego w języku C#, obejmują argumentu typu `TraceWriter`. Zaleca się jej nazwa `log`. Unikaj używania `Console.Write` w funkcji platformy Azure. 

`TraceWriter`jest zdefiniowany w [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs). Poziom dziennika `TraceWriter` można skonfigurować w [host.json](functions-host-json.md).

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Asynchroniczne
Aby funkcja asynchronicznego, użyj `async` — słowo kluczowe i przywracać `Task` obiektu.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName,
        Stream blobInput,
        Stream blobOutput)
{
    await blobInput.CopyToAsync(blobOutput, 4096, token);
}
```

## <a name="cancellation-token"></a>Token anulowania
Niektóre operacje wymagają łagodne zamykanie. Mimo że zawsze jest najlepiej napisać kod, który może obsługiwać awarii, w przypadkach, w którym ma do obsługi bezpiecznego zamknięcia żądań, należy zdefiniować [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) typu argumentu.  A `CancellationToken` podano sygnalizują wyzwoleniu zamknięcie hosta.

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName,
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Importowanie przestrzenie nazw
Należy zaimportować przestrzeni nazw, należy tak jak zwykle, z `using` klauzuli.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Następujących przestrzeni nazw są automatycznie importowane i w związku z tym są opcjonalne:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`

## <a name="referencing-external-assemblies"></a>Zewnętrzne zestawy odwołujące
Dla zestawów struktury, dodaj odwołania przy użyciu `#r "AssemblyName"` dyrektywy.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Następujące zestawy są automatycznie dodawane przez usługi Azure Functions Środowisko hostingu:

* `mscorlib`
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`

Następujące zestawy mogą być używane przez prostą nazwę (na przykład `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNet.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

## <a name="referencing-custom-assemblies"></a>Odwołania do zestawów niestandardowych

Aby odwołać niestandardowego zestawu, możesz użyć dowolnej *udostępnionego* zestawu lub *prywatnej* zestawu:
- Zestawy udostępnione są współużytkowane przez wszystkie funkcje w aplikacji funkcji. Aby odwołać się do niestandardowego zestawu Przekaż zestawu do aplikacji funkcji, takich jak w `bin` folder w katalogu głównym aplikacji funkcji. 
- Zestawy prywatne są częścią kontekstu daną funkcję i obsługuje ładowania bezpośredniego w różnych wersjach. Zestawy prywatne należy przekazać w `bin` folderu w katalogu funkcji. Odwołanie przy użyciu nazwy pliku, takich jak `#r "MyAssembly.dll"`. 

Aby uzyskać informacje na temat przekazywania plików do folderu funkcji zobacz sekcję poniżej pakietu zarządzania.

### <a name="watched-directories"></a>Monitorowane katalogów

Katalog zawierający plik skryptu funkcji automatycznie jest monitorowana zmian do zestawów. Aby obejrzeć zmiany zestawu w innych katalogów, dodaj je do `watchDirectories` na liście [host.json](functions-host-json.md).

## <a name="using-nuget-packages"></a>Za pomocą pakietów NuGet
Aby użyć pakietów NuGet w funkcji języka C#, Przekaż *project.json* plik do folderu funkcji w systemie plików aplikacji funkcji. Oto przykład *project.json* pliku, który dodaje odwołanie do wersji 1.1.0 Microsoft.ProjectOxford.Face:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Obsługiwane jest tylko .NET Framework 4.6, upewnij się, że Twoje *project.json* Określa plik `net46` w sposób pokazany poniżej.

Po przekazaniu *project.json* plików, środowisko uruchomieniowe pobiera pakiety i automatycznie dodaje odwołania do zestawów pakietu. Nie trzeba dodać `#r "AssemblyName"` dyrektywy. Aby użyć typów zdefiniowanych w pakietach NuGet, Dodaj wymagane `using` instrukcje do Twojej *run.csx* pliku. 

W środowisku wykonawczym funkcji Przywracanie NuGet działa na podstawie porównania ilości `project.json` i `project.lock.json`. Jeśli pliki sygnatur Data i godzina **nie** uruchamia Przywracanie NuGet dopasowania i pliki do pobrania NuGet zaktualizowane pakiety. Jednak jeśli oznaczenie daty i godziny plików **czy** dopasowania, NuGet nie wykonuje operację przywracania. W związku z tym `project.lock.json` nie powinny zostać wdrożone, ponieważ powoduje on NuGet pominąć Przywracanie pakietu. Aby uniknąć wdrażanie pliku blokady, Dodaj `project.lock.json` do `.gitignore` pliku.

Aby użyć niestandardowej NuGet źródła danych, określ źródła danych w *Nuget.Config* w katalogu głównym aplikacji funkcji. Aby uzyskać więcej informacji, zobacz [NuGet Konfigurowanie zachowania](/nuget/consume-packages/configuring-nuget-behavior).

### <a name="using-a-projectjson-file"></a>Przy użyciu pliku project.json
1. Otwarcie funkcji w portalu Azure. Na karcie dzienniki są wyświetlane dane wyjściowe instalacji pakietu.
2. Aby przekazać plik project.json, użyj jednej z metod opisanych w [jak zaktualizować pliki aplikacji funkcji](functions-reference.md#fileupdate) w temacie Odwołanie do usługi Azure Functions dla deweloperów.
3. Po *project.json* przekazać pliku, wyświetlone dane wyjściowe podobne do poniższego przykładu w funkcji do przesyłania strumieniowego dzienników:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Zmienne środowiskowe
Aby uzyskać wartość zmiennej środowiskowej lub wartość ustawienia aplikacji, należy użyć `System.Environment.GetEnvironmentVariable`, jak pokazano w poniższym przykładzie:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " +
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a>Ponowne wykorzystywanie kodu csx
Można użyć klasy i metody zdefiniowane w innych *csx* pliki w Twojej *run.csx* pliku. Aby to zrobić, użyj `#load` dyrektywy w Twojej *run.csx* pliku. W poniższym przykładzie o nazwie Procedura rejestrowania `MyLogger` są udostępniane w *myLogger.csx* i ładowane do *run.csx* przy użyciu `#load` dyrektywy:

Przykład *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}");
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Przykład *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext);
}
```

Za pomocą udostępnionej *csx* jest wspólnym wzorcem można zdecydowanie wpisz argumentów pomiędzy funkcjami przy użyciu obiektu POCO. W poniższym przykładzie uproszczony wyzwalacza HTTP i kolejki wyzwalacza udziału obiektu POCO o nazwie `Order` do silnie typu danych kolejności:

Przykład *run.csx* wyzwalacza HTTP:

```cs
#load "..\shared\order.csx"

using System.Net;

public static async Task<HttpResponseMessage> Run(Order req, IAsyncCollector<Order> outputQueueItem, TraceWriter log)
{
    log.Info("C# HTTP trigger function received an order.");
    log.Info(req.ToString());
    log.Info("Submitting to processing queue.");

    if (req.orderId == null)
    {
        return new HttpResponseMessage(HttpStatusCode.BadRequest);
    }
    else
    {
        await outputQueueItem.AddAsync(req);
        return new HttpResponseMessage(HttpStatusCode.OK);
    }
}
```

Przykład *run.csx* wyzwalacza kolejki:

```cs
#load "..\shared\order.csx"

using System;

public static void Run(Order myQueueItem, out Order outputQueueItem,TraceWriter log)
{
    log.Info($"C# Queue trigger function processed order...");
    log.Info(myQueueItem.ToString());

    outputQueueItem = myQueueItem;
}
```

Przykład *order.csx*:

```cs
public class Order
{
    public string orderId {get; set; }
    public string custName {get; set;}
    public string custAddress {get; set;}
    public string custEmail {get; set;}
    public string cartId {get; set; }

    public override String ToString()
    {
        return "\n{\n\torderId : " + orderId +
                  "\n\tcustName : " + custName +             
                  "\n\tcustAddress : " + custAddress +             
                  "\n\tcustEmail : " + custEmail +             
                  "\n\tcartId : " + cartId + "\n}";             
    }
}
```

Można użyć ścieżki względnej z `#load` dyrektywy:

* `#load "mylogger.csx"`ładuje plik znajduje się w folderze funkcji.
* `#load "loadedfiles\mylogger.csx"`ładuje plik znajdujący się w folderze w folderze funkcji.
* `#load "..\shared\mylogger.csx"`ładuje plik znajdujący się w folderze na tym samym poziomie co folder funkcji, bezpośrednio pod *wwwroot*.

`#load` Dyrektywy działa tylko w przypadku *csx* plików (C# skrypt), nie z *.cs* plików.

<a name="imperative-bindings"></a> 

## <a name="binding-at-runtime-via-imperative-bindings"></a>Powiązanie w czasie wykonywania za pośrednictwem powiązania imperatywne

W języku C# i innych języków .NET, można użyć [imperatywnych](https://en.wikipedia.org/wiki/Imperative_programming) powiązanie wzorzec, w przeciwieństwie do [ *deklaratywne* ](https://en.wikipedia.org/wiki/Declarative_programming) powiązania w *function.json*. Powiązanie konieczne jest przydatne, gdy Parametry wiążące muszą ma zostać obliczony w czasie środowiska uruchomieniowego zamiast projektu. Z tego wzorca można powiązać z obsługiwanych danych wejściowych i wyjściowych powiązania na bieżąco w kodzie funkcji.

Zdefiniuj staje się niezbędna powiązania w następujący sposób:

- **Nie** obejmują wpis w *function.json* dla żądanego imperatywnych powiązania.
- Przekaż parametr wejściowy [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) lub [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- Aby wykonać wiązania danych, użyj następującego wzorca C#.

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

`BindingTypeAttribute`atrybut .NET, który definiuje wiązania jest i `T` jest typ danych wejściowych lub wyjściowych, który jest obsługiwany przez ten typ powiązania. `T`nie może być `out` typ parametru (takie jak `out JObject`). Na przykład, w tabeli Mobile Apps output powiązanie obsługuje [sześć output typy](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), ale można używać tylko [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) lub [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) dla `T`.

Poniższy przykładowy kod tworzy [powiązania wyjściowego obiektu blob magazynu](functions-bindings-storage-blob.md#blob-storage-input--output-bindings) z obiektu blob ścieżki, która jest zdefiniowana w czasie wykonywania, następnie zapisuje ciąg obiektu blob.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    using (var writer = await binder.BindAsync<TextWriter>(new BlobAttribute("samples-output/path")))
    {
        writer.Write("Hello World!!");
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) definiuje [obiektu blob magazynu](functions-bindings-storage-blob.md) wejściowych lub wyjściowych powiązanie, i [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) jest typ powiązania obsługiwanych danych wyjściowych.
W poprzednim przykładzie kodu, kod pobiera ustawienia aplikacji dla aplikacji funkcja parametrów połączenia w głównym konta magazynu (czyli `AzureWebJobsStorage`). Można określić ustawienie niestandardowych aplikacji, aby użyć konta magazynu przez dodanie [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) i przekazanie tablicy atrybut do `BindAsync<T>()`. Na przykład:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    var attributes = new Attribute[]
    {    
        new BlobAttribute("samples-output/path"),
        new StorageAccountAttribute("MyStorageAccount")
    };

    using (var writer = await binder.BindAsync<TextWriter>(attributes))
    {
        writer.Write("Hello World!");
    }
}
```

W poniższej tabeli przedstawiono atrybuty .NET dla każdego typu powiązania i pakiety, w których są zdefiniowane.

> [!div class="mx-codeBreakAll"]
| Powiązanie | Atrybut | Dodaj odwołanie |
|------|------|------|
| Cosmos DB | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.DocumentDB"` |
| Usługa Event Hubs | [`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
| Mobile Apps | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
| Notification Hubs | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
| Service Bus | [`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
| Kolejki magazynu | [`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Obiektu blob magazynu | [`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Tabela magazynu | [`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Twilio | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |



## <a name="next-steps"></a>Następne kroki
Więcej informacji zawierają następujące zasoby:

* [Najlepsze rozwiązania dotyczące usługi Azure Functions](functions-best-practices.md)
* [Dokumentacja usługi Azure Functions dla deweloperów](functions-reference.md)
* [Azure funkcje wyzwalaczy i powiązań](functions-triggers-bindings.md)
