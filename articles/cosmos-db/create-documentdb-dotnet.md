---
title: "Azure Cosmos DB: Budowanie aplikacji sieci Web za pomocą platformy .NET i interfejsu API usługi DocumentDB | Microsoft Docs"
description: "Przykładowy kod platformy .NET, którego można użyć do nawiązywania połączenia z interfejsem API usługi DocumentDB usługi Azure Cosmos DB i do wykonywania w niej zapytań"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc, devcenter
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: b8ab132a3e90032c4d70c310a2dd88f7441c4f0a
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-web-app-with-net-and-the-azure-portal"></a>Azure Cosmos DB: Budowanie aplikacji sieci Web interfejsu API usługi DocumentDB za pomocą platformy .NET i witryny Azure Portal

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

Ten przewodnik Szybki start przedstawia sposób tworzenia konta usługi Azure Cosmos DB, bazy danych dokumentów i kolekcji przy użyciu witryny Azure Portal. Następnie zbudujesz i wdrożysz aplikację sieci Web z listą zadań do wykonania bazującą na [interfejsie API usługi DocumentDB platformy .NET](documentdb-sdk-dotnet.md), jak pokazano na poniższym zrzucie ekranu. 

![Aplikacja z listą zadań do wykonania z przykładowymi danymi](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

## <a name="prerequisites"></a>Wymagania wstępne

Jeśli nie masz jeszcze zainstalowanego programu Visual Studio 2017, możesz pobrać program [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) i używać go **bezpłatnie**. Podczas instalacji programu Visual Studio upewnij się, że jest włączona opcja **Programowanie na platformie Azure**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]  

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="create-collection"></a>
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

     Teraz można użyć zapytań w Eksploratorze danych w celu pobrania danych. Domyślnie Eksplorator danych używa instrukcji `SELECT * FROM c` do pobrania wszystkich dokumentów w kolekcji, ale można zmienić tę instrukcję na inne [zapytanie SQL](documentdb-sql-query.md), np. `SELECT * FROM c ORDER BY c._ts DESC`, aby zwrócić wszystkie dokumenty w kolejności malejącej w oparciu o sygnaturę czasową.
 
     Eksplorator danych umożliwia również tworzenie procedur składowanych, funkcji definiowanych przez użytkownika (UDF) i wyzwalaczy w celu wykonania logiki biznesowej po stronie serwera oraz skalowania przepływności. Eksplorator danych udostępnia wszystkie wbudowane programowe procedury dostępu do danych w interfejsach API, ale umożliwia łatwy dostęp do danych za pośrednictwem witryny Azure Portal.

## <a name="clone-the-sample-application"></a>Klonowanie przykładowej aplikacji

Teraz przejdźmy do pracy z kodem. Sklonujemy aplikację interfejsu API usługi DocumentDB z repozytorium GitHub, ustawimy parametry połączenia i uruchomimy ją. Zobaczysz, jak łatwo jest pracować programowo z danymi. 

1. Otwórz okno terminalu usługi Git, na przykład git bash, i za pomocą polecenia `CD` przejdź do katalogu roboczego.  

2. Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium. 

    ```bash
    git clone https://github.com/Azure-Samples/documentdb-dotnet-todo-app.git
    ```

3. Następnie otwórz plik rozwiązania z listą zadań do wykonania w programie Visual Studio. 

## <a name="review-the-code"></a>Przeglądanie kodu

Dokonajmy szybkiego przeglądu działań wykonywanych w aplikacji. Otwórz plik DocumentDBRepository.cs i zobacz, że następujące wiersze kodu tworzą zasoby usługi Azure Cosmos DB. 

* Obiektu DocumentClient został zainicjowany w wierszu 78.

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);
    ```

* Nowa baza danych jest tworzony w wierszu 93.

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* Nowa kolekcja jest tworzony w wierszu 112.

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new DocumentCollection
            {
               Id = CollectionId,
               PartitionKey = new PartitionKeyDefinition() { Paths = new Collection<string>() { "/category" } }
            },
        new RequestOptions { OfferThroughput = 1000 });
    ```

## <a name="update-your-connection-string"></a>Aktualizowanie parametrów połączenia

Teraz wróć do witryny Azure Portal, aby uzyskać informacje o parametrach połączenia i skopiować je do aplikacji.

1. W witrynie [Azure Portal](http://portal.azure.com/), korzystając ze swojego konta usługi Azure Cosmos DB, kliknij na lewym panelu nawigacyjnym pozycję **Klucze**, a następnie pozycję **Klucze odczytu i zapisu**. W następnym kroku, korzystając z przycisków kopiowania dostępnych po prawej stronie ekranu, skopiujesz identyfikator URI i klucz podstawowy do pliku web.config.

    ![Wyświetlanie i kopiowanie klucza dostępu w witrynie Azure Portal, blok Klucze](./media/create-documentdb-dotnet/keys.png)

2. W programie Visual Studio 2017 otwórz plik web.config. 

3. Skopiuj wartość identyfikatora URI z portalu (przy użyciu przycisku kopiowania) i przypisz ją do klucza endpoint w pliku web.config. 

    `<add key="endpoint" value="FILLME" />`

4. Następnie skopiuj wartość klucza podstawowego z portalu i przypisz ją do klucza authKey w pliku web.config. Aplikacja została zaktualizowana i zawiera teraz wszystkie informacje potrzebne do nawiązania komunikacji z usługą Azure Cosmos DB. 

    `<add key="authKey" value="FILLME" />`
    
## <a name="run-the-web-app"></a>Uruchamianie aplikacji sieci Web
1. W programie Visual Studio kliknij projekt prawym przyciskiem myszy w **Eksploratorze rozwiązań**, a następnie kliknij polecenie **Zarządzaj pakietami NuGet**. 

2. W polu **Przeglądaj** pakietu NuGet wpisz ciąg *DocumentDB*.

3. Korzystając z wyników, zainstaluj bibliotekę **Microsoft.Azure.DocumentDB**. Spowoduje to zainstalowanie pakietu Microsoft.Azure.DocumentDB, a także wszystkich zależności.

4. Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Aplikacja zostanie wyświetlona w przeglądarce. 

5. Kliknij pozycję **Utwórz nowy** w przeglądarce i utwórz kilka nowych zadań w swojej aplikacji z zadaniami do wykonania.

   ![Aplikacja z listą zadań do wykonania z przykładowymi danymi](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

Teraz można wrócić do Eksploratora danych i zobaczyć, jak się pracuje z nowymi danymi, modyfikuje je i tworzy zapytania o nie. 

## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie zamierzasz w przyszłości korzystać z tej aplikacji, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego przewodnika Szybki start:

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij nazwę utworzonego zasobu. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę zasobu do usunięcia, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wyjaśniono sposób tworzenia konta usługi Azure Cosmos DB, tworzenia kolekcji za pomocą Eksploratora danych i uruchamiania aplikacji sieci Web. Teraz możesz zaimportować dodatkowe dane do swojego konta usługi Cosmos DB. 

> [!div class="nextstepaction"]
> [Importowanie danych do usługi Azure Cosmos DB](import-data.md)


