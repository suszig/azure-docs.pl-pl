---
title: "Azure rozwiązania Cosmos bazy danych: Interfejs API Graph w .NET opracowywania | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak tworzyć aplikacje za pomocą interfejsu API usługi DocumentDB DB rozwiązania Cosmos Azure przy użyciu platformy .NET"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: cc8df0be-672b-493e-95a4-26dd52632261
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: denlee
ms.custom: mvc
ms.openlocfilehash: b1419e5aad9446b9d96450cfad79b200cda9a518
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cosmos-db-develop-with-the-graph-api-in-net"></a>Azure rozwiązania Cosmos bazy danych: Interfejs API Graph w .NET opracowywania
Azure DB rozwiązania Cosmos jest usługa globalnie rozproszone wielu modelu bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

W tym samouczku przedstawiono tworzenie konta bazy danych rozwiązania Cosmos Azure przy użyciu portalu Azure oraz do tworzenia bazy danych wykresu i kontenera. Następnie aplikacja tworzy prosty sieci społecznościowych cztery osoby korzystające z [interfejsu API programu Graph](graph-sdk-dotnet.md) (wersja zapoznawcza), a następnie przechodzi przez i zapytanie przy użyciu Gremlin wykresu.

Ten samouczek obejmuje następujące zadania:

> [!div class="checklist"]
> * Tworzenie konta usługi Azure Cosmos DB 
> * Tworzenie bazy danych wykresu i kontenera
> * Serializować wierzchołki i krawędzi obiekty .NET
> * Dodaj wierzchołki i krawędzi.
> * Wykres przy użyciu Gremlin zapytania

## <a name="graphs-in-azure-cosmos-db"></a>Wykresy w Azure rozwiązania Cosmos bazy danych
Bazy danych rozwiązania Cosmos Azure umożliwia tworzenie, aktualizowanie i zapytania przy użyciu wykresów [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) biblioteki. Biblioteka Microsoft.Azure.Graph udostępnia metody rozszerzenia pojedynczego `CreateGremlinQuery<T>` nad `DocumentClient` klasy na wykonanie kwerend Gremlin.

Gremlin jest funkcjonalny język programowania, który obsługuje zapisu operacji zapytań i przechodzenie i operacje (DML). Firma Microsoft obejmuje kilka przykładów w tym artykule można pobrać z wprowadzeniem z Gremlin. Zobacz [zapytania Gremlin](gremlin-support.md) Aby uzyskać szczegółowe wskazówki Gremlin funkcji dostępnych w usłudze Azure DB rozwiązania Cosmos. 

## <a name="prerequisites"></a>Wymagania wstępne
Upewnij się, że masz:

* Aktywne konto platformy Azure. Jeśli go nie masz, możesz zarejestrować się w celu [utworzenia bezpłatnego konta](https://azure.microsoft.com/free/). 
    * Na potrzeby tego samouczka możesz także użyć [emulatora usługi Azure DocumentDB](local-emulator.md).
* Program [Visual Studio](http://www.visualstudio.com/).

## <a name="create-database-account"></a>Tworzenie konta bazy danych

Zacznijmy od utworzenia konta Azure DB rozwiązania Cosmos w portalu Azure.  

> [!TIP]
> * Masz już konto bazy danych rozwiązania Cosmos Azure? Jeśli tak, przejdź do [konfigurowanie rozwiązania Visual Studio](#SetupVS)
> * Czy miał konto usługi Azure DocumentDB? Jeśli tak, Twoje konto jest kontem bazy danych Azure rozwiązania Cosmos i możesz przejść od razu do [konfigurowanie rozwiązania Visual Studio](#SetupVS).  
> * Jeśli używasz emulatora usługi Azure rozwiązania Cosmos bazy danych, wykonaj kroki opisane w temacie [Azure rozwiązania Cosmos DB emulatora](local-emulator.md) skonfigurować emulatora i przejść od razu do [konfigurowanie rozwiązania Visual Studio](#SetupVS). 
>
> 

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a id="SetupVS"></a>Konfigurowanie rozwiązania programu Visual Studio
1. Otwórz program **Visual Studio** na komputerze.
2. W menu **Plik** wybierz polecenie **Nowy**, a następnie wybierz pozycję **Projekt**.
3. W **nowy projekt** okno dialogowe, wybierz opcję **szablony** / **Visual C#** / **aplikacji konsoli (.NET Framework)** , nazwę projektu, a następnie kliknij przycisk **OK**.
4. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy nową aplikację konsolową, która znajduje się w ramach rozwiązania Visual Studio, a następnie kliknij pozycję **Zarządzaj pakietami NuGet...**
5. W **NuGet** , kliknij pozycję **Przeglądaj**i wpisz **Microsoft.Azure.Graphs** w polu wyszukiwania, a także sprawdź **obejmują wersje wstępne**.
6. W wynikach, Znajdź **Microsoft.Azure.Graphs** i kliknij przycisk **zainstalować**.
   
   Jeśli zostanie wyświetlony komunikat dotyczący przejrzenia zmian wprowadzonych w rozwiązaniu, kliknij przycisk **OK**. Jeśli wyświetlany jest komunikat o akceptacji licencji, kliknij pozycję **Akceptuję**.
   
    `Microsoft.Azure.Graphs` Biblioteka udostępnia metody rozszerzenia pojedynczego `CreateGremlinQuery<T>` do wykonywania operacji Gremlin. Gremlin jest funkcjonalny język programowania, który obsługuje zapisu operacji zapytań i przechodzenie i operacje (DML). Firma Microsoft obejmuje kilka przykładów w tym artykule można pobrać z wprowadzeniem z Gremlin. [Zapytania gremlin](gremlin-support.md) ma szczegółowy przewodnik na temat możliwości Gremlin w usłudze Azure DB rozwiązania Cosmos.

## <a id="add-references"></a>Łączenie aplikacji

Dodaj te dwie stałe i *klienta* zmiennej w aplikacji. 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
Następnie z powrotem do head [portalu Azure](https://portal.azure.com) można pobrać adresu URL punktu końcowego, a klucz podstawowy. Adres URL punktu końcowego i klucz podstawowy są niezbędne, aby aplikacja wiedziała, z jakim elementem ma się połączyć, oraz aby usługa Azure Cosmos DB ufała połączeniu aplikacji. 

W portalu Azure, przejdź do swojego konta bazy danych rozwiązania Cosmos platformy Azure, kliknij przycisk **klucze**, a następnie kliknij przycisk **odczytu i zapisu kluczy**. 

Skopiuj identyfikator URI z portalu i wklej go za pośrednictwem `Endpoint` we właściwości punktu końcowego powyżej. Następnie skopiuj podstawowy klucz z portalu i wklej ją do `AuthKey` właściwości powyżej. 

! [Zrzut ekranu przedstawiający portal Azure używany przez samouczek tworzenia aplikacji C#. Pokazuje bazy danych Azure rozwiązania Cosmos konta, przyciskiem KLUCZE wyróżnionym w nawigacji bazy danych Azure rozwiązania Cosmos i wartości URI oraz PRIMARY KEY wyróżnionym w bloku klucze] [klucze] 
 
## <a id="instantiate"></a>Utwórz wystąpienie obiektu DocumentClient 
Następnie utwórz nowe wystąpienie klasy **DocumentClient**.  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <a id="create-database"></a>Tworzenie bazy danych 

Teraz Utwórz bazę danych Azure rozwiązania Cosmos [bazy danych](documentdb-resources.md#databases) za pomocą [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metody lub [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metody  **DocumentClient** klasę z [zestawu SDK .NET usługi DocumentDB](documentdb-sdk-dotnet.md).  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a>Tworzenie wykresu. 

Następnie należy utworzyć kontener wykresu przy użyciu przy użyciu [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metody lub [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metody **DocumentClient** klasy. Kolekcja jest kontenerem jednostek wykresu. 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 1000 }); 
``` 

## <a id="serializing"></a>Serializować wierzchołki i krawędzi obiekty .NET
Używa Azure DB rozwiązania Cosmos [formacie łańcuchowym GraphSON](gremlin-support.md), który definiuje schematu JSON dla wierzchołków, krawędzie i właściwości. Zestaw .NET SDK usługi Azure rozwiązania Cosmos DB zawiera struktury JSON.NET jako zależność i pozwala na serializacji/deserializacji GraphSON na obiekty .NET, które firma Microsoft może współpracować w kodzie.

Na przykład załóżmy współpracować z prostego sieci społecznościowych z czterech osób. Opisano, jak utworzyć `Person` wierzchołków, Dodaj `Knows` relacji między nimi, a następnie zapytań i przechodzenia wykres można znaleźć relacji "friend friend". 

`Microsoft.Azure.Graphs.Elements` Przestrzeń nazw zawiera `Vertex`, `Edge`, `Property` i `VertexProperty` klasy do deserializacji odpowiedzi GraphSON dobrze zdefiniowany obiekty .NET.

## <a name="run-gremlin-using-creategremlinquery"></a>Uruchom przy użyciu CreateGremlinQuery Gremlin
Gremlin, takiego jak SQL, obsługuje odczytu, zapisu i operacje zapytań. Na przykład poniższy fragment kodu przedstawia sposób tworzenia wierzchołków, krawędzi, wykonać niektóre przykładowe zapytania przy użyciu `CreateGremlinQuery<T>`i asynchronicznie iterację te wyniki za pomocą `ExecuteNextAsync` i "HasMoreResults.

```cs
Dictionary<string, string> gremlinQueries = new Dictionary<string, string>
{
    { "Cleanup",        "g.V().drop()" },
    { "AddVertex 1",    "g.addV('person').property('id', 'thomas').property('firstName', 'Thomas').property('age', 44)" },
    { "AddVertex 2",    "g.addV('person').property('id', 'mary').property('firstName', 'Mary').property('lastName', 'Andersen').property('age', 39)" },
    { "AddVertex 3",    "g.addV('person').property('id', 'ben').property('firstName', 'Ben').property('lastName', 'Miller')" },
    { "AddVertex 4",    "g.addV('person').property('id', 'robin').property('firstName', 'Robin').property('lastName', 'Wakefield')" },
    { "AddEdge 1",      "g.V('thomas').addE('knows').to(g.V('mary'))" },
    { "AddEdge 2",      "g.V('thomas').addE('knows').to(g.V('ben'))" },
    { "AddEdge 3",      "g.V('ben').addE('knows').to(g.V('robin'))" },
    { "UpdateVertex",   "g.V('thomas').property('age', 44)" },
    { "CountVertices",  "g.V().count()" },
    { "Filter Range",   "g.V().hasLabel('person').has('age', gt(40))" },
    { "Project",        "g.V().hasLabel('person').values('firstName')" },
    { "Sort",           "g.V().hasLabel('person').order().by('firstName', decr)" },
    { "Traverse",       "g.V('thomas').outE('knows').inV().hasLabel('person')" },
    { "Traverse 2x",    "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')" },
    { "Loop",           "g.V('thomas').repeat(out()).until(has('id', 'robin')).path()" },
    { "DropEdge",       "g.V('thomas').outE('knows').where(inV().has('id', 'mary')).drop()" },
    { "CountEdges",     "g.E().count()" },
    { "DropVertex",     "g.V('thomas').drop()" },
};

foreach (KeyValuePair<string, string> gremlinQuery in gremlinQueries)
{
    Console.WriteLine($"Running {gremlinQuery.Key}: {gremlinQuery.Value}");

    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, gremlinQuery.Value);
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    Console.WriteLine();
}
```

## <a name="add-vertices-and-edges"></a>Dodaj wierzchołki i krawędzi.

Oto instrukcje Gremlin wyświetlany w poprzedniej sekcji więcej szczegółów. Pierwszy możemy niektórych wierzchołków przy użyciu jego Gremlin `addV` metody. Na przykład poniższy fragment kodu tworzy wierzchołek "Blogu Thomasa Andersen" typu "Osoba" z właściwości imię i nazwisko, wiek.

```cs
// Create a vertex
IDocumentQuery<Vertex> createVertexQuery = client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.addV('person').property('firstName', 'Thomas')");

while (createVertexQuery.HasMoreResults)
{
    Vertex thomas = (await create.ExecuteNextAsync<Vertex>()).First();
}
```

Następnie utworzymy niektórych krawędzi między te wierzchołków przy użyciu jego Gremlin `addE` metody. 

```cs
// Add a "knows" edge
IDocumentQuery<Edge> createEdgeQuery = client.CreateGremlinQuery<Edge>(
    graphCollection, 
    "g.V('thomas').addE('knows').to(g.V('mary'))");

while (create.HasMoreResults)
{
    Edge thomasKnowsMaryEdge = (await create.ExecuteNextAsync<Edge>()).First();
}
```

Możemy zaktualizować istniejące wierzchołków przy użyciu `properties` krok w Gremlin. Firma Microsoft pominąć wywołania do wykonania zapytania za pomocą `HasMoreResults` i `ExecuteNextAsync` dla pozostałych przykładach.

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

Można usunąć krawędzi oraz wierzchołków przy użyciu jego Gremlin `drop` kroku. Oto fragment, pokazujący sposób usuwania wierzchołek i krawędzi. Należy pamiętać, że porzucenie wierzchołek wykonuje kaskadowych Usuń skojarzone krawędzi.

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-the-graph"></a>Zapytanie wykresu

Można wykonać zapytania, a także za pomocą Gremlin traversals. Na przykład poniższy fragment kodu przedstawia sposób liczbę wierzchołki na wykresie:

```cs
// Run a query to count vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
Można wykonywać przy użyciu jego Gremlin filtry `has` i `hasLabel` kroki i połączenie ich za pomocą `and`, `or`, i `not` do tworzenia bardziej złożonych filtrów:

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

Niektóre właściwości wyników zapytania za pomocą można projektu `values` krok:

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

Firma Microsoft do tej pory tylko przedstawiono operatorów zapytań, które działają w dowolnej bazy danych. Wykresy są szybkie i wydajne dla operacji przechodzenia, gdy musisz przejść do krawędzi pokrewne i wierzchołków. Spróbujmy wszystkich znajomych blogu Thomasa. Firma Microsoft to zrobić przy użyciu jego Gremlin `outE` krok, aby znaleźć wszystkie wyjściowego krawędzi blogu Thomasa, a następnie przechodzenie do wierzchołków w z tych krawędzi przy użyciu jego Gremlin `inV` krok:

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

Zapytanie dalej wykonuje dwie przeskoków w celu znalezienia wszystkich blogu Thomasa "znajomych przyjaciół", wywołując `outE` i `inV` dwa razy. 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

Można tworzyć bardziej złożone kwerendy i wdrożyć logikę przechodzenie zaawansowanych wykresu przy użyciu Gremlin, tym wyrażeniach filtru mieszanie wykonywania pętli przy użyciu `loop` krok i wykonawcze przy użyciu warunkowego nawigacji `choose` kroku. Dowiedz się więcej o co można zrobić z [Obsługa Gremlin](gremlin-support.md)!

To wszystko, ten samouczek bazy danych Azure rozwiązania Cosmos została zakończona! 

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie zamierzasz w przyszłości korzystać z tej aplikacji, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego samouczka.  

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij nazwę utworzonego zasobu. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę zasobu do usunięcia, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

W tym samouczku wykonaniu następujących czynności:

> [!div class="checklist"]
> * Utworzone konto bazy danych Azure rozwiązania Cosmos 
> * Utworzony wykres bazy danych i kontener
> * Zserializowany wierzchołki i krawędzi obiekty .NET
> * Dodano wierzchołków i krawędzi.
> * Zapytanie wykresu przy użyciu Gremlin

Teraz możesz tworzyć bardziej złożone zapytania i implementować zaawansowaną logikę przechodzenia grafu za pomocą języka Gremlin. 

> [!div class="nextstepaction"]
> [Wykonywanie zapytań przy użyciu języka Gremlin](tutorial-query-graph.md)
