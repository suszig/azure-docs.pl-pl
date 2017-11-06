---
title: "Biblioteki połączeń dla bazy danych Azure dla PostgreSQL | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano kilka bibliotek i sterowniki, deweloperzy mogą używać podczas kodowania aplikacji do nawiązywania połączeń i PostgreSQL kwerendy bazy danych Azure."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: 7d6f13336b3913299433e25c69a41f8c94f598b2
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/13/2017
---
# <a name="connection-libraries-for-azure-database-for-postgresql"></a>Biblioteki połączeń dla bazy danych Azure dla PostgreSQL
Ten temat zawiera listę bibliotek i sterowniki, które umożliwiają deweloperom do aplikacji programu w celu nawiązania połączenia i wyszukiwać w bazie danych Azure PostgreSQL.

## <a name="client-interfaces"></a>Interfejsy klienta
Większość bibliotek klienckich język używany do łączenia z serwerem PostgreSQL projektów zewnętrznych i są dystrybuowane niezależnie. Są obsługiwane na platformach Windows, Linux lub Mac. Niektóre sterowniki popularnych klienta są wymienione w poniższej tabeli:

| **Język** | **Interfejs klienta** | **Dodatkowe informacje** | **Pobieranie** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | Interfejs API DB 2.0 zgodne | [Pobieranie](http://initd.org/psycopg/download/) |
| PHP | [PHP pgsql](https://php.net/manual/en/book.pgsql.php) | Rozszerzenia bazy danych | [Instalowanie](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [Pakiet npm PG](https://www.npmjs.com/package/pg) | Czysty JavaScript bez blokowania klienta | [Instalowanie](https://www.npmjs.com/package/pg) |
| Java | [JDBC](http://jdbc.postgresql.org/) | Sterownik JDBC typu 4 | [Pobieranie](https://jdbc.postgresql.org/download.html)  |
| Ruby | [PG gem](https://deveiate.org/code/pg/) | Interfejs Ruby | [Pobieranie](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Przejdź | [Pakiet pq](https://godoc.org/github.com/lib/pq) | Czysty Przejdź postgres sterownika | [Instalowanie](https://github.com/lib/pq/blob/master/README.md) |
| C\#/ .NET | [Npgsql](http://www.npgsql.org/) | Dostawca danych ADO.NET | [Pobieranie](https://www.microsoft.com/net/) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | Sterownik ODBC | [Pobieranie](http://www.postgresql.org/ftp/odbc/versions/) |
| C | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | Podstawowy interfejs języka C | Dołączono |
| C++ | [libpqxx](http://pqxx.org/) | Nowy styl interfejsu C++ | [Pobieranie](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Następne kroki
Przeczytaj te przewodniki Szybki Start dotyczące nawiązania połączenia i kwerendy bazy danych platformy Azure przy użyciu języka wybór PostgreSQL:

[Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md)  |  [.NET (C#)](./connect-csharp.md) | [przejdź](./connect-go.md)
