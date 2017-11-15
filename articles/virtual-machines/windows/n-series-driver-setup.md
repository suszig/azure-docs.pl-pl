---
title: "Instalacja sterownika N-series usługi Azure dla systemu Windows | Dokumentacja firmy Microsoft"
description: "Jak skonfigurować wersji sterowników procesora GPU NVIDIA N serii maszyn wirtualnych systemu Windows na platformie Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/09/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b16e57e06d5055fc0c2750385630a908e10bd217
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2017
---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows-server"></a>Konfigurowanie wersji sterowników procesora GPU dla maszyn wirtualnych N-series, system operacyjny Windows Server
Aby skorzystać z możliwości procesora GPU Azure N-series maszyny wirtualne z systemami Windows Server 2016 lub Windows Server 2012 R2, zainstaluj obsługiwanych NVIDIA grafiki sterowników. Ten artykuł zawiera kroki konfiguracji sterownika po wdrożeniu maszyny Wirtualnej N serii. Informacje o instalacji sterowników jest również dostępny do [maszyn wirtualnych systemu Linux](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Dla podstawowych specyfikacji, pojemności magazynu i dysku szczegółów, zobacz [rozmiarów maszyn wirtualnych systemu Windows GPU](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 


[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]



## <a name="driver-installation"></a>Instalacja sterownika

1. Połączenia pulpitu zdalnego na każdej maszynie Wirtualnej N serii.

2. Pobierz, Wyodrębnij i zainstaluj obsługiwany sterownik dla systemu operacyjnego Windows.

Na maszynach wirtualnych z wirtualizacją sieci Azure po instalacji sterowników jest wymagane ponowne uruchomienie komputera. Na maszynach wirtualnych NC ponowne uruchomienie nie jest wymagane.

## <a name="verify-driver-installation"></a>Sprawdzić, czy instalacja sterownika

Instalacja sterownika w Menedżerze urządzeń można sprawdzić. W poniższym przykładzie przedstawiono Konfiguracja zakończyła się pomyślnie karty K80 tesla — na maszynie Wirtualnej platformy Azure NC.

![Właściwości sterownika procesora GPU](./media/n-series-driver-setup/GPU_driver_properties.png)

Aby sprawdzić stan urządzenia procesora GPU, uruchom [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) zainstalowane ze sterownikiem narzędzie wiersza polecenia.

1. Otwórz wiersz polecenia i zmień **C:\Program Files\NVIDIA Corporation\NVSMI** katalogu.

2. Uruchom polecenie `nvidia-smi`. Jeśli jest zainstalowany sterownik zostaną wyświetlone dane wyjściowe podobne do następującego. Należy pamiętać, że **GPU Util** pokazuje **0%** chyba, że obciążenie procesora GPU są aktualnie uruchomione na maszynie Wirtualnej. Wersja sterownika, a szczegóły GPU mogą być inne niż te wyświetlane.

![Stan urządzenia NVIDIA](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-for-nc24r-vms"></a>RDMA sieci dla maszyn wirtualnych NC24r

Połączenie sieciowe RDMA można włączyć dla NC24r maszyn wirtualnych wdrożonych w tym samym zestawie dostępności. Aby zainstalować sterowniki urządzeń sieciowych systemu Windows, umożliwiających łączności RDMA należy dodać rozszerzenie HpcVmDrivers. Aby dodać rozszerzenie maszyny Wirtualnej do NC24r maszyny Wirtualnej, użyj [programu Azure PowerShell](/powershell/azure/overview) poleceń cmdlet dla usługi Azure Resource Manager.

> [!NOTE]
> Obecnie tylko systemu Windows Server 2012 R2 obsługuje sieciowych RDMA na maszynach wirtualnych NC24r.
> 

Aby zainstalować najnowszą wersję 1.1 rozszerzenia HpcVMDrivers na istniejącej maszyny Wirtualnej z funkcją RDMA o nazwie myVM regionu zachodnie stany USA:
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Aby uzyskać więcej informacji, zobacz [rozszerzenia maszyny wirtualnej i funkcje systemu Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Sieć RDMA obsługuje ruch interfejsu Message (Passing) dla aplikacji działających z [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) lub Intel MPI 5.x. 


## <a name="next-steps"></a>Następne kroki

* Aby uzyskać więcej informacji o NVIDIA GPU na maszynach wirtualnych N-series zobacz:
    * [NVIDIA tesla — K80](http://www.nvidia.com/object/tesla-k80.html) (w przypadku maszyn wirtualnych Azure NC)
    * [NVIDIA tesla — M60](http://www.nvidia.com/object/tesla-m60.html) (w przypadku maszyn wirtualnych z wirtualizacją sieci Azure)

* Deweloperzy tworzący aplikacje przyspieszony GPU dla jednostki GPU tesla — NVIDIA można również pobrać i zainstalować CUDA 8 narzędzi dla [systemu Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) lub [systemu Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe). Aby uzyskać więcej informacji, zobacz [Przewodnik instalacji CUDA](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


