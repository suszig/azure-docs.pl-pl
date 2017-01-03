---
title: "Instrukcje T-SQL nieobsługiwane w usłudze Azure SQL Database | Dokumentacja firmy Microsoft"
description: "Instrukcje języka Transact-SQL, które nie są w pełni obsługiwane w usłudze Azure SQL Database"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: c05abd9e-28a7-4c97-9bdf-bc60d08fc92e
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/28/2016
ms.author: rick.byham@microsoft.com
translationtype: Human Translation
ms.sourcegitcommit: 7db5939c88045ab59d7d9fb4b58f07374d7bca01
ms.openlocfilehash: ebec6ca87cfd5e9d2c4ea726d5d31d20998ef33f


---
# <a name="azure-sql-database-transact-sql-differences"></a>Różnice w języku Transact-SQL usługi Azure SQL Database   
Większość funkcji języka Transact-SQL, od których są zależne aplikacje, jest obsługiwana w programie Microsoft SQL Server i usłudze Azure SQL Database. Na przykład podstawowe elementy języka SQL, takie jak typy danych, operatory, funkcje arytmetyczne, logiczne oraz dotyczące ciągów i kursora, działają bez żadnych różnic w porównaniu z programem SQL Server.

## <a name="why-some-transact-sql-is-not-supported"></a>Dlaczego niektóre instrukcje języka Transact-SQL nie są obsługiwane
Usługa Azure SQL Database została tak zaprojektowana, aby uniezależnić funkcje od bazy danych master i systemu operacyjnego. W rezultacie wiele działań na poziomie serwera nie ma zastosowania do bazy danych SQL. Zazwyczaj nie są dostępne instrukcje języka Transact-SQL służące do konfigurowania opcji na poziomie serwera, składników systemu operacyjnego lub do określania pliku konfiguracji systemu. Jeśli wymagane są funkcje używane poza bazą danych użytkownika, właściwe rozwiązanie jest często dostępne w usłudze SQL Database lub w innej funkcji albo usłudze systemu Azure. 

Na przykład funkcja Always On została zastąpiona przez aktywną replikację geograficzną. Z tego powodu pozostałe instrukcje języka Transact-SQL dotyczące grup dostępności nie są obsługiwane przez bazę danych SQL Database. Nie są również obsługiwane widoki zarządzania dynamicznego dotyczące funkcji Always On.  

Aby zapoznać się z listą funkcji obsługiwanych i nieobsługiwanych przez usługę SQL Database, zobacz artykuł [Azure SQL Database considerations, guidelines and features](sql-database-features.md) (Zagadnienia, wytyczne i funkcje dotyczące usługi SQL Azure Database).

Ogólnie rzecz biorąc, składnia, która jest przestarzała w programie SQL Server, nie jest obsługiwana w usłudze SQL Database.

## <a name="transact-sql-syntax-partially-supported-in-sql-database"></a>Składnia języka Transact-SQL częściowo obsługiwana w bazie danych SQL Database
Baza danych SQL Database obsługuje niektóre (lecz nie wszystkie) argumenty, które istnieją w odpowiedniej instrukcjach języka Transact SQL serwera SQL Server 2016. Na przykład instrukcja `CREATE PROCEDURE` jest dostępna, ale nie są dostępne żadne opcje instrukcji `CREATE PROCEDURE`. Pełny opis składni byłby tutaj zbędny, a może nawet mylący. Aby otrzymać szczegółowe informacje o każdej instrukcji, zapoznaj się z linkami dotyczącymi poszczególnych tematów.

- Bazy danych: [CREATE](https://msdn.microsoft.com/library/dn268335.aspx)/[ALTER DATABASE](https://msdn.microsoft.com/library/ms174269.aspx)   
- Funkcje: [CREATE](https://msdn.microsoft.com/library/ms186755.aspx)/[ALTER FUNCTION](https://msdn.microsoft.com/library/ms186967.aspx)   
- Nazwy logowania: [CREATE](https://msdn.microsoft.com/library/ms189751.aspx)/[ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx)   
- Procedury składowane: [CREATE](https://msdn.microsoft.com/library/ms187926.aspx)/[ALTER PROCEDURE](https://msdn.microsoft.com/library/ms189762.aspx)   
- Tabele: [CREATE](https://msdn.microsoft.com/library/dn305849.aspx)/[ALTER TABLE](https://msdn.microsoft.com/library/ms190273.aspx)   
- Typy (niestandardowe): [CREATE TYPE](https://msdn.microsoft.com/library/ms175007.aspx)   
- Użytkownicy: [CREATE](https://msdn.microsoft.com/library/ms173463.aspx)/[ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx)   
- Widoki: [CREATE](https://msdn.microsoft.com/library/ms187956.aspx)/[ALTER VIEW](https://msdn.microsoft.com/library/ms173846.aspx)   

## <a name="transact-sql-syntax-not-supported-in-sql-database"></a>Składnia języka Transact-SQL nieobsługiwana w bazie danych SQL Database   
Oprócz instrukcji języka Transact-SQL dotyczących nieobsługiwanych funkcji opisanych w artykule [Azure SQL Database considerations, guidelines and features](sql-database-features.md) (Zagadnienia, wytyczne i funkcje dotyczące usługi SQL Azure Database), nie są obsługiwane następujące instrukcje i grupy instrukcji.
- Sortowanie obiektów systemu
- Dotyczące połączeń: instrukcje Endpoint, `ORIGINAL_DB_NAME`. Usługa SQL Database nie obsługuje uwierzytelniania systemu Windows, ale obsługuje podobne uwierzytelnianie usługi Azure Active Directory. Niektóre typy uwierzytelniania wymagają najnowszej wersji usługi SSMS. Aby uzyskać więcej informacji, zobacz artykuł [Connecting to SQL Database or SQL Data Warehouse By Using Azure Active Directory Authentication](sql-database-aad-authentication.md) (Nawiązywanie połączeń z usługami SQL Database lub SQL Data Warehouse przy użyciu uwierzytelniania usługi Azure Active Directory).
- Zapytania obejmujące wiele baz danych, korzystające z nazw trój- i czteroczęściowych. (Zapytania tylko do odczytu obejmujące wiele baz danych są obsługiwane za pomocą [zapytania elastycznej bazy danych](sql-database-elastic-query-overview.md)).
- Tworzenie łańcucha własności między wieloma bazami danych, ustawienie `TRUSTWORTHY`
- `DATABASEPROPERTY` Zamiast tego użyj instrukcji `DATABASEPROPERTYEX`.
- `EXECUTE AS LOGIN` Zamiast tego użyj instrukcji „EXECUTE AS USER”.
- Szyfrowanie jest obsługiwane z wyjątkiem rozszerzonego zarządzania kluczami
- Obsługa zdarzeń: zdarzenia, powiadomienia o zdarzeniach, powiadomienia o zapytaniach
- Składnia dotycząca rozmieszczenia pliku bazy danych, rozmiaru i plików bazy danych, które są zarządzane automatycznie przez program Microsoft Azure.
- Składnia dotycząca wysokiej dostępności, która jest zarządzana przez konto Microsoft Azure. Obejmuje to składnię kopii zapasowych, przywracania, funkcji Always On, dublowania bazy danych, wysyłania dziennika oraz trybów odzyskiwania.
- Składnia opierająca się na odczytywaniu dziennika, która nie jest dostępna w bazie danych SQL Database: Push Replication, Change Data Capture. Baza danych SQL Database może być subskrybentem artykułu replikacji wypychanej.
- Składnia, która opiera się na bazie danych MSDB lub agencie programu SQL Server: alerty, operatory, centralne serwery zarządzania. Zamiast tego używaj skryptów, takich jak Azure PowerShell.
- Funkcje: `fn_get_sql`, `fn_virtualfilestats`, `fn_virtualservernodes`
- Globalne tabele tymczasowe
- Składnia dotycząca ustawień związanych ze sprzętem serwera: pamięć, wątki robocze, koligacje procesora, flagi śledzenia itp. Zamiast tego użyj poziomów usług.
- `HAS_DBACCESS`
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET`, `OPENDATASOURCE`, `BULK INSERT` i czteroczęściowe nazwy
- .NET Framework [Integracja CLR z programem SQL Server](http://msdn.microsoft.com/library/ms254963.aspx)
- Wyszukiwanie semantyczne
- Poświadczenia serwera. Zamiast tego użyj zakresu poświadczeń bazy danych.
- Elementy na poziomie serwera: role serwera, `IS_SRVROLEMEMBER`, `sys.login_token`. Instrukcje `GRANT`, `REVOKE` i `DENY` w odniesieniu do uprawnień na poziomie serwera nie są dostępne, chociaż niektóre zostały zastąpione przez uprawnienia na poziomie bazy danych. Niektóre przydatne dynamiczne widoki zarządzania na poziomie serwera mają swoje odpowiedniki na poziomie bazy danych.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
- Opcje polecenia `sp_configure` i instrukcja `RECONFIGURE`. Niektóre opcje są dostępne po użyciu instrukcji [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- Inspekcja programu SQL Server. Zamiast tego użyj inspekcji bazy danych SQL Database.
- Śledzenie programu SQL Server
- Flagi śledzenia. Niektóre elementy flag śledzenia zostały przeniesione do trybów zgodności.
- Debugowanie języka Transact-SQL
- Wyzwalacze: wyzwalacze zakresu serwera lub wyzwalacze logowania
- Instrukcja `USE`: aby zmienić kontekst bazy danych na inną bazę danych, należy wprowadzić nowe połączenie z nową bazą danych.

## <a name="full-transact-sql-reference"></a>Pełna dokumentacja języka Transact-SQL
Aby uzyskać więcej informacji dotyczących gramatyki języka Transact-SQL oraz przykłady jego użycia, zobacz artykuł [Transact-SQL Reference (Database Engine)](https://msdn.microsoft.com/library/bb510741.aspx) (Dokumentacja języka Transact-SQL (aparat bazy danych)) w dokumentacji SQL Server — książki online. 

### <a name="about-the-applies-to-tags"></a>Tagi „Applies to” (Dotyczy)
Dokumentacja języka Transact-SQL zawiera tematy dotyczące programu SQL Server od wersji 2008 do wersji bieżącej. Poniżej tytułu tematu znajduje się pasek ikon, na którym wyświetlono cztery platformy programu SQL Server oraz wskazania, której wersji dotyczy temat. Na przykład grupy dostępności zostały wprowadzone w programie SQL Server 2012. Temat [CREATE AVAILABILTY GROUP](https://msdn.microsoft.com/library/ff878399.aspx) wskazuje, że instrukcja ma zastosowanie do **programu SQL Server (począwszy od wersji 2012)**. Instrukcja nie dotyczy programu SQL Server 2008, SQL Server 2008 R2, usługi Azure SQL Database, programu Azure SQL Data Warehouse ani Parallel Data Warehouse.

W niektórych przypadkach funkcjonalność podana w temacie może być używana w produkcie, ale między produktami występują niewielkie różnice. Różnice są wskazane w podpunktach tematu, gdy jest to wymagane.



<!--HONumber=Dec16_HO5-->


