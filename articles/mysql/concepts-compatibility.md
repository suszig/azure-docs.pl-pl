---
title: "Sterowniki MySQL i zarządzania narzędzi zgodności | Dokumentacja firmy Microsoft"
description: "Sterowniki MySQL i narzędzia do zarządzania zgodny z bazą danych Azure dla programu MySQL"
services: mysql
author: seanli1988
ms.author: seanli
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 10/27/2017
ms.openlocfilehash: 7578ae710a3d6c81fdfa2952c53a20c2cdccb6d0
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/06/2018
---
# <a name="mysql-drivers-and-management-tools-compatible-with-azure-database-for-mysql"></a>Sterowniki MySQL i narzędzia do zarządzania zgodny z bazą danych Azure dla programu MySQL
W tym artykule opisano sterowniki i narzędzia do zarządzania, które są zgodne z bazą danych Azure dla programu MySQL.

## <a name="mysql-drivers"></a>Sterowniki MySQL
Bazy danych platformy Azure dla programu MySQL używa najpopularniejszych wersja community na świecie bazy danych MySQL. W związku z tym jest zgodny z różnych języków i sterowniki programowania. Celem jest zapewnienie pomocy technicznej trzy najnowsze wersje sterowników MySQL i kontynuować działania z autorów społeczność typu open source, stale zwiększające funkcjonalność i użyteczność sterowników MySQL. Lista sterowników, które zostały przetestowane i jest zgodny z bazą danych Azure MySQL 5.6 i 5.7 znajduje się w poniższej tabeli:

| **Sterownik** | **Łącza** | **Niezgodne wersje** | **Wersje niezgodną** | **Uwagi** |
| :-------- | :------------------------ | :----------- | :---------------------- | :--------------------------------------- |
| PHP | http://PHP.NET/downloads.php | 5.5 5.6 7.x | 5.3 | W przypadku PHP 7.0 połączenia z MySQLi protokołu SSL należy dodać MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT w parametrach połączenia. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> Zestaw PDO: ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` opcję na wartość false.|
| .Net | [MySqlConnector w witrynie GitHub](https://github.com/mysql-net/MySqlConnector) <br> [Pakiet instalacyjny z Nuget](https://www.nuget.org/packages/MySqlConnector/) | 0.27 oraz po | 0.26.5 i przed | |
| Nodejs |  [MySQLjs w witrynie GitHub](https://github.com/mysqljs/mysql/releases) <br> Pakiet instalacyjny z pakietu NPM:<br> Uruchom `npm install mysql` z pakietu NPM | 2.15 | 2.14.1 i przed | |
| PRZEJDŹ | https://github.com/go-SQL-Driver/MySQL/Releases | 1.3 | 1.2 i przed | Użyj allowNativePasswords = true w parametrach połączenia |
| Python | https://pypi.Python.org/pypi/MySQL-Connector-Python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 i przed | |
| Java | https://downloads.mariadb.org/Connector-Java/ | 2.1 2.0 1.6 | 1.5.5 i przed | |

## <a name="management-tools"></a>Narzędzia do zarządzania
Zaletą zgodności obejmuje również narzędzia do zarządzania bazą danych. Istniejących narzędzi powinno być kontynuowane do pracy z bazą danych Azure dla programu MySQL, jak długo Manipulowanie bazą danych działa w granicach uprawnień użytkownika. Trzy wspólnej bazy danych narzędzia do zarządzania, które zostały przetestowane i jest zgodny z bazą danych Azure MySQL 5.6 i 5.7 są wymienione w poniższej tabeli:

|                                     | **MySQL Workbench 6.x lub nowszego** | **Navicat 12** | **PHPMyAdmin 4.x i nowsze** |
| :---------------------------------- | :----------------------------- | :------------- | :-------------------------|
| Tworzenie, aktualizowanie, odczytu, zapisu i usuwania | X | X | X |
| Połączenie SSL | X | X | X |
| SQL kwerendy automatycznego uzupełniania | X | X |  |
| Importowanie i eksportowanie danych | X | X | X |
| Eksportowanie do wielu formatów | X | X | X |
| Wykonywanie kopii zapasowych i przywracanie |  | X |  |
| Wyświetl parametry serwera | X | X | X |
| Wyświetlanie połączeń klienckich | X | X | X |
