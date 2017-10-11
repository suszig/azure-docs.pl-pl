---
title: Rozmiary maszyny Wirtualnej systemu Windows Azure - HPC | Dokumentacja firmy Microsoft
description: "Wyświetla listę różnych rozmiarów dostępnych Windows komputerowych o wysokiej wydajności maszyn wirtualnych na platformie Azure."
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: a0596d134e9c26877848f93d72f35bfd2c957570
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="high-performance-compute-vm-sizes"></a>Rozmiary maszyn wirtualnych wysokiej wydajności obliczeniowej

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>Z funkcją RDMA wystąpień
Podzbiór wystąpień obliczeniowych (H16r, H16mr A8 i A9) funkcji interfejsu sieciowego dla zdalnego pamięci bezpośredniego dostępu (do pamięci RDMA) łączności. Ten interfejs jest oprócz interfejsu standardowe sieć platformy Azure, dostępne dla innych rozmiarów maszyn wirtualnych. 
  
Ten interfejs umożliwia wystąpienia z funkcją RDMA do komunikacji za pośrednictwem sieci InfiniBand, działających z rozdzielczością stawki FDR H16r i H16mr maszyn wirtualnych i szybkości QDR A8 i A9 maszyn wirtualnych. Tych funkcji RDMA może zwiększyć skalowalność i wydajność aplikacji komunikat interfejsu (Passing Interface).

Poniżej przedstawiono wymagania dotyczące maszyn wirtualnych systemu Windows z funkcją RDMA do uzyskania dostępu do sieci Azure RDMA: 

* **System operacyjny**
  
  Windows Server 2012 R2, Windows Server 2012
  
  > [!NOTE]
  > Windows Server 2016 nie obsługuje obecnie, łączności RDMA na platformie Azure.
  >

* **Zestaw dostępności lub usługi w chmurze** — wdrażanie maszyn wirtualnych z funkcją RDMA w tym samym zestawie dostępności (przy użyciu modelu wdrażania usługi Azure Resource Manager) lub samej usługi w chmurze (przy użyciu klasycznego modelu wdrażania). Jeśli używasz partii zadań Azure, maszyny wirtualne z funkcją RDMA musi być w tej samej puli.

* **MPI** -MPI firmy Microsoft (MS-MPI) 2012 R2 lub nowszym, Intel MPI biblioteki 5.x

  Obsługiwanych implementacje MPI Użyj interfejsu Network Direct Microsoft interfejsu do komunikacji między wystąpieniami. 

* **Przestrzeń adresową sieci RDMA** -RDMA sieci na platformie Azure rezerwuje 172.16.0.0/16 przestrzeni adresowej. Do uruchamiania aplikacji MPI w wystąpieniach wdrożony w sieci wirtualnej platformy Azure, upewnij się, że przestrzeń adresową sieci wirtualnej nakłada się na sieciowych RDMA.

* **Rozszerzenia maszyny Wirtualnej HpcVmDrivers** — na maszynach wirtualnych z funkcją RDMA, należy dodać rozszerzenie HpcVmDrivers, aby zainstalować sterowniki urządzeń sieciowych systemu Windows dla łączności RDMA. (W przypadku niektórych wdrożeń wystąpień A8 i A9, rozszerzenia HpcVmDrivers jest dodawane automatycznie). Aby dodać rozszerzenie maszyny Wirtualnej do maszyny Wirtualnej, można użyć [programu Azure PowerShell](/powershell/azure/overview) polecenia cmdlet. 

  
  Polecenie instaluje najnowsze rozszerzenia HpcVMDrivers wersji 1.1 na istniejącej maszyny Wirtualnej z funkcją RDMA, o nazwie *myVM* wdrożone w grupie zasobów o nazwie *myResourceGroup* w  *Zachodnie stany USA* regionu:

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  Aby uzyskać więcej informacji, zobacz [rozszerzenia maszyn wirtualnych i funkcji](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Może również współpracować z rozszerzeniami dla maszyn wirtualnych wdrożonych w [klasycznego modelu wdrażania](classic/manage-extensions.md).


## <a name="using-hpc-pack"></a>Przy użyciu pakietu HPC

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), rozwiązania firmy Microsoft wolnego HPC klastra i zadania zarządzania, jest jedną z opcji tworzenia klastra obliczeniowego na platformie Azure do uruchamiania aplikacji opartych na systemie Windows MPI i innych obciążeń HPC. HPC Pack 2012 R2 i nowsze wersje zawierają środowisko uruchomieniowe MPI MS wykorzystującego sieć Azure RDMA po wdrożeniu na maszynach wirtualnych z funkcją RDMA.




## <a name="other-sizes"></a>Innych rozmiarach
- [Zastosowania ogólne](sizes-general.md)
- [Optymalizacja pod kątem obliczeń](sizes-compute.md)
- [Optymalizacja pod kątem pamięci](../virtual-machines-windows-sizes-memory.md)
- [Optymalizacja pod kątem magazynu](../virtual-machines-windows-sizes-storage.md)
- [Optymalizacja pod kątem procesora GPU](sizes-gpu.md)

## <a name="next-steps"></a>Następne kroki

- Dla listy kontrolne użyć wystąpienia obliczeniowych pakietem HPC w systemie Windows Server, zobacz [Konfigurowanie klastra RDMA systemu Windows z pakietem HPC do uruchamiania aplikacji MPI](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

- Aby użyć wystąpienia obliczeniowych, podczas uruchamiania aplikacji MPI za pomocą partii zadań Azure, zobacz [korzystać z zadań w wielu wystąpieniach na uruchamianie aplikacji komunikat interfejsu (Passing Interface) w partii zadań Azure](../../batch/batch-mpi.md).

- Dowiedz się więcej na temat [jednostki (ACU) rozwiązań usługi obliczenia Azure](acu.md) ułatwia porównanie wydajności obliczeniowej różnych jednostki SKU Azure.




