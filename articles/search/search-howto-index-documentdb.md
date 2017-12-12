---
title: "Indeksowanie źródła danych rozwiązania Cosmos bazy danych dla usługi Azure Search | Dokumentacja firmy Microsoft"
description: "W tym artykule przedstawiono sposób tworzenia indeksator usługi Azure Search DB rozwiązania Cosmos w postaci źródła danych."
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: search
ms.date: 08/10/2017
ms.author: eugenesh
ms.openlocfilehash: dfa9bf1661e3a77ae4a2bf51285ab9a8da35e297
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="connecting-cosmos-db-with-azure-search-using-indexers"></a>Łączenie z usługi Azure Search przy użyciu indeksatorów DB rozwiązania Cosmos

Jeśli chcesz wdrożyć środowisko wyszukiwania dużą za pośrednictwem danych DB rozwiązania Cosmos, można użyć indeksator usługi Azure Search do pobierania danych do indeksu usługi Azure Search. W tym artykule firma Microsoft opisano, jak zintegrować bazy danych Azure rozwiązania Cosmos z usługi Azure Search bez konieczności pisania kodu do obsługi infrastruktury indeksowania.

Aby skonfigurować indeksator rozwiązania Cosmos bazy danych, musisz mieć [usługi Azure Search](search-create-service-portal.md)i Utwórz indeks, źródła danych, a na końcu indeksatora. Można utworzyć te obiekty przy użyciu [portal](search-import-data-portal.md), [zestawu .NET SDK](/dotnet/api/microsoft.azure.search), lub [interfejsu API REST](/rest/api/searchservice/) dla wszystkich języków innych niż .NET. 

Jeśli wybierzesz portalu, [Kreatora importu danych](search-import-data-portal.md) prowadzi użytkownika przez utworzenie wszystkich tych zasobów.

> [!NOTE]
> Azure DB rozwiązania Cosmos jest generacji usługi DocumentDB. Mimo że nazwa produktu została zmieniona, składnia jest taka sama jak przed. Przejdź dalej, aby określić `documentdb` zgodnie ze wskazówkami zawartymi w tym artykule indeksatora. 

> [!TIP]
> Możesz uruchomić **importowania danych** kreatora z poziomu pulpitu nawigacyjnego rozwiązania Cosmos DB uprościć indeksowania dla tego źródła danych. Aby rozpocząć, w obszarze nawigacyjnym po lewej stronie przejdź do pozycji **Kolekcje** > **Dodaj usługę Azure Search**.

<a name="Concepts"></a>
## <a name="azure-search-indexer-concepts"></a>Azure pojęcia indeksatora wyszukiwania
Azure obsługuje wyszukiwania, tworzenie i zarządzanie nimi danych źródła (w tym rozwiązania Cosmos bazy danych) i indeksatorów, które działają na tych źródłach danych.

A **źródła danych** Określa dane do indeksu, poświadczenia i zasad do identyfikowania zmian w danych (takich jak dokumenty zmodyfikowany lub usunięty w kolekcji). Źródło danych jest zdefiniowany jako niezależnym zasobem, dzięki czemu mogą być używane przez wiele indeksatorów.

**Indeksatora** w tym artykule opisano, jak przepływ danych ze źródła danych do indeksu wyszukiwania docelowego. Indeksator może służyć do:

* Wykonywać jednorazowe kopię danych do wypełniania indeksu.
* Synchronizowanie indeksu ze zmianami w źródle danych, zgodnie z harmonogramem. Harmonogram jest częścią definicji indeksatora.
* Wywołaj aktualizacje na żądanie do indeksu w razie potrzeby.

<a name="CreateDataSource"></a>
## <a name="step-1-create-a-data-source"></a>Krok 1. Tworzenie źródła danych
Aby utworzyć źródło danych, wykonaj ogłoszenie (POST):

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId", "query": null },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        }
    }

Treść żądania zawiera definicję źródła danych, które powinny zawierać następujące pola:

* **Nazwa**: Nazwa do reprezentowania bazy danych DB rozwiązania Cosmos.
* **Typ**: musi być `documentdb`.
* **poświadczenia**:
  
  * **connectionString**: wymagane. Określ informacje o połączeniu z bazą danych Azure DB rozwiązania Cosmos w następującym formacie:`AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>`
* **kontener**:
  
  * **Nazwa**: wymagane. Określ identyfikator kolekcji DB rozwiązania Cosmos do indeksowania.
  * **Zapytanie**: opcjonalne. Można określić zapytania do spłaszczenia dowolnych dokumentów JSON na płaskiej schemat, który można indeksu usługi Azure Search.
* **dataChangeDetectionPolicy**: zalecane. Zobacz [indeksowania dokumentów zmienić](#DataChangeDetectionPolicy) sekcji.
* **dataDeletionDetectionPolicy**: opcjonalne. Zobacz [indeksowania dokumentów usunięte](#DataDeletionDetectionPolicy) sekcji.

### <a name="using-queries-to-shape-indexed-data"></a>Za pomocą zapytań do kształtu indeksowane danych
Określ zapytanie DB rozwiązania Cosmos do spłaszczenia właściwości zagnieżdżonych lub tablic, właściwości projektu JSON i filtrowanie danych do indeksowania. 

Przykładowy dokument:

    {
        "userId": 10001,
        "contact": {
            "firstName": "andy",
            "lastName": "hoh"
        },
        "company": "microsoft",
        "tags": ["azure", "documentdb", "search"]
    }

Filtr zapytania:

    SELECT * FROM c WHERE c.company = "microsoft" and c._ts >= @HighWaterMark ORDER BY c._ts

Spłaszczanie zapytania:

    SELECT c.id, c.userId, c.contact.firstName, c.contact.lastName, c.company, c._ts FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts
    
    
Projekcja zapytanie:

    SELECT VALUE { "id":c.id, "Name":c.contact.firstName, "Company":c.company, "_ts":c._ts } FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts


Tablica spłaszczania zapytanie:

    SELECT c.id, c.userId, tag, c._ts FROM c JOIN tag IN c.tags WHERE c._ts >= @HighWaterMark ORDER BY c._ts

<a name="CreateIndex"></a>
## <a name="step-2-create-an-index"></a>Krok 2. Tworzenie indeksu
Tworzenie indeksu usługi Azure Search docelowego, jeśli nie masz już. Można utworzyć indeksu przy użyciu [interfejsu użytkownika portalu Azure](search-create-index-portal.md), [tworzenia indeksu interfejsu API REST](/rest/api/searchservice/create-index) lub [indeksu klasy](/dotnet/api/microsoft.azure.search.models.index).

Poniższy przykład tworzy indeks z polem Identyfikator i opis:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

Upewnij się, że schemat indeksu docelowego jest zgodna z schematu dokumentów JSON źródła lub dane wyjściowe z projekcji zapytań niestandardowych.

> [!NOTE]
> Dla kolekcji partycjonowanych domyślny klucz dokumentu jest DB rozwiązania Cosmos `_rid` właściwość, która pobiera zmieniona na `rid` w usłudze Azure Search. Ponadto rozwiązania Cosmos bazy danych w `_rid` wartości zawiera znaki, które nie są prawidłowe w kluczach usługi Azure Search. Z tego powodu `_rid` wartości są kodowany w standardzie Base64.
> 
> 

### <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Mapowanie między typami danych JSON i typy danych w usłudze Azure Search
| TYP DANYCH JSON | TYPY ELEMENTÓW DOCELOWYCH ZGODNY INDEKS POLA |
| --- | --- |
| wartość logiczna |Typem Edm.Boolean, typem Edm.String |
| Numery, które wyglądają jak liczby całkowite |Typem Edm.String z typem Edm.Int32, Edm.Int64, |
| Numery tego wygląd zmiennoprzecinkową punkty |Edm.Double, typem Edm.String |
| Ciąg |Edm.String |
| Tablice typów pierwotnych, na przykład ["a", "b", "c"] |Collection(Edm.String) |
| Ciągi, które wyglądają dat |Edm.DateTimeOffset, typem Edm.String |
| Obiekty GeoJSON, na przykład {"type": "Point", "coordinates": [długie, lat]} |Edm.GeographyPoint |
| Inne obiekty JSON |Nie dotyczy |

<a name="CreateIndexer"></a>
## <a name="step-3-create-an-indexer"></a>Krok 3: Utwórz indeksator

Po utworzeniu indeks i źródło danych, możesz przystąpić do tworzenia indeksatora:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "mydocdbindexer",
      "dataSourceName" : "mydocdbdatasource",
      "targetIndexName" : "mysearchindex",
      "schedule" : { "interval" : "PT2H" }
    }

Ten indeksator jest uruchamiane co dwie godziny (interwał harmonogramu jest ustawiony na "PT2H"). Aby uruchomić indeksatora co 30 minut, należy ustawić interwał o "PT30M". Najkrótszy obsługiwany interwał wynosi 5 minut. Harmonogram jest opcjonalny — w przypadku pominięcia, indeksatora działa tylko wtedy, gdy po jego utworzeniu. Jednak w dowolnym momencie można uruchomić indeksatora na żądanie.   

Aby uzyskać więcej szczegółów na tworzenie interfejsu API indeksatora, zapoznaj się [Utwórz indeksator](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

<a id="RunIndexer"></a>
### <a name="running-indexer-on-demand"></a>Uruchomiona indeksatora na żądanie
Poza uruchamianiem okresowo, zgodnie z harmonogramem, indeksatora może być wywoływana na żądanie:

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2016-09-01
    api-key: [Search service admin key]

> [!NOTE]
> Podczas uruchamiania interfejsu API zwraca pomyślnie, zaplanowano wywołanie indeksatora, ale aktualnie przetwarzanego sytuacji asynchronicznie. 

Możesz monitorować stan indeksator w portalu lub przy użyciu uzyskać indeksatora stan interfejsu API, który opisano dalej. 

<a name="GetIndexerStatus"></a>
### <a name="getting-indexer-status"></a>Pobieranie stanu indeksatora
Można pobrać historii stanu i wykonywanie indeksatora:

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2016-09-01
    api-key: [Search service admin key]

Odpowiedź zawiera ogólny stan indeksatora, wywołanie indeksatora ostatniego (lub w toku) i historię ostatnich wywołań indeksatora.

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Historii wykonywania zawiera maksymalnie 50 najnowszych wykonaniami zakończonych, które są sortowane w kolejności odwrotnej (aby najnowsze wykonywania zostanie osiągnięty jako pierwszy w odpowiedzi).

<a name="DataChangeDetectionPolicy"></a>
## <a name="indexing-changed-documents"></a>Indeksowanie dokumentów zmienione
Zasady wykrywania zmian danych ma na celu wydajnie zidentyfikować elementy zmienione dane. Obecnie jest obsługiwane tylko zasady `High Water Mark` przy użyciu zasad `_ts` właściwości (sygnatury czasowej) udostępniane przez rozwiązania Cosmos bazy danych, które jest określone w następujący sposób:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Aby zapewnić wydajność indeksatora dobrej zdecydowanie zaleca przy użyciu tych zasad. 

Jeśli używasz niestandardowe zapytanie, upewnij się, że `_ts` właściwości jest chroniony przez zapytanie.

<a name="IncrementalProgress"></a>
### <a name="incremental-progress-and-custom-queries"></a>Postęp przyrostowych i zapytań niestandardowych
Przyrostowe postępu podczas indeksowania gwarantuje, że jeśli wykonywanie indeksatora jest przerwane z powodu błędów przejściowych lub limit czasu wykonywania, indeksatora mogą odbierać którym przerwał następnym uruchomieniu, zamiast ponownego indeksowania całą kolekcję od początku. Jest to szczególnie ważne w przypadku, gdy przeprowadzane jest indeksowanie dużych kolekcjach. 

Aby włączyć przyrostowego postępu, za pomocą niestandardowe zapytanie, upewnij się, że kwerenda porządkuje wyniki według `_ts` kolumny. Dzięki temu okresowego sprawdzania wskazującej, która używa usługi Azure Search, aby zapewnić przyrostowego postępu obecności błędów.   

W niektórych przypadkach, nawet jeśli zapytanie zawiera `ORDER BY [collection alias]._ts` klauzuli, wyszukiwanie Azure może nie wnioskować że zapytania są uporządkowane według `_ts`. Można ustalić usługi Azure Search że wyniki są uporządkowane przy użyciu `assumeOrderByHighWaterMarkColumn` właściwości konfiguracji. Aby określić tę wskazówkę, należy utworzyć lub zaktualizować indeksator w następujący sposób: 

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "assumeOrderByHighWaterMarkColumn" : true } }
    } 

<a name="DataDeletionDetectionPolicy"></a>
## <a name="indexing-deleted-documents"></a>Indeksowanie usunięte dokumentów
Jeśli wiersze są usuwane z kolekcji, zwykle chcesz usunąć te wiersze z z indeksu wyszukiwania. Zasady wykrywania usuwania danych ma na celu wydajnie zidentyfikować elementy usunięte dane. Obecnie jest obsługiwane tylko zasady `Soft Delete` zasad (usuwanie jest oznaczony przy użyciu flagi jakiegoś), który został określony w następujący sposób:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

Jeśli używasz niestandardowe zapytanie, upewnij się, że właściwość odwołuje się `softDeleteColumnName` jest chroniony przez zapytanie.

Poniższy przykład tworzy źródła danych za pomocą zasad usuwania nietrwałego:

    POST https://[Search service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId" },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

## <a name="NextSteps"></a>Następne kroki
Gratulacje! Znasz sposobu integracji bazy danych Azure rozwiązania Cosmos z usługi Azure Search przy użyciu indeksatora rozwiązania Cosmos bazy danych.

* Aby dowiedzieć się, jak więcej informacji na temat bazy danych rozwiązania Cosmos platformy Azure, zobacz [stronę usługi bazy danych Azure rozwiązania Cosmos](https://azure.microsoft.com/services/cosmos-db/).
* Aby dowiedzieć się, jak więcej informacji na temat usługi Azure Search, zobacz [strony usługi wyszukiwania](https://azure.microsoft.com/services/search/).
