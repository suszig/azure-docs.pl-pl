---
title: "Powiązania funkcji magazynu obiektów Blob platformy Azure | Dokumentacja firmy Microsoft"
description: "Zrozumienie, jak używać usługi Azure Storage wyzwalaczy i powiązań w funkcji platformy Azure."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "funkcje usługi Azure, funkcje, przetwarzania zdarzeń, dynamiczne obliczeń niekorzystającą architektury"
ms.assetid: aba8976c-6568-4ec7-86f5-410efd6b0fb9
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/27/2017
ms.author: glenga
ms.openlocfilehash: 79b9dbee89a4e33a1768343b9242d6b2b1118355
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2017
---
# <a name="azure-functions-blob-storage-bindings"></a>Azure powiązania magazynu obiektów Blob funkcji
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule opisano sposób konfigurowania i pracować z usługi Azure Functions powiązania magazynu obiektów Blob platformy Azure. Wyzwalacz obsługuje funkcje platformy Azure, wejściowa i wyjściowa powiązania dla magazynu obiektów Blob platformy Azure. Na temat funkcji, które są dostępne w wszystkie powiązania [usługi Azure Functions wyzwalaczy i powiązań pojęcia](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> A [tylko kontem magazynu obiektów blob](../storage/common/storage-create-storage-account.md#blob-storage-accounts) nie jest obsługiwane. Obiekt blob magazynu wyzwalaczy i powiązań wymagają konta magazynu ogólnego przeznaczenia. 
> 

<a name="trigger"></a>
<a name="storage-blob-trigger"></a>
## <a name="blob-storage-triggers-and-bindings"></a>Obiekt blob magazynu wyzwalaczy i powiązań

Za pomocą wyzwalacza magazynu obiektów Blob platformy Azure, kod funkcja jest wywoływana po wykryciu nowych lub zaktualizowanych obiektów blob. Zawartość obiektu blob są dostarczane jako dane wejściowe do funkcji.

Zdefiniuj użyciu wyzwalacza magazynu obiektów blob **integracji** kartę w portalu funkcji. Portal tworzy następującą definicję w **powiązania** sekcji *function.json*:

```json
{
    "name": "<The name used to identify the trigger data in your code>",
    "type": "blobTrigger",
    "direction": "in",
    "path": "<container to monitor, and optionally a blob name pattern - see below>",
    "connection": "<Name of app setting - see below>"
}
```

Obiekt blob danych wejściowych i powiązania dane wyjściowe są definiowane przy użyciu `blob` jako typ powiązania:

```json
{
  "name": "<The name used to identify the blob input in your code>",
  "type": "blob",
  "direction": "in", // other supported directions are "inout" and "out"
  "path": "<Path of input blob - see below>",
  "connection":"<Name of app setting - see below>"
},
```

* `path` Obsługuje właściwość powiązania wyrażeń i parametry filtru. Zobacz [nazwy wzorców](#pattern).
* `connection` Właściwości musi zawierać nazwę ustawienia aplikacji, która zawiera parametry połączenia magazynu. W portalu Azure, standardowego edytora w **integracji** kartę konfiguruje to ustawienie aplikacji, można wybrać konto magazynu.

> [!NOTE]
> Podczas korzystania z wyzwalacza obiektu blob w planie zużycia, może istnieć maksymalnie 10-minutowych opóźnienia w przetwarzaniu nowe obiekty BLOB po aplikacji funkcji przeszedł bezczynności. Po uruchomieniu aplikacji funkcja obiekty BLOB są przetwarzane natychmiast. Aby uniknąć tego opóźnienia początkowej, weź pod uwagę jedną z następujących opcji:
> - Za pomocą plan usługi aplikacji na zawsze włączone.
> - Użyj innego mechanizmu wyzwalanie obiektów blob, przetwarzanie, takie jak wiadomość z kolejki nazwa obiektu blob. Na przykład zobacz [wyzwalacza kolejki z obiektu blob danych wejściowych powiązania](#input-sample).

<a name="pattern"></a>

### <a name="name-patterns"></a>Wzorce nazw
Można określić wzorzec nazwy obiektów blob w `path` właściwość, która może być wyrażenie filtru lub powiązanie. Zobacz [powiązania wyrażeń i wzorce](functions-triggers-bindings.md#binding-expressions-and-patterns).

Na przykład aby zastosować filtr do obiektów blob, które rozpoczyna się od ciągu "oryginalnego", należy użyć następującej definicji. Ta ścieżka znajduje obiektu blob o nazwie *Blob1.txt oryginalne* w *wejściowych* kontenera i wartość `name` zmienna w funkcji kodu jest `Blob1`.

```json
"path": "input/original-{name}",
```

Aby powiązać nazwę pliku obiektu blob i rozszerzenie oddzielnie, należy użyć dwóch wzorców. Ta ścieżka umożliwia znalezienie obiektu blob o nazwie *Blob1.txt oryginalne*i wartość `blobname` i `blobextension` zmienne w kodzie funkcja *Blob1 oryginalne* i *txt*.

```json
"path": "input/{blobname}.{blobextension}",
```

Typ pliku BLOB można ograniczyć przy użyciu wartości stałej dla rozszerzenia pliku. Na przykład aby wyzwolić tylko na pliki PNG, użyj następującego wzorca:

```json
"path": "samples/{name}.png",
```

Nawiasy klamrowe są znaki specjalne w wzorce nazw. Aby określić nazwy obiektu blob, które mają nawiasy klamrowe w nazwie, zmiany znaczenia przy użyciu dwa nawiasy klamrowe nawiasy klamrowe. Poniższy przykład umożliwia znalezienie obiektu blob o nazwie *{20140101}-soundfile.mp3* w *obrazów* kontenera i `name` jest wartość zmiennej w kodzie funkcja *soundfile.mp3*. 

```json
"path": "images/{{20140101}}-{name}",
```

### <a name="trigger-metadata"></a>Wyzwalacz metadanych

Wyzwalacz obiektów blob udostępnia kilka właściwości metadanych. Te właściwości mogą służyć jako część wyrażenia powiązania w pozostałych powiązaniach lub parametrów w kodzie. Te wartości mogą być tej samej semantyki jako [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).

- **BlobTrigger**. Wpisz polecenie `string`. Wyzwalająca ścieżka obiektu blob
- **Identyfikator URI**. Wpisz polecenie `System.Uri`. Identyfikator URI obiektu blob dla lokalizacji głównej.
- **Właściwości**. Wpisz polecenie `Microsoft.WindowsAzure.Storage.Blob.BlobProperties`. Właściwości systemu obiektu blob.
- **Metadane**. Wpisz polecenie `IDictionary<string,string>`. Zdefiniowane przez użytkownika metadane obiektu blob.

<a name="receipts"></a>

### <a name="blob-receipts"></a>Potwierdzenia obiektów blob
Środowisko uruchomieniowe usługi Azure Functions zapewnia, że żadna funkcja wyzwalacza obiektu blob jest wywoływana więcej niż raz dla tego samego obiektu blob nowe lub zaktualizowane. Aby określić wersję danego obiektu blob został przetworzony, przechowuje *obiektu blob potwierdzenia*.

Potwierdzenia w kontenerze o nazwie obiektu blob Azure magazynów funkcji *azure webjobs hostów* na koncie magazynu Azure dla aplikacji funkcji (zdefiniowane przez ustawienie aplikacji `AzureWebJobsStorage`). Potwierdzenie obiektu blob zawiera następujące informacje:

* Funkcja wyzwalana ("*&lt;funkcja Nazwa aplikacji >*. Funkcje.  *&lt;nazwy funkcji >*", na przykład:"MyFunctionApp.Functions.CopyBlob")
* Nazwa kontenera
* Typ obiektów blob ("BlockBlob" lub "PageBlob")
* Nazwa obiektu blob
* Element ETag (identyfikator wersji obiektów blob, na przykład: "0x8D1DC6E70A277EF")

Aby wymusić ponowne przetworzenie obiektu blob, należy usunąć potwierdzenia obiektu blob dla tego obiektu blob z *azure webjobs hostów* kontenera ręcznie.

<a name="poison"></a>

### <a name="handling-poison-blobs"></a>Obsługa skażone obiektów blob
Jeśli funkcja wyzwalacza obiektu blob nie powiodło się dla danego obiektu blob usługi Azure Functions ponownych prób, które działać łącznie 5 razy domyślnie. 

Jeśli nie wszystkie próby 5, usługi Azure Functions dodaje komunikat do kolejki magazynu o nazwie *webjob blobtrigger-poison*. Komunikat z kolejki skażone obiektów blob jest obiekt JSON, który zawiera następujące właściwości:

* FunctionId (w formacie  *&lt;funkcja Nazwa aplikacji >*. Funkcje.  *&lt;nazwy funkcji >*)
* BlobType ("BlockBlob" lub "PageBlob")
* Właściwość ContainerName
* Element BlobName
* Element ETag (identyfikator wersji obiektów blob, na przykład: "0x8D1DC6E70A277EF")

### <a name="blob-polling-for-large-containers"></a>Sondowanie w dużych kontenerów obiektów blob
Jeśli kontenera obiektów blob monitorowanych zawiera więcej niż 10 000 obiektów blob, funkcje skanowania środowiska uruchomieniowego plików dziennika do wyszukiwać nowych lub zmienionych obiektów blob. Ten proces nie jest w czasie rzeczywistym. Funkcja może nie pobrać są uruchamiane kilka minut lub dłużej po utworzeniu obiektu blob. Ponadto [magazynu dzienniki są tworzone na "optymalnego"](/rest/api/storageservices/About-Storage-Analytics-Logging) podstawy. Nie ma żadnej gwarancji, że wszystkie zdarzenia są przechwytywane. W niektórych warunkach Dzienniki mogą zostać pominięci. Jeśli potrzebujesz przetwarzania szybsze i bardziej niezawodny obiektów blob, należy rozważyć utworzenie [komunikatu w kolejce](../storage/queues/storage-dotnet-how-to-use-queues.md) podczas tworzenia obiektu blob. Następnie należy użyć [wyzwalacza kolejki](functions-bindings-storage-queue.md) zamiast wyzwalacza obiektu blob do przetworzenia w obiekcie blob.

<a name="triggerusage"></a>

## <a name="using-a-blob-trigger-and-input-binding"></a>Za pomocą wyzwalacza obiektu blob i powiązania wejściowego
W przypadku funkcji .NET dostęp do danych obiektów blob przy użyciu parametru metody, takich jak `Stream paramName`. W tym miejscu `paramName` jest wartością określoną w [konfiguracji wyzwalacza](#trigger). W przypadku funkcji Node.js dostęp do danych wejściowych obiektu blob przy użyciu `context.bindings.<name>`.

W środowisku .NET można powiązać z dowolnego typu na liście poniżej. Jeśli używany jako powiązania wejściowego, niektóre z tych typów wymagają `inout` powiązanie kierunek *function.json*. Tym kierunku nie jest obsługiwany przez standardowy edytor, trzeba używać Zaawansowany edytor.

* `TextReader`
* `Stream`
* `ICloudBlob`(wymaga kierunek powiązania "inout")
* `CloudBlockBlob`(wymaga kierunek powiązania "inout")
* `CloudPageBlob`(wymaga kierunek powiązania "inout")
* `CloudAppendBlob`(wymaga kierunek powiązania "inout")

Jeśli oczekiwano tekstu w obiektach blob, również można powiązać .NET `string` typu. To ustawienie jest zalecane tylko, jeśli rozmiar obiektu blob jest mała, ponieważ zawartość obiektu blob całego są ładowane do pamięci. Ogólnie rzecz biorąc, zaleca się używania `Stream` lub `CloudBlockBlob` typu.

## <a name="trigger-sample"></a>Przykładowe wyzwalacza
Załóżmy, że masz następujące function.json, który definiuje wyzwalacz magazynu obiektów blob:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":"MyStorageAccount"
        }
    ]
}
```

Patrz zaloguje się zawartość każdy obiekt blob dodawanej do kontenera monitorowanych próbki specyficzny dla języka.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="blob-trigger-examples-in-c"></a>Przykłady wyzwalacza obiektu blob w języku C# #

```cs
// Blob trigger sample using a Stream binding
public static void Run(Stream myBlob, TraceWriter log)
{
   log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

```cs
// Blob trigger binding to a CloudBlockBlob
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

<a name="triggernodejs"></a>

### <a name="trigger-example-in-nodejs"></a>Przykład wyzwalacza w środowisku Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```
<a name="outputusage"></a>
<a name="storage-blob-output-binding"></a>

## <a name="using-a-blob-output-binding"></a>Przy użyciu obiektu blob powiązania wyjściowego

W przypadku funkcji .NET należy użyj `out string` parametru w sygnatura funkcji lub użyj jednego z typów na poniższej liście. W przypadku funkcji Node.js dostęp przy użyciu obiektu blob danych wyjściowych `context.bindings.<name>`.

W przypadku funkcji .NET można danych wyjściowych do żadnej z następujących typów:

* `out string`
* `TextWriter`
* `Stream`
* `CloudBlobStream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

<a name="input-sample"></a>

## <a name="queue-trigger-with-blob-input-and-output-sample"></a>Wyzwalacz kolejki z obiektu blob wejściowymi i wyjściowymi próbki
Załóżmy, że masz następujące function.json, który definiuje [wyzwalacza magazynu kolejek](functions-bindings-storage-queue.md)magazynu obiektów blob danych wejściowych i wyjściowych magazynu obiektów blob. Zwróć uwagę na `queueTrigger` właściwości metadanych. obiektu blob danych wejściowych i wyjściowych `path` właściwości:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

Zobacz przykład specyficzny dla języka, który kopiuje blob danych wejściowych do obiektu blob danych wyjściowych.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="blob-binding-example-in-c"></a>Przykład powiązania obiektu blob w języku C# #

```cs
// Copy blob from input to output, based on a queue trigger
public static void Run(string myQueueItem, Stream myInputBlob, out Stream myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

<a name="innodejs"></a>

### <a name="blob-binding-example-in-nodejs"></a>Przykład powiązania obiektu blob w środowisku Node.js

```javascript
// Copy blob from input to output, based on a queue trigger
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="next-steps"></a>Następne kroki
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

