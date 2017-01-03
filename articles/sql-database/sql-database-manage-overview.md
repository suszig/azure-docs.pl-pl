---
title: "Przegląd: narzędzia do zarządzania dla usługi SQL Database | Microsoft Docs"
description: "Porównanie narzędzi i opcji zarządzania usługą Azure SQL Database"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 37767380-975f-4dee-a28d-80bc2036dda3
ms.service: sql-database
ms.custom: overview
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 73cc9d1d02ed915466f0eac2d9a42fe8cf9e2fb9
ms.openlocfilehash: 32d0a1fd8671bbd9d450711dd686ca49c2178c16


---
# <a name="overview-management-tools-for-sql-database"></a>Przegląd: narzędzia do zarządzania dla usługi SQL Database
W tym temacie omówiono i porównano narzędzia oraz opcje zarządzania bazami danych Azure SQL, co pozwoli Ci wybrać narzędzie odpowiednie pod kątem danego zadania, firmy i własnych potrzeb. Wybór właściwego narzędzia zależy od liczby baz danych, którymi zarządzasz, zadania oraz częstotliwości jego wykonywania.

## <a name="azure-portal"></a>Portal Azure
[Azure Portal](https://portal.azure.com) to oparta na sieci Web aplikacja umożliwiająca tworzenie, aktualizowanie i usuwanie baz danych oraz serwerów logicznych, a także monitorowanie działań w bazie danych. To doskonałe narzędzie, jeśli jesteś początkującym użytkownikiem platformy Azure, zarządzasz kilkoma bazami danych lub chcesz szybko wykonać jakąś czynność.

Aby uzyskać dodatkowe informacje dotyczące korzystania z portalu, zobacz temat [Manage SQL Databases using the Azure portal](sql-database-manage-portal.md) (Zarządzanie bazami danych SQL Database przy użyciu witryny Azure Portal).

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>Program SQL Server Management Studio i narzędzia SQL Server Data Tools w programie Visual Studio
SQL Server Management Studio (SSMS) oraz SQL Server Data Tools (SSDT) to narzędzia klienckie uruchamiane na komputerze w celu opracowywania bazy danych w chmurze i zarządzania nią. Jeśli jesteś twórcą aplikacji zaznajomionym z programem Visual Studio lub innymi zintegrowanymi środowiskami projektowymi (IDE), [wypróbuj używanie narzędzi SSDT w programie Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). Wielu administratorów baz danych zna program SSMS, którego można używać z bazami danych Azure SQL. [Pobierz najnowszą wersję programu SSMS](https://msdn.microsoft.com/library/mt238290) i zawsze używaj najnowszej wersji podczas pracy z usługą Azure SQL Database. Aby uzyskać więcej informacji na temat zarządzania bazami danych SQL Azure Database za pomocą programu SSMS, zobacz temat [Manage SQL Databases using SSMS](sql-database-manage-azure-ssms.md) (Zarządzanie bazami danych SQL Database przy użyciu programu SSMS).

> [!IMPORTANT]
> Zawsze używaj najnowszej wersji programu [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290) i narzędzi [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx), aby zachować synchronizację z aktualizacjami platformy Microsoft Azure i usługi SQL Database.
>  

## <a name="powershell"></a>PowerShell
Za pomocą programu PowerShell można zarządzać bazami danych i pulami elastycznymi, a także automatyzować wdrożenia zasobów platformy Azure. Firma Microsoft poleca to narzędzie na potrzeby zarządzania dużą liczbą baz danych i automatyzacji wdrożeń oraz zmian zasobów w środowisku produkcyjnym.

Aby uzyskać więcej informacji, zobacz temat [Zarządzanie usługą SQL Database za pomocą programu PowerShell](sql-database-manage-powershell.md)

## <a name="elastic-database-tools"></a>Narzędzia elastycznych baz danych
Za pomocą narzędzi elastycznych baz danych można wykonywać następujące działania: 

* Wykonywanie skryptu T-SQL w odniesieniu do zestawu baz danych przy użyciu [zadania elastycznego](sql-database-elastic-jobs-overview.md)
* Przenoszenie baz danych z modelu wielu dzierżaw do modelu jednej dzierżawy za pomocą [narzędzia do dzielenia i scalania](sql-database-elastic-scale-overview-split-and-merge.md)
* Zarządzanie bazami danych w modelu jednej dzierżawy lub wielu dzierżaw przy użyciu [biblioteki klienckiej skalowania elastycznego](sql-database-elastic-database-client-library.md).

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
* [Azure Automation](https://azure.microsoft.com/documentation/services/automation/)
* [Azure Scheduler](https://azure.microsoft.com/documentation/services/scheduler/)




<!--HONumber=Dec16_HO5-->


