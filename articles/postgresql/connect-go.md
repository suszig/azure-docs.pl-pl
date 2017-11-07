---
title: "Nawiązywanie połączeń z usługą Azure Database for PostgreSQL za pomocą języka Go | Microsoft Docs"
description: "Ten przewodnik Szybki start zawiera przykładowy języka programowania Go, którego można używać do nawiązywania połączeń z danymi usługi Azure Database for PostgreSQL i wykonywania zapytań względem nich."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: go
ms.topic: quickstart
ms.date: 11/03/2017
ms.openlocfilehash: 8b52aeaadf7ba94d6b79ef447600cd7b57e70dfa
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
# <a name="azure-database-for-postgresql-use-go-language-to-connect-and-query-data"></a>Usługa Azure Database for PostgreSQL: nawiązywanie połączeń z danymi i wykonywanie na nich zapytań za pomocą języka Go
Ten przewodnik Szybki start przedstawia sposób nawiązywania połączeń z usługą Azure Database for PostgreSQL przy użyciu kodu napisanego w języku [Go](https://golang.org/) (golang). Pokazano w nim, jak używać instrukcji języka SQL w celu wysyłania zapytań o dane oraz wstawiania, aktualizowania i usuwania danych w bazie danych. W tym artykule założono, że wiesz już, jak programować za pomocą języka Go, i dopiero zaczynasz pracę z usługą Azure Database for PostgreSQL.

## <a name="prerequisites"></a>Wymagania wstępne
Ten przewodnik Szybki start jako punktu wyjścia używa zasobów utworzonych w jednym z tych przewodników:
- [Tworzenie bazy danych — portal](quickstart-create-server-database-portal.md)
- [Tworzenie bazy danych — interfejs wiersza polecenia platformy Azure](quickstart-create-server-database-azure-cli.md)

## <a name="install-go-and-pq-connector"></a>Instalowanie łączników języków Go i pq
Zainstaluj język [Go](https://golang.org/doc/install) i [sterownik Pure Go Postgres (pq)](https://github.com/lib/pq) na własnej maszynie. W zależności od platformy wykonaj odpowiednie czynności:

### <a name="windows"></a>Windows
1. [Pobierz](https://golang.org/dl/) i zainstaluj środowisko języka Go dla systemu Microsoft Windows zgodnie z [instrukcjami dotyczącymi instalacji](https://golang.org/doc/install).
2. Uruchom wiersz polecenia z menu Start.
3. Utwórz folder dla projektu, takie jak `mkdir  %USERPROFILE%\go\src\postgresqlgo`.
4. Przejdź do folderu projektu, np. `cd %USERPROFILE%\go\src\postgresqlgo`.
5. Ustaw zmienną środowiskową GOPATH, aby wskazywała katalog kodu źródłowego. `set GOPATH=%USERPROFILE%\go`.
6. Zainstaluj [sterownik Pure Go Postgres (pq)](https://github.com/lib/pq), uruchamiając polecenie `go get github.com/lib/pq`.

   Podsumowując: zainstaluj środowisko języka Go, a następnie uruchom następujące polecenia w wierszu polecenia:
   ```cmd
   mkdir  %USERPROFILE%\go\src\postgresqlgo
   cd %USERPROFILE%\go\src\postgresqlgo
   set GOPATH=%USERPROFILE%\go
   go get github.com/lib/pq
   ```

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. Uruchom powłokę Bash. 
2. Zainstaluj środowisko języka Go, uruchamiając polecenie `sudo apt-get install golang-go`.
3. Utwórz folder dla projektu w katalogu macierzystym, np. `mkdir -p ~/go/src/postgresqlgo/`.
4. Przejdź do tego folderu, np. `cd ~/go/src/postgresqlgo/`.
5. Ustaw zmienną środowiskową GOPATH tak, aby wskazywała prawidłowy katalog źródłowy, np. folder go w bieżącym katalogu macierzystym. W powłoce Bash uruchom polecenie `export GOPATH=~/go`, aby dodać katalog go jako zmienną GOPATH dla bieżącej sesji powłoki.
6. Zainstaluj [sterownik Pure Go Postgres (pq)](https://github.com/lib/pq), uruchamiając polecenie `go get github.com/lib/pq`.

   Podsumowując: uruchom te polecenia programu Bash:
   ```bash
   sudo apt-get install golang-go
   mkdir -p ~/go/src/postgresqlgo/
   cd ~/go/src/postgresqlgo/
   export GOPATH=~/go/
   go get github.com/lib/pq
   ```

### <a name="apple-macos"></a>Apple macOS
1. Pobierz i zainstaluj język Go zgodnie z [instrukcjami dotyczącymi instalacji](https://golang.org/doc/install) odpowiedniej platformy. 
2. Uruchom powłokę Bash. 
3. Utwórz folder dla projektu w katalogu macierzystym, np. `mkdir -p ~/go/src/postgresqlgo/`.
4. Przejdź do tego folderu, np. `cd ~/go/src/postgresqlgo/`.
5. Ustaw zmienną środowiskową GOPATH tak, aby wskazywała prawidłowy katalog źródłowy, np. folder go w bieżącym katalogu macierzystym. W powłoce Bash uruchom polecenie `export GOPATH=~/go`, aby dodać katalog go jako zmienną GOPATH dla bieżącej sesji powłoki.
6. Zainstaluj [sterownik Pure Go Postgres (pq)](https://github.com/lib/pq), uruchamiając polecenie `go get github.com/lib/pq`.

   Podsumowując: zainstaluj środowisko języka Go, następnie uruchom te polecenia programu Bash:
   ```bash
   mkdir -p ~/go/src/postgresqlgo/
   cd ~/go/src/postgresqlgo/
   export GOPATH=~/go/
   go get github.com/lib/pq
   ```

## <a name="get-connection-information"></a>Pobieranie informacji o połączeniu
Uzyskaj parametry połączenia potrzebne do nawiązania połączenia z usługą Azure Database for PostgreSQL. Potrzebna jest w pełni kwalifikowana nazwa serwera i poświadczenia logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. W menu po lewej stronie w witrynie Azure Portal kliknij pozycję **Wszystkie zasoby** i wyszukaj utworzony serwer, taki jak **mypgserver-20170401**.
3. Kliknij nazwę serwera **mypgserver 20170401**.
4. Wybierz stronę serwera **Przegląd**. Zanotuj wartości **Nazwa serwera** i **Identyfikator logowania administratora serwera**.
 ![Azure Database for PostgreSQL — dane logowania administratora serwera](./media/connect-go/1-connection-string.png)
5. Jeśli nie pamiętasz informacji logowania do serwera, przejdź do strony **Przegląd** i wyświetl nazwę logowania administratora serwera. W razie potrzeby zresetuj hasło.

## <a name="build-and-run-go-code"></a>Kompilowanie i uruchamianie kodu języka Go 
1. Do pisania kodu, Golang, można użyć edytora zwykłego tekstu, takiego jak Notatnik w systemie Microsoft Windows [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) lub [Nano](https://www.nano-editor.org/) Ubuntu lub TextEdit w macOS. Jeśli wolisz bardziej zaawansowane środowisko IDE, wypróbuj rozwiązanie [Gogland](https://www.jetbrains.com/go/) firmy Jetbrains, edytor [Visual Studio Code](https://code.visualstudio.com/) firmy Microsoft lub [Atom](https://atom.io/).
2. Wklej kod Golang z następujących sekcji w plikach tekstowych i zapisać w folderze projektu z rozszerzeniem pliku \*.go, takich jak ścieżki systemu Windows `%USERPROFILE%\go\src\postgresqlgo\createtable.go` lub ścieżka systemu Linux `~/go/src/postgresqlgo/createtable.go`.
3. Zlokalizuj zmienne `HOST`, `DATABASE`, `USER` i `PASSWORD` w kodzie i zastąp przykładowe wartości wybranymi samodzielnie wartościami.  
4. Uruchom wiersz polecenia lub powłokę bash. Przejdź do folderu projektu. Na przykład w systemie Windows uruchom polecenie `cd %USERPROFILE%\go\src\postgresqlgo\`. W systemie Linux: `cd ~/go/src/postgresqlgo/`. Niektóre z wymienionych środowisk IDE oferują możliwości debugowania i uruchamiania bez konieczności używania poleceń powłoki.
5. Uruchom kod, wpisując polecenie `go run createtable.go`, aby skompilować aplikację i uruchomić ją. 
6. Alternatywnie, aby skompilować kod w aplikację natywną, uruchom polecenie `go build createtable.go`, a następnie uruchom plik `createtable.exe` w celu uruchomienia aplikacji.

## <a name="connect-and-create-a-table"></a>Łączenie i tworzenie tabeli
Użyj poniższego kodu w celu nawiązania połączenia i utworzenia tabeli za pomocą instrukcji **CREATE TABLE** języka SQL, a następnie instrukcji **INSERT INTO** języka SQL, aby dodać wiersze do tabeli.

Kod importuje pakiety trzy: [pakietu sql](https://golang.org/pkg/database/sql/), [pakietu pq](http://godoc.org/github.com/lib/pq) jako sterownik do komunikacji z serwerem PostgreSQL i [formatowaniu pakietu](https://golang.org/pkg/fmt/) reakcję wydruku i dane wyjściowe w wierszu polecenia.

Kod wywołuje metodę [sql. Metody Open()](http://godoc.org/github.com/lib/pq#Open) nawiązać połączenia z bazą danych Azure w przypadku bazy danych PostgreSQL i sprawdzić połączenie przy użyciu metody [bazy danych. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). Używane jest [dojście do bazy danych](https://golang.org/pkg/database/sql/#DB), które utrzymuje pulę połączeń dla serwera bazy danych. Kod wywołuje metodę [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) kilka razy, aby uruchomić kilka poleceń SQL. Za każdym razem metody niestandardowe checkError() sprawdza, czy wystąpił błąd i awaryjne, aby zakończyć działanie po wystąpieniu błędu.

Zastąp parametry `HOST`, `DATABASE`, `USER` i `PASSWORD` własnymi wartościami. 

```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/lib/pq"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    // Initialize connection string.
    var connectionString string = fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Drop previous table of same name if one exists.
    _, err = db.Exec("DROP TABLE IF EXISTS inventory;")
    checkError(err)
    fmt.Println("Finished dropping table (if existed)")

    // Create table.
    _, err = db.Exec("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
    checkError(err)
    fmt.Println("Finished creating table")

    // Insert some data into table.
    sql_statement := "INSERT INTO inventory (name, quantity) VALUES ($1, $2);"
    _, err = db.Exec(sql_statement, "banana", 150)
    checkError(err)
    _, err = db.Exec(sql_statement, "orange", 154)
    checkError(err)
    _, err = db.Exec(sql_statement, "apple", 100)
    checkError(err)
    fmt.Println("Inserted 3 rows of data")
}
```

## <a name="read-data"></a>Odczyt danych
Użyj poniższego kodu, aby nawiązać połączenie i odczytać dane za pomocą instrukcji **SELECT** języka SQL. 

Kod importuje pakiety trzy: [pakietu sql](https://golang.org/pkg/database/sql/), [pakietu pq](http://godoc.org/github.com/lib/pq) jako sterownik do komunikacji z serwerem PostgreSQL i [formatowaniu pakietu](https://golang.org/pkg/fmt/) reakcję wydruku i dane wyjściowe w wierszu polecenia.

Kod wywołuje metodę [sql. Metody Open()](http://godoc.org/github.com/lib/pq#Open) nawiązać połączenia z bazą danych Azure w przypadku bazy danych PostgreSQL i sprawdzić połączenie przy użyciu metody [bazy danych. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). Używane jest [dojście do bazy danych](https://golang.org/pkg/database/sql/#DB), które utrzymuje pulę połączeń dla serwera bazy danych. Wybierz zapytanie jest uruchamiany przez wywołanie metody [bazy danych. Query()](https://golang.org/pkg/database/sql/#DB.Query), a wynikowy wiersze są przechowywane w zmiennej typu [wierszy](https://golang.org/pkg/database/sql/#Rows). Kod odczytuje wartości danych kolumny danych w bieżącym wierszu przy użyciu metody [rows.Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) i wykonuje operacje dla wierszy w pętli, używając iteratora [rows.Next()](https://golang.org/pkg/database/sql/#Rows.Next) do momentu, gdy nie ma już kolejnych wierszy. Wartości kolumn w każdym wierszu są drukowane do konsoli. Zawsze metody checkError() niestandardowych służy do sprawdzania, jeśli wystąpił błąd i awaryjne, aby zakończyć działanie po wystąpieniu błędu.

Zastąp parametry `HOST`, `DATABASE`, `USER` i `PASSWORD` własnymi wartościami. 

```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/lib/pq"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString string = fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Read rows from table.
    var id int
    var name string
    var quantity int

    sql_statement := "SELECT * from inventory;"
    rows, err := db.Query(sql_statement)
    checkError(err)

    for rows.Next() {
        switch err := rows.Scan(&id, &name, &quantity); err {
        case sql.ErrNoRows:
            fmt.Println("No rows were returned")
        case nil:
            fmt.Printf("Data row = (%d, %s, %d)\n", id, name, quantity)
        default:
            checkError(err)
        }
    }
}
```

## <a name="update-data"></a>Aktualizowanie danych
Użyć poniższego kodu do łączenia i aktualizowanie danych przy użyciu **aktualizacji** instrukcji SQL.

Kod importuje trzy pakiety: [pakiet sql](https://golang.org/pkg/database/sql/), [pakiet pq](http://godoc.org/github.com/lib/pq) jako sterownik do komunikacji z serwerem Postgres i [pakiet fmt](https://golang.org/pkg/fmt/) dla drukowanych danych wejściowych i wyjściowych w wierszu polecenia.

Kod wywołuje metodę [sql. Metody Open()](http://godoc.org/github.com/lib/pq#Open) nawiązać połączenia z bazą danych Azure w przypadku bazy danych PostgreSQL i sprawdzić połączenie przy użyciu metody [bazy danych. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). Używane jest [dojście do bazy danych](https://golang.org/pkg/database/sql/#DB), które utrzymuje pulę połączeń dla serwera bazy danych. Kod wywołuje metodę [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) w celu uruchomienia instrukcji SQL służącej do aktualizowania tabeli. Metody niestandardowe checkError() służy do sprawdzania, jeśli wystąpił błąd i awaryjne, aby zakończyć działanie po wystąpieniu błędu.

Zastąp parametry `HOST`, `DATABASE`, `USER` i `PASSWORD` własnymi wartościami. 
```go
package main

import (
  "database/sql"
  _ "github.com/lib/pq"
  "fmt"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    
    // Initialize connection string.
    var connectionString string = 
        fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Modify some data in table.
    sql_statement := "UPDATE inventory SET quantity = $2 WHERE name = $1;"
    _, err = db.Exec(sql_statement, "banana", 200)
    checkError(err)
    fmt.Println("Updated 1 row of data")
}
```

## <a name="delete-data"></a>Usuwanie danych
Użyj poniższego kodu, aby nawiązać połączenie i usunąć dane za pomocą instrukcji **DELETE** języka SQL. 

Kod importuje trzy pakiety: [pakiet sql](https://golang.org/pkg/database/sql/), [pakiet pq](http://godoc.org/github.com/lib/pq) jako sterownik do komunikacji z serwerem Postgres i [pakiet fmt](https://golang.org/pkg/fmt/) dla drukowanych danych wejściowych i wyjściowych w wierszu polecenia.

Kod wywołuje metodę [sql. Metody Open()](http://godoc.org/github.com/lib/pq#Open) nawiązać połączenia z bazą danych Azure w przypadku bazy danych PostgreSQL i sprawdzić połączenie przy użyciu metody [bazy danych. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). Używane jest [dojście do bazy danych](https://golang.org/pkg/database/sql/#DB), które utrzymuje pulę połączeń dla serwera bazy danych. Kod wywołuje [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metodę do uruchomienia instrukcji SQL, która usuwa wiersz z tabeli. Metody niestandardowe checkError() służy do sprawdzania, jeśli wystąpił błąd i awaryjne, aby zakończyć działanie po wystąpieniu błędu.

Zastąp parametry `HOST`, `DATABASE`, `USER` i `PASSWORD` własnymi wartościami. 
```go
package main

import (
  "database/sql"
  _ "github.com/lib/pq"
  "fmt"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    
    // Initialize connection string.
    var connectionString string = 
        fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Delete some data from table.
    sql_statement := "DELETE FROM inventory WHERE name = $1;"
    _, err = db.Exec(sql_statement, "orange")
    checkError(err)
    fmt.Println("Deleted 1 row of data")
}
```

## <a name="next-steps"></a>Następne kroki
> [!div class="nextstepaction"]
> [Migrowanie bazy danych przy użyciu funkcji eksportowania i importowania](./howto-migrate-using-export-and-import.md)
