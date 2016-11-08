---
title: Tworzenie bazy danych w usłudze SQL Data Warehouse w portalu Azure | Microsoft Docs
description: Dowiedz się, jak utworzyć bazę danych w usłudze Azure SQL Data Warehouse w portalu Azure
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: ''
tags: azure-sql-data-warehouse

ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 08/25/2016
ms.author: barbkess;lodipalm;sonyama

---
# Tworzenie bazy danych w usłudze Azure SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
> 
> 

Z tego samouczka dowiesz się, jak za pomocą witryny Azure Portal utworzyć bazę danych w usłudze SQL Data Warehouse zawierającą przykładową bazę danych AdventureWorksDW.

## Wymagania wstępne
Aby rozpocząć pracę, potrzebne będą następujące elementy:

* **Konto platformy Azure**: Aby utworzyć konto, odwiedź witrynę [Bezpłatna wersja próbna platformy Azure][Bezpłatna wersja próbna platformy Azure] lub [Środki na korzystanie z systemu Azure w ramach usługi MSDN][Środki na korzystanie z systemu Azure w ramach usługi MSDN].
* **Serwer Azure SQL**: Aby uzyskać więcej informacji, zobacz [Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu witryny Azure Portal][Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu witryny Azure Portal].

> [!NOTE]
> Utworzenie bazy danych w usłudze SQL Data Warehouse może skutkować powstaniem nowej usługi płatnej.  Aby uzyskać więcej informacji o cenach, zobacz [Cennik usługi SQL Data Warehouse][Cennik usługi SQL Data Warehouse].
> 
> 

## Tworzenie bazy danych w usłudze SQL Data Warehouse
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Kliknij kolejno opcje **+ Nowy** > **Dane i magazyn** > **SQL Data Warehouse**.
   
    ![Tworzenie](./media/sql-data-warehouse-get-started-provision/create-sample.gif)
3. W bloku **SQL Data Warehouse** podaj potrzebne informacje, a następnie naciśnij przycisk „Utwórz”, aby utworzyć usługę.
   
    ![Tworzenie bazy danych](./media/sql-data-warehouse-get-started-provision/create-database.png)
   
   * **Serwer**: zalecamy, aby najpierw wybrać serwer.  
   * **Nazwa bazy danych**: Nazwa używana jako odwołanie do usługi SQL Data Warehouse.  Musi być unikatowa dla serwera.
   * **Wydajność**: Zalecamy zacząć od 400 jednostek [DWU][DWU]. Możesz przesuwać suwak w lewo lub w prawo, aby dostosować wydajność magazynu danych albo skalować w górę lub w dół po utworzeniu.  Aby dowiedzieć się więcej o jednostkach DWU, zobacz dokumentację dotyczącą [skalowania](sql-data-warehouse-manage-compute-overview.md) lub nasz [cennik][Cennik usługi SQL Data Warehouse]. 
   * **Subskrypcja**: Wybierz [subskrypcję], która będzie obciążana płatnościami za tę usługę SQL Data Warehouse.
   * **Grupa zasobów**: [Grupy zasobów][Grupa zasobów] to kontenery, których zadaniem jest ułatwianie zarządzania kolekcją zasobów platformy Azure. Dowiedz się więcej o [grupach zasobów](../resource-group-overview.md).
   * **Wybierz źródło**: kliknij kolejno **Wybierz źródło**  >  **Przykład**. Platforma Azure automatycznie wypełni opcję **Wybierz przykład** bazą danych AdventureWorksDW.

> [!NOTE]
> Sortowanie domyślne dla usługi SQL Data Warehouse to SQL_Latin1_General_CP1_CI_AS. Jeśli potrzebne jest inne sortowanie, za pomocą polecenia języka [T-SQL][T-SQL] można utworzyć bazę danych z innym sortowaniem.
> 
> 

1. Kliknij przycisk **Utwórz**, aby utworzyć usługę SQL Data Warehouse.
2. Poczekaj kilka minut. Po utworzeniu magazynu danych powinien nastąpić powrót do witryny [Azure Portal](https://portal.azure.com). Usługę SQL Data Warehouse można znaleźć na pulpicie nawigacyjnym, na liście Bazy danych SQL lub w grupie zasobów użytej podczas tworzenia. 
   
    ![widok portalu](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## Następne kroki
Po utworzeniu bazy danych w usłudze SQL Data Warehouse można rozpocząć [nawiązywanie połączenia](sql-data-warehouse-connect-overview.md) i wykonywanie zapytań.

Aby załadować dane do usługi SQL Data Warehouse, zobacz [omówienie ładowania](sql-data-warehouse-overview-load.md).

Jeśli próbujesz przeprowadzić migrację z istniejącej bazy danych do usługi SQL Data Warehouse, zobacz [omówienie migracji](sql-data-warehouse-overview-migrate.md) lub użyj [narzędzia do migracji](sql-data-warehouse-migrate-migration-utility.md).

Reguły zapory można również skonfigurować za pomocą języka Transact-SQL. Aby uzyskać więcej informacji, zobacz artykuły dotyczące poleceń [sp_set_firewall_rule][sp_set_firewall_rule] i [sp_set_database_firewall_rule][sp_set_database_firewall_rule].

Warto również zapoznać się z artykułem [Najlepsze praktyki][Najlepsze praktyki].

<!--Article references-->
[Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu witryny Azure Portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu programu PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[grupy zasobów]: ../resource-group-template-deploy-portal.md
[Najlepsze praktyki]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md#data-warehouse-units
[subskrypcję]: ../azure-glossary-cloud-terminology.md#subscription
[grupa zasobów]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md

<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[Cennik usługi SQL Data Warehouse]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Bezpłatna wersja próbna platformy Azure]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[Środki na korzystanie z systemu Azure w ramach usługi MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F




<!--HONumber=sep16_HO1-->


