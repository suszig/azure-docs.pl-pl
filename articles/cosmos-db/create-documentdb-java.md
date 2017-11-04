---
title: "Tworzenie bazy danych dokumentów usługi Azure Cosmos DB przy użyciu języka Java | Microsoft Docs"
description: "Przykładowy kod w języku Java, którego można użyć do nawiązywania połączenia z interfejsem API usługi DocumentDB usługi Azure Cosmos DB i do wykonywania w niej zapytań"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect, mvc, devcenter
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 10/30/2017
ms.author: mimig
ms.openlocfilehash: 5a793abdc24387ae2b758d29b9dfb25f134097d3
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-the-azure-portal"></a>Azure Cosmos DB: Tworzenie bazy danych dokumentów przy użyciu języka Java i witryny Azure Portal

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Przy użyciu bazy danych rozwiązania Cosmos platformy Azure, można szybko tworzyć i kwerend dokumentu zarządzanego, tabeli i bazy danych wykresu.

Ten przewodnik Szybki start tworzy bazę danych dokumentów przy użyciu narzędzi witryny Azure Portal dla usługi Azure Cosmos DB. Ponadto ten przewodnik Szybki start pokazuje, jak szybko utworzyć aplikację konsolową Java przy użyciu [interfejsu API języka Java usługi DocumentDB](documentdb-sdk-java.md). Instrukcje podane w tym przewodniku Szybki start można wykonać w dowolnym systemie operacyjnym, w którym można uruchomić oprogramowanie Java. Wykonując tego przewodnika Szybki Start będzie znane tworzenia i modyfikowania zasobów bazy danych dokumentów w Interfejsie użytkownika lub programowo, nastąpi swoich preferencji.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Ponadto: 

* [Zestaw Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * W systemie Ubuntu uruchom polecenie `apt-get install default-jdk`, aby zainstalować zestaw JDK.
    * Upewnij się, że zmienna środowiskowa JAVA_HOME wskazuje folder, w którym zainstalowano zestaw JDK.
* [Pobierz](http://maven.apache.org/download.cgi) i [zainstaluj](http://maven.apache.org/install.html) archiwum binarne [Maven](http://maven.apache.org/)
    * W systemie Ubuntu możesz uruchomić polecenie `apt-get install maven`, aby zainstalować narzędzie Maven.
* [Git](https://www.git-scm.com/)
    * W systemie Ubuntu możesz uruchomić polecenie `sudo apt-get install git`, aby zainstalować usługę Git.

## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

Przed utworzeniem bazy danych dokumentów musisz utworzyć konto bazy danych SQL (DocumentDB) z użyciem usługi Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Dodawanie kolekcji

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Dodawanie danych przykładowych

Teraz można dodać dane do nowej kolekcji za pomocą Eksploratora danych.

1. Rozwiń węzeł **elementów** kolekcji, kliknij przycisk **dokumenty** > **nowy dokument**.

   ![Tworzenie nowych dokumentów w Eksploratorze danych w witrynie Azure Portal](./media/create-documentdb-java/azure-cosmosdb-data-explorer-new-document.png)
  
2. Teraz dodać do kolekcji o następującej strukturze dokument i kliknij przycisk **zapisać**.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

    ![Kopiowanie danych json i klikanie pozycji Zapisz w Eksploratorze danych w witrynie Azure Portal](./media/create-documentdb-java/azure-cosmosdb-data-explorer-save-document.png)

3.  Tworzenie i zapisywanie jeden dokument więcej gdzie zmienić `id` 2 i inne właściwości można znaleźć w temacie dopasować zmiany. Nowe dokumenty mogą mieć dowolną strukturę, ponieważ usługa Azure Cosmos DB nie wymusza żadnego schematu danych.

## <a name="query-your-data"></a>Wyszukiwanie danych

Zapytania można teraz używać w Eksploratorze danych do pobierania i filtrowanie danych.

1. Zobacz, czy domyślnie zapytanie ma ustawioną `SELECT * FROM c`. To zapytanie domyślne pobiera i wyświetla wszystkie dokumenty w kolekcji. 

    ![Domyślne zapytanie w Eksploratorze danych jest "Wybierz * c"](./media/create-documentdb-java/azure-cosmosdb-data-explorer-query.png)

2. Zmień zapytanie, klikając **edytowanie filtru** przycisk dodawania `ORDER BY c._ts DESC` do pola predykatu zapytania, a następnie klikając **Zastosuj filtr**.

    ![Zmień zapytanie domyślne przez dodanie ORDER BY c._ts DESC i kliknij przycisk Zastosuj filtr](./media/create-documentdb-java/azure-cosmosdb-data-explorer-edit-query.png)

To zmodyfikować zapytanie Wyświetla dokumenty w kolejności malejącej według ich sygnatury czasowej, więc drugiego dokumentu jest wyświetlana na początku listy. Jeśli znasz składni SQL, możesz wprowadzić dowolną z obsługiwanych [zapytania SQL](documentdb-sql-query.md) w tym polu. 

Na tym kończy się pracę w Eksploratorze danych. Przed możemy przejść do pracy z kodem, należy pamiętać, można również użyć Eksploratora danych do utworzenia procedur składowanych, funkcje UDF i wyzwalaczy wykonywania logiki biznesowej po stronie serwera, jak również skalować przepływność. Eksplorator danych udostępnia wszystkie wbudowane programowe procedury dostępu do danych w interfejsach API, ale umożliwia łatwy dostęp do danych za pośrednictwem witryny Azure Portal.

## <a name="clone-the-sample-application"></a>Klonowanie przykładowej aplikacji

Teraz przejdźmy do pracy z kodem. Sklonujemy aplikację interfejsu API usługi DocumentDB z repozytorium GitHub, ustawimy parametry połączenia i uruchomimy ją. Zobaczysz, jak łatwo jest pracować programowo z danymi. 

1. Otwórz okno terminala git, np. git bash i użyj `cd` polecenie, aby przejść do folderu instalacji aplikacji przykładowej. 

    ```bash
    cd "C:\git-samples"
    ```

2. Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium. To polecenie tworzy kopię przykładowej aplikacji na komputerze.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-the-code"></a>Przeglądanie kodu

Ten krok jest opcjonalny. Jeśli chcesz się dowiedzieć, jak zasoby bazy danych są tworzone w kodzie, można przejrzeć poniższe fragmenty kodu. Fragmenty kodu są pobierane z `Program.java` plik w folderze C:\git-samples\azure-cosmos-db-documentdb-java-getting-started\src\GetStarted. W przeciwnym razie możesz przejść od razu do [zaktualizować parametry połączenia](#update-your-connection-string). 

* `DocumentClient`Inicjowanie. [DocumentClient](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client) zapewnia logiczna reprezentacja po stronie klienta dla usługi Azure DB rozwiązania Cosmos bazy danych. Ten klient jest używana do konfigurowania i wykonywać żądania z usługą.

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* [Baza danych](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._database) tworzenia.

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* [Elementu DocumentCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_collection) tworzenia.

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* Dokument tworzenia przy użyciu [createDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.createdocument) metody.

    ```java
    // Any Java object within your code can be serialized into JSON and written to Azure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* Zapytania SQL w formacie JSON są wykonywane przy użyciu [queryDocuments](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) metody.

    ```java
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setPageSize(-1);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    FeedResponse<Document> queryResults = this.client.queryDocuments(
        collectionLink,
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }
    ```    

## <a name="update-your-connection-string"></a>Aktualizowanie parametrów połączenia

Teraz wróć do witryny Azure Portal, aby uzyskać informacje o parametrach połączenia i skopiować je do aplikacji. Dzięki temu aplikacja do komunikacji z bazą danych hostowanej.

1. W [portalu Azure](http://portal.azure.com/), kliknij przycisk **klucze**. 

    Przyciski Kopiuj po prawej stronie ekranu do skopiowania najwyższą wartość identyfikatora URI.

    ![Wyświetlanie i kopiowanie kluczy dostępu w Azure strony portalu, kluczy](./media/create-documentdb-java/keys.png)

2. Otwórz `Program.java` plik z folderu C:\git-samples\azure-cosmos-db-documentdb-java-getting-started\src\GetStarted. 

3. Wklej wartość identyfikatora URI z portalu za pośrednictwem `https://FILLME.documents.azure.com` wierszu 45.

4. Wróć do portalu i skopiuj wartość klucza podstawowego, jak pokazano na zrzucie ekranu. Wklej wartość klucza podstawowego z portalu za pośrednictwem `FILLME` wierszu 46.

    Metoda getStartedDemo powinna wyglądać podobnie do poniższego: 
    
    ```java
    private void getStartedDemo() throws DocumentClientException, IOException {
        this.client = new DocumentClient("https://youraccountname.documents.azure.com:443/",
                "your-primary-key...RJhQrqQ5QQ==", 
                new ConnectionPolicy(),
                ConsistencyLevel.Session);
    ```

5. Zapisz plik Program.java.

## <a name="run-the-app"></a>Uruchomienie aplikacji

1. W oknie terminalu usługi Git za pomocą polecenia `cd` przejdź do folderu azure-cosmos-db-documentdb-java-getting-started.

    ```git
    cd "C:\git-samples\azure-cosmos-db-documentdb-java-getting-started"
    ```

2. W oknie terminalu usługi Git wpisz polecenie `mvn package`, aby zainstalować wymagane pakiety języka Java.

3. W oknie terminalu git Uruchom `mvn exec:java -D exec.mainClass=GetStarted.Program` można uruchomić aplikacji Java.

    Okno terminalu wyświetla powiadomienie, że FamilyDB bazy danych została utworzona. Naciśnij klawisz, aby utworzyć kolekcję, następnie przejdź do Eksploratora danych, i zobaczysz, że teraz zawiera FamilyDB bazy danych.
    
    Nadal klawiszy, aby utworzyć dokumentów, a następnie wykonaj zapytanie.
    
    W końcu program wszystkie zasoby z tej aplikacji są usuwane z Twojego konta, aby nie poniesiesz żadnych dodatkowych opłat. 

    ![Dane wyjściowe konsoli](./media/create-documentdb-java/console-output.png)


## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wyjaśniono sposób tworzenia konta usługi Azure Cosmos DB, bazy danych dokumentów i kolekcji za pomocą Eksploratora danych oraz uruchamiania aplikacji w celu wykonania tych samych czynności w sposób programowy. Teraz możesz zaimportować dane dodatkowe kolekcji bazy danych Azure rozwiązania Cosmos. 

> [!div class="nextstepaction"]
> [Importowanie danych do usługi Azure Cosmos DB](import-data.md)


