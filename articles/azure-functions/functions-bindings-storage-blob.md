---
title: "Azure powiązania magazynu obiektów Blob dla usługi Azure Functions"
description: "Zrozumienie, jak używać magazynu obiektów Blob platformy Azure Usługa wyzwalaczy i powiązań w usługi Azure Functions."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "funkcje usługi Azure, funkcje, przetwarzania zdarzeń, dynamiczne obliczeń niekorzystającą architektury"
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/27/2017
ms.author: glenga
ms.openlocfilehash: f00bda8e4700676e70f958eff511495f0ea564b1
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="azure-blob-storage-bindings-for-azure-functions"></a>Azure powiązania magazynu obiektów Blob dla usługi Azure Functions

W tym artykule opisano sposób pracy z usługi Azure Functions powiązania magazynu obiektów Blob platformy Azure. Usługi Azure Functions obsługuje wyzwolenia, danych wejściowych i wyjściowych powiązań dla obiektów blob.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> [Konta magazynu tylko do obiektów blob](../storage/common/storage-create-storage-account.md#blob-storage-accounts) nie są obsługiwane. Obiekt blob magazynu wyzwalaczy i powiązań wymagają konta magazynu ogólnego przeznaczenia. 

## <a name="trigger"></a>Wyzwalacz

Użyj wyzwalacz magazynu obiektów Blob, aby uruchomić funkcję o wykryciu nowych lub zaktualizowanych obiektów blob. Zawartość obiektu blob są dostarczane jako dane wejściowe do funkcji.

> [!NOTE]
> Podczas korzystania z wyzwalacza obiektu blob w planie zużycia, może istnieć maksymalnie 10-minutowych opóźnienia w przetwarzaniu nowe obiekty BLOB po aplikacji funkcji przeszedł bezczynności. Po uruchomieniu aplikacji funkcja obiekty BLOB są przetwarzane natychmiast. Aby uniknąć tego opóźnienia początkowej, weź pod uwagę jedną z następujących opcji:
> - Za pomocą plan usługi aplikacji na zawsze włączone.
> - Użyj innego mechanizmu wyzwalanie obiektów blob, przetwarzanie, takie jak wiadomość z kolejki nazwa obiektu blob. Na przykład zobacz [przykład wejścia/wyjścia powiązania obiektu blob w dalszej części tego artykułu](#input--output---example).

## <a name="trigger---example"></a>Wyzwalacz — przykład

Zapoznaj się z przykładem specyficzny dla języka:

* [C#](#trigger---c-example)
* [Skryptu C# (csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Wyzwalacz — przykład C#

W poniższym przykładzie przedstawiono [C# funkcja](functions-dotnet-class-library.md) który zapisuje dziennik obiektu blob jest dodane lub zaktualizowane w `samples-workitems` kontenera.

```csharp
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Aby uzyskać więcej informacji na temat `BlobTrigger` atrybutów, zobacz [wyzwalacza — atrybuty](#trigger---attributes).

### <a name="trigger---c-script-example"></a>Wyzwalacz — przykładowy skrypt w języku C#

W poniższym przykładzie przedstawiono wyzwalacza obiektu blob powiązanie w *function.json* pliku i [skryptu C# (csx)](functions-reference-csharp.md) kodu korzystającego z powiązania. Jeśli obiekt blob jest dodane lub zaktualizowane w funkcji zapisuje dziennik `samples-workitems` kontenera.

W tym miejscu jest powiązanie danych *function.json* pliku:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

[Konfiguracji](#trigger---configuration) sekcji opisano te właściwości.

Oto C# kodu skryptu, który jest powiązany z `Stream`:

```cs
public static void Run(Stream myBlob, TraceWriter log)
{
   log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Oto C# kodu skryptu, który jest powiązany z `CloudBlockBlob`:

```cs
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

### <a name="trigger---javascript-example"></a>Wyzwalacz — przykład JavaScript

W poniższym przykładzie przedstawiono wyzwalacza obiektu blob powiązanie w *function.json* plików i [kod JavaScript] (funkcje — odwołanie node.md), która używa powiązania. Jeśli obiekt blob jest dodane lub zaktualizowane w funkcji zapisuje dziennik `samples-workitems` kontenera.

Oto *function.json* pliku:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

[Konfiguracji](#trigger---configuration) sekcji opisano te właściwości.

Oto kod JavaScript:

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```

## <a name="trigger---attributes"></a>Wyzwalacz — atrybuty

W [bibliotek klas C#](functions-dotnet-class-library.md), umożliwia skonfigurowanie wyzwalacza obiektu blob następujące atrybuty:

* [BlobTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobTriggerAttribute.cs), zdefiniowany w pakiecie NuGet [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs)

  Konstruktor atrybutu ma ciąg ścieżki, który wskazuje kontenera, aby obejrzeć i opcjonalnie [wzorzec nazwy obiektu blob](#trigger---blob-name-patterns). Oto przykład:

  ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}")] Stream image, 
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
  ```

  Można ustawić `Connection` właściwości w celu określenia konta magazynu do użycia, jak pokazano w poniższym przykładzie:

   ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}", Connection = "StorageConnectionAppSetting")] Stream image, 
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
  ```

  Pełny przykład, zobacz [wyzwalacza — przykład C#](#trigger---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs), zdefiniowany w pakiecie NuGet [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs)

  Udostępnia innym sposobem określania konta magazynu do użycia. Konstruktor przyjmuje nazwę ustawienia aplikacji, która zawiera parametry połączenia magazynu. Ten atrybut można stosować na poziomie klasy parametrów, metody lub. W poniższym przykładzie przedstawiono poziom klasy i metody:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("BlobTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ....
  }
  ```

Konta magazynu do użycia jest określany w następującej kolejności:

* `BlobTrigger` Atrybutu `Connection` właściwości.
* `StorageAccount` Atrybut zastosowany do tego samego parametru jako `BlobTrigger` atrybutu.
* `StorageAccount` Atrybut zastosowany do funkcji.
* `StorageAccount` Atrybut zastosowany do klasy.
* Domyślne konto magazynu dla funkcji aplikacji (ustawienie aplikacji "AzureWebJobsStorage").

## <a name="trigger---configuration"></a>Wyzwalacz — Konfiguracja

W poniższej tabeli opisano powiązania właściwości konfiguracyjne, które można ustawić w *function.json* pliku i `BlobTrigger` atrybutu.

|Właściwość Function.JSON | Właściwość atrybutu |Opis|
|---------|---------|----------------------|
|**Typ** | Nie dotyczy | należy wybrać opcję `blobTrigger`. Ta właściwość ma wartość automatycznie, podczas tworzenia wyzwalacza w portalu Azure.|
|**Kierunek** | Nie dotyczy | należy wybrać opcję `in`. Ta właściwość ma wartość automatycznie, podczas tworzenia wyzwalacza w portalu Azure. Wyjątki w [użycia](#trigger---usage) sekcji. |
|**Nazwa** | Nie dotyczy | Nazwa zmiennej, która reprezentuje obiektu blob w kodzie funkcji. | 
|**Ścieżka** | **BlobPath** |Kontener do monitorowania.  Może być [wzorzec nazwy obiektu blob](#trigger-blob-name-patterns). | 
|**połączenia** | **Połączenia** | Nazwa ustawienia aplikacji, która zawiera parametry połączenia magazynu do użycia dla tego powiązania. Jeśli nazwa ustawienia aplikacji rozpoczyna się od "AzureWebJobs", można określić tylko w pozostałej części nazwy w tym miejscu. Na przykład jeśli ustawisz `connection` do "MyStorage" środowisko uruchomieniowe Functions szuka ustawienie aplikacji o nazwie "AzureWebJobsMyStorage." Jeśli opuścisz `connection` pusta, środowisko uruchomieniowe Functions używa domyślnego ciągu połączenia magazynu w ustawieniu aplikacji o nazwie `AzureWebJobsStorage`.<br><br>Ciąg połączenia nie może być dla konta magazynu ogólnego przeznaczenia, [konta magazynu tylko do obiektów blob](../storage/common/storage-create-storage-account.md#blob-storage-accounts).|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Wyzwalacz — użycie

W języku C# i skryptu C#, dostęp do danych obiektów blob za pomocą parametru metody, takie jak `Stream paramName`. W języku C# skryptu `paramName` jest wartością określoną w `name` właściwość *function.json*. Można powiązać żadnego z następujących typów:

* `TextReader`
* `Stream`
* `ICloudBlob`(wymaga kierunek powiązania "inout" *function.json*)
* `CloudBlockBlob`(wymaga kierunek powiązania "inout" *function.json*)
* `CloudPageBlob`(wymaga kierunek powiązania "inout" *function.json*)
* `CloudAppendBlob`(wymaga kierunek powiązania "inout" *function.json*)

Jak wspomniano, niektóre z tych typów wymagają `inout` powiązanie kierunek *function.json*. Tym kierunku nie jest obsługiwany przez standardowy edytor w portalu Azure, trzeba używać Zaawansowany edytor.

Jeśli oczekiwano tekstu w obiektach blob, można powiązać `string` typu. To ustawienie jest zalecane tylko, jeśli rozmiar obiektu blob jest mała, ponieważ zawartość obiektu blob całego są ładowane do pamięci. Ogólnie rzecz biorąc, zaleca się używania `Stream` lub `CloudBlockBlob` typu.

W języku JavaScript, dostęp do danych wejściowych obiektu blob przy użyciu `context.bindings.<name>`.

## <a name="trigger---blob-name-patterns"></a>Wyzwalacz - wzorce nazw obiektów blob

Można określić wzorzec nazwy obiektów blob w `path` właściwości w *function.json* lub `BlobTrigger` atrybut konstruktora. Wzorzec nazwy mogą być [wyrażenie filtru lub powiązanie](functions-triggers-bindings.md#binding-expressions-and-patterns).

### <a name="filter-on-blob-name"></a>Filtrowanie według nazwy obiektu blob

Tylko dla obiektów blob w następujących wyzwalaczy przykład `input` kontenera, w którym rozpoczyna się od ciągu "oryginalny —":

```json
"path": "input/original-{name}",
```
 
Jeśli nazwa obiektu blob jest *Blob1.txt oryginalne*, wartość `name` zmienna w funkcji kodu jest `Blob1`.

### <a name="filter-on-file-type"></a>Filtrowanie według typu pliku

Poniższy przykład wyzwala tylko na *.png* plików:

```json
"path": "samples/{name}.png",
```

### <a name="filter-on-curly-braces-in-file-names"></a>Filtrowanie według nawiasy klamrowe w nazwach plików

Aby wyszukać nawiasy klamrowe w nazwach plików, poza nawiasy klamrowe przy użyciu dwa nawiasy klamrowe. Następujące przykładowe filtry dla obiektów blob, które mają nawiasy klamrowe w nazwie:

```json
"path": "images/{{20140101}}-{name}",
```

Jeśli w nazwie obiektu blob *{20140101}-soundfile.mp3*, `name` jest wartość zmiennej w kodzie funkcja *soundfile.mp3*. 

### <a name="get-file-name-and-extension"></a>Pobierz nazwę i rozszerzenie pliku

Poniższy przykład przedstawia sposób powiązania osobno nazwę pliku obiektu blob i rozszerzenie:

```json
"path": "input/{blobname}.{blobextension}",
```
Jeśli nosi nazwę obiektu blob *Blob1.txt oryginalne*, wartość `blobname` i `blobextension` zmienne w kodzie funkcja *Blob1 oryginalne* i *txt*.

## <a name="trigger---metadata"></a>Wyzwalacz - metadanych

Wyzwalacz obiektów blob udostępnia kilka właściwości metadanych. Te właściwości mogą służyć jako część wyrażenia powiązania w pozostałych powiązaniach lub parametrów w kodzie. Te wartości mogą być tej samej semantyki jako [CloudBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet) typu.


|Właściwość  |Typ  |Opis  |
|---------|---------|---------|
|`BlobTrigger`|`string`|Ścieżka do wyzwalania obiektu blob.|
|`Uri`|`System.Uri`|Identyfikator URI obiektu blob dla lokalizacji głównej.|
|`Properties` |[BlobProperties](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties)|Właściwości systemu obiektu blob. |
|`Metadata` |`IDictionary<string,string>`|Zdefiniowane przez użytkownika metadane obiektu blob.|

## <a name="trigger---blob-receipts"></a>Wyzwalacz — obiekt blob potwierdzenia

Środowisko uruchomieniowe usługi Azure Functions zapewnia, że żadna funkcja wyzwalacza obiektu blob jest wywoływana więcej niż raz dla tego samego obiektu blob nowe lub zaktualizowane. Aby określić wersję danego obiektu blob został przetworzony, przechowuje *obiektu blob potwierdzenia*.

Potwierdzenia w kontenerze o nazwie obiektu blob Azure magazynów funkcji *azure webjobs hostów* na koncie magazynu Azure dla aplikacji funkcji (zdefiniowane przez ustawienie aplikacji `AzureWebJobsStorage`). Potwierdzenie obiektu blob zawiera następujące informacje:

* Funkcja wyzwalana ("*&lt;funkcja Nazwa aplikacji >*. Funkcje.  *&lt;nazwy funkcji >*", na przykład:"MyFunctionApp.Functions.CopyBlob")
* Nazwa kontenera
* Typ obiektów blob ("BlockBlob" lub "PageBlob")
* Nazwa obiektu blob
* Element ETag (identyfikator wersji obiektów blob, na przykład: "0x8D1DC6E70A277EF")

Aby wymusić ponowne przetworzenie obiektu blob, należy usunąć potwierdzenia obiektu blob dla tego obiektu blob z *azure webjobs hostów* kontenera ręcznie.

## <a name="trigger---poison-blobs"></a>Wyzwalacz - skażone obiektów blob

Jeśli funkcja wyzwalacza obiektu blob nie powiodło się dla danego obiektu blob usługi Azure Functions ponownych prób, które działać łącznie 5 razy domyślnie. 

Jeśli nie wszystkie próby 5, usługi Azure Functions dodaje komunikat do kolejki magazynu o nazwie *webjob blobtrigger-poison*. Komunikat z kolejki skażone obiektów blob jest obiekt JSON, który zawiera następujące właściwości:

* FunctionId (w formacie  *&lt;funkcja Nazwa aplikacji >*. Funkcje.  *&lt;nazwy funkcji >*)
* BlobType ("BlockBlob" lub "PageBlob")
* Właściwość ContainerName
* Element BlobName
* Element ETag (identyfikator wersji obiektów blob, na przykład: "0x8D1DC6E70A277EF")

## <a name="trigger---polling-for-large-containers"></a>Wyzwalacz - sondowanie dużych kontenerów

Jeśli kontenera obiektów blob monitorowanych zawiera więcej niż 10 000 obiektów blob, funkcje skanowania środowiska uruchomieniowego plików dziennika do wyszukiwać nowych lub zmienionych obiektów blob. Ten proces może spowodować opóźnienia. Funkcja może nie pobrać są uruchamiane kilka minut lub dłużej po utworzeniu obiektu blob. Ponadto [magazynu dzienniki są tworzone na "optymalnego"](/rest/api/storageservices/About-Storage-Analytics-Logging) podstawy. Nie ma żadnej gwarancji, że wszystkie zdarzenia są przechwytywane. W niektórych warunkach Dzienniki mogą zostać pominięci. Jeśli potrzebujesz przetwarzania szybsze i bardziej niezawodny obiektów blob, należy rozważyć utworzenie [komunikatu w kolejce](../storage/queues/storage-dotnet-how-to-use-queues.md) podczas tworzenia obiektu blob. Następnie użyj [wyzwalacza kolejki](functions-bindings-storage-queue.md) zamiast wyzwalacza obiektu blob do przetworzenia w obiekcie blob. Inną opcją jest użycie siatki zdarzeń; Zobacz samouczek [zmiana rozmiaru Automatyzacja przekazać obrazów przy użyciu siatki zdarzeń](../event-grid/resize-images-on-storage-blob-upload-event.md).

## <a name="input--output"></a>Dane wejściowe & e wyjściowe

Użyj magazynu obiektów Blob wejściowa i wyjściowa powiązania do odczytywania i zapisywania obiektów blob.

## <a name="input--output---example"></a>Wejście i wyjście — przykład

Zapoznaj się z przykładem specyficzny dla języka:

* [C#](#input--output---c-example)
* [Skryptu C# (csx)](#input--output---c-script-example)
* [JavaScript](#input--output---javascript-example)

### <a name="input--output---c-example"></a>Dane wejściowe & e wyjściowe — przykład C#

Poniżej przedstawiono przykład [C# funkcja](functions-dotnet-class-library.md) używającą wyzwalacz obiektów blob i output dwa powiązania obiektu blob. Funkcja jest wyzwalany przez utworzenie obiektu blob obrazu w *przykładowe obrazy* kontenera. Tworzy kopie małych i średnich obiekt blob obrazu. 

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
{
    var imageBuilder = ImageResizer.ImageBuilder.Current;
    var size = imageDimensionsTable[ImageSize.Small];

    imageBuilder.Build(image, imageSmall,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);

    image.Position = 0;
    size = imageDimensionsTable[ImageSize.Medium];

    imageBuilder.Build(image, imageMedium,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);
}

public enum ImageSize { ExtraSmall, Small, Medium }

private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
    { ImageSize.ExtraSmall, (320, 200) },
    { ImageSize.Small,      (640, 400) },
    { ImageSize.Medium,     (800, 600) }
};
```        

### <a name="input--output---c-script-example"></a>Dane wejściowe & e wyjściowe — przykładowy skrypt w języku C#

W poniższym przykładzie przedstawiono obiektu blob danych wejściowych i wyjściowych powiązania w *function.json* pliku i [skryptu C# (csx)](functions-reference-csharp.md) kodu korzystającego z powiązania. Funkcja tworzy kopię obiektu blob tekstu. Funkcja jest wyzwalany przez komunikat z kolejki, zawierający nazwę można skopiować obiektu blob. Nosi nazwę nowego obiektu blob *{originalblobname}-kopiowania*.

W *function.json* pliku `queueTrigger` metadanych jest używana do określenia nazwy obiektów blob w `path` właściwości:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

[Konfiguracji](#input--output---configuration) sekcji opisano te właściwości.

Oto kod skryptu C#:

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

### <a name="input--output---javascript-example"></a>Dane wejściowe & e wyjściowe — przykład JavaScript

W poniższym przykładzie przedstawiono obiektu blob danych wejściowych i wyjściowych powiązania w *function.json* plików i [kod JavaScript] (funkcje — odwołanie node.md), która używa powiązania. Funkcja tworzy kopię obiektu blob. Funkcja jest wyzwalany przez komunikat z kolejki, zawierający nazwę można skopiować obiektu blob. Nosi nazwę nowego obiektu blob *{originalblobname}-kopiowania*.

W *function.json* pliku `queueTrigger` metadanych jest używana do określenia nazwy obiektów blob w `path` właściwości:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

[Konfiguracji](#input--output---configuration) sekcji opisano te właściwości.

Oto kod JavaScript:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="input--output---attributes"></a>Wejście i wyjście — atrybuty

W [bibliotek klas C#](functions-dotnet-class-library.md), użyj [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), która jest zdefiniowana w pakiecie NuGet [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs).

Konstruktor atrybutu ma ścieżki do obiektu blob i `FileAccess` parametr wskazujący odczytu lub zapisu, jak pokazano w poniższym przykładzie:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
{
    ...
}
```

Można ustawić `Connection` właściwości w celu określenia konta magazynu do użycia, jak pokazano w poniższym przykładzie:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-md/{name}", FileAccess.Write, Connection = "StorageConnectionAppSetting")] Stream imageSmall)
{
    ...
}
```

Pełny przykład, zobacz [danych wejściowych i wyjściowych — przykład C#](#input--output---c-example).

Można użyć `StorageAccount` atrybutu, aby określić konto magazynu na poziomie klasy, metody lub parametru. Aby uzyskać więcej informacji, zobacz [wyzwalacza — atrybuty](#trigger---attributes).

## <a name="input--output---configuration"></a>Wejście i wyjście — Konfiguracja

W poniższej tabeli opisano powiązania właściwości konfiguracyjne, które można ustawić w *function.json* pliku i `Blob` atrybutu.

|Właściwość Function.JSON | Właściwość atrybutu |Opis|
|---------|---------|----------------------|
|**Typ** | Nie dotyczy | należy wybrać opcję `blob`. |
|**Kierunek** | Nie dotyczy | Należy wybrać opcję `in` dla powiązania wejściowego lub `out` dla powiązania danych wyjściowych. Wyjątki w [użycia](#input--output---usage) sekcji. |
|**Nazwa** | Nie dotyczy | Nazwa zmiennej, która reprezentuje obiektu blob w kodzie funkcji.  Ustaw `$return` odwoływać się do wartości zwracane funkcji.|
|**Ścieżka** |**BlobPath** | Ścieżka do obiektu blob. | 
|**połączenia** |**Połączenia**| Nazwa ustawienia aplikacji, która zawiera parametry połączenia magazynu do użycia dla tego powiązania. Jeśli nazwa ustawienia aplikacji rozpoczyna się od "AzureWebJobs", można określić tylko w pozostałej części nazwy w tym miejscu. Na przykład jeśli ustawisz `connection` do "MyStorage" środowisko uruchomieniowe Functions szuka ustawienie aplikacji o nazwie "AzureWebJobsMyStorage." Jeśli opuścisz `connection` pusta, środowisko uruchomieniowe Functions używa domyślnego ciągu połączenia magazynu w ustawieniu aplikacji o nazwie `AzureWebJobsStorage`.<br><br>Ciąg połączenia nie może być dla konta magazynu ogólnego przeznaczenia, [konta magazynu tylko do obiektów blob](../storage/common/storage-create-storage-account.md#blob-storage-accounts).|
|Nie dotyczy | **Dostęp** | Wskazuje, czy będzie można odczytu lub zapisu. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input--output---usage"></a>Dane wejściowe & e wyjściowe — użycie

Biblioteki klas C# i skryptu C#, dostęp do obiektu blob przy użyciu parametru metody takie jak `Stream paramName`. W języku C# skryptu `paramName` jest wartością określoną w `name` właściwość *function.json*. Można powiązać żadnego z następujących typów:

* `out string`
* `TextWriter` 
* `TextReader`
* `Stream`
* `ICloudBlob`(wymaga kierunek powiązania "inout" *function.json*)
* `CloudBlockBlob`(wymaga kierunek powiązania "inout" *function.json*)
* `CloudPageBlob`(wymaga kierunek powiązania "inout" *function.json*)
* `CloudAppendBlob`(wymaga kierunek powiązania "inout" *function.json*)

Jak wspomniano, niektóre z tych typów wymagają `inout` powiązanie kierunek *function.json*. Tym kierunku nie jest obsługiwany przez standardowy edytor w portalu Azure, trzeba używać Zaawansowany edytor.

Podczas czytania tekstu obiekty BLOB można powiązać `string` typu. Ten typ jest zalecane tylko w przypadku, gdy zawartość całego obiektu blob są ładowane do pamięci jest mała, rozmiar obiektu blob. Ogólnie rzecz biorąc, zaleca się używania `Stream` lub `CloudBlockBlob` typu.

W języku JavaScript, uzyskiwać dostęp za pomocą danych obiektów blob `context.bindings.<name>`.

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Przejdź do szybkiego startu, który używa wyzwalacz magazynu obiektów Blob](functions-create-storage-blob-triggered-function.md)

> [!div class="nextstepaction"]
> [Dowiedz się więcej o usługę Azure functions wyzwalaczy i powiązań](functions-triggers-bindings.md)
