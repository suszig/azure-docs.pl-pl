---
title: "Zarządzanie moc obliczeniową w usłudze Azure SQL Data Warehouse (PowerShell) | Dokumentacja firmy Microsoft"
description: "Zadania programu PowerShell do zarządzania mocy obliczeniowej. Zasoby obliczeniowe skali przez dostosowanie wartości dwu. Wstrzymać lub wznowić zasobów obliczeniowych w celu ograniczenia kosztów."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 8354a3c1-4e04-4809-933f-db414a8c74dc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 6a185d96447c2e1b0b463439dd062081e783da5f
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Zarządzanie moc obliczeniową w usłudze Azure SQL Data Warehouse (PowerShell)
> [!div class="op_single_selector"]
> * [Omówienie](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

## <a name="before-you-begin"></a>Przed rozpoczęciem
### <a name="install-the-latest-version-of-azure-powershell"></a>Zainstaluj najnowszą wersję programu Azure PowerShell
> [!NOTE]
> Przy użyciu programu Azure PowerShell z usługą Magazyn danych SQL, potrzebujesz programu Azure PowerShell w wersji 1.0.3 lub nowszej.  Aby sprawdzić wersji bieżącej, uruchom polecenie **Get-Module - ListAvailable-Name Azure**. Można zainstalować najnowszą wersję ze [Instalatora platformy sieci Web firmy Microsoft][Microsoft Web Platform Installer].  Aby uzyskać więcej informacji, zobacz [jak instalowanie i konfigurowanie programu Azure PowerShell][How to install and configure Azure PowerShell].
>
> 

### <a name="get-started-with-azure-powershell-cmdlets"></a>Wprowadzenie do poleceń cmdlet programu Azure PowerShell
Aby rozpocząć:

1. Otwórz program Azure PowerShell.
2. W wierszu polecenia programu PowerShell, uruchom następujące polecenia, aby zarejestrować się w usłudze Azure Resource Manager i wyboru subskrypcji.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Moc obliczeniową skali
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Aby zmienić liczbę jednostek dwu, użyj [Set-AzureRmSqlDatabase] [ Set-AzureRmSqlDatabase] polecenia cmdlet programu PowerShell. Poniższy przykład ustawia cel poziomu usługi DW1000 MySQLDW, która jest hostowana na serwerze MyServer bazy danych.

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Wstrzymaj obliczeń
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Aby wstrzymać bazy danych, należy użyć [Suspend-AzureRmSqlDatabase] [ Suspend-AzureRmSqlDatabase] polecenia cmdlet. W następującym przykładzie wstrzymano bazy danych o nazwie Database02 znajdującej się na serwerze o nazwie Serwer01. Serwer jest w grupie zasobów platformy Azure o nazwie ResourceGroup1.

> [!NOTE]
> Należy pamiętać, że jeśli serwer jest foo.database.windows.net, użyj "foo" jako ServerName — poleceń cmdlet programu PowerShell.
>
> 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
Zmiany, w tym przykładzie dalej pobiera bazy danych do obiektu $database. Następnie przewody obiektu [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]. Wyniki są przechowywane w resultDatabase obiektu. Polecenia końcowego pokazuje wyniki.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Wznów obliczeń
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Aby utworzyć bazę danych, należy użyć [Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] polecenia cmdlet. W następującym przykładzie uruchomiono bazę danych o nazwie Database02 znajdującej się na serwerze o nazwie Serwer01. Serwer jest w grupie zasobów platformy Azure o nazwie ResourceGroup1.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

Zmiany, w tym przykładzie dalej pobiera bazy danych do obiektu $database. Następnie przewody obiektu [Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] i przechowuje wyniki w $resultDatabase. Polecenia końcowego pokazuje wyniki.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state"></a>Sprawdź stan bazy danych

Jak pokazano w powyższych przykładach, można użyć jednego [Get-AzureRmSqlDatabase] [ Get-AzureRmSqlDatabase] polecenia cmdlet, aby uzyskać informacje o bazie danych, co sprawdzanie stanu, ale również użyć jako argumentu. 

```powershell
Get-AzureRmSqlDatabase [-ResourceGroupName] <String> [-ServerName] <String> [[-DatabaseName] <String>]
 [-InformationAction <ActionPreference>] [-InformationVariable <String>] [-Confirm] [-WhatIf]
 [<CommonParameters>]
```

Które będą powodować coś, takich jak 

```powershell   
ResourceGroupName             : nytrg
ServerName                    : nytsvr
DatabaseName                  : nytdb
Location                      : West US
DatabaseId                    : 86461aae-8e3d-4ded-9389-ac9d4bc69bbb
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1General_CP1CI_AS
CatalogCollation              :
MaxSizeBytes                  : 32212254720
Status                        : Online
CreationDate                  : 10/26/2016 4:33:14 PM
CurrentServiceObjectiveId     : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
CurrentServiceObjectiveName   : System2
RequestedServiceObjectiveId   : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           : 1/1/0001 12:00:00 AM
```

Gdy użytkownik może następnie sprawdź *stan* bazy danych. W takim przypadku widać, że ta baza danych jest w trybie online. 

Po uruchomieniu tego polecenia, powinien zostać wyświetlony stan wartość albo Online, wstrzymywanie, wznawianie, skalowanie i wstrzymana.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Następne kroki
Inne zadania zarządzania, zobacz [omówienie zarządzania][Management overview].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Get-AzureRmSqlDatabase]: /powershell/servicemanagement/azure.sqldatabase/v1.6.1/get-azuresqldatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
