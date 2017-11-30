---
title: "Powiązania zewnętrznego pliku funkcji platformy Azure (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Przy użyciu powiązań zewnętrznych plików w usługi Azure Functions"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/27/2017
ms.author: alkarche
ms.openlocfilehash: c7a1ff4d4488b37b1969edfbb6f935eccd63413c
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2017
---
# <a name="azure-functions-external-file-bindings-preview"></a>Powiązania zewnętrznego pliku funkcji platformy Azure (wersja zapoznawcza)
W tym artykule przedstawiono sposób manipulowanie plikami od różnych dostawców SaaS (np. OneDrive, Dropbox) w ramach funkcji przy użyciu wbudowanych powiązania. Azure functions obsługuje wyzwolenia, danych wejściowych i wyjściowych powiązania zewnętrznego pliku.

To powiązanie tworzy interfejsu API połączeń z dostawcami SaaS lub wykorzystuje istniejące połączenia interfejsu API z grupy zasobów aplikacji funkcji.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-file-connections"></a>Obsługiwane połączenia plików

|Łącznik|Wyzwalacz|Dane wejściowe|Dane wyjściowe|
|:-----|:---:|:---:|:---:|
|[Pole](https://www.box.com)|x|x|x
|[Skrzynki](https://www.dropbox.com)|x|x|x
|[FTP](https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp)|x|x|x
|[Usługi OneDrive](https://onedrive.live.com)|x|x|x
|[OneDrive dla Firm](https://onedrive.live.com/about/business/)|x|x|x
|[SFTP](https://docs.microsoft.com/azure/connectors/connectors-create-api-sftp)|x|x|x
|[Dysk Google](https://www.google.com/drive/)||x|x|

> [!NOTE]
> Połączenia zewnętrzne pliku można również w [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)

## <a name="external-file-trigger-binding"></a>Plik zewnętrzny wyzwolenia powiązania

Wyzwalacza zewnętrznego plików na platformę Azure umożliwia monitorowanie folderu zdalnego i uruchomić kod funkcja, gdy zostaną wykryte zmiany.

Wyzwalacz zewnętrzny plik wykorzystuje następujące obiekty JSON w `bindings` tablicy function.json

```json
{
  "type": "apiHubFileTrigger",
  "name": "<Name of input parameter in function signature>",
  "direction": "in",
  "path": "<folder to monitor, and optionally a name pattern - see below>",
  "connection": "<name of external file connection - see above>"
}
```
<!---
See one of the following subheadings for more information:

* [Name patterns](#pattern)
* [File receipts](#receipts)
* [Handling poison files](#poison)
--->

<a name="pattern"></a>

### <a name="name-patterns"></a>Wzorce nazw
Można określić wzorzec nazwy pliku, w `path` właściwości. Folder, do których odwołuje się musi istnieć w dostawcy SaaS.
Przykłady:

```json
"path": "input/original-{name}",
```

Ta ścieżka znajdował się plik o nazwie *więc Plik1.txt oryginalne* w *wejściowych* folderu, a wartość `name` byłoby zmiennej w kodzie funkcja `File1.txt`.

Inny przykład:

```json
"path": "input/{filename}.{fileextension}",
```

Ta ścieżka będzie również znaleźć w pliku o nazwie *więc Plik1.txt oryginalne*i wartość `filename` i `fileextension` będzie zmienne w kodzie funkcja *plik1 oryginalne* i *txt*.

Typ pliku plików można ograniczyć przy użyciu wartości stałej dla rozszerzenia pliku. Na przykład:

```json
"path": "samples/{name}.png",
```

W takim przypadku tylko *.png* plików *przykłady* folderu wyzwalanie funkcji.

Nawiasy klamrowe są znaki specjalne w wzorce nazw. Aby określić nazwy plików, które mają nawiasy klamrowe w nazwie, dwukrotnie nawiasów klamrowych.
Na przykład:

```json
"path": "images/{{20140101}}-{name}",
```

Ta ścieżka znajdował się plik o nazwie *{20140101}-soundfile.mp3* w *obrazów* folderu i `name` będzie wartość zmiennej w kodzie funkcja *soundfile.mp3*.

<a name="receipts"></a>

<!--- ### File receipts
The Azure Functions runtime makes sure that no external file trigger function gets called more than once for the same new or updated file.
It does so by maintaining *file receipts* to determine if a given file version has been processed.

File receipts are stored in a folder named *azure-webjobs-hosts* in the Azure storage account for your function app
(specified by the `AzureWebJobsStorage` app setting). A file receipt has the following information:

* The triggered function ("*&lt;function app name>*.Functions.*&lt;function name>*", for example: "functionsf74b96f7.Functions.CopyFile")
* The folder name
* The file type ("BlockFile" or "PageFile")
* The file name
* The ETag (a file version identifier, for example: "0x8D1DC6E70A277EF")

To force reprocessing of a file, delete the file receipt for that file from the *azure-webjobs-hosts* folder manually.
--->
<a name="poison"></a>

### <a name="handling-poison-files"></a>Obsługa plików skażone
W przypadku awarii funkcja wyzwalacza zewnętrznego pliku usługi Azure Functions ponowi próbę tej funkcji do 5 razy domyślnie (w tym pierwszej próby) dla danego pliku.
Jeśli nie wszystkie próby 5, funkcje dodaje komunikat do kolejki magazynu o nazwie *webjob apihubtrigger-poison*. Komunikat z kolejki skażone plików jest obiekt JSON, który zawiera następujące właściwości:

* FunctionId (w formacie  *&lt;funkcja Nazwa aplikacji >*. Funkcje.  *&lt;nazwy funkcji >*)
* Typ pliku
* Nazwa folderu
* Nazwa pliku
* Element ETag (identyfikator wersji pliku, na przykład: "0x8D1DC6E70A277EF")


<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Użycie wyzwalacza
W języku C# funkcji, można powiązać z danymi pliku wejściowego przy użyciu nazwanego parametru w podpisu funkcji tak samo, jak `<T> <name>`.
Gdzie `T` jest typ danych chcesz danych do deserializacji i `paramName` jest nazwa określona w [wyzwolenia JSON](#trigger). W przypadku funkcji Node.js dostęp przy użyciu danych pliku wejściowego `context.bindings.<name>`.

Plik może zostać przeprowadzona deserializacja żadnego z następujących typów:

* Wszelkie [obiektu](https://msdn.microsoft.com/library/system.object.aspx) — jest to przydatne w przypadku danych pliku serializacji JSON.
  W przypadku niestandardowy typ danych wejściowych (np. `FooType`), usługi Azure Functions próbuje deserializowanie danych JSON w sieci określonego typu.
* Parametry - przydatne w przypadku danych pliku tekstowego.

W języku C# funkcji można również wiązać z dowolnej z następujących typów, a środowisko uruchomieniowe Functions podejmuje próbę deserializacji danych plików za pomocą tego typu:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="trigger-sample"></a>Przykładowe wyzwalacza
Załóżmy, że masz następujące function.json, który definiuje wyzwalacza zewnętrznego pliku:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myFile",
            "type": "apiHubFileTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection": "<name of external file connection>"
        }
    ]
}
```

Patrz zaloguje się zawartość każdego pliku, który jest dodawany do folderu monitorowanych próbki specyficzny dla języka.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-usage-in-c"></a>Użycie wyzwalacza w języku C# #

```cs
public static void Run(string myFile, TraceWriter log)
{
    log.Info($"C# File trigger function processed: {myFile}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger usage in F# ##
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-usage-in-nodejs"></a>Użycie wyzwalacza w środowisku Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

<a name="input"></a>

## <a name="external-file-input-binding"></a>Powiązanie danych wejściowych plików zewnętrznych
Powiązania wejściowego pliku zewnętrznego Azure pozwala na użycie pliku z zewnętrznego folderu w funkcji.

Następujące obiekty JSON w korzysta z zewnętrznego pliku dane wejściowe do funkcji `bindings` tablicy function.json:

```json
{
  "name": "<Name of input parameter in function signature>",
  "type": "apiHubFile",
  "direction": "in",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
},
```

Pamiętaj o następujących kwestiach:

* `path`musi zawierać nazwę folderu i nazwę pliku. Na przykład, jeśli masz [wyzwalacza kolejki](functions-bindings-storage-queue.md) w funkcji, można użyć `"path": "samples-workitems/{queueTrigger}"` wskaż plik w `samples-workitems` folder o nazwie, która jest zgodna z nazwą pliku podane w komunikacie wyzwalacza.   

<a name="inputusage"></a>

## <a name="input-usage"></a>Użycie wejściowych
W języku C# funkcji, można powiązać z danymi pliku wejściowego przy użyciu nazwanego parametru w podpisu funkcji tak samo, jak `<T> <name>`.
Gdzie `T` jest typ danych chcesz danych do deserializacji i `paramName` jest nazwa określona w [wejściowych powiązania](#input). W przypadku funkcji Node.js dostęp przy użyciu danych pliku wejściowego `context.bindings.<name>`.

Plik może zostać przeprowadzona deserializacja żadnego z następujących typów:

* Wszelkie [obiektu](https://msdn.microsoft.com/library/system.object.aspx) — jest to przydatne w przypadku danych pliku serializacji JSON.
  W przypadku niestandardowy typ danych wejściowych (np. `InputType`), usługi Azure Functions próbuje deserializowanie danych JSON w sieci określonego typu.
* Parametry - przydatne w przypadku danych pliku tekstowego.

W języku C# funkcji można również wiązać z dowolnej z następujących typów, a środowisko uruchomieniowe Functions podejmuje próbę deserializacji danych plików za pomocą tego typu:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`


<a name="output"></a>

## <a name="external-file-output-binding"></a>Powiązania wyjściowego pliku zewnętrznego
Azure pliku zewnętrznego powiązania danych wyjściowych umożliwia zapisu plików do zewnętrznego folderu w funkcji.

Zewnętrzny plik wyjściowy dla funkcji używa następujących obiektów JSON w `bindings` tablicy function.json:

```json
{
  "name": "<Name of output parameter in function signature>",
  "type": "apiHubFile",
  "direction": "out",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
}
```

Pamiętaj o następujących kwestiach:

* `path`musi zawierać nazwę folderu i nazwę pliku do zapisu. Na przykład, jeśli masz [wyzwalacza kolejki](functions-bindings-storage-queue.md) w funkcji, można użyć `"path": "samples-workitems/{queueTrigger}"` wskaż plik w `samples-workitems` folder o nazwie, która jest zgodna z nazwą pliku podane w komunikacie wyzwalacza.   

<a name="outputusage"></a>

## <a name="output-usage"></a>Użycie danych wyjściowych
W języku C# funkcji, możesz powiązać do pliku wyjściowego przy użyciu nazwanego `out` parametru w podpisu funkcji, takich jak `out <T> <name>`, gdzie `T` jest mają do serializowania danych do typów danych i `paramName` jest nazwa określona w [powiązania wyjściowego](#output). W przypadku funkcji Node.js dostęp przy użyciu pliku wyjściowego `context.bindings.<name>`.

Możesz zapisywać do pliku wyjściowego przy użyciu dowolnego z następujących typów:

* Wszelkie [obiektu](https://msdn.microsoft.com/library/system.object.aspx) — jest to przydatne w przypadku serializacji JSON.
  W przypadku typu danych wyjściowych niestandardowego (np. `out OutputType paramName`), usługi Azure Functions podejmuje próbę serializacji obiektu do postaci JSON. Jeśli parametr wyjściowy ma wartość null, gdy funkcja kończy, środowisko uruchomieniowe Functions tworzy plik jako obiekt null.
* Parametry - (`out string paramName`) przydatne w przypadku danych pliku tekstowego. środowisko uruchomieniowe Functions tworzy plik tylko wtedy, gdy parametr ciągu jest różna od null, gdy funkcja jest kończona.

W języku C# funkcji można również dane wyjściowe do żadnego z następujących typów:

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

<a name="outputsample"></a>

<a name="sample"></a>

## <a name="input--output-sample"></a>Dane wejściowe + przykładowe dane wyjściowe
Załóżmy, że masz następujące function.json, który definiuje [wyzwalacza kolejki magazynu](functions-bindings-storage-queue.md)zewnętrzny plik wejściowy i wyjściowy plik:

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
      "name": "myInputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "<name of external file connection>",
      "direction": "in"
    },
    {
      "name": "myOutputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "<name of external file connection>",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Zobacz przykład specyficzny dla języka, który kopiuje plik wejściowy do pliku wyjściowego.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="usage-in-c"></a>Użycie w języku C# #

```cs
public static void Run(string myQueueItem, string myInputFile, out string myOutputFile, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputFile = myInputFile;
}
```

<!--
<a name="infsharp"></a>
### Input usage in F# ##
```fsharp

```
-->

<a name="innodejs"></a>

### <a name="usage-in-nodejs"></a>Użycie w środowisku Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Dowiedz się więcej o usługę Azure functions wyzwalaczy i powiązań](functions-triggers-bindings.md)
