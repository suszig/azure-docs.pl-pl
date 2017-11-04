---
title: "Azure rozwiązania Cosmos bazy danych: Tworzenie tabeli interfejsu API programu .NET | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak tworzyć aplikacje za pomocą interfejsu API tabeli DB rozwiązania Cosmos Azure przy użyciu platformy .NET"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 4b22cb49-8ea2-483d-bc95-1172cd009498
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: arramac
ms.custom: mvc
ms.openlocfilehash: 2189dc7900f03a45c360fceffbcd7c1ff36f7e48
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2017
---
# <a name="azure-cosmos-db-develop-with-the-table-api-in-net"></a>Azure rozwiązania Cosmos bazy danych: Tworzenie tabeli interfejsu API programu .NET

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów.

Ten samouczek obejmuje następujące zadania: 

> [!div class="checklist"] 
> * Tworzenie konta usługi Azure Cosmos DB 
> * Włącz funkcjonalność w pliku app.config 
> * Tworzenie tabeli przy użyciu [API tabeli](table-introduction.md) (wersja zapoznawcza)
> * Dodawanie jednostki do tabeli 
> * Zbiorcze wstawianie jednostek 
> * Pobieranie pojedynczej jednostki 
> * Przy użyciu automatycznego indeksów pomocniczych podmiotów zapytania 
> * Zastępowanie jednostki 
> * Usuwanie jednostki 
> * Usuwanie tabeli
 
## <a name="tables-in-azure-cosmos-db"></a>Tabele Azure rozwiązania Cosmos bazy danych 

Udostępnia bazę danych systemu Azure rozwiązania Cosmos [API tabeli](table-introduction.md) (wersja zapoznawcza) dla aplikacji, które muszą magazyn kluczy i wartości, z projektem bez schematu. Zestawy SDK [magazynu tabel Azure](../storage/common/storage-introduction.md) i interfejsy API REST można wykorzystywać do pracy z usługą Azure Cosmos DB. Usługa Azure Cosmos DB umożliwia tworzenie tabel o wysokich wymaganiach dotyczących przepływności. Usługa Azure Cosmos DB obsługuje tabele o zoptymalizowanej przepływności (nazywane nieformalnie „tabelami premium”), dostępne obecnie w publicznej wersji zapoznawczej. 

Z magazynu tabel Azure można także korzystać w przypadku tabel o wysokich wymaganiach dotyczących pamięci i niższych wymaganiach w kwestii przepływności.

Jeśli obecnie używasz magazynu tabel Azure, możesz zyskać następujące korzyści z wersji zapoznawczej "Tabela premium":

- Gotowe [globalne dystrybucji](distribute-data-globally.md) z wielu i [automatycznej i ręcznej pracy awaryjnej.](regional-failover.md)
- Obsługa automatycznego schematu niezwiązane z żadnym indeksowania względem wszystkich właściwości ("indeksów pomocniczych") i szybkie zapytań 
- Obsługa [niezależne skalowanie pamięci masowej i przepływność](partition-data.md), przez dowolną liczbę regionów
- Obsługa [dedykowanych przepływności na tabelę](request-units.md) mogą być skalowane od kilkuset do miliony żądań na sekundę
- Obsługa [pięć dostosowywalne poziomy spójności](consistency-levels.md) celu kompromis dostępności, opóźnienia i spójności dla aplikacji
- dostępności 99,99% w ramach jednego regionu, a możliwość dodawania kolejnych regionach na potrzeby wyższej dostępności i [SLA kompleksowe branży](https://azure.microsoft.com/support/legal/sla/cosmos-db/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) na ogólnej dostępności
- Praca z istniejącego magazynu Azure .NET SDK i bez wprowadzania zmian kodu w aplikacji

W wersji zapoznawczej bazy danych rozwiązania Cosmos Azure obsługuje interfejs API tabeli przy użyciu zestawu .NET SDK. Możesz pobrać [zestawu SDK w wersji zapoznawczej usługi Magazyn Azure](https://aka.ms/premiumtablenuget) z pakietu NuGet, która ma tej samej klasy i podpisy metod jako [zestawu SDK usługi Magazyn Azure](https://www.nuget.org/packages/WindowsAzure.Storage), ale również mogą łączyć się z konta bazy danych rozwiązania Cosmos Azure przy użyciu interfejsu API tabeli.

Aby dowiedzieć się więcej o skomplikowanych zadaniach magazynu tabel Azure, zobacz:

* [Wprowadzenie do platformy Azure rozwiązania Cosmos DB: Tabela interfejsu API](table-introduction.md)
* Dokumentację referencyjną usługi tabel, aby uzyskać szczegółowe informacje o dostępnych interfejsach API [biblioteki klienta usługi Storage dla platformy .NET odwołania](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)

### <a name="about-this-tutorial"></a>Informacje o tym samouczku
W tym samouczku jest dla deweloperów, którzy są zaznajomieni z magazynem tabel Azure SDK i chcesz korzystać z funkcji premium dostępne przy użyciu bazy danych Azure rozwiązania Cosmos. Jest on oparty na [Rozpoczynanie pracy z magazynem tabel Azure przy użyciu platformy .NET](table-storage-how-to-use-dotnet.md) i pokazuje, jak korzystać z dodatkowych funkcji, takich jak indeksów pomocniczych, udostępnionej przepływności i wielu. Firma Microsoft opisano, jak utworzyć konto bazy danych Azure rozwiązania Cosmos, a następnie utworzyć i wdrożyć aplikację tabeli za pomocą portalu Azure. Przeprowadzenie możemy również przykłady .NET do tworzenia i usuwania tabeli, wstawianie, aktualizowanie, usuwanie i przeszukiwaniem danych w tabeli. 

Jeśli nie masz jeszcze programu Visual Studio 2017 r zainstalowany, możesz pobrać i użyć **wolnego** [programu Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Podczas instalacji programu Visual Studio upewnij się, że jest włączona opcja **Programowanie na platformie Azure**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

Zacznijmy od utworzenia konta Azure DB rozwiązania Cosmos w portalu Azure.  

> [!TIP]
> * Masz już konto bazy danych rozwiązania Cosmos Azure? Jeśli tak, przejdź do [konfigurowanie rozwiązania Visual Studio](#SetupVS).
> * Czy miał konto usługi Azure DocumentDB? Jeśli tak, Twoje konto jest kontem bazy danych Azure rozwiązania Cosmos i możesz przejść od razu do [konfigurowanie rozwiązania Visual Studio](#SetupVS).  
> * Jeśli używasz emulatora usługi Azure rozwiązania Cosmos bazy danych, wykonaj kroki opisane w temacie [Azure rozwiązania Cosmos DB emulatora](local-emulator.md) skonfigurować emulatora i przejść od razu do [konfigurowanie rozwiązania Visual Studio](#SetupVS).
<!---Loc Comment: Please, check link [Set up your Visual Studio solution] since it's not redirecting to any location.---> 
>
>

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)] 

## <a name="clone-the-sample-application"></a>Klonowanie przykładowej aplikacji

Teraz sklonujemy aplikację Tabela z repozytorium GitHub, ustawimy parametry połączenia i uruchomimy ją.

1. Otwórz okno terminalu usługi Git, na przykład git bash, i za pomocą polecenia `cd` przejdź do katalogu roboczego.  

2. Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started
    ```

3. Następnie otwórz plik rozwiązania w programie Visual Studio.

## <a name="update-your-connection-string"></a>Aktualizowanie parametrów połączenia

Teraz wróć do witryny Azure Portal, aby uzyskać informacje o parametrach połączenia i skopiować je do aplikacji.

1. W witrynie [Azure Portal](http://portal.azure.com/), korzystając ze swojego konta usługi Azure Cosmos DB, kliknij na lewym panelu nawigacyjnym pozycję **Klucze**, a następnie pozycję **Klucze odczytu i zapisu**. Użyjesz przyciski Kopia po prawej stronie ekranu, aby skopiować parametry połączenia do pliku app.config w następnym kroku.

2. W programie Visual Studio otwórz plik app.config. 

3. Skopiuj wartość identyfikatora URI z portalu (przy użyciu przycisku Kopiuj) i była wartość klucza konta w pliku app.config. Użyj nazwy konta utworzonego wcześniej nazwy konta w pliku app.config.
  
```
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;TableEndpoint=https://account-name.documents.azure.com" />
```

> [!NOTE]
> Aby użyć tej aplikacji z magazynem tabel Azure standardowe, musisz zmienić parametry połączenia w `app.config file`. Użyj nazwy konta jako nazwa tabeli konta i klucz jako klucz podstawowy magazyn Azure. <br>
>`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;EndpointSuffix=core.windows.net" />`
> 
>

## <a name="build-and-deploy-the-app"></a>Tworzenie i wdrażanie aplikacji
1. W programie Visual Studio kliknij projekt prawym przyciskiem myszy w **Eksploratorze rozwiązań**, a następnie kliknij polecenie **Zarządzaj pakietami NuGet**. 

2. W polu **Przeglądaj** pakietu NuGet wpisz ***WindowsAzure.Storage-PremiumTable***. Sprawdź **obejmują wersje wstępne**.

3. Z wyników, należy zainstalować **WindowsAzure.Storage PremiumTable** i wybierz polecenie kompilacji w wersji zapoznawczej `0.0.1-preview`. Ta akcja instaluje pakiet magazynu tabel Azure oraz wszystkie zależności.

4. Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. 

Teraz można wrócić do Eksploratora danych i zobacz zapytania, modyfikowania i pracować z danymi w tej tabeli. 

> [!NOTE]
> Aby użyć tej aplikacji w emulatorze DB rozwiązania Cosmos Azure, wystarczy zmienić parametry połączenia w `app.config file`. Użyj poniżej wartości dla emulatora. <br>
>`<add key="StorageConnectionString" value=DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=<insertkey>==;TableEndpoint=https://localhost -->`
> 
>

## <a name="azure-cosmos-db-capabilities"></a>Możliwości platformy Azure DB rozwiązania Cosmos
Azure DB rozwiązania Cosmos obsługuje wiele funkcji, które nie są dostępne w magazynie tabel Azure, interfejsu API. Nowe funkcje można włączyć za pomocą następujących `appSettings` wartości konfiguracji. Firma Microsoft nie wprowadzają, ani nowe sygnatury przeciążenia do zestawu SDK usługi Magazyn Azure w wersji zapoznawczej. Dzięki temu można nawiązać połączenia z tabel zarówno warstwy standardowa i premium i pracy z innymi usługami Azure Storage, takich jak obiekty BLOB i kolejki. 


| Klucz | Opis |
| --- | --- |
| TableConnectionMode  | Azure DB rozwiązania Cosmos obsługuje dwa tryby łączności. W `Gateway` trybie żądania zawsze są kierowane do bramy bazy danych Azure rozwiązania Cosmos przekazuje je do odpowiedniej partycji danych. W `Direct` tryb łączności, klient pobiera Mapowanie tabel na partycje i żądań bezpośrednio przed partycji danych. Firma Microsoft zaleca `Direct`, wartość domyślna.  |
| TableConnectionProtocol | Azure DB rozwiązania Cosmos obsługuje dwa protokoły połączenia - `Https` i `Tcp`. `Tcp`Wartość domyślna to i zalecane, ponieważ jest bardziej lightweight. |
| TablePreferredLocations | Rozdzielaną przecinkami listę preferowanych (podłączonej do wielu sieci) lokalizacji odczytów. Każde konto bazy danych rozwiązania Cosmos Azure może być skojarzony z 1-30 + regionów. Każde wystąpienie klienta można określić podzbiór tych regionów, w preferowanej kolejności dla odczytów małe opóźnienia. Regionów muszą nosić przy użyciu ich [wyświetlane nazwy](https://msdn.microsoft.com/library/azure/gg441293.aspx), na przykład `West US`. Zobacz też [wielu interfejsów API](tutorial-global-distribution-table.md).
| TableConsistencyLevel | Można kompromis między dostępności opóźnienia, spójność i wybierając pięć dobrze zdefiniowane poziomy spójności: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, i `Eventual`. Domyślnie jest `Session`. Wybór poziomu spójności sprawia, że różnica znaczących wydajności w konfiguracji w przypadku. Zobacz [poziomy spójności](consistency-levels.md) szczegółowe informacje. |
| TableThroughput | Zarezerwowaną przepływnością dla tabeli w jednostkach żądań (RU) na sekundę. Tabele pojedynczego może obsługiwać miliony 100s RU/s. Zobacz [jednostek żądania](request-units.md). Domyślnie`400` |
| TableIndexingPolicy | Spójne i automatyczne dodatkowej indeksowania wszystkie kolumny w tabeli | Zgodne ze specyfikacją zasady indeksowania ciągu JSON. Zobacz [zasady indeksowania](indexing-policies.md) aby zobaczyć, jak można zmienić zasady indeksowania do dołączania/wykluczania określonych kolumn. | Automatyczne indeksowanie wszystkich właściwości (skrót ciągi), a zakres numerów |
| TableQueryMaxItemCount | Skonfiguruj maksymalną liczbę elementów zwróconych dla tabeli kwerendy w jednym cyklu. Domyślnie jest `-1`, który umożliwia Azure DB rozwiązania Cosmos dynamiczne określanie wartości w czasie wykonywania. |
| TableQueryEnableScan | Jeśli zapytanie nie można użyć indeksu dla dowolny filtr, uruchom ją mimo to za pomocą skanowania. Domyślnie jest `false`.|
| TableQueryMaxDegreeOfParallelism | Stopień równoległości do wykonania zapytania różnych partycji. `0`jest szeregowe z nie pobierania, `1` jest seryjny z wcześniej pobrano i wyższe wartości zwiększyć stopień równoległości. Domyślnie jest `-1`, który umożliwia Azure DB rozwiązania Cosmos dynamiczne określanie wartości w czasie wykonywania. |

Aby zmienić domyślną wartość, otwórz `app.config` plików w Eksploratorze rozwiązań w programie Visual Studio. Dodaj zawartość elementu `<appSettings>` widocznego poniżej. Zastąp `account-name` z nazwą konta magazynu i `account-key` kluczem dostępu do konta. 

```xml
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
    <appSettings>
      <!-- Client options -->
      <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key; TableEndpoint=https://account-name.documents.azure.com" />
      <add key="TableConnectionMode" value="Direct"/>
      <add key="TableConnectionProtocol" value="Tcp"/>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>
      <add key="TableConsistencyLevel" value="Eventual"/>

      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
      <add key="TableIndexingPolicy" value="{""indexingMode"": ""Consistent""}"/>

      <!-- Table query options -->
      <add key="TableQueryMaxItemCount" value="-1"/>
      <add key="TableQueryEnableScan" value="false"/>
      <add key="TableQueryMaxDegreeOfParallelism" value="-1"/>
      <add key="TableQueryContinuationTokenLimitInKb" value="16"/>
            
    </appSettings>
</configuration>
```

Dokonajmy szybkiego przeglądu działań wykonywanych w aplikacji. Otwórz `Program.cs` plików i można znaleźć, te wiersze kodu utworzyć zasoby tabeli. 

## <a name="create-the-table-client"></a>Tworzenie klienta tabeli
Możesz zainicjować `CloudTableClient` do łączenia się kontem tabeli.

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
Ten klient został zainicjowany przy użyciu `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, i `TablePreferredLocations` wartości konfiguracji, jeśli określony w ustawieniach aplikacji.
    
## <a name="create-a-table"></a>Tworzenie tabeli
Następnie można utworzyć tabeli za pomocą `CloudTable`. Tabele w bazie danych rozwiązania Cosmos Azure mogą być skalowane niezależnie pod względem pamięci masowej i przepływność i Partycjonowanie jest automatycznie obsługiwane przez usługę. Azure DB rozwiązania Cosmos obsługuje zarówno o stałym rozmiarze i nieograniczoną liczbę tabel. Zobacz [partycjonowania w usłudze Azure DB rozwiązania Cosmos](partition-data.md) szczegółowe informacje. 

```csharp
CloudTable table = tableClient.GetTableReference("people");

table.CreateIfNotExists();
```

Brak istotną różnicą w sposób tworzenia tabel. Azure DB rozwiązania Cosmos rezerwuje przepływności, w przeciwieństwie do magazynu Azure na podstawie zużycia modelu dla transakcji. Model zastrzeżenie ma dwie kluczowe korzyści:

* Przepustowość sieci jest w wersji dedykowanej/zarezerwowana, więc można nigdy nie pobrać ograniczany Jeśli częstotliwość żądania jest poziomie lub poniżej z udostępnionej przepływności
* Model rezerwacji jest więcej [ekonomiczne w przypadku obciążeń intensywnie przepływności](key-value-store-cost.md)

Można skonfigurować domyślne przepływności przez skonfigurowanie ustawienia dla `TableThroughput` pod względem RU (jednostki żądania) na sekundę. 

Odczyt jednostki 1 KB jest znormalizowany jako 1 RU i inne operacje są znormalizowane do wartości stałej RU oparte na ich użycie procesora CPU, pamięci i IOPS. Dowiedz się więcej o [jednostek w usłudze Azure DB rozwiązania Cosmos żądania](request-units.md).

> [!NOTE]
> Podczas gdy magazyn tabel zestawu SDK nie obsługuje obecnie modyfikowanie przepływności, można zmienić przepływność natychmiast w dowolnym momencie przy użyciu portalu Azure lub interfejsu wiersza polecenia Azure.

Następnie możemy przeprowadzenie proste odczytu i zapisu (CRUD) przy użyciu magazynu tabel Azure SDK. W tym samouczku przedstawiono przewidywalną niski milisekund jednocyfrowej opóźnienia i szybkie zapytania pochodzącymi z bazy danych Azure rozwiązania Cosmos.

## <a name="add-an-entity-to-a-table"></a>Dodawanie jednostki do tabeli
Jednostki w magazynie tabel Azure rozszerza z `TableEntity` klasy i musi mieć `PartitionKey` i `RowKey` właściwości. Oto przykład definicji jednostki klienta.

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

Poniższy fragment kodu przedstawia sposób Wstaw jednostkę z usługą Azure storage SDK. Azure DB rozwiązania Cosmos jest przeznaczona dla gwarantowane małe opóźnienia w dowolnej skali, na całym świecie.

Ukończenie operacji zapisu < 15 ms p99 i ms ~ 6 w p50 aplikacji uruchomionych na tym samym regionie co konto bazy danych Azure rozwiązania Cosmos. I ten czas trwania kont fakt, że zapisy są potwierdzone do klienta dopiero po ich są synchronicznie replikowane, trwale zatwierdzone, i całą jego zawartość jest indeksowana.

Interfejs API tabeli dla bazy danych Azure rozwiązania Cosmos jest w wersji zapoznawczej. Ogólnie gwarancje opóźnienia p99 bazują na umów SLA, takich jak innych interfejsów API Azure rozwiązania Cosmos bazy danych. 

```csharp
// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute the insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a>Zbiorcze wstawianie jednostek
Azure obsługuje magazynu tabeli operacji zbiorczej interfejsu API, pozwala na łączenie aktualizacji, usuwanie i wstawianie w tej samej operacji pojedyncza partia. Azure DB rozwiązania Cosmos nie ma niektóre ograniczenia na interfejsu API partii jako magazynu tabel Azure. Na przykład można wykonywać wielu odczytów w partii, można wykonywać wiele zapisuje dane w tej samej jednostki w partii, a nie ma żadnego limitu na 100 operacje na partię. 

```csharp
// Create the batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it to the table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it to the table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities to the batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute the batch operation.
table.ExecuteBatch(batchOperation);
```
## <a name="retrieve-a-single-entity"></a>Pobieranie pojedynczej jednostki
Pobiera (pobiera) w usłudze Azure DB rozwiązania Cosmos pełną < 10 ms p99 i ~ 1 ms na p50 w tym samym regionie Azure. Dodaj tyle regionów do swojego konta dla odczytów małe opóźnienia i wdrażania aplikacji można odczytać z ich lokalny region ("wieloadresowej"), ustawiając `TablePreferredLocations`. 

Można pobrać pojedynczą jednostką przy użyciu następującego fragmentu kodu:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> Dowiedz się więcej o wielu interfejsów API w [programowania z użyciem wielu regionów](tutorial-global-distribution-table.md)
>

## <a name="query-entities-using-automatic-secondary-indexes"></a>Przy użyciu automatycznego indeksów pomocniczych podmiotów zapytania
Tabele mogą być przeszukiwane przy użyciu `TableQuery` klasy. Azure DB rozwiązania Cosmos ma aparat zoptymalizowanych pod kątem zapisu bazy danych, który automatycznie indeksuje wszystkie kolumny w tabeli. Indeksowanie w usłudze Azure DB rozwiązania Cosmos jest niezależny od schematu. W związku z tym nawet jeśli schemat różni się od wierszy lub schematu rozwoju wraz z upływem czasu, jest automatycznie indeksowane. Ponieważ bazy danych rozwiązania Cosmos Azure obsługuje automatyczne indeksów pomocniczych, zapytań dotyczących dowolnej właściwości można użyć indeksu i obsłużona wydajnie.

```csharp
CloudTable table = tableClient.GetTableReference("people");

// Filter against a property that's not partition key or row key
TableQuery<CustomerEntity> emailQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.GenerateFilterCondition("Email", QueryComparisons.Equal, "Ben@contoso.com"));

foreach (CustomerEntity entity in table.ExecuteQuery(emailQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

W wersji zapoznawczej bazy danych rozwiązania Cosmos Azure obsługuje zapytania funkcji magazynu tabel Azure API tabeli. Azure DB rozwiązania Cosmos obsługuje również sortowanie, agregacje, dane geograficzne zapytania hierarchii i szeroki zakres funkcji wbudowanych. Dodatkowe funkcje będzie dostępna w interfejsie API tabeli w przyszłej aktualizacji. Zobacz [kwerendy bazy danych Azure rozwiązania Cosmos](documentdb-sql-query.md) omówienie tych funkcji. 

## <a name="replace-an-entity"></a>Zastępowanie jednostki
Aby zaktualizować jednostkę, pobierz ją z usługi tabel, zmodyfikuj obiekt jednostki, a następnie zapisz zmiany w usłudze tabel. Poniższy kod zmienia istniejący numer telefonu klienta. 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
Podobnie można wykonywać `InsertOrMerge` lub `Merge` operacji.  

## <a name="delete-an-entity"></a>Usuwanie jednostki
Można z łatwością usunąć jednostkę po jej pobraniu, korzystając z tego samego wzorca co w przypadku aktualizowania jednostki. Poniższy kod umożliwia pobranie i usunięcie jednostki klienta.

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a>Usuwanie tabeli
Poniższy przykład kodu usuwa tabelę z konta magazynu. Możesz usunąć i ponownie utwórz tabelę bezpośrednio z bazy danych Azure rozwiązania Cosmos.

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów 

Jeśli nie zamierzasz w przyszłości korzystać z tej aplikacji, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego samouczka.   

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij nazwę utworzonego zasobu.  
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę zasobu do usunięcia, a następnie kliknij pozycję **Usuń**. 

## <a name="next-steps"></a>Następne kroki

W tym samouczku możemy omówione jak rozpocząć korzystanie z bazy danych rozwiązania Cosmos Azure przy użyciu interfejsu API tabeli, a jego wykonaniu następujących czynności: 

> [!div class="checklist"] 
> * Utworzone konto bazy danych Azure rozwiązania Cosmos 
> * Funkcje włączone w pliku app.config 
> * Utworzyć tabelę 
> * Dodać jednostkę do tabeli 
> * Wstawić partię jednostek 
> * Pobrać pojedynczą jednostką 
> * Jednostki, którego dotyczy kwerenda przy użyciu automatycznego indeksów pomocniczych 
> * Zastąpione jednostki 
> * Usunięte jednostki 
> * Usunięto tabelę  

Możesz teraz przejść do następnego samouczek i Dowiedz się więcej o przeszukiwaniem danych w tabeli. 

> [!div class="nextstepaction"]
> [Zapytanie do tabeli interfejsu API](tutorial-query-table.md)
