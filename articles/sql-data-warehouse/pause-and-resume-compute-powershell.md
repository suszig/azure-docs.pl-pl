---
title: "Szybki Start: Wstrzymywanie i wznawianie obliczeń w Azure SQL Data Warehouse - PowerShell | Dokumentacja firmy Microsoft"
description: "Zadania programu PowerShell, które wstrzymać obliczeniowe dla magazynu danych SQL Azure w celu ograniczenia kosztów. Wznowić operacje obliczeniowe, gdy wszystko będzie gotowe do użycia w magazynie danych."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/25/2018
ms.author: barbkess
ms.openlocfilehash: 799210366978c68a390fa6d671184e94cf021301
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="quickstart-pause-and-resume-compute-for-an-azure-sql-data-warehouse-in-powershell"></a>Szybki Start: Wstrzymywanie i wznawianie obliczeniowe dla usługi Azure SQL Data Warehouse w programie PowerShell
Za pomocą programu PowerShell wstrzymać obliczeniowe dla magazynu danych SQL Azure w celu ograniczenia kosztów. Wznowić operacje obliczeniowe, gdy wszystko będzie gotowe do użycia w magazynie danych.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

Ten samouczek wymaga programu Azure PowerShell w wersji modułu 5.1.1 lub nowszym. Uruchom ` Get-Module -ListAvailable AzureRM` można znaleźć wersji masz obecnie. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps.md). 

## <a name="before-you-begin"></a>Przed rozpoczęciem

Tego przewodnika Szybki Start założono, że masz już magazyn danych SQL, który można wstrzymywać i wznawiać. Jeśli potrzebujesz go utworzyć, możesz użyć [tworzenie i Connect - portal](create-data-warehouse-portal.md) można utworzyć magazynu danych, nazywane **mySampleDataWarehouse**. 

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

## <a name="pause-compute"></a>Wstrzymaj obliczeń
W celu ograniczenia kosztów, możesz wstrzymywać i wznawiać obliczeń zasobów na żądanie. Na przykład jeśli nie będzie używać bazy danych w nocy i w weekendy, można wstrzymywać go w tych godzinach i wznawiać go w ciągu dnia. Użytkownik nie zostanie obciążona dla zasobów obliczeniowych, podczas bazy danych zostało wstrzymane. Jednak będą nadal naliczane za magazynu. 

Aby wstrzymać bazy danych, należy użyć [Suspend-AzureRmSqlDatabase](/powershell/module/azurerm.sql/suspend-azurermsqldatabase.md) polecenia cmdlet. W następującym przykładzie wstrzymano hurtowni danych o nazwie **mySampleDataWarehouse** znajdującej się na serwerze o nazwie **20171113 NowySerwer**. Serwer jest w grupie zasobów platformy Azure o nazwie **myResourceGroup**.


```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
```

Zmiany, w tym przykładzie dalej pobiera bazy danych do obiektu $database. Następnie przewody obiektu [Suspend-AzureRmSqlDatabase](/powershell/module/azurerm.sql/suspend-azurermsqldatabase). Wyniki są przechowywane w resultDatabase obiektu. Polecenia końcowego pokazuje wyniki.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```


## <a name="resume-compute"></a>Wznów obliczeń
Aby utworzyć bazę danych, należy użyć [Resume-AzureRmSqlDatabase](/powershell/module/azurerm.sql/resume-azurermsqldatabase) polecenia cmdlet. W następującym przykładzie uruchomiono bazę danych o nazwie mySampleDataWarehouse znajdującej się na serwerze o nazwie 20171113 NowySerwer. Serwer jest w grupie zasobów platformy Azure o nazwie myResourceGroup.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" -DatabaseName "mySampleDataWarehouse"
```

Zmiany, w tym przykładzie dalej pobiera bazy danych do obiektu $database. Następnie przewody obiektu [Resume-AzureRmSqlDatabase](/powershell/module/azurerm.sql/resume-azurermsqldatabase.md) i przechowuje wyniki w $resultDatabase. Polecenia końcowego pokazuje wyniki.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Płacisz za jednostki magazynu danych i dane przechowywane w Twoim magazynie danych. Opłaty za te zasoby obliczeniowe i magazynowe są naliczane osobno. 

- Jeśli chcesz przechowywać dane w magazynie, wstrzymać obliczeń.
- Aby uniknąć opłat w przyszłości, możesz usunąć magazyn danych. 

Wykonaj następujące kroki, aby wyczyścić zasoby zgodnie z potrzebami.

1. Zaloguj się do [portalu Azure](https://portal.azure.com)i kliknij polecenie w magazynie danych.

    ![Oczyszczanie zasobów](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. Aby wstrzymać obliczenia, kliknij przycisk **Wstrzymaj**. Gdy magazyn danych jest wstrzymany, zobaczysz przycisk **Uruchom**.  Aby wznowić obliczenia, kliknij przycisk **Uruchom**.

2. Aby usunąć magazyn danych i nie płacić za obliczenia oraz magazynowanie, kliknij przycisk **Usuń**.

3. Aby usunąć serwer SQL został utworzony, kliknij przycisk **mynewserver 20171113.database.windows.net**, a następnie kliknij przycisk **usunąć**.  Należy zachować ostrożność podczas usuwania, ponieważ usunięcie serwera spowoduje również usunięcie wszystkich baz danych przypisanych do tego serwera.

4. Aby usunąć grupę zasobów, kliknij pozycję **myResourceGroup**, a następnie kliknij pozycję **Usuń grupę zasobów**.


## <a name="next-steps"></a>Kolejne kroki
Masz teraz wstrzymana i wznowione obliczeniowe magazynu danych. Aby dowiedzieć się więcej na temat usługi Azure SQL Data Warehouse, przejdź do samouczka na temat ładowania danych.

> [!div class="nextstepaction"]
>[Ładowanie danych do magazynu danych SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)