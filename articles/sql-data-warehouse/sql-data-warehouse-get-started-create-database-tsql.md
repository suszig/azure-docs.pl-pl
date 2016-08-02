<properties
   pageTitle="Tworzenie bazy danych w usłudze SQL Data Warehouse przy użyciu języka TSQL | Microsoft Azure"
   description="Dowiedz się, jak utworzyć bazę danych w usłudze Azure SQL Data Warehouse przy użyciu języka TSQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/04/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Tworzenie bazy danych w usłudze SQL Data Warehouse przy użyciu języka Transact-SQL (TSQL)

> [AZURE.SELECTOR]
- [Portalu Azure](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

W tym artykule opisano sposób tworzenia bazy danych w usłudze SQL Data Warehouse przy użyciu języka Transact-SQL (T-SQL).

## Wymagania wstępne
Przed rozpoczęciem upewnij się, że zostały spełnione następujące wymagania wstępne.

- **Konto platformy Azure**: aby utworzyć konto, zobacz tematy [Bezpłatna wersja próbna platformy Azure][] lub [Środki na korzystanie z systemu Azure w ramach usługi MSDN][].
- **Serwer SQL V12 platformy Azure**: zobacz tematy [Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu Portalu Azure][] lub [Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu programu PowerShell][].
- **Nazwa grupy zasobów**: użyj tej samej grupy zasobów, z której korzysta serwer SQL V12 platformy Azure, lub zobacz [grupy zasobów][], aby utworzyć nową grupę zasobów.
- **Program Visual Studio z narzędziami SQL Server Data Tools**: instrukcje dotyczące instalacji znajdują się w temacie [Instalowanie programu Visual Studio i narzędzi SSDT][].

> [AZURE.NOTE] Utworzenie nowej usługi SQL Data Warehouse może skutkować nową usługą płatną.  Zobacz [Cennik usługi SQL Data Warehouse][], aby uzyskać więcej informacji o cenach.

## Tworzenie bazy danych przy użyciu programu Visual Studio

Jeśli zaczynasz dopiero korzystać z programu Visual Studio, zobacz artykuł [Nawiązywanie połączenia z usługą SQL Data Warehouse przy użyciu programu Visual Studio][].  Aby rozpocząć, otwórz Eksplorator obiektów SQL Server w programie Visual Studio i połącz się z serwerem, który będzie hostem bazy danych usługi SQL Data Warehouse.  Po nawiązaniu połączenia można utworzyć bazę danych usługi SQL Data Warehouse, uruchamiając polecenie SQL **master** w odniesieniu do bazy danych.  To polecenie spowoduje utworzenie bazy danych MySqlDwDb z celem usługi DW400 i możliwością wzrostu bazy danych do maksymalnego rozmiaru 10 TB.

```sql
CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## Tworzenie bazy danych przy użyciu narzędzia sqlcmd

Alternatywnie możesz uruchomić to samo polecenie za pomocą narzędzia sqlcmd w wierszu polecenia w następujący sposób.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Parametry **MAXSIZE** i **SERVICE_OBJECTIVE** określają maksymalną ilość miejsca, którą baza danych może zajmować na dysku, oraz zasoby obliczeniowe przydzielone do tego wystąpienia usługi Data Warehouse.  Celem usługi (SERVICE_OBJECTIVE) jest zasadniczo alokacja zasobów procesora i pamięci skalowanych liniowo zgodnie z rozmiarem jednostki DWU.  

Parametr MAXSIZE może mieć wartość od 250 GB do 240 TB.  Cel usługi może być mieć wartość od DW100 do DW2000.  Pełna lista wszystkich prawidłowych wartości parametrów MAXSIZE i SERVICE_OBJECTIVE znajduje się w dokumentacji MSDN dotyczącej polecenia [CREATE DATABASE][].  Zarówno parametr MAXSIZE, jak i parametr SERVICE_OBJECTIVE można zmienić za pomocą polecenia T-SQL [ALTER DATABASE][].  W przypadku zmiany parametru SERVICE_OBJECTIVE należy zachować ostrożność, ponieważ powoduje to ponowne uruchomienie usług, co z kolei anuluje wszystkie bieżące zapytania.  Zmiana parametru MAXSIZE nie powoduje ponownego uruchamiania usług, ponieważ jest to tylko prosta operacja na metadanych.

## Następne kroki
Po zakończeniu aprowizacji bazy danych usługi SQL Data Warehouse możesz przeprowadzić [ładowanie danych przykładowych][] lub poznać sposoby wykonywania takich czynności, jak [opracowywanie][], [ładowanie][] lub [migrowanie][].

<!--Article references-->
[tworzenie usługi SQL Data Warehouse przy użyciu Portalu Azure]: ./sql-data-warehouse-get-started-provision.md
[Nawiązywanie połączenia z usługą SQL Data Warehouse przy użyciu programu Visual Studio]: ./sql-data-warehouse-get-started-connect.md
[migrowanie]: ./sql-data-warehouse-overview-migrate.md
[opracowywanie]: ./sql-data-warehouse-overview-develop.md
[ładowanie]: ./sql-data-warehouse-overview-load.md
[ładowanie danych przykładowych]: ./sql-data-warehouse-get-started-manually-load-samples.md
[Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu Portalu Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu programu PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[grupy zasobów]: ../azure-portal/resource-group-portal.md
[Instalowanie programu Visual Studio i narzędzi SSDT]: ./sql-data-warehouse-install-visual-studio.md

<!--MSDN references--> 
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Cennik usługi SQL Data Warehouse]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Bezpłatna wersja próbna platformy Azure]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[Środki na korzystanie z systemu Azure w ramach usługi MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=Jun16_HO2-->


