---
title: "Azure powiązania funkcji Mobile Apps | Dokumentacja firmy Microsoft"
description: "Zrozumienie, jak używać usługi Azure Mobile Apps powiązania w usługi Azure Functions."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "funkcje usługi Azure, funkcje, przetwarzania zdarzeń, dynamiczne obliczeń niekorzystającą architektury"
ms.assetid: faad1263-0fa5-41a9-964f-aecbc0be706a
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/31/2016
ms.author: glenga
ms.openlocfilehash: d2c0e4e233761584bad2df05a8e702e4fc77e84f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-mobile-apps-bindings"></a>Azure powiązania funkcji Mobile Apps
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule opisano sposób konfigurowania i kodu [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) powiązania usługi Azure Functions. Azure Functions obsługuje wejściowa i wyjściowa powiązania dla aplikacji mobilnych.

Wejściowy Mobile Apps i powiązania danych wyjściowych pozwalają [odczytywać i zapisywać do tabel danych](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) w aplikacji mobilnej.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="mobile-apps-input-binding"></a>Powiązania wejściowego Mobile Apps
Powiązania wejściowego Mobile Apps ładuje rekord z punktu końcowego przenośnych tabeli i przekazuje ją do funkcji. C# i F # funkcje wszelkie zmiany wprowadzone do rekordu są automatycznie wysyłane powrót do tabeli, gdy funkcja kończy się pomyślnie.

Dane wejściowe funkcji Mobile Apps używa następujący obiekt JSON w `bindings` tablicy function.json:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "id" : "<Id of the record to retrieve - see below>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "in"
}
```

Pamiętaj o następujących kwestiach:

* `id`może być statyczna, ani może bazować na wyzwalacz, który wywołuje funkcję. Na przykład, jeśli używasz [wyzwalacza kolejki]() dla funkcji, następnie `"id": "{queueTrigger}"` używa wartości ciągu komunikatu w kolejce jako identyfikator rekordu do pobrania.
* `connection`powinna zawierać nazwę ustawienia aplikacji w aplikacji funkcji, który z kolei zawiera adres URL aplikacji mobilnej. Funkcja używa tego adresu URL do skonstruowania wymagane operacje REST względem aplikacji mobilnej. Możesz [utworzyć ustawienie aplikacji w aplikacji funkcji]() zawierający adres URL aplikacji mobilnej (który wygląda podobnie `http://<appname>.azurewebsites.net`), określ nazwę ustawienia aplikacji w `connection` właściwości w Twojej powiązania wejściowego. 
* Należy określić `apiKey` Jeśli możesz [implementować klucz interfejsu API zaplecza aplikacji mobilnej Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), lub [implementować klucz interfejsu API zaplecza aplikacji mobilnej .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). W tym celu należy [utworzyć ustawienie aplikacji w aplikacji funkcji]() zawierający klucz interfejsu API, następnie dodać `apiKey` właściwości w Twojej powiązania wejściowego o nazwie ustawienia aplikacji. 
  
  > [!IMPORTANT]
  > Ten klucz interfejsu API nie może być udostępniany z klientami aplikacji mobilnej. Go tylko należy rozdzielić bezpiecznie klientom po stronie serwera, takie jak usługi Azure Functions. 
  > 
  > [!NOTE]
  > Azure Functions przechowuje informacje dotyczące połączenia, a klucze interfejsu API zgodnie z ustawieniami aplikacji, dzięki czemu nie są sprawdzane w Twoim repozytorium kontroli źródła. To chroni poufne informacje.
  > 
  > 

<a name="inputusage"></a>

## <a name="input-usage"></a>Użycie wejściowych
W tej sekcji przedstawiono sposób użycia Twojego powiązania wejściowego Mobile Apps w kodzie funkcji. 

Po znalezieniu rekordu z określonej tabeli i identyfikator rekordu jest przekazywana do nazwanego [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parametr (lub w środowisku Node.js, są przekazywane do `context.bindings.<name>` obiektu). Jeśli rekord nie zostanie znaleziony, jest parametr `null`. 

W funkcji języka C# i F #, zmiany wprowadzone w danych wejściowych rekordu (parametr wejściowy) są wysyłane automatycznie powrót do tabeli Mobile Apps, gdy funkcja kończy się pomyślnie. W przypadku funkcji Node.js użyj `context.bindings.<name>` uzyskać dostęp do rekordu wejściowego. Nie można zmodyfikować rekord w środowisku Node.js.

<a name="inputsample"></a>

## <a name="input-sample"></a>Przykładowe wejściowych
Załóżmy, że masz następujące function.json, pobierający rekord tabeli aplikacji mobilnej z identyfikatorem komunikatu wyzwalacza w kolejce:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
        "name": "record",
        "type": "mobileTable",
        "tableName": "MyTable",
        "id" : "{queueTrigger}",
        "connection": "My_MobileApp_Url",
        "apiKey": "My_MobileApp_Key",
        "direction": "in"
    }
],
"disabled": false
}
```

Zobacz przykładowe specyficzny dla języka, który korzysta z rekordu wejściowego z powiązania. Przykłady w języku C# i F # również zmodyfikować rekordu `text` właściwości.

* [C#](#inputcsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>Przykładowe wejściowego w języku C# #

```cs
#r "Newtonsoft.Json"    
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, JObject record)
{
    if (record != null)
    {
        record["Text"] = "This has changed.";
    }    
}
```

<!--
<a name="inputfsharp"></a>
### Input sample in F# ## 

```fsharp
#r "Newtonsoft.Json"    
open Newtonsoft.Json.Linq
let Run(myQueueItem: string, record: JObject) =
  inputDocument?text <- "This has changed."
```
-->

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a>Przykładowe wejściowego w środowisku Node.js

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

<a name="output"></a>

## <a name="mobile-apps-output-binding"></a>Aplikacje mobilne powiązania wyjściowego
Za pomocą raportu Mobile Apps powiązanie można zapisać rekordu punktu końcowego tabeli Mobile Apps.  

Aplikacje mobilne, dane wyjściowe dla funkcji używa następujący obiekt JSON w `bindings` tablicy function.json:

```json
{
    "name": "<Name of output parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "out"
}
```

Pamiętaj o następujących kwestiach:

* `connection`powinna zawierać nazwę ustawienia aplikacji w aplikacji funkcji, który z kolei zawiera adres URL aplikacji mobilnej. Funkcja używa tego adresu URL do skonstruowania wymagane operacje REST względem aplikacji mobilnej. Możesz [utworzyć ustawienie aplikacji w aplikacji funkcji]() zawierający adres URL aplikacji mobilnej (który wygląda podobnie `http://<appname>.azurewebsites.net`), określ nazwę ustawienia aplikacji w `connection` właściwości w Twojej powiązania wejściowego. 
* Należy określić `apiKey` Jeśli możesz [implementować klucz interfejsu API zaplecza aplikacji mobilnej Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), lub [implementować klucz interfejsu API zaplecza aplikacji mobilnej .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). W tym celu należy [utworzyć ustawienie aplikacji w aplikacji funkcji]() zawierający klucz interfejsu API, następnie dodać `apiKey` właściwości w Twojej powiązania wejściowego o nazwie ustawienia aplikacji. 
  
  > [!IMPORTANT]
  > Ten klucz interfejsu API nie może być udostępniany z klientami aplikacji mobilnej. Go tylko należy rozdzielić bezpiecznie klientom po stronie serwera, takie jak usługi Azure Functions. 
  > 
  > [!NOTE]
  > Azure Functions przechowuje informacje dotyczące połączenia, a klucze interfejsu API zgodnie z ustawieniami aplikacji, dzięki czemu nie są sprawdzane w Twoim repozytorium kontroli źródła. To chroni poufne informacje.
  > 
  > 

<a name="outputusage"></a>

## <a name="output-usage"></a>Użycie danych wyjściowych
W tej sekcji przedstawiono sposób użycia powiązania w kodzie funkcji Mobile Apps dane wyjściowe. 

W języku C# funkcji, należy użyć parametru wyjściowego o nazwie typu `out object` uzyskać dostęp do rekordu danych wyjściowych. W przypadku funkcji Node.js użyj `context.bindings.<name>` uzyskać dostęp do rekordu danych wyjściowych.

<a name="outputsample"></a>

## <a name="output-sample"></a>Przykładowe dane wyjściowe
Załóżmy, że masz następujące function.json, definiujący wyzwalacz kolejki i wyjścia Mobile Apps:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
    "name": "record",
    "type": "mobileTable",
    "tableName": "MyTable",
    "connection": "My_MobileApp_Url",
    "apiKey": "My_MobileApp_Key",
    "direction": "out"
    }
],
"disabled": false
}
```

Zobacz próbka specyficzny dla języka, która tworzy rekord w punkcie końcowym tabeli aplikacje mobilne z zawartością komunikatu w kolejce.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Przykładowe dane wyjściowe w języku C# #

```cs
public static void Run(string myQueueItem, out object record)
{
    record = new {
        Text = $"I'm running in a C# function! {myQueueItem}"
    };
}
```

<!--
<a name="outfsharp"></a>
### Output sample in F# ## 
```fsharp

```
-->
<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Przykładowe dane wyjściowe w środowisku Node.js

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="next-steps"></a>Następne kroki
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

