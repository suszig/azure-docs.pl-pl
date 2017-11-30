---
title: "Biblioteki połączeń dla bazy danych SQL | Dokumentacja firmy Microsoft"
description: "Zawiera łącza do pobrania modułów, które umożliwiają połączenie programu SQL Server i bazy danych SQL z szerokiej gamy klienta języków programowania."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: genemi
ms.assetid: 13d899d3-cf46-4e4d-8919-cf4b41ca836d
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: genemi
ms.openlocfilehash: 44c4491e7c198eec43a31480ef3df748626b7b75
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2017
---
# <a name="connectivity-libraries-and-frameworks-for-microsoft-sql-server"></a>Łączność bibliotek i platform dla programu Microsoft SQL Server

Zapoznaj się z naszym [pobierania samouczków dotyczących](http://aka.ms/sqldev) szybkie wprowadzenie do programowania języków, takich jak C#, Java, Node.js, PHP i Python i tworzenie aplikacji za pomocą programu SQL Server w systemie Linux lub Windows lub platformy Docker na macOS.

Poniższa tabela zawiera listę bibliotek łączności lub *sterowniki* czy aplikacje klienckie mogą używać z różnych języków nawiązywania i używać programu Microsoft SQL Server, uruchamiane lokalnie lub w chmurze w systemie Linux, Windows lub Docker, a także do usługi Azure SQL Database i Azure SQL Data Warehouse. 

| Język | Platforma | Dodatkowe zasoby | Do pobrania | Wprowadzenie |
| :-- | :-- | :-- | :-- | :-- |
| C# | System macOS Windows, Linux, | [Microsoft ADO.NET dla programu SQL Server](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-for-sql-server) | [Pobieranie](https://www.microsoft.com/net/download/) | [Rozpoczęcie pracy](https://www.microsoft.com/en-us/sql-server/developer-get-started/csharp/ubuntu)
| Java | System macOS Windows, Linux, | [Sterownik JDBC firmy Microsoft dla programu SQL Server](http://msdn.microsoft.com/library/mt484311.aspx) | [Pobieranie](https://go.microsoft.com/fwlink/?linkid=852460) |  [Rozpoczęcie pracy](https://www.microsoft.com/en-us/sql-server/developer-get-started/java/ubuntu)
| PHP | System macOS Windows, Linux,| [Sterownik PHP SQL dla programu SQL Server](http://msdn.microsoft.com/library/dn865013.aspx) | System operacyjny: <br/> \*[Systemu Windows](https://www.microsoft.com/download/details.aspx?id=20098) <br/> \*[Systemu Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \*[macOS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [Rozpoczęcie pracy](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/ubuntu)
| Node.js | System macOS Windows, Linux, | [Sterownik programu node.js dla programu SQL Server](http://msdn.microsoft.com/library/mt652093.aspx) | [Instalowanie](https://msdn.microsoft.com/library/mt652094.aspx) |  [Rozpoczęcie pracy](https://www.microsoft.com/en-us/sql-server/developer-get-started/node/ubuntu)
| Python | System macOS Windows, Linux, | [Sterownik SQL Python](http://msdn.microsoft.com/library/mt652092.aspx) | Zainstaluj opcje: <br/> \*[pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \*[pyodbc](http://msdn.microsoft.com/library/mt763257.aspx) |  [Rozpoczęcie pracy](https://www.microsoft.com/en-us/sql-server/developer-get-started/python/ubuntu)
| Ruby | System macOS Windows, Linux, | [Sterownik dopisków fonetycznych dla programu SQL Server](http://msdn.microsoft.com/library/mt691981.aspx) | [Instalowanie](https://msdn.microsoft.com/library/mt711041.aspx) | [Rozpoczęcie pracy](https://www.microsoft.com/en-us/sql-server/developer-get-started/ruby/ubuntu)
| C++ | System macOS Windows, Linux, | [Sterownik ODBC firmy Microsoft dla programu SQL Server](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) | [Pobieranie](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) |  

W poniższej tabeli przedstawiono kilka przykładów struktur relacyjne mapowania ORM (Object) i platformy sieci web, korzystających z aplikacji klienckich programu Microsoft SQL Server, uruchamiane lokalnie lub w chmurze w systemie Linux, Windows lub Docker, a także do usługi Azure SQL Database i Azure SQL Data Warehouse. 

| Język | Platforma | ORM(s) |
| :-- | :-- | :-- |
| C# | System macOS Windows, Linux, | [Entity Framework](https://docs.microsoft.com/en-us/ef)<br>[Program Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/index) |
| Java | System macOS Windows, Linux, |[Hibernacji ORM](http://hibernate.org/orm)|
| PHP | Windows, Linux | [Laravel (Eloquent)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | System macOS Windows, Linux, | [Sequelize ORM](http://docs.sequelizejs.com) |
| Python | System macOS Windows, Linux, |[Django](https://www.djangoproject.com/) |
| Ruby | System macOS Windows, Linux, | [Ruby na szyny](http://rubyonrails.org/) |
||||

## <a name="related-links"></a>Powiązane linki
- [SQL Server sterowniki](http://msdn.microsoft.com/library/mt654049.aspx) do łączenia za pośrednictwem aplikacji klienckich
- Połączenie z bazą danych SQL:
    - [Nawiązywanie połączenia z bazą danych SQL Database przy użyciu platformy .NET (C#)](sql-database-connect-query-dotnet.md)
    - [Nawiązywanie połączenia z bazą danych SQL Database przy użyciu języka PHP](sql-database-connect-query-php.md)
    - [Nawiązywanie połączenia z bazą danych SQL Database przy użyciu języka Node.js](sql-database-connect-query-nodejs.md)
    - [Nawiązywanie połączenia z bazą danych SQL Database przy użyciu platformy Java](sql-database-connect-query-java.md)
    - [Nawiązywanie połączenia z bazą danych SQL Database przy użyciu języka Python](sql-database-connect-query-python.md)
    - [Nawiązywanie połączenia z bazą danych SQL Database przy użyciu języka Ruby](sql-database-connect-query-ruby.md)
- Ponów próbę logiki przykłady kodu:
    - [Resiliently połączenia z serwerem SQL z ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
    - [Resiliently połączenia z serwerem SQL za pomocą języka PHP][step-4-connect-resiliently-to-sql-with-php-p42h]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php

