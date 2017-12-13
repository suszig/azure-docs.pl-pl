---
title: "Przykładowy skrypt programu PowerShell Azure - IIS z usługi Konfiguracja DSC | Dokumentacja firmy Microsoft"
description: "Przykładowy skrypt programu PowerShell Azure - IIS z usługi Konfiguracja DSC"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: nepeters
ms.openlocfilehash: c80c2a3229866833dbbe188ec5150a7095816ea8
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2017
---
# <a name="create-an-iis-vm-with-powershell"></a>Tworzenie maszyny Wirtualnej usług IIS przy użyciu programu PowerShell

Ten skrypt tworzy maszynę wirtualną platformy Azure z systemem Windows Server 2016, a następnie używa rozszerzenia DSC maszyny wirtualnej Azure, aby zainstalować usługi IIS. Po uruchomieniu skryptu, można uzyskać dostępu do domyślnej witryny usług IIS na publiczny adres IP maszyny wirtualnej.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-dsc/create-windows-vm-iis-dsc.ps1 "Create VM IIS DSC")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń w celu utworzenia wdrożenia. Każdy element w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Nowe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Nowe AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Tworzy maszynę wirtualną i podłączony do karty sieciowej, sieci wirtualnej, podsieci i grupy zabezpieczeń sieci. To polecenie również otwiera port 80 i ustawia poświadczenia administracyjne. |
| [Zestaw AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) | Dodaj rozszerzenie maszyny Wirtualnej do maszyny wirtualnej. W tym przykładzie niestandardowe rozszerzenie skryptu jest używany do instalacji usług IIS. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Usuwa grupę zasobów i wszystkie zasoby zawarte w ciągu. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących modułu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](/powershell/azure/overview).

Przykłady skryptów PowerShell dodatkowe maszyny wirtualnej znajdują się w [dokumentacji maszyny Wirtualnej systemu Windows Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
