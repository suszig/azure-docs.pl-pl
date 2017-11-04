---
title: "Nawiązywanie połączeń z usługą Azure Database for MySQL z poziomu środowiska Node.js | Microsoft Docs"
description: "Ten przewodnik Szybki start zawiera kilka przykładów kodu Node.js, których można używać do nawiązywania połączeń z danymi usługi Azure Database for MySQL i wykonywania zapytań względem nich."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: 2f18016614b229273aa4d661991149be949ce238
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-database-for-mysql-use-nodejs-to-connect-and-query-data"></a>Usługa Azure Database for MySQL: nawiązywanie połączeń z danymi i wykonywanie na nich zapytań za pomocą środowiska Node.js
Ten przewodnik Szybki start przedstawia sposób nawiązywania połączeń z usługą Azure Database for MySQL przy użyciu języka [Node.js](https://nodejs.org/) na platformach Windows, Ubuntu Linux i Mac. Pokazano w nim, jak używać instrukcji języka SQL w celu wysyłania zapytań o dane oraz wstawiania, aktualizowania i usuwania danych w bazie danych. W tym temacie założono, że znasz tworzenie przy użyciu środowiska Node.js i czy masz doświadczenia w pracy z bazą danych Azure dla programu MySQL.

## <a name="prerequisites"></a>Wymagania wstępne
Ten przewodnik Szybki start jako punktu wyjścia używa zasobów utworzonych w jednym z tych przewodników:
- [Tworzenie serwera usługi Azure Database for MySQL za pomocą witryny Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Tworzenie serwera usługi Azure Database for MySQL za pomocą interfejsu wiersza polecenia platformy Azure](./quickstart-create-mysql-server-database-using-azure-cli.md)

Należy również:
- Zainstalować środowisko uruchomieniowe [Node.js](https://nodejs.org).
- Zainstalować pakiet [mysql2](https://www.npmjs.com/package/mysql2) w celu połączenia z programem MySQL z poziomu aplikacji Node.js. 

## <a name="install-nodejs-and-the-mysql-connector"></a>Instalowanie środowiska Node.js i łącznika programu MySQL
W zależności od platformy postępuj zgodnie z instrukcjami w odpowiedniej sekcji, aby zainstalować środowisko Node.js. Użyj programu npm, aby zainstalować pakiet mysql2 i jego zależności w folderze projektu.

### <a name="windows"></a>**Windows**
1. Odwiedź stronę [Node.js pobiera strony](https://nodejs.org/en/download/), a następnie wybierz z odpowiednią opcję Instalatora systemu Windows.
2. Utwórz lokalny folder projektu, taki jak `nodejsmysql`. 
3. Uruchom wiersz polecenia i zmień katalog na folder projektu, takie jak`cd c:\nodejsmysql\`
4. Uruchom narzędzie NPM, aby zainstalować bibliotekę mysql2 w folderze projektu.

   ```cmd
   cd c:\nodejsmysql\
   "C:\Program Files\nodejs\npm" install mysql2
   "C:\Program Files\nodejs\npm" list
   ```

5. Sprawdź instalację, sprawdzając tekst danych wyjściowych polecenia `npm list` dla `mysql2@1.3.5`.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
1. Uruchom następujące polecenia, aby zainstalować środowisko **Node.js** i program **npm**, czyli menedżera pakietów dla środowiska Node.js.

   ```bash
   sudo apt-get install -y nodejs npm
   ```

2. Uruchom następujące polecenia, aby utworzyć folder projektu `mysqlnodejs` i zainstaluj pakiet mysql2 do tego folderu.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```
3. Sprawdź instalację, sprawdzając tekst danych wyjściowych listy npm dla `mysql2@1.3.5`.

### <a name="mac-os"></a>**Mac OS**
1. Wprowadź następujące polecenia, aby zainstalować rozwiązanie **brew**, czyli łatwy w użyciu menedżer pakietów dla systemu Mac OS X i środowiska **Node.js**.

   ```bash
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   brew install node
   ```
2. Uruchom następujące polecenia, aby utworzyć folder projektu `mysqlnodejs` i zainstaluj pakiet mysql2 do tego folderu.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```

3. Sprawdź instalację, sprawdzając tekst danych wyjściowych polecenia `npm list` dla `mysql2@1.3.6`. Numer wersji może się różnić po wydaniu nowych poprawek.

## <a name="get-connection-information"></a>Pobieranie informacji o połączeniu
Pobierz informacje o połączeniu potrzebne do nawiązania połączenia z usługą Azure Database for MySQL. Potrzebna jest w pełni kwalifikowana nazwa serwera i poświadczenia logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. W lewym okienku kliknij pozycję **Wszystkie zasoby**, a następnie wyszukaj utworzony serwer (na przykład **myserver4demo**).
3. Kliknij nazwę serwera **myserver4demo**.
4. Wybierz serwer **właściwości** strony, a następnie zanotuj **nazwy serwera** i **nazwę logowania administratora serwera**.
 ![Azure Database for MySQL — dane logowania administratora serwera](./media/connect-nodejs/1_server-properties-name-login.png)
5. Jeśli użytkownik zapomni swoje informacje logowania serwera, przejdź do **omówienie** strony, aby wyświetlić nazwę logowania administratora serwera, a w razie potrzeby zresetowania hasła.

## <a name="running-the-javascript-code-in-nodejs"></a>Uruchamianie kodu JavaScript w środowisku Node.js
1. Wklej kod JavaScript do plików tekstowych, a następnie zapisz go w folderze projektu z js rozszerzenie pliku (na przykład C:\nodejsmysql\createtable.js lub /home/username/nodejsmysql/createtable.js).
2. Uruchom wiersz polecenia lub powłoki bash, a następnie zmień katalog na folder projektu `cd nodejsmysql`.
3. Aby uruchomić aplikację, wpisz polecenie node, a po nim nazwę pliku, taką jak `node createtable.js`.
4. W systemie Windows, jeśli aplikacja Node nie znajduje się w ścieżce zmiennej środowiskowej, może być konieczne użycie pełnej ścieżki do uruchomienia aplikacji Node, takiej jak `"C:\Program Files\nodejs\node.exe" createtable.js`

## <a name="connect-create-table-and-insert-data"></a>Nawiązywanie połączenia, tworzenie tabeli i wstawianie danych
Użyć poniższego kodu do łączenia i załadować dane przy użyciu **CREATE TABLE** i **INSERT INTO** instrukcji SQL.

Metoda [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) służy do połączenia interfejsem z serwerem programu MySQL. Funkcja [connect()](https://github.com/mysqljs/mysql#establishing-connections) służy do nawiązywania połączenia z serwerem. Funkcja [query()](https://github.com/mysqljs/mysql#performing-queries) służy do wykonywania zapytania SQL względem bazy danych MySQL. 

Zastąp parametry `host`, `user`, `password` i `database` wartościami określonymi podczas tworzenia serwera i bazy danych.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
    if (err) { 
        console.log("!!! Cannot connect !!! Error:");
        throw err;
    }
    else
    {
       console.log("Connection established.");
           queryDatabase();
    }   
});

function queryDatabase(){
       conn.query('DROP TABLE IF EXISTS inventory;', function (err, results, fields) { 
            if (err) throw err; 
            console.log('Dropped inventory table if existed.');
        })
       conn.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);', 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Created inventory table.');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['banana', 150], 
            function (err, results, fields) {
                if (err) throw err;
            else console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['orange', 154], 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['apple', 100], 
        function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.end(function (err) { 
        if (err) throw err;
        else  console.log('Done.') 
        });
};
```

## <a name="read-data"></a>Odczyt danych
Użyć poniższego kodu do łączenia i odczytywanie danych przy użyciu **wybierz** instrukcji SQL. 

Metoda [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) służy do połączenia interfejsem z serwerem programu MySQL. Metoda [connect()](https://github.com/mysqljs/mysql#establishing-connections) służy do nawiązywania połączenia z serwerem. Metoda [query()](https://github.com/mysqljs/mysql#performing-queries) służy do wykonywania zapytania SQL względem bazy danych MySQL. Tablica wyników jest używana do przechowywania wyników zapytania.

Zastąp parametry `host`, `user`, `password` i `database` wartościami określonymi podczas tworzenia serwera i bazy danych.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            readData();
        }   
    });

function readData(){
        conn.query('SELECT * FROM inventory', 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Selected ' + results.length + ' row(s).');
                for (i = 0; i < results.length; i++) {
                    console.log('Row: ' + JSON.stringify(results[i]));
                }
                console.log('Done.');
            })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Closing connection.') 
        });
};
```

## <a name="update-data"></a>Aktualizowanie danych
Użyć poniższego kodu do łączenia i odczytywanie danych przy użyciu **aktualizacji** instrukcji SQL. 

Metoda [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) służy do połączenia interfejsem z serwerem programu MySQL. Metoda [connect()](https://github.com/mysqljs/mysql#establishing-connections) służy do nawiązywania połączenia z serwerem. Metoda [query()](https://github.com/mysqljs/mysql#performing-queries) służy do wykonywania zapytania SQL względem bazy danych MySQL. 

Zastąp parametry `host`, `user`, `password` i `database` wartościami określonymi podczas tworzenia serwera i bazy danych.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            updateData();
        }   
    });

function updateData(){
       conn.query('UPDATE inventory SET quantity = ? WHERE name = ?', [200, 'banana'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Updated ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="delete-data"></a>Usuwanie danych
Użyć poniższego kodu do łączenia i odczytywanie danych przy użyciu **usunąć** instrukcji SQL. 

Metoda [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) służy do połączenia interfejsem z serwerem programu MySQL. Metoda [connect()](https://github.com/mysqljs/mysql#establishing-connections) służy do nawiązywania połączenia z serwerem. Metoda [query()](https://github.com/mysqljs/mysql#performing-queries) służy do wykonywania zapytania SQL względem bazy danych MySQL. 

Zastąp parametry `host`, `user`, `password` i `database` wartościami określonymi podczas tworzenia serwera i bazy danych.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            deleteData();
        }   
    });

function deleteData(){
       conn.query('DELETE FROM inventory WHERE name = ?', ['orange'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Deleted ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="next-steps"></a>Następne kroki
> [!div class="nextstepaction"]
> [Migrowanie bazy danych przy użyciu funkcji eksportowania i importowania](./concepts-migrate-import-export.md)
