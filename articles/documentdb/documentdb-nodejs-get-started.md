<properties
  pageTitle="Samouczek NoSQL środowiska Node.js dla usługi DocumentDB | Microsoft Azure"
  description="Samouczek NoSQL środowiska Node.js, który pokazuje tworzenie bazy danych Node i aplikacji konsolowej przy użyciu zestawu SDK dla środowiska Node.js usługi DocumentDB. Usługa DocumentDB jest bazą danych NoSQL dla formatu JSON."
    keywords="samouczek node.js, baza danych node"
  services="documentdb"
  documentationCenter="node.js"
  authors="AndrewHoh"
  manager="jhubbard"
  editor="monicar"/>

<tags
  ms.service="documentdb"
  ms.workload="data-services"
  ms.tgt_pltfrm="na"
  ms.devlang="node"
  ms.topic="hero-article"
  ms.date="08/11/2016"
  ms.author="anhoh"/>

# Samouczek NoSQL środowiska Node.js: aplikacja konsolowa Node.js usługi DocumentDB  

> [AZURE.SELECTOR]
- [.NET](documentdb-get-started.md)
- [Node.js](documentdb-nodejs-get-started.md)

Witamy w samouczku środowiska Node.js dla zestawu SDK środowiska Node.js usługi Azure DocumentDB! W ramach tego samouczka zostanie utworzona aplikacja konsolowa, która tworzy zasoby usługi DocumentDB, w tym bazę danych Node i wykonuje względem nich zapytania.

Omówione zostaną następujące czynności:

- Tworzenie konta usługi DocumentDB i nawiązywanie z nim połączenia
- Instalowanie aplikacji
- Tworzenie bazy danych Node
- Tworzenie kolekcji
- Tworzenie dokumentów JSON
- Wykonywanie zapytań względem kolekcji
- Zastępowanie dokumentu
- Usuwanie dokumentu
- Usuwanie bazy danych Node

Nie masz czasu? Nie martw się! Kompletne rozwiązanie jest dostępne w witrynie [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started). Przejdź do sekcji [Pobieranie kompletnego rozwiązania](#GetSolution), aby uzyskać krótkie instrukcje.

Po ukończeniu samouczka środowiska Node.js użyj przycisków głosowania u góry lub u dołu tej strony, aby przesłać nam swoją opinię. Jeśli chcesz, abyśmy skontaktowali się z Tobą bezpośrednio, możesz w komentarzach podać swój adres e-mail.

Teraz do dzieła!

## Wymagania wstępne dla samouczka środowiska Node.js

Upewnij się, że masz:

- Aktywne konto platformy Azure. Jeśli go nie masz, możesz zarejestrować się w celu uzyskania [bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
- [Node.js](https://nodejs.org/) w wersji 0.10.29 lub nowszej.

## Krok 1. Tworzenie konta usługi DocumentDB

Utwórz konto usługi DocumentDB. Jeśli masz już konto, którego chcesz użyć, możesz przejść od razu do kroku [Instalowanie aplikacji Node.js](#SetupNode).

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a id="SetupNode"></a>Krok 2. Instalowanie aplikacji Node.js

1. Otwórz swój ulubiony terminal.
2. Zlokalizuj folder lub katalog, w którym chcesz zapisać aplikację Node.js.
3. Utwórz dwa puste pliki JavaScript za pomocą następujących poleceń:
  - W systemie Windows:
      * ```fsutil file createnew app.js 0```
        * ```fsutil file createnew config.js 0```
  - W systemie Linux/OS X:
      * ```touch app.js```
        * ```touch config.js```
4. Zainstaluj moduł documentdb za pomocą menedżera npm. Użyj następującego polecenia:
    * ```npm install documentdb --save```

Wspaniale! Teraz, po zakończeniu instalacji, zacznijmy pisanie kodu.

## <a id="Config"></a>Krok 3. Ustawianie konfiguracji aplikacji

Otwórz plik ```config.js``` w ulubionym edytorze tekstu.

Następnie skopiuj i wklej poniższy fragment kodu oraz ustaw właściwości ```config.endpoint``` i ```config.primaryKey``` na identyfikator URI punktu końcowego i klucz podstawowy usługi DocumentDB. Obie te konfiguracje można znaleźć w [portalu Azure](https://portal.azure.com).

![Samouczek środowiska Node.js — zrzut ekranu Portalu Azure przedstawiający konto usługi DocumentDB z wyróżnionym AKTYWNYM centrum, przyciskiem KLUCZE wyróżnionym w bloku konta usługi DocumentDB oraz wartościami IDENTYFIKATOR URI, KLUCZ PODSTAWOWY i KLUCZ POMOCNICZY wyróżnionymi w bloku Klucze — baza danych Node][keys]

    // ADD THIS PART TO YOUR CODE
    var config = {}

    config.endpoint = "~your DocumentDB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

Skopiuj i wklej elementy ```database id```, ```collection id``` i ```JSON documents``` do obiektu ```config``` poniżej miejsca, w którym zostały ustawione właściwości ```config.endpoint``` i ```config.authKey```. Jeśli masz już dane, które chcesz przechowywać w bazie danych, możesz użyć [narzędzia migracji danych](documentdb-import-data.md) usługi DocumentDB zamiast dodawać definicje dokumentów.

    config.endpoint = "~your DocumentDB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART TO YOUR CODE
    config.database = {
        "id": "FamilyDB"
    };

    config.collection = {
        "id": "FamilyColl"
    };

    config.documents = {
        "Andersen": {
            "id": "Anderson.1",
            "lastName": "Andersen",
            "parents": [{
                "firstName": "Thomas"
            }, {
                    "firstName": "Mary Kay"
                }],
            "children": [{
                "firstName": "Henriette Thaulow",
                "gender": "female",
                "grade": 5,
                "pets": [{
                    "givenName": "Fluffy"
                }]
            }],
            "address": {
                "state": "WA",
                "county": "King",
                "city": "Seattle"
            }
        },
        "Wakefield": {
            "id": "Wakefield.7",
            "parents": [{
                "familyName": "Wakefield",
                "firstName": "Robin"
            }, {
                    "familyName": "Miller",
                    "firstName": "Ben"
                }],
            "children": [{
                "familyName": "Merriam",
                "firstName": "Jesse",
                "gender": "female",
                "grade": 8,
                "pets": [{
                    "givenName": "Goofy"
                }, {
                        "givenName": "Shadow"
                    }]
            }, {
                    "familyName": "Miller",
                    "firstName": "Lisa",
                    "gender": "female",
                    "grade": 1
                }],
            "address": {
                "state": "NY",
                "county": "Manhattan",
                "city": "NY"
            },
            "isRegistered": false
        }
    };


Definicje bazy danych, kolekcji i dokumentów będą pełnić rolę elementów ```database id``` i ```collection id``` oraz danych dokumentów usługi DocumentDB.

Na koniec wyeksportuj obiekt ```config```, aby można było odwoływać się do niego w pliku ```app.js```.

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART TO YOUR CODE
    module.exports = config;

##<a id="Connect"></a> Krok 4. Łączenie z kontem usługi DocumentDB

Otwórz pusty plik ```app.js``` w edytorze tekstu. Skopiuj i wklej kod poniżej, aby zaimportować moduł ```documentdb``` i nowo utworzony moduł ```config```.

    // ADD THIS PART TO YOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    var config = require("./config");
    var url = require('url');

Skopiuj i wklej kod, aby użyć wcześniej zapisanych właściwości ```config.endpoint``` i ```config.primaryKey``` do utworzenia nowego wystąpienia klasy DocumentClient.

    var config = require("./config");
    var url = require('url');

    // ADD THIS PART TO YOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

Teraz, gdy masz kod do zainicjowania klienta usługi DocumentDB, przyjrzyjmy się pracy z zasobami usługi DocumentDB.

## Krok 5. Tworzenie bazy danych Node
Skopiuj i wklej kod poniżej, aby ustawić stan HTTP, jeśli dane nie zostaną znalezione, adres URL bazy danych i adres URL kolekcji. Te adresy URL określają sposób znajdowania właściwej bazy danych i kolekcji przez klienta usługi DocumentDB.

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART TO YOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseUrl = `dbs/${config.database.id}`;
    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

[Bazę danych](documentdb-resources.md#databases) można utworzyć za pomocą funkcji [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) klasy **DocumentClient**. Baza danych jest kontenerem logicznym magazynu dokumentów podzielonym na partycje w kolekcjach.

Skopiuj i wklej funkcję **getDatabase** w celu utworzenia nowej bazy danych w pliku app.js z właściwością ```id``` określoną w obiekcie ```config```. Funkcja sprawdzi, czy istnieje baza danych o takim samym identyfikatorze ```FamilyRegistry```. Jeśli istnieje, zostanie zwrócona ta baza danych zamiast tworzenia nowej.

    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

    // ADD THIS PART TO YOUR CODE
    function getDatabase() {
        console.log(`Getting database:\n${config.database.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDatabase(databaseUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDatabase(config.database, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Skopiuj i wklej kod poniżej miejsca ustawienia funkcji **getDatabase**, aby dodać funkcję pomocnika **exit**, która będzie drukować komunikat zakończenia i wywołanie funkcji **getDatabase**.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key to exit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    }

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

W terminalu znajdź swój plik ```app.js```, a następnie uruchom polecenie: ```node app.js```

Gratulacje! Pomyślnie utworzono bazę danych usługi DocumentDB.

##<a id="CreateColl"></a>Krok 6. Tworzenie kolekcji  

> [AZURE.WARNING] Metoda **CreateDocumentCollectionAsync** utworzy nową kolekcję, co ma implikacje cenowe. Aby uzyskać więcej informacji, odwiedź naszą [stronę cennika](https://azure.microsoft.com/pricing/details/documentdb/).

[Kolekcję](documentdb-resources.md#collections) można utworzyć za pomocą funkcji [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) klasy **DocumentClient**. Kolekcja jest kontenerem dokumentów JSON i skojarzonej logiki aplikacji JavaScript.

Skopiuj i wklej funkcję **getCollection** poniżej funkcji **getDatabase** w celu utworzenia nowej kolekcji z właściwością ```id``` określoną w obiekcie ```config```. Znowu sprawdzimy w celu upewnienia się, że kolekcja o takim samym identyfikatorze ```FamilyCollection``` jeszcze nie istnieje. Jeśli istnieje, zostanie zwrócona ta kolekcja zamiast tworzenia nowej.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function getCollection() {
        console.log(`Getting collection:\n${config.collection.id}\n`);

        return new Promise((resolve, reject) => {
            client.readCollection(collectionUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Skopiuj i wklej kod poniżej wywołania funkcji **getDatabase**, aby wykonać funkcję **getCollection**.

    getDatabase()

    // ADD THIS PART TO YOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

W terminalu znajdź swój plik ```app.js```, a następnie uruchom polecenie: ```node app.js```

Gratulacje! Pomyślnie utworzono kolekcję usługi DocumentDB.

##<a id="CreateDoc"></a>Krok 7. Tworzenie dokumentu
[Dokument](documentdb-resources.md#documents) można utworzyć za pomocą funkcji [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) klasy **DocumentClient**. Dokumenty są zawartością JSON zdefiniowaną przez użytkownika (dowolną). Teraz można wstawić dokument do usługi DocumentDB.

Skopiuj i wklej funkcję **getFamilyDocument** poniżej funkcji **getCollection** w celu utworzenia dokumentów zawierających dane JSON zapisane w obiekcie ```config```. Znowu sprawdzimy w celu upewnienia się, że dokument o takim samym identyfikatorze jeszcze nie istnieje.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function getFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Getting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDocument(documentUrl, { partitionKey: document.district }, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDocument(collectionUrl, document, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

Skopiuj i wklej kod poniżej wywołania funkcji **getCollection**, aby wykonać funkcję **getFamilyDocument**.

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

W terminalu znajdź swój plik ```app.js```, a następnie uruchom polecenie: ```node app.js```

Gratulacje! Pomyślnie utworzono dokumenty usługi DocumentDB.

![Samouczek środowiska Node.js — diagram pokazujący hierarchiczną relację między kontem, bazą danych, kolekcją i dokumentami — baza danych Node](./media/documentdb-nodejs-get-started/node-js-tutorial-account-database.png)

##<a id="Query"></a>Krok 8. Wykonywanie zapytań względem zasobów usługi DocumentDB

Usługa DocumentDB obsługuje [zaawansowane zapytania](documentdb-sql-query.md) względem dokumentów JSON przechowywanych w każdej kolekcji. Następujący przykładowy kod przedstawia zapytanie, które można uruchomić dla dokumentów w kolekcji.

Skopiuj i wklej funkcję **queryCollection** poniżej funkcji **getFamilyDocument**. Usługa DocumentDB obsługuje zapytania podobne do zapytań SQL, jak pokazano poniżej. Aby uzyskać więcej informacji na temat tworzenia złożonych zapytań, zobacz [plac zabaw dla zapytań](https://www.documentdb.com/sql/demo) i [dokumentację dotyczącą zapytań](documentdb-sql-query.md).

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function queryCollection() {
        console.log(`Querying collection through index:\n${config.collection.id}`);

        return new Promise((resolve, reject) => {
            client.queryDocuments(
                collectionUrl,
                'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
            ).toArray((err, results) => {
                if (err) reject(err)
                else {
                    for (var queryResult of results) {
                        let resultString = JSON.stringify(queryResult);
                        console.log(`\tQuery returned ${resultString}`);
                    }
                    console.log();
                    resolve(results);
                }
            });
        });
    };


Na poniższym diagramie przedstawiono sposób, w jaki składnia zapytania SQL usługi DocumentDB jest wywoływana względem utworzonej kolekcji.

![Samouczek środowiska Node.js — diagram pokazujący zakres i znaczenie zapytania — baza danych Node](./media/documentdb-nodejs-get-started/node-js-tutorial-collection-documents.png)

Słowo kluczowe [FROM](documentdb-sql-query.md#from-clause) jest opcjonalne w zapytaniu, ponieważ zapytania usługi DocumentDB mają już zakres określony jako jedna kolekcja. W związku z tym klauzula "FROM Families f" może być zamieniona na "FROM root r" lub dowolną inną wybraną nazwę zmiennej. Usługa DocumentDB będzie wnioskowała, że zmienne Families, root lub wybrana nazwa zmiennej domyślnie odwołują się do bieżącej kolekcji.

Skopiuj i wklej kod poniżej wywołania funkcji **getFamilyDocument**, aby wykonać funkcję **queryCollection**.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART TO YOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

W terminalu znajdź swój plik ```app.js```, a następnie uruchom polecenie: ```node app.js```

Gratulacje! Pomyślnie wykonano zapytanie dla dokumentów usługi DocumentDB.

##<a id="ReplaceDocument"></a>Krok 9. Zastępowanie dokumentu
Usługa DocumentDB obsługuje zastępowanie dokumentów JSON.

Skopiuj i wklej funkcję **replaceDocument** poniżej funkcji **queryCollection**.

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function replaceFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Replacing document:\n${document.id}\n`);
        document.children[0].grade = 6;

        return new Promise((resolve, reject) => {
            client.replaceDocument(documentUrl, document, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Skopiuj i wklej kod poniżej wywołania funkcji **queryCollection**, aby wykonać funkcję **replaceDocument**. Ponadto dodaj kod w celu ponownego wywołania funkcji **queryCollection**, aby sprawdzić, czy dokument został pomyślnie zmieniony.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

W terminalu znajdź swój plik ```app.js```, a następnie uruchom polecenie: ```node app.js```

Gratulacje! Pomyślnie zastąpiono dokument usługi DocumentDB.

##<a id="DeleteDocument"></a>Krok 10. Usuwanie dokumentu
Usługa DocumentDB obsługuje usuwanie dokumentów JSON.

Skopiuj i wklej funkcję **deleteDocument** poniżej funkcji **replaceDocument**.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function deleteFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Deleting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.deleteDocument(documentUrl, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Skopiuj i wklej kod poniżej wywołania drugiej funkcji **queryCollection**, aby wykonać funkcję **deleteDocument**.

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

W terminalu znajdź swój plik ```app.js```, a następnie uruchom polecenie: ```node app.js```

Gratulacje! Pomyślnie usunięto dokument usługi DocumentDB.

##<a id="DeleteDatabase"></a>Krok 11. Usuwanie bazy danych Node

Usunięcie utworzonej bazy danych spowoduje usunięcie bazy danych i wszystkich zasobów podrzędnych (kolekcji, dokumentów itd.).

Skopiuj i wklej poniższy fragment kodu (funkcja **cleanup**), aby usunąć bazę danych i wszystkie zasoby podrzędne.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${config.database.id}`);

        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    }

Skopiuj i wklej kod poniżej wywołania funkcji **deleteDocument**, aby wykonać funkcję **cleanup**.

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART TO YOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

##<a id="Run"></a>Krok 12. Uruchamianie całej aplikacji Node.js

Cała sekwencja wywoływania funkcji powinna wyglądać następująco:

    getDatabase()
    .then(() => getCollection())
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

W terminalu znajdź swój plik ```app.js```, a następnie uruchom polecenie: ```node app.js```

Powinny zostać wyświetlone dane wyjściowe aplikacji rozpoczynania pracy. Dane wyjściowe powinny odpowiadać tekstowi przykładu poniżej.

    Getting database:
    FamilyDB

    Getting collection:
    FamilyColl

    Getting document:
    Anderson.1

    Getting document:
    Wakefield.7

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing document:
    Anderson.1

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleting document:
    Anderson.1

    Cleaning up by deleting database FamilyDB
    Completed successfully
    Press any key to exit

Gratulacje! Udało Ci się ukończyć samouczek środowiska Node.js i utworzyć swoją pierwszą aplikację konsolową usługi DocumentDB!

## <a id="GetSolution"></a>Pobieranie kompletnego rozwiązania samouczka środowiska Node.js
Do utworzenia rozwiązania GetStarted, które zawiera wszystkie przykłady w tym artykule, będą potrzebne następujące elementy:

-   [Konto usługi DocumentDB][documentdb-create-account].
-   Rozwiązanie [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) dostępne w witrynie GitHub.

Zainstaluj moduł **documentdb** za pomocą menedżera npm. Użyj następującego polecenia:
* ```npm install documentdb --save```

Następnie w pliku ```config.js``` zaktualizuj wartości config.endpoint i config.authKey, zgodnie z opisem w sekcji [Krok 3. Ustawianie konfiguracji aplikacji](#Config).

## Następne kroki

-   Potrzebujesz bardziej złożonego przykładu środowiska Node.js? Zobacz [Tworzenie aplikacji sieci Web Node.js za pomocą usługi DocumentDB](documentdb-nodejs-application.md).
-  Dowiedz się, jak [monitorować konto usługi DocumentDB](documentdb-monitor-accounts.md).
-  Uruchom zapytania względem naszego przykładowego zestawu danych na [placu zabaw dla zapytań](https://www.documentdb.com/sql/demo).
-  Dowiedz się więcej o modelu programowania w sekcji Dla deweloperów [strony dokumentacji usługi DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/).

[documentdb-create-account]: documentdb-create-account.md
[documentdb-manage]: documentdb-manage.md

[keys]: media/documentdb-nodejs-get-started/node-js-tutorial-keys.png



<!--HONumber=sep16_HO1-->


