---
title: "Azure rozwiązania Cosmos bazy danych: Tworzenie przy użyciu interfejsu API SQL w programie .NET | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak tworzyć aplikacje za pomocą interfejsu API SQL Azure rozwiązania Cosmos DB przy użyciu platformy .NET"
services: cosmos-db
documentationcenter: 
author: rafats
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: rafats
ms.custom: mvc
ms.openlocfilehash: e37a0993567b6cec7ed6a91e6dad1f2e2c097198
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-cosmosdb-develop-with-the-sql-api-in-net"></a>Azure CosmosDB: Tworzenie przy użyciu interfejsu API SQL w .NET

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

Ten samouczek pokazuje, jak utworzyć konto bazy danych rozwiązania Cosmos Azure przy użyciu portalu Azure, a następnie utwórz bazą danych dokumentów i kolekcji z [klucza partycji](sql-api-partition-data.md#partition-keys) przy użyciu [interfejs API .NET SQL](sql-api-introduction.md). Definiując klucza partycji podczas tworzenia kolekcji, aplikacja jest przygotowana do łatwego skalowania wraz z rozwojem danych. 

Ten samouczek obejmuje następujące zadania za pomocą [interfejs API .NET SQL](sql-api-sdk-dotnet.md):

> [!div class="checklist"]
> * Tworzenie konta usługi Azure Cosmos DB
> * Tworzenie bazy danych i kolekcji z użyciem klucza partycji
> * Tworzenie dokumentów JSON
> * Aktualizowanie dokumentu
> * Zapytanie względem kolekcji podzielone na partycje
> * Uruchamianie procedury składowane
> * Usuwanie dokumentu
> * Usuwanie bazy danych

## <a name="prerequisites"></a>Wymagania wstępne
Upewnij się, że masz:

* Aktywne konto platformy Azure. Jeśli go nie masz, możesz zarejestrować się w celu [utworzenia bezpłatnego konta](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Jeśli nie masz jeszcze zainstalowanego programu Visual Studio 2017, możesz pobrać program [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) i używać go **bezpłatnie**. Podczas instalacji programu Visual Studio upewnij się, że jest włączona opcja **Programowanie na platformie Azure**.

## <a name="create-an-azure-cosmos-db-account"></a>Tworzenie konta usługi Azure Cosmos DB

Zacznijmy od utworzenia konta Azure DB rozwiązania Cosmos w portalu Azure.

> [!TIP]
> * Masz już konto bazy danych rozwiązania Cosmos Azure? Jeśli tak, przejdź do [konfigurowanie rozwiązania Visual Studio](#SetupVS)
> * Jeśli używasz emulatora usługi Azure rozwiązania Cosmos bazy danych, wykonaj kroki opisane w temacie [Azure rozwiązania Cosmos DB emulatora](local-emulator.md) skonfigurować emulatora i przejść od razu do [konfigurowanie rozwiązania Visual Studio](#SetupVS). 
>
>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Konfigurowanie rozwiązania programu Visual Studio
1. Otwórz program **Visual Studio** na komputerze.
2. W menu **Plik** wybierz polecenie **Nowy**, a następnie wybierz pozycję **Projekt**.
3. W **nowy projekt** okno dialogowe, wybierz opcję **szablony** / **Visual C#** / **aplikacji konsoli (.NET Framework)** , nazwę projektu, a następnie kliknij przycisk **OK**.
   ![Zrzut ekranu przedstawiający okno Nowy projekt](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-new-project-2.png)

4. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy nową aplikację konsolową, która znajduje się w ramach rozwiązania Visual Studio, a następnie kliknij pozycję **Zarządzaj pakietami NuGet...**
    
    ![Zrzut ekranu przedstawiający menu projektu kliknięte prawym przyciskiem myszy](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-manage-nuget-pacakges.png)
5. W **NuGet** , kliknij pozycję **Przeglądaj**i wpisz **documentdb** w polu wyszukiwania.
<!---stopped here--->
6. W wynikach znajdź pozycję **Microsoft.Azure.DocumentDB** i kliknij przycisk **Zainstaluj**.
   Identyfikator pakietu dla biblioteki klienta usługi Azure rozwiązania Cosmos bazy danych jest [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).
   ![Zrzut ekranu przedstawiający NuGet Menu do znajdowania zestawu SDK klienta usługi Azure rozwiązania Cosmos bazy danych](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)

    Jeśli zostanie wyświetlony komunikat dotyczący przejrzenia zmian wprowadzonych w rozwiązaniu, kliknij przycisk **OK**. Jeśli wyświetlany jest komunikat o akceptacji licencji, kliknij pozycję **Akceptuję**.

## <a id="Connect"></a>Dodaj odwołania do projektu
Pozostałe kroki w tym samouczku Podaj wstawki kodu interfejsu API SQL, które są wymagane do tworzenia i aktualizacji bazy danych Azure rozwiązania Cosmos zasobów w projekcie.

Najpierw dodaj te odwołania do aplikacji.
<!---These aren't added by default when you install the pkg?--->

```csharp
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

## <a id="add-references"></a>Łączenie aplikacji

Następnie dodaj te dwie stałe i *klienta* zmiennej w aplikacji.

```csharp
private const string EndpointUrl = "<your endpoint URL>";
private const string PrimaryKey = "<your primary key>";
private DocumentClient client;
```

Następnie, head z powrotem do [portalu Azure](https://portal.azure.com) można pobrać adresu URL punktu końcowego, a klucz podstawowy. Adres URL punktu końcowego i klucz podstawowy są niezbędne, aby aplikacja wiedziała, z jakim elementem ma się połączyć, oraz aby usługa Azure Cosmos DB ufała połączeniu aplikacji.

W portalu Azure, przejdź do swojego konta bazy danych rozwiązania Cosmos platformy Azure, kliknij przycisk **klucze**, a następnie kliknij przycisk **odczytu i zapisu kluczy**.

Skopiuj identyfikator URI z portalu i wklej go za pośrednictwem `<your endpoint URL>` w pliku program.cs. Następnie skopiuj podstawowy klucz z portalu i wklej go za pośrednictwem `<your primary key>`. Należy usunąć `<` i `>` z własnymi wartościami.

![Zrzut ekranu przedstawiający portal Azure używany przez samouczek NoSQL do tworzenia aplikacji konsolowej C#. Przedstawia konto bazy danych Azure rozwiązania Cosmos z KLUCZE wyróżnionym w bloku konta usługi Azure DB rozwiązania Cosmos i wartości URI oraz PRIMARY KEY wyróżnionym w bloku klucze](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-keys.png)

## <a id="instantiate"></a>Utwórz wystąpienie obiektu DocumentClient

Teraz, Utwórz nowe wystąpienie klasy **DocumentClient**.

```csharp
DocumentClient client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
```

## <a id="create-database"></a>Tworzenie bazy danych

Następnie należy utworzyć bazy danych Azure rozwiązania Cosmos [bazy danych](sql-api-resources.md#databases) za pomocą [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metody lub [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metody  **DocumentClient** klasę z [zestawu .NET SDK SQL](sql-api-sdk-dotnet.md). Baza danych jest kontenerem logicznym magazynu dokumentów JSON podzielonym na partycje w kolekcjach.

```csharp
await client.CreateDatabaseAsync(new Database { Id = "db" });
```
## <a name="decide-on-a-partition-key"></a>Podejmowanie decyzji o klucza partycji 

Kolekcje są kontenerami do przechowywania dokumentów. Zasoby logiczne i można je [obejmować co najmniej jednej partycji fizycznej](partition-data.md). A [klucza partycji](sql-api-partition-data.md) jest właściwość (lub ścieżki) w dokumencie, które jest używane do rozpowszechniania danych między serwerami lub partycji. Wszystkie dokumenty z tym samym kluczem partycji są przechowywane w tej samej partycji. 

Określanie klucza partycji jest ważne decyzja podejmowana przed utworzeniem kolekcji. Klucze partycji są właściwości (lub ścieżki) w dokumentach używane przez bazy danych Azure rozwiązania Cosmos w dystrybucji danych między wieloma serwerami lub partycji. Rozwiązania cosmos DB skróty wartość klucza partycji i używa skrótu wynik w celu określenia partycji, w której ma zostać zapisany dokument. Wszystkie dokumenty z tym samym kluczem partycji są przechowywane w tej samej partycji, a klucze partycji nie można zmienić po utworzeniu kolekcji. 

W tym samouczku, chcemy się wartość klucza partycji `/deviceId` tak, aby wszystkie dane na jednym urządzeniu są przechowywane w jednej partycji. Chcesz wybrać klucza partycji, który ma wiele wartości, z których każdy są używane na temat taką samą częstotliwością do upewnij się, że rozwiązania Cosmos DB może równoważyć obciążenie danych rozwoju lub osiągnięcia pełnej przepływności kolekcji. 

Aby uzyskać więcej informacji na temat partycjonowania, zobacz [partycji i skali w usłudze Azure DB rozwiązania Cosmos?](partition-data.md) 

## <a id="CreateColl"></a>Tworzenie kolekcji 

Teraz, wiemy naszych klucza partycji `/deviceId`, umożliwia tworzenie [kolekcji](sql-api-resources.md#collections) za pomocą [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metody lub [CreateDocumentCollectionIfNotExistsAsync ](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metody **DocumentClient** klasy. Kolekcja jest kontenerem dokumentów JSON i wszelkie skojarzonej logiki aplikacji JavaScript. 

> [!WARNING]
> Tworzenie kolekcji ma cenę, jak są rezerwacji przepustowości dla aplikacji do komunikowania się z bazy danych Azure rozwiązania Cosmos. Aby uzyskać więcej informacji, odwiedź stronę naszych [stronie dotyczącej cen](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

```csharp
// Collection for device telemetry. Here the JSON property deviceId is used  
// as the partition key to spread across partitions. Configured for 2500 RU/s  
// throughput and an indexing policy that supports sorting against any  
// number or string property. .
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 2500 });
```

Ta metoda powoduje, że wywołanie bazy danych Azure rozwiązania Cosmos i przepisy usługi liczba partycji na podstawie przepływności żądanego interfejsu API REST. Można zmienić przepływność kolekcji, ponieważ wydajność potrzebuje rozwija przy użyciu zestawu SDK lub [portalu Azure](set-throughput.md).

## <a id="CreateDoc"></a>Tworzenie dokumentów JSON
Teraz można wstawić niektórych dokumentów JSON do bazy danych Azure rozwiązania Cosmos. [Dokument](sql-api-resources.md#documents) można utworzyć za pomocą metody [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) klasy **DocumentClient**. Dokumenty są zawartością JSON zdefiniowaną przez użytkownika (dowolną). Ta klasa przykładowe zawiera urządzenia odczytywania i wywołanie CreateDocumentAsync Wstaw nowe urządzenie odczytu do kolekcji.

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here the partition key is extracted 
// as "XMS-0001" based on the collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```
## <a name="read-data"></a>Odczyt danych

Załóżmy odczytu dokumentu przez klucz partycji i identyfikator przy użyciu metody ReadDocumentAsync. Należy pamiętać, że odczyty obejmują wartość PartitionKey (odpowiadający `x-ms-documentdb-partitionkey` nagłówek żądania w interfejsie API REST).

```csharp
// Read document. Needs the partition key and the Id to be specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;
```

## <a name="update-data"></a>Aktualizowanie danych

Teraz załóżmy zaktualizować niektóre dane przy użyciu metody ReplaceDocumentAsync.

```csharp
// Update the document. Partition key is not required, again extracted from the document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);
```

## <a name="delete-data"></a>Usuwanie danych

Umożliwia teraz usunąć przy użyciu metody DeleteDocumentAsync dokumentu przez klucz partycji i identyfikator.

```csharp
// Delete a document. The partition key is required.
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```
## <a name="query-partitioned-collections"></a>Zapytanie względem kolekcji podzielone na partycje

Kwerendy danych w kolekcjach podzielonym na partycje, bazy danych Azure rozwiązania Cosmos automatycznie rozsyła kwerendy do partycji odpowiadający wartości klucza partycji podana w filtrze (jeśli istnieją). Na przykład to zapytanie jest kierowany do właśnie partycji zawierającej klucz partycji "XMS 0001".

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
Następujące zapytanie nie ma filtr klucza partycji (DeviceId) i jest rozwiniętym do wszystkich partycji, w którym jest przeprowadzana z partycji indeksu. Należy pamiętać, że trzeba określić EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` w interfejsie API REST) ma zestaw SDK, aby wykonać zapytania na partycje.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

## <a name="parallel-query-execution"></a>Równoległego wykonywania zapytań
Azure rozwiązania Cosmos bazy danych SQL SDK 1.9.0 i powyżej opcje wykonywania zapytania równoległe pomocy technicznej, które umożliwiają wykonywanie zapytań o małych opóźnieniach dotyczących kolekcji partycjonowanych nawet wtedy, gdy konieczne jest touch dużą liczbę partycji. Na przykład następujące zapytanie jest skonfigurowany do uruchomienia równoległego na partycje.

```csharp
// Cross-partition Order By queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
Możesz zarządzać równoległego wykonywania zapytań przez dostrajanie następujące parametry:

* Przez ustawienie `MaxDegreeOfParallelism`, można kontrolować stopień równoległości tj., maksymalną liczbę równoczesnych połączeń sieciowych do partycji w kolekcji. Jeśli ustawisz to-1, stopień równoległości jest zarządzany przez zestaw SDK. Jeśli `MaxDegreeOfParallelism` nie jest określony lub ustawiona na 0, co jest to wartość domyślna, będzie jednego połączenia sieciowego z partycjami kolekcji.
* Przez ustawienie `MaxBufferedItemCount`, można kompromis wykorzystanie pamięci zapytania opóźnienia i po stronie klienta. Jeśli ten parametr lub ustaw tę wartość-1, liczba elementów buforowane podczas równoległego wykonywania zapytań jest zarządzany przez zestaw SDK.

Podana takim samym stanie kolekcji, zapytań równoległych zwróci wyniki w tej samej kolejności jak wykonanie szeregowego. Podczas wykonywania kwerendy między partycji zawierającej, sortowanie (ORDER BY i/lub góry), zestawu SDK SQL wystawia zapytania równolegle na partycje i scala częściowo sortowane wyniki w po stronie klienta, aby wygenerować globalnie uporządkowanych wyników.

## <a name="execute-stored-procedures"></a>Wykonanie procedury składowane
Ponadto można wykonywać transakcje atomic względem dokumentów mających taki sam identyfikator urządzenia, np. Jeśli wartości zagregowanych lub najnowszy stan urządzeń w jednym dokumencie jest obsługa przez dodanie poniższego kodu do projektu.

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```

I to już wszystko! to są głównymi składnikami aplikacji bazy danych rozwiązania Cosmos platformy Azure, która używa klucza partycji wydajne skalowanie dystrybucji danych na partycji.  

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie zamierzasz nadal korzystać z tej aplikacji, należy usunąć wszystkie zasoby utworzone przez tego samouczka w portalu Azure następujące czynności:

1. Z menu po lewej stronie w portalu Azure kliknij **grup zasobów** a następnie kliknij przycisk unikatową nazwę utworzonego zasobu. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę zasobu do usunięcia, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

W tym samouczku wykonaniu następujących czynności: 

> [!div class="checklist"]
> * Utworzone konto bazy danych Azure rozwiązania Cosmos
> * Utworzony z użyciem klucza partycji bazy danych i kolekcji
> * Utworzony dokumentów JSON
> * Zaktualizowano dokumentu
> * Zapytanie kolekcji partycjonowanych
> * Uruchomiono procedurę składowaną
> * Usunąć dokumentu
> * Usunięte z bazy danych

Można teraz przejść do następnego samouczek i zaimportuj dane dodatkowe do swojego konta DB rozwiązania Cosmos. 

> [!div class="nextstepaction"]
> [Importowanie danych do usługi Azure Cosmos DB](import-data.md)
