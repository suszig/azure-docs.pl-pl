---
title: "Nawiązywanie połączeń z usługą Azure Database for MySQL za pomocą języka Go | Microsoft Docs"
description: "Ten przewodnik Szybki start zawiera kilka przykładów kodu Go, których można używać do nawiązywania połączeń z danymi usługi Azure Database for MySQL i wykonywania zapytań względem nich."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.devlang: go
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: 1f18a35a3c22ecdc379bdffa1ecacb931c62a59d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-database-for-mysql-use-go-language-to-connect-and-query-data"></a>Usługa Azure Database for MySQL: nawiązywanie połączeń z danymi i wykonywanie względem nich zapytań za pomocą języka Go
Tego przewodnika Szybki Start przedstawiono sposób nawiązywania połączenia z bazą danych Azure dla programu MySQL z systemu Windows, Ubuntu Linux i Apple platform system macOS przy użyciu kodu napisanego w [Przejdź](https://golang.org/) języka. Pokazano w nim, jak używać instrukcji języka SQL w celu wysyłania zapytań o dane oraz wstawiania, aktualizowania i usuwania danych w bazie danych. W tym temacie założono, że znasz Programowanie przy użyciu Przejdź i że jesteś nowym użytkownikiem pracy z bazą danych Azure dla programu MySQL.

## <a name="prerequisites"></a>Wymagania wstępne
Ten przewodnik Szybki start jako punktu wyjścia używa zasobów utworzonych w jednym z tych przewodników:
- [Tworzenie serwera usługi Azure Database for MySQL za pomocą witryny Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Tworzenie serwera usługi Azure Database for MySQL za pomocą interfejsu wiersza polecenia platformy Azure](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-go-and-mysql-connector"></a>Instalowanie środowiska języka Go i łącznika bazy danych MySQL
Zainstaluj [Przejdź](https://golang.org/doc/install) i [Przejdź sterownik sql dla programu MySQL](https://github.com/go-sql-driver/mysql#installation) na komputerze lokalnym. W zależności od platformy wykonaj kroki opisane w odpowiedniej sekcji:

### <a name="windows"></a>Windows
1. [Pobierz](https://golang.org/dl/) i zainstaluj środowisko języka Go dla systemu Microsoft Windows zgodnie z [instrukcjami dotyczącymi instalacji](https://golang.org/doc/install).
2. Uruchom wiersz polecenia z menu Start.
3. Utwórz folder dla projektu, np. `mkdir  %USERPROFILE%\go\src\mysqlgo`.
4. Przejdź do folderu projektu, np. `cd %USERPROFILE%\go\src\mysqlgo`.
5. Ustaw zmienną środowiskową GOPATH, aby wskazywała katalog kodu źródłowego. `set GOPATH=%USERPROFILE%\go`.
6. Zainstaluj sterownik [go-sql-driver dla bazy danych MySQL](https://github.com/go-sql-driver/mysql#installation), uruchamiając polecenie `go get github.com/go-sql-driver/mysql`.

   Podsumowując: zainstaluj środowisko języka Go, a następnie uruchom następujące polecenia w wierszu polecenia:
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
5. Ustaw zmienną środowiskową GOPATH tak, aby wskazywała prawidłowy katalog źródłowy, np. folder go w bieżącym katalogu macierzystym. Z powłoki Bash, uruchom `export GOPATH=~/go` można dodać katalogu Przejdź jako GOPATH dla bieżącej sesji powłoki.
6. Zainstaluj sterownik [go-sql-driver dla bazy danych MySQL](https://github.com/go-sql-driver/mysql#installation), uruchamiając polecenie `go get github.com/go-sql-driver/mysql`.

   Podsumowując: uruchom te polecenia programu Bash:
   ```bash
   sudo apt-get install golang-go
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```

### <a name="apple-macos"></a>Apple macOS
1. Pobierz i zainstaluj Go zgodnie z [instrukcje dotyczące instalacji](https://golang.org/doc/install) platformy do dopasowania. 
2. Uruchom powłokę Bash.
3. Utwórz folder dla projektu w katalogu macierzystym, np. `mkdir -p ~/go/src/mysqlgo/`.
4. Przejdź do tego folderu, np. `cd ~/go/src/mysqlgo/`.
5. Ustaw zmienną środowiskową GOPATH tak, aby wskazywała prawidłowy katalog źródłowy, np. folder go w bieżącym katalogu macierzystym. Z powłoki Bash, uruchom `export GOPATH=~/go` można dodać katalogu Przejdź jako GOPATH dla bieżącej sesji powłoki.
6. Zainstaluj sterownik [go-sql-driver dla bazy danych MySQL](https://github.com/go-sql-driver/mysql#installation), uruchamiając polecenie `go get github.com/go-sql-driver/mysql`.

   Podsumowując: zainstaluj środowisko języka Go, następnie uruchom te polecenia programu Bash:
   ```bash
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```

## <a name="get-connection-information"></a>Pobieranie informacji o połączeniu
Pobierz informacje o połączeniu potrzebne do nawiązania połączenia z usługą Azure Database for MySQL. Potrzebna jest w pełni kwalifikowana nazwa serwera i poświadczenia logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Z menu po lewej stronie w portalu Azure kliknij **wszystkie zasoby** , a następnie wyszukaj serwera ma wydłużało (takich jak **myserver4demo**).
3. Kliknij nazwę serwera **myserver4demo**.
4. Wybierz serwer **właściwości** strony, a następnie zanotuj **nazwy serwera** i **nazwę logowania administratora serwera**.
 ![Azure Database for MySQL — dane logowania administratora serwera](./media/connect-go/1_server-properties-name-login.png)
5. Jeśli użytkownik zapomni swoje informacje logowania serwera, przejdź do **omówienie** strony, aby wyświetlić nazwę logowania administratora serwera, a w razie potrzeby zresetowania hasła.
   

## <a name="build-and-run-go-code"></a>Kompilowanie i uruchamianie kodu języka Go 
1. Do pisania kodu w języku Golang można użyć prostego edytora tekstów, takiego jak Notatnik w systemie Microsoft Windows, [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) lub [Nano](https://www.nano-editor.org/) w systemie Ubuntu lub TextEdit w systemie macOS. Jeśli chcesz bardziej zaawansowane funkcje interaktywne programowanie środowiska (IDE), spróbuj [Gogland](https://www.jetbrains.com/go/) przez Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) przez firmę Microsoft lub [Atom](https://atom.io/).
2. Wklej kod Go z sekcji poniżej do plików tekstowych, a następnie zapisz je w folderze projektu z rozszerzeniem pliku \*.go (takich jak ścieżki systemu Windows `%USERPROFILE%\go\src\mysqlgo\createtable.go` lub ścieżka systemu Linux `~/go/src/mysqlgo/createtable.go`).
3. Zlokalizuj `HOST`, `DATABASE`, `USER`, i `PASSWORD` stałe w kodu, a następnie zastąp przykładowe wartości z własne wartości. 
4. Uruchom wiersz polecenia lub powłoki Bash. Przejdź do folderu projektu. Na przykład w systemie Windows uruchom polecenie `cd %USERPROFILE%\go\src\mysqlgo\`. W systemie Linux: `cd ~/go/src/mysqlgo/`.  Niektóre z wymienionych edytorów IDE oferują możliwości debugowania i uruchamiania bez konieczności używania poleceń powłoki.
5. Uruchom kod, wpisując polecenie `go run createtable.go`, aby skompilować aplikację i uruchomić ją. 
6. Alternatywnie, aby skompilować kod w aplikację natywną, uruchom polecenie `go build createtable.go`, a następnie uruchom plik `createtable.exe` w celu uruchomienia aplikacji.

## <a name="connect-create-table-and-insert-data"></a>Nawiązywanie połączenia, tworzenie tabeli i wstawianie danych
Użyć poniższego kodu do łączenia się z serwerem, Utwórz tabelę i załadować dane przy użyciu **Wstaw** instrukcji SQL. 

Kod importuje trzy pakiety: [pakiet sql](https://golang.org/pkg/database/sql/), [pakiet go-sql-driver dla bazy danych MySQL](https://github.com/go-sql-driver/mysql#installation) jako sterownik do komunikacji z usługą Azure Database for MySQL i [pakiet fmt](https://golang.org/pkg/fmt/) na potrzeby drukowanych danych wejściowych i wyjściowych w wierszu polecenia.

Kod wywołuje metodę [sql. Metody Open()](http://go-database-sql.org/accessing.html) nawiązać połączenia z bazą danych Azure dla MySQL który umożliwia sprawdzenie połączenia przy użyciu metody [bazy danych. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). Używane jest [dojście do bazy danych](https://golang.org/pkg/database/sql/#DB), które utrzymuje pulę połączeń dla serwera bazy danych. Kod wywołuje metodę [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) kilka razy, aby uruchomić kilka poleceń DDL. Kod używa również [Prepare()](http://go-database-sql.org/prepared.html) i Exec() do uruchomienia przygotowanych instrukcji o innych parametrach, aby wstawić trzy wiersze. Zawsze, metoda checkError() niestandardowych służy do sprawdzania, jeśli wystąpił błąd i awaryjne, aby zakończyć.

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
Użyć poniższego kodu do łączenia i odczytywanie danych przy użyciu **wybierz** instrukcji SQL. 

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
