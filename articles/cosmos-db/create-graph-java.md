---
title: "Tworzenie aplikacji Java usługi Azure Cosmos DB za pomocą interfejsu API programu Graph | Microsoft Docs"
description: "Przykładowy kod Java, którego można używać do nawiązywania połączeń z danymi grafu i wykonywania zapytań względem nich w usłudze Azure Cosmos DB za pomocą języka Gremlin."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/27/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: d9619bd9a012a347634282788b3a318886967a3f
ms.contentlocale: pl-pl
ms.lasthandoff: 06/28/2017


---
<a id="azure-cosmos-db-build-a-java-application-using-the-graph-api" class="xliff"></a>

# Azure Cosmos DB: Tworzenie aplikacji Java za pomocą interfejsu API programu Graph

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

Ten przewodnik Szybki start przedstawia sposób tworzenia konta usługi Azure Cosmos DB dla interfejsu API programu Graph (wersja zapoznawcza), bazy danych i grafu przy użyciu witryny Azure Portal. Następnie przy użyciu sterownika OSS [Java Gremlin](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver) zostanie utworzona i uruchomiona aplikacja konsolowa.  

<a id="prerequisites" class="xliff"></a>

## Wymagania wstępne

* Przed uruchomieniem tego przykładu muszą być spełnione następujące wymagania wstępne:
   * Zestaw JDK 1.7 lub nowszy (uruchom polecenie `apt-get install default-jdk`, jeśli zestaw JDK nie jest zainstalowany) i ustawienie zmiennych środowiskowych takich jak `JAVA_HOME`
   * Narzędzie Maven (uruchom polecenie `apt-get install maven`, jeśli narzędzie Maven nie jest zainstalowane)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-a-database-account" class="xliff"></a>

## Tworzenie konta bazy danych

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

<a id="add-a-graph" class="xliff"></a>

## Dodawanie grafu

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

<a id="clone-the-sample-application" class="xliff"></a>

## Klonowanie przykładowej aplikacji

Teraz sklonujemy aplikację z interfejsem API programu Graph (wersja zapoznawcza) z repozytorium GitHub, ustawimy parametry połączenia i uruchomimy ją. Zobaczysz, jak łatwo jest pracować programowo z danymi. 

1. Otwórz okno terminalu usługi Git, na przykład git bash, i za pomocą polecenia `cd` przejdź do katalogu roboczego.  

2. Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started.git
    ```

<a id="review-the-code" class="xliff"></a>

## Przeglądanie kodu

Dokonajmy szybkiego przeglądu działań wykonywanych w aplikacji. Otwórz plik `Program.java`, gdzie znajdziesz następujące wiersze kodu. 

* Element `Client` języka Gremlin jest inicjowany z poziomu konfiguracji w pliku `src/remote.yaml`.

    ```java
    Cluster cluster = Cluster.build(new File("src/remote.yaml")).create();
    
    Client client = cluster.connect();
    ```

* Wykonywanie serii kroków języka Gremlin przy użyciu metody `client.submit`.

    ```java
    ResultSet results = client.submit("g.V()");

    CompletableFuture<List<Result>> completableFutureResults = results.all();
    List<Result> resultList = completableFutureResults.get();

    for (Result result : resultList) {
        System.out.println(result.toString());
    }
    ```
<a id="update-your-connection-string" class="xliff"></a>

## Aktualizowanie parametrów połączenia

Teraz wróć do witryny Azure Portal, aby uzyskać informacje o parametrach połączenia i skopiować je do aplikacji.

1. W witrynie [Azure Portal](http://portal.azure.com/), korzystając ze swojego konta usługi Azure Cosmos DB, kliknij na lewym panelu nawigacyjnym pozycję **Klucze**, a następnie pozycję **Klucze odczytu i zapisu**. W następnym kroku, korzystając z przycisków kopiowania dostępnych po prawej stronie ekranu, skopiujesz identyfikator URI i klucz podstawowy do pliku `Program.java`.

    ![Wyświetlanie i kopiowanie klucza dostępu w witrynie Azure Portal, blok Klucze](./media/create-graph-java/keys.png)

2. Otwórz plik `src/remote.yaml`. 

3. Wypełnij ustawienia konfiguracji *host*, *port*, *username*, *password*, *connectionPool* i *serializer* w pliku `src/remote.yaml`:

    Ustawienie|Sugerowana wartość|Opis
    ---|---|---
    Hosts|***.graphs.azure.com|Identyfikator URI usługi Graph, który można pobrać z witryny Azure Portal
    Port|443|Ustawiony numer 443
    Nazwa użytkownika|*Twoja nazwa użytkownika*|Zasób w postaci `/dbs/<db>/colls/<coll>`.
    Hasło|*Twój podstawowy klucz główny*|Twój podstawowy klucz główny dla usługi Azure Cosmos DB
    ConnectionPool|{enableSsl: true}|Ustawienie puli połączeń protokołu SSL
    Serializer|{ className:org.apache.tinkerpop.gremlin.<br>driver.ser.GraphSONMessageSerializerV1d0,<br> config: { serializeResultToString: true }}|Ustaw na tę wartość

<a id="run-the-console-app" class="xliff"></a>

## Uruchamianie aplikacji konsolowej

1. Uruchom polecenie `mvn package` w terminalu, aby zainstalować wymagane pakiety języka Java.

2. Uruchom polecenie `mvn exec:java -D exec.mainClass=GetStarted.Program` w terminalu, aby uruchomić aplikację Java.

Teraz można wrócić do Eksploratora danych i zobaczyć, jak się pracuje z nowymi danymi, modyfikuje je i tworzy zapytania o nie. 

<a id="browse-using-the-data-explorer" class="xliff"></a>

## Przeglądanie za pomocą Eksploratora danych

Teraz możesz wrócić do Eksploratora danych w witrynie Azure Portal, aby przeglądać nowe dane i wykonywać zapytania względem nich.

* W Eksploratorze danych nowa baza danych jest wyświetlana w okienku Kolekcje. Rozwiń węzeł **graphdb** i **graphcoll**, a następnie kliknij pozycję **Graf**.

    Dane wygenerowane przez przykładową aplikację zostaną wyświetlone w okienku Grafy.

<a id="review-slas-in-the-azure-portal" class="xliff"></a>

## Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

<a id="clean-up-resources" class="xliff"></a>

## Oczyszczanie zasobów

Jeśli nie zamierzasz w przyszłości korzystać z tej aplikacji, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego przewodnika Szybki start: 

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij nazwę utworzonego zasobu. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę zasobu do usunięcia, a następnie kliknij pozycję **Usuń**.

<a id="next-steps" class="xliff"></a>

## Następne kroki

W tym przewodniku Szybki start wyjaśniono sposób tworzenia konta usługi Azure Cosmos DB, tworzenia grafu za pomocą Eksploratora danych i uruchamiania aplikacji. Teraz możesz tworzyć bardziej złożone zapytania i implementować zaawansowaną logikę przechodzenia grafu za pomocą języka Gremlin. 

> [!div class="nextstepaction"]
> [Wykonywanie zapytań przy użyciu języka Gremlin](tutorial-query-graph.md)


