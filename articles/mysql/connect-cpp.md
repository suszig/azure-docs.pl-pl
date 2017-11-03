---
title: "Nawiązywanie połączeń z usługą Azure Database for MySQL za pomocą języka C++ | Microsoft Docs"
description: "Ten przewodnik Szybki start zawiera przykład kodu w języku C++, za pomocą którego można nawiązywać połączenie z danymi usługi Azure Database for MySQL i wykonywać zapytania względem nich."
services: mysql
author: seanli1988
ms.author: seal
manager: janders
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.devlang: C++
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: 92620c8081b1f0f5c96cc3ae09465b3526e74042
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-database-for-mysql-use-connectorc-to-connect-and-query-data"></a>Usługa Azure Database for MySQL: nawiązywanie połączeń z danymi i wykonywanie na nich zapytań przy użyciu łącznika/języka C++
Tego przewodnika Szybki Start przedstawiono sposób nawiązywania połączenia z bazą danych Azure dla programu MySQL przy użyciu aplikacji C++. Pokazano w nim, jak używać instrukcji języka SQL w celu wysyłania zapytań o dane oraz wstawiania, aktualizowania i usuwania danych w bazie danych. W tym temacie założono, że znasz tworzenie przy użyciu języka C++ i czy masz doświadczenia w pracy z bazą danych Azure dla programu MySQL.

## <a name="prerequisites"></a>Wymagania wstępne
Zasoby utworzone w jednym z następujących przewodnikach jako punkt początkowy korzysta z tego przewodnika Szybki Start:
- [Tworzenie serwera usługi Azure Database for MySQL za pomocą witryny Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Tworzenie serwera usługi Azure Database for MySQL za pomocą interfejsu wiersza polecenia platformy Azure](./quickstart-create-mysql-server-database-using-azure-cli.md)

Należy również:
- Zainstalować program [.NET Framework](https://www.microsoft.com/net/download)
- Zainstalować program [Visual Studio](https://www.visualstudio.com/downloads/)
- Zainstalować [łącznik bazy danych MySQL/środowisko C++](https://dev.mysql.com/downloads/connector/cpp/) 
- Zainstalować rozwiązanie [Boost](http://www.boost.org/)

## <a name="install-visual-studio-and-net"></a>Instalowanie programu Visual Studio i technologii .NET
W krokach w tej sekcji założono, że wiesz już, jak programować za pomocą technologii .NET.

### <a name="windows"></a>**Windows**
- Instalowanie programu Visual Studio Community 2017, czyli pełnej polecany, rozszerzalny, bezpłatna IDE do tworzenia nowoczesnych aplikacji dla systemu Android, iOS, Windows, a także sieci web i aplikacji baz danych i usług w chmurze. Można zainstalować pełne .NET Framework lub po prostu .NET Core: wstawki kodu w szybki start pracy z jednym. Jeśli masz już zainstalowanego na komputerze programu Visual Studio, pomiń następne dwa kroki.
   1. Pobierz [Instalator programu Visual Studio 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15). 
   2. Uruchom instalatora i postępuj zgodnie z wyświetlanymi monitami, aby ukończyć instalację.

### <a name="configure-visual-studio"></a>**Konfigurowanie programu Visual Studio**
1. W programie Visual Studio w lokalizacji Właściwość projektu > Właściwości konfiguracji > C/C++ > Konsolidator > Ogólne > Dodatkowe katalogi biblioteki dodaj katalog lib\opt (tj. C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\lib\opt) łącznika języka C++.
2. W programie Visual Studio projektu właściwości > Właściwości konfiguracji > C/C++ > Ogólne > Dodatkowe katalogi dołączenia:
   - Dodaj obejmują / directory łącznika c ++ (np.: C:\Program Files (x86) \MySQL\MySQL C++ łącznika 1.1.9\include\).
   - Dodaj katalog główny zwiększanie wyniku biblioteki (np.: C:\boost_1_64_0\).
3. W programie Visual Studio projektu właściwości > Właściwości konfiguracji > C/C++ > konsolidatora > wprowadzania > istnieją dodatkowe zależności, Dodaj mysqlcppconn.lib do pola tekstowego.
4. Skopiuj mysqlcppconn.dll z folderu biblioteki C++ łącznika w kroku 3 na tym samym katalogu co plik wykonywalny aplikacji, albo Dodaj do zmiennej środowiskowej, można odnaleźć aplikacji.

## <a name="get-connection-information"></a>Pobieranie informacji o połączeniu
Pobierz informacje o połączeniu potrzebne do nawiązania połączenia z usługą Azure Database for MySQL. Potrzebna jest w pełni kwalifikowana nazwa serwera i poświadczenia logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Z menu po lewej stronie w portalu Azure kliknij **wszystkie zasoby**i poszukaj serwera utworzono (takich jak **myserver4demo**).
3. Kliknij nazwę serwera.
4. Wybierz serwer **właściwości** strony, a następnie zanotuj **nazwy serwera** i **nazwę logowania administratora serwera**.
 ![Nazwa serwera usługi Azure Database for MySQL](./media/connect-cpp/1_server-properties-name-login.png)
5. Jeśli użytkownik zapomni swoje informacje logowania serwera, przejdź do **omówienie** strony, aby wyświetlić nazwę logowania administratora serwera, a w razie potrzeby zresetowania hasła.

## <a name="connect-create-table-and-insert-data"></a>Nawiązywanie połączenia, tworzenie tabeli i wstawianie danych
Użyć poniższego kodu do łączenia i załadować dane przy użyciu **CREATE TABLE** i **INSERT INTO** instrukcji SQL. Kod używa klasy sql::Driver z metodą connect() w celu ustanowienia połączenia z programem MySQL. Następnie kod używa metod createStatement() i execute(), aby uruchamiać polecenia bazy danych. 

Zastąp parametry hosta, nazwy bazy danych, użytkownika i hasła wartościami, które zostały określone podczas tworzenia serwera i bazy danych. 

```c++
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::Statement *stmt;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }

    stmt = con>createStatement();
    stmt>execute("DROP TABLE IF EXISTS inventory");
    cout << "Finished dropping table (if existed)" << endl;
    stmt>execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
    cout << "Finished creating table" << endl;
    delete stmt;

    pstmt = con>prepareStatement("INSERT INTO inventory(name, quantity) VALUES(?,?)");
    pstmt>setString(1, "banana");
    pstmt>setInt(2, 150);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "orange");
    pstmt>setInt(2, 154);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "apple");
    pstmt>setInt(2, 100);
    pstmt>execute();
    cout << "One row inserted." << endl;
    
    delete pstmt;   
    delete con;
    system("pause");
    return 0;

```

## <a name="read-data"></a>Odczyt danych

Użyć poniższego kodu do łączenia i odczytywanie danych przy użyciu **wybierz** instrukcji SQL. Kod używa klasy sql::Driver z metodą connect() w celu ustanowienia połączenia z programem MySQL. Następnie kod używa metod prepareStatement() i executeQuery(), aby uruchamiać polecenia select. Następnie kod używa next(), aby przejść do rekordów w wynikach. Na koniec kod używa getInt() i getString() można przeanalizować wartości w rekordzie.

Zastąp parametry hosta, nazwy bazy danych, użytkownika i hasła wartościami, które zostały określone podczas tworzenia serwera i bazy danych. 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

//  select  
    pstmt = con>prepareStatement("SELECT * FROM inventory;");
    result = pstmt>executeQuery();  
    
    while (result>next())
        printf("Reading from table=(%d, %s, %d)\n", result>getInt(1), result>getString(2).c_str(), result>getInt(3));   
    
    delete result;
    delete pstmt;   
    delete con;
    system("pause");
    return 0;
}
```

## <a name="update-data"></a>Aktualizowanie danych
Użyć poniższego kodu do łączenia i odczytywanie danych przy użyciu **aktualizacji** instrukcji SQL. Kod używa klasy sql::Driver z metodą connect() w celu ustanowienia połączenia z programem MySQL. Następnie kod używa metod prepareStatement() i executeQuery(), aby uruchamiać polecenia update. 

Zastąp parametry hosta, nazwy bazy danych, użytkownika i hasła wartościami, które zostały określone podczas tworzenia serwera i bazy danych. 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

    //update
    pstmt = con>prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?");
    pstmt>setInt(1, 200);
    pstmt>setString(2, "banana");
    pstmt>executeQuery();
    printf("Row updated\n");
    
    delete con;
    delete pstmt;
    system("pause");
    return 0;
}
```


## <a name="delete-data"></a>Usuwanie danych
Użyć poniższego kodu do łączenia i odczytywanie danych przy użyciu **usunąć** instrukcji SQL. Kod używa klasy sql::Driver z metodą connect() w celu ustanowienia połączenia z programem MySQL. Następnie kod używa metod prepareStatement() i executeQuery(), aby uruchamiać polecenia delete.

Zastąp parametry hosta, nazwy bazy danych, użytkownika i hasła wartościami, które zostały określone podczas tworzenia serwera i bazy danych. 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }
        
    //delete
    pstmt = con>prepareStatement("DELETE FROM inventory WHERE name = ?");
    pstmt>setString(1, "orange");
    result = pstmt>executeQuery();
    printf("Row deleted\n");    
    
    delete pstmt;
    delete con;
    delete result;
    system("pause");
    return 0;
}
```

## <a name="next-steps"></a>Następne kroki
> [!div class="nextstepaction"]
> [Migrowanie bazy danych MySQL do usługi Azure Database for MySQL przy użyciu zrzutu i przywracania](concepts-migrate-dump-restore.md)
