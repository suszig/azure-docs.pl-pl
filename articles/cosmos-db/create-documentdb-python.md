---
title: "Azure Cosmos DB: Budowanie aplikacji za pomocą języka Python i interfejsu API usługi DocumentDB | Microsoft Docs"
description: "Przykładowy kod w języku Python, którego można użyć do nawiązywania połączenia z interfejsem API usługi DocumentDB usługi Azure Cosmos DB i do wykonywania w niej zapytań"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 51c11be2-af6d-425f-a86a-39cbfe61da29
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 05/13/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: 08d467ea27484e7d1d07d6c21b2e04b6525fbcd8
ms.contentlocale: pl-pl
ms.lasthandoff: 06/01/2017


---
<a id="azure-cosmos-db-build-a-documentdb-api-app-with-python-and-the-azure-portal" class="xliff"></a>

# Azure Cosmos DB: Budowanie aplikacji interfejsu API usługi DocumentDB za pomocą języka Python i witryny Azure Portal

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

Ten przewodnik Szybki start przedstawia sposób tworzenia konta usługi Azure Cosmos DB, bazy danych dokumentów i kolekcji przy użyciu witryny Azure Portal. Następnie za pomocą [interfejsu API usługi DocumentDB języka Python](documentdb-sdk-python.md) zbudujesz i uruchomisz aplikację konsoli.

<a id="prerequisites" class="xliff"></a>

## Wymagania wstępne

* Przed uruchomieniem tego przykładu muszą być spełnione następujące wymagania wstępne:
    * [Program Visual Studio w wersji 2015](http://www.visualstudio.com/) lub nowszej.
    * Narzędzia Python Tools for Visual Studio pobrane z witryny [GitHub](http://microsoft.github.io/PTVS/). W tym samouczku są używane narzędzia Python Tools for VS 2015.
    * Środowisko Python w wersji 2.7 pobrane z witryny [python.org](https://www.python.org/downloads/release/python-2712/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-a-database-account" class="xliff"></a>

## Tworzenie konta bazy danych

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="add-a-collection" class="xliff"></a>

## Dodawanie kolekcji

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="clone-the-sample-application" class="xliff"></a>

## Klonowanie przykładowej aplikacji

Teraz sklonujemy aplikację interfejsu API usługi DocumentDB z repozytorium github, ustawimy parametry połączenia i uruchomimy ją. Zobaczysz, jak łatwo jest pracować programowo z danymi. 

1. Otwórz okno terminalu usługi Git, na przykład git bash, i za pomocą polecenia `cd` przejdź do katalogu roboczego.  

2. Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-python-getting-started.git
    ```  
<a id="review-the-code" class="xliff"></a>

## Przeglądanie kodu

Dokonajmy szybkiego przeglądu działań wykonywanych w aplikacji. Otwórz plik DocumentDBGetStarted.py i zobacz, że następujące wiersze kodu tworzą zasoby usługi Azure Cosmos DB. 


* Inicjowanie klienta DocumentClient.

    ```python
    # Initialize the Python DocumentDB client
    client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})
    ```

* Tworzenie nowej bazy danych.

    ```python
    # Create a database
    db = client.CreateDatabase({ 'id': config['DOCUMENTDB_DATABASE'] })
    ```

* Tworzenie nowej kolekcji.

    ```python
    # Create collection options
    options = {
        'offerEnableRUPerMinuteThroughput': True,
        'offerVersion': "V2",
        'offerThroughput': 400
    }

    # Create a collection
    collection = client.CreateCollection(db['_self'], { 'id': config['DOCUMENTDB_COLLECTION'] }, options)
    ```

* Tworzenie kilku dokumentów.

    ```python
    # Create some documents
    document1 = client.CreateDocument(collection['_self'],
        { 
            'id': 'server1',
            'Web Site': 0,
            'Cloud Service': 0,
            'Virtual Machine': 0,
            'name': 'some' 
        })
    ```

* Wykonywanie zapytania przy użyciu programu SQL.

    ```python
    # Query them in SQL
    query = { 'query': 'SELECT * FROM server s' }    
            
    options = {} 
    options['enableCrossPartitionQuery'] = True
    options['maxItemCount'] = 2

    result_iterable = client.QueryDocuments(collection['_self'], query, options)
    results = list(result_iterable);

    print(results)
    ```

<a id="update-your-connection-string" class="xliff"></a>

## Aktualizowanie parametrów połączenia

Teraz wróć do witryny Azure Portal, aby uzyskać informacje o parametrach połączenia i skopiować je do aplikacji.

1. W witrynie [Azure Portal](http://portal.azure.com/), korzystając ze swojego konta usługi Azure Cosmos DB, kliknij na lewym panelu nawigacyjnym pozycję **Klucze**, a następnie pozycję **Klucze odczytu i zapisu**. W następnym kroku, korzystając z przycisków kopiowania dostępnych po prawej stronie ekranu, skopiujesz identyfikator URI i klucz podstawowy do pliku `DocumentDBGetStarted.py`.

    ![Wyświetlanie i kopiowanie klucza dostępu w witrynie Azure Portal, blok Klucze](./media/create-documentdb-dotnet/keys.png)

2. Otwórz plik `DocumentDBGetStarted.py`. 

3. Skopiuj wartość identyfikatora URI z portalu (przy użyciu przycisku kopiowania) i przypisz ją do klucza endpoint w pliku `DocumentDBGetStarted.py`. 

    `config.ENDPOINT : "https://FILLME.documents.azure.com"`

4. Następnie skopiuj wartość klucza podstawowego z portalu i przypisz ją do klucza `config.MASTERKEY` w pliku `DocumentDBGetStarted.py`. Aplikacja została zaktualizowana i zawiera teraz wszystkie informacje potrzebne do nawiązania komunikacji z usługą Azure Cosmos DB. 

    `config.MASTERKEY : "FILLME"`
    
<a id="run-the-app" class="xliff"></a>

## Uruchomienie aplikacji
1. W programie Visual Studio kliknij prawym przyciskiem myszy projekt w **Eksploratorze rozwiązań**, wybierz bieżące środowisko Python, a następnie kliknij je prawym przyciskiem myszy.

2. Wybierz polecenie Zainstaluj pakiet języka Python, a następnie wpisz ciąg **pydocumentdb**.

3. Naciśnij klawisz F5, aby uruchomić aplikację. Aplikacja zostanie wyświetlona w przeglądarce. 

Teraz można wrócić do Eksploratora danych i zobaczyć, jak się pracuje z nowymi danymi, modyfikuje je i tworzy zapytania o nie. 

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

W tym przewodniku Szybki start wyjaśniono sposób tworzenia konta usługi Azure Cosmos DB, tworzenia kolekcji za pomocą Eksploratora danych i uruchamiania aplikacji. Teraz możesz zaimportować dodatkowe dane do swojego konta usługi Cosmos DB. 

> [!div class="nextstepaction"]
> [Importowanie danych do usługi Azure Cosmos DB na potrzeby interfejsu API usługi DocumentDB](import-data.md)



