---
title: "Szybki Start: Tabela interfejsu API z platformą .NET — rozwiązania Cosmos Azure DB | Dokumentacja firmy Microsoft"
description: "Ta opcja szybkiego startu przedstawia sposób użycia interfejsu API Azure rozwiązania Cosmos DB tabeli do tworzenia aplikacji z portalu Azure i .NET"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quickstart connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 11/20/2017
ms.author: mimig
ms.openlocfilehash: e0f0a95ea086e83ef0c46145b33b348071407aa5
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2017
---
# <a name="quickstart-build-a-table-api-app-with-net-and-azure-cosmos-db"></a>Szybki Start: Tworzenie tabeli aplikacji interfejsu API platformy .NET i Azure rozwiązania Cosmos bazy danych 

Ta opcja szybkiego startu przedstawia sposób użycia języka Java i Azure DB rozwiązania Cosmos [API tabeli](table-introduction.md) do tworzenia aplikacji w klonowania przykład z usługi GitHub. Tego przewodnika Szybki Start również pokazano, jak utworzyć konto bazy danych Azure rozwiązania Cosmos oraz sposobie używania Eksploratora danych do tworzenia tabel i jednostek w sieci web portalu Azure.

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

## <a name="prerequisites"></a>Wymagania wstępne

Jeśli nie masz jeszcze zainstalowanego programu Visual Studio 2017, możesz pobrać program [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) i używać go **bezpłatnie**. Podczas instalacji programu Visual Studio upewnij się, że jest włączona opcja **Programowanie na platformie Azure**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

> [!IMPORTANT] 
> Musisz utworzyć nowe konto tabeli interfejsu API do pracy z ogólnie dostępne zestawy SDK interfejsu API tabeli. Konta interfejsu API Tabela utworzona w okresie obowiązywania wersji zapoznawczej nie są obsługiwane przez ogólnie dostępne zestawy SDK.
>

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

3. Ponieważ ten samouczek nie korzysta z emulatora magazynu, usuń znaczniki komentarza StorageConnectionString na wiersz 8 ujmij w komentarz StorageConnectionString w wierszu 7. Wiersz 7 i 8 powinna wyglądać następująco:

    ```
    <!--key="StorageConnectionString" value="UseDevelopmentStorage=true;" />-->
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]" />
    ```

4. Wklej podstawowe parametry połączenia z portalu na wartość StorageConnectionString w wierszu 8. Wklej parametry wewnątrz cudzysłowów. 

    > [!IMPORTANT]
    > Jeśli documents.azure.com, która oznacza, że masz konto w wersji zapoznawczej, używa punktu końcowego i należy utworzyć [nowe konto interfejsu API tabeli](#create-a-database-account) do pracy z zestawu SDK ogólnie dostępna interfejsu API tabeli. 
    > 

    Wiersz 8 powinna wyglądać podobnie do:

    ```
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=txZACN9f...==;TableEndpoint=https://<account name>.table.cosmosdb.azure.com;" />
    ```

5. Zapisz plik App.config.

Aplikacja została zaktualizowana i zawiera teraz wszystkie informacje potrzebne do nawiązania komunikacji z usługą Azure Cosmos DB. 

## <a name="build-and-deploy-the-app"></a>Tworzenie i wdrażanie aplikacji

1. W programie Visual Studio, kliknij prawym przyciskiem myszy **TableStorage** projektu w **Eksploratora rozwiązań** , a następnie kliknij przycisk **Zarządzaj pakietami NuGet**. 

2. W polu **Przeglądaj** wpisz *Microsoft.Azure.CosmosDB.Table*.

3. Z wyników, należy zainstalować **Microsoft.Azure.CosmosDB.Table** biblioteki. Spowoduje to zainstalowanie pakietu interfejsu API Azure rozwiązania Cosmos DB tabeli, a także wszystkie zależności.

4. Otwórz BasicSamples.cs i Dodaj punkt przerwania 30 wiersz i wiersz 52.

5. Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.

    W oknie konsoli wyświetla dane w tabeli dodawane do nowej tabeli bazy danych w usłudze Azure DB rozwiązania Cosmos. 
    
    Jeśli wystąpi błąd o zależnościach, zobacz [Rozwiązywanie problemów](table-sdk-dotnet.md#troubleshooting).

    Po trafieniu punktu przerwania w pierwszym wrócić do Eksploratora danych w portalu Azure i rozwiń tabeli pokaz * i kliknij przycisk **jednostek**. **Jednostek** karta po prawej stronie zawiera nową jednostkę, która została dodana, należy zwrócić uwagę ten numer telefonu użytkownika jest 425-555-0101.
    
6. Zamknij kartę jednostek w Eksploratorze danych.
    
7. Kontynuuj uruchomić aplikację do następnego punktu przerwania.

    Po trafieniu punktu przerwania wrócić do portalu kliknij pozycję jednostek ponownie, aby otworzyć kartę jednostek i należy pamiętać, że numer telefonu został zaktualizowany do 425-555-0105.

8. W oknie konsoli naciśnij klawisze CTRL + C, aby zakończyć wykonywanie aplikacji. 

    Teraz można wrócić do Eksploratora danych i dodać lub zmodyfikować entitites i wykonywania zapytań o dane.

## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wyjaśniono sposób tworzenia konta usługi Azure Cosmos DB, tworzenia tabeli za pomocą Eksploratora danych i uruchamiania aplikacji.  Teraz można tworzyć zapytania do danych przy użyciu interfejsu API tabel.  

> [!div class="nextstepaction"]
> [Importowanie danych z tabeli do interfejsu API tabeli](table-import.md)

