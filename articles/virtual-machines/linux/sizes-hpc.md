---
title: Rozmiar maszyny Wirtualnej systemu Linux platformy Azure - HPC | Dokumentacja firmy Microsoft
description: "Wyświetla listę różnych rozmiarów dostępnych Linux komputerowych o wysokiej wydajności maszyn wirtualnych na platformie Azure."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/10/2017
ms.author: jonbeck
ms.openlocfilehash: fecc0656264f1c1d44aa8ad3aea321aa8cc4a0c8
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="high-performance-compute-linux-vm-sizes"></a>Wysoka wydajność obliczeniowe rozmiary maszyny Wirtualnej systemu Linux

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>Z funkcją RDMA wystąpień
Podzbiór wystąpień obliczeniowych (H16r, H16mr A8 i A9) funkcji interfejsu sieciowego dla zdalnego pamięci bezpośredniego dostępu (do pamięci RDMA) łączności. Ten interfejs jest oprócz interfejsu standardowe sieć platformy Azure, dostępne dla innych rozmiarów maszyn wirtualnych. 
  
Ten interfejs umożliwia wystąpienia z funkcją RDMA do komunikacji za pośrednictwem sieci InfiniBand, działających z rozdzielczością stawki FDR H16r i H16mr maszyn wirtualnych i szybkości QDR A8 i A9 maszyn wirtualnych. Tych funkcji RDMA może zwiększyć skalowalność i wydajność aplikacji komunikat interfejsu (Passing Interface) do uruchamiania Intel MPI 5.x lub nowszej wersji.

Wdrażanie maszyn wirtualnych z funkcją RDMA w tym samym zestawie dostępności (przy użyciu modelu wdrażania usługi Azure Resource Manager) lub samej usługi w chmurze (przy użyciu klasycznego modelu wdrażania). Postępuj zgodnie z dodatkowymi wymaganiami dotyczącymi maszyn wirtualnych systemu Linux z funkcją RDMA do uzyskania dostępu do sieci Azure RDMA.

### <a name="distributions"></a>Dystrybucje
 
Wdróż obliczeniowych maszyn wirtualnych z jednego z obrazów w portalu Azure Marketplace obsługującej funkcję RDMA połączenia:
  
* **Ubuntu** -LTS Ubuntu Server 16.04. Konfigurowanie sterowników RDMA na maszynie Wirtualnej i rejestrowanie z Intel, aby pobrać Intel MPI:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **SUSE Linux Enterprise Server** -SLES 12 SP3 dla HPC, SLES 12 SP3 dla HPC (Premium), SLES 12 SP1 w przypadku HPC, SLES 12 SP1 w przypadku HPC (Premium). RDMA sterowniki są zainstalowane i Intel MPI pakiety są dystrybuowane na maszynie Wirtualnej. Zainstaluj MPI, uruchamiając następujące polecenie:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
    
* **Na podstawie centOS HPC** — na podstawie CentOS 7.3 HPC, na podstawie CentOS 7.1 HPC, na podstawie CentOS 6.8 HPC lub na podstawie CentOS HPC 6.5 (H-serii, jest zalecane w wersji 7.1 lub nowszej). Sterowniki RDMA i Intel MPI 5.1 są zainstalowane na maszynie Wirtualnej.  
 
  > [!NOTE]
  > Na obrazach na podstawie CentOS HPC aktualizacji jądra są wyłączone w **yum** pliku konfiguracji. To dlatego sterowniki Linux RDMA są dystrybuowane jako pakiet RPM, a aktualizacje sterowników może nie działać w przypadku jądra jest aktualizowany.
  > 
 
### <a name="cluster-configuration"></a>Konfiguracja klastra 
    
Dodatkowy system konfiguracji jest wymagany do uruchomienia zadań MPI na klastrowanych maszyn wirtualnych. Na przykład w klastrze maszyn wirtualnych, należy ustanowić relację zaufania między węzły obliczeniowe. W przypadku typowych ustawień, zobacz [Konfigurowanie klastra Linux RDMA na uruchamianie aplikacji MPI](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="network-topology-considerations"></a>Zagadnienia dotyczące topologii sieci
* W przypadku komputerów z obsługą RDMA maszyn wirtualnych systemu Linux na platformie Azure Eth1 jest zarezerwowana dla ruchu sieciowego RDMA. Nie należy zmieniać ustawienia Eth1 lub wszelkie informacje zawarte w pliku konfiguracji odnoszące się do tej sieci. Eth0 jest zarezerwowana dla regularnych Azure ruchu sieciowego.

* IP przez InfiniBand (IB) nie jest obsługiwana na platformie Azure. Tylko RDMA over IB jest obsługiwana.

## <a name="using-hpc-pack"></a>Przy użyciu pakietu HPC
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), rozwiązania firmy Microsoft wolnego HPC klastra i zadania zarządzania, jest jedną z opcji można użyć wystąpienia obliczeniowych w systemie Linux. Najnowsze wersje Obsługa HPC Pack kilka dystrybucje systemu Linux na obliczeniowe węzłów wdrożone w maszynach wirtualnych platformy Azure, zarządzane przez system Windows Server węzła głównego. Węzły obliczeniowe z funkcją RDMA Linux uruchomiony MPI firmy Intel HPC Pack można zaplanować i uruchamiać aplikacje, które uzyskują dostęp do sieci RDMA MPI systemu Linux. Zobacz [wprowadzenie węzły obliczeniowe systemu Linux w klastrze HPC Pack na platformie Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Innych rozmiarach
- [Zastosowania ogólne](sizes-general.md)
- [Optymalizacja pod kątem obliczeń](sizes-compute.md)
- [Optymalizacja pod kątem pamięci](sizes-memory.md)
- [Optymalizacja pod kątem magazynu](sizes-storage.md)
- [Procesor GPU](../windows/sizes-gpu.md)


## <a name="next-steps"></a>Następne kroki

- Aby rozpocząć wdrażanie i z protokołem RDMA w systemie Linux przy użyciu obliczeniowych rozmiarów, zobacz [Konfigurowanie klastra Linux RDMA na uruchamianie aplikacji MPI](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

- Dowiedz się więcej na temat [jednostki (ACU) rozwiązań usługi obliczenia Azure](acu.md) ułatwia porównanie wydajności obliczeniowej różnych jednostki SKU Azure.




