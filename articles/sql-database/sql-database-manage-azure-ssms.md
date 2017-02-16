---
title: "Zarządzanie usługą SQL Database przy użyciu programu SSMS | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zarządzać serwerami i bazami danych usługi SQL Database przy użyciu programu SQL Server Management Studio."
services: sql-database
documentationcenter: .net
author: stevestein
manager: jhubbard
editor: tysonn
ms.assetid: da6f3608-5993-4134-a497-ff2811e9f31f
ms.service: sql-database
ms.custom: overview
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/29/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: d9ff74a49742fa77f5989b8b05e0567e3ca81dc5
ms.openlocfilehash: 89cb8827745b31b3a77b64d5cafd586957d60d30


---
# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>Zarządzanie usługą Azure SQL Database za pomocą programu SQL Server Management Studio
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-manage-portal.md)
> * [SSMS](sql-database-manage-azure-ssms.md)
> * [Program PowerShell](sql-database-manage-powershell.md)
> 
> 

Program SQL Server Management Studio (SSMS) umożliwia administrowanie serwerami i bazami danych usługi Azure SQL Database. W tym temacie omówiono typowe zadania związane z programem SSMS. Przed rozpoczęciem pracy w usłudze Azure SQL Database powinny już być utworzone serwer i baza danych. Zobacz artykuły [Tworzenie pierwszej bazy danych w usłudze Azure SQL Database](sql-database-get-started.md) i [Nawiązywanie połączenia i tworzenie zapytań za pomocą programu SSMS](sql-database-connect-query-ssms.md), aby uzyskać więcej informacji.

> [!TIP]
> Samouczek z omówieniem tworzenia serwera i opartej na serwerze zapory, wyświetlania właściwości serwera, nawiązywania połączenia przy użyciu programu SQL Server Management Studio, odpytywania głównej bazy danych, tworzenia przykładowej i pustej bazy danych, tworzenia zapytań o właściwości bazy danych, nawiązywania połączenia przy użyciu programu SQL Server Management Studio, a także odpytywania przykładowej bazy danych, znajduje się w artykule [Samouczek z wprowadzeniem](sql-database-get-started.md).
>

Przy korzystaniu z usługi Azure SQL Database zaleca się używanie najnowszej wersji programu SSMS. 

> [!IMPORTANT]
> Należy zawsze używać najnowszej wersji programu SSMS, ponieważ jest on stale udoskonalany w celu umożliwienia współpracy z najnowszymi aktualizacjami usług Azure i SQL Database. Aby pobrać najnowszą wersję, zobacz artykuł [Pobieranie programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="create-and-manage-azure-sql-databases"></a>Tworzenie baz danych usługi Azure SQL i zarządzanie nimi
Po połączeniu się z bazą danych **master** można tworzyć bazy danych na serwerze, a także modyfikować lub usuwać istniejące bazy danych. W poniższych krokach opisano sposób wykonywania kilku typowych zadań zarządzania bazą danych za pomocą programu Management Studio. Aby wykonać te zadania, upewnij się, że istnieje połączenie z bazą danych **master** nawiązane z użyciem głównej nazwy logowania na poziomie serwera utworzonej podczas konfigurowania serwera.

Aby otworzyć okno zapytania w programie Management Studio, otwórz folder baz danych, rozwiń folder **Systemowe bazy danych**, kliknij prawym przyciskiem myszy bazę danych **master**, a następnie kliknij przycisk **Nowe zapytanie**.

* Użyj instrukcji **CREATE DATABASE**, aby utworzyć bazę danych. Aby uzyskać więcej informacji, zobacz artykuł [CREATE DATABASE (usługa SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx). Poniższa instrukcja umożliwia utworzenie bazy danych o nazwie **myTestDB** i określenie jej jako bazy danych Standard S0 Edition o domyślnym maksymalnym rozmiarze 250 GB.
  
      CREATE DATABASE myTestDB
      (EDITION='Standard',
       SERVICE_OBJECTIVE='S0');

Kliknij przycisk **Wykonaj**, aby uruchomić zapytanie.

* Użyj instrukcji **ALTER DATABASE**, aby zmodyfikować istniejącą bazę danych, na przykład zmienić nazwę lub wersję bazy danych. Aby uzyskać więcej informacji, zobacz artykuł [ALTER DATABASE (usługa SQL Database)](https://msdn.microsoft.com/library/ms174269.aspx). Poniższa instrukcja umożliwia zmodyfikowanie bazy danych utworzonej w poprzednim kroku przez zmianę wersji na Standard S1.
  
      ALTER DATABASE myTestDB
      MODIFY
      (SERVICE_OBJECTIVE='S1');
* Użyj instrukcji **DROP DATABASE**, aby usunąć istniejącą bazę danych. Aby uzyskać więcej informacji, zobacz artykuł [DROP DATABASE (usługa SQL Database)](https://msdn.microsoft.com/library/ms178613.aspx). Poniższa instrukcja umożliwia usunięcie bazy danych **myTestDB**, ale nie należy usuwać jej w tym momencie, ponieważ w następnym kroku zostanie ona użyta do utworzenia nazw logowania.
  
      DROP DATABASE myTestBase;
* Widok **sys.databases** w bazie danych master służy do wyświetlania szczegółowych informacji o wszystkich bazach danych. Aby wyświetlić wszystkie istniejące bazy danych, należy wykonać następującą instrukcję:
  
      SELECT * FROM sys.databases;
* W usłudze SQL Database instrukcja **USE** nie obsługuje przełączania między bazami danych. Zamiast tego należy ustanowić połączenie bezpośrednio z docelową bazą danych.

> [!NOTE]
> Wiele instrukcji języka Transact-SQL umożliwiających utworzenie lub zmodyfikowanie bazy danych wymaga uruchomienia w ramach własnych zestawów instrukcji. Nie można ich grupować z innymi instrukcjami języka Transact-SQL. Aby dowiedzieć się więcej, zobacz informacje dotyczące określonej instrukcji.
> 
> 

## <a name="create-and-manage-logins"></a>Tworzenie nazw logowania i zarządzanie nimi
Baza danych **master** zawiera nazwy logowania i informacje o tym, które z nich mają uprawnienia do tworzenia baz danych lub innych nazw logowania. Aby zarządzać nazwami logowania, połącz się z bazą danych **master** przy użyciu głównej nazwy logowania na poziomie serwera utworzonej podczas konfigurowania serwera. Możesz użyć instrukcji **CREATE LOGIN**, **ALTER LOGIN** lub **DROP LOGIN**, aby wykonać zapytania względem bazy danych master, która zarządza nazwami logowania dla całego serwera. Aby uzyskać więcej informacji, zobacz artykuł [Zarządzanie bazami danych i nazwami logowania w usłudze SQL Database](http://msdn.microsoft.com/library/azure/ee336235.aspx). 

* Użyj instrukcji **CREATE LOGIN**, aby utworzyć nazwę logowania na poziomie serwera. Aby uzyskać więcej informacji, zobacz artykuł [CREATE LOGIN (usługa SQL Database)](https://msdn.microsoft.com/library/ms189751.aspx). Poniższa instrukcja umożliwia utworzenie nazwy logowania **login1**. Zastąp hasło **password1** wybranym przez siebie hasłem.
  
      CREATE LOGIN login1 WITH password='password1';
* Użyj instrukcji **CREATE USER**, aby przyznać uprawnienia na poziomie bazy danych. Wszystkie nazwy logowania muszą być utworzone w bazie danych **master**. Aby utworzyć nazwę logowania pozwalającą nawiązać połączenie z inną bazą danych, należy przyznać jej uprawnienia na poziomie bazy danych przy użyciu instrukcji **CREATE USER** w tej bazie danych. Aby uzyskać więcej informacji, zobacz artykuł [CREATE USER (usługa SQL Database)](https://msdn.microsoft.com/library/ms173463.aspx). 
* Aby nazwie login1 przyznać uprawnienia do bazy danych o nazwie **myTestDB**, wykonaj następujące kroki:
  
  1. Aby odświeżyć Eksploratora obiektów i wyświetlić utworzoną bazę danych **myTestDB**, kliknij prawym przyciskiem myszy nazwę serwera w Eksploratorze obiektów, a następnie kliknij pozycję **Odśwież**.  
     
     Jeśli połączenie zostanie zakończone, można połączyć się ponownie, wybierając polecenie **Połącz z Eksploratorem obiektów** w menu Plik.
  2. Kliknij prawym przyciskiem myszy bazę danych **myTestDB**, a następnie wybierz pozycję **Nowe zapytanie**.
  3. Wykonaj następującą instrukcję względem bazy danych myTestDB, aby utworzyć użytkownika bazy danych o nazwie **login1User**, który odpowiada nazwie logowania na poziomie serwera **login1**.
     
         CREATE USER login1User FROM LOGIN login1;
* Użyj składowanej procedury **sp\_addrolemember**, aby nadać kontu użytkownika odpowiedni poziom uprawnień w bazie danych. Aby uzyskać więcej informacji, zobacz artykuł [sp_addrolemember (język Transact-SQL)](http://msdn.microsoft.com/library/ms187750.aspx). Poniższa instrukcja przyznaje użytkownikowi **login1User** uprawnienia tylko do odczytu do bazy danych przez dodanie użytkownika **login1User** do roli **db\_datareader**.
  
      exec sp_addrolemember 'db_datareader', 'login1User';    
* Użyj instrukcji **ALTER LOGIN**, aby zmodyfikować istniejącą nazwę logowania, na przykład jeśli chcesz zmienić hasło dla nazwy logowania. Aby uzyskać więcej informacji, zobacz artykuł [ALTER LOGIN (usługa SQL Database)](https://msdn.microsoft.com/library/ms189828.aspx). Instrukcja **ALTER LOGIN** powinna być uruchamiana względem bazy danych **master**. Wróć do okna zapytania, które jest połączone z tą bazą danych. Poniższa instrukcja umożliwia zmodyfikowanie nazwy logowania **login1** przez zresetowanie hasła. Zastąp ciąg **newPassword** wybranym przez siebie hasłem oraz ciąg **oldPassword** bieżącym hasłem dla danej nazwy logowania.
  
      ALTER LOGIN login1
      WITH PASSWORD = 'newPassword'
      OLD_PASSWORD = 'oldPassword';
* Użyj instrukcji **DROP LOGIN**, aby usunąć istniejącą nazwę logowania. Usunięcie nazwy logowania na poziomie serwera spowoduje także usunięcie skojarzonych kont użytkowników bazy danych. Aby uzyskać więcej informacji, zobacz artykuł [DROP DATABASE (usługa SQL Database)](https://msdn.microsoft.com/library/ms178613.aspx). Instrukcja **DROP LOGIN** powinna być uruchamiana względem bazy danych **master**. Instrukcja powoduje usunięcie nazwy logowania **login1**.
  
      DROP LOGIN login1;
* Widok **sys.sql\_logins** w bazie danych master pozwala wyświetlić nazwy logowania. Aby wyświetlić wszystkie istniejące nazwy logowania, należy wykonać następującą instrukcję:
  
      SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-views"></a>Monitorowanie bazy danych SQL Database przy użyciu dynamicznych widoków zarządzania
Usługa SQL Database obsługuje kilka widoków dynamicznego zarządzania, których można używać do monitorowania poszczególnych baz danych. Oto kilka przykładów typów monitorowanych danych, które można pobrać przy użyciu tych widoków. Pełne informacje i więcej przykładów użycia przedstawiono w artykule [Monitorowanie bazy danych SQL Database przy użyciu dynamicznych widoków zarządzania](https://msdn.microsoft.com/library/azure/ff394114.aspx).

* Wykonywanie zapytań dotyczących dynamicznych widoków zarządzania wymaga uprawnień **VIEW DATABASE STATE** . Aby przyznać określonemu użytkownikowi bazy danych uprawnienie **VIEW DATABASE STATE**, połącz się z bazą danych i wykonaj następującą instrukcję względem bazy danych:
  
      GRANT VIEW DATABASE STATE TO login1User;
* Oblicz rozmiar bazy danych, używając widoku **sys.dm\_db\_partition\_stats**. Widok **sys.dm\_db\_partition\_stats** zwraca informacje o liczbie stron i wierszy dla każdej partycji w bazie danych, przy użyciu których można obliczyć rozmiar bazy danych. Następujące zapytanie zwraca rozmiar bazy danych w megabajtach:
  
      SELECT SUM(reserved_page_count)*8.0/1024
      FROM sys.dm_db_partition_stats;   
* Użyj widoków **sys.dm\_exec\_connections** i **sys.dm\_exec\_sessions** w celu pobrania informacji o połączeniach bieżącego użytkownika i wewnętrznych zadaniach skojarzonych z bazą danych. Następujące zapytanie zwraca informacje dotyczące bieżącego połączenia:
  
      SELECT
          e.connection_id,
          s.session_id,
          s.login_name,
          s.last_request_end_time,
          s.cpu_time
      FROM
          sys.dm_exec_sessions s
          INNER JOIN sys.dm_exec_connections e
            ON s.session_id = e.session_id;
* Użyj widoku **sys.dm\_exec\_query\_stats** w celu pobrania zagregowanych statystyk wydajności dotyczących planów zapytań zapisanych w pamięci podręcznej. Następujące zapytanie zwraca informacje o pierwszych pięciu zapytaniach uszeregowanych według średniego czasu procesora CPU.
  
      SELECT TOP 5 query_stats.query_hash AS "Query Hash",
          SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
          MIN(query_stats.statement_text) AS "Statement Text"
      FROM
          (SELECT QS.*,
          SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
          ((CASE statement_end_offset
              WHEN -1 THEN DATALENGTH(ST.text)
              ELSE QS.statement_end_offset END
                  - QS.statement_start_offset)/2) + 1) AS statement_text
           FROM sys.dm_exec_query_stats AS QS
           CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
      GROUP BY query_stats.query_hash
      ORDER BY 2 DESC;




<!--HONumber=Dec16_HO3-->


