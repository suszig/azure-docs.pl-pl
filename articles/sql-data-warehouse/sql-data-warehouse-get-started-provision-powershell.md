<properties
   pageTitle="Tworzenie bazy danych w usłudze SQL Data Warehouse za pomocą programu PowerShell | Microsoft Azure"
   description="Tworzenie bazy danych w usłudze SQL Data Warehouse za pomocą programu PowerShell"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Tworzenie bazy danych w usłudze SQL Data Warehouse przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Azure Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

W tym artykule przedstawiono, jak utworzyć bazę danych w usłudze SQL Data Warehouse przy użyciu programu PowerShell.

## Wymagania wstępne

Aby rozpocząć pracę, potrzebne będą następujące elementy:

- **Konto platformy Azure**: Aby utworzyć konto, odwiedź witrynę [Bezpłatna wersja próbna platformy Azure][] lub [Środki na korzystanie z systemu Azure w ramach usługi MSDN][].
- **Serwer Azure SQL**: Aby uzyskać więcej informacji, zobacz temat [Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu witryny Azure Portal][] lub [Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu programu PowerShell][].
- **Grupa zasobów**: Użyj tej samej grupy zasobów, z której korzysta serwer Azure SQL, lub zobacz, [jak utworzyć grupę zasobów][].
- Masz program **PowerShell w wersji 1.0.3 lub nowszej**: wersję można sprawdzić za pomocą polecenia **Get-Module -ListAvailable -Name Azure**.  Najnowszą wersję można zainstalować za pomocą [Instalatora platformy Microsoft Web][].  Aby uzyskać więcej informacji na temat instalowania najnowszej wersji, zobacz artykuł [How to install and configure Azure PowerShell][] (Instalowanie i konfigurowanie programu Azure PowerShell).

> [AZURE.NOTE] Utworzenie bazy danych w usłudze SQL Data Warehouse może skutkować powstaniem nowej usługi płatnej.  Zobacz [Cennik usługi SQL Data Warehouse][], aby uzyskać więcej informacji o cenach.

## Tworzenie bazy danych w usłudze SQL Data Warehouse

1. Otwórz program Windows PowerShell.
2. Uruchom to polecenie cmdlet, aby zalogować się do usługi Azure Resource Manager.

    ```Powershell
    Login-AzureRmAccount
    ```
    
3. Wybierz subskrypcję, której chcesz użyć dla bieżącej sesji.

    ```Powershell
    Get-AzureRmSubscription -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```

4.  Utwórz bazę danych. W tym przykładzie tworzymy bazę danych o nazwie „mynewsqldw” z poziomem celu usługi „DW400” na serwerze o nazwie „sqldwserver1”, który znajduje się w grupie zasobów o nazwie „mywesteuroperesgp1”.

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
    ```

Wymagane parametry:

- **RequestedServiceObjectiveName**: żądana wartość jednostek [DWU][].  Obsługiwane są następujące wartości: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 i DW6000.
- **DatabaseName**: nazwa tworzonej usługi SQL Data Warehouse.
- **ServerName**: nazwa serwera używanego do tworzenia (musi być w wersji V12).
- **ResourceGroupName**: używana grupa zasobów.  Aby znaleźć grupy zasobów dostępne w ramach subskrypcji, użyj polecenia cmdlet Get-AzureResource.
- **Edition**: musi mieć wartość „DataWarehouse”, aby utworzyć magazyn SQL Data Warehouse.

Opcjonalne parametry:

- **CollationName**: sortowanie domyślne, gdy sortowanie nie jest określone, to COLLATE SQL_Latin1_General_CP1_CI_AS.  Nie można zmienić sortowania bazy danych.
- **MaxSizeBytes**: domyślny maksymalny rozmiar bazy danych to 10 GB.


Więcej informacji na temat opcji parametrów można znaleźć w artykule [CREATE DATABASE (Azure SQL Data Warehouse)][] (Tworzenie bazy danych — Azure SQL Data Warehouse) i artykule dotyczącym polecenia [New-AzureRmSqlDatabase][].

## Następne kroki

Po zakończeniu aprowizacji usługi SQL Data Warehouse warto [załadować przykładowe dane][] lub poznać sposoby wykonywania takich czynności, jak [opracowywanie][], [ładowanie][] czy [migrowanie][].

Jeśli bardziej interesujesz się programistycznym zarządzaniem usługą SQL Data Warehouse, zapoznaj się z artykułem dotyczącym [poleceń cmdlet programu PowerShell i interfejsów API REST][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[migrowanie]: ./sql-data-warehouse-overview-migrate.md
[opracowywanie]: ./sql-data-warehouse-overview-develop.md
[ładowanie]: ./sql-data-warehouse-load-with-bcp.md
[załadować przykładowe dane]: ./sql-data-warehouse-load-sample-databases.md
[poleceń cmdlet programu PowerShell i interfejsów API REST]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[reguły zapory]: ../sql-database-configure-firewall-settings.md

[How to install and configure Azure PowerShell]: ../powershell/powershell-install-configure.md
[tworzenie usługi SQL Data Warehouse przy użyciu Portalu Azure]: ./sql-data-warehouse-get-started-provision.md
[Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu witryny Azure Portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu programu PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[jak utworzyć grupę zasobów]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[CREATE DATABASE (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Instalatora platformy Microsoft Web]: https://aka.ms/webpi-azps
[Cennik usługi SQL Data Warehouse]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Bezpłatna wersja próbna platformy Azure]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[Środki na korzystanie z systemu Azure w ramach usługi MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=sep16_HO1-->


