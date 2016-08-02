<properties
   pageTitle="Tworzenie bazy danych w usłudze SQL Data Warehouse za pomocą programu Powershell | Microsoft Azure"
   description="Tworzenie bazy danych w usłudze SQL Data Warehouse za pomocą programu Powershell"
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
   ms.date="06/04/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Tworzenie bazy danych w usłudze SQL Data Warehouse przy użyciu programu Powershell

> [AZURE.SELECTOR]
- [Portalu Azure](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

## Wymagania wstępne
Przed rozpoczęciem upewnij się, że zostały spełnione następujące wymagania wstępne.

- **Konto platformy Azure**: aby utworzyć konto, zobacz tematy [Bezpłatna wersja próbna platformy Azure][] lub [Środki na korzystanie z systemu Azure w ramach usługi MSDN][].
- **Serwer SQL V12 platformy Azure**: zobacz tematy [Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu Portalu Azure][] lub [Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu programu PowerShell][].
- **Nazwa grupy zasobów**: użyj tej samej grupy zasobów, z której korzysta serwer SQL V12 platformy Azure, lub zobacz [grupy zasobów][], aby utworzyć nową grupę zasobów.
- Masz program **PowerShell w wersji 1.0.3 lub nowszej**: wersję można sprawdzić za pomocą polecenia **Get-Module -ListAvailable -Name Azure**.  Najnowszą wersję można zainstalować za pomocą [Instalatora platformy sieci Web firmy Microsoft][].  Aby uzyskać więcej informacji na temat instalowania najnowszej wersji, zobacz artykuł [How to install and configure Azure PowerShell][] (Instalowanie i konfigurowanie programu Azure PowerShell).

> [AZURE.NOTE] Utworzenie nowej usługi SQL Data Warehouse może skutkować nową usługą płatną.  Zobacz [Cennik usługi SQL Data Warehouse][], aby uzyskać więcej informacji o cenach.

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

4.  Utwórz bazę danych. W tym przykładzie tworzymy nową bazę danych o nazwie „mynewsqldw” z poziomem celu usługi „DW400” na serwerze o nazwie „sqldwserver1”, który znajduje się w grupie zasobów o nazwie „mywesteuroperesgp1”.

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse"
    ```

Wymaganymi parametrami tego polecenia cmdlet są:

- **RequestedServiceObjectiveName**: żądana wartość jednostek DWU w postaci "DWXXX". Jednostka DWU reprezentuje alokację procesora i pamięci.  Każda wartość DWU reprezentuje liniowy wzrost tych zasobów.  Obecnie obsługiwane są następujące wartości: 100, 200, 300, 400, 500, 600, 1000, 1200, 1500, 2000.
- **DatabaseName**: nazwa tworzonej usługi SQL Data Warehouse.
- **ServerName**: nazwa serwera używanego do tworzenia (musi być w wersji V12).
- **ResourceGroupName**: używana grupa zasobów.  Aby znaleźć grupy zasobów dostępne w ramach subskrypcji, użyj polecenia cmdlet Get-AzureResource.
- **Edition**: należy podać wartość „DataWarehouse”, aby utworzyć usługę SQL Data Warehouse.

Więcej informacji na temat opcji parametrów można znaleźć w artykule [CREATE DATABASE (Azure SQL Data Warehouse)][] (TWORZENIE BAZY DANYCH (Azure SQL Data Warehouse)).
Aby poznać opis polecenia, zobacz artykuł [New-AzureRmSqlDatabase][]

## Następne kroki
Po zakończeniu aprowizacji usługi SQL Data Warehouse warto [załadować przykładowe dane][] lub poznać sposoby wykonywania takich czynności, jak [opracowywanie][], [ładowanie][] czy [migracja][].

Jeśli bardziej interesujesz się programistycznym zarządzaniem usługą SQL Data Warehouse, zapoznaj się z artykułem dotyczącym [poleceń cmdlet programu PowerShell i interfejsów API REST][].

<!--Image references-->

<!--Article references-->
[migracja]: ./sql-data-warehouse-overview-migrate.md
[opracowywanie]: ./sql-data-warehouse-overview-develop.md
[ładowanie]: ./sql-data-warehouse-load-with-bcp.md
[załadować przykładowe dane]: ./sql-data-warehouse-get-started-manually-load-samples.md
[poleceń cmdlet programu PowerShell i interfejsów API REST]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[reguły zapory]: ./sql-database-configure-firewall-settings.md
[How to install and configure Azure PowerShell]: ../powershell/powershell-install-configure.md
[tworzenie usługi SQL Data Warehouse przy użyciu Portalu Azure]: ./sql-data-warehouse-get-started-provision.md
[Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu Portalu Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Tworzenie serwera logicznego usługi Azure SQL Database przy użyciu programu PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[grupy zasobów]: ../azure-portal/resource-group-portal.md

<!--MSDN references--> 
[MSDN]:https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[CREATE DATABASE (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Instalatora platformy sieci Web firmy Microsoft]: https://aka.ms/webpi-azps
[Cennik usługi SQL Data Warehouse]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Bezpłatna wersja próbna platformy Azure]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[Środki na korzystanie z systemu Azure w ramach usługi MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=Jun16_HO2-->


