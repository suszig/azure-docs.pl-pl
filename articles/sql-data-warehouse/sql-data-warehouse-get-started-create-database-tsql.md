---
title: "Tworzenie magazynu danych SQL Data Warehouse przy użyciu języka TSQL | Microsoft Docs"
description: "Dowiedz się, jak utworzyć bazę danych w usłudze Azure SQL Data Warehouse przy użyciu języka TSQL"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: a4e2e68e-aa9c-4dd3-abb0-f7df997d237a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 5d3bcc3c1434b16279778573ccf3034f9ac28a4d
ms.openlocfilehash: 836d72e32e54ecef9691b55214766a1fc3ff9701
ms.lasthandoff: 12/06/2016


---
# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>Tworzenie bazy danych w usłudze SQL Data Warehouse przy użyciu języka Transact-SQL (TSQL)
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

W tym artykule przedstawiono, jak utworzyć bazę danych w usłudze SQL Data Warehouse przy użyciu języka T-SQL.

## <a name="prerequisites"></a>Wymagania wstępne
Aby rozpocząć pracę, potrzebne będą następujące elementy:

* **Konto platformy Azure**: aby utworzyć konto, odwiedź witrynę [Bezpłatna wersja próbna platformy Azure][Azure Free Trial] lub [Środki na korzystanie z systemu Azure w ramach usługi MSDN][MSDN Azure Credits].
* **Serwer Azure SQL**: aby uzyskać więcej informacji, zobacz [Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu witryny Azure Portal][Create an Azure SQL Database logical server with the Azure Portal] lub [Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu programu PowerShell][Create an Azure SQL Database logical server with PowerShell].
* **Grupa zasobów**: użyj tej samej grupy zasobów, z której korzysta serwer Azure SQL, lub zobacz, [jak utworzyć grupę zasobów][how to create a resource group].
* **Środowisko do wykonywania poleceń języka T-SQL**: do wykonywania poleceń języka T-SQL możesz użyć programu [Visual Studio][Installing Visual Studio and SSDT], narzędzia [sqlcmd][sqlcmd] lub [programu SSMS][SSMS].

> [!NOTE]
> Utworzenie bazy danych w usłudze SQL Data Warehouse może skutkować powstaniem nowej usługi płatnej.  Aby uzyskać więcej informacji o cenach, zobacz [Cennik usługi SQL Data Warehouse][SQL Data Warehouse pricing].
>
>

## <a name="create-a-database-with-visual-studio"></a>Tworzenie bazy danych przy użyciu programu Visual Studio
Jeśli dopiero zaczynasz korzystać z programu Visual Studio, zobacz [Tworzenie zapytań względem usługi Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)].  Aby rozpocząć, otwórz Eksplorator obiektów SQL Server w programie Visual Studio i połącz się z serwerem, który będzie hostem bazy danych usługi SQL Data Warehouse.  Po nawiązaniu połączenia można utworzyć bazę danych usługi SQL Data Warehouse, uruchamiając polecenie SQL **master** w odniesieniu do bazy danych.  To polecenie tworzy bazę danych MySqlDwDb z celem usługi wynoszącym DW400 i możliwością wzrostu bazy danych do maksymalnego rozmiaru 10 TB.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Tworzenie bazy danych przy użyciu narzędzia sqlcmd
Możesz też uruchomić to samo polecenie za pomocą narzędzia sqlcmd w wierszu polecenia w następujący sposób.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Sortowanie domyślne, gdy sortowanie nie jest określone, to COLLATE SQL_Latin1_General_CP1_CI_AS.  Parametr `MAXSIZE` może mieć wartość od 250 GB do 240 TB.  Parametr `SERVICE_OBJECTIVE` może mieć wartość od DW100 do DW2000 jednostek [DWU][DWU].  Lista wszystkich prawidłowych wartości znajduje się w dokumentacji MSDN dotyczącej polecenia [CREATE DATABASE][CREATE DATABASE].  Zarówno parametr MAXSIZE, jak i parametr SERVICE_OBJECTIVE można zmienić za pomocą polecenia języka T-SQL [ALTER DATABASE][ALTER DATABASE].  Po utworzeniu bazy danych nie można zmienić jej sortowania.   W przypadku zmiany parametru SERVICE_OBJECTIVE należy zachować ostrożność, ponieważ zmiana liczby jednostek DWU powoduje ponowne uruchomienie usług, co z kolei anuluje wszystkie bieżące zapytania.  Zmiana parametru MAXSIZE nie powoduje ponownego uruchamiania usług, ponieważ jest to tylko prosta operacja na metadanych.

## <a name="next-steps"></a>Następne kroki
Po zakończeniu aprowizacji bazy danych usługi SQL Data Warehouse możesz przeprowadzić [ładowanie danych przykładowych][load sample data] lub poznać sposoby wykonywania takich czynności, jak [opracowywanie][develop], [ładowanie][load] lub [migrowanie][migrate].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[how to create a SQL Data Warehouse from the Azure portal]: sql-data-warehouse-get-started-provision.md
[Query Azure SQL Data Warehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data]: sql-data-warehouse-load-sample-databases.md
[Create an Azure SQL database with the Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-create-and-configure-database-powershell
[how to create a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references-->
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

