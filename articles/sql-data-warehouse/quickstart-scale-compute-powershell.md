---
title: 'Szybki Start: Skalowanie obliczeniowych w magazynie danych SQL Azure - PowerShell | Dokumentacja firmy Microsoft'
description: "Zadania programu PowerShell do skalowania w poziomie zasoby obliczeniowe przez dostosowanie wartości właściwości jednostki magazynu danych."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/31/2018
ms.author: elbutter;barbkess
ms.openlocfilehash: a3a435d6bdb0d35c96349540d5e9f9b5be61bd9b
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-powershell"></a>Szybki Start: Skali obliczeń w usłudze Azure SQL Data Warehouse w programie PowerShell

Skalowanie możliwości obliczeniowych w magazynie danych SQL Azure w programie PowerShell. [Skalowanie w poziomie obliczeń](sql-data-warehouse-manage-compute-overview.md) dla lepszą wydajność i skalę kopii obliczeń w celu ograniczenia kosztów. 

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

Ten samouczek wymaga programu Azure PowerShell w wersji modułu 5.1.1 lub nowszym. Uruchom `Get-Module -ListAvailable AzureRM` można znaleźć wersji masz obecnie. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps.md). 

## <a name="before-you-begin"></a>Przed rozpoczęciem

Tego przewodnika Szybki Start założono, że masz już magazyn danych SQL, który można skalować. Aby go utworzyć, należy użyć [tworzenie i Connect - portal](create-data-warehouse-portal.md) można utworzyć magazynu danych, nazywane **mySampleDataWarehouse**. 

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```powershell
Add-AzureRmAccount
```

Aby wyświetlić subskrypcji, której używasz, uruchom [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

```powershell
Get-AzureRmSubscription
```

Jeśli musisz użyć innej subskrypcji niż domyślna, uruchom [Select-AzureRmSubscription](/powershell/module/azurerm.profile/select-azurermsubscription).

```powershell
Select-AzureRmSubscription -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>Wyszukiwanie danych magazynu danych

Znajdź nazwę bazy danych, nazwę serwera i grupy zasobów dla magazynu danych, które mają być wstrzymywać i wznawiać. 

Wykonaj następujące kroki, aby znaleźć informacje o lokalizacji dla magazynu danych.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
2. Kliknij przycisk **baz danych** w po lewej stronie portalu Azure.
3. Wybierz **mySampleDataWarehouse** z **baz danych SQL** strony. Spowoduje to otwarcie magazynu danych. 

    ![Nazwa i zasobów grupy serwerów](media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

4. Zanotuj nazwę magazynu danych, która będzie używana jako nazwa bazy danych. Również Zanotuj nazwę serwera i grupy zasobów. Będzie używane w wstrzymanie i wznowić poleceń.
5. Jeśli serwer jest foo.database.windows.net, należy użyć pierwszej części jako nazwę serwera, polecenia cmdlet programu PowerShell. Na poprzedniej ilustracji pełną nazwę serwera jest NowySerwer 20171113.database.windows.net. Używamy **20171113 NowySerwer** jako nazwę serwera w poleceniu cmdlet programu PowerShell.

## <a name="scale-compute"></a>Skalowanie zasobów obliczeniowych

W usłudze SQL Data Warehouse można zwiększyć lub zmniejszyć zasoby obliczeniowe przez dostosowanie wartości właściwości jednostki magazynu danych. [Tworzenie i Connect - portal](create-data-warehouse-portal.md) utworzony **mySampleDataWarehouse** i zainicjować go od 400 jednostek dwu. Poniższe kroki Dostosuj liczbę jednostek dwu dla **mySampleDataWarehouse**.

Aby zmienić jednostki magazynu danych, użyj [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) polecenia cmdlet programu PowerShell. Poniższy przykład przedstawia jednostki magazynu danych DW300 dla bazy danych **mySampleDataWarehouse** którym znajduje się w grupie zasobów **myResourceGroup** na serwerze  **mynewserver 20171113**.

```Powershell
Set-AzureRmSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300"
```

## <a name="check-database-state"></a>Sprawdź stan bazy danych

Aby wyświetlić bieżący stan magazynu danych, użyj [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) polecenia cmdlet programu PowerShell. To pobiera stan **mySampleDataWarehouse** bazy danych w grupie zasobów **myResourceGroup** i serwer **mynewserver 20171113.database.windows.net**.

```powershell
Get-AzureRmSqlDatabase -ResourceGroupName myResourceGroup -ServerName mynewserver-20171113 -DatabaseName mySampleDataWarehouse
```

Czego skutkiem będzie podobny do następującego:

```powershell   
ResourceGroupName             : myResourceGroup
ServerName                    : mynewserver-20171113
DatabaseName                  : mySampleDataWarehouse
Location                      : North Europe
DatabaseId                    : 34d2ffb8-b70a-40b2-b4f9-b0a39833c974
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1_General_CP1_CI_AS
CatalogCollation              :
MaxSizeBytes                  : 263882790666240
Status                        : Online
CreationDate                  : 11/20/2017 9:18:12 PM
CurrentServiceObjectiveId     : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
CurrentServiceObjectiveName   : DW300
RequestedServiceObjectiveId   : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           :
Tags                          :
ResourceId                    : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/
                                resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/mynewserver-20171113/databases/mySampleDataWarehouse
CreateMode                    :
ReadScale                     : Disabled
ZoneRedundant                 : False
```

Następnie można sprawdzić, zobacz **stan** bazy danych. W takim przypadku widać, że ta baza danych jest w trybie online.  Po uruchomieniu tego polecenia powinien otrzymać wartość stanu Online, wstrzymywanie, wznawianie, skalowanie lub wstrzymana.

## <a name="next-steps"></a>Kolejne kroki
Teraz uzyskanych jak skalować obliczeń do magazynu danych. Aby dowiedzieć się więcej na temat usługi Azure SQL Data Warehouse, przejdź do samouczka na temat ładowania danych.

> [!div class="nextstepaction"]
>[Ładowanie danych do magazynu danych SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
