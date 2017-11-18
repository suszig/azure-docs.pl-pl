---
title: "Szybki Start: Cassandra interfejsu API z platformą .NET — rozwiązania Cosmos Azure DB | Dokumentacja firmy Microsoft"
description: "Ta opcja szybkiego startu przedstawia sposób użycia interfejsu API Azure rozwiązania Cosmos DB Cassandra do tworzenia profilu aplikacji z portalu Azure i .NET"
services: cosmos-db
author: mimig1
manager: jhubbard
documentationcenter: 
ms.assetid: 73839abf-5af5-4ae0-a852-0f4159bc00a0
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: mimig
ms.openlocfilehash: ebfe845fa4f695064773a03f6d765da37ab44189
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="quickstart-build-a-cassandra-app-with-net-and-azure-cosmos-db"></a>Szybki Start: Tworzenie aplikacji Cassandra .NET i Azure rozwiązania Cosmos bazy danych

Ta opcja szybkiego startu przedstawia sposób użycia usług .NET i Azure DB rozwiązania Cosmos [Cassandra API](cassandra-introduction.md) do tworzenia aplikacji profil w klonowania przykład z usługi GitHub. Ta opcja szybkiego startu również przeprowadzi Cię przez tworzenie konta bazy danych rozwiązania Cosmos Azure przy użyciu portalu Azure opartej na sieci web.   

Azure DB rozwiązania Cosmos jest usługa globalnie rozproszone wielu modelu bazy danych firmy Microsoft. Można szybko tworzyć i kwerend dokumentu, tabeli, klucz wartość i baz danych wykresu, które korzystają z globalnego dystrybucji i skalowanie w poziomie możliwości fundament bazy danych Azure rozwiązania Cosmos. 

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]Można też [bezpłatnie spróbuj Azure DB rozwiązania Cosmos](https://azure.microsoft.com/try/cosmosdb/) bez subskrypcji platformy Azure, bez opłat ani zobowiązań.

Dostęp do interfejsu API Azure rozwiązania Cosmos DB Cassandra program w wersji zapoznawczej. Jeśli nie zostały zastosowane do dostępu jeszcze [Zamów teraz](cassandra-introduction.md#sign-up-now).

Ponadto: 
* Jeśli nie masz jeszcze programu Visual Studio 2017 r zainstalowany, możesz pobrać i użyć **wolnego** [programu Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Podczas instalacji programu Visual Studio upewnij się, że jest włączona opcja **Programowanie na platformie Azure**.
* Zainstaluj [Git](https://www.git-scm.com/) sklonować w przykładzie.

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]


## <a name="clone-the-sample-application"></a>Klonowanie przykładowej aplikacji

Teraz przejdźmy do pracy z kodem. Załóżmy sklonować aplikacji interfejsu API Cassandra w witrynie GitHub, Ustaw ciąg połączenia i uruchom go. Zobaczysz, jak łatwo jest pracować programowo z danymi. 

1. Otwórz okno terminala git, np. git bash i użyj `cd` polecenie, aby przejść do folderu instalacji aplikacji przykładowej. 

    ```bash
    cd "C:\git-samples"
    ```

2. Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium. To polecenie tworzy kopię przykładowej aplikacji na komputerze.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-dotnet-getting-started.git
    ```

3. Następnie otwórz plik rozwiązania CassandraQuickStartSample w programie Visual Studio. 

## <a name="review-the-code"></a>Przeglądanie kodu

Ten krok jest opcjonalny. Jeśli chcesz się dowiedzieć, jak zasoby bazy danych są tworzone w kodzie, można przejrzeć poniższe fragmenty kodu. Fragmenty kodu są pobierane z `Program.cs` plik w folderze C:\git-samples\azure-cosmos-db-cassandra-dotnet-getting-started\CassandraQuickStartSample. W przeciwnym razie możesz przejść od razu do [zaktualizować parametry połączenia](#update-your-connection-string).

* Inicjowanie sesji przez nawiązanie połączenia punktu końcowego klastra Cassandra. Interfejs API Cassandra dla bazy danych rozwiązania Cosmos Azure obsługuje tylko TLSv1.2. 

  ```csharp
   var options = new Cassandra.SSLOptions(SslProtocols.Tls12, true, ValidateServerCertificate);
   options.SetHostNameResolver((ipAddress) => CassandraContactPoint);
   Cluster cluster = Cluster.Builder().WithCredentials(UserName, Password).WithPort(CassandraPort).AddContactPoint(CassandraContactPoint).WithSSL(options).Build();
   ISession session = cluster.Connect();
   ```

* Utwórz nowy przestrzeni kluczy.

    ```csharp
    session.Execute("CREATE KEYSPACE uprofile WITH REPLICATION = { 'class' : 'NetworkTopologyStrategy', 'datacenter1' : 1 };"); 
    ```

* Utwórz nową tabelę.

   ```csharp
  session.Execute("CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)");
   ```

* Wstaw jednostkami użytkownika przy użyciu obiektu IMapper przy użyciu nowej sesji, z którym łączy się przestrzeni kluczy uprofile.

    ```csharp
    mapper.Insert<User>(new User(1, "LyubovK", "Dubai"));
    ```
    
* Zapytania w celu uzyskania informacji o wszystkich użytkownikach.

    ```csharp
   foreach (User user in mapper.Fetch<User>("Select * from user"))
   {
      Console.WriteLine(user);
   }
    ```
    
 * Zapytanie, aby uzyskać informacje dotyczące jednego użytkownika.

    ```csharp
    mapper.FirstOrDefault<User>("Select * from user where user_id = ?", 3);
    ```

## <a name="update-your-connection-string"></a>Aktualizowanie parametrów połączenia

Teraz wróć do witryny Azure Portal, aby uzyskać informacje o parametrach połączenia i skopiować je do aplikacji. Informacje o parametrach połączenia umożliwia aplikacji komunikację z bazą danych hostowanej.

1. W [portalu Azure](http://portal.azure.com/), kliknij przycisk **ciąg połączenia**. 

    Użyj ![Przycisk Kopiuj](./media/create-cassandra-dotnet/copy.png) przycisk po prawej stronie ekranu, aby skopiować wartości nazwy użytkownika.

    ![Wyświetlanie i kopiowanie kluczy dostępu w Azure strony portalu, ciąg połączenia](./media/create-cassandra-dotnet/keys.png)

2. W programie Visual Studio 2017 r Otwórz plik Program.cs. 

3. Wklej wartości nazwy użytkownika z portalu za pośrednictwem `<FILLME>` wierszu 13.

    Wiersz 13 Program.cs powinna wyglądać podobnie do 

    `private const string UserName = "cosmos-db-quickstart";`

3. Wróć do portalu i skopiuj wartość HASŁA. Wklej wartość HASŁA z portalu za pośrednictwem `<FILLME>` w wierszu 14.

    Wiersz 14 Program.cs powinna wyglądać podobnie do 

    `private const string Password = "2Ggkr662ifxz2Mg...==";`

4. Wróć do portalu i skopiuj wartość punktu kontaktu. Wklej wartość punktu kontaktu z portalu za pośrednictwem `<FILLME>` w wierszu 15.

    Wiersz 15 Program.cs powinna wyglądać podobnie do 

    `private const string CassandraContactPoint = "cosmos-db-quickstarts.documents.azure.com"; //  DnsName`

5. Zapisz plik Program.cs.
    
## <a name="run-the-app"></a>Uruchomienie aplikacji

1. W programie Visual Studio, kliknij przycisk **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.

2. W wierszu polecenia użyj następującego polecenia można zainstalować pakietu NuGet sterownika .NET. 

    ```cmd
    Install-Package CassandraCSharpDriver
    ```
3. Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Aplikacja wyświetla w oknie konsoli. 

    ![Wyświetlanie i sprawdź dane wyjściowe](./media/create-cassandra-dotnet/output.png)

    Naciśnij klawisze CTRL + C, aby zatrzymać exection program i zamknąć okno konsoli. 
    
    Teraz możesz otworzyć Eksploratora danych w portalu Azure, aby wyświetlić zapytania, modyfikować i pracy z tym nowych danych. 

    ![Wyświetlanie danych w Eksploratorze danych](./media/create-cassandra-dotnet/data-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wyjaśniono sposób tworzenia konta usługi Azure Cosmos DB, tworzenia kolekcji za pomocą Eksploratora danych i uruchamiania aplikacji sieci Web. Teraz możesz zaimportować dodatkowe dane do swojego konta usługi Cosmos DB. 

> [!div class="nextstepaction"]
> [Importowanie danych Cassandra do bazy danych Azure rozwiązania Cosmos](cassandra-import-data.md)
