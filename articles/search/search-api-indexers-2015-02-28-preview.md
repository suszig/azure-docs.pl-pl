---
title: "Operacje indeksatora (interfejsu API REST usługi Azure Search: 2015-02-28-Preview) | Dokumentacja firmy Microsoft"
description: "Operacje indeksatora (interfejsu API REST usługi Azure Search: 2015-02-28-Preview)"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 42b5f85c-8304-4bc7-8e1e-a9c76b8ca25a
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: eugenesh
ms.openlocfilehash: 356ceb98106d080d8c24dedc3547bee33750156e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="indexer-operations-azure-search-service-rest-api-2015-02-28-preview"></a>Operacje indeksatora (interfejsu API REST usługi Azure Search: 2015-02-28-Preview)
> [!NOTE]
> W tym artykule opisano indeksatorów w [interfejsu API REST 2015-02-28-Preview](search-api-2015-02-28-preview.md). Ta wersja interfejsu API dodaje wersji preview programów indeksator magazynu obiektów Blob Azure o wyodrębniania dokumentu indeksatora magazynu tabel Azure, a także inne ulepszenia. Interfejs API obsługuje również ogólnie dostępna indeksatorów (GA), w tym indeksatory dla bazy danych SQL Azure, programu SQL Server na maszynach wirtualnych platformy Azure i bazy danych Azure rozwiązania Cosmos.
> 
> 

## <a name="overview"></a>Omówienie
Wyszukiwanie Azure można łączyć bezpośrednio z niektórych wspólnych źródeł danych, eliminuje konieczność pisania kodu do indeksowania danych. Aby skonfigurować ten w górę, można wywołać interfejsu API wyszukiwania usługi Azure umożliwia tworzenie i zarządzanie **indeksatory** i **źródeł danych**. 

**Indeksatora** jest z zasobem, który łączy źródeł danych z docelowym indeksy wyszukiwania. Indeksator jest używana w następujący sposób: 

* Wykonywać jednorazowe kopię danych do wypełniania indeksu.
* Synchronizowanie indeksu ze zmianami w źródle danych, zgodnie z harmonogramem. Harmonogram jest częścią definicji indeksatora.
* Wywołania na żądanie można zaktualizować indeksu, zgodnie z potrzebami. 

**Indeksatora** jest przydatne, gdy chcesz regularne aktualizacje do indeksu. Można albo ustaw harmonogram wbudowanego jako część definicji indeksatora lub uruchomić ją na żądanie przy użyciu [uruchomić indeksatora](#RunIndexer). 

A **źródła danych** Określa, jakie dane muszą zostać pomyślnie zindeksowane, poświadczenia dostępu do danych i zasady w celu włączenia usługi Azure Search wydajnie zidentyfikować zmiany danych (takie jak zmodyfikowane lub usunięte wiersze w tabeli bazy danych). Jest on zdefiniowany jako niezależnym zasobem, dzięki czemu mogą być używane przez wiele indeksatorów.

Obecnie obsługiwane są następujące źródła danych:

* **Baza danych Azure SQL** i **programu SQL Server na maszynach wirtualnych Azure**. Dla docelowej przewodnika [w tym artykule](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md). 
* **Rozwiązania Cosmos Azure DB**. Dla docelowej przewodnika [w tym artykule](search-howto-index-documentdb.md). 
* **Magazyn obiektów Blob Azure**, w tym następujące dokumentu formaty: PDF, Microsoft Office (DOCX/DOC, XSLX/XLS, PPTX/PPT, MSG), HTML, XML, ZIP i zwykły tekst plików (w tym JSON). Dla docelowej przewodnika [w tym artykule](search-howto-indexing-azure-blob-storage.md).
* **Magazyn tabel Azure**. Dla docelowej przewodnika [w tym artykule](search-howto-indexing-azure-tables.md).

Firma Microsoft jest uwzględnieniu obsługę dodatkowych źródeł danych w przyszłości. W celu ułatwienia określenia priorytetu tych decyzji, podaj swoją opinię na [forum opinii usługi Azure Search](http://feedback.azure.com/forums/263029-azure-search).

Zobacz [ograniczenia usługi](search-limits-quotas-capacity.md) maksymalną limitów związane z zasobami źródła danych i indeksatora.

## <a name="typical-usage-flow"></a>Typowy sposób przepływu
Możesz utworzyć i zarządzać indeksatorów i źródeł danych za pomocą prostego żądania HTTP (POST, GET, PUT, DELETE) przed danym `data source` lub `indexer` zasobów.

Konfigurowanie automatycznego indeksowania jest zwykle proces krok 4:

1. Określ źródło danych, który zawiera dane, które muszą zostać pomyślnie zindeksowane. Należy pamiętać, że usługi Azure Search może nie obsługiwać wszystkich typów danych w źródle danych. Zobacz [obsługiwane typy danych](https://msdn.microsoft.com/library/azure/dn798938.aspx) dla listy.
2. Tworzenie indeksu usługi Azure Search, do którego schemat jest zgodny z źródła danych.
3. Utwórz źródło danych usługi Azure Search, zgodnie z opisem w [Utwórz źródło danych](#CreateDataSource).
4. Utwórz indeksator usługi Azure Search, zgodnie z opisem [Utwórz indeksator](#CreateIndexer).

Należy zaplanować na tworzenie jeden indeksator dla każdej kombinacji źródła docelowej, jak indeksu i danych. Może mieć wiele indeksatorów zapisu do tego samego indeksu, a następnie można ponownie użyć tego samego źródła danych dla wielu indeksatorów. Jednak indeksatora może używać tylko jednego źródła danych w czasie i można zapisywać tylko jeden indeks. 

Po utworzeniu indeksatora, można pobrać jej wykonanie stanu przy użyciu [pobierania stanu indeksatora](#GetIndexerStatus) operacji. Można również uruchomić na dowolnym czasie (zamiast lub oprócz uruchomienie jej okresowo zgodnie z harmonogramem) przy użyciu indeksatora [uruchomić indeksatora](#RunIndexer) operacji.

<!-- MSDN has 2 art files plus a API topic link list -->


## <a name="create-data-source"></a>Utwórz źródło danych
W usłudze Azure Search źródło danych jest używany z indeksatorów, dostarczanie informacji połączenia odświeżania danych ad hoc ani zaplanowanego indeksu docelowego. Można utworzyć nowego źródła danych w ramach usługi Azure Search przy użyciu żądania HTTP POST.

    POST https://[service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Alternatywnie można użyć PUT i określ nazwę źródła danych w identyfikatorze URI. Źródło danych nie istnieje, zostanie utworzona.

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]

> [!NOTE]
> Maksymalna liczba źródeł danych, dozwolone jest zależna od warstwy cenowej. Umożliwia bezpłatnej usługi maksymalnie 3 źródeł danych. Standardowa usługa umożliwia 50 źródeł danych. Zobacz [ograniczenia usługi](search-limits-quotas-capacity.md) szczegółowe informacje.
> 
> 

**Żądanie**

HTTPS jest wymagana dla wszystkich żądań obsługi. **Utwórz źródło danych** żądania może być skonstruowany przy użyciu metody POST i PUT. Korzystając z POST, musisz podać nazwę źródła danych w treści żądania wraz z definicji źródła danych. Z PUT nazwa to część adresu URL. Jeśli źródło danych nie istnieje, jest tworzony. Jeśli już istnieje, jest aktualizowana do nowej definicji. 

Nazwa źródła danych muszą być małe litery, zaczynać się literą lub cyfrą nie ukośniki lub kropki i mniej niż 128 znaków. Po uruchomieniu nazwa źródła danych z literą lub cyfrą, pozostałą część nazwy mogą zawierać żadnych list, numer i kreski, tak długo, jak nie są następujących po sobie kresek. Zobacz [reguły nazewnictwa](https://msdn.microsoft.com/library/azure/dn857353.aspx) szczegółowe informacje.

`api-version` Jest wymagana. Bieżąca wersja to `2015-02-28`.

**Nagłówki żądania**

Poniższa lista zawiera nagłówki żądań wymaganych i opcjonalnych. 

* `Content-Type`: Wymagane. Ustaw tę wartość na`application/json`
* `api-key`: Wymagane. `api-key` Służy do uwierzytelniania żądań do usługi wyszukiwania. Jest to wartość ciągu, unikatowe z usługą. **Utwórz źródło danych** żądanie musi zawierać `api-key` ustawione na wartość klucza administratora (w przeciwieństwie do klucza zapytania). 

Należy również nazwę usługi do konstruowania adresu URL żądania. Można uzyskać nazwy usługi i `api-key` z pulpitu nawigacyjnego usługi w [Azure Portal](https://portal.azure.com/). Zobacz [Utwórz usługę wyszukiwania, w portalu](search-create-service-portal.md) dla pomocy nawigacji strony.

<a name="CreateDataSourceRequestSyntax"></a>
**Składnia treść żądania**

Treść żądania zawiera definicję źródła danych, w tym poświadczenia, które mają być odczytywane dane, a także opcjonalnymi danymi zmienić wykrywania i zasady wykrywania usunięcia danych, które są używane do identyfikowania wydajnie lub usunięcie danych w źródle danych, gdy jest używany z okresowo zaplanowane indeksatora typu źródła danych. 

Składnia struktury ładunku żądania ma następującą składnię. Przykładowe żądanie znajduje się bardziej szczegółowo na, w tym temacie.

    { 
        "name" : "Required for POST, optional for PUT. The name of the data source",
        "description" : "Optional. Anything you want, or nothing at all",
        "type" : "Required. Must be one of 'azuresql', 'documentdb', 'azureblob', or 'azuretable'",
        "credentials" : { "connectionString" : "Required. Connection string for your data source" },
        "container" : { "name" : "Required. The name of the table, collection, or blob container you wish to index" },
        "dataChangeDetectionPolicy" : { Optional. See below for details }, 
        "dataDeletionDetectionPolicy" : { Optional. See below for details }
    }

Żądanie zawiera następujące właściwości: 

* `name`: Wymagane. Nazwa źródła danych. Nazwa źródła danych musi jedynie zawierać małe litery, cyfry i łączniki, nie może zaczynać się ani kończyć kreski i może zawierać maksymalnie 128 znaków.
* `description`: Opcjonalny opis. 
* `type`: Wymagane. Musi mieć jedną z obsługiwanych typów źródła:
  * `azuresql`-Azure SQL Database lub SQL Server na maszynach wirtualnych Azure
  * `documentdb`-DB azure rozwiązania Cosmos
  * `azureblob`-Azure Blob Storage
  * `azuretable`-Azure Table Storage
* `credentials`:
  * Wymagane `connectionString` właściwości określa parametry połączenia dla źródła danych. Format ciągu połączenia jest zależna od typu źródła danych: 
    * Dla bazy danych SQL Azure jest to zwykle parametry połączenia SQL Server. Jeśli używasz portalu Azure można pobrać ciągu połączenia, należy użyć `ADO.NET connection string` opcji.
    * Dla bazy danych Azure rozwiązania Cosmos, ciąg połączenia musi być w następującym formacie: `"AccountEndpoint=https://[your account name].documents.azure.com;AccountKey=[your account key];Database=[your database id]"`. Wszystkie wartości są wymagane. Można je znaleźć w [portalu Azure](https://portal.azure.com/).  
    * W przypadku obiektów Blob platformy Azure i Magazyn tabel jest parametry połączenia konta magazynu. Format jest opisany [tutaj](https://azure.microsoft.com/documentation/articles/storage-configure-connection-string/). Wymagany jest protokół punktu końcowego protokołu HTTPS.  
* `container`, wymagane: Określa dane do indeksu przy użyciu `name` i `query` właściwości: 
  * `name`Wymagane:
    * Azure SQL: Określa tabelę lub widok. Można użyć nazwy kwalifikowanej schematu, takie jak `[dbo].[mytable]`.
    * DocumentDB: Określa kolekcję. 
    * Azure Blob Storage: Określa kontener magazynu.
    * Magazyn tabel Azure: Określa nazwę tabeli. 
  * `query`, opcjonalne:
    * Usługa DocumentDB: umożliwia określenie kwerendę, która spłaszcza dowolnego układu dokumentu JSON na płaskiej schemat, który można indeksu usługi Azure Search.  
    * Azure Blob Storage: umożliwia określenie folder wirtualny w kontenerze obiektów blob. Na przykład dla obiekt blob ścieżki `mycontainer/documents/blob.pdf`, `documents` mogą być używane jako folder wirtualny.
    * Magazyn tabel Azure: umożliwia określenie zapytanie filtrujące zestawu wierszy do zaimportowania.
    * Azure SQL: zapytanie nie jest obsługiwane. Jeśli chcesz dodać tę funkcję, proszę Zagłosuj na [sugestia ta](https://feedback.azure.com/forums/263029-azure-search/suggestions/9893490-support-user-provided-query-in-sql-indexer)
* Opcjonalny `dataChangeDetectionPolicy` i `dataDeletionDetectionPolicy` właściwości są opisane poniżej.

<a name="DataChangeDetectionPolicies"></a>
**Zasady wykrywania zmian danych**

Zasady wykrywania zmian danych ma na celu wydajnie zidentyfikować elementy zmienione dane. Obsługiwane zasady różnić w zależności od typu źródła danych. Poniższe rozdziały zawierają opis poszczególnych zasad. 

***Zasady wykrywania zmian górnego limitu*** 

Użyj tych zasad, gdy źródło danych zawiera kolumny lub właściwości, które spełnia następujące kryteria:

* Wstawia wszystkie Określ wartość dla kolumny. 
* Wszystkie aktualizacje do elementu również zmienić wartość kolumny. 
* Przy każdej zmianie powoduje zwiększenie wartości tej kolumny.
* Zapytania korzystających z klauzuli filtru podobny do następującego `WHERE [High Water Mark Column] > [Current High Water Mark Value]` mogą być wykonywane wydajnie.

Na przykład używając źródeł danych Azure SQL, indeksowanego `rowversion` kolumna jest idealny kandydatem do użycia z z zasadami, znacznik limitu górnego. 

Te zasady można określić w następujący sposób:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
    } 

Podczas korzystania z bazy danych Azure rozwiązania Cosmos źródeł danych, należy użyć `_ts` właściwości udostępniane przez bazy danych Azure rozwiązania Cosmos. 

Używając źródeł danych obiektów Blob platformy Azure, Azure Search automatycznie używa górny limit zmienić zasady wykrywania oparte na sygnatura czasowa ostatniej modyfikacji obiektu blob; nie trzeba samodzielnie określić takie zasady.   

***Zasady wykrywania zmian zintegrowane ze środowiskiem SQL***

Jeśli baza danych SQL obsługuje [śledzenie zmian](https://msdn.microsoft.com/library/bb933875.aspx), firma Microsoft zaleca używanie SQL zintegrowane zmienić zasady śledzenia. Ta zasada umożliwia śledzenie zmian najbardziej efektywny i umożliwia wyszukiwanie Azure, aby zidentyfikować usuniętych wierszy bez konieczności ma jawne "usuwania nietrwałego" kolumny w schemat.

Zintegrowane śledzenie zmian jest obsługiwane począwszy od następujących wersji bazy danych programu SQL Server: 

* SQL Server 2008 R2, jeśli używasz programu SQL Server na maszynach wirtualnych platformy Azure.
* Azure SQL Database V12, jeśli używasz bazy danych SQL Azure.  

Gdy za pomocą zintegrowanego śledzenia zmian programu SQL zasad, nie określaj odrębne zasady usuwania i wykrywania — ta zasada ma wbudowaną obsługę identyfikowanie usuniętych wierszy. 

Te zasady można używać tylko z tabelami; Nie można używać z widokami. Musisz włączyć śledzenia zmian dla tabeli używanej przed użyciem tych zasad. Zobacz [włączyć lub wyłączyć śledzenie zmian](https://msdn.microsoft.com/library/bb964713.aspx) instrukcje.    

Przy tworzeniu struktury **Utwórz źródło danych** poprosić, SQL zintegrowane zasad śledzenia zmian można określić w następujący sposób:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
    }

<a name="DataDeletionDetectionPolicies"></a>
**Zasady wykrywania usunięcia danych**

Zasady wykrywania usuwania danych ma na celu wydajnie zidentyfikować elementy usunięte dane. Obecnie jest obsługiwane tylko zasady `Soft Delete` zasada, która umożliwia zidentyfikowanie elementów usuniętych na podstawie wartości z `soft delete` kolumny lub właściwości w źródle danych. Te zasady można określić w następujący sposób:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the column that specifies whether a row was deleted", 
        "softDeleteMarkerValue" : "the value that identifies a row as deleted" 
    }

> [!NOTE]
> Obsługiwane są tylko kolumny z ciągiem, liczbą całkowitą lub wartości logiczne. Wartość używana jako `softDeleteMarkerValue` musi być ciągiem, nawet wtedy, gdy odpowiednia kolumna zawiera liczb całkowitych lub wartości logiczne. Na przykład, jeśli wartość, która jest wyświetlana w źródle danych 1, użyj `"1"` jako `softDeleteMarkerValue`.    
> 
> 

<a name="CreateDataSourceRequestExamples"></a>
**Przykłady treść żądania**

Jeśli zamierzasz używać źródła danych z indeksatora, który jest uruchamiany zgodnie z harmonogramem, w tym przykładzie pokazano, jak określać zasady wykrywania zmian i usuwanie: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy", "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }

Jeśli zamierzasz używać źródła danych dla jednorazowych kopii danych, można pominąć zasady:

    { 
        "name" : "asqldatasource",
        "description" : "anything you want, or nothing at all",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" }
    } 

**Odpowiedź**

Żądanie powiodło się: 201 utworzono. 

<a name="UpdateDataSource"></a>

## <a name="update-data-source"></a>Aktualizacja źródła danych
Można aktualizować istniejącego źródła danych za pomocą żądania HTTP PUT. Należy określić nazwę źródła danych, aby zaktualizować na identyfikator URI żądania:

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

`api-version` Jest wymagana. Bieżąca wersja to `2015-02-28`. [Azure wersji interfejsu API Search](https://msdn.microsoft.com/library/azure/dn864560.aspx) ma szczegóły i dodatkowe informacje o alternatywne wersje.

`api-key` Musi być klucza administratora (w przeciwieństwie do klucza zapytania). Zapoznaj się z rozdziałem uwierzytelniania w [interfejsu API REST usługi wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) Aby dowiedzieć się więcej na temat kluczy. [Utwórz usługę wyszukiwania, w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i klucza właściwości używane w żądaniu.

**Żądanie**

Składnia treść żądania jest taka sama, jak w przypadku [Utwórz źródło danych żądań](#CreateDataSourceRequestSyntax).

Nie można zaktualizować niektóre właściwości w istniejącym źródle danych. Na przykład nie można zmienić typu istniejącego źródła danych.  

Jeśli nie chcesz zmienić parametry połączenia dla istniejącego źródła danych, można określić literał `<unchanged>` ciągu połączenia. Jest to przydatne w sytuacji, w którym należy zaktualizować danych źródłowych, ale nie mieć pełny dostęp do ciągu połączenia, ponieważ są to dane dotyczące zabezpieczeń.

**Odpowiedź**

Dla pomyślnego żądania: 201 utworzono Jeśli nowego źródła danych został utworzony i 204 nr zawartości, jeśli źródło danych zostało zaktualizowane.

<a name="ListDataSource"></a>

## <a name="list-data-sources"></a>Lista źródeł danych
**Listy źródeł danych** operacji zwraca listę źródeł danych w usłudze Azure Search. 

    GET https://[service name].search.windows.net/datasources?api-version=[api-version]
    api-key: [admin key]

`api-version` Jest wymagana. Bieżąca wersja to `2015-02-28`. 

`api-key` Musi być klucza administratora (w przeciwieństwie do klucza zapytania). Zapoznaj się z rozdziałem uwierzytelniania w [interfejsu API REST usługi wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) Aby dowiedzieć się więcej na temat kluczy. [Utwórz usługę wyszukiwania, w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i klucza właściwości używane w żądaniu.

**Odpowiedź**

Żądanie powiodło się: 200 OK.

Oto przykład treści odpowiedzi:

    {
      "value" : [
        {
          "name": "datasource1",
          "type": "azuresql",
          ... other data source properties
        }]
    }

Należy pamiętać, filtrować odpowiedzi dół tylko właściwości, które są zainteresowani. Na przykład, jeśli chcesz tylko listę nazw źródeł danych, użyj OData `$select` opcji zapytania:

    GET /datasources?api-version=205-02-28&$select=name

W takim przypadku odpowiedzi z powyższym przykładzie będzie wyglądać następująco: 

    {
      "value" : [ { "name": "datasource1" }, ... ]
    }

Jest to przydatne technika, aby oszczędzić przepustowość, jeśli masz wiele źródeł danych w usłudze wyszukiwania.

<a name="GetDataSource"></a>

## <a name="get-data-source"></a>Pobierz źródła danych
**Pobrać źródła danych** operacji definicji źródła danych są pobierane z usługi Azure Search.

    GET https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

`api-version` Jest wymagana. Bieżąca wersja to `2015-02-28`. 

`api-key` Musi być klucza administratora (w przeciwieństwie do klucza zapytania). Zapoznaj się z rozdziałem uwierzytelniania w [interfejsu API REST usługi wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) Aby dowiedzieć się więcej na temat kluczy. [Utwórz usługę wyszukiwania, w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i klucza właściwości używane w żądaniu.

**Odpowiedź**

Kod stanu: 200 OK jest zwracana dla pomyślnej odpowiedzi.

Odpowiedź jest podobne do w [Utwórz źródło danych żądań przykład](#CreateDataSourceRequestExamples): 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted", 
            "softDeleteMarkerValue" : "true" }
    }

> [!NOTE]
> Nie ustawiaj `Accept` nagłówek żądania do `application/json;odata.metadata=none` po wywołujący ten interfejs API jako w ten sposób spowoduje, że `@odata.type` atrybut można pominąć odpowiedzi i nie będzie można odróżnić zmiany danych i zasad wykrywania usunięcia danych o różnych typach. 
> 
> 

<a name="DeleteDataSource"></a>

## <a name="delete-data-source"></a>Usunąć źródła danych
**Usunąć źródło danych** operacji usuwa źródła danych z usługi Azure Search.

    DELETE https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> Jeśli wszystkie indeksatory odwołuje się do źródła danych, które są usuwane, nadal będzie kontynuowana operacji usuwania. Jednak te indeksatory przechodzi w stan błędu po jego następnym uruchomieniu.  
> 
> 

`api-version` Jest wymagana. Bieżąca wersja to `2015-02-28`. 

`api-key` Musi być klucza administratora (w przeciwieństwie do klucza zapytania). Zapoznaj się z rozdziałem uwierzytelniania w [interfejsu API REST usługi wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) Aby dowiedzieć się więcej na temat kluczy. [Utwórz usługę wyszukiwania, w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i klucza właściwości używane w żądaniu.

**Odpowiedź**

Kod stanu: 204 nr zawartości jest zwracana dla pomyślnej odpowiedzi.

<a name="CreateIndexer"></a>

## <a name="create-indexer"></a>Utwórz indeksator
Można utworzyć nowego indeksatora w ramach usługi Azure Search przy użyciu żądania HTTP POST.

    POST https://[service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Alternatywnie można użyć PUT i określ nazwę źródła danych w identyfikatorze URI. Źródło danych nie istnieje, zostanie utworzona.

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]

> [!NOTE]
> Maksymalna liczba indeksatorów dozwolone jest zależna od warstwy cenowej. Bezpłatnej usługi umożliwia indeksatory do 3. Standardowa usługa umożliwia indeksatory 50. Zobacz [ograniczenia usługi](search-limits-quotas-capacity.md) szczegółowe informacje.
> 
> 

`api-version` Jest wymagana. Bieżąca wersja to `2015-02-28`. 

`api-key` Musi być klucza administratora (w przeciwieństwie do klucza zapytania). Zapoznaj się z rozdziałem uwierzytelniania w [interfejsu API REST usługi wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) Aby dowiedzieć się więcej na temat kluczy. [Utwórz usługę wyszukiwania, w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i klucza właściwości używane w żądaniu.

<a name="CreateIndexerRequestSyntax"></a>
**Składnia treść żądania**

Treść żądania zawiera definicję indeksatora, który określa źródło danych i indeksu docelowego indeksowania, a także opcjonalnie harmonogram indeksowania i parametrów. 

Składnia struktury ładunku żądania ma następującą składnię. Przykładowe żądanie znajduje się bardziej szczegółowo na, w tym temacie.

    { 
        "name" : "Required for POST, optional for PUT. The name of the indexer",
        "description" : "Optional. Anything you want, or null",
        "dataSourceName" : "Required. The name of an existing data source",
        "targetIndexName" : "Required. The name of an existing index",
        "schedule" : { Optional. See Indexing Schedule below. },
        "parameters" : { Optional. See Indexing Parameters below. },
        "fieldMappings" : { Optional. See Field Mappings below. },
        "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.  
    }

**Planowanie uruchamiania indeksatora**

Indeksator Opcjonalnie można określić harmonogramu. Jeśli harmonogram, indeksator będzie uruchamiany okresowo zgodnie z harmonogramem. Harmonogram ma następujące atrybuty:

* `interval`: Wymagane. Uruchamia wartości czasu trwania, który określa interwał lub okres indeksatora. Najmniejszy dopuszczalny interwał wynosi 5 minut; najdłuższej to jeden dzień. Musi być sformatowany jako wartość XSD "dayTimeDuration" (ograniczony podzestaw [czasu trwania ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) wartości). Wzór: `"P[nD][T[nH][nM]]"`. Przykłady: `PT15M` co 15 minut, `PT2H` co 2 godziny. 
* `startTime`: Wymagane. Element datetime UTC podczas indeksatora powinny uruchomione. 

**Parametry indeksatora**

Indeksator Opcjonalnie można określić kilka parametrów, które mają wpływ na jego zachowania. Wszystkie parametry są opcjonalne.  

* `maxFailedItems`: Liczba elementów, które mogą nie zostać pomyślnie zindeksowane, zanim uruchomienia indeksatora zostanie uznane za błąd. Domyślna to 0. Zwraca informacje o elementach nie powiodło się [pobierania stanu indeksatora](#GetIndexerStatus) operacji. 
* `maxFailedItemsPerBatch`: Liczba elementów, które mogą nie zostać pomyślnie zindeksowane w każdej z partii, zanim uruchomienia indeksatora zostanie uznane za błąd. Domyślna to 0.
* `base64EncodeKeys`: Określa, czy klucze dokumentu będą algorytmem base-64. Wyszukiwanie Azure nakłada ograniczenia dotyczące znaków, które mogą być obecne w kluczu dokumentu. Jednak wartości w źródle danych może zawierać znaków, które nie są prawidłowe. Jeśli zachodzi konieczność indeksu takie wartości jako klucze dokumentu, można ustawić tej flagi na wartość true. Domyślnie jest `false`.
* `batchSize`: Określa liczbę elementów, które ze źródła danych do odczytu i indeksowane jako pojedyncza partia w celu zwiększenia wydajności. Wartość domyślna zależy od typu źródła danych: 1000 Azure SQL i bazy danych Azure rozwiązania Cosmos i 10 dla magazynu obiektów Blob Azure.

**Mapowania pól**

Mapowania pól służy do mapowania nazwa pola źródła danych na inną nazwę pola w indeksie docelowego. Rozważmy na przykład tabeli źródłowej z polem `_id`. Usługa Azure Search nie zezwala na nazwę pola, począwszy od znaku podkreślenia, można zmienić nazwy pola. Można to zrobić przy użyciu `fieldMappings` właściwości indeksatora w następujący sposób: 

    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 

Można określić wiele mapowań pól: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "_id", "targetFieldName" : "id" },
        { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" },
     ]

Nazwy pól w pliku źródłowym i docelowym jest rozróżniana wielkość liter.

<a name="FieldMappingFunctions"></a>
***Funkcje mapowania pól***

Mapowania pól mogą służyć do przekształcenia wartości pól źródła przy użyciu *mapowania funkcji*.

Tylko jeden taki funkcja jest obecnie obsługiwany: `jsonArrayToStringCollection`. Analizuje pola, które zawiera ciąg sformatowany jako tablica JSON pola Collection(Edm.String) indeksu docelowego. Ma ona do użycia z programem Azure SQL indeksatora w szczególności, ponieważ SQL nie ma typu natywnego kolekcji danych. Mogą być używane w następujący sposób: 

    "fieldMappings" : [ { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } } ] 

Na przykład, jeśli pole źródła zawiera ciąg `["red", "white", "blue"]`, następnie pola docelowego typu `Collection(Edm.String)` zostaną wypełnione wartościami trzy `"red"`, `"white"` i `"blue"`.

Należy pamiętać, że `targetFieldName` właściwość jest opcjonalna; Jeśli po lewej, `sourceFieldName` wartość jest używana. 

<a name="CreateIndexerRequestExamples"></a>
**Przykłady treść żądania**

Poniższy przykład tworzy indeksatorze, który kopiuje dane z tabeli przywoływanej przez `ordersds` źródła danych do `orders` indeksu zgodnie z harmonogramem, który rozpoczyna się 1 stycznia 2015 UTC i jest uruchamiana co godzinę. Każde wywołanie indeksatora zakończy się pomyślnie, jeśli nie więcej niż 5 elementów nie zostać pomyślnie zindeksowane w każdej z partii, a nie więcej niż 10 elementów nie zostać pomyślnie zindeksowane w sumie. 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }

**Odpowiedź**

201 utworzono dla pomyślnego żądania.

<a name="UpdateIndexer"></a>

## <a name="update-indexer"></a>Aktualizacja indeksatora
Można aktualizować istniejącego indeksatora za pomocą żądania HTTP PUT. Należy określić nazwę indeksatora, aby zaktualizować na identyfikator URI żądania:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

`api-version` Jest wymagana. Bieżąca wersja to `2015-02-28`. 

`api-key` Musi być klucza administratora (w przeciwieństwie do klucza zapytania). Zapoznaj się z rozdziałem uwierzytelniania w [interfejsu API REST usługi wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) Aby dowiedzieć się więcej na temat kluczy. [Utwórz usługę wyszukiwania, w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i klucza właściwości używane w żądaniu.

**Żądanie**

Składnia treść żądania jest taka sama, jak w przypadku [żądań tworzenia indeksatora](#CreateIndexerRequestSyntax).

**Odpowiedź**

Dla pomyślnego żądania: 201 utworzono, jeśli nowy indeksator został utworzony i 204 nr zawartości jeśli istniejącego indeksatora został zaktualizowany.

<a name="ListIndexers"></a>

## <a name="list-indexers"></a>Indeksatory listy
**Indeksatory listy** operacji zwraca listę indeksatorów w usłudze Azure Search. 

    GET https://[service name].search.windows.net/indexers?api-version=[api-version]
    api-key: [admin key]


`api-version` Jest wymagana. Wersja zapoznawcza jest `2015-02-28-Preview`. [Azure versioning wyszukiwania](https://msdn.microsoft.com/library/azure/dn864560.aspx) ma szczegóły i dodatkowe informacje o alternatywne wersje.

`api-key` Musi być klucza administratora (w przeciwieństwie do klucza zapytania). Zapoznaj się z rozdziałem uwierzytelniania w [interfejsu API REST usługi wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) Aby dowiedzieć się więcej na temat kluczy. [Utwórz usługę wyszukiwania, w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i klucza właściwości używane w żądaniu.

**Odpowiedź**

Żądanie powiodło się: 200 OK.

Oto przykład treści odpowiedzi:

    {
      "value" : [
      {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        ... other indexer properties
      }]
    }

Należy pamiętać, filtrować odpowiedzi dół tylko właściwości, które są zainteresowani. Na przykład, jeśli chcesz tylko listę nazw indeksatora, użyj OData `$select` opcji zapytania:

    GET /indexers?api-version=2014-10-20-Preview&$select=name

W takim przypadku odpowiedzi z powyższym przykładzie będzie wyglądać następująco: 

    {
      "value" : [ { "name": "myindexer" } ]
    }

Jest to przydatne technika, aby oszczędzić przepustowość, jeśli masz wiele indeksatorów w swojej usłudze wyszukiwania.

<a name="GetIndexer"></a>

## <a name="get-indexer"></a>Pobierz indeksatora
**Uzyskać indeksatora** operacji definicji indeksatora są pobierane z usługi Azure Search.

    GET https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

`api-version` Jest wymagana. Wersja zapoznawcza jest `2015-02-28-Preview`. 

`api-key` Musi być klucza administratora (w przeciwieństwie do klucza zapytania). Zapoznaj się z rozdziałem uwierzytelniania w [interfejsu API REST usługi wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) Aby dowiedzieć się więcej na temat kluczy. [Utwórz usługę wyszukiwania, w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i klucza właściwości używane w żądaniu.

**Odpowiedź**

Kod stanu: 200 OK jest zwracana dla pomyślnej odpowiedzi.

Odpowiedź jest podobne do w [żądań przykład utwórz indeksator](#CreateIndexerRequestExamples): 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }


<a name="DeleteIndexer"></a>

## <a name="delete-indexer"></a>Usuń indeksatora
**Usunąć indeksator** operacji usuwa indeksatora z usługi Azure Search.

    DELETE https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Po usunięciu indeksatora wykonaniami indeksatora w toku w tym czasie będą działać do zakończenia, ale nie dalsze wykonaniami zostanie zaplanowane. Próbuje korzystać z indeksatora nieistniejącą spowoduje kod stanu HTTP 404 Nie znaleziono. 

`api-version` Jest wymagana. Wersja zapoznawcza jest `2015-02-28-Preview`. 

`api-key` Musi być klucza administratora (w przeciwieństwie do klucza zapytania). Zapoznaj się z rozdziałem uwierzytelniania w [interfejsu API REST usługi wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) Aby dowiedzieć się więcej na temat kluczy. [Utwórz usługę wyszukiwania, w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i klucza właściwości używane w żądaniu.

**Odpowiedź**

Kod stanu: 204 nr zawartości jest zwracana dla pomyślnej odpowiedzi.

<a name="RunIndexer"></a>

## <a name="run-indexer"></a>Uruchomić indeksatora
Poza uruchamianiem okresowo, zgodnie z harmonogramem, indeksatora może być wywoływana na żądanie za pośrednictwem **uruchomić indeksatora** operacji: 

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [admin key]

`api-version` Jest wymagana. Wersja zapoznawcza jest `2015-02-28-Preview`. 

`api-key` Musi być klucza administratora (w przeciwieństwie do klucza zapytania). Zapoznaj się z rozdziałem uwierzytelniania w [interfejsu API REST usługi wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) Aby dowiedzieć się więcej na temat kluczy. [Utwórz usługę wyszukiwania, w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i klucza właściwości używane w żądaniu.

**Odpowiedź**

Kod stanu: 202 zaakceptowane jest zwracana dla pomyślnej odpowiedzi.

<a name="GetIndexerStatus"></a>

## <a name="get-indexer-status"></a>Pobierz stan indeksatora
**Pobierania stanu indeksatora** operacji pobiera bieżący stan i wykonywanie historii indeksatora: 

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [admin key]


`api-version` Jest wymagana. Wersja zapoznawcza jest `2015-02-28-Preview`. 

`api-key` Musi być klucza administratora (w przeciwieństwie do klucza zapytania). Zapoznaj się z rozdziałem uwierzytelniania w [interfejsu API REST usługi wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) Aby dowiedzieć się więcej na temat kluczy. [Utwórz usługę wyszukiwania, w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i klucza właściwości używane w żądaniu.

**Odpowiedź**

Kod stanu: 200 OK dla pomyślnej odpowiedzi.

Treść odpowiedzi zawiera informacje o ogólny stan kondycji indeksatora, ostatnim wywołaniem indeksatora, jak również historię ostatnich wywołań indeksatora (jeśli istnieje). 

Treść odpowiedzi próbki wygląda następująco: 

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

**Stan indeksatora**

Stan indeksatora może być jedną z następujących wartości:

* `running`Wskazuje, że indeksator działa normalnie. Pamiętaj, że nadal mogą niektóre wykonaniami indeksatora nie, dlatego warto sprawdzić `lastResult` również właściwość. 
* `error`Wskazuje, że indeksator wystąpił błąd, który nie można poprawić bez udziału człowieka. Na przykład mogło wygasnąć poświadczenia źródła danych lub schematu źródła danych lub indeksu docelowego została zmieniona w podziału sposób. 

**Wynik wykonywania indeksatora**

Wynik wykonania indeksatora zawiera informacje o wykonanie jednej indeksatora. Najnowsze wyniki są udostępniane jako `lastResult` właściwości stan indeksatora. Inne najnowsze wyniki, jeśli jest obecny, są zwracane jako `executionHistory` właściwości stan indeksatora. 

Wynik wykonywania indeksatora zawiera następujące właściwości: 

* `status`: stanu wykonywania. Zobacz [stan wykonania indeksatora](#IndexerExecutionStatus) poniżej szczegółowe informacje. 
* `errorMessage`: komunikat o błędzie dla wykonanie nie powiodło się. 
* `startTime`: godzina w formacie UTC przy uruchamianiu to wykonanie.
* `endTime`: godzina w formacie UTC po zakończeniu wykonywania tej. Ta wartość nie jest ustawiona, jeśli wykonanie jest nadal w toku.
* `errors`: tablica błędy na poziomie elementu, jeśli istnieje. Każdy wpis zawiera klucz dokumentu (`key` właściwości) oraz komunikat o błędzie (`errorMessage` właściwości). 
* `itemsProcessed`: liczba źródła danych elementów (na przykład wiersze tabeli), które indeksatora podjęto próbę indeksowania podczas wykonywania tego. 
* `itemsFailed`: liczba elementów, których nie powiodła się podczas wykonywania tego.  
* `initialTrackingState`: zawsze `null` pierwszego uruchomienia indeksatora, lub jeśli dane zmiany zasad śledzenia nie jest włączona na źródło danych używane. Po włączeniu tych zasad podczas kolejnych wykonań kodu ta wartość oznacza śledzenia wartość przetwarzane przez to wykonanie pierwszej zmian (najniższy). 
* `finalTrackingState`: zawsze `null` zasad śledzenia zmiany danych nie włączono w źródle danych używane. Wskazuje, w przeciwnym razie śledzenia wartość pomyślnie przetworzone przez wykonanie tego najnowszych zmian (najwyższy). 

<a name="IndexerExecutionStatus"></a>
**Stan wykonania indeksatora**

Stan wykonania indeksatora przechwytuje stan wykonanie jednej indeksatora. Może mieć następujące wartości:

* `success`Wskazuje, że wykonywanie indeksatora została pomyślnie ukończona.
* `inProgress`Wskazuje, że trwa wykonywanie indeksatora. 
* `transientFailure`Wskazuje, że wykonywanie indeksatora nie powiodła się. Zobacz `errorMessage` właściwości, aby uzyskać szczegółowe informacje. Niepowodzenie może lub nie może wymagać interwencji człowieka, aby rozwiązać — wymaga na przykład ustalenia niezgodności między źródłem danych i indeksu docelowego schematów akcja ze strony użytkownika, a przestoju źródła danych tymczasowych nie. Wywołania indeksator będzie według harmonogramu, jeśli występuje. 
* `persistentFailure`Wskazuje, że indeksator nie powiodło się w taki sposób, który wymaga udziału człowieka. Zatrzymaj wykonaniami zaplanowane indeksatora. Po rozwiązaniu problemu, umożliwia ponowne uruchomienie zaplanowanego wykonaniami zresetować indeksatora API. 
* `reset`Wskazuje, że indeksator został zresetowany przez wywołanie do zresetować indeksatora interfejsu API (patrz poniżej). 

<a name="ResetIndexer"></a>

## <a name="reset-indexer"></a>Zresetować indeksatora
**Zresetować indeksatora** operacji resetuje stan skojarzony z indeksatora śledzenia zmian. Dzięki temu można wyzwolić od podstaw ponownego indeksowania (na przykład w przypadku zmiany schematu źródła danych) lub zmienić zasady wykrywania zmian danych dla źródła danych skojarzone z indeksatora.   

    POST https://[service name].search.windows.net/indexers/[indexer name]/reset?api-version=[api-version]
    api-key: [admin key]

`api-version` Jest wymagana. Wersja zapoznawcza jest `2015-02-28-Preview`. 

`api-key` Musi być klucza administratora (w przeciwieństwie do klucza zapytania). Zapoznaj się z rozdziałem uwierzytelniania w [interfejsu API REST usługi wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) Aby dowiedzieć się więcej na temat kluczy. [Utwórz usługę wyszukiwania, w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i klucza właściwości używane w żądaniu.

**Odpowiedź**

Kod stanu: 204 nr zawartości dla pomyślnej odpowiedzi.

## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Mapowanie między typy danych SQL i typy danych w usłudze Azure Search
<table style="font-size:12">
<tr>
<td>Typ danych SQL</td>    
<td>Dozwolone typy pól indeksu docelowego</td>
<td>Uwagi</td>
</tr>
<tr>
<td>bitowe</td>
<td>Typem Edm.Boolean, typem Edm.String</td>
<td></td>
</tr>
<tr>
<td>int, smallint, tinyint</td>
<td>Typem Edm.String z typem Edm.Int32, Edm.Int64,</td>
<td></td>
</tr>
<tr>
<td>bigint</td>
<td>Edm.Int64, typem Edm.String</td>
<td></td>
</tr>
<tr>
<td>rzeczywiste, float</td>
<td>Edm.Double, typem Edm.String</td>
<td></td>
</tr>
<tr>
<td>Smallmoney, pieniędzy<br>Decimal<br>numeryczne
</td>
<td>Edm.String</td>
<td>Usługa Azure Search nie obsługuje konwersji typu decimal do Edm.Double, ponieważ spowoduje to utratę dokładności
</td>
</tr>
<tr>
<td>char, nchar, varchar, nvarchar</td>
<td>Edm.String<br/>Collection(Edm.String)</td>
<td>Zobacz [funkcje mapowania pól](#FieldMappingFunctions) w tym dokumencie, aby uzyskać więcej informacji na temat przekształcania kolumny typu string Collection(Edm.String)</td>
</tr>
<tr>
<td>smalldatetime, datetime, datetime2, date, datetimeoffset</td>
<td>Edm.DateTimeOffset, typem Edm.String</td>
<td></td>
</tr>
<tr>
<td>uniqueidentifer</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>Lokalizacja geograficzna</td>
<td>Edm.GeographyPoint</td>
<td>Obsługiwane są tylko geograficzne wystąpienia typu punktu z 4326 SRID, (która jest wartością domyślną)</td>
</tr>
<tr>
<td>ROWVERSION</td>
<td>Nie dotyczy</td>
<td>Kolumny wersji wiersza nie mogą być przechowywane w indeksie wyszukiwania, ale może służyć do śledzenia zmian</td>
</tr>
<tr>
<td>czas, timespan<br>Binary, varbinary, obraz,<br>Kod XML, geometry, typy CLR</td>
<td>Nie dotyczy</td>
<td>Nieobsługiwane</td>
</tr>
</table>

## <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Mapowanie między typami danych JSON i typy danych w usłudze Azure Search
<table style="font-size:12">
<tr>
<td>Typ danych JSON</td>    
<td>Dozwolone typy pól indeksu docelowego</td>
<td>Uwagi</td>
</tr>
<tr>
<td>wartość logiczna</td>
<td>Typem Edm.Boolean, typem Edm.String</td>
<td></td>
</tr>
<tr>
<td>Liczby całkowite</td>
<td>Typem Edm.String z typem Edm.Int32, Edm.Int64,</td>
<td></td>
</tr>
<tr>
<td>Liczby zmiennoprzecinkowe</td>
<td>Edm.Double, typem Edm.String</td>
<td></td>
</tr>
<tr>
<td>Ciąg</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>Tablice typów pierwotnych typów, np. ["a", "b", "c"]</td>
<td>Collection(Edm.String)</td>
<td></td>
</tr>
<tr>
<td>Ciągi, które wyglądają dat</td>
<td>Edm.DateTimeOffset, typem Edm.String</td>
<td></td>
</tr>
<tr>
<td>Obiekty punktu GeoJSON</td>
<td>Edm.GeographyPoint</td>
<td>Punkty GeoJSON są obiektów JSON w następującym formacie: {"type": "Point", "coordinates": [długie, lat]} </td>
</tr>
<tr>
<td>Inne obiekty JSON</td>
<td>Nie dotyczy</td>
<td>Nieobsługiwane. Usługa Azure Search aktualnie obsługuje tylko typy pierwotne i kolekcji ciągów</td>
</tr>
</table>
