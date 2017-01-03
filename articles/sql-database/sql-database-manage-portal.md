---
title: "Zarządzanie usługą SQL Azure Database za pomocą witryny Azure Portal | Dokumentacja firmy Microsoft"
description: "Krótki przewodnik stosowania witryny Azure Portal do zarządzania relacyjnymi bazami danych w chmurze za pomocą witryny Azure Portal."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 3a56e9de-c21a-40ba-9a35-958172cb4e5b
ms.service: sql-database
ms.custom: overview
ms.devlang: NA
ms.workload: data-management
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.date: 09/19/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 420b2153f6115dd712d3033e30f11f79b18cd80f
ms.openlocfilehash: be89a2799af3bdc2938f73e3d54f00f81d9ab9cd


---
# <a name="manage-azure-sql-databases-using-the-azure-portal"></a>Zarządzanie usługą Azure SQL Database za pomocą witryny Azure Portal
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-manage-portal.md)
> * [SSMS](sql-database-manage-azure-ssms.md)
> * [PowerShell](sql-database-manage-powershell.md)
> 
> 

Witryna [Azure Portal](https://portal.azure.com/) służy do tworzenia serwerów i usług Azure SQL Database, ich monitorowania i zarządzania nimi. Ten artykuł zawiera krótki opis typowych zadań i linki do szczegółowych informacji o nich.

> [!TIP]
> Samouczek z omówieniem tworzenia serwera i opartej na serwerze zapory, wyświetlania właściwości serwera, nawiązywania połączenia przy użyciu programu SQL Server Management Studio, odpytywania głównej bazy danych, tworzenia przykładowej i pustej bazy danych, tworzenia zapytań o właściwości bazy danych, nawiązywania połączenia przy użyciu programu SQL Server Management Studio, a także odpytywania przykładowej bazy danych, znajduje się w artykule [Samouczek z wprowadzeniem](sql-database-get-started.md).

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Wyświetlanie baz danych Azure SQL, serwerów i pul
Aby wyświetlić dostępne usługi bazy danych SQL Database, kliknij przycisk **Więcej usług** i wpisz **SQL** w polu wyszukiwania:

![SQL Database](./media/sql-database-manage-portal/sql-services.png)

## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Jak utworzyć lub wyświetlić bazy danych Azure SQL
Aby otworzyć blok **baz danych SQL**, kliknij przycisk **Bazy danych SQL**, a następnie kliknij bazę danych, z którą chcesz pracować, lub kliknij przycisk **+ Dodaj** w celu utworzenia bazy danych SQL. Aby uzyskać szczegółowe informacje, zobacz artykuł [Wprowadzenie do serwerów, baz danych i reguł zapory usługi Azure SQL Database przy użyciu witryny Azure Portal i programu SQL Server Management Studio](sql-database-get-started.md).

![Bazy danych SQL](./media/sql-database-manage-portal/sql-databases.png)

## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Jak utworzyć lub wyświetlić serwery Azure SQL
Aby otworzyć blok **serwerów SQL**, kliknij przycisk **Serwery SQL**, a następnie kliknij serwer, z którym chcesz pracować, lub kliknij przycisk **+ Dodaj** w celu utworzenia serwera SQL. Aby uzyskać szczegółowe informacje, zobacz artykuł [Wprowadzenie do serwerów, baz danych i reguł zapory usługi Azure SQL Database przy użyciu witryny Azure Portal i programu SQL Server Management Studio](sql-database-get-started.md).

![Serwery SQL](./media/sql-database-manage-portal/sql-servers.png)

## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Jak utworzyć lub wyświetlić pule elastyczne SQL
Aby otworzyć blok **pul elastycznych SQL**, kliknij przycisk **Pule elastyczne SQL**, a następnie kliknij pulę, z którą chcesz pracować, lub kliknij przycisk **+ Dodaj** w celu utworzenia puli. Aby uzyskać szczegółowe informacje, zobacz artykuł [Tworzenie nowej elastycznej puli baz danych za pomocą witryny Azure Portal](sql-database-elastic-pool-create-portal.md).

![Pule elastyczne SQL](./media/sql-database-manage-portal/elastic-pools.png)

## <a name="how-do-i-update-or-view-sql-database-settings"></a>Jak zaktualizować lub wyświetlić ustawienia bazy danych SQL
Aby wyświetlić lub zaktualizować ustawienia bazy danych, kliknij odpowiednie ustawienie w bloku bazy danych SQL:

![Ustawienia bazy danych SQL](./media/sql-database-manage-portal/settings.png)

## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Jak znaleźć w pełni kwalifikowaną nazwę serwera baz danych SQL
Aby wyświetlić nazwę serwera baz danych, kliknij przycisk **Przegląd** w bloku **bazy danych SQL** i zanotuj nazwę serwera:

![Ustawienia bazy danych SQL](./media/sql-database-manage-portal/server-name.png)

## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Jak zarządzać regułami zapory w celu kontrolowania dostępu do serwera SQL i bazy danych
Aby wyświetlić, utworzyć lub zaktualizować reguły zapory, kliknij przycisk **Ustaw zaporę serwera** w bloku **bazy danych SQL**. Aby uzyskać szczegółowe informacje, zobacz artykuł [Tworzenie reguł zapory na poziomie serwera usługi Azure SQL Database i zarządzanie nimi za pomocą witryny Azure Portal](sql-database-configure-firewall-settings.md).

![reguły zapory](./media/sql-database-manage-portal/sql-database-firewall.png)

## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Jak zmienić warstwę usług lub poziom wydajności bazy danych SQL
Aby zaktualizować warstwę usług lub poziom wydajności bazy danych SQL, kliknij przycisk **Warstwa cenowa (jednostki DTU skalowania)** w bloku **bazy danych SQL**. Aby uzyskać szczegółowe informacje, zobacz artykuł [Change the service tier and performance level (pricing tier) of a SQL database](sql-database-scale-up.md) (Zmiana warstwy usług i poziomu wydajności (warstwy cenowej) bazy danych SQL).

![warstwy cenowe](./media/sql-database-manage-portal/pricing-tier.png)

## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Jak skonfigurować inspekcję i wykrywanie zagrożeń dla bazy danych SQL
Aby skonfigurować inspekcję i wykrywanie zagrożeń dla bazy danych SQL, kliknij przycisk **Inspekcja i wykrywanie i zagrożeń** w bloku **bazy danych SQL**. Aby uzyskać szczegółowe informacje, zobacz artykuły [Get started with SQL database auditing](sql-database-auditing-get-started.md) (Rozpoczynanie prazy z inspekcją bazy danych SQL) i [Get started with SQL Database Threat Detection](sql-database-threat-detection-get-started.md) (Rozpoczynanie pracy z wykrywaniem zagrożeń bazy danych SQL Database).

## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Jak skonfigurować dynamiczne maskowanie danych dla bazy danych SQL
Aby skonfigurować dynamiczne maskowanie danych dla bazy danych SQL, kliknij przycisk **Dynamiczne maskowanie danych** w bloku **bazy danych SQL**. Aby uzyskać szczegółowe informacje, zobacz [Get started with SQL Database Dynamic Data Masking](sql-database-dynamic-data-masking-get-started.md) (Rozpoczynanie pracy z dynamicznym maskowaniem danych w usłudze SQL Database).

## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Jak skonfigurować przezroczyste szyfrowanie danych (TDE) dla bazy danych SQL
Aby skonfigurować przezroczyste szyfrowanie danych dla bazy danych SQL, kliknij przycisk **Przezroczyste szyfrowanie danych** w bloku **bazy danych SQL**. Aby uzyskać szczegółowe informacje, zobacz sekcję [Enable TDE on a database using the portal](https://msdn.microsoft.com/library/dn948096#Anchor_1) (Włączanie funkcji TDE w bazie danych za pomocą portalu).

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Jak wyświetlić lub zmienić maksymalny rozmiar bazy danych SQL
Aby wyświetlić lub zmienić rozmiar bazy danych SQL, kliknij przycisk **Rozmiar bazy danych** w bloku **bazy danych SQL**. Zaktualizuj maksymalny rozmiar bazy danych, zmieniając warstwę usług lub poziom wydajności. Aby uzyskać szczegółowe informacje, zobacz artykuł [Change the service tier and performance level (pricing tier) of a SQL database](sql-database-scale-up.md) (Zmiana warstwy usług i poziomu wydajności (warstwy cenowej) bazy danych SQL).

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Jak monitorować i poprawić wydajność bazy danych SQL
Aby monitorować i poprawić charakterystyki wydajności bazy danych SQL, kliknij przycisk **Przegląd wydajności** w bloku **bazy danych SQL**. Aby uzyskać szczegółowe informacje, zobacz artykuł [SQL Database Performance Insight](sql-database-performance.md) (Szczegółowe informacje o wydajności usługi SQL Database).

## <a name="how-do-i-configure-geo-replication"></a>Jak skonfigurować replikację geograficzną
Aby skonfigurować replikację geograficzną dla bazy danych SQL, kliknij przycisk **Replikacja geograficzna** w bloku **bazy danych SQL**. Aby uzyskać szczegółowe informacje, zobacz artykuł [Configure Geo-Replication for Azure SQL Database with the Azure portal](sql-database-geo-replication-portal.md) (Konfigurowanie replikacji geograficznej na potrzeby usługi Azure SQL Database za pomocą witryny Azure Portal).

## <a name="how-do-i-fail-over-to-a-geo-replicated-sql-database"></a>Jak w przypadku awarii przełączyć się do bazy danych SQL replikowanej geograficznie
Aby w przypadku awarii przełączyć się do pomocniczej bazy danych replikowanej geograficznie, kliknij przycisk **Replikacja geograficzna** w bloku **bazy danych SQL**, a następnie kliknij przycisk **Praca awaryjna**. Aby uzyskać szczegółowe informacje, zobacz artykuł [Initiate a planned or unplanned failover for Azure SQL Database with the Azure portal](sql-database-geo-replication-failover-portal.md) (Inicjowanie planowanego lub nieplanowanego przejścia w tryb failover na potrzeby usługi Azure SQL Database za pomocą witryny Azure Portal).

## <a name="how-do-i-copy-a-sql-database"></a>Jak skopiować bazę danych SQL
Aby skopiować bazę danych SQL, kliknij przycisk **Kopiuj** w bloku **bazy danych SQL**. Aby uzyskać szczegółowe informacje, zobacz artykuł [Copy an Azure SQL database using the Azure portal](sql-database-copy-portal.md) (Kopiowanie bazy danych Azure SQL Database za pomocą witryny Azure Portal).

![Ustawienia bazy danych SQL](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Jak archiwizować bazę danych Azure SQL do pliku BACPAC
Aby z bazy danych SQL utworzyć plik BACPAC, kliknij przycisk **Eksportuj** w bloku **bazy danych SQL**. Aby uzyskać szczegółowe informacje, zobacz artykuł [Archive an Azure SQL database to a BACPAC file using the Azure portal](sql-database-export.md) (Archiwizacja bazy danych Azure SQL do pliku BACPAC za pomocą witryny Azure Portal).

![Eksport bazy danych SQL](./media/sql-database-manage-portal/sql-database-export.png)

## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Jak przywrócić bazę danych SQL do wcześniejszego punktu w czasie
Aby przywrócić bazę danych SQL, kliknij przycisk **Przywróć** w bloku **bazy danych SQL**. Aby uzyskać szczegółowe informacje, zobacz artykuł [Restore an Azure SQL Database to a previous point in time with the Azure portal](sql-database-point-in-time-restore.md) (Odzyskiwanie bazy danych Azure SQL Database do wcześniejszego punktu w czasie za pomocą witryny Azure Portal).

![Ustawienia bazy danych SQL](./media/sql-database-manage-portal/sql-database-restore.png)

## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Jak utworzyć bazę danych Azure SQL z pliku BACPAC
Aby utworzyć bazę danych SQL z pliku BACPAC, kliknij przycisk **Importuj bazę danych** w bloku **serwera SQL**. Aby uzyskać szczegółowe informacje, zobacz artykuł [Import a BACPAC file to create an Azure SQL database](sql-database-import.md) (Importowanie pliku BACPAC w celu utworzenia bazy danych Azure SQL).

![Serwer SQL](./media/sql-database-manage-portal/server-commands.png)

## <a name="how-do-i-restore-a-deleted-sql-database"></a>Jak przywrócić usuniętą bazę danych SQL
Aby przywrócić usuniętą bazę danych SQL, kliknij przycisk **Usunięte bazy danych** w bloku **serwera SQL**, który zawierał usuniętą bazę danych. Aby uzyskać szczegółowe informacje, zobacz artykuł [Restore a deleted Azure SQL database using the Azure portal](sql-database-restore-deleted-database-portal.md) (Odzyskiwanie usuniętej bazy danych Azure SQL za pomocą witryny Azure Portal).

## <a name="how-do-i-delete-a-sql-database"></a>Jak usunąć bazę danych SQL
Aby usunąć bazę danych SQL, kliknij przycisk **Usuń** w bloku **bazy danych SQL**. 

![Ustawienia bazy danych SQL](./media/sql-database-manage-portal/sql-database-delete.png)

## <a name="additional-resources"></a>Dodatkowe zasoby
* [SQL Database](sql-database-technical-overview.md)
* [Monitorowanie i zarządzanie pulą elastyczną za pomocą witryny Azure Portal](sql-database-elastic-pool-manage-portal.md)




<!--HONumber=Dec16_HO5-->


