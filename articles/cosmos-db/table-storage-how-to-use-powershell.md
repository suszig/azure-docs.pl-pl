---
title: "Wykonywanie operacji magazynu tabel Azure przy użyciu programu PowerShell | Dokumentacja firmy Microsoft"
description: "Wykonywanie operacji magazynu tabel Azure przy użyciu programu PowerShell"
services: storage
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 11/02/2017
ms.author: robinsh
ms.openlocfilehash: 0174b6fe02008a1c22a165b077c694af7e8618ab
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/08/2017
---
# <a name="perform-azure-table-storage-operations-with-azure-powershell"></a>Wykonywanie operacji magazynu tabel Azure przy użyciu programu Azure PowerShell 

>[!NOTE]
>Interfejsu API Azure rozwiązania Cosmos DB tabeli zawiera funkcje premium magazynu tabel gotowe dystrybucji globalnych, odczyty małe opóźnienia i zapisy, automatycznego indeksowania dodatkowej i dedykowanych przepływności. W większości przypadków poleceń programu PowerShell w pracach tego artykułu dla magazynu zarówno interfejsu API Azure rozwiązania Cosmos DB tabeli i tabel Azure, ale w tym artykule jest specyficzne dla magazynu tabel Azure. Jeśli korzystasz z interfejsu API Azure rozwiązania Cosmos DB tabeli, zobacz [operacje interfejsu API Azure rozwiązania Cosmos DB tabeli przy użyciu programu Azure PowerShell](table-powershell.md).
>

Magazyn tabel Azure to magazyn danych NoSQL, która służy do przechowywania i zapytań dotyczących dużych zestawów strukturalnych danych nierelacyjnych. Główne składniki usługi są tabele, jednostki i właściwości. Tabela jest kolekcji jednostek. Jednostka jest zbiór właściwości. Każdy obiekt może mieć maksymalnie 252 właściwości, które są wszystkie pary nazwa wartość. W tym artykule przyjęto założenie, że znasz już pojęcia dotyczące usługi Magazyn tabel Azure. Aby uzyskać szczegółowe informacje, zobacz [opis modelu danych usługi tabel](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model) i [Rozpoczynanie pracy z magazynem tabel Azure przy użyciu platformy .NET](table-storage-how-to-use-dotnet.md).

W tym artykule opisano typowe operacje magazynu tabel Azure. Omawiane kwestie: 

> [!div class="checklist"]
> * Tworzenie tabeli
> * Pobierz tabelę
> * Dodaj jednostki tabeli
> * Zapytania dotyczącego tabeli
> * Usuwanie jednostek tabeli
> * Usuwanie tabeli

W tym artykule przedstawiono sposób tworzenia nowego konta magazynu w nową grupę zasobów, można łatwo usunąć ją po zakończeniu. Jeśli zamiast czy użyć istniejącego konta magazynu, możesz to zrobić zamiast tego.

Przykłady wymagają programu Azure PowerShell w wersji modułu 4.4.0 lub nowszym. W oknie programu PowerShell, uruchom `Get-Module -ListAvailable AzureRM` można znaleźć wersji. Jeśli nie będą wyświetlane żadne lub konieczne uaktualniania, zobacz [modułu instalacji programu Azure PowerShell](/powershell/azure/install-azurerm-ps). 

Po zainstalowaniu programu Azure PowerShell lub zaktualizowaniu, należy zainstalować moduł **AzureRmStorageTable**, który zawiera polecenia do zarządzania jednostek. Aby zainstalować ten moduł, uruchom program PowerShell jako administrator i użyj **instalacji modułu** polecenia.

```powershell
Install-Module AzureRmStorageTable
```

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia `Login-AzureRmAccount` i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```powershell
Login-AzureRmAccount
```

## <a name="retrieve-list-of-locations"></a>Pobieranie listy lokalizacji

Jeśli nie znasz lokalizacji, która ma być używany, można wyświetlić listę dostępnych lokalizacji. Po wyświetleniu listy znaleźć ten, który ma być używany. Użyj tych przykładów **eastus**. Przechowywanie tej wartości w zmiennej **lokalizacji** do użytku w przyszłości.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów na pomocą polecenia [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/New-AzureRmResourceGroup). 

Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. Nazwa grupy zasobów należy przechowywać w zmiennej do użytku w przyszłości. W tym przykładzie grupy zasobów o nazwie *pshtablesrg* jest tworzony w *eastus* regionu.

```powershell
$resourceGroup = "pshtablesrg"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Tworzenie konta magazynu

Tworzenie konta standardowe magazynu ogólnego przeznaczenia z magazyn lokalnie nadmiarowy (LRS) przy użyciu [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Pobiera kontekst konta magazynu, który definiuje konto magazynu do użycia. Działając na konto magazynu, możesz odwoływać się kontekstu zamiast wielokrotnie podawania poświadczeń.

```powershell
$storageAccountName = "pshtablestorage"
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```

## <a name="create-a-new-table"></a>Utwórz nową tabelę

Aby utworzyć tabelę, użyj [AzureStorageTable nowy](/powershell/module/azure.storage/New-AzureStorageTable) polecenia cmdlet. W tym przykładzie tabela o nazwie `pshtesttable`.

```powershell
$tableName = "pshtesttable"
New-AzureStorageTable –Name $tableName –Context $ctx
```

## <a name="retrieve-a-list-of-tables-in-the-storage-account"></a>Pobieranie listy tabel na koncie magazynu

Pobieranie listy tabel za pomocą konta magazynu [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable).

```powershell
$storageTable = Get-AzureStorageTable –Context $ctx | select Name
```

## <a name="retrieve-a-reference-to-a-specific-table"></a>Pobierz odwołanie do określonej tabeli

Wykonywanie operacji w tabeli, należy odwołanie do określonej tabeli. Odwołanie przy użyciu [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable). 

```powershell
$storageTable = Get-AzureStorageTable –Name $tableName –Context $ctx
```

[!INCLUDE [storage-table-entities-powershell-include](../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Usuwanie tabeli

Aby usunąć tabelę, użyj [AzureStorageTable Usuń](/powershell/module/azure.storage/Remove-AzureStorageTable). To polecenie cmdlet spowoduje usunięcie tabeli z uwzględnieniem wszystkich swoich danych.

```powershell
Remove-AzureStorageTable –Name $tableName –Context $ctx

# Retrieve the list of tables to verify the table has been removed.
Get-AzureStorageTable –Context $Ctx | select Name
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli utworzono nowe konto grupy i przechowywanie zasobów na początku tego jak to zrobić, należy usunąć wszystkie zasoby utworzone w tym ćwiczeniu przez usunięcie grupy zasobów. To polecenie usuwa wszystkie zasoby zawarte w grupie, a także samej grupy zasobów.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Następne kroki

W tym artykule przedstawiono o typowych operacji magazynu tabel Azure przy użyciu programu PowerShell, w tym jak: 

> [!div class="checklist"]
> * Tworzenie tabeli
> * Pobierz tabelę
> * Dodaj jednostki tabeli
> * Zapytania dotyczącego tabeli
> * Usuwanie jednostek tabeli
> * Usuwanie tabeli

Aby uzyskać więcej informacji zobacz następujące artykuły

* [Polecenia cmdlet programu PowerShell magazynu](/powershell/module/azurerm.storage#storage)

* [Praca z tabelami magazynu platformy Azure z programu PowerShell](https://blogs.technet.microsoft.com/paulomarques/2017/01/17/working-with-azure-storage-tables-from-powershell/)

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) jest bezpłatną aplikacją autonomiczną oferowaną przez firmę Microsoft, która umożliwia wizualną pracę z danymi w usłudze Azure Storage w systemach Windows, macOS i Linux.