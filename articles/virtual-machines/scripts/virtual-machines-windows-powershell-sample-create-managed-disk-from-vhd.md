---
title: "Skrypt programu PowerShell Azure przykładowe — tworzenie dysków zarządzanych z pliku VHD na koncie magazynu w tym samym lub różnych subskrypcji | Dokumentacja firmy Microsoft"
description: "Skrypt programu PowerShell Azure przykładowe — tworzenie dysków zarządzanych z pliku VHD na koncie magazynu w tym samym lub różnych subskrypcji."
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
ms.openlocfilehash: 6d37fb1308ce0b866b42f961ada84d0869f25615
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell"></a>Tworzenie dysku zarządzanego z pliku VHD na koncie magazynu w tym samym lub różnych subskrypcji przy użyciu programu PowerShell

Ten skrypt tworzy dysków zarządzanych z pliku VHD na koncie magazynu w tym samym lub różnych subskrypcji. Użyj tego skryptu, aby importować specjalne (nie uogólniony/Sysprep) wirtualnego dysku twardego na dysku systemu operacyjnego zarządzanych do utworzenia maszyny wirtualnej. Należy także użyć go do importowania danych dysku VHD na dysk danych zarządzanych. 

Nie należy tworzyć wiele identycznych dysków zarządzanych z pliku VHD w krótkim czasie. Do tworzenia zarządzanego dysków z pliku vhd, obiektów blob migawki pliku vhd jest tworzony i następnie jest używany do tworzenia dysków zarządzanych. W ciągu jednej minuty powodujące błędy tworzenia dysku z powodu dławienia można utworzyć migawki tylko jeden obiekt blob. Aby uniknąć tego ograniczenia przepustowości, Utwórz [zarządzanych migawki z pliku vhd](virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) i następnie użyj zarządzanego migawki, aby utworzyć wiele zarządzanych w skrócie dyski ilość czasu. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Jeśli wybierzesz do zainstalowania i używania programu PowerShell lokalnie, ten samouczek wymaga programu Azure PowerShell modułu w wersji 4.0 lub nowszej. Uruchom polecenie `Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli musisz zainstalować lub uaktualnić, zobacz [instalacji programu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure. 

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "Create managed disk from VHD")]


## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń w celu utworzenia dysków zarządzanych z dysku VHD w innej subskrypcji. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Nowe AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Tworzy konfiguracji dysku, który jest używany do tworzenia dysku. Zawiera typ magazynu, lokalizacji, identyfikator konto magazynu, w którym przechowywane nadrzędnego dysku VHD, identyfikator URI dysku VHD elementu nadrzędnego dysku VHD zasobu. |
| [Nowe AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Tworzy dysk przy użyciu konfiguracji dysku, nazwa dysku i przekazywane jako parametry nazwę grupy zasobów. |

## <a name="next-steps"></a>Następne kroki

[Utwórz maszynę wirtualną, dołączając dysków zarządzanych jako dysk systemu operacyjnego](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Aby uzyskać więcej informacji dotyczących modułu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](/powershell/azure/overview).

Przykłady skryptów PowerShell dodatkowe maszyny wirtualnej znajdują się w [dokumentacji maszyny Wirtualnej systemu Windows Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).