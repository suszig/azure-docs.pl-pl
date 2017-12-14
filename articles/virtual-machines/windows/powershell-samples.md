---
title: "Przykłady środowiska PowerShell maszyny wirtualnej platformy Azure | Dokumentacja firmy Microsoft"
description: "Przykłady środowiska PowerShell maszyny wirtualnej platformy Azure"
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
ms.date: 09/20/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: d30361a7149342000c6e6cfdbfe1ee7e6caf945d
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2017
---
# <a name="azure-virtual-machine-powershell-samples"></a>Przykładów dla platformy Azure PowerShell maszyny wirtualnej

Poniższa tabela zawiera linki do przykłady skryptów środowiska PowerShell, które tworzenia i zarządzania maszynami wirtualnymi systemu Windows.

| | |
|---|---|
|**Tworzenie maszyn wirtualnych**||
| [Szybko utworzyć maszynę wirtualną](./../scripts/virtual-machines-windows-powershell-sample-create-vm-quick.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tworzy grupę zasobów, maszyny wirtualnej i wszystkie powiązane zasoby z minimalnymi monitów.|
| [Utwórz maszynę wirtualną w pełni skonfigurowany](./../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tworzy grupę zasobów, maszyny wirtualnej i wszystkie powiązane zasoby.|
| [Tworzenie maszyn wirtualnych o wysokiej dostępności](./../scripts/virtual-machines-windows-powershell-sample-create-nlb-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tworzy kilka maszyny wirtualne o wysokiej dostępności i konfiguracji równoważenia obciążenia.|
| [Tworzenie maszyny Wirtualnej, a następnie uruchom skrypt konfiguracji](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tworzy maszynę wirtualną i wykorzystują rozszerzenie Azure niestandardowego skryptu, aby zainstalować usługi IIS. |
| [Utwórz maszynę Wirtualną i uruchomić konfiguracji DSC](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tworzy maszynę wirtualną i używa rozszerzenia konfiguracji żądanego stanu Azure (DSC), aby zainstalować usługi IIS. |
| [Przekazywanie wirtualnego dysku twardego i tworzyć maszyny wirtualne](./../scripts/virtual-machines-windows-powershell-upload-generalized-script.md) | Przekazuje plik lokalny wirtualnego dysku twardego na platformie Azure, tworzy i obraz z dysku VHD, a następnie tworzy Maszynę wirtualną z tego obrazu. |
| [Utwórz maszynę Wirtualną z dyskiem zarządzanym systemu operacyjnego](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tworzy maszynę wirtualną, dołączając istniejący dysk zarządzane jako dysk systemu operacyjnego. |
| [Tworzenie maszyny Wirtualnej z migawki](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tworzy maszynę wirtualną z migawki, najpierw tworząc dysków zarządzanych z migawki, a następnie Trwa dołączanie nowego dysku zarządzanego jako dysk systemu operacyjnego. |
|**Tworzenie maszyn wirtualnych przy użyciu nowego AzVM**||
| [Utwórz maszynę wirtualną w pełni skonfigurowany](./../scripts/virtual-machines-windows-powershell-sample-create-vm-auto.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tworzy grupę zasobów, maszyny wirtualnej i wszystkie powiązane zasoby.|
| [Tworzenie maszyny Wirtualnej, a następnie uruchom skrypt konfiguracji](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis-auto.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tworzy maszynę wirtualną i wykorzystują rozszerzenie Azure niestandardowego skryptu, aby zainstalować usługi IIS. |
| [Utwórz maszynę Wirtualną i uruchomić konfiguracji DSC](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc-auto.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tworzy maszynę wirtualną i używa rozszerzenia konfiguracji żądanego stanu Azure (DSC), aby zainstalować usługi IIS. |
|**Zarządzanie magazynem**||
| [Tworzenie dysków zarządzanych z dysku VHD w tych samych lub różnych subskrypcji.](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tworzy dysków zarządzanych z specjalne wirtualny dysk twardy jako dysk systemu operacyjnego lub dane wirtualnego dysku twardego jako dysk danych w tych samych lub różnych subskrypcji.  |
| [Tworzenie dysku zarządzanego z migawki](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tworzy dysków zarządzanych z migawki. |
| [Kopiowanie dysków zarządzanych w do tej samej lub innej subskrypcji](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopiuje zarządzane dysku do tych samych lub różnych subskrypcji, ale w tym samym regionie co element nadrzędny zarządzane dysku. 
| [Eksportowanie migawki jako dysk VHD do konta magazynu](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Eksportuje migawkę zarządzane jako dysk VHD do konta magazynu w innym regionie. |
| [Utwórz migawkę z wirtualnego dysku twardego](../scripts/virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tworzy migawki z dysku VHD, aby utworzyć wiele dysków zarządzanych identyczne z migawki w krótkim czasie.  |
| [Kopiowanie migawek do tych samych lub różnych subskrypcji.](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Kopie migawki do tych samych lub różnych subskrypcji, ale w tym samym regionie co migawki nadrzędnej. |
|**Bezpieczne maszyny wirtualne**||
| [Szyfrowanie dysków maszyny Wirtualnej i danych](./../scripts/virtual-machines-windows-powershell-sample-encrypt-vm.md?toc=%2fpowershell%2fazure%2ftoc.json) | Tworzy usługi Azure Key Vault, klucz szyfrowania i nazwy głównej usługi, a następnie szyfruje maszyny Wirtualnej. |
|**Monitorowanie maszyn wirtualnych**||
| [Monitor maszyny Wirtualnej w usłudze Operations Management Suite](./../scripts/virtual-machines-windows-powershell-sample-create-vm-oms.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tworzy maszynę wirtualną, a następnie instaluje agenta programu Operations Management Suite i rejestruje maszyny Wirtualnej w obszarze roboczym pakietu OMS.  |
| | |

