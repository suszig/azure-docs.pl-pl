<properties
   pageTitle="Tworzenie bazy danych w usłudze SQL Data Warehouse w Portalu Azure | Microsoft Azure"
   description="Dowiedz się, jak utworzyć bazę danych usługi Azure SQL Data Warehouse w Portalu Azure"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/05/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Tworzenie bazy danych w usłudze Azure SQL Data Warehouse

> [AZURE.SELECTOR]
- [Portal Azure](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

W tym samouczku za pomocą Portalu Azure utworzysz usługę SQL Data Warehouse zawierającą przykładową bazę danych AdventureWorksDW.


[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]


1. Zaloguj się do [Portalu Azure](https://portal.azure.com).

2. Kliknij kolejno opcje **+ Nowy**  >  **Dane i magazyn**  >  **SQL Data Warehouse**.

    ![Tworzenie](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. W bloku **SQL Data Warehouse** podaj potrzebne informacje, a następnie naciśnij przycisk „Utwórz”, aby utworzyć usługę.

    ![Tworzenie bazy danych](./media/sql-data-warehouse-get-started-provision/create-database.png)

    - **Serwer**: zalecamy, aby najpierw wybrać serwer.  Możesz wybrać istniejący serwer lub [utworzyć nowy](./sql-data-warehouse-get-started-new-server.md). 

    - **Nazwa bazy danych**: nazwa, która będzie służyć jako odwołanie do usługi SQL Data Warehouse.  Musi być unikatowa dla serwera.
    
    - **Wydajność**: Zalecamy rozpoczynanie od 400 jednostek DWU. Możesz przesuwać suwak w lewo lub w prawo, aby dostosować wydajność magazynu danych albo skalować w górę lub w dół po utworzeniu.  Aby dowiedzieć się więcej o jednostkach DWU, zobacz dokumentację dotyczącą [skalowania](./sql-data-warehouse-manage-compute-overview.md) lub nasz [cennik](https://azure.microsoft.com/en-us/pricing/details/sql-data-warehouse/). 

    - **Subskrypcja**: wybierz subskrypcję, która będzie obciążana płatnościami za tę usługę SQL Data Warehouse.

    - **Grupa zasobów**: grupy zasobów to kontenery, które mają ułatwiać zarządzanie kolekcją zasobów systemu Azure. Dowiedz się więcej o [grupach zasobów](../azure-portal/resource-group-portal.md).

    - **Wybierz źródło**: kliknij kolejno **Wybierz źródło**  >  **Przykład**. Ponieważ w tej chwili jest dostępna tylko jedna przykładowa baza danych, po wybraniu opcji Przykład system Azure automatycznie wypełnia opcję **Wybierz przykład** nazwą AdventureWorksDW.

4. Kliknij przycisk **Utwórz**, aby utworzyć usługę SQL Data Warehouse.

5. Zaczekaj kilka minut, aż usługa SQL Data Warehouse będzie gotowa. Po zakończeniu powinien nastąpić powrót do [Portalu Azure](https://portal.azure.com). Usługę SQL Data Warehouse można znaleźć na pulpicie nawigacyjnym, na liście Bazy danych SQL lub w grupie zasobów użytej podczas tworzenia. 

    ![Widok portalu](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL DataBase create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## Następne kroki

Po utworzeniu usługi SQL Data Warehouse można rozpocząć [nawiązywanie połączenia](./sql-data-warehouse-get-started-connect.md) i wykonywanie zapytań.

Aby załadować dane do usługi SQL Data Warehouse, zobacz [omówienie ładowania](./sql-data-warehouse-overview-load.md).

Jeśli próbujesz przeprowadzić migrację z istniejącej bazy danych do usługi SQL Data Warehouse, zobacz [omówienie migracji](./sql-data-warehouse-overview-migrate.md) lub użyj [narzędzia do migracji](./sql-data-warehouse-migrate-migration-utility.md).




<!--HONumber=Jun16_HO2-->


