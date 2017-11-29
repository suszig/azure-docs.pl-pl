---
title: "Zarządzanie dyskami Azure przy użyciu programu Azure PowerShell | Dokumentacja firmy Microsoft"
description: "Samouczek — Zarządzanie dyskami Azure przy użyciu programu Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 956f44068db8fe9c8c7a839a0ce80c19e2b2f11c
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="manage-azure-disks-with-powershell"></a>Zarządzanie dyskami Azure przy użyciu programu PowerShell

Maszyny wirtualne platformy Azure dysków do przechowywania systemu operacyjnego, aplikacje i dane maszyn wirtualnych. Podczas tworzenia maszyny Wirtualnej jest ważne wybrać rozmiar dysku i odpowiednie do oczekiwanego obciążenia w konfiguracji. W tym samouczku opisano wdrażanie i Zarządzanie dyskami maszyny Wirtualnej. Informacje o:

> [!div class="checklist"]
> * Dyski systemu operacyjnego i tymczasowego
> * Dyski z danymi
> * Standard i dysków w warstwie Premium
> * Wydajność dysku
> * Dołączanie i przygotowywania dysków z danymi

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Jeśli postanowisz zainstalować program PowerShell i używać go lokalnie, ten samouczek wymaga modułu Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie ` Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure. 

## <a name="default-azure-disks"></a>Domyślna dysku systemu Azure

Po utworzeniu maszyny wirtualnej platformy Azure, dwa dyski są automatycznie dołączane do maszyny wirtualnej. 

**Dysk systemu operacyjnego** -dysków systemu operacyjnego może być o rozmiarze maksymalnie 4 terabajt i hostuje maszyny wirtualne systemu operacyjnego.  Dysk systemu operacyjnego jest przypisany literą dysku z *c:* domyślnie. Dysk buforowanie konfiguracji dysku systemu operacyjnego jest zoptymalizowana pod kątem wydajności systemu operacyjnego. Dysk systemu operacyjnego **nie powinien** hosta aplikacji i danych. Dla aplikacji i danych należy użyć dysku danych, które szczegółowo w dalszej części tego artykułu.

**Dysku tymczasowym** -tymczasowego dysków używać dysków SSD, który znajduje się na tym samym hoście Azure co maszyna wirtualna. Tymczasowy dyski są wysokiej wydajności i mogą być używane do operacji, takich jak przetwarzanie danych tymczasowych. Jednak jeśli maszyna wirtualna zostanie przeniesiona do nowego hosta, zostaną usunięte wszystkie dane przechowywane na dysku tymczasowym. Rozmiar dysku tymczasowym zależy od rozmiaru maszyny Wirtualnej. Tymczasowe dyski są przypisane literę dysku *d:* domyślnie.

### <a name="temporary-disk-sizes"></a>Rozmiary dysku tymczasowym

| Typ | Rozmiar maszyny wirtualnej | Maksymalny rozmiar dysku tymczasowego (GB) |
|----|----|----|
| [Zastosowania ogólne](sizes-general.md) | A i serii D | 800 |
| [Optymalizacja pod kątem obliczeń](sizes-compute.md) | Seria F | 800 |
| [Optymalizacja pod kątem pamięci](../virtual-machines-windows-sizes-memory.md) | D i G serii | 6144 |
| [Optymalizacja pod kątem magazynu](../virtual-machines-windows-sizes-storage.md) | Seria L | 5630 |
| [Procesor GPU](sizes-gpu.md) | N serii | 1440 |
| [Wysoka wydajność](sizes-hpc.md) | A i serii H | 2000 |

## <a name="azure-data-disks"></a>Dyski danych Azure

Instalowanie aplikacji i przechowywania danych można dodać dodatkowego dysku z danymi. Dyski danych używanego w sytuacji, gdy wymagane jest trwałe i szybko reagowały pamięci masowej. Każdy dysk danych ma maksymalną pojemność 1 terabajt. Rozmiar maszyny wirtualnej Określa, jak wiele dysków z danymi może zostać dołączony do maszyny Wirtualnej. Dla każdej maszyny Wirtualnej vCPU można dołączyć danych dwóch dysków. 

### <a name="max-data-disks-per-vm"></a>Maksymalna liczba dysków z danymi dla maszyny Wirtualnej

| Typ | Rozmiar maszyny wirtualnej | Maksymalna liczba dysków z danymi dla maszyny Wirtualnej |
|----|----|----|
| [Zastosowania ogólne](sizes-general.md) | A i serii D | 32 |
| [Optymalizacja pod kątem obliczeń](sizes-compute.md) | Seria F | 32 |
| [Optymalizacja pod kątem pamięci](../virtual-machines-windows-sizes-memory.md) | D i G serii | 64 |
| [Optymalizacja pod kątem magazynu](../virtual-machines-windows-sizes-storage.md) | Seria L | 64 |
| [Procesor GPU](sizes-gpu.md) | N serii | 48 |
| [Wysoka wydajność](sizes-hpc.md) | A i serii H | 32 |

## <a name="vm-disk-types"></a>Typy dysków maszyny Wirtualnej

Platforma Azure oferuje dwa rodzaje dysku.

### <a name="standard-disk"></a>Dysków w warstwie standardowa

Magazyn Standard Storage bazuje na dyskach twardych (HDD) i stanowi ekonomiczne, chociaż wciąż wydajne rozwiązanie. Dyski standardowe idealnie nadają się do deweloperów ekonomiczne i testów obciążenia.

### <a name="premium-disk"></a>Dysku Premium

Dyski Premium bazują na dysk SSD dysku wysokiej wydajności i małych opóźnień. Idealny dla maszyn wirtualnych obciążeniu produkcji. Magazyn w warstwie Premium obsługuje serii DS, DSv2 serii GS-series i FS serii maszyn wirtualnych. Dyski w warstwie Premium są dostępne w trzech typów (P10, P20, P30, P40, P50), rozmiar dysku Określa typ dysku. Podczas wybierania, rozmiar dysku wartość jest zaokrąglana do dalej typu. Na przykład, jeśli rozmiar jest poniżej 128 GB typu dysku będzie P10, między 129 i 512 P20, 512 P30, P40 2TB i P50 4TB. 

### <a name="premium-disk-performance"></a>Wydajność dysku Premium

|Typ dysku magazynu w warstwie Premium | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Rozmiar dysku (zaokrąglony) | 128 GB | 512 GB | 1024 GB (1 TB) |
| Liczba operacji wejścia/wyjścia na sekundę na dysk | 500 | 2,300 | 5,000 |
Przepływność na dysk | 100 MB/s | 150 MB/s | 200 MB/s |

Gdy powyższej tabeli określa maksymalną liczbę IOPS dla każdego dysku, można osiągnąć wyższy poziom wydajności przez stosowanie wielu dysków z danymi. Na przykład dysków z danymi 64 może zostać dołączona do maszyny Wirtualnej Standard_GS5. Jeśli każda z tych dysków o jako P30 można osiągnąć maksymalnie 80 000 IOPS. Aby uzyskać szczegółowe informacje o maksymalnej IOPS dla maszyny Wirtualnej, zobacz [typów maszyny Wirtualnej, a rozmiary](./sizes.md).

## <a name="create-and-attach-disks"></a>Utwórz i Dołącz dysków

Aby ukończyć przykład, w tym samouczku, musi mieć istniejącej maszyny wirtualnej. Jeśli to konieczne, to [przykładowym skrypcie](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) można utworzyć. Podczas pracy nad samouczek, Zastąp maszyny Wirtualnej i grupy zasobów nazwy w razie potrzeby.

Tworzenie początkowej konfiguracji z [AzureRmDiskConfig nowy](/powershell/module/azurerm.compute/new-azurermdiskconfig). Poniższy przykład służy do konfigurowania dysku, który wynosi 128 GB w rozmiarze.

```azurepowershell-interactive
$diskConfig = New-AzureRmDiskConfig -Location EastUS -CreateOption Empty -DiskSizeGB 128
```

Tworzenie dysku z danymi z [AzureRmDisk nowy](/powershell/module/azurerm.compute/new-azurermdisk) polecenia.

```azurepowershell-interactive
$dataDisk = New-AzureRmDisk -ResourceGroupName myResourceGroup -DiskName myDataDisk -Disk $diskConfig
```

Uzyskiwanie maszyny wirtualnej, który chcesz dodać dysk danych w celu z [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) polecenia.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

Dodaj dysk danych do konfiguracji maszyny wirtualnej z [AzureRmVMDataDisk Dodaj](/powershell/module/azurerm.compute/add-azurermvmdatadisk) polecenia.

```azurepowershell-interactive
$vm = Add-AzureRmVMDataDisk -VM $vm -Name myDataDisk -CreateOption Attach -ManagedDiskId $dataDisk.Id -Lun 1
```

Zaktualizuj maszynę wirtualną z [AzureRmVM aktualizacji](/powershell/module/azurerm.compute/add-azurermvmdatadisk) polecenia.

```azurepowershell-interactive
Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm
```

## <a name="prepare-data-disks"></a>Przygotowanie dysków z danymi

Po dysk został dołączony do maszyny wirtualnej, musi być skonfigurowana do używania dysku systemu operacyjnego. Poniższy przykład pokazuje, jak ręcznie skonfigurować pierwszego dysku dodane do maszyny Wirtualnej. Ten proces można również zautomatyzować przy użyciu [niestandardowe rozszerzenie skryptu](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>Konfiguracja ręczna

Utwórz połączenie RDP z maszyny wirtualnej. Otwieranie programu PowerShell i uruchom ten skrypt.

```azurepowershell-interactive
Get-Disk | Where partitionstyle -eq 'raw' | `
Initialize-Disk -PartitionStyle MBR -PassThru | `
New-Partition -AssignDriveLetter -UseMaximumSize | `
Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono tematy dysków maszyny Wirtualnej takich jak:

> [!div class="checklist"]
> * Dyski systemu operacyjnego i tymczasowego
> * Dyski z danymi
> * Standard i dysków w warstwie Premium
> * Wydajność dysku
> * Dołączanie i przygotowywania dysków z danymi

Przejdź do następnego w samouczku więcej informacji na temat automatyzacji konfiguracji maszyny Wirtualnej.

> [!div class="nextstepaction"]
> [Automatyzowanie konfiguracji maszyny wirtualnej](./tutorial-automate-vm-deployment.md)
