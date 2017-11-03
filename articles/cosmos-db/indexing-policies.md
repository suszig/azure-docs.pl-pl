---
title: "Azure DB rozwiązania Cosmos indeksowania zasad | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak indeksowania działa w usłudze Azure DB rozwiązania Cosmos. Dowiedz się, jak skonfigurować i zmienić zasady indeksowania dla automatycznego indeksowania i zwiększyć wydajność."
keywords: "Indeksowanie działania, automatycznego indeksowania, indeksowania bazy danych"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: d5e8f338-605d-4dff-8a61-7505d5fc46d7
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/17/2017
ms.author: arramac
ms.openlocfilehash: 30a21645831f0cfcb3b52c797dbddfa6b5283960
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-does-azure-cosmos-db-index-data"></a>Jak dane indeksu bazy danych rozwiązania Cosmos Azure?

Domyślnie wszystkie dane z bazy danych Azure rozwiązania Cosmos jest indeksowany. I wielu klientów są wszystkiego umożliwić automatycznie obsługiwać wszystkie aspekty indeksowania bazy danych rozwiązania Cosmos Azure, Azure DB rozwiązania Cosmos obsługuje również określenie niestandardowego **indeksowania zasad** kolekcji podczas tworzenia. Zasady indeksowania w usłudze Azure DB rozwiązania Cosmos są bardziej elastyczne i wydajne niż indeksów pomocniczych oferowanych na innych platformach bazy danych, ponieważ umożliwiają projektowanie i dostosować kształt indeksu bez ograniczania elastyczność schematu. Aby dowiedzieć się, jak indeksowania działa w usłudze Azure DB rozwiązania Cosmos, trzeba zrozumieć, zarządzając zasady indeksowania, można oznaczyć szczegółowych wady i zalety między narzut na przechowywanie indeksu, zapisu i przepływności zapytania i spójności zapytania.  

W tym artykule, firma Microsoft Przyjrzyjmy się zamknięcia bazy danych Azure rozwiązania Cosmos indeksowania zasad, jak można dostosować zasady indeksowania, a skojarzone kompromis. 

Po przeczytaniu tego artykułu, będziesz mieć możliwość odpowiedzieć na następujące pytania:

* Jak można zastąpić właściwości, aby uwzględnić lub wykluczyć indeksowania?
* Jak można skonfigurować indeksu ostatecznego aktualizacji?
* Jak skonfigurować indeksowania do wykonywania zapytań Order By i zakres?
* Jak dokonać zmiany do zbioru zasady indeksowania
* Jak porównać magazynu i wydajności różnych zasad indeksowania

## <a id="CustomizingIndexingPolicy"></a>Dostosowywanie zasady indeksowania w kolekcji
Deweloperzy można dostosować kompromis między magazynu, wydajność zapisu/zapytań i spójności zapytania zastępowanie domyślne zasady indeksowania w kolekcji usługi Azure DB rozwiązania Cosmos i konfigurując następujące aspekty.

* **W tym/wykluczenie dokumentów i ścieżki do/z indeksu**. Deweloperzy mogą włączać niektórych dokumentów, które mają być wyłączone lub zawarte w indeksie, w tym czasie Wstawianie lub zastępowanie je do kolekcji. Deweloperzy można również dołączyć lub wykluczyć pewne właściwości JSON alias ścieżki (takie jak wzorców symboli wieloznacznych) do indeksowania wszystkich dokumentów, które znajdują się w indeksie.
* **Konfigurowanie różnych indeksu typy**. Dla każdego z dołączone ścieżki deweloperzy można również określić typ indeksu wymaganą przez kolekcję na podstawie swoich danych i oczekiwano obciążenia zapytania i numeryczne/ciąg "dokładności" dla każdej ścieżki.
* **Konfigurowanie trybów aktualizacji indeksu**. Azure DB rozwiązania Cosmos obsługuje trzy tryby indeksowania, które mogą być konfigurowane przez zasady indeksowania w kolekcji usługi Azure DB rozwiązania Cosmos: spójne, Lazy i None. 

Poniższy fragment kodu .NET przedstawiono sposób ustawiania niestandardowe zasady indeksowania podczas tworzenia kolekcji. W tym miejscu możemy ustawienie zasad z indeksem typu zakres dla ciągów i numery na precyzja maksymalna. Te zasady ułatwiają wykonywanie zapytań Order By względem ciągów.

    DocumentCollection collection = new DocumentCollection { Id = "myCollection" };

    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), collection);   


> [!NOTE]
> Schematu JSON dla zasady indeksowania został zmieniony w wersji interfejsu API REST wersji 2015-06-03 do obsługi indeksów zakresu względem ciągów. Zestaw .NET SDK 1.2.0 i Java, Python i Node.js SDK 1.1.0 obsługuje nowego schematu zasad. Starsze zestawy SDK za pomocą interfejsu API REST wersji 2015-04-08 i obsługuje starsze schemat zasady indeksowania.
> 
> Domyślnie bazy danych Azure rozwiązania Cosmos indeksuje wszystkie właściwości ciągu w dokumentach zgodnie z indeksu skrótu i numeryczne właściwości z indeksem typu zakres.  
> 
> 

### <a name="customizing-the-indexing-policy-using-the-portal"></a>Dostosowywanie zasady indeksowania, przy użyciu portalu

Można zmienić zasady indeksowania w kolekcji przy użyciu portalu Azure. Otwórz konta bazy danych Azure rozwiązania Cosmos w portalu Azure wybierz kolekcję, w menu nawigacji po lewej stronie kliknij **ustawienia**, a następnie kliknij przycisk **zasady indeksowania**. W **zasady indeksowania** bloku, zmienić zasady indeksowania, a następnie kliknij przycisk **OK** Aby zapisać zmiany. 

### <a id="indexing-modes"></a>Tryby indeksowania bazy danych
Azure DB rozwiązania Cosmos obsługuje trzy indeksowania tryby, które mogą być konfigurowane przez zasady indeksowania w kolekcji z bazy danych Azure rozwiązania Cosmos — spójność opóźnieniem i brak.

**Spójne**: zasady zbierania bazy danych Azure rozwiązania Cosmos jest oznaczony jako "spójne", kwerendy dla danej kolekcji bazy danych Azure rozwiązania Cosmos wykonaj poziomu spójności określone dla odczytów punktu (tj. silne, nieaktualność, sesji lub ostatecznego). Indeks jest aktualizowana synchronicznie, w ramach aktualizacji dokumentu (tj. insert, replace, update i delete dokumentu w kolekcji usługi Azure DB rozwiązania Cosmos).  Spójne indeksowania obsługuje zapytania spójne kosztem możliwość zmniejszenia przepływność zapisu. Redukcja jest funkcja unikatowy ścieżek, które muszą zostać pomyślnie zindeksowane i "poziom zgodności". Spójne tryb indeksowania jest przeznaczona dla obciążeń "write szybkiego zapytań od razu".

**Opóźnieniem**: umożliwia dokumentu maksymalną przepływność wprowadzanie bazy danych Azure rozwiązania Cosmos kolekcji można skonfigurować za pomocą opóźnieniem spójności; znaczenie zapytania są zgodne po pewnym czasie. Indeks jest aktualizowane asynchronicznie kolekcji usługi Azure DB rozwiązania Cosmos jest spoczynku tj. gdy przepływności kolekcji nie jest w pełni wykorzystane do obsługi żądań użytkownika. "Pozyskiwania teraz zapytanie później" w przypadku obciążeń wymagających wprowadzanie swobodnego dokumentu może być odpowiedni "opóźnieniem" Tryb indeksowania.

**Brak**: kolekcja oznaczonej jako tryb indeksu "None" nie ma indeksu skojarzonych z nim. Jest ona powszechnie stosowana w przypadku bazy danych Azure rozwiązania Cosmos jest wykorzystywany jako magazyn kluczy i wartości i dokumenty są dostępne tylko dla ich właściwość Identyfikatora. 

> [!NOTE]
> Konfigurowanie zasady indeksowania z "None" ma efektem ubocznym usunięcie wszelkich istniejący indeks. Użyj, jeśli Twoich wzorców dostępu są wymagane tylko "id" lub "link do samego siebie".
> 
> 

W poniższej tabeli przedstawiono spójności dla zapytań na podstawie tryb indeksowania (spójność i Lazy) skonfigurowane dla kolekcji i poziomu spójności określonego dla żądania zapytania. To ma zastosowanie do zapytania przy użyciu żadnych zestawów SDK interfejs - interfejsu API REST, lub z poziomu procedury składowane i wyzwalaczy. 

|Spójność|Tryb indeksowania: zgodne|Tryb indeksowania: powolne|
|---|---|---|
|Silna|Silna|Ostateczna|
|Powiązana nieaktualność|Powiązana nieaktualność|Ostateczna|
|Sesja|Sesja|Ostateczna|
|Ostateczna|Ostateczna|Ostateczna|

Azure DB rozwiązania Cosmos zwraca błąd dla zapytania Brak Tryb indeksowania w kolekcji. Nadal można wykonać zapytania jako skanowania za pomocą jawnych `x-ms-documentdb-enable-scan` nagłówka w interfejsie API REST lub `EnableScanInQuery` żądania opcji przy użyciu zestawu .NET SDK. Niektóre funkcje zapytania, takie jak ORDER BY nie są obsługiwane jako skanowania z `EnableScanInQuery`.

W poniższej tabeli przedstawiono spójności dla zapytań na podstawie trybu indeksowania (spójność opóźnieniem i None), gdy EnableScanInQuery został określony.

|Spójność|Tryb indeksowania: zgodne|Tryb indeksowania: powolne|Tryb indeksowania: Brak|
|---|---|---|---|
|Silna|Silna|Ostateczna|Silna|
|Powiązana nieaktualność|Powiązana nieaktualność|Ostateczna|Powiązana nieaktualność|
|Sesja|Sesja|Ostateczna|Sesja|
|Ostateczna|Ostateczna|Ostateczna|Ostateczna|

Następujące Pokaż przykładowy kod, jak utworzyć kolekcję usługi Azure DB rozwiązania Cosmos z indeksowania spójna na wszystkich wstawienia dokumentu przy użyciu zestawu .NET SDK.

     // Default collection creates a hash index for all string fields and a range index for all numeric    
     // fields. Hash indexes are compact and offer efficient performance for equality queries.

     var collection = new DocumentCollection { Id ="defaultCollection" };

     collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

     collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("mydb"), collection);


### <a name="index-paths"></a>Indeks ścieżki
Azure DB rozwiązania Cosmos modeli dokumentów JSON i indeksu drzewa i umożliwia dostrajanie zasad dla ścieżki w drzewie. W dokumentach możesz ścieżek, które muszą być uwzględnione lub wykluczone z indeksowania. Można to zapewniają zapisu lepszą wydajność i dolnym indeksu magazynu w scenariuszach, gdy wcześniej znane są wzorcami zapytań.

Indeks ścieżek rozpoczynać się od katalogu głównego (/) i zwykle kończyć? operator symboli wieloznacznych, określające, istnieje wiele możliwych wartości dla prefiksu. Na przykład, aby obsługiwać SELECT * z F.familyName WHERE rodzin F = "Andersen" musi zawierać ścieżkę indeksu /familyName/? w zasadach indeks kolekcji.

Umożliwia także ścieżek w indeksie * symbolu wieloznacznego operatora, aby określić zachowanie dla ścieżki rekursywnie w obszarze prefiks. Na przykład/ładunku / * można wykluczyć wszystkie elementy w obszarze właściwości ładunku z indeksowania.

Poniżej przedstawiono typowe wzorce służący do określania ścieżki indeksu:

| Ścieżka                | Przypadek użycia/opis                                                                                                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /                   | Domyślna ścieżka dla kolekcji. Cykliczne i ma zastosowanie do całego dokumentu drzewa.                                                                                                                                                                                                                                   |
| / prop /?             | Wymagane do obsługi zapytań, podobnie do następującej ścieżki indeksu (skrótu lub zakres typów odpowiednio):<br><br>Wybierz z kolekcji c WHERE c.prop = "value"<br><br>Wybierz z kolekcji c WHERE c.prop > 5<br><br>Wybierz z kolekcji c ORDER BY c.prop                                                                       |
| / prop / *             | Ścieżka indeksu dla wszystkich ścieżek w ramach określonej etykiety. Działa z następujące kwerendy<br><br>Wybierz z kolekcji c WHERE c.prop = "value"<br><br>Wybierz z kolekcji c WHERE c.prop.subprop > 5<br><br>Wybierz z kolekcji c WHERE c.prop.subprop.nextprop = "value"<br><br>Wybierz z kolekcji c ORDER BY c.prop         |
| [] / właściwości / /?         | Ścieżka indeksu wymagane do obsługi iteracji, a następnie dołącz zapytań dotyczących tablic wartości skalarne, takie jak ["a", "b", "c"]:<br><br>Wybierz znacznik z collection.props w tagu WHERE tag = "value"<br><br>Wybierz znacznik z kolekcji c sprzężenia tagu w c.props gdzie tagu > 5                                                                         |
| /props/ /subprop/ []? | Ścieżka indeksu wymaganych do obsłużenia iteracji i sprzężenia zapytania względem tablice obiektów, takich jak [{subprop: ""}, {subprop: "b"}]:<br><br>Wybierz znacznik z collection.props w tagu WHERE tag.subprop = "value"<br><br>Wybierz znacznik z kolekcji c sprzężenia tagu w c.props WHERE tag.subprop = "value"                                  |
| / prop/subprop /?     | Ścieżka indeksu wymaganych do obsłużenia zapytania (skrótu lub zakres typów odpowiednio):<br><br>Wybierz z kolekcji c WHERE c.prop.subprop = "value"<br><br>Wybierz z kolekcji c WHERE c.prop.subprop > 5                                                                                                                    |

> [!NOTE]
> Podczas konfigurowania ścieżek w indeksie niestandardowym, jest wymagane do określenia indeksowania reguły domyślnej dla drzewa całego dokumentu wskazywane przez specjalne ścieżka "/ *". 
> 
> 

Poniższy przykład konfiguruje określonej ścieżki z zakresu indeksowania i wartość niestandardowego dokładności 20 bajtów:

    var collection = new DocumentCollection { Id = "rangeSinglePathCollection" };    

    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = 20 } } 
            });

    // Default for everything else
    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/*" ,
            Indexes = new Collection<Index> {
                new HashIndex(DataType.String) { Precision = 3 }, 
                new RangeIndex(DataType.Number) { Precision = -1 } 
            }
        });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), pathRange);


### <a name="index-data-types-kinds-and-precisions"></a>Typy danych, typy i opisie
Teraz, gdy firma Microsoft zajęło przyjrzeć się jak określać ścieżki, Przyjrzyjmy się opcje, które możemy użyć, aby skonfigurować zasady indeksowania dla ścieżki. Można określić co najmniej jedną definicję indeksowania dla każdej ścieżki:

* Typ danych: **ciąg**, **numer**, **punktu**, **wielokąta**, lub **LineString** (może zawierać tylko jeden wpis dla typu danych na ścieżkę)
* Indeksu rodzaj: **skrótu** (zapytań o równość), **zakres** (równości, zakres lub zapytań Order By) lub **przestrzennym** (przestrzennych zapytań) 
* Dokładność: Dla indeksu skrótu to różni się od 1 do 8 dla ciągi i liczby bez domyślnego jako 3. Indeks zakresu ta wartość może mieć wartość -1 (precyzja maksymalna) i różnią się od 1 do 100 (precyzja maksymalna) na ciąg lub wartości liczbowe.

#### <a name="index-kind"></a>Typ indeksu
Azure DB rozwiązania Cosmos obsługuje rodzaje indeksu skrótu i zakres dla każdej ścieżki (do którego można skonfigurować dla ciągów, liczb lub obu).

* **Skrót** obsługuje równości wydajne i sprzężenia zapytania. W większości przypadków użycia indeksów skrótu nie ma potrzeby większą dokładność niż domyślna wartość 3 bajtów. Typ danych może być ciąg lub liczba.
* **Zakres** obsługuje zapytań o równość wydajne, kwerendy zakresu (przy użyciu >, <>, =, < =,! =) oraz zapytań Order By. Zapytań Order By domyślnie wymagają również precyzja maksymalna wartość indeksu (-1). Typ danych może być ciąg lub liczba.

Azure DB rozwiązania Cosmos obsługuje również rodzaj indeks przestrzenny dla każdej ścieżki, który może być określony dla typów danych punktu wielokąta i LineString. Wartość w określonej ścieżce musi być prawidłowy fragment GeoJSON, takich jak `{"type": "Point", "coordinates": [0.0, 10.0]}`.

* **Przestrzenne** obsługuje wydajne przestrzennych (w ramach i odległość) zapytania. Typ danych może być punkt, wielokąta lub LineString.

> [!NOTE]
> Azure DB rozwiązania Cosmos obsługuje automatycznego indeksowania punktów, wielokątów i LineStrings.
> 
> 

Poniżej przedstawiono typy obsługiwanych indeksu i przykłady zapytań, które mogą być używane do obsługi:

| Typ indeksu | Przypadek użycia/opis                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Wyznaczania wartości skrótu       | Hash — za pośrednictwem/prop /? (lub /) umożliwia wydajne służą następujące zapytania:<br><br>Wybierz z kolekcji c WHERE c.prop = "value"<br><br>Skrót za pośrednictwem/właściwości / [] /? (i / lub/właściwości /) umożliwia wydajne służą następujące zapytania:<br><br>Wybierz znacznik z kolekcji c sprzężenia tagu w c.props WHERE tag = 5                                                                                                                       |
| Zakres      | Zakres za pośrednictwem/prop /? (lub /) umożliwia wydajne służą następujące zapytania:<br><br>Wybierz z kolekcji c WHERE c.prop = "value"<br><br>Wybierz z kolekcji c WHERE c.prop > 5<br><br>Wybierz z kolekcji c ORDER BY c.prop                                                                                                                                                                                                              |
| Przestrzenne     | Zakres za pośrednictwem/prop /? (lub /) umożliwia wydajne służą następujące zapytania:<br><br>Wybierz z c kolekcji<br><br>GDZIE ST_DISTANCE (c.prop, {"type": "Punkt", "coordinates": [0.0, 10.0]}) < 40<br><br>Wybierz z kolekcji c gdzie ST_WITHIN(c.prop, {"type": "Polygon",...}) — za pomocą indeksowania w punktach włączone<br><br>Wybierz z kolekcji c gdzie ST_WITHIN({"type": "Point",...}, c.prop) — za pomocą indeksowania na wielokątów włączone              |

Domyślnie, zwracany jest błąd dla zapytania z zakres operatorów, takich jak > = w przypadku nie zakresu indeks (żadnych precyzja) aby sygnalizuje, że skanowania może być niezbędne do obsługi zapytań. Zakres zapytania mogą być wykonywane bez indeksu zakresu przy użyciu nagłówka x-ms-documentdb Włącz — skanowania w opcję żądania EnableScanInQuery przy użyciu zestawu .NET SDK lub interfejsu API REST. Jeśli istnieją inne filtry w zapytaniu bazy danych Azure rozwiązania Cosmos umożliwia filtrowanie przed indeksu, a następnie zostanie zwrócony błąd.

Te same zasady mają zastosowanie dla zapytania przestrzennych. Domyślnie jeśli istnieje nie indeks przestrzenny, a nie filtrów, które mogą być przekazywane z indeksu, przestrzennych zapytań zostanie zwrócony błąd. Mogą one zostać wykonane jako skanowania przy użyciu x-ms-documentdb-enable skanowania/EnableScanInQuery.

#### <a name="index-precision"></a>Dokładność indeksu
Dokładność indeksu umożliwia zależności między narzut magazynu indeksu i wydajności zapytania. W przypadku numerów firma Microsoft zaleca używanie domyślnej konfiguracji precision-1 ("maksymalną"). Ponieważ liczby 8 bajtów w formacie JSON, co jest równoważne konfiguracji 8 bajtów. Pobrania niższa wartość precyzji, takich jak 1-7, oznacza to, że wartości w niektórych zakresy mapowania tej samej wpis indeksu. W związku z tym można zmniejszyć miejsce do magazynowania indeksu, ale wykonywania zapytań może być konieczne przetwarzać więcej dokumentów i w związku z tym zużywać więcej przepustowości np. jednostek żądania.

Konfiguracja dokładności indeksu zawiera bardziej praktyczne aplikacji z zakresami ciągu. Ponieważ ciągi może być dowolnym dowolnej długości, wybór dokładność indeksu można wpływ na wydajność ciąg kwerendy zakresu i mieć wpływ na ilość wymaganego miejsca do magazynowania indeksu. Ciąg indeksów zakresu można skonfigurować za pomocą 1-100 lub wartość -1 ("maksymalną"). Jeśli chcesz wykonywać zapytań Order By właściwości ciągów, należy określić precision-1 dla odpowiednich ścieżek.

Indeksy przestrzenne zawsze używać dokładność indeksu domyślnego dla wszystkich typów (punkty, LineStrings i wielokątów) i nie może zostać zastąpiona. 

Poniższy przykład pokazuje, jak zwiększyć dokładność dla indeksów zakresu w kolekcji przy użyciu zestawu .NET SDK. 

**Utwórz kolekcję o indeksie niestandardowym dokładności**

    var rangeDefault = new DocumentCollection { Id = "rangeCollection" };

    // Override the default policy for Strings to range indexing and "max" (-1) precision
    rangeDefault.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), rangeDefault);   


> [!NOTE]
> Azure DB rozwiązania Cosmos zwraca błąd, gdy zapytanie używa Order By, ale nie ma indeks zakresu względem ścieżki, którego dotyczy kwerenda za precyzja maksymalna. 
> 
> 

Podobnie ścieżki mogą być całkowicie wykluczone z indeksowania. W kolejnym przykładzie pokazano, jak wykluczyć całą sekcję dokumentów () poddrzewa) z indeksowania przy użyciu "*" symboli wieloznacznych.

    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*" });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);



## <a name="opting-in-and-opting-out-of-indexing"></a>Jeśli wybierzesz i Rezygnacja z indeksowania
Można wybrać, czy ma kolekcję do automatycznego indeksowania wszystkie dokumenty. Domyślnie wszystkie dokumenty są automatycznie indeksowane, ale można ją wyłączyć. Po wyłączeniu indeksowania dokumentów jest możliwy tylko za pomocą ich linki do samego siebie lub przez zapytania przy użyciu identyfikatora.

Z automatycznego indeksowania wyłączone, nadal selektywnie można dodać tylko dokumenty do indeksu. Z drugiej strony można pozostawić automatycznego indeksowania na i selektywnie wykluczyć wybrany tylko określonych dokumentów. Indeksowanie lub wyłącza konfiguracje są przydatne, jeśli masz tylko podzbiór dokumentów, które trzeba można wykonać zapytania.

Na przykład poniższy przykład przedstawia sposób obejmują dokumentu jawnie za pomocą [zestawu SDK .NET interfejsu API usługi DocumentDB](https://docs.microsoft.com/en-us/azure/cosmos-db/documentdb-sdk-dotnet) i [RequestOptions.IndexingDirective](http://msdn.microsoft.com/library/microsoft.azure.documents.client.requestoptions.indexingdirective.aspx) właściwości.

    // If you want to override the default collection behavior to either
    // exclude (or include) a Document from indexing,
    // use the RequestOptions.IndexingDirective property.
    client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new { id = "AndersenFamily", isRegistered = true },
        new RequestOptions { IndexingDirective = IndexingDirective.Include });

## <a name="modifying-the-indexing-policy-of-a-collection"></a>Modyfikowanie zasady indeksowania w kolekcji
Azure DB rozwiązania Cosmos umożliwia zmianę zasady indeksowania w kolekcji na bieżąco. Zmiana indeksowania zasad w kolekcji usługi Azure DB rozwiązania Cosmos może prowadzić do zmiany kształtu tym indeksu, które mogą być indeksowane ścieżki, ich dokładność, a także modelu spójności samego indeksu. W związku z tym zmiana zasady indeksowania, efektywnie wymaga transformacji starego indeksu do nowej. 

**Przekształcenia indeksu w trybie online**

![Indeksowanie działania — przekształcenia indeksu w trybie online bazy danych Azure rozwiązania Cosmos](./media/indexing-policies/index-transformations.png)

Indeks przekształceń są wprowadzane online, co oznacza, że indeksowane dla poszczególnych zasad stare dokumenty są wydajnie przekształcone na nowe zasady **bez wpływu na dostępność zapisu lub zainicjowaną przepływności** kolekcji. Spójność odczytu i zapisu zostało nawiązane przy użyciu interfejsu API REST, zestawy SDK lub z poziomu procedury składowane i wyzwalaczy nie ma wpływu na podczas transformacji indeksu. Oznacza to, że nie ma żadnej spadek wydajności ani przestoju do aplikacji po wprowadzeniu zmian zasad indeksowania.

Jednak w czasie, który przekształcenie indeksu jest postęp zapytania są ostatecznie spójne niezależnie od konfiguracji trybu indeksowania (spójność lub Lazy). To również dotyczy zapytań ze wszystkich interfejsów — interfejs API REST, zestawy SDK i z poziomu procedury składowane i wyzwalaczy. Podobnie jak z opóźnieniem indeksowania, przekształcania indeksu jest wykonywane asynchronicznie w tle repliki przy użyciu zapasowe zasoby dostępne dla danej repliki. 

Indeks przekształceń są również **w miejscu** (w miejscu), tj. rozwiązania Cosmos bazy danych Azure nie Obsługa dwie kopie indeksu i Zamień stary indeks się nowym. Oznacza to, że nie dodatkowe miejsce na dysku jest wymagane lub używane w kolekcji podczas wykonywania przekształcenia indeksu.

Po zmianie zasady indeksowania, jak zmiany zostaną zastosowane do przenoszenia z starego indeksu na nowego jedną zależy przede wszystkim indeksowania konfiguracji trybu więcej więc niż inne wartości, takich jak włączone/wyłączone ścieżek, rodzaje indeksu i opisie. Użycie sieci starej i nowej zasady indeksowania spójne, bazy danych Azure rozwiązania Cosmos wykonuje transformację indeksu w trybie online. Inna zmiana zasady indeksowania, spójne indeksowania w trybie nie można zastosować, gdy transformacja jest w toku.

Jednak można przenieść opóźnieniem lub brak indeksowania tryb podczas przekształcenia jest w toku. 

* Przesunięcie Lazy indeksu zasad zmian skuteczne natychmiast i bazy danych Azure rozwiązania Cosmos rozpoczyna asynchronicznie ponownego tworzenia indeksu. 
* W przypadku przenoszenia None, następnie indeksu zostało porzucone skuteczne natychmiast. Przenoszenie None jest przydatne, gdy chcesz anulować toku transformacji i rozpocząć o różnych zasady indeksowania. 

Jeśli używasz zestawu .NET SDK, należy rozpocząć poza indeksowania zmian zasad przy użyciu nowego **ReplaceDocumentCollectionAsync** — metoda i Śledź postęp procent przekształcania indeks, używając **IndexTransformationProgress** właściwość odpowiedzi z **ReadDocumentCollectionAsync** wywołania. Inne zestawy SDK i interfejsu API REST obsługuje równoważne właściwości i metod do indeksowania zmiany zasad.

Oto fragment kodu, pokazujący sposób zmodyfikować zasady indeksowania kolekcji z spójne trybu indeksowania do opóźnieniem.

**Zmodyfikuj zasady indeksowania z spójne do opóźnieniem**

    // Switch to lazy indexing.
    Console.WriteLine("Changing from Default to Lazy IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.Lazy;

    await client.ReplaceDocumentCollectionAsync(collection);


Można sprawdzić postęp transformację indeksu przez wywołanie metody ReadDocumentCollectionAsync, na przykład, jak pokazano poniżej.

**Śledź postęp przekształcania indeksu**

    long smallWaitTimeMilliseconds = 1000;
    long progress = 0;

    while (progress < 100)
    {
        ResourceResponse<DocumentCollection> collectionReadResponse = await client.ReadDocumentCollectionAsync(
            UriFactory.CreateDocumentCollectionUri("db", "coll"));

        progress = collectionReadResponse.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromMilliseconds(smallWaitTimeMilliseconds));
    }

Przenosząc None indeksowania w trybie, musisz porzucić indeksu dla kolekcji. Może to być przydatne narzędzie operational, jeśli chcesz anulować transformację w toku i natychmiast rozpocząć nową.

**Porzucenie indeksu dla kolekcji**

    // Switch to lazy indexing.
    Console.WriteLine("Dropping index by changing to to the None IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.None;

    await client.ReplaceDocumentCollectionAsync(collection);

Podczas czy wprowadzania zmian zasad indeksowania do kolekcji bazy danych rozwiązania Cosmos Azure? Poniżej przedstawiono typowe przypadki użycia:

* Obsługiwać spójne wyniki podczas normalnego działania, ale spadek do indeksowania opóźnieniem podczas importowania danych zbiorczego
* Start przy użyciu nowych indeksowania na kolekcji bazy danych Azure rozwiązania Cosmos bieżące funkcje np. dane geograficzne zapytań, które wymagają rodzaj indeksu przestrzennego lub Order By, takich jak / ciąg kwerendy zakresu, wymagających ciągu rodzaju indeks zakresu
* Ręcznie wybierz właściwości, które mają być indeksowane i zmień je, wraz z upływem czasu
* Dostosuj indeksowania dokładność poprawić wydajność zapytań, lub Zmniejsz magazynu używane

> [!NOTE]
> Aby zmodyfikować zasady indeksowania, przy użyciu ReplaceDocumentCollectionAsync, potrzebujesz wersji > = 1.3.0 zestawu .NET SDK
> 
> Dla indeksu transformacji pomyślnie musi upewnij się, że jest wystarczająco dużo wolnego miejsca dostępne w kolekcji. Jeśli kolekcja osiągnie przydział magazynowania, transformacja indeks zostanie wstrzymana. Transformacja indeks zostanie automatycznie wznowiona po miejsca do magazynowania jest dostępna, np. usunięcie niektórych dokumentów.
> 
> 

## <a name="performance-tuning"></a>Dostosowywanie wydajności
Interfejsy API usługi DocumentDB zapewniają informacje o metryki wydajności, takie jak magazyn indeksu używane i kosztów przepustowości (jednostki żądania) dla każdej operacji. Te informacje można porównać różnych zasad indeksowania i dostrajania wydajności.

Aby sprawdzić przydział pamięci masowej i użycia kolekcji, uruchamianie żądania HEAD lub GET dla zasobu kolekcji, a sprawdzić x-ms żądania przydziału i nagłówków x-ms żądania użycia. W zestawie SDK .NET [DocumentSizeQuota](http://msdn.microsoft.com/library/dn850325.aspx) i [DocumentSizeUsage](http://msdn.microsoft.com/library/azure/dn850324.aspx) właściwości w [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) zawierają te wartości.

     // Measure the document size usage (which includes the index size) against   
     // different policies.
     ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));  
     Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);


Do mierzenia koszty indeksowania na każdej operacji zapisu (Tworzenie, aktualizowanie lub usuwanie), sprawdź nagłówek x-ms żądania — opłata (lub odpowiednik [RequestCharge](http://msdn.microsoft.com/library/dn799099.aspx) właściwości w [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) zestawu .NET SDK) do mierzenia liczby jednostek żądania używanych przez te operacje.

     // Measure the performance (request units) of writes.     
     ResourceResponse<Document> response = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), myDocument);              
     Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);

     // Measure the performance (request units) of queries.    
     IDocumentQuery<dynamic> queryable =  client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"), queryString).AsDocumentQuery();

     double totalRequestCharge = 0;
     while (queryable.HasMoreResults)
     {
        FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>(); 
        Console.WriteLine("Query batch consumed {0} request units",queryResponse.RequestCharge);
        totalRequestCharge += queryResponse.RequestCharge;
     }

     Console.WriteLine("Query consumed {0} request units in total", totalRequestCharge);

## <a name="changes-to-the-indexing-policy-specification"></a>Zmiany w specyfikacji zasady indeksowania
Zmiany w schemacie dla zasady indeksowania został wprowadzony w dniu 7 lipca 2015 z interfejsu API REST wersji 2015-06-03. Z odpowiednich klas w wersji zestawu SDK ma nowe implementacje odpowiadające schematu. 

Następujące zmiany zostały wprowadzone w specyfikacji JSON:

* Zasady indeksowania obsługuje indeksów zakresu dla ciągów
* Każda ścieżka może mieć wiele definicji indeksu, po jednej dla każdego typu danych
* Indeksowania dokładności obsługuje 1 – 8 cyfr, 1 – 100 dla ciągów i -1 (maksymalna dozwolona dokładność)
* Segmenty ścieżki nie wymagają cudzysłowu ucieczki poszczególnych ścieżek. Można na przykład, Dodaj ścieżkę do tytułu /? zamiast / "title" /?
* Ścieżka katalogu głównego reprezentujący "wszystkie ścieżki" może być reprezentowana jako / * (oprócz /)

Jeśli masz kod tej kolekcji przepisy z niestandardowe zasady indeksowania zapisywane w wersji 1.1.0 zestawu SDK .NET lub starsze, należy zmienić kod aplikacji w celu obsługi tych zmian, aby przejść do zestawu SDK w wersji 1.2.0. Jeśli nie masz kod, który konfiguruje zasady indeksowania lub planowane jest nadal używać starszej wersji zestawu SDK, zmiany nie są wymagane.

Porównanie praktyczne Oto jeden niestandardowe zasady indeksowania przykład napisane przy użyciu interfejsu API REST wersji 2015-06-03, a także poprzedniej wersji 2015-04-08.

**Poprzednie zasady indeksowania JSON**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "IncludedPaths":[
          {
             "IndexType":"Hash",
             "Path":"/",
             "NumericPrecision":7,
             "StringPrecision":3
          }
       ],
       "ExcludedPaths":[
          "/\"nonIndexedContent\"/*"
       ]
    }

**Bieżące zasady indeksowania JSON**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Hash",
                   "dataType":"String",
                   "precision":3
                },
                {
                   "kind":"Hash",
                   "dataType":"Number",
                   "precision":7
                }
             ]
          }
       ],
       "ExcludedPaths":[
          {
             "path":"/nonIndexedContent/*"
          }
       ]
    }

## <a name="next-steps"></a>Następne kroki
Skorzystaj z łączy poniżej dla indeksu zasad zarządzania przykładów i dowiedzieć się więcej o język zapytań usługi Azure rozwiązania Cosmos DB.

1. [Przykłady kodu Zarządzanie indeksami .NET interfejsu API usługi DocumentDB](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/IndexManagement/Program.cs)
2. [Operacje kolekcji usługi DocumentDB interfejsu API REST](https://msdn.microsoft.com/library/azure/dn782195.aspx)
3. [Zapytania SQL](documentdb-sql-query.md)

