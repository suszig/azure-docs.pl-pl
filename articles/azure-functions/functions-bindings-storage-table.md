---
title: "Powiązania tabeli magazynu funkcji platformy Azure | Dokumentacja firmy Microsoft"
description: "Zrozumienie, jak używać usługi Azure Storage powiązania w usługi Azure Functions."
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: "funkcje usługi Azure, funkcje, przetwarzania zdarzeń, dynamiczne obliczeń niekorzystającą architektury"
ms.assetid: 65b3437e-2571-4d3f-a996-61a74b50a1c2
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/28/2016
ms.author: chrande
ms.openlocfilehash: 486b7c31c914ba7bb2d75e3f83ccf346a09104e8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-storage-table-bindings"></a>Azure powiązania tabeli w funkcji magazynu
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule opisano sposób konfigurowania i powiązania tabeli usługi Azure Storage kodu w usługi Azure Functions. Azure Functions obsługuje wejściowa i wyjściowa powiązań dla tabel usługi Azure Storage.

Powiązanie tabeli magazynu obsługuje następujące scenariusze:

* **Przeczytaj pojedynczy wiersz w funkcji języka C# lub Node.js** — skonfiguruj `partitionKey` i `rowKey`. `filter` i `take` właściwości nie są używane w tym scenariuszu.
* **Odczytuje wiele wierszy w funkcji języka C#** — środowisko uruchomieniowe Functions zapewnia `IQueryable<T>` obiekt powiązany z tabelą. Typ `T` musi pochodzić od `TableEntity` , lub zaimplementuj `ITableEntity`. `partitionKey`, `rowKey`, `filter`, I `take` właściwości nie są używane w tym scenariuszu; można użyć `IQueryable` obiekt do dowolnego filtrowanie wymagane. 
* **Odczytuje wiele wierszy w funkcji węzła** — skonfiguruj `filter` i `take` właściwości. Nie należy ustawiać `partitionKey` lub `rowKey`.
* **Zapis co najmniej jeden wiersz w funkcji języka C#** — środowisko uruchomieniowe Functions zapewnia `ICollector<T>` lub `IAsyncCollector<T>` powiązany z tabelą, gdzie `T` Określa schemat jednostek, które chcesz dodać. Zwykle, wpisz `T` pochodzi z `TableEntity` lub implementuje `ITableEntity`, ale nie ma. `partitionKey`, `rowKey`, `filter`, I `take` właściwości nie są używane w tym scenariuszu.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="storage-table-input-binding"></a>Powiązania wejściowego tabeli magazynu
Powiązania wejściowego tabeli magazynu Azure umożliwia korzystanie z tabeli magazynu w funkcji. 

Dane wejściowe funkcji tabeli magazynu używa następujących obiektów JSON w `bindings` tablicy function.json:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "in",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity to read - see below>",
    "rowKey": "<RowKey of table entity to read - see below>",
    "take": "<Maximum number of entities to read in Node.js - optional>",
    "filter": "<OData filter expression for table input in Node.js - optional>",
    "connection": "<Name of app setting - see below>",
}
```

Pamiętaj o następujących kwestiach: 

* Użyj `partitionKey` i `rowKey` ze sobą w celu odczytu pojedynczej jednostki. Te właściwości są opcjonalne. 
* `connection`musi zawierać nazwę ustawienia aplikacji, która zawiera parametry połączenia magazynu. W portalu Azure, standardowego edytora w **integracji** kartę konfiguruje to ustawienie aplikacji do tworzenia magazynu konta lub wybiera istniejący. Możesz również [konfigurowania tej aplikacji ręcznie ustawienie](functions-how-to-use-azure-function-app-settings.md#settings).  

<a name="inputusage"></a>

## <a name="input-usage"></a>Użycie wejściowych
W języku C# funkcji, można powiązać tabeli wejściowej (lub jednostek) przy użyciu nazwanego parametru w podpisu funkcji tak samo, jak `<T> <name>`.
Gdzie `T` jest typ danych chcesz danych do deserializacji i `paramName` jest nazwa określona w [wejściowych powiązania](#input). W przypadku funkcji Node.js dostępu tabeli wejściowej (lub jednostek) przy użyciu `context.bindings.<name>`.

Może być zdeserializowany danych wejściowych w funkcjach Node.js lub C#. Zdeserializowana obiekty mają `RowKey` i `PartitionKey` właściwości.

W języku C# funkcji można również wiązać z dowolnej z następujących typów, a środowisko uruchomieniowe Functions podejmie próbę deserializacji danych tabeli za pomocą tego typu:

* Dowolnego typu, który implementuje`ITableEntity`
* `IQueryable<T>`

<a name="inputsample"></a>

## <a name="input-sample"></a>Przykładowe wejściowych
Powinien mieć następujące function.json, używający wyzwalacz kolejki można odczytać wiersza pojedynczej tabeli. Określa JSON `PartitionKey`  
 `RowKey`. `"rowKey": "{queueTrigger}"`Wskazuje, czy klucz wiersza pochodzą z ciągu kolejki wiadomości.

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
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Zobacz próbka specyficzny dla języka, która odczytuje jednostki pojedynczej tabeli.

* [C#](#inputcsharp)
* [F#](#inputfsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>Przykładowe wejściowego w języku C# #
```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

<a name="inputfsharp"></a>

### <a name="input-sample-in-f"></a>Przykładowe wejściowego w języku F # #
```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a>Przykładowe wejściowego w środowisku Node.js
```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

<a name="output"></a>

## <a name="storage-table-output-binding"></a>Tabela magazynu powiązania wyjściowego
Dane wyjściowe tabeli magazynu Azure umożliwia powiązanie, możesz zapisać jednostek magazynu tabeli w funkcji. 

Tabela magazynu danych wyjściowych dla funkcji używa następujących obiektów JSON w `bindings` tablicy function.json:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "out",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity to write - see below>",
    "rowKey": "<RowKey of table entity to write - see below>",
    "connection": "<Name of app setting - see below>",
}
```

Pamiętaj o następujących kwestiach: 

* Użyj `partitionKey` i `rowKey` ze sobą w celu zapisania pojedynczej jednostki. Te właściwości są opcjonalne. Można również określić `PartitionKey` i `RowKey` podczas tworzenia obiektów jednostek w kodzie funkcji.
* `connection`musi zawierać nazwę ustawienia aplikacji, która zawiera parametry połączenia magazynu. W portalu Azure, standardowego edytora w **integracji** kartę konfiguruje to ustawienie aplikacji do tworzenia magazynu konta lub wybiera istniejący. Możesz również [konfigurowania tej aplikacji ręcznie ustawienie](functions-how-to-use-azure-function-app-settings.md#settings). 

<a name="outputusage"></a>

## <a name="output-usage"></a>Użycie danych wyjściowych
W języku C# funkcji, należy powiązać z tabeli danych wyjściowych przy użyciu nazwanego `out` parametru w podpisu funkcji, takich jak `out <T> <name>`, gdzie `T` jest mają do serializowania danych do typów danych i `paramName` jest nazwa określona w [powiązania wyjściowego](#output). W przypadku funkcji Node.js dostępu tabeli wyjściowe `context.bindings.<name>`.

Może serializować obiektów w funkcjach Node.js lub C#. W języku C# funkcji można powiązać następujących typów:

* Dowolnego typu, który implementuje`ITableEntity`
* `ICollector<T>`(do wyjściowego wiele jednostek. Zobacz [próbki](#outcsharp).)
* `IAsyncCollector<T>`(wersja async `ICollector<T>`)
* `CloudTable`(przy użyciu zestawu SDK usługi Magazyn Azure. Zobacz [próbki](#readmulti).)

<a name="outputsample"></a>

## <a name="output-sample"></a>Przykładowe dane wyjściowe
Następujące *function.json* i *run.csx* przykład przedstawia sposób zapisania wiele jednostek tabeli.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Zobacz próbka specyficzny dla języka, która tworzy wiele jednostek tabeli.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Przykładowe dane wyjściowe w języku C# #
```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```
<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Przykładowe dane wyjściowe w języku F # #
```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Przykładowe dane wyjściowe w środowisku Node.js
```javascript
module.exports = function (context) {

    context.bindings.tableBinding = [];

    for (var i = 1; i < 10; i++) {
        context.bindings.tableBinding.push({
            PartitionKey: "Test",
            RowKey: i.toString(),
            Name: "Name " + i
        });
    }
    
    context.done();
};
```

<a name="readmulti"></a>

## <a name="sample-read-multiple-table-entities-in-c"></a>Przykład: Odczytu wiele jednostek tabeli w języku C#  #
Następujące *function.json* i przykładowy kod w języku C# odczytuje jednostek dla klucza partycji, który jest określony w komunikacie kolejki.

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
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Kod C# dodaje odwołanie do zestawu SDK usługi Magazyn Azure, aby typ jednostki mogą dziedziczyć po `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

## <a name="next-steps"></a>Następne kroki
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

