---
title: "Tworzenie aplikacji .NET usługi Azure Cosmos DB za pomocą interfejsu API tabel | Microsoft Docs"
description: "Rozpoczynanie pracy z interfejsem API tabel usługi Azure Cosmos DB przy użyciu platformy .NET"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 06/22/2017
ms.author: arramac
ms.openlocfilehash: 0ce99a4754d7ec6f35bda63af6fc0166cf7e0eb4
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="azure-cosmos-db-build-a-net-application-using-the-table-api"></a>Azure Cosmos DB: Tworzenie aplikacji .NET za pomocą interfejsu API tabel

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

Ten przewodnik Szybki start przedstawia sposób tworzenia konta usługi Azure Cosmos DB i tworzenia tabeli w ramach konta przy użyciu witryny Azure Portal. Następnie napiszemy kod umożliwiający wstawianie, aktualizowanie i usuwanie jednostek oraz uruchomimy kilka zapytań przy użyciu nowego pakietu NuGet [Windows Azure Storage Premium Table](https://aka.ms/premiumtablenuget) (wersja zapoznawcza). Ta biblioteka ma takie same klasy i podpisy metod jak publiczny [zestaw SDK usługi Windows Azure Storage](https://www.nuget.org/packages/WindowsAzure.Storage), ale ma również możliwość łączenia się z kontami usługi Azure Cosmos DB przy użyciu [interfejsu API tabel](table-introduction.md) (wersja zapoznawcza). 

## <a name="prerequisites"></a>Wymagania wstępne

Jeśli nie masz jeszcze zainstalowanego programu Visual Studio 2017, możesz pobrać program [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) i używać go **bezpłatnie**. Podczas instalacji programu Visual Studio upewnij się, że jest włączona opcja **Programowanie na platformie Azure**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Dodawanie tabeli

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Dodawanie danych przykładowych

Teraz można dodać dane do nowej tabeli za pomocą Eksploratora danych (wersja zapoznawcza).

1. W Eksploratorze danych rozwiń węzeł **sample-table**, kliknij pozycję **Jednostki**, a następnie kliknij przycisk **Dodaj jednostkę**.

   ![Tworzenie nowych jednostek w Eksploratorze danych w witrynie Azure Portal](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. Teraz dodaj dane do pól wartości PartitionKey i RowKey, a następnie kliknij przycisk **Dodaj jednostkę**.

   ![Ustawianie klucza partycji i klucza wiersza dla nowej jednostki](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    Teraz możesz dodać więcej jednostek do swojej tabeli, edytować jednostki lub przeszukiwać dane w Eksploratorze danych. W Eksploratorze danych możesz również skalować przepływność oraz dodawać do tabeli procedury składowane, funkcje zdefiniowane przez użytkownika i wyzwalacze.

## <a name="clone-the-sample-application"></a>Klonowanie przykładowej aplikacji

Teraz sklonujemy aplikację Tabela z repozytorium GitHub, ustawimy parametry połączenia i uruchomimy ją. Zobaczysz, jak łatwo jest pracować programowo z danymi. 

1. Otwórz okno terminalu usługi Git, na przykład git bash, i za pomocą polecenia `cd` przejdź do katalogu roboczego.  

2. Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started.git
    ```

3. Następnie otwórz plik rozwiązania w programie Visual Studio. 

## <a name="review-the-code"></a>Przeglądanie kodu

Dokonajmy szybkiego przeglądu działań wykonywanych w aplikacji. Otwórz plik Program.cs i zobacz, że następujące wiersze kodu tworzą zasoby usługi Azure Cosmos DB. 

* Obiekt CloudTableClient został zainicjowany.

    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); 
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

* Tworzenie nowej tabeli, jeśli nie istnieje.

    ```csharp
    CloudTable table = tableClient.GetTableReference("people");
    table.CreateIfNotExists();
    ```

* Pewne czynności są wykonywane przy użyciu tabeli `TableOperation` klasy.

   ```csharp
   TableOperation insertOperation = TableOperation.Insert(item);
   table.Execute(insertOperation);
   ```
   
   ```csharp
   TableOperation retrieveOperation = TableOperation.Retrieve<T>(items[i].PartitionKey, items[i].RowKey);
   table.Execute(retrieveOperation);
   ```
   
   ```csharp
   TableOperation deleteOperation = TableOperation.Delete(items[i]);
   table.Execute(deleteOperation);
   ```


## <a name="update-your-connection-string"></a>Aktualizowanie parametrów połączenia

Teraz dokonamy aktualizacji informacji o parametrach połączenia, dzięki czemu aplikacja będzie mogła komunikować się z usługą Azure Cosmos DB. 

1. W programie Visual Studio otwórz plik app.config. 

2. W witrynie [Azure Portal](http://portal.azure.com/) w menu nawigacji usługi Azure Cosmos DB po lewej stronie kliknij pozycję **Parametry połączenia**. Następnie w nowym okienku kliknij przycisk kopiowania parametrów połączenia. 

    ![Wyświetlanie i kopiowanie punktu końcowego i klucza konta w okienku Parametry połączenia](./media/create-table-dotnet/keys.png)

3. Wklej tę wartość do pliku app.config jako wartość elementu PremiumStorageConnectionString. 

    `<add key="PremiumStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://COSMOSDB.documents.azure.com" />`    

    Możesz pozostawić ciąg StandardStorageConnectionString bez zmian.

Aplikacja została zaktualizowana i zawiera teraz wszystkie informacje potrzebne do nawiązania komunikacji z usługą Azure Cosmos DB. 

## <a name="run-the-console-app"></a>Uruchamianie aplikacji konsolowej

1. W programie Visual Studio kliknij prawym przyciskiem myszy projekt **PremiumTableGetStarted** w **Eksploratorze rozwiązań**, a następnie kliknij polecenie **Zarządzaj pakietami NuGet**. 

2. W polu **Przeglądaj** pakietu NuGet wpisz *WindowsAzure.Storage-PremiumTable*.

3. Zaznacz pole wyboru **Uwzględnij wersję wstępną**. 

4. Z zestawu wyników zainstaluj bibliotekę **WindowsAzure.Storage-PremiumTable**. Spowoduje to zainstalowanie wersji zapoznawczej pakietu interfejsu API tabel usługi Azure Cosmos DB oraz wszystkich zależności. Należy pamiętać, że jest to inny pakiet NuGet niż pakiet usługi Windows Azure Storage używany przez magazyn Tabel platformy Azure. 

5. Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.

    W oknie konsoli wyświetlane są dane dodane, pobrane, wyszukane, zastąpione i usunięte z tabeli. Po zakończeniu działania skryptu naciśnij dowolny klawisz, aby zamknąć okno konsoli. 
    
    ![Dane wyjściowe konsoli Szybkiego startu](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-console-output.png)

6. Jeśli chcesz zobaczyć nowe jednostki w Eksploratorze danych, po prostu zakomentuj wiersze 188–208 w pliku program.cs, tak aby nie zostały usunięte, a następnie ponownie uruchom przykład. 

    Możesz teraz powrócić do Eksploratora danych, kliknąć przycisk **Odśwież**, rozwinąć tabelę **osób** i kliknąć przycisk **Jednostki**, a następnie pracować z nowymi danymi. 

    ![Nowe jednostki w Eksploratorze danych](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-data-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie zamierzasz w przyszłości korzystać z tej aplikacji, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego przewodnika Szybki start: 

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij nazwę utworzonego zasobu. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę zasobu do usunięcia, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wyjaśniono sposób tworzenia konta usługi Azure Cosmos DB, tworzenia tabeli za pomocą Eksploratora danych i uruchamiania aplikacji.  Teraz można tworzyć zapytania do danych przy użyciu interfejsu API tabel.  

> [!div class="nextstepaction"]
> [Tworzenie zapytań za pomocą interfejsu API tabel](tutorial-query-table.md)

