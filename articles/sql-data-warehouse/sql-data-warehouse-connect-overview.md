<properties
   pageTitle="Nawiązywanie połączenia z usługą Azure SQL Data Warehouse | Microsoft Azure"
   description="Przegląd połączenia do łączenia się z usługą Azure SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/27/2016"
   ms.author="sonyama;barbkess"/>

# Nawiązywanie połączenia z usługą Azure SQL Data Warehouse

> [AZURE.SELECTOR]
- [Omówienie](sql-data-warehouse-connect-overview.md)
- [Authentication](sql-data-warehouse-authentication.md)
- [Sterowniki](sql-data-warehouse-connection-strings.md)

Przegląd nawiązywania połączenia z usługą Azure SQL Data Warehouse. 

## Odnajdywanie parametrów połączenia z poziomu portalu

Usługa SQL Data Warehouse jest skojarzona z serwerem Azure SQL. Do nawiązania połączenia potrzebna jest w pełni kwalifikowana nazwa serwera.  Przykład: **myserver**.database.windows.net.

Aby znaleźć w pełni kwalifikowaną nazwę serwera:

1. Przejdź do witryny [Azure Portal][].
2. Kliknij przycisk **Bazy danych SQL** i kliknij bazę danych, z którą chcesz się połączyć. W tym przykładzie użyto przykładowej bazy danych AdventureWorksDW.
3. Znajdź pełną nazwę serwera.

    ![Pełna nazwa serwera][1]

## Ustawienia połączenia

Usługa SQL Data Warehouse standaryzuje kilka ustawień podczas tworzenia obiektu i połączenia. Nie można ich przesłonić.

| Ustawienia bazy danych       | Wartość                        |
| :--------------------- | :--------------------------- |
| [ANSI_NULLS][]         | ON                           |
| [QUOTED_IDENTIFIERS][] | ON                           |
| [DATEFORMAT][]         | mdy                          |
| [DATEFIRST][]          | 7                            |

## Monitorowanie połączeń i zapytań

Po nawiązaniu połączenia i ustanowieniu sesji możesz pisać i przesyłać zapytania do usługi SQL Data Warehouse.  Aby dowiedzieć się, jak monitorować sesje i zapytania, zobacz [Monitor your workload using DMVs][] (Monitorowanie obciążenia przy użyciu widoków DMV).

## Następne kroki

Aby rozpocząć tworzenie zapytań względem magazynu danych przy użyciu programu Visual Studio i innych aplikacji, zobacz artykuł [Query with Visual Studio][] (Wykonywanie zapytań przy użyciu programu Visual Studio). 

<!--Articles-->
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Monitor your workload using DMVs]: ./sql-data-warehouse-manage-monitor.md

<!--MSDN references-->
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATEFORMAT]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure Portal]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png





<!--HONumber=sep16_HO1-->


