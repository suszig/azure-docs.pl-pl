---
title: "Tworzenie aplikacji Node.js usługi Azure Cosmos DB za pomocą interfejsu API programu Graph | Microsoft Docs"
description: "Przykładowy kod Node.js, którego można używać do nawiązywania połączeń z usługą Azure Cosmos DB i wykonywania w niej zapytań"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/14/2017
ms.author: denlee
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: 153b4cc668fdebd28cec5f3d95093a595064202a
ms.contentlocale: pl-pl
ms.lasthandoff: 07/19/2017

---
# <a name="azure-cosmos-db-build-a-nodejs-application-by-using-graph-api"></a>Azure Cosmos DB: Tworzenie aplikacji Node.js za pomocą interfejsu API programu Graph

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

Ten artykuł Szybki start przedstawia sposób tworzenia konta usługi Azure Cosmos DB dla interfejsu API programu Graph (wersja zapoznawcza), bazy danych i grafu przy użyciu witryny Azure Portal. Następnie przy użyciu sterownika open source [Node.js Gremlin](https://www.npmjs.com/package/gremlin-secure) zostanie utworzona i uruchomiona aplikacja konsoli.  

> [!NOTE]
> Moduł npm `gremlin-secure` to zmodyfikowana wersja modułu `gremlin` z obsługą SSL i SASL wymaganych do łączenia się z bazą danych Azure Cosmos DB. Kod źródłowy jest dostępny w usłudze [GitHub](https://github.com/CosmosDB/gremlin-javascript).
>

## <a name="prerequisites"></a>Wymagania wstępne

Przed uruchomieniem tego przykładu muszą być spełnione następujące wymagania wstępne:
* [Node.js](https://nodejs.org/en/) w wersji 0.10.29 lub nowszej
* [Git](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Dodawanie grafu

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a>Klonowanie przykładowej aplikacji

Teraz sklonujemy aplikację interfejsu API programu Graph z repozytorium GitHub, ustawimy parametry połączenia i uruchomimy ją. Zobaczysz, jak łatwo jest pracować programowo z danymi. 

1. Otwórz okno terminala usługi Git, na przykład Git Bash, i za pomocą polecenia `cd` przejdź do katalogu roboczego.  

2. Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-nodejs-getting-started.git
    ```

3. Otwórz plik rozwiązania w programie Visual Studio. 

## <a name="review-the-code"></a>Przeglądanie kodu

Dokonajmy szybkiego przeglądu działań wykonywanych w aplikacji. Otwórz plik `app.js`, w którym znajdziesz następujące wiersze kodu. 

* Tworzenie klienta języka Gremlin.

    ```nodejs
    const client = Gremlin.createClient(
        443, 
        config.endpoint, 
        { 
            "session": false, 
            "ssl": true, 
            "user": `/dbs/${config.database}/colls/${config.collection}`,
            "password": config.primaryKey
        });
    ```

  Wszystkie konfiguracje są w pliku `config.js`, który będziemy edytować w następnej sekcji.

* Seria kroków języka Gremlin jest wykonywana przy użyciu metody `client.execute`.

    ```nodejs
    console.log('Running Count'); 
    client.execute("g.V().count()", { }, (err, results) => {
        if (err) return console.error(err);
        console.log(JSON.stringify(results));
        console.log();
    });
    ```

## <a name="update-your-connection-string"></a>Aktualizowanie parametrów połączenia

1. Otwórz plik config.js. 

2. W pliku Config.js przypisz kluczowi config.endpoint wartość **Identyfikator URI Gremlin** ze strony **Przegląd** w witrynie Azure Portal. 

    `config.endpoint = "GRAPHENDPOINT";`

    ![Wyświetlanie i kopiowanie klucza dostępu w witrynie Azure Portal, blok Klucze](./media/create-graph-nodejs/gremlin-uri.png)

   Jeśli wartość **Identyfikator URI Gremlin** jest pusta, na stronie **Klucze** w portalu możesz wygenerować tę wartość przy użyciu pozycji **Identyfikator URI** wartości, usuwając ciąg https:// i zmieniając dokumenty na grafy.

   Punkt końcowy Gremlin musi być samą nazwą hosta, bez protokołu/numeru portu, czyli powinien mieć postać `mygraphdb.graphs.azure.com` (a nie `https://mygraphdb.graphs.azure.com` lub `mygraphdb.graphs.azure.com:433`).

3. W pliku config.js przypisz elementowi config.primaryKey wartość z pola **Klucz podstawowy** na stronie **Klucze** w witrynie Azure Portal. 

    `config.primaryKey = "PRIMARYKEY";`

   ![Blok Klucze w witrynie Azure Portal](./media/create-graph-nodejs/keys.png)

4. Wprowadź nazwę bazy danych i nazwę grafu (kontenera) dla wartości parametrów config.database i config.collection. 

Oto przykład wypełnionego pliku config.js:

```nodejs
var config = {}

// Note that this must not have HTTPS or the port number
config.endpoint = "testgraphacct.graphs.azure.com";
config.primaryKey = "Pams6e7LEUS7LJ2Qk0fjZf3eGo65JdMWHmyn65i52w8ozPX2oxY3iP0yu05t9v1WymAHNcMwPIqNAEv3XDFsEg==";
config.database = "graphdb"
config.collection = "Persons"

module.exports = config;
```

## <a name="run-the-console-app"></a>Uruchamianie aplikacji konsolowej

1. Otwórz okno terminala i za pomocą polecenia `cd` przejdź do katalogu instalacyjnego pliku package.json uwzględnionego w projekcie.  

2. Uruchom polecenie `npm install`, aby zainstalować wymagane moduły npm, w tym `gremlin-secure`.

3. Uruchom polecenie `node app.js` w terminalu, aby uruchomić aplikację Node.

## <a name="browse-with-data-explorer"></a>Przeglądanie w Eksploratorze danych

Teraz możesz wrócić do Eksploratora danych w witrynie Azure Portal, aby wyświetlać nowe dane grafu, wykonywać o nie zapytania, modyfikować je i pracować z nimi.

W Eksploratorze danych nowa baza danych jest wyświetlana w okienku **Kolekcje**. Rozwiń węzeł **graphdb** i **graphcoll**, a następnie kliknij pozycję **Graf**.

Dane wygenerowane przez przykładową aplikację zostaną wyświetlone w okienku **Grafy**.

## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-your-resources"></a>Czyszczenie zasobów

Jeśli nie planujesz dalszego korzystania z tej aplikacji, usuń wszystkie zasoby utworzone zgodnie z tym artykułem, wykonując następujące czynności: 

1. W menu nawigacji po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij nazwę utworzonego zasobu. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz nazwę zasobu do usunięcia, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

W tym artykule wyjaśniono sposób tworzenia konta usługi Azure Cosmos DB, tworzenia grafu za pomocą Eksploratora danych i uruchamiania aplikacji. Teraz możesz tworzyć bardziej złożone zapytania i implementować zaawansowaną logikę przechodzenia grafu za pomocą języka Gremlin. 

> [!div class="nextstepaction"]
> [Wykonywanie zapytań przy użyciu języka Gremlin](tutorial-query-graph.md)

