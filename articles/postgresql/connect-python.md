---
title: "Nawiązywanie połączeń z usługą Azure Database for PostgreSQL z poziomu środowiska Python | Microsoft Docs"
description: "Ten przewodnik Szybki start zawiera przykładowy kod języka Python, za pomocą którego można nawiązywać połączenie z danymi usługi Azure Database for PostgreSQL i wykonywać zapytania względem nich."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.devlang: python
ms.topic: hero-article
ms.date: 08/10/2017
ms.translationtype: HT
ms.sourcegitcommit: 14915593f7bfce70d7bf692a15d11f02d107706b
ms.openlocfilehash: 0d52a7728e2292946e9328065b973ca7ad37b4f5
ms.contentlocale: pl-pl
ms.lasthandoff: 08/10/2017

---
# <a name="azure-database-for-postgresql-use-python-to-connect-and-query-data"></a>Usługa Azure Database for PostgreSQL: nawiązywanie połączeń z danymi i wykonywanie na nich zapytań za pomocą języka Python
W tym przewodniku Szybki start pokazano, jak używać narzędzi [Python](https://python.org) w celu nawiązywania połączenia z usługą Azure Database for PostgreSQL, a następnie, korzystając z instrukcji języka SQL, wykonywać zapytania o dane, a także wstawiać, aktualizować i usuwać dane z bazy danych z platform Windows, Mac OS i Ubuntu Linux. W krokach w tym artykule założono, że wiesz już, jak programować za pomocą języka Python, i dopiero zaczynasz pracę z usługą Azure Database for PostgreSQL.

## <a name="prerequisites"></a>Wymagania wstępne
Ten przewodnik Szybki start jako punktu wyjścia używa zasobów utworzonych w jednym z tych przewodników:
- [Tworzenie bazy danych — portal](quickstart-create-server-database-portal.md)
- [Tworzenie bazy danych — interfejs wiersza polecenia](quickstart-create-server-database-azure-cli.md)

Wymagane są również:
- Zainstalowany język [python](https://www.python.org/downloads/)
- Zainstalowany pakiet [pip](https://pip.pypa.io/en/stable/installing/) (pakiet pip jest już zainstalowany, jeśli używasz plików binarnych języka Python 2 w wersji >= 2.7.9 lub Python 3 w wersji >= 3.4 pobranych z witryny [python.org](https://python.org), ale należy uaktualnić pakiet pip).

## <a name="install-the-python-connection-libraries-for-postgresql"></a>Instalowanie bibliotek połączeń języka Python dla PostgreSQL
Zainstaluj pakiet [psycopg2](http://initd.org/psycopg/docs/install.html), który umożliwia nawiązywanie połączenia z bazą danych i wykonywanie w niej zapytań. Pakiet psycopg2 jest [dostępny w repozytorium PyPI](https://pypi.python.org/pypi/psycopg2/) w formie pakietów [wheel](http://pythonwheels.com/) dla najpopularniejszych platform (Linux, OSX, Windows), a więc możesz użyć instalacji pakietu pip, aby pobrać binarną wersję modułu, w tym wszystkie zależności:

```cmd
pip install psycopg2
```
Upewnij się, że używasz aktualnej wersji pakietu pip (można go uaktualnić go przy użyciu polecenia podobnego do `pip install -U pip`).

## <a name="get-connection-information"></a>Pobieranie informacji o połączeniu
Uzyskaj parametry połączenia potrzebne do nawiązania połączenia z usługą Azure Database for PostgreSQL. Potrzebna jest w pełni kwalifikowana nazwa serwera i poświadczenia logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. W menu po lewej stronie w witrynie Azure Portal kliknij pozycję **Wszystkie zasoby** i wyszukaj właśnie utworzony serwer **mypgserver 20170401**.
3. Kliknij nazwę serwera **mypgserver 20170401**.
4. Wybierz stronę serwera **Przegląd**. Zanotuj wartości **Nazwa serwera** i **Identyfikator logowania administratora serwera**.
 ![Azure Database for PostgreSQL — dane logowania administratora serwera](./media/connect-python/1-connection-string.png)
5. Jeśli nie pamiętasz informacji logowania do serwera, przejdź do strony **Przegląd**, aby wyświetlić nazwę logowania administratora serwera oraz w razie konieczności zresetować hasło.

## <a name="how-to-run-python-code"></a>Jak uruchomić kod języka Python
- Za pomocą ulubionego edytora tekstu utwórz nowy plik o nazwie postgres.py i zapisz go w folderze projektu. Skopiuj przykład kodu pokazany poniżej i wklej go do pliku tekstowego. Zastąp parametry hosta, nazwy bazy danych, użytkownika i hasła wartościami, które zostały określone podczas tworzenia serwera i bazy danych. Następnie zapisz plik. Upewnij się, że wybrano kodowanie UTF-8 podczas zapisywania pliku w systemie operacyjnym Windows. 
- Aby uruchomić kod, uruchom wiersz polecenia lub powłokę bash. Zmień katalog na folder projektu, taki jak `cd postgresql`. Następnie wpisz polecenie python, a po nim nazwę pliku, na przykład `python postgres.py`.

> [!NOTE]
> W przypadku wersji 3 języka Python może zostać wyświetlony błąd `SyntaxError: Missing parentheses in call to 'print'` podczas uruchamiania poniższych bloków kodu. Jeśli tak się stanie, zastąp każde wywołanie polecenia `print "string"` wywołaniem funkcji, używając nawiasów, na przykład `print("string")`.

## <a name="connect-create-table-and-insert-data"></a>Nawiązywanie połączenia, tworzenie tabeli i wstawianie danych
Użyj poniższego kodu, aby nawiązać połączenie i załadować dane przy użyciu funkcji [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) z instrukcją **INSERT** języka SQL. Funkcja [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) jest używana do wykonywania zapytania SQL względem bazy danych PostgreSQL. Zastąp parametry hosta, nazwy bazy danych, użytkownika i hasła wartościami, które zostały określone podczas tworzenia serwera i bazy danych.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Drop previous table of same name if one exists
cursor.execute("DROP TABLE IF EXISTS inventory;")
print "Finished dropping table (if existed)"

# Create table
cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
print "Finished creating table"

# Insert some data into table
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
print "Inserted 3 rows of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="read-data"></a>Odczyt danych
Użyj poniższego kodu, aby odczytać wstawione dane przy użyciu funkcji [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) z instrukcją **SELECT** języka SQL. Ta funkcja akceptuje zapytanie i zwraca zestaw wyników, względem którego można przeprowadzić iterację przy użyciu funkcji [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall). Zastąp parametry hosta, nazwy bazy danych, użytkownika i hasła wartościami, które zostały określone podczas tworzenia serwera i bazy danych.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Fetch all rows from table
cursor.execute("SELECT * FROM inventory;")
rows = cursor.fetchall()

# Print all rows
for row in rows:
    print "Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2]))

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="update-data"></a>Aktualizowanie danych
Użyj poniższego kodu, aby zaktualizować poprzednio wstawiony wiersz spisu przy użyciu funkcji [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) z instrukcją **UPDATE** języka SQL. Zastąp parametry hosta, nazwy bazy danych, użytkownika i hasła wartościami, które zostały określone podczas tworzenia serwera i bazy danych.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Update a data row in the table
cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
print "Updated 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="delete-data"></a>Usuwanie danych
Użyj poniższego kodu, aby usunąć poprzednio wstawiony element spisu przy użyciu funkcji [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) z instrukcją **DELETE** języka SQL. Zastąp parametry hosta, nazwy bazy danych, użytkownika i hasła wartościami, które zostały określone podczas tworzenia serwera i bazy danych.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Delete data row from table
cursor.execute("DELETE FROM inventory WHERE name = %s;", ("orange",))
print "Deleted 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="next-steps"></a>Następne kroki
> [!div class="nextstepaction"]
> [Migrowanie bazy danych przy użyciu funkcji eksportowania i importowania](./howto-migrate-using-export-and-import.md)

