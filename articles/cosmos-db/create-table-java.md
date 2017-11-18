---
title: "Szybki Start: Tabela interfejsu API za pomocą języka Java — rozwiązania Cosmos Azure DB | Dokumentacja firmy Microsoft"
description: "Ta opcja szybkiego startu przedstawia sposób użycia interfejsu API Azure rozwiązania Cosmos DB tabeli do tworzenia aplikacji z portalu Azure i Java"
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
ms.devlang: java
ms.topic: quickstart
ms.date: 11/16/2017
ms.author: arramac
ms.openlocfilehash: ce5c1c25a73bdc02d7e66768752c6942144de517
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="quickstart-build-a-table-api-app-with-java-and-azure-cosmos-db"></a>Szybki Start: Tworzenie tabeli aplikacji interfejsu API języka Java i Azure rozwiązania Cosmos bazy danych

Ta opcja szybkiego startu przedstawia sposób użycia języka Java i Azure DB rozwiązania Cosmos [API tabeli](table-introduction.md) do tworzenia aplikacji w klonowania przykład z usługi GitHub. Tego przewodnika Szybki Start również pokazano, jak utworzyć konto bazy danych Azure rozwiązania Cosmos oraz sposobie używania Eksploratora danych do tworzenia tabel i jednostek w sieci web portalu Azure.

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

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
    git clone https://github.com/Azure-Samples/storage-table-java-getting-started.git 
    ```

## <a name="update-your-connection-string"></a>Aktualizowanie parametrów połączenia

Teraz wróć do witryny Azure Portal, aby uzyskać informacje o parametrach połączenia i skopiować je do aplikacji. Dzięki temu aplikacja do komunikacji z bazą danych hostowanej. 

1. W [portalu Azure](http://portal.azure.com/), kliknij przycisk **ciąg połączenia**. 

   ![Wyświetlanie i kopiowanie informacji o ciągu wymagane połączenie z okienka parametry połączenia](./media/create-table-java/connection-string.png)

2. Skopiuj podstawowy parametry połączenia za pomocą przycisku Kopiuj po prawej stronie.

3. Otwórz config.properties z folderu C:\git-samples\storage-table-java-getting-started\src\main\resources. 

5. W komentarz wiersz jedną i Usuń komentarz linii dwa. Pierwsze dwa wiersze powinna wyglądać następująco.

    ```
    #StorageConnectionString = UseDevelopmentStorage=true
    StorageConnectionString = DefaultEndpointsProtocol=https;AccountName=[ACCOUNTNAME];AccountKey=[ACCOUNTKEY]
    ```

6. Wklej podstawowe parametry połączenia z portalu do wartości StorageConnectionString w wierszu 2. Jeśli punkt końcowy część ciągu połączenia używa documents.azure.com, popraw tej części, aby zamiast niej używać table.cosmosdb.azure.com.

7. Zapisz plik config.properties.

Aplikacja została zaktualizowana i zawiera teraz wszystkie informacje potrzebne do nawiązania komunikacji z usługą Azure Cosmos DB. 

## <a name="run-the-app"></a>Uruchomienie aplikacji

1. W oknie terminalu git `cd` do folderu magazynu tabeli java-getting-started.

    ```git
    cd "C:\git-samples\storage-table-java-getting-started"
    ```

2. W usłudze git okno terminalu, uruchom następujące polecenia, aby uruchomić uruchomienie aplikacji Java.

    ```git
    mvn compile exec:java 
    ```

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
