---
title: "Samouczek NoSQL: zestaw Java SDK usługi Azure DocumentDB | Microsoft Docs"
description: "Samouczek NoSQL, który pokazuje tworzenie bazy danych w trybie online i aplikacji konsolowej Java przy użyciu zestawu Java SDK usługi DocumentDB. Usługa Azure DocumentDB jest bazą danych NoSQL dla formatu JSON."
keywords: nosql tutorial, online database, java console application
services: documentdb
documentationcenter: Java
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 75a9efa1-7edd-4fed-9882-c0177274cbb2
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 01/05/2017
ms.author: arramac
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: 74af5fda495adc726bfa85ad48a407fd61d4dd88
ms.lasthandoff: 03/08/2017


---
# <a name="nosql-tutorial-build-a-documentdb-java-console-application"></a>Samouczek NoSQL: tworzenie aplikacji konsolowej w języku Java dla usługi DocumentDB
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js dla MongoDB](documentdb-mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Witamy w samouczku NoSQL dla zestawu Java SDK usługi Azure DocumentDB! W ramach tego samouczka zostanie utworzona aplikacja konsolowa, która tworzy zasoby usługi DocumentDB i wykonuje dla nich zapytania.

Zostaną opisane:

* Tworzenie konta usługi DocumentDB i nawiązywanie z nim połączenia
* Konfigurowanie rozwiązania Visual Studio
* Tworzenie bazy danych w trybie online
* Tworzenie kolekcji
* Tworzenie dokumentów JSON
* Wykonywanie zapytań względem kolekcji
* Tworzenie dokumentów JSON
* Wykonywanie zapytań względem kolekcji
* Zastępowanie dokumentu
* Usuwanie dokumentu
* Usuwanie bazy danych

Teraz do dzieła!

## <a name="prerequisites"></a>Wymagania wstępne
Upewnij się, że masz:

* Aktywne konto platformy Azure. Jeśli go nie masz, możesz zarejestrować się w celu [utworzenia bezpłatnego konta](https://azure.microsoft.com/free/). Na potrzeby tego samouczka możesz także użyć [emulatora usługi Azure DocumentDB](documentdb-nosql-local-emulator.md).
* [Git](https://git-scm.com/downloads)
* [Zestaw Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-a-documentdb-account"></a>Krok 1. Tworzenie konta usługi DocumentDB
Utwórz konto usługi DocumentDB. Jeśli masz już konto, którego chcesz użyć, możesz przejść od razu do kroku [Klonowanie projektu Github](#GitClone). Jeśli używasz emulatora usługi DocumentDB, wykonaj czynności opisane w temacie [Emulator usługi Azure DocumentDB](documentdb-nosql-local-emulator.md), aby skonfigurować emulator, a następnie przejdź do kroku [Klonowanie projektu Github](#GitClone).

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a id="GitClone"></a>Krok 2. Klonowanie projektu Github
Możesz rozpocząć od sklonowania repozytorium Github dla tematu [Rozpoczynanie pracy z bazą danych DocumentDB i językiem Java](https://github.com/Azure-Samples/documentdb-java-getting-started). Na przykład z katalogu lokalnego uruchom następujące polecenie, aby lokalnie pobrać przykładowy projekt.

    git clone git@github.com:Azure-Samples/documentdb-java-getting-started.git

    cd documentdb-java-getting-started

Katalog zawiera plik `pom.xml` dla projektu i folder `src` zawierający kod źródłowy języka Java łącznie z plikiem `Program.java`, który pokazuje sposób wykonywania prostych operacji za pomocą usługi Azure DocumentDB, takich jak tworzenie dokumentów oraz wykonywanie zapytań o dane w ramach kolekcji. Plik `pom.xml` zawiera zależność dla [zestawu Java SDK bazy danych DocumentDB w Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <a id="Connect"></a>Krok 3. Łączenie z kontem usługi DocumentDB
Następnie wróć do witryny [Azure Portal](https://portal.azure.com), aby pobrać punkt końcowy i główny klucz podstawowy. Punkt końcowy i klucz podstawowy usługi DocumentDB są niezbędne, aby aplikacja wiedziała, z jakim elementem ma się połączyć, oraz aby usługa DocumentDB ufała połączeniu aplikacji.

W witrynie Azure Portal przejdź do swojego konta usługi DocumentDB i kliknij przycisk **Klucze**. Skopiuj identyfikator URI z portalu i wklej go w miejsce `<your endpoint URI>` w pliku Program.java. Następnie skopiuj KLUCZ PODSTAWOWY z portalu i wklej go w miejsce `<your key>`.

    this.client = new DocumentClient(
        "<your endpoint URI>",
        "<your key>"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Zrzut ekranu przedstawiający witrynę Azure Portal używaną przez samouczek NoSQL do tworzenia aplikacji konsolowej Java. Przedstawia konto usługi DocumentDB z wyróżnionym AKTYWNYM centrum, przyciskiem KLUCZE wyróżnionym w bloku konta usługi DocumentDB oraz wartościami IDENTYFIKATOR URI, KLUCZ PODSTAWOWY i KLUCZ POMOCNICZY wyróżnionymi w bloku Klucze.][keys]

## <a name="step-4-create-a-database"></a>Krok 4. Tworzenie bazy danych
Własną [bazę danych](documentdb-resources.md#databases) usługi DocumentDB można utworzyć za pomocą metody [createDatabase](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createDatabase-com.microsoft.azure.documentdb.Database-com.microsoft.azure.documentdb.RequestOptions-) klasy **DocumentClient**. Baza danych jest kontenerem logicznym magazynu dokumentów JSON podzielonym na partycje w kolekcjach.

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <a id="CreateColl"></a>Krok 5. Tworzenie kolekcji
> [!WARNING]
> Metoda **createCollection** tworzy nową kolekcję z zarezerwowaną przepływnością, co ma wpływ na cenę. Aby uzyskać więcej informacji, odwiedź naszą [stronę cennika](https://azure.microsoft.com/pricing/details/documentdb/).
> 
> 

[Kolekcję](documentdb-resources.md#collections) można utworzyć za pomocą metody [createCollection](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createCollection-java.lang.String-com.microsoft.azure.documentdb.DocumentCollection-com.microsoft.azure.documentdb.RequestOptions-) klasy **DocumentClient**. Kolekcja jest kontenerem dokumentów JSON i skojarzonej logiki aplikacji JavaScript.


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // DocumentDB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <a id="CreateDoc"></a>Krok 6. Tworzenie dokumentów JSON
[Dokument](documentdb-resources.md#documents) można utworzyć za pomocą metody [createDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createDocument-java.lang.String-java.lang.Object-com.microsoft.azure.documentdb.RequestOptions-boolean-) klasy **DocumentClient**. Dokumenty są zawartością JSON zdefiniowaną przez użytkownika (dowolną). Można teraz wstawić jeden lub więcej dokumentów. Jeśli masz już dane, które chcesz przechowywać w bazie danych, możesz użyć [narzędzia migracji danych](documentdb-import-data.md) usługi DocumentDB, aby zaimportować dane do bazy danych.

    // Insert your Java objects as documents 
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen")

    // More initialization skipped for brevity. You can have nested references
    andersenFamily.setParents(new Parent[] { parent1, parent2 });
    andersenFamily.setDistrict("WA5");
    Address address = new Address();
    address.setCity("Seattle");
    address.setCounty("King");
    address.setState("WA");

    andersenFamily.setAddress(address);
    andersenFamily.setRegistered(true);

    this.client.createDocument("/dbs/familydb/colls/familycoll", family, new RequestOptions(), true);

![Diagram pokazujący hierarchiczną relację między kontem, bazą danych w trybie online, kolekcją i dokumentami używanymi przez samouczek NoSQL do tworzenia aplikacji konsolowej Java](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Krok 7. Wykonywanie zapytań względem zasobów usługi DocumentDB
Usługa DocumentDB obsługuje zaawansowane [zapytania](documentdb-sql-query.md) względem dokumentów JSON przechowywanych w każdej kolekcji.  Następujący przykładowy kod przedstawia sposób wysyłania zapytania w usłudze DocumentDB przy użyciu składni SQL za pomocą metody [queryDocuments](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#queryDocuments-java.lang.String-com.microsoft.azure.documentdb.SqlQuerySpec-com.microsoft.azure.documentdb.FeedOptions-).

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <a id="ReplaceDocument"></a>Krok 8. Zastępowanie dokumentu JSON
Usługa DocumentDB obsługuje aktualizowanie dokumentów JSON za pomocą metody [replaceDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#replaceDocument-com.microsoft.azure.documentdb.Document-com.microsoft.azure.documentdb.RequestOptions-).

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <a id="DeleteDocument"></a>Krok 9. Usuwanie dokumentu JSON
Analogicznie usługa DocumentDB obsługuje usuwanie dokumentów JSON za pomocą metody [deleteDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#deleteDocument-java.lang.String-com.microsoft.azure.documentdb.RequestOptions-).  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <a id="DeleteDatabase"></a>Krok 10. Usuwanie bazy danych
Usunięcie utworzonej bazy danych usunie bazę danych i wszystkie zasoby podrzędne (kolekcje, dokumenty itd.).

    this.client.deleteDatabase("/dbs/familydb", null);

## <a id="Run"></a>Krok 11. Uruchamianie całej aplikacji konsolowej Java!
Aby uruchomić aplikację z konsoli, najpierw należy ją skompilować przy użyciu Maven:
    
    mvn package

Uruchomienie polecenia `mvn package` pobierze najnowszą bibliotekę DocumentDB z Maven i utworzy plik `GetStarted-0.0.1-SNAPSHOT.jar`. Następnie można uruchomić aplikację za pomocą następującego polecenia:

    mvn exec:java -D exec.mainClass=GetStarted.Program

Gratulacje! Pomyślnie ukończono ten samouczek NoSQL i utworzono działającą aplikację konsolową Java!

## <a name="next-steps"></a>Następne kroki
* Czy chcesz samouczek aplikacji sieci Web w języku Java? Zobacz [Build a web application with Java using DocumentDB](documentdb-java-application.md) (Tworzenie aplikacji sieci Web w języku Java przy użyciu usługi DocumentDB).
* Dowiedz się, jak [monitorować konto usługi DocumentDB](documentdb-monitor-accounts.md).
* Uruchom zapytania względem naszego przykładowego zestawu danych na [placu zabaw dla zapytań](https://www.documentdb.com/sql/demo).
* Dowiedz się więcej o modelu programowania w sekcji Dla deweloperów [strony dokumentacji usługi DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/).

[documentdb-create-account]: documentdb-create-account.md
[keys]: media/documentdb-get-started/nosql-tutorial-keys.png

