---
title: "Tworzenie aplikacji .NET usługi Azure Cosmos DB za pomocą interfejsu API tabel | Microsoft Docs"
description: "Rozpoczynanie pracy z interfejsem API tabel usługi Azure Cosmos DB przy użyciu platformy .NET"
services: cosmosdb
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmosdb
ms.custom: quick start connect
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: cba0b278d84e25876a8b73cedb7e35f84500fc5e
ms.contentlocale: pl-pl
ms.lasthandoff: 05/11/2017


---
# <a name="azure-cosmos-db-build-a-net-application-using-the-table-api"></a>Azure Cosmos DB: Tworzenie aplikacji .NET za pomocą interfejsu API tabel

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

Ten przewodnik Szybki start przedstawia sposób tworzenia konta usługi Azure Cosmos DB i tworzenia tabeli w ramach konta przy użyciu witryny Azure Portal. Następnie napiszesz kod do wstawiania, aktualizacji i usuwania jednostek oraz uruchomisz kilka zapytań. Z pakietu NuGet możesz pobrać [wersję zapoznawczą zestawu SDK usługi Azure Storage](https://aka.ms/premiumtablenuget), która ma takie same podpisy klas i metod jak publiczny [zestaw SDK usługi Azure Storage](https://www.nuget.org/packages/WindowsAzure.Storage), ale ma również możliwość łączenia się z kontami usługi Azure Cosmos DB przy użyciu [interfejsu API tabel](table-introduction.md) (wersja zapoznawcza). 

## <a name="prerequisites"></a>Wymagania wstępne

Jeśli nie masz jeszcze zainstalowanego programu Visual Studio 2017, możesz pobrać program [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) i używać go **bezpłatnie**. Podczas instalacji programu Visual Studio upewnij się, że jest włączona opcja **Programowanie na platformie Azure**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmosdb-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Dodawanie tabeli

[!INCLUDE [cosmosdb-create-table](../../includes/cosmosdb-create-table.md)]

## <a name="add-sample-data"></a>Dodawanie danych przykładowych

Teraz możesz dodać dane do swojej nowej tabeli za pomocą Eksploratora danych.

1. W Eksploratorze danych rozwiń węzeł **sample-database** i **sample-table**, kliknij pozycję **Jednostki**, a następnie kliknij przycisk **Dodawanie jednostki**.
2. Teraz dodaj dane do pól wartości PartitionKey i RowKey, a następnie kliknij przycisk **Dodaj jednostkę**.

   ![Tworzenie nowych dokumentów w Eksploratorze danych w witrynie Azure Portal](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
    Teraz możesz dodać więcej jednostek do swojej tabeli, edytować jednostki lub przeszukiwać dane w Eksploratorze danych. W Eksploratorze danych możesz również skalować przepływność oraz dodawać do tabeli procedury składowane, funkcje zdefiniowane przez użytkownika i wyzwalacze.

## <a name="clone-the-sample-application"></a>Klonowanie przykładowej aplikacji

Teraz sklonujemy aplikację interfejsu API usługi DocumentDB z repozytorium github, ustawimy parametry połączenia i uruchomimy ją. Zobaczysz, jak łatwo jest pracować programowo z danymi. 

1. Otwórz okno terminalu usługi Git, na przykład git bash, i za pomocą polecenia `cd` przejdź do katalogu roboczego.  

2. Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started.git
    ```

3. Następnie otwórz plik rozwiązania w programie Visual Studio. 

## <a name="review-the-code"></a>Przeglądanie kodu

Dokonajmy szybkiego przeglądu działań wykonywanych w aplikacji. Otwórz plik Program.cs i zobacz, że następujące wiersze kodu tworzą zasoby usługi Azure Cosmos DB. 

* Inicjowanie klienta DocumentClient.

    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); 
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

* Tworzenie nowej tabeli, jeśli nie istnieje.

    ```csharp
    CloudTable table = tableClient.GetTableReference("people");
    table.CreateIfNotExists();
    ```

* Tworzenie nowego kontenera tabeli. Można zauważyć, że ten kod jest bardzo podobny do zwykłego kodu zestawu SDK usługi Azure Table Storage. 

    ```csharp
    CustomerEntity item = new CustomerEntity()
                {
                    PartitionKey = Guid.NewGuid().ToString(),
                    RowKey = Guid.NewGuid().ToString(),
                    Email = $"{GetRandomString(6)}@contoso.com",
                    PhoneNumber = "425-555-0102",
                    Bio = GetRandomString(1000)
                };
    ```

## <a name="update-your-connection-string"></a>Aktualizowanie parametrów połączenia

Teraz wróć do witryny Azure Portal, aby uzyskać informacje o parametrach połączenia i skopiować je do aplikacji.

1. W witrynie [Azure Portal](http://portal.azure.com/), korzystając ze swojego konta usługi Azure Cosmos DB, kliknij na lewym panelu nawigacyjnym pozycję **Klucze**, a następnie pozycję **Klucze odczytu i zapisu**. W następnym kroku, korzystając z przycisków kopiowania dostępnych po prawej stronie ekranu, skopiujesz identyfikator URI i klucz podstawowy do pliku app.config.

    ![Wyświetlanie i kopiowanie klucza dostępu w witrynie Azure Portal, blok Klucze](./media/create-documentdb-dotnet-core/keys.png)

2. W programie Visual Studio otwórz plik app.config. 

3. Skopiuj nazwę swojego konta usługi Azure Cosmos DB z portalu i przypisz ją do nazwy AccountName w wartości ciągu PremiumStorageConnection w pliku app.config. Na powyższym zrzucie ekranu nazwa konta to cosmos-db-quickstart. Twoja nazwa konta jest w wyświetlana w górnej części portalu.

    `<add key="PremiumStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://COSMOSDB.documents.azure.com" />`

4. Następnie skopiuj swoją wartość klucza podstawowego z portalu i przypisz ją do klucza AccountKey w ciągu PremiumStorageConnectionString. 

    `AccountKey=AUTHKEY`

5. Wreszcie skopiuj swoją wartość identyfikatora URI ze strony Klucze portalu (za pomocą przycisku Kopiuj) i przypisz ją do wartości TableEndpoint ciągu PremiumStorageConnectionString.

    `TableEndpoint=https://COSMOSDB.documents.azure.com`

    Możesz pozostawić ciąg StandardStorageConnectionString bez zmian.

Aplikacja została zaktualizowana i zawiera teraz wszystkie informacje potrzebne do nawiązania komunikacji z usługą Azure Cosmos DB. 

## <a name="run-the-web-app"></a>Uruchamianie aplikacji sieci Web

1. W programie Visual Studio kliknij projekt prawym przyciskiem myszy w **Eksploratorze rozwiązań**, a następnie kliknij polecenie **Zarządzaj pakietami NuGet**. 

2. W polu **Przeglądaj** obszaru pakietów NuGet wpisz ciąg *WindowsAzure.Storage*, a następnie zaznacz pole **Uwzględnij wersję wstępną**. 

3. Z wyników zainstaluj bibliotekę **WindowsAzure.Storage**. Spowoduje to zainstalowanie wersji zapoznawczej pakietu interfejsu API tabel usługi Azure Cosmos DB oraz wszystkich zależności.

4. Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.

    W oknie konsoli są wyświetlane dane dodawane do tabeli. Po zakończeniu działania skryptu zamknij okno konsoli. 

Teraz można wrócić do Eksploratora danych i zobaczyć, jak się pracuje z nowymi danymi, modyfikuje je i tworzy zapytania o nie. 

## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmosdb-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie zamierzasz w przyszłości korzystać z tej aplikacji, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego przewodnika Szybki start: 

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij nazwę utworzonego zasobu. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę zasobu do usunięcia, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wyjaśniono sposób tworzenia konta usługi Azure Cosmos DB, tworzenia tabeli za pomocą Eksploratora danych i uruchamiania aplikacji.  Teraz można tworzyć zapytania do danych przy użyciu interfejsu API tabel.  

> [!div class="nextstepaction"]
> [Tworzenie zapytań za pomocą interfejsu API tabel](tutorial-query-table.md)


