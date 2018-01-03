---
title: Tworzenie konta maszyny Wirtualnej i magazynu dla skalowalnych aplikacji na platformie Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak wdrożyć maszynę Wirtualną, który ma być używany do uruchomienia skalowalnych aplikacji przy użyciu magazynu obiektów blob platformy Azure"
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 0fd1cd93ca6faabcbe0007136fe427028e722733
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/23/2017
---
# <a name="create-a-virtual-machine-and-storage-account-for-a-scalable-application"></a>Tworzenie maszyny wirtualnej i konto magazynu na potrzeby skalowalnych aplikacji

W tym samouczku wchodzi w jednej serii. Ten samouczek pokazuje, że wdrożyć aplikację, która przekazuje i Pobierz dużych ilości danych losowe z kontem magazynu platformy Azure. Po zakończeniu, masz działające na maszynie wirtualnej, Przekaż, a następnie Pobierz dużych ilości danych na konto magazynu aplikacji konsoli.

W części jednej serii, możesz dowiedzieć się, jak:

> [!div class="checklist"]
> * Tworzenie konta magazynu
> * Tworzenie maszyny wirtualnej
> * Skonfiguruj rozszerzenie skryptu niestandardowego

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Jeśli postanowisz zainstalować program PowerShell i używać go lokalnie, ten samouczek wymaga modułu Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie ` Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure.

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów platformy Azure za pomocą polecenia [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-storage-account"></a>Tworzenie konta magazynu
 
Przykład przekazuje 50 dużych plików do kontenera obiektów blob na koncie magazynu Azure. Konto magazynu zapewnia unikatową przestrzeń nazw do przechowywania i udostępniania obiektów danych usługi Azure storage. Utwórz konto magazynu w grupie zasobów został utworzony za pomocą [New-AzureRmStorageAccount](/powershell/module/AzureRM.Storage/New-AzureRmStorageAccount) polecenia.

W poniższym poleceniu zastąp własne globalnie unikatowej nazwy dla konta magazynu obiektów Blob, w którym można zobaczyć `<blob_storage_account>` symbolu zastępczego.

```powershell-interactive
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName myResourceGroup `
  -Name "<blob_storage_account>" `
  -Location EastUS `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob
```

## <a name="create-a-virtual-machine"></a>Tworzenie maszyny wirtualnej

Utwórz konfigurację maszyny wirtualnej. Ta konfiguracja zawiera ustawienia, które są używane podczas wdrażania maszyny wirtualnej, takie jak obraz maszyny wirtualnej, rozmiar i konfiguracja uwierzytelniania. Podczas wykonywania tego kroku jest wyświetlany monit o poświadczenia. Wprowadzane wartości są konfigurowane jako nazwa użytkownika i hasło dla maszyny wirtualnej.

Utwórz maszynę wirtualną za pomocą polecenia [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
# Variables for common values
$resourceGroup = "myResourceGroup"
$location = "eastus"
$vmName = "myVM"

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Location $location `
  -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create a virtual network card and associate with public IP address
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS14_v2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create a virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig

Write-host "Your public IP address is $($pip.IpAddress)"
```

## <a name="deploy-configuration"></a>Wdrażanie konfiguracji

W tym samouczku istnieją wymagania wstępne, które muszą być zainstalowane na maszynie wirtualnej. Niestandardowe rozszerzenie skryptu jest używany do uruchomienia skryptu programu PowerShell, który wykonuje następujące zadania:

> [!div class="checklist"]
> * Zainstaluj oprogramowanie .NET core 2.0
> * Instalacja chocolatey
> * Zainstaluj usługę GIT
> * Klonowanie repozytorium przykładowej
> * Przywracanie pakietów NuGet
> * Tworzy 50 1 GB pliki z losowe dane

Uruchom następujące polecenie cmdlet, aby zakończyć konfigurację maszyny wirtualnej. Ten krok może zająć 5 – 15 minut.

```azurepowershell-interactive
# Start a CustomScript extension to use a simple PowerShell script to install .NET core, dependencies, and pre-create the files to upload.
Set-AzureRMVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location EastUS `
    -FileUri https://raw.githubusercontent.com/azure-samples/storage-dotnet-perf-scale-app/master/setup_env.ps1 `
    -Run 'setup_env.ps1' `
    -Name DemoScriptExtension
```

## <a name="next-steps"></a>Kolejne kroki

W części jednej serii przedstawiono dotyczące tworzenia konta magazynu, wdrażania maszyny wirtualnej i konfigurowanie maszyny wirtualnej z wymagane wstępne np.:

> [!div class="checklist"]
> * Tworzenie konta magazynu
> * Tworzenie maszyny wirtualnej
> * Skonfiguruj rozszerzenie skryptu niestandardowego

Przejdź do części dwóch serii do przekazania na konto magazynu przy użyciu ponawiania wykładniczego i równoległości dużych ilości danych.

> [!div class="nextstepaction"]
> [Przekaż dużych ilości dużych plików równolegle na konto magazynu](storage-blob-scalable-app-upload-files.md)
