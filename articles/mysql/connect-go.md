---
title: "Nawiązywanie połączeń z usługą Azure Database for MySQL za pomocą języka Go | Microsoft Docs"
description: "Ten przewodnik Szybki start zawiera kilka przykładów kodu Go, których można używać do nawiązywania połączeń z danymi usługi Azure Database for MySQL i wykonywania zapytań względem nich."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: go
ms.topic: hero-article
ms.date: 07/18/2017
ms.translationtype: HT
ms.sourcegitcommit: bde1bc7e140f9eb7bb864c1c0a1387b9da5d4d22
ms.openlocfilehash: ffe09758d0bf5dd4a6e599b1a606d9ae5fce4bf9
ms.contentlocale: pl-pl
ms.lasthandoff: 07/21/2017

---

# <a name="azure-database-for-mysql-use-go-language-to-connect-and-query-data"></a>Usługa Azure Database for MySQL: nawiązywanie połączeń z danymi i wykonywanie względem nich zapytań za pomocą języka Go
Ten przewodnik Szybki start przedstawia sposób nawiązywania połączeń z usługą Azure Database for MySQL przy użyciu kodu napisanego w języku [Go](https://golang.org/) na platformach Windows, Ubuntu Linux i Apple macOS. Pokazano w nim, jak używać instrukcji języka SQL w celu wysyłania zapytań o dane oraz wstawiania, aktualizowania i usuwania danych w bazie danych. W tym artykule założono, że wiesz już, jak programować za pomocą języka Go, ale dopiero zaczynasz pracę z usługą Azure Database for MySQL.

## <a name="prerequisites"></a>Wymagania wstępne
Ten przewodnik Szybki start jako punktu wyjścia używa zasobów utworzonych w jednym z tych przewodników:
- [Tworzenie serwera usługi Azure Database for MySQL za pomocą witryny Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Tworzenie serwera usługi Azure Database for MySQL za pomocą interfejsu wiersza polecenia platformy Azure](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-go-and-mysql-connector"></a>Instalowanie środowiska języka Go i łącznika bazy danych MySQL
Zainstaluj środowisko języka [Go](https://golang.org/doc/install) i sterownik [go-sql-driver dla bazy danych MySQL](https://github.com/go-sql-driver/mysql#installation) na swojej maszynie. W zależności od używanej platformy wykonaj następujące kroki:

### <a name="windows"></a>Windows
1. [Pobierz](https://golang.org/dl/) i zainstaluj środowisko języka Go dla systemu Microsoft Windows zgodnie z [instrukcjami dotyczącymi instalacji](https://golang.org/doc/install).
2. Uruchom wiersz polecenia z menu Start.
3. Utwórz folder dla projektu, np. `mkdir  %USERPROFILE%\go\src\mysqlgo`.
4. Przejdź do folderu projektu, np. `cd %USERPROFILE%\go\src\mysqlgo`.
5. Ustaw zmienną środowiskową GOPATH, aby wskazywała katalog kodu źródłowego. `set GOPATH=%USERPROFILE%\go`.
6. Zainstaluj sterownik [go-sql-driver dla bazy danych MySQL](https://github.com/go-sql-driver/mysql#installation), uruchamiając polecenie `go get github.com/go-sql-driver/mysql`.

   ```cmd
   mkdir  %USERPROFILE%\go\src\mysqlgo
   cd %USERPROFILE%\go\src\mysqlgo
   set GOPATH=%USERPROFILE%\go
   go get github.com/go-sql-driver/mysql
   ```

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. Uruchom powłokę Bash. 
2. Zainstaluj środowisko języka Go, uruchamiając polecenie `sudo apt-get install golang-go`.
3. Utwórz folder dla projektu w katalogu macierzystym, np. `mkdir -p ~/go/src/mysqlgo/`.
4. Przejdź do tego folderu, np. `cd ~/go/src/mysqlgo/`.
5. Ustaw zmienną środowiskową GOPATH tak, aby wskazywała prawidłowy katalog źródłowy, np. folder go w bieżącym katalogu macierzystym. W powłoce Bash uruchom polecenie `export GOPATH=~/go`, aby dodać katalog go jako zmienną GOPATH dla bieżącej sesji powłoki.
6. Zainstaluj sterownik [go-sql-driver dla bazy danych MySQL](https://github.com/go-sql-driver/mysql#installation), uruchamiając polecenie `go get github.com/go-sql-driver/mysql`.

   ```bash
   sudo apt-get install golang-go
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```

### <a name="apple-macos"></a>Apple macOS
1. Pobierz i zainstaluj język Go zgodnie z [instrukcjami dotyczącymi instalacji](https://golang.org/doc/install) odpowiedniej platformy. 
2. Uruchom powłokę Bash. 
3. Utwórz folder dla projektu w katalogu macierzystym, np. `mkdir -p ~/go/src/mysqlgo/`.
4. Przejdź do tego folderu, np. `cd ~/go/src/mysqlgo/`.
5. Ustaw zmienną środowiskową GOPATH tak, aby wskazywała prawidłowy katalog źródłowy, np. folder go w bieżącym katalogu macierzystym. W powłoce Bash uruchom polecenie `export GOPATH=~/go`, aby dodać katalog go jako zmienną GOPATH dla bieżącej sesji powłoki.
6. Zainstaluj sterownik [go-sql-driver dla bazy danych MySQL](https://github.com/go-sql-driver/mysql#installation), uruchamiając polecenie `go get github.com/go-sql-driver/mysql`.

   ```bash
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```


## <a name="get-connection-information"></a>Pobieranie informacji o połączeniu
Pobierz informacje o połączeniu potrzebne do nawiązania połączenia z usługą Azure Database for MySQL. Potrzebna jest w pełni kwalifikowana nazwa serwera i poświadczenia logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. W menu po lewej stronie w witrynie Azure Portal kliknij pozycję **Wszystkie zasoby** i wyszukaj utworzony serwer, taki jak **myserver4demo**.
3. Kliknij nazwę serwera **myserver4demo**.
4. Wybierz stronę **Właściwości** serwera. Zanotuj wartości **Nazwa serwera** i **Identyfikator logowania administratora serwera**.
 ![Azure Database for MySQL — dane logowania administratora serwera](./media/connect-go/1_server-properties-name-login.png)
5. Jeśli nie pamiętasz informacji logowania do serwera, przejdź do strony **Przegląd**, aby wyświetlić nazwę logowania administratora serwera oraz w razie konieczności zresetować hasło.
   

## <a name="build-and-run-go-code"></a>Kompilowanie i uruchamianie kodu języka Go 
1. Wklej kod języka Go z poniższych sekcji do plików tekstowych i zapisz pliki w folderze projektu z rozszerzeniem pliku go. Na przykład ścieżka w systemie Windows: `%USERPROFILE%\go\src\mysqlgo\createtable.go`, ścieżka w systemie Linux: `~/go/src/mysqlgo/createtable.go`.
2. Uruchom wiersz polecenia lub powłokę Bash. Przejdź do folderu projektu. Na przykład w systemie Windows uruchom polecenie `cd %USERPROFILE%\go\src\mysqlgo\`. W systemie Linux: `cd ~/go/src/mysqlgo/`.
3. Uruchom kod, wpisując polecenie `go run createtable.go`, aby skompilować aplikację i uruchomić ją.
4. Alternatywnie, aby skompilować kod w aplikację natywną, uruchom polecenie `go build createtable.go`, a następnie uruchom plik `createtable.exe` w celu uruchomienia aplikacji.

## <a name="connect-create-table-and-insert-data"></a>Nawiązywanie połączenia, tworzenie tabeli i wstawianie danych
Użyj poniższego kodu, aby nawiązać połączenie z serwerem, utworzyć tabelę i załadować dane przy użyciu instrukcji **INSERT** języka SQL. 

Kod importuje trzy pakiety: [pakiet sql](https://golang.org/pkg/database/sql/), [pakiet go-sql-driver dla bazy danych MySQL](https://github.com/go-sql-driver/mysql#installation) jako sterownik do komunikacji z usługą Azure Database for MySQL i [pakiet fmt](https://golang.org/pkg/fmt/) na potrzeby drukowanych danych wejściowych i wyjściowych w wierszu polecenia.

Kod wywołuje metodę [sql.Open()](http://go-database-sql.org/accessing.html), aby nawiązać połączenie z usługą Azure Database for MySQL, i sprawdza połączenie przy użyciu metody [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping). Używane jest [dojście do bazy danych](https://golang.org/pkg/database/sql/#DB), które utrzymuje pulę połączeń dla serwera bazy danych. Kod wywołuje metodę [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) kilka razy, aby uruchomić kilka poleceń DDL. Kod używa również metod [Prepare()](http://go-database-sql.org/prepared.html) i Exec() w celu uruchomienia przygotowanych instrukcji z różnymi parametrami, aby wstawić trzy wiersze. Za każdym razem jest wywoływana niestandardowa metoda checkError() w celu sprawdzenia, czy wystąpił błąd, i awaryjnego zakończenia pracy.

Zastąp stałe `host`, `database`, `user` i `password` własnymi wartościami. 

```Go
package main

import (
    "database/sql"
    "fmt"

    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database.")

    // Drop previous table of same name if one exists.
    _, err = db.Exec("DROP TABLE IF EXISTS inventory;")
    checkError(err)
    fmt.Println("Finished dropping table (if existed).")

    // Create table.
    _, err = db.Exec("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
    checkError(err)
    fmt.Println("Finished creating table.")

    // Insert some data into table.
    sqlStatement, err := db.Prepare("INSERT INTO inventory (name, quantity) VALUES (?, ?);")
    res, err := sqlStatement.Exec("banana", 150)
    checkError(err)
    rowCount, err := res.RowsAffected()
    fmt.Printf("Inserted %d row(s) of data.\n", rowCount)

    res, err = sqlStatement.Exec("orange", 154)
    checkError(err)
    rowCount, err = res.RowsAffected()
    fmt.Printf("Inserted %d row(s) of data.\n", rowCount)

    res, err = sqlStatement.Exec("apple", 100)
    checkError(err)
    rowCount, err = res.RowsAffected()
    fmt.Printf("Inserted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}

```

## <a name="read-data"></a>Odczyt danych
Użyj poniższego kodu, aby nawiązać połączenie i odczytać dane za pomocą instrukcji **SELECT** języka SQL. 

Kod importuje trzy pakiety: [pakiet sql](https://golang.org/pkg/database/sql/), [pakiet go-sql-driver dla bazy danych MySQL](https://github.com/go-sql-driver/mysql#installation) jako sterownik do komunikacji z usługą Azure Database for MySQL i [pakiet fmt](https://golang.org/pkg/fmt/) na potrzeby drukowanych danych wejściowych i wyjściowych w wierszu polecenia.

Kod wywołuje metodę [sql.Open()](http://go-database-sql.org/accessing.html), aby nawiązać połączenie z usługą Azure Database for MySQL, i sprawdza połączenie przy użyciu metody [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping). Używane jest [dojście do bazy danych](https://golang.org/pkg/database/sql/#DB), które utrzymuje pulę połączeń dla serwera bazy danych. Kod wywołuje metodę [Query()](https://golang.org/pkg/database/sql/#DB.Query), aby uruchomić polecenie select. Następnie uruchamia polecenie [Next()](https://golang.org/pkg/database/sql/#Rows.Next) w celu iterowania przez zestaw wyników i polecenie [Scan()](https://golang.org/pkg/database/sql/#Rows.Scan), aby przeanalizować wartości kolumn i zapisać wartości w zmiennych. Za każdym razem jest wywoływana niestandardowa metoda checkError() w celu sprawdzenia, czy wystąpił błąd, i awaryjnego zakończenia pracy.

Zastąp stałe `host`, `database`, `user` i `password` własnymi wartościami. 

```Go
package main

import (
    "database/sql"
    "fmt"

    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database.")

    // Variables for printing column data when scanned.
    var (
        id       int
        name     string
        quantity int
    )

    // Read some data from the table.
    rows, err := db.Query("SELECT id, name, quantity from inventory;")
    checkError(err)
    defer rows.Close()
    fmt.Println("Reading data:")
    for rows.Next() {
        err := rows.Scan(&id, &name, &quantity)
        checkError(err)
        fmt.Printf("Data row = (%d, %s, %d)\n", id, name, quantity)
    }
    err = rows.Err()
    checkError(err)
    fmt.Println("Done.")
}
```

## <a name="update-data"></a>Aktualizowanie danych
Użyj poniższego kodu, aby nawiązać połączenie i zaktualizować dane za pomocą instrukcji **UPDATE** języka SQL. 

Kod importuje trzy pakiety: [pakiet sql](https://golang.org/pkg/database/sql/), [pakiet go-sql-driver dla bazy danych MySQL](https://github.com/go-sql-driver/mysql#installation) jako sterownik do komunikacji z usługą Azure Database for MySQL i [pakiet fmt](https://golang.org/pkg/fmt/) na potrzeby drukowanych danych wejściowych i wyjściowych w wierszu polecenia.

Kod wywołuje metodę [sql.Open()](http://go-database-sql.org/accessing.html), aby nawiązać połączenie z usługą Azure Database for MySQL, i sprawdza połączenie przy użyciu metody [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping). Używane jest [dojście do bazy danych](https://golang.org/pkg/database/sql/#DB), które utrzymuje pulę połączeń dla serwera bazy danych. Kod wywołuje metodę [Exec()](https://golang.org/pkg/database/sql/#DB.Exec), aby uruchomić polecenie update. Za każdym razem jest wywoływana niestandardowa metoda checkError() w celu sprawdzenia, czy wystąpił błąd, i awaryjnego zakończenia pracy.

Zastąp stałe `host`, `database`, `user` i `password` własnymi wartościami. 

```Go
package main

import (
    "database/sql"
    "fmt"

    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database.")

    // Modify some data in table.
    rows, err := db.Exec("UPDATE inventory SET quantity = ? WHERE name = ?", 200, "banana")
    checkError(err)
    rowCount, err := rows.RowsAffected()
    fmt.Printf("Deleted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}
```

## <a name="delete-data"></a>Usuwanie danych
Użyj poniższego kodu, aby nawiązać połączenie i usunąć dane za pomocą instrukcji **DELETE** języka SQL. 

Kod importuje trzy pakiety: [pakiet sql](https://golang.org/pkg/database/sql/), [pakiet go-sql-driver dla bazy danych MySQL](https://github.com/go-sql-driver/mysql#installation) jako sterownik do komunikacji z usługą Azure Database for MySQL i [pakiet fmt](https://golang.org/pkg/fmt/) na potrzeby drukowanych danych wejściowych i wyjściowych w wierszu polecenia.

Kod wywołuje metodę [sql.Open()](http://go-database-sql.org/accessing.html), aby nawiązać połączenie z usługą Azure Database for MySQL, i sprawdza połączenie przy użyciu metody [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping). Używane jest [dojście do bazy danych](https://golang.org/pkg/database/sql/#DB), które utrzymuje pulę połączeń dla serwera bazy danych. Kod wywołuje metodę [Exec()](https://golang.org/pkg/database/sql/#DB.Exec), aby uruchomić polecenie delete. Za każdym razem jest wywoływana niestandardowa metoda checkError() w celu sprawdzenia, czy wystąpił błąd, i awaryjnego zakończenia pracy.

Zastąp stałe `host`, `database`, `user` i `password` własnymi wartościami. 

```Go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database.")

    // Modify some data in table.
    rows, err := db.Exec("DELETE FROM inventory WHERE name = ?", "orange")
    checkError(err)
    rowCount, err := rows.RowsAffected()
    fmt.Printf("Deleted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}
```

## <a name="next-steps"></a>Następne kroki
> [!div class="nextstepaction"]
> [Migrowanie bazy danych przy użyciu funkcji eksportowania i importowania](./concepts-migrate-import-export.md)

