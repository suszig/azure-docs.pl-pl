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
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 08/02/2017
ms.author: mimig
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: f22e886ab3e59e69607e572d05a9f01d3d57f181
ms.contentlocale: pl-pl
ms.lasthandoff: 08/08/2017

---
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-the-azure-portal"></a>Azure Cosmos DB: Tworzenie bazy danych dokumentów przy użyciu języka Java i witryny Azure Portal

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

Ten przewodnik Szybki start tworzy bazę danych dokumentów przy użyciu narzędzi witryny Azure Portal dla usługi Azure Cosmos DB. Ponadto ten przewodnik Szybki start pokazuje, jak szybko utworzyć aplikację konsolową Java przy użyciu [interfejsu API języka Java usługi DocumentDB](documentdb-sdk-java.md). Instrukcje podane w tym przewodniku Szybki start można wykonać w dowolnym systemie operacyjnym, w którym można uruchomić oprogramowanie Java. Realizując ten przewodnik Szybki start, zaznajomisz się z tworzeniem i modyfikowaniem zasobów bazy danych dokumentów z wykorzystaniem interfejsu użytkownika lub programowo, zgodnie z preferencjami.

## <a name="prerequisites"></a>Wymagania wstępne

* [Zestaw Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * W systemie Ubuntu uruchom polecenie `apt-get install default-jdk`, aby zainstalować zestaw JDK.
    * Upewnij się, że zmienna środowiskowa JAVA_HOME wskazuje folder, w którym zainstalowano zestaw JDK.
* [Pobierz](http://maven.apache.org/download.cgi) i [zainstaluj](http://maven.apache.org/install.html) archiwum binarne [Maven](http://maven.apache.org/)
    * W systemie Ubuntu możesz uruchomić polecenie `apt-get install maven`, aby zainstalować narzędzie Maven.
* [Git](https://www.git-scm.com/)
    * W systemie Ubuntu możesz uruchomić polecenie `sudo apt-get install git`, aby zainstalować usługę Git.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

Przed utworzeniem bazy danych grafów musisz utworzyć konto bazy danych SQL (DocumentDB) z użyciem usługi Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Dodawanie kolekcji

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Dodawanie danych przykładowych

Teraz można dodać dane do nowej kolekcji za pomocą Eksploratora danych.

1. W Eksploratorze danych nowa baza danych jest wyświetlana w okienku Kolekcje. Rozwiń bazę danych **Tasks**, rozwiń kolekcję **Items**, kliknij pozycję **Dokumenty**, a następnie kliknij pozycję **Nowe dokumenty**. 

   ![Tworzenie nowych dokumentów w Eksploratorze danych w witrynie Azure Portal](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. Teraz dodaj do kolekcji dokument o następującej strukturze.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. Po dodaniu danych json do karty **Dokumenty** kliknij pozycję **Zapisz**.

    ![Kopiowanie danych json i klikanie pozycji Zapisz w Eksploratorze danych w witrynie Azure Portal](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  Utwórz i zapisz jeszcze jeden dokument, w którym wstawisz unikatową wartość dla właściwości `id` i zmienisz inne właściwości wedle uznania. Nowe dokumenty mogą mieć dowolną strukturę, ponieważ usługa Azure Cosmos DB nie wymusza żadnego schematu danych.

     Teraz możesz używać zapytań w Eksploratorze danych, aby pobierać dane, klikając przyciski **Edytuj filtr** i **Zastosuj filtr**. Domyślnie Eksplorator danych używa instrukcji `SELECT * FROM c` do pobrania wszystkich dokumentów w kolekcji, ale można zmienić tę instrukcję na inne [zapytanie SQL](documentdb-sql-query.md), np. `SELECT * FROM c ORDER BY c._ts DESC`, aby zwrócić wszystkie dokumenty w kolejności malejącej w oparciu o sygnaturę czasową. 
 
     Eksplorator danych umożliwia również tworzenie procedur składowanych, funkcji definiowanych przez użytkownika (UDF) i wyzwalaczy w celu wykonania logiki biznesowej po stronie serwera oraz skalowania przepływności. Eksplorator danych udostępnia wszystkie wbudowane programowe procedury dostępu do danych w interfejsach API, ale umożliwia łatwy dostęp do danych za pośrednictwem witryny Azure Portal.

## <a name="clone-the-sample-application"></a>Klonowanie przykładowej aplikacji

Teraz przejdźmy do pracy z kodem. Sklonujemy aplikację interfejsu API usługi DocumentDB z repozytorium GitHub, ustawimy parametry połączenia i uruchomimy ją. Zobaczysz, jak łatwo jest pracować programowo z danymi. 

1. Otwórz okno terminalu usługi Git, na przykład git bash, i za pomocą polecenia `CD` przejdź do katalogu roboczego.  

2. Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-the-code"></a>Przeglądanie kodu

Dokonajmy szybkiego przeglądu działań wykonywanych w aplikacji. Otwórz plik `Program.java` z folderu \src\GetStarted i znajdź następujące wiersze kodu tworzące zasoby usługi Azure Cosmos DB. 

* Inicjowanie obiektu `DocumentClient`.

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* Tworzenie nowej bazy danych.

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* Tworzenie nowej kolekcji.

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* Tworzenie kilku dokumentów.

    ```java
    // Any Java object within your code can be serialized into JSON and written to Azure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* Wykonywanie zapytania SQL w formacie JSON.

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

Teraz wróć do witryny Azure Portal, aby uzyskać informacje o parametrach połączenia i skopiować je do aplikacji. Umożliwi to aplikacji komunikację z hostowaną bazą danych.

1. W witrynie [Azure Portal](http://portal.azure.com/), korzystając ze swojego konta usługi Azure Cosmos DB, kliknij na lewym panelu nawigacyjnym pozycję **Klucze**, a następnie pozycję **Klucze odczytu i zapisu**. W następnym kroku, korzystając z przycisków kopiowania dostępnych po prawej stronie ekranu, skopiujesz wartości IDENTYFIKATOR URI i KLUCZ PODSTAWOWY do pliku `Program.java`.

    ![Wyświetlanie i kopiowanie klucza dostępu w witrynie Azure Portal, blok Klucze](./media/create-documentdb-dotnet/keys.png)

2. Po otworzeniu pliku `Program.java` skopiuj wartość IDENTYFIKATOR URI z portalu (przy użyciu przycisku kopiowania) i przypisz ją jako wartość punktu końcowego konstruktora DocumentClient w pliku `Program.java`. 

    `"https://FILLME.documents.azure.com"`

4. Następnie skopiuj wartość KLUCZ PODSTAWOWY z portalu i wklej ją zamiast ciągu „FILLME”. W ten sposób stanie się ona drugim parametrem w konstruktorze DocumentClient. Aplikacja została zaktualizowana i zawiera teraz wszystkie informacje potrzebne do nawiązania komunikacji z usługą Azure Cosmos DB. 
    
## <a name="run-the-app"></a>Uruchomienie aplikacji

1. W oknie terminalu usługi Git za pomocą polecenia `cd` przejdź do folderu azure-cosmos-db-documentdb-java-getting-started.

2. W oknie terminalu usługi Git wpisz polecenie `mvn package`, aby zainstalować wymagane pakiety języka Java.

3. W oknie terminalu usługi Git uruchom polecenie `mvn exec:java -D exec.mainClass=GetStarted.Program`, aby uruchomić aplikację Java.

    W oknie terminalu otrzymasz powiadomienie o utworzeniu bazy danych FamilyDB oraz monit o naciśnięcie klawisza w celu kontynuowania. Naciśnij klawisz, aby utworzyć bazę danych, a następnie przejdź do Eksploratora danych, a zobaczysz, że teraz zawiera bazę danych FamilyDB. Kontynuuj naciskanie klawiszy, aby utworzyć kolekcję i dokumenty, a następnie wykonaj zapytanie. Po zakończeniu projektu zasoby zostaną usunięte z Twojego konta. 

    ![Wyświetlanie i kopiowanie klucza dostępu w witrynie Azure Portal, blok Klucze](./media/create-documentdb-java/console-output.png)


## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie zamierzasz w przyszłości korzystać z tej aplikacji, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego przewodnika Szybki start:

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij nazwę utworzonego zasobu. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę zasobu do usunięcia, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wyjaśniono sposób tworzenia konta usługi Azure Cosmos DB, bazy danych dokumentów i kolekcji za pomocą Eksploratora danych oraz uruchamiania aplikacji w celu wykonania tych samych czynności w sposób programowy. Teraz możesz zaimportować dodatkowe dane do swojego konta usługi Cosmos DB. 

> [!div class="nextstepaction"]
> [Importowanie danych do usługi Azure Cosmos DB](import-data.md)



