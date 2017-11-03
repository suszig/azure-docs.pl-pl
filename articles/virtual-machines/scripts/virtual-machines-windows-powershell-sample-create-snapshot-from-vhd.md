---
title: "Skrypt programu PowerShell Azure przykładowe — Tworzenie migawki z dysku VHD, aby utworzyć wiele takich samych dysków zarządzanych w krótkim czasie | Dokumentacja firmy Microsoft"
description: "Skrypt programu PowerShell Azure przykładowe — Tworzenie migawki z dysku VHD, aby utworzyć wiele takich samych dysków zarządzanych w krótkim czasie"
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 4cd6d9cc4f2b1fa41530349c957e180e2513586e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a>Utwórz migawkę z dysku VHD, aby utworzyć wiele takich samych dysków zarządzanych w krótkim czasie przy użyciu programu PowerShell

Ten skrypt tworzy migawkę z pliku VHD na koncie magazynu w tym samym lub różnych subskrypcji. Użyj tego skryptu, aby importować specjalne (nie uogólniony/Sysprep) dysk VHD do migawki i skorzystaj z migawki można tworzyć wiele identycznych dyskach zarządzanych w krótkim czasie. Ponadto należy zaimportować dane wirtualnego dysku twardego do migawki, a następnie utwórz wiele dysków zarządzanych w krótkim czasie za pomocą migawki. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Jeśli wybierzesz do zainstalowania i używania programu PowerShell lokalnie, ten samouczek wymaga programu Azure PowerShell modułu w wersji 4.0 lub nowszej. Uruchom polecenie `Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli musisz zainstalować lub uaktualnić, zobacz [instalacji programu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure. 

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]


## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń w celu utworzenia dysków zarządzanych z dysku VHD w innej subskrypcji. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Nowe AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Tworzy konfiguracji dysku, który jest używany do tworzenia dysku. Zawiera typ magazynu, lokalizacji, identyfikator konta magazynu, w którym przechowywana jest nadrzędny dysk VHD zasobu i identyfikator URI dysku VHD elementu nadrzędnego dysku VHD. |
| [Nowe AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Tworzy dysk przy użyciu konfiguracji dysku, nazwa dysku i przekazywane jako parametry nazwę grupy zasobów. |

## <a name="next-steps"></a>Następne kroki

[Tworzenie dysku zarządzanego z migawki](virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[Utwórz maszynę wirtualną, dołączając dysków zarządzanych jako dysk systemu operacyjnego](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Aby uzyskać więcej informacji dotyczących modułu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](/powershell/azure/overview).

Przykłady skryptów PowerShell dodatkowe maszyny wirtualnej znajdują się w [dokumentacji maszyny Wirtualnej systemu Windows Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).