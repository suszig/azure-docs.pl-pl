---
title: "Nawiązywanie połączeń z usługą Azure Database for PostgreSQL z poziomu środowiska Node.js | Microsoft Docs"
description: "Ten przewodnik Szybki start zawiera przykładowy kod Node.js, którego można używać do nawiązywania połączeń z danymi usługi Azure Database for PostgreSQL i wykonywania zapytań względem nich."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 06/23/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: 6a51fcf4f4494e5b32ccf6dabb19f8d004bb20d4
ms.contentlocale: pl-pl
ms.lasthandoff: 06/26/2017

---

<a id="azure-database-for-postgresql-use-nodejs-to-connect-and-query-data" class="xliff"></a>

# Usługa Azure Database for PostgreSQL: nawiązywanie połączeń z danymi i wykonywanie na nich zapytań za pomocą środowiska Node.js
Ten przewodnik Szybki start przedstawia sposób nawiązywania połączeń z usługą Azure Database for PostgreSQL przy użyciu języka [Node.js](https://nodejs.org/) z platform Windows, Ubuntu Linux i Mac. Pokazano w nim, jak używać instrukcji języka SQL w celu wysyłania zapytań o dane oraz wstawiania, aktualizowania i usuwania danych w bazie danych. W krokach w tym artykule założono, że wiesz już, jak programować za pomocą języka Node.js, i dopiero zaczynasz pracę z usługą Azure Database for PostgreSQL.

<a id="prerequisites" class="xliff"></a>

## Wymagania wstępne
Ten przewodnik Szybki start jako punktu wyjścia używa zasobów utworzonych w jednym z tych przewodników:
- [Tworzenie bazy danych — portal](quickstart-create-server-database-portal.md)
- [Tworzenie bazy danych — interfejs wiersza polecenia](quickstart-create-server-database-azure-cli.md)

Należy również:
- Zainstalować środowisko [Node.js](https://nodejs.org).
- Zainstalować pakiet [pg](https://www.npmjs.com/package/pg). 

<a id="install-nodejs" class="xliff"></a>

## Instalowanie środowiska Node.js 
W zależności od platformy, aby zainstalować środowisko Node.js:

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Wprowadź następujące polecenia, aby zainstalować rozwiązanie **brew**, czyli łatwy w użyciu menedżer pakietów dla systemu Mac OS X i środowiska **Node.js**.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install node
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
Wprowadź następujące polecenia, aby zainstalować środowisko **Node.js** i program **npm**, czyli menedżer pakietów dla środowiska Node.js.

```bash
sudo apt-get install -y nodejs npm
```

<a id="windows" class="xliff"></a>

### **Windows**
Odwiedź [stronę pobierania środowiska Node.js](https://nodejs.org/en/download/) i wybierz odpowiednią opcję Instalatora Windows.

<a id="install-pg-client" class="xliff"></a>

## Instalowanie klienta pg
Zainstaluj program [pg](https://www.npmjs.com/package/pg) będący niepowodującym blokowania klientem w czystym języku JavaScript dla środowiska Node.js, przydatnym do nawiązywania połączeń z programem PostgreSQL i wykonywania w nim zapytań.

W tym celu uruchom narzędzie Node Package Manager (npm) dla języka JavaScript z poziomu wiersza polecenia, aby zainstalować klienta pg.
```bash
npm install pg
```

Zweryfikuj instalację, wyświetlając listę zainstalowanych pakietów.
```bash
npm list
```
Dane wyjściowe polecenia listy potwierdzają wersję każdego składnika. 
```
`-- pg@6.2.3
  +-- buffer-writer@1.0.1
  +-- packet-reader@0.3.1
etc...
```

<a id="get-connection-information" class="xliff"></a>

## Pobieranie informacji o połączeniu
Uzyskaj parametry połączenia potrzebne do nawiązania połączenia z usługą Azure Database for PostgreSQL. Potrzebna jest w pełni kwalifikowana nazwa serwera i poświadczenia logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. W menu po lewej stronie w witrynie Azure Portal kliknij pozycję **Wszystkie zasoby** i wyszukaj właśnie utworzony serwer **mypgserver 20170401**.
3. Kliknij nazwę serwera **mypgserver 20170401**.
4. Wybierz stronę serwera **Przegląd**. Zanotuj wartości **Nazwa serwera** i **Identyfikator logowania administratora serwera**.
 ![Azure Database for PostgreSQL — dane logowania administratora serwera](./media/connect-nodejs/1-connection-string.png)
5. Jeśli nie pamiętasz informacji logowania do serwera, przejdź do strony **Przegląd**, aby wyświetlić nazwę logowania administratora serwera oraz w razie konieczności zresetować hasło.

<a id="running-the-javascript-code-in-nodejs" class="xliff"></a>

## Uruchamianie kodu JavaScript w środowisku Node.js
Środowisko Node.js można uruchomić w wierszu polecenia powłoki bash lub systemu Windows, wpisując polecenie `node`, a następnie uruchomić przykładowy kod JavaScript przez skopiowanie i wklejenie go w wierszu polecenia. Alternatywnie można zapisać kod JavaScript w pliku tekstowym, a następnie uruchomić polecenie `node filename.js` z nazwą pliku jako parametrem uruchamiania.

<a id="connect-create-table-and-insert-data" class="xliff"></a>

## Nawiązywanie połączenia, tworzenie tabeli i wstawianie danych
Użyj poniższego kodu, aby nawiązać połączenie i załadować dane przy użyciu instrukcji **CREATE TABLE** i **INSERT INTO** języka SQL.
Obiekt [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) służy do połączenia interfejsem z serwerem programu PostgreSQL. Funkcja [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) służy do nawiązywania połączenia z serwerem. Funkcja [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) służy do wykonywania zapytania SQL względem bazy danych PostgreSQL. 

Zastąp parametry hosta, nazwy bazy danych, użytkownika i hasła wartościami, które zostały określone podczas tworzenia serwera i bazy danych. 

```javascript
const pg = require('pg');

var config =
{
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: 'mypgsqldb',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(function (err)
{
    if (err)
        throw err;
    else
    {
        queryDatabase();
    }
});

function queryDatabase()
{
    client.query(
        ' \
            DROP TABLE IF EXISTS inventory; \
            CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER); \
            INSERT INTO inventory (name, quantity) VALUES (\'banana\', 150); \
            INSERT INTO inventory (name, quantity) VALUES (\'orange\', 154); \
            INSERT INTO inventory (name, quantity) VALUES (\'apple\', 100); \
        ',
        function (err)
    {
        console.log("Connection established");

        if (err)
            throw err;
        else
        {
            client.end(function (err)
            {
                if (err)
                    throw err;

                // Else closing connection finished without error
                console.log("Closed client connection");
            });

            console.log("Finished execution, exiting now");
            process.exit()
        }
    });
}
```

<a id="read-data" class="xliff"></a>

## Odczyt danych
Użyj poniższego kodu, aby nawiązać połączenie i odczytać dane za pomocą instrukcji **SELECT** języka SQL. Obiekt [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) służy do połączenia interfejsem z serwerem programu PostgreSQL. Funkcja [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) służy do nawiązywania połączenia z serwerem. Funkcja [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) służy do wykonywania zapytania SQL względem bazy danych PostgreSQL. 

Zastąp parametry hosta, nazwy bazy danych, użytkownika i hasła wartościami, które zostały określone podczas tworzenia serwera i bazy danych. 

```javascript
const pg = require('pg');

var config =
{
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: 'mypgsqldb',
    port: 5432,
    ssl: true
};


const client = new pg.Client(config);

client.connect(function (err)
{
    if (err)
        throw err;

    else
    {
        console.log("Connected to Azure Database for PostgreSQL server:" + config.host);
        queryDatabase();
    }
});

function queryDatabase()
{
    // Declare array to hold query result set
    const results = [];

    console.log("Running query to PostgreSQL server:" + config.host);

    // Perform query
    var query = client.query('SELECT * FROM inventory;');

    // Print result set
    query.on('row', function(row)
    {
        console.log("Read " + JSON.stringify(row));
    });

    // Exit program after execution
    query.on('end', function(row)
    {
        console.log("Finished execution, exiting now");
        process.exit()
    });
}
```

<a id="update-data" class="xliff"></a>

## Aktualizowanie danych
Użyj poniższego kodu, aby nawiązać połączenie i odczytać dane za pomocą instrukcji **UPDATE** języka SQL. Obiekt [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) służy do połączenia interfejsem z serwerem programu PostgreSQL. Funkcja [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) służy do nawiązywania połączenia z serwerem. Funkcja [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) służy do wykonywania zapytania SQL względem bazy danych PostgreSQL. 

Zastąp parametry hosta, nazwy bazy danych, użytkownika i hasła wartościami, które zostały określone podczas tworzenia serwera i bazy danych. 

```javascript
const pg = require('pg');

var config =
{
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: 'mypgsqldb',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(function (err)
{
    if (err)
        throw err;
    else
    {
        queryDatabase();
    }   
});

function queryDatabase()
{
    client.query('UPDATE inventory SET quantity= 1000 WHERE name=\'banana\';', function (err, result)
    {
        console.log("Connection established");

        if (err)
            throw err;
        else
        {
            client.end(function (err)
            {
                if (err)
                    throw err;
                
                // Else closing connection finished without error
                console.log("Closed client connection");
            });             
        }

        console.log("Finished execution, exiting now");
        process.exit()
    });
}
```

<a id="delete-data" class="xliff"></a>

## Usuwanie danych
Użyj poniższego kodu, aby nawiązać połączenie i odczytać dane za pomocą instrukcji **DELETE** języka SQL. Obiekt [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) służy do połączenia interfejsem z serwerem programu PostgreSQL. Funkcja [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) służy do nawiązywania połączenia z serwerem. Funkcja [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) służy do wykonywania zapytania SQL względem bazy danych PostgreSQL. 

Zastąp parametry hosta, nazwy bazy danych, użytkownika i hasła wartościami, które zostały określone podczas tworzenia serwera i bazy danych. 

```javascript
const pg = require('pg');

var config =
{
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: 'mypgsqldb',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(function (err)
{
    if (err)
        throw err;
    else
    {
        queryDatabase();
    }   
});

function queryDatabase()
{
    client.query('DELETE FROM inventory WHERE name=\'apple\';', function (err, result)
    {
        console.log("Connection established");
        
        if (err)
            throw err;
        else
        {
            client.end(function (err)
            {
                if (err)
                    throw err;
                
                // Else closing connection finished without error
                console.log("Closed client connection");
            });

            console.log("Finished execution, exiting now");
            process.exit()          
        }
    }); 
}
```

<a id="next-steps" class="xliff"></a>

## Następne kroki
> [!div class="nextstepaction"]
> [Migrowanie bazy danych przy użyciu funkcji eksportowania i importowania](./howto-migrate-using-export-and-import.md)

