---
title: "Szybki Start: Tabela interfejsu API za pomocą języka Python — rozwiązania Cosmos Azure DB | Dokumentacja firmy Microsoft"
description: "Ta opcja szybkiego startu przedstawia sposób użycia interfejsu API Azure rozwiązania Cosmos DB tabeli do tworzenia aplikacji z portalu Azure i języka Python"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: quickstart
ms.date: 11/15/2017
ms.author: mimig
ms.openlocfilehash: 0900ec1931cc622339133393b72b558076a42710
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="quickstart-build-a-table-api-app-with-python-and-azure-cosmos-db"></a>Szybki Start: Tworzenie tabeli aplikacji interfejsu API z języka Python i Azure rozwiązania Cosmos bazy danych

Ta opcja szybkiego startu przedstawia sposób użycia języka Python i usłudze Azure DB rozwiązania Cosmos [API tabeli](table-introduction.md) do tworzenia aplikacji w klonowania przykład z usługi GitHub. Tego przewodnika Szybki Start również pokazano, jak utworzyć konto bazy danych Azure rozwiązania Cosmos oraz sposobie używania Eksploratora danych do tworzenia tabel i jednostek w sieci web portalu Azure.

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Można szybko utworzyć i wyszukiwać dokumentu, klucz/wartość kolumny całej i baz danych wykresu, które korzystają z globalnego dystrybucji i skalowanie w poziomie możliwości fundament bazy danych Azure rozwiązania Cosmos. 

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Ponadto:

* Jeśli nie masz jeszcze zainstalowanego programu Visual Studio 2017, możesz pobrać program [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) i używać go **bezpłatnie**. Podczas instalacji programu Visual Studio upewnij się, że jest włączona opcja **Programowanie na platformie Azure**.
* Narzędzia Python Tools for Visual Studio pobrane z witryny [GitHub](http://microsoft.github.io/PTVS/). W tym samouczku są używane narzędzia Python Tools for VS 2015.
* Środowisko Python w wersji 2.7 pobrane z witryny [python.org](https://www.python.org/downloads/release/python-2712/)

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
    git clone https://github.com/Azure-Samples/storage-python-getting-started.git
    ```

3. Następnie otwórz plik rozwiązania w programie Visual Studio. 

## <a name="update-your-connection-string"></a>Aktualizowanie parametrów połączenia

Teraz wróć do witryny Azure Portal, aby uzyskać informacje o parametrach połączenia i skopiować je do aplikacji. Dzięki temu aplikacja do komunikacji z bazą danych hostowanej. 

1. W [portalu Azure](http://portal.azure.com/), kliknij przycisk **ciąg połączenia**. 

    ![Wyświetl i skopiuj parametry połączenia w okienku parametry połączenia](./media/create-table-python/connection-string.png)

2. Otwórz plik config.py i skopiować właściwości parametrów połączenia wymagane do pliku konfiguracji.

3. Zapisz plik config.py.

## <a name="run-the-app"></a>Uruchomienie aplikacji

1. W programie Visual Studio kliknij prawym przyciskiem myszy projekt w **Eksploratorze rozwiązań**, wybierz bieżące środowisko Python, a następnie kliknij je prawym przyciskiem myszy.

2. Wybierz zainstaluj pakiet języka Python, a następnie wpisz w **tabel azure — Magazyn**

3. Naciśnij klawisz F5, aby uruchomić aplikację. Aplikacja zostanie wyświetlona w przeglądarce. 

Teraz można wrócić do Eksploratora danych i zobaczyć, jak się pracuje z nowymi danymi, modyfikuje je i tworzy zapytania o nie. 

## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wyjaśniono sposób tworzenia konta usługi Azure Cosmos DB, tworzenia tabeli za pomocą Eksploratora danych i uruchamiania aplikacji.  Teraz można tworzyć zapytania do danych przy użyciu interfejsu API tabel.  

> [!div class="nextstepaction"]
> [Importowanie danych z tabeli do interfejsu API tabeli](table-import.md)
