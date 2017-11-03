---
title: "Szybki Start Azure — Tworzenie konta magazynu przy użyciu programu PowerShell | Dokumentacja firmy Microsoft"
description: "Szybko dowiedzieć się utworzyć nowe konto magazynu przy użyciu programu PowerShell"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/29/2017
ms.author: robinsh
ms.openlocfilehash: c9175cce0cb93e73009fb8d751e54f631603d482
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-storage-account-using-powershell"></a>Utwórz konto magazynu przy użyciu programu PowerShell

Moduł Azure PowerShell umożliwia tworzenie zasobów platformy Azure i zarządzanie nimi za pomocą wiersza polecenia programu PowerShell lub skryptów. Szczegóły ten przewodnik przy użyciu programu PowerShell, aby utworzyć konto magazynu Azure. 

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

Dla tego przewodnika Szybki start jest wymagany moduł Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie `Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia `Login-AzureRmAccount` i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```powershell
Login-AzureRmAccount
```

Jeśli nie znasz lokalizacji, która ma być używany, można wyświetlić listę dostępnych lokalizacji. Po wyświetleniu listy znaleźć ten, który ma być używany. W tym przykładzie będzie używać **eastus**. Zapisać w zmiennej i użyj zmiennej, dzięki czemu można go zmieniać w jednym miejscu.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów platformy Azure za pomocą polecenia [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. 

```powershell
# put resource group in a variable so you can use the same group name going forward
#   without hardcoding it repeatedly
$resourceGroup = "contoso-storage-accounts"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-general-purpose-standard-storage-account"></a>Tworzenie konta standardowe magazynu ogólnego przeznaczenia

Istnieją różne typy kont magazynu, w zależności od tego, jak ma to być używane, a także usługę (obiekty BLOB, plików, tabel lub kolejek). W poniższej tabeli przedstawiono możliwości.

|**Typ konta magazynu**|**Przeznaczenie ogólne w warstwie Standardowa**|**Przeznaczenie ogólne w warstwie Premium**|**Usługa Blob Storage w gorącej i chłodnej warstwie dostępu**|
|-----|-----|-----|-----|
|**Obsługiwane usługi**| Obiekt blob, plików, tabeli, kolejki usług | Usługa Obiekty Blob | Usługa Obiekty Blob|
|**Obsługiwane typy obiektów blob**|Blokowe i stronicowe obiekty BLOB, uzupełnialne. | Stronicowe obiekty blob | Blokowe obiekty blob i uzupełnialne obiekty blob|

Użyj [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount) do utworzenia konta standardowe magazynu ogólnego przeznaczenia, używanego programu wszystkie cztery usługi. Nazwa konta magazynu *contosomvcstandard*i skonfiguruj ją lokalnie nadmiarowego magazynu i obiektów blob jest włączone szyfrowanie.

```powershell
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "contosomvcstandard" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, można użyć [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) polecenie Usuń grupę zasobów i wszystkie powiązane zasoby. W takim przypadku spowoduje usunięcie konta magazynu, który został utworzony.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Następne kroki

W tym szybki start utworzeniu konta standardowe magazynu ogólnego przeznaczenia. Aby dowiedzieć się, jak przekazywanie i pobieranie obiektów blob z konta magazynu, nadal Szybki Start magazynu obiektów Blob.
> [!div class="nextstepaction"]
> [Obiekty transferu do/z magazynu obiektów Blob platformy Azure przy użyciu programu PowerShell](../blobs/storage-quickstart-blobs-powershell.md)