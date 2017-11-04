---
title: "Azure DB rozwiązania Cosmos powiązania dla funkcji | Dokumentacja firmy Microsoft"
description: "Zrozumienie sposobu korzystania z bazy danych Azure rozwiązania Cosmos wyzwalaczy i powiązań w funkcji platformy Azure."
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: "funkcje usługi Azure, funkcje, przetwarzania zdarzeń, dynamiczne obliczeń niekorzystającą architektury"
ms.assetid: 3d8497f0-21f3-437d-ba24-5ece8c90ac85
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/19/2017
ms.author: glenga
ms.openlocfilehash: d05c0342e771e229a7175570ad227c4359980990
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2017
---
# <a name="azure-cosmos-db-bindings-for-functions"></a>Azure DB rozwiązania Cosmos powiązania dla funkcji
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule opisano sposób konfigurowania i Zakoduj powiązania bazy danych Azure rozwiązania Cosmos w funkcji platformy Azure. Obsługuje funkcje wyzwolenia należy wprowadzić i dane wyjściowe powiązania dla bazy danych Azure rozwiązania Cosmos.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Aby uzyskać więcej informacji na niekorzystającą przetwarzania danych z bazy danych Azure rozwiązania Cosmos, zobacz [bazy danych Azure rozwiązania Cosmos: pliki bazy danych obliczeniowych, przy użyciu usługi Azure Functions](..\cosmos-db\serverless-computing-database.md).

<a id="trigger"></a>
<a id="cosmosdbtrigger"></a>

## <a name="azure-cosmos-db-trigger"></a>Azure wyzwalacza DB rozwiązania Cosmos

Wyzwalacz DB rozwiązania Cosmos Azure wykorzystuje [Azure rozwiązania Cosmos DB zmiany źródła danych](../cosmos-db/change-feed.md) do nasłuchiwania zmian w partycji. Druga kolekcja, która jest używana do przechowywania wymaga wyzwalacza _dzierżawy_ na partycje.

Zarówno kolekcji monitorowane i kolekcji, która zawiera dzierżawy musi być dostępny dla wyzwalacza do pracy.

Wyzwalacz bazy danych rozwiązania Cosmos Azure obsługuje następujące właściwości:

|Właściwość  |Opis  |
|---------|---------|
|**Typ** | należy wybrać opcję `cosmosDBTrigger`. |
|**Nazwa** | Nazwa zmiennej używany w funkcji kod, który reprezentuje listy dokumentów o zmiany. | 
|**Kierunek** | należy wybrać opcję `in`. Ten parametr jest ustawiany automatycznie, podczas tworzenia wyzwalacza w portalu Azure. |
|**connectionStringSetting** | Nazwa ustawienia aplikacji, które zawiera parametry połączenia używane do nawiązania połączenia konta bazy danych Azure rozwiązania Cosmos monitorowane. |
|**databaseName** | Nazwa bazy danych DB rozwiązania Cosmos Azure z tą kolekcją monitorowane. |
|**collectionName** | Nazwa kolekcji monitorowane. |
| **leaseConnectionStringSetting** | (Opcjonalnie) Nazwa ustawienia aplikacji, które zawiera parametry połączenia do usługi, która posiada kolekcji dzierżawy. Gdy nie są ustawione, `connectionStringSetting` wartość jest używana. Ten parametr ma wartość automatycznie, podczas tworzenia powiązania w portalu. |
| **leaseDatabaseName** | (Opcjonalnie) Nazwa bazy danych, która przechowuje kolekcji używany do przechowywania dzierżawy. Gdy nie są ustawione, wartość `databaseName` ustawienie jest używane. Ten parametr ma wartość automatycznie, podczas tworzenia powiązania w portalu. |
| **leaseCollectionName** | (Opcjonalnie) Nazwa kolekcji używany do przechowywania dzierżawy. Gdy nie są ustawione, wartość `leases` jest używany. |
| **createLeaseCollectionIfNotExists** | (Opcjonalnie) Jeśli wartość `true`, kolekcji dzierżawy jest tworzony automatycznie podczas już nie istnieje. Wartość domyślna to `false`. |
| **leaseCollectionThroughput** | (Opcjonalnie) Definiuje liczbę jednostek żądań można przypisać podczas tworzenia kolekcji dzierżawy. To ustawienie jest tylko do użycia podczas `createLeaseCollectionIfNotExists` ma ustawioną wartość `true`. Ten parametr ma wartość automatycznie, podczas tworzenia powiązania za pomocą portalu.

>[!NOTE] 
>Ciąg połączenia używany do łączenia z kolekcji dzierżawy musi mieć uprawnienia do zapisu.

Te właściwości można ustawić na karcie integracji dla funkcji w portalu Azure lub edytując `function.json` pliku projektu.

## <a name="using-an-azure-cosmos-db-trigger"></a>Za pomocą wyzwalacza bazy danych Azure rozwiązania Cosmos

Ta sekcja zawiera przykłady użyć wyzwalacza bazy danych Azure rozwiązania Cosmos. Przykłady przykładowa metadanych wyzwalacz, który wygląda następująco:

```json
{
  "type": "cosmosDBTrigger",
  "name": "documents",
  "direction": "in",
  "leaseCollectionName": "leases",
  "connectionStringSetting": "<connection-app-setting>",
  "databaseName": "Tasks",
  "collectionName": "Items",
  "createLeaseCollectionIfNotExists": true
}
```
 
Na przykład sposobu tworzenia wyzwalacza bazy danych Azure rozwiązania Cosmos z funkcji aplikacji w portalu, zobacz [Utwórz funkcję wyzwalane przez bazy danych Azure rozwiązania Cosmos](functions-create-cosmos-db-triggered-function.md). 

### <a name="trigger-sample-in-c"></a>Przykładowe wyzwalacza w języku C# #
```cs 
    #r "Microsoft.Azure.Documents.Client"
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    using System;
    public static void Run(IReadOnlyList<Document> documents, TraceWriter log)
    {
        log.Verbose("Documents modified " + documents.Count);
        log.Verbose("First document Id " + documents[0].Id);
    }
```


### <a name="trigger-sample-in-javascript"></a>Przykładowe wyzwalacza w języku JavaScript
```javascript
    module.exports = function (context, documents) {
        context.log('First document Id modified : ', documents[0].id);

        context.done();
    }
```

<a id="docdbinput"></a>

## <a name="documentdb-api-input-binding"></a>Powiązania wejściowego interfejsu API usługi DocumentDB
Powiązania wejściowego interfejsu API usługi DocumentDB pobiera dokument bazy danych Azure rozwiązania Cosmos i przekazuje je do nazwany parametr wejściowy funkcji. Dokument, który można ustalić Identyfikatora oparte na wyzwalacz, który wywołuje funkcję. 

Powiązania wejściowego interfejsu API usługi DocumentDB ma następujące właściwości *function.json*:

|Właściwość  |Opis  |
|---------|---------|
|**Nazwa**     | Nazwa parametru powiązania, który reprezentuje dokument w funkcji.  |
|**Typ**     | należy wybrać opcję `documentdb`.        |
|**databaseName** | Baza danych zawierająca dokumentu.        |
|**collectionName**  | Nazwa kolekcji, która zawiera dokument. |
|**Identyfikator**     | Identyfikator dokumentu do pobrania. Ta właściwość obsługuje powiązania parametrów. Aby dowiedzieć się więcej, zobacz [powiązania niestandardowe właściwości wejściowych w wyrażeniu powiązania](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression). |
|**sqlQuery**     | Zapytanie Azure rozwiązania Cosmos bazy danych SQL, używane do pobierania wielu dokumentów. Zapytanie obsługuje powiązań środowiska uruchomieniowego, takie w przykładzie: `SELECT * FROM c where c.departmentId = {departmentId}`.        |
|**połączenia**     |Nazwa ustawienia aplikacji zawierający parametry połączenia bazy danych Azure rozwiązania Cosmos.        |
|**Kierunek**     | należy wybrać opcję `in`.         |

Nie można jednocześnie ustawić **identyfikator** i **sqlQuery** właściwości. Jeśli nie są skonfigurowane, są pobierane całą kolekcję.

## <a name="using-a-documentdb-api-input-binding"></a>Przy użyciu interfejsu API usługi DocumentDB powiązania wejściowego

* W języku C# i F # funkcjach gdy funkcja kończy się pomyślnie, wszelkie zmiany wprowadzone do dokument wejściowy za pośrednictwem nazwanych parametrów wejściowych automatycznie są zachowywane. 
* W funkcji JavaScript aktualizacje nie będą automatycznie po wyjściu z funkcji. Zamiast tego należy użyć `context.bindings.<documentName>In` i `context.bindings.<documentName>Out` aby aktualizacje. Zobacz [próbki JavaScript](#injavascript).

<a name="inputsample"></a>

## <a name="input-sample-for-single-document"></a>Przykład wejściowego dla pojedynczego dokumentu
Załóżmy, że masz następujące interfejsu API usługi DocumentDB wejściowych powiązanie w `bindings` tablicy function.json:

```json
{
  "name": "inputDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "id" : "{queueTrigger}",
  "connection": "MyAccount_COSMOSDB",     
  "direction": "in"
}
```

Zobacz przykładowe specyficzny dla języka, który używa tego powiązania wejściowe, aby zaktualizować wartości tekstowej dokumentu.

* [C#](#incsharp)
* [F#](#infsharp)
* [JavaScript](#injavascript)

<a name="incsharp"></a>
### <a name="input-sample-in-c"></a>Przykładowe wejściowego w języku C# #

```cs
// Change input document contents using DocumentDB API input binding 
public static void Run(string myQueueItem, dynamic inputDocument)
{   
  inputDocument.text = "This has changed.";
}
```
<a name="infsharp"></a>

### <a name="input-sample-in-f"></a>Przykładowe wejściowego w języku F # #

```fsharp
(* Change input document contents using DocumentDB API input binding *)
open FSharp.Interop.Dynamic
let Run(myQueueItem: string, inputDocument: obj) =
  inputDocument?text <- "This has changed."
```

W tym przykładzie wymaga `project.json` pliku, który określa `FSharp.Interop.Dynamic` i `Dynamitey` zależności NuGet:

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

Aby dodać `project.json` plików, zobacz [zarządzania pakietami F #](functions-reference-fsharp.md#package).

<a name="injavascript"></a>

### <a name="input-sample-in-javascript"></a>Przykładowe wejściowego w języku JavaScript

```javascript
// Change input document contents using DocumentDB API input binding, using context.bindings.inputDocumentOut
module.exports = function (context) {   
  context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
  context.bindings.inputDocumentOut.text = "This was updated!";
  context.done();
};
```

## <a name="input-sample-with-multiple-documents"></a>Przykładowe wejściowy z wieloma dokumentami

Załóżmy, że chcesz pobrać wiele dokumentów określonych przez zapytanie SQL, przy użyciu wyzwalacz kolejki, aby dostosować parametry zapytania. 

W tym przykładzie wyzwalacza kolejki zawiera parametr `departmentId`. Komunikat z kolejki z `{ "departmentId" : "Finance" }` zwróci wszystkie rekordy dotyczące działu finansowego. Należy użyć następującego w *function.json*:

```
{
    "name": "documents",
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connection": "CosmosDBConnection"
}
```

### <a name="input-sample-with-multiple-documents-in-c"></a>Przykładowe wejściowy z wieloma dokumentami w języku C#

```csharp
public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
{   
    foreach (var doc in documents)
    {
        // operate on each document
    }    
}

public class QueuePayload
{
    public string departmentId { get; set; }
}
```

### <a name="input-sample-with-multiple-documents-in-javascript"></a>Przykładowe wejściowy z wieloma dokumentami w języku JavaScript

```javascript
module.exports = function (context, input) {    
    var documents = context.bindings.documents;
    for (var i = 0; i < documents.length; i++) {
        var document = documents[i];
        // operate on each document
    }       
    context.done();
};
```

## <a id="docdboutput"></a>Powiązania wyjściowego interfejsu API usługi DocumentDB
Dane wyjściowe interfejsu API usługi DocumentDB umożliwia powiązanie nowy dokument zapisu do bazy danych Azure DB rozwiązania Cosmos. Ma następujące właściwości w *function.json*:

|Właściwość  |Opis  |
|---------|---------|
|**Nazwa**     | Nazwa parametru powiązania, który reprezentuje dokument w funkcji.  |
|**Typ**     | należy wybrać opcję `documentdb`.        |
|**databaseName** | Baza danych zawierający kolekcję, do której jest tworzony dokumentu.     |
|**collectionName**  | Nazwa kolekcji, których tworzone jest dokumentu. |
|**createIfNotExists**     | Wartość logiczna wskazująca, czy kolekcja jest tworzony, gdy nie istnieje. Wartość domyślna to *false*. Jest to spowodowane nowe kolekcje są tworzone z zarezerwowaną przepływnością, co ma koszt skutki. Aby uzyskać więcej informacji, odwiedź stronę [cennikiem](https://azure.microsoft.com/pricing/details/documentdb/).  |
|**połączenia**     |Nazwa ustawienia aplikacji zawierający parametry połączenia bazy danych Azure rozwiązania Cosmos.        |
|**Kierunek**     | należy wybrać opcję `out`.         |

## <a name="using-a-documentdb-api-output-binding"></a>Powiązania wyjściowego przy użyciu interfejsu API usługi DocumentDB
W tej sekcji przedstawiono sposób użycia interfejsu API usługi DocumentDB dane wyjściowe powiązania w kodzie funkcji.

Domyślnie podczas zapisywania do parametru wyjściowego w funkcji, dokument jest tworzony w bazie danych. Ten dokument ma automatycznie generowanym identyfikatorze GUID jako identyfikatora dokumentu. Identyfikator dokumentu dokumentu wyjściowego można określić, podając `id` właściwości w obiekcie JSON przekazanego do parametru wyjściowego. 

>[!Note]  
>Po określeniu Identyfikatora istniejącego dokumentu pobiera zastąpione przez nowy dokument danych wyjściowych. 

Do wyjściowego wielu dokumentów, można także powiązać `ICollector<T>` lub `IAsyncCollector<T>` gdzie `T` jest jednym z obsługiwanych typów.

<a name="outputsample"></a>

## <a name="documentdb-api-output-binding-sample"></a>Przykładowe powiązania danych wyjściowych interfejsu API usługi DocumentDB
Załóżmy, że masz następujące interfejsu API usługi DocumentDB output powiązanie w `bindings` tablicy function.json:

```json
{
  "name": "employeeDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "createIfNotExists": true,
  "connection": "MyAccount_COSMOSDB",     
  "direction": "out"
}
```

I ma powiązania wejściowego kolejki dla kolejki, który odbiera JSON w następującym formacie:

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

I chcesz utworzyć dokumentów bazy danych Azure rozwiązania Cosmos w następującym formacie, dla każdego rekordu:

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Zobacz przykładowe specyficzny dla języka, który używa tego powiązania danych wyjściowych, aby dodawać dokumenty do bazy danych.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [JavaScript](#outjavascript)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Przykładowe dane wyjściowe w języku C# #

```cs
#r "Newtonsoft.Json"

using System;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
{
  log.Info($"C# Queue trigger function processed: {myQueueItem}");

  dynamic employee = JObject.Parse(myQueueItem);

  employeeDocument = new {
    id = employee.name + "-" + employee.employeeId,
    name = employee.name,
    employeeId = employee.employeeId,
    address = employee.address
  };
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Przykładowe dane wyjściowe w języku F # #

```fsharp
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Employee = {
  id: string
  name: string
  employeeId: string
  address: string
}

let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
  log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
  let employee = JObject.Parse(myQueueItem)
  employeeDocument <-
    { id = sprintf "%s-%s" employee?name employee?employeeId
      name = employee?name
      employeeId = employee?employeeId
      address = employee?address }
```

W tym przykładzie wymaga `project.json` pliku, który określa `FSharp.Interop.Dynamic` i `Dynamitey` zależności NuGet:

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

Aby dodać `project.json` plików, zobacz [zarządzania pakietami F #](functions-reference-fsharp.md#package).

<a name="outjavascript"></a>

### <a name="output-sample-in-javascript"></a>Przykładowe dane wyjściowe w języku JavaScript

```javascript
module.exports = function (context) {

  context.bindings.employeeDocument = JSON.stringify({ 
    id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
    name: context.bindings.myQueueItem.name,
    employeeId: context.bindings.myQueueItem.employeeId,
    address: context.bindings.myQueueItem.address
  });

  context.done();
};
```
