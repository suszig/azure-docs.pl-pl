---
title: "Szybki Start: Tabela interfejsu API z platformą .NET — rozwiązania Cosmos Azure DB | Dokumentacja firmy Microsoft"
description: "Ta opcja szybkiego startu przedstawia sposób użycia interfejsu API Azure rozwiązania Cosmos DB tabeli do tworzenia aplikacji z portalu Azure i .NET"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quickstart connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 11/15/2017
ms.author: arramac
ms.openlocfilehash: 5d22b23d687dba2382e009e73f20014a5d528d78
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="quickstart-build-a-table-api-app-with-net-and-azure-cosmos-db"></a>Szybki Start: Tworzenie tabeli aplikacji interfejsu API platformy .NET i Azure rozwiązania Cosmos bazy danych 

Ta opcja szybkiego startu przedstawia sposób użycia języka Java i Azure DB rozwiązania Cosmos [API tabeli](table-introduction.md) do tworzenia aplikacji w klonowania przykład z usługi GitHub. Tego przewodnika Szybki Start również pokazano, jak utworzyć konto bazy danych Azure rozwiązania Cosmos oraz sposobie używania Eksploratora danych do tworzenia tabel i jednostek w sieci web portalu Azure.

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

## <a name="prerequisites"></a>Wymagania wstępne

Jeśli nie masz jeszcze zainstalowanego programu Visual Studio 2017, możesz pobrać program [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) i używać go **bezpłatnie**. Podczas instalacji programu Visual Studio upewnij się, że jest włączona opcja **Programowanie na platformie Azure**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Dodawanie tabeli

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Dodawanie danych przykładowych

Teraz możesz dodać dane do swojej nowej tabeli za pomocą Eksploratora danych.

1. W Eksploratorze danych rozwiń węzeł **sample-table**, kliknij pozycję **Jednostki**, a następnie kliknij przycisk **Dodaj jednostkę**.

   ![Tworzenie nowych jednostek w Eksploratorze danych w witrynie Azure Portal](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. Teraz Dodaj PartitionKey wartości pól oraz RowKey wartości pola danych i kliknij przycisk **Dodaj jednostki**.

   ![Ustawianie klucza partycji i klucza wiersza dla nowej jednostki](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    Teraz możesz dodać więcej jednostek do swojej tabeli, edytować jednostki lub przeszukiwać dane w Eksploratorze danych. W Eksploratorze danych możesz również skalować przepływność oraz dodawać do tabeli procedury składowane, funkcje zdefiniowane przez użytkownika i wyzwalacze.

## <a name="clone-the-sample-application"></a>Klonowanie przykładowej aplikacji

Teraz sklonujemy aplikację Tabela z repozytorium GitHub, ustawimy parametry połączenia i uruchomimy ją. Zobaczysz, jak łatwo jest pracować programowo z danymi. 

1. Otwórz okno terminala git, np. git bash i użyj `cd` polecenie, aby przejść do folderu instalacji aplikacji przykładowej. 

    ```bash
    cd "C:\git-samples"
    ```

2. Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium. To polecenie tworzy kopię przykładowej aplikacji na komputerze. 

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-dotnet-getting-started.git
    ```

3. Następnie otwórz plik rozwiązania TableStorage w programie Visual Studio. 

## <a name="update-your-connection-string"></a>Aktualizowanie parametrów połączenia

Teraz wróć do witryny Azure Portal, aby uzyskać informacje o parametrach połączenia i skopiować je do aplikacji. Dzięki temu aplikacja do komunikacji z bazą danych hostowanej. 

1. W [portalu Azure](http://portal.azure.com/), kliknij przycisk **ciąg połączenia**. 

    Przyciski Kopiuj po prawej stronie ekranu do skopiuj parametry połączenia podstawowej.

    ![Wyświetl i skopiuj parametry połączenia podstawowej w okienku parametry połączenia](./media/create-table-dotnet/connection-string.png)

2. W programie Visual Studio Otwórz plik App.config. 

3. Ponieważ ten samouczek nie korzysta z emulatora magazynu, usuń znaczniki komentarza StorageConnectionString na wiersz 8 ujmij w komentarz StorageConnectionString w wierszu 7. 

3. Wklej wartość podstawowe parametry połączenia do wartości StorageConnectionString w wierszu 8. 

    ```
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]" />`
    ```

    Wiersz 8 powinna wyglądać podobnie do

    ```
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=txZACN9f...==;TableEndpoint=https://<account name>.table.cosmosdb.azure.com;" />
    ```

4. Zapisz plik App.config.

Aplikacja została zaktualizowana i zawiera teraz wszystkie informacje potrzebne do nawiązania komunikacji z usługą Azure Cosmos DB. 

## <a name="build-and-deploy-the-app"></a>Tworzenie i wdrażanie aplikacji

1. W programie Visual Studio, kliknij prawym przyciskiem myszy **TableStorage** projektu w **Eksploratora rozwiązań** , a następnie kliknij przycisk **Zarządzaj pakietami NuGet**. 

2. W polu **Przeglądaj** wpisz *Microsoft.Azure.CosmosDB.Table*.

3. Z wyników, należy zainstalować **Microsoft.Azure.CosmosDB.Table** biblioteki. Spowoduje to zainstalowanie pakietu interfejsu API Azure rozwiązania Cosmos DB tabeli, a także wszystkie zależności.

4. Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.

    W oknie konsoli wyświetla dane w tabeli dodawane do nowej tabeli bazy danych w usłudze Azure DB rozwiązania Cosmos.

    Teraz można wrócić do Eksploratora danych i zobaczyć, jak się pracuje z nowymi danymi, modyfikuje je i tworzy zapytania o nie.

## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wyjaśniono sposób tworzenia konta usługi Azure Cosmos DB, tworzenia tabeli za pomocą Eksploratora danych i uruchamiania aplikacji.  Teraz można tworzyć zapytania do danych przy użyciu interfejsu API tabel.  

> [!div class="nextstepaction"]
> [Importowanie danych z tabeli do interfejsu API tabeli](table-import.md)

