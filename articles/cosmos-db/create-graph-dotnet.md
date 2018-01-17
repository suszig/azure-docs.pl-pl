---
title: "Tworzenie aplikacji .NET Framework lub Core usługi Azure Cosmos DB za pomocą interfejsu API programu Graph | Microsoft Docs"
description: "Przykładowy kod programu .NET Framework/Core, którego można używać do nawiązywania połączeń z usługą Azure Cosmos DB i wykonywania w niej zapytań"
services: cosmos-db
documentationcenter: 
author: luisbosquez
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 01/08/2018
ms.author: lbosq
ms.openlocfilehash: c7fff37e1b59fd90952826a1410a8dd8c6931e77
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/09/2018
---
# <a name="azure-cosmos-db-build-a-net-framework-or-core-application-using-the-graph-api"></a>Azure Cosmos DB: tworzenie aplikacji .NET Framework lub Core za pomocą interfejsu API programu Graph

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, możesz szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość oraz grafów. 

Ten przewodnik Szybki start przedstawia sposób tworzenia konta usługi Azure Cosmos DB, bazy danych i grafu (kontenera) przy użyciu witryny Azure Portal. Następnie przy użyciu [interfejsu API programu Graph](graph-sdk-dotnet.md) zostanie utworzona i uruchomiona aplikacja konsolowa.  

## <a name="prerequisites"></a>Wymagania wstępne

Jeśli nie masz jeszcze zainstalowanego programu Visual Studio 2017, możesz pobrać program [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) i używać go **bezpłatnie**. Podczas instalacji programu Visual Studio upewnij się, że jest włączona opcja **Programowanie na platformie Azure**.

Jeśli masz już zainstalowany program Visual Studio 2017, upewnij się, że zainstalowano wersję [Visual Studio 2017 z aktualizacją Update 3](https://www.visualstudio.com/en-us/news/releasenotes/vs2017-relnotes).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Dodawanie grafu

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a>Klonowanie przykładowej aplikacji

Teraz sklonujemy aplikację interfejsu API programu Graph z repozytorium GitHub, ustawimy parametry połączenia i uruchomimy ją. Zobaczysz, jak łatwo jest pracować programowo z danymi. 

W tym przykładowym projekcie użyto formatu projektu .NET Core i jest on przeznaczony do pracy z następującymi strukturami:
 - netcoreapp2.0
 - net461

1. Otwórz okno terminalu usługi Git, na przykład git bash, i za pomocą polecenia `cd` przejdź do katalogu roboczego.  

2. Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started.git
    ```

3. Następnie otwórz program Visual Studio i otwórz plik rozwiązania. 

## <a name="review-the-code"></a>Przeglądanie kodu

Dokonajmy szybkiego przeglądu działań wykonywanych w aplikacji. Otwórz plik Program.cs i zobacz, że następujące wiersze kodu tworzą zasoby usługi Azure Cosmos DB. 

* Inicjowanie klienta DocumentClient. 

    ```csharp
    using (DocumentClient client = new DocumentClient(
        new Uri(endpoint),
        authKey,
        new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    ```

* Tworzenie nowej bazy danych.

    ```csharp
    Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" });
    ```

* Tworzenie nowego grafu.

    ```csharp
    DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync(
        UriFactory.CreateDatabaseUri("graphdb"),
        new DocumentCollection { Id = "graph" },
        new RequestOptions { OfferThroughput = 1000 });
    ```
* Wykonywanie serii kroków języka Gremlin przy użyciu metody `CreateGremlinQuery`.

    ```csharp
    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, "g.V().count()");
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    ```

## <a name="update-your-connection-string"></a>Aktualizowanie parametrów połączenia

Teraz wróć do witryny Azure Portal, aby uzyskać informacje o parametrach połączenia i skopiować je do aplikacji.

1. W witrynie [Azure Portal](http://portal.azure.com/) kliknij pozycję **Klucze**. 

    Skopiuj pierwszą część wartości identyfikatora URI.

    ![Wyświetlanie i kopiowanie klucza dostępu w witrynie Azure Portal, strona Klucze](./media/create-graph-dotnet/keys.png)

2. W programie Visual Studio 2017 otwórz plik appsettings.json i wklej wartość w miejsce elementu `FILLME` w lokalizacji `endpoint`. 

    `"endpoint": "https://FILLME.documents.azure.com:443/",`

    Wartość punktu końcowego powinna wyglądać następująco:

    `"endpoint": "https://testgraphacct.documents.azure.com:443/",`

3. Skopiuj wartość **KLUCZ PODSTAWOWY** z portalu i przypisz ją do klucza AuthKey w pliku App.config, a następnie zapisz zmiany. 

    `"authkey": "FILLME"`

4. Zapisz plik appsettings.json. 

Aplikacja została zaktualizowana i zawiera teraz wszystkie informacje potrzebne do nawiązania komunikacji z usługą Azure Cosmos DB. 

## <a name="run-the-console-app"></a>Uruchamianie aplikacji konsolowej

Przed uruchomieniem aplikacji zalecane jest uaktualnienie pakietu *Microsoft.Azure.Graphs* do najnowszej wersji.

1. W programie Visual Studio kliknij prawym przyciskiem myszy projekt **GraphGetStarted** w **Eksploratorze rozwiązań**, a następnie kliknij polecenie **Zarządzaj pakietami NuGet**. 

2. Na karcie **Aktualizacje** menedżera pakietów NuGet wpisz ciąg *Microsoft.Azure.Graphs* i zaznacz pole **Obejmuje wersje wstępne**. 

3. Na liście wyników zaktualizuj bibliotekę **Microsoft.Azure.Graphs** do najnowszej wersji pakietu. Spowoduje to zainstalowanie pakietu biblioteki rozszerzenia grafu usługi Azure Cosmos DB oraz wszystkich jego zależności.

    Jeśli zostanie wyświetlony komunikat dotyczący przejrzenia zmian wprowadzonych w rozwiązaniu, kliknij przycisk **OK**. Jeśli wyświetlany jest komunikat o akceptacji licencji, kliknij pozycję **Akceptuję**.

4. Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.

   W oknie konsoli będą widoczne wierzchołki i krawędzie dodawane do grafu. Po zakończeniu działania skryptu naciśnij klawisz ENTER dwa razy, aby zamknąć okno konsoli.

## <a name="browse-using-the-data-explorer"></a>Przeglądanie za pomocą Eksploratora danych

Teraz możesz wrócić do Eksploratora danych w witrynie Azure Portal, aby przeglądać nowe dane i wykonywać zapytania względem nich.

1. W Eksploratorze danych nowa baza danych jest wyświetlana w okienku Grafy. Rozwiń węzeł **graphdb** i **graphcollz**, a następnie kliknij pozycję **Graf**.

2. Kliknij przycisk **Zastosuj filtr**, aby użyć domyślnego zapytania do wyświetlenia wszystkich wierzchołków grafu. Dane wygenerowane przez przykładową aplikację zostaną wyświetlone w okienku Grafy.

    Możesz powiększać i zmniejszać graf, rozszerzać obszar wyświetlania grafu, dodawać kolejne wierzchołki oraz przenosić wierzchołki na wyświetlanej powierzchni.

    ![Wyświetlanie grafu w Eksploratorze danych w witrynie Azure Portal](./media/create-graph-dotnet/graph-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie zamierzasz w przyszłości korzystać z tej aplikacji, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego przewodnika Szybki start: 

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij nazwę utworzonego zasobu. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę zasobu do usunięcia, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wyjaśniono sposób tworzenia konta usługi Azure Cosmos DB, tworzenia grafu za pomocą Eksploratora danych i uruchamiania aplikacji. Teraz możesz tworzyć bardziej złożone zapytania i implementować zaawansowaną logikę przechodzenia grafu za pomocą języka Gremlin. 

> [!div class="nextstepaction"]
> [Wykonywanie zapytań przy użyciu języka Gremlin](tutorial-query-graph.md)

