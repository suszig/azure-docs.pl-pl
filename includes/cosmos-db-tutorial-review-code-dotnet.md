Dokonajmy szybkiego przeglądu działań wykonywanych w aplikacji. Otwórz plik DocumentDBRepository.cs i zobacz, że następujące wiersze kodu tworzą zasoby usługi DocumentDB. 

* Inicjowanie klienta DocumentClient.

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* Tworzenie nowej bazy danych.

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* Tworzenie nowej kolekcji.

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```
