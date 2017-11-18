---
title: "Szybki Start: Cassandra interfejsu API za pomocą języka Node.js — rozwiązania Cosmos Azure DB | Dokumentacja firmy Microsoft"
description: "Ta opcja szybkiego startu przedstawia sposób użycia interfejsu API Azure rozwiązania Cosmos DB Cassandra do tworzenia profilu aplikacji za pomocą języka Node.js"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 4732e57d-32ed-40e2-b148-a8df4ff2630d
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 11/15/2017
ms.author: govindk
ms.openlocfilehash: abf88cf96b32bc4168cb8c09a6e70ad0e395e88c
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="quickstart-build-a-cassandra-app-with-nodejs-and-azure-cosmos-db"></a>Szybki Start: Tworzenie aplikacji Cassandra Node.js i Azure rozwiązania Cosmos bazy danych

Ta opcja szybkiego startu przedstawia sposób użycia środowiska Node.js i Azure DB rozwiązania Cosmos [Cassandra API](cassandra-introduction.md) do tworzenia aplikacji profil w klonowania przykład z usługi GitHub. Ta opcja szybkiego startu również przeprowadzi Cię przez tworzenie konta bazy danych rozwiązania Cosmos Azure przy użyciu portalu Azure opartej na sieci web.

Azure DB rozwiązania Cosmos jest usługa globalnie rozproszone wielu modelu bazy danych firmy Microsoft. Można szybko tworzyć i kwerend dokumentu, tabeli, klucz wartość i baz danych wykresu, które korzystają z globalnego dystrybucji i skalowanie w poziomie możliwości fundament bazy danych Azure rozwiązania Cosmos. 

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]Można też [bezpłatnie spróbuj Azure DB rozwiązania Cosmos](https://azure.microsoft.com/try/cosmosdb/) bez subskrypcji platformy Azure, bez opłat ani zobowiązań.

Dostęp do interfejsu API Azure rozwiązania Cosmos DB Cassandra program w wersji zapoznawczej. Jeśli nie zostały zastosowane do dostępu jeszcze [Zamów teraz](cassandra-introduction.md#sign-up-now).

Ponadto:
* [Node.js](https://nodejs.org/en/) w wersji 0.10.29 lub nowszej
* [Git](http://git-scm.com/)

## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

Przed utworzeniem bazy danych dokumentów, musisz utworzyć konto Cassandra z bazy danych Azure rozwiązania Cosmos.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>Klonowanie przykładowej aplikacji

Teraz załóżmy aplikacji w klonowania Cassandra API z serwisu github, Ustaw ciąg połączenia i uruchom go. Zobaczysz, jak łatwo jest pracować programowo z danymi. 

1. Otwórz okno terminala git, np. git bash i użyj `cd` polecenie, aby przejść do folderu instalacji aplikacji przykładowej. 

    ```bash
    cd "C:\git-samples"
    ```

2. Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium. To polecenie tworzy kopię przykładowej aplikacji na komputerze.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-nodejs-getting-started.git
    ```

## <a name="review-the-code"></a>Przeglądanie kodu

Ten krok jest opcjonalny. Jeśli chcesz się dowiedzieć, jak zasoby bazy danych są tworzone w kodzie, można przejrzeć poniższe fragmenty kodu. Fragmenty kodu są pobierane z `uprofile.js` pliku w folderze C:\git-samples\azure-cosmos-db-cassandra-nodejs-getting-started. W przeciwnym razie możesz przejść od razu do [zaktualizować parametry połączenia](#update-your-connection-string). 

* Nazwa użytkownika i hasła jest ustawiona, za pomocą strony ciągu połączenia w portalu Azure. Path\to\cert zawiera ścieżkę do X509 certyfikatu. 

   ```nodejs
   var ssl_option = {
        cert : fs.readFileSync("path\to\cert"),
        rejectUnauthorized : true,
        secureProtocol: 'TLSv1_2_method'
        };
   const authProviderLocalCassandra = new cassandra.auth.PlainTextAuthProvider(config.username, config.password);
   ```

* `client` Jest inicjowany z contactPoint informacji. ContactPoint są pobierane z portalu Azure.

    ```nodejs
    const client = new cassandra.Client({contactPoints: [config.contactPoint], authProvider: authProviderLocalCassandra, sslOptions:ssl_option});
    ```

* `client` Łączy do interfejsu API Azure rozwiązania Cosmos DB Cassandra.

    ```nodejs
    client.connect(next);
    ```

* Tworzony jest nowy przestrzeni kluczy.

    ```nodejs
    function createKeyspace(next) {
        var query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {\'class\': \'NetworkTopologyStrategy\', \'datacenter1\' : \'1\' }";
        client.execute(query, next);
        console.log("created keyspace");    
  }
    ```

* Nowa tabela została utworzona.

   ```nodejs
   function createTable(next) {
    var query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)";
        client.execute(query, next);
        console.log("created table");
   },
   ```

* Dodaje się klucza i wartości jednostki.

    ```nodejs
    ...
       {
          query: 'INSERT INTO  uprofile.user  (user_id, user_name , user_bcity) VALUES (?,?,?)',
          params: [5, 'IvanaV', 'Belgaum', '2017-10-3136']
        }
    ];
    client.batch(queries, { prepare: true}, next);
    ```

* Zapytanie, aby uzyskać Pobierz wszystkie wartości klucza.

    ```nodejs
   var query = 'SELECT * FROM uprofile.user';
    client.execute(query, { prepare: true}, function (err, result) {
      if (err) return next(err);
      result.rows.forEach(function(row) {
        console.log('Obtained row: %d | %s | %s ',row.user_id, row.user_name, row.user_bcity);
      }, this);
      next();
    });
    ```  
    
* Zapytanie, aby uzyskać klucz wartość.

    ```nodejs
    function selectById(next) {
        console.log("\Getting by id");
        var query = 'SELECT * FROM uprofile.user where user_id=1';
        client.execute(query, { prepare: true}, function (err, result) {
        if (err) return next(err);
            result.rows.forEach(function(row) {
            console.log('Obtained row: %d | %s | %s ',row.user_id, row.user_name, row.user_bcity);
        }, this);
        next();
        });
    }
    ```  

## <a name="update-your-connection-string"></a>Aktualizowanie parametrów połączenia

Teraz wróć do witryny Azure Portal, aby uzyskać informacje o parametrach połączenia i skopiować je do aplikacji. Dzięki temu aplikacja do komunikacji z bazą danych hostowanej.

1. W [portalu Azure](http://portal.azure.com/), kliknij przycisk **ciąg połączenia**. 

    Użyj ![Przycisk Kopiuj](./media/create-cassandra-nodejs/copy.png) przycisk po prawej stronie ekranu, aby skopiować wartość górnego punktu kontaktu.

    ![Wyświetlanie i kopiowanie punkt kontaktu, nazwę użytkownika i hasło z Azure strony portalu, ciąg połączenia](./media/create-cassandra-nodejs/keys.png)

2. Otwórz plik `config.js`. 

3. Wklej wartość punktu kontaktu z portalu za pośrednictwem `<FillMEIN>` w wierszu 4.

    4. wiersz powinien teraz wyglądać podobnie do 

    `config.contactPoint = "cosmos-db-quickstarts.documents.azure.com:10350"`

4. Skopiuj wartości nazwy użytkownika z portalu i wklej go za pośrednictwem `<FillMEIN>` na wiersz 2.

    2. wiersz powinien teraz wyglądać podobnie do 

    `config.username = 'cosmos-db-quickstart';`
    
5. Skopiuj wartość HASŁA z portalu i wklej go za pośrednictwem `<FillMEIN>` w wierszu 3.

    Wiersz 3 powinna wyglądać podobnie do

    `config.password = '2Ggkr662ifxz2Mg==';`

6. Zapisywanie pliku config.js.
    
## <a name="use-the-x509-certificate"></a>Użyj X509 certyfikatu 

1. Jeśli konieczne jest dodanie Baltimore CyberTrust Root, ma numer seryjny 02:00:00:b9 i c d4🇩🇪20:d0:5e:66:fc:53:fe:1a:50:88:2 odcisk palca SHA1: 78:db:28:52:ca:e4:74. Można go pobrać z https://cacert.omniroot.com/bc2025.crt zapisywane w lokalnym pliku z rozszerzeniem cer. 

2. Otwórz uprofile.js i zmień "path\to\cert" aby wskazywały nowy certyfikat. 

3. Zapisz uprofile.js. 

## <a name="run-the-app"></a>Uruchomienie aplikacji

1. W oknie terminalu git Uruchom `npm install` zainstalować moduły npm wymagane.

2. Uruchom `node uprofile.js` można uruchomić aplikacji węzła.

3. Sprawdź wyniki, zgodnie z oczekiwaniami w wierszu polecenia.

    ![Wyświetlanie i sprawdź dane wyjściowe](./media/create-cassandra-nodejs/output.png)

    Naciśnij klawisze CTRL + C, aby zatrzymać exection program i zamknąć okno konsoli. 

    Teraz możesz otworzyć Eksploratora danych w portalu Azure, aby wyświetlić zapytania, modyfikować i pracy z tym nowych danych. 

    ![Wyświetlanie danych w Eksploratorze danych](./media/create-cassandra-nodejs/data-explorer.png) 

## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wyjaśniono sposób tworzenia konta usługi Azure Cosmos DB, tworzenia kolekcji za pomocą Eksploratora danych i uruchamiania aplikacji. Teraz możesz zaimportować dodatkowe dane do swojego konta usługi Cosmos DB. 

> [!div class="nextstepaction"]
> [Importowanie danych Cassandra do bazy danych Azure rozwiązania Cosmos](cassandra-import-data.md)


