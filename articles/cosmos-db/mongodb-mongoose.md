---
title: "Przy użyciu wtyczki Mongoose framework z bazy danych Azure rozwiązania Cosmos | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak połączyć aplikację Node.js Mongoose do bazy danych Azure rozwiązania Cosmos"
services: cosmos-db
documentationcenter: 
author: romitgirdhar
manager: jhubbard
editor: 
ms.assetid: de5eea58-ee7c-4609-b1c9-4af3e61a5883
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: rogirdh
ms.openlocfilehash: 9878936b5dd76730633dec16b1c3a3eaac78e95a
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="azure-cosmos-db-using-the-mongoose-framework-with-azure-cosmos-db"></a>Azure rozwiązania Cosmos bazy danych: Za pomocą środowiska Mongoose z bazy danych Azure rozwiązania Cosmos

W tym samouczku przedstawiono sposób użycia [Mongoose Framework](http://mongoosejs.com/) podczas zapisywania danych w usłudze Azure DB rozwiązania Cosmos. Firma Microsoft za pomocą interfejsu API bazy danych MongoDB dla bazy danych Azure rozwiązania Cosmos w ramach tego przewodnika. Dla osób, które nieznane Mongoose to struktura modelowania obiektu bazy danych mongodb w środowisku Node.js i rozwiązaniem jest proste, na podstawie schematu do modelu danych aplikacji.

Azure Cosmos DB to rozproszona globalnie, wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, możesz szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość oraz grafów.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

[Node.js](https://nodejs.org/) w wersji 0.10.29 lub nowszej.

## <a name="create-an-azure-cosmos-db-account"></a>Tworzenie konta usługi Azure Cosmos DB

Utwórzmy konto usługi Azure Cosmos DB. Jeśli masz już konto, którego chcesz użyć, możesz przejść od razu do [skonfigurować aplikację Node.js](#SetupNode). Jeśli używasz emulatora usługi Azure rozwiązania Cosmos bazy danych, wykonaj kroki opisane w temacie [Azure rozwiązania Cosmos DB emulatora](local-emulator.md) emulatora i przejść od razu do [skonfigurować aplikację Node.js](#SetupNode).

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="set-up-your-nodejs-application"></a>Konfigurowanie aplikacji Node.js

>[!Note]
> Jeśli chcesz tylko wskazówki przykładowy kod zamiast instalacji aplikacji, klonowanie [próbki](https://github.com/Azure-Samples/Mongoose_CosmosDB) używane na potrzeby tego samouczka i skompilować aplikację Node.js Mongoose dla bazy danych Azure rozwiązania Cosmos.

1. Aby utworzyć aplikację Node.js w wybranego folderu, uruchom następujące polecenie w wierszu polecenia węzła.

    ```npm init```

    Należy odpowiedzieć na pytania i projektu będą gotowe.

1. Dodaj nowy plik do folderu i nadaj mu nazwę ```index.js```.
1. Instalowanie wymaganych pakietów przy użyciu jednej z ```npm install``` opcje:
    * Mongoose:```npm install mongoose --save```
    * Dotenv (Jeśli chcesz załadować z pliku .env tajnych):```npm install dotenv --save```
    
    >[!Note]
    > ```--save``` Flaga dodaje zależność w pliku package.json.

1. Importuj zależności w pliku index.js.
    ```JavaScript
    var mongoose = require('mongoose');
    var env = require('dotenv').load();    //Use the .env file to load the variables
    ```

1. Dodaj parametry połączenia bazy danych rozwiązania Cosmos i nazwa bazy danych rozwiązania Cosmos do ```.env``` pliku.

    ```JavaScript
    COSMOSDB_CONNSTR={Your MongoDB Connection String Here}
    COSMOSDB_DBNAME={Your DB Name Here}
    ```

1. Łączenie do bazy danych rozwiązania Cosmos Azure przy użyciu wtyczki Mongoose framework przez dodanie poniższego kodu do końca index.js.
    ```JavaScript
    mongoose.connect(process.env.COSMOSDB_CONNSTR+process.env.COSMOSDB_DBNAME+"?ssl=true&replicaSet=globaldb"); //Creates a new DB, if it doesn't already exist

    var db = mongoose.connection;
    db.on('error', console.error.bind(console, 'connection error:'));
    db.once('open', function() {
    console.log("Connected to DB");
    });
    ```
    >[!Note]
    > W tym miejscu zmienne środowiskowe są ładowane przy użyciu process.env. {variableName} przy użyciu pakietu npm "dotenv".

    Po nawiązaniu połączenia do bazy danych Azure rozwiązania Cosmos w Mongoose można teraz rozpocząć konfigurowanie modeli obiektów.
    
## <a name="caveats-to-using-mongoose-with-azure-cosmos-db"></a>Zastrzeżeniami dotyczącymi przy użyciu wtyczki Mongoose w usłudze Azure DB rozwiązania Cosmos

Dla każdego utworzonego modelu Mongoose tworzy nową kolekcję bazy danych MongoDB poniżej obejmuje. Jednak podane modelu rozliczeń dla kolekcji z bazy danych Azure rozwiązania Cosmos, może nie być najbardziej ekonomiczny sposób przejść, jeśli masz wiele modeli obiektów, które są wypełniane słabo.

Ten przewodnik dotyczy obu modeli. Najpierw zostanie omówiony wskazówki dotyczące przechowywania jeden typ danych w jednej kolekcji. Jest to zachowanie defacto Mongoose.

Mongoose ma również pojęcie o nazwie [Discriminators](http://mongoosejs.com/docs/discriminators.html). Discriminators to mechanizm dziedziczenia schematu. Umożliwiają one mieć wiele modeli z nakładającymi się schematy oparte na tej samej kolekcji źródłowej bazy danych MongoDB.

Można przechowywać różne modele danych w tej samej kolekcji i następnie umożliwia klauzuli filtru podczas przeszukiwania rozwiń tylko dane potrzebne.

### <a name="one-collection-per-object-model"></a>Jedną kolekcję dla modelu obiektów

Domyślnym zachowaniem Mongoose jest utworzyć kolekcję bazy danych MongoDB przy tworzeniu modelu obiektów. Ta sekcja opisuje jak to osiągnąć z bazy danych MongoDB dla bazy danych Azure rozwiązania Cosmos. Ta metoda jest zalecana z bazy danych Azure rozwiązania Cosmos w przypadku modeli obiektów z dużych ilości danych. Jest to domyślny działają modelu dla wtyczki Mongoose, tak, można zapoznać się z tym, jeśli znasz Mongoose.

1. Otwórz z ```index.js``` ponownie.

1. Utwórz definicję schematu "Rodzina".

    ```JavaScript
    const Family = mongoose.model('Family', new mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
            gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }));
    ```

1. Utwórz obiekt "Rodziny".

    ```JavaScript
    const family = new Family({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });
    ```

1. Na koniec zapisz obiekt do bazy danych Azure rozwiązania Cosmos. Spowoduje to utworzenie kolekcji poniżej obejmuje.

    ```JavaScript
    family.save((err, saveFamily) => {
        console.log(JSON.stringify(saveFamily));
    });
    ```

1. Teraz Utwórzmy inny schemat i obiektu. Ten czas, możemy utworzyć "Urlopu miejsc docelowych", które mogą być zainteresowani rodzin.
    1. Podobnie jak czas ostatniego Utwórzmy systemu
    ```JavaScript
    const VacationDestinations = mongoose.model('VacationDestinations', new mongoose.Schema({
        name: String,
        country: String
    }));
    ```

    1. Utwórz obiekt próbki (można dodać wiele obiektów do tego schematu) i zapisz go.
    ```JavaScript
    const vacaySpot = new VacationDestinations({
        name: "Honolulu",
        country: "USA"
    });

    vacaySpot.save((err, saveVacay) => {
        console.log(JSON.stringify(saveVacay));
    });
    ```

1. Teraz przechodząc do portalu Azure, można zauważyć dwie kolekcje utworzone w usłudze Azure DB rozwiązania Cosmos.

    ![Samouczek środowiska node.js — zrzut ekranu portalu Azure przedstawiający konto bazy danych Azure rozwiązania Cosmos o nazwie kolekcji wyróżnione — baza danych Node][alldata]

1. Na koniec Przyjrzyjmy się na odczytywanie danych z bazy danych Azure rozwiązania Cosmos. Ponieważ aktualnie używamy modelu operacyjnego Mongoose domyślne odczytami są takie same jak innych odczyty z Mongoose.

    ```JavaScript
    Family.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family: " + JSON.stringify(fam)));
    });
    ```

### <a name="using-mongoose-discriminators-to-store-data-in-a-single-collection"></a>Przy użyciu wtyczki Mongoose discriminators do przechowywania danych w jednej kolekcji

W przypadku tej metody używamy [Mongoose Discriminators](http://mongoosejs.com/docs/discriminators.html) do optymalizowania kosztów każdej kolekcji bazy danych Azure rozwiązania Cosmos. Discriminators umożliwiają definiowanie różnego 'Key', dzięki czemu można przechowywać, rozróżnianie i filtrowanie w modelach inny obiekt.

W tym miejscu utworzymy obiektu podstawowego modelu, zdefiniuj różnego klucz i Dodaj "Rodzina" i "VacationDestinations" jako rozszerzenie do podstawowego modelu.

1. Umożliwia ustawienie konfiguracji podstawowej i zdefiniuj klucza rozróżniacza.

    ```JavaScript
    const baseConfig = {
        discriminatorKey: "_type", //If you've got a lot of different data types, you could also consider setting up a secondary index here.
        collection: "alldata"   //Name of the Common Collection
    };
    ```

1. Następnie zdefiniujmy wspólnego modelu obiektów

    ```JavaScript
    const commonModel = mongoose.model('Common', new mongoose.Schema({}, baseConfig));
    ```

1. Teraz definiujemy modelu "Rodzina". Zauważ, że firma Microsoft korzysta z ```commonModel.discriminator``` zamiast ```mongoose.model```. Ponadto trwa również dodawanie podstawowej konfiguracji mongoose schematu. Dlatego, w tym miejscu discriminatorKey jest ```FamilyType```.

    ```JavaScript
    const Family_common = commonModel.discriminator('FamilyType', new     mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
           gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }, baseConfig));
    ```

1. Podobnie możemy dodać inny schemat czas VacationDestinations. W tym miejscu jest DiscriminatorKey ```VacationDestinationsType```.

    ```JavaScript
    const Vacation_common = commonModel.discriminator('VacationDestinationsType', new mongoose.Schema({
        name: String,
        country: String
    }, baseConfig));
    ```

1. Ponadto umożliwia tworzenie obiektów dla modelu i zapisz go.
    1. Umożliwia dodawanie obiektów do modelu "Rodzina".
    ```JavaScript
    const family_common = new Family_common({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });

    family_common.save((err, saveFamily) => {
        console.log("Saved: " + JSON.stringify(saveFamily));
    });
    ```

    1. Następnie możemy dodać obiekty do modelu "VacationDestinations" i zapisz go.
    ```JavaScript
    const vacay_common = new Vacation_common({
        name: "Honolulu",
        country: "USA"
    });

    vacay_common.save((err, saveVacay) => {
        console.log("Saved: " + JSON.stringify(saveVacay));
    });
    ```

1. Teraz, jeśli możesz powrócić do portalu Azure, można zauważyć, że masz tylko jedną kolekcję o nazwie ```alldata``` z danymi zarówno "Rodzina" i "VacationDestinations".

    ![Samouczek środowiska node.js — zrzut ekranu portalu Azure przedstawiający konto bazy danych Azure rozwiązania Cosmos o nazwie kolekcji wyróżnione — baza danych Node][mutiple-coll]

1. Zauważ również, że każdy obiekt ma inny atrybut o nazwie jako ```__type```, które pomagają rozróżnianie między dwa modele inny obiekt.

1. Na koniec Przyjrzyjmy odczytać dane, które są przechowywane w usłudze Azure DB rozwiązania Cosmos. Mongoose odpowiada on za filtrowanie danych na podstawie modelu. Tak należy podczas odczytu danych nic nie rób inny. Określa model (w tym przypadku ```Family_common```) i filtrowanie DiscriminatorKey uchwytów Mongoose.

    ```JavaScript
    Family_common.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family (using discriminator): " + JSON.stringify(fam)));
    });
    ```

Jak widać, jest proste w użyciu wtyczki Mongoose discriminators. Tak Jeśli masz aplikację, która używa Mongoose framework, w tym samouczku jest sposób do uruchomienia aplikacji i systemem API bazy danych MongoDB dla bazy danych Azure rozwiązania Cosmos bez konieczności zbyt dużej liczby zmian.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się więcej na temat bazy danych MongoDB operacje, operatory, etapy, poleceń i opcji obsługiwanych przez rozwiązania Cosmos bazy danych MongoDB interfejsu API Azure w [obsługę interfejsu API bazy danych MongoDB dla funkcji bazy danych MongoDB i składni](mongodb-feature-support.md).

[alldata]: ./media/mongodb-mongoose/mongo-collections-alldata.png
[mutiple-coll]: ./media/mongodb-mongoose/mongo-mutliple-collections.png