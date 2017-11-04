---
title: "Tworzenie maszyny Wirtualnej systemu Windows na platformie Azure przy użyciu nowego AzVM | Dokumentacja firmy Microsoft"
description: "Skrypt programu PowerShell Azure przykładowe — tworzenie maszyny Wirtualnej systemu Windows za pomocą polecenia cmdlet New-AzVM."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/29/2017
ms.author: cynthn
ms.openlocfilehash: 36660ea1e8609ea8a03f4fa63e6f12f765bf9721
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-fully-configured-virtual-machine-with-the-new-azvm-powershell-cmdlet-preview"></a>Utwórz maszynę wirtualną w pełni skonfigurowany za pomocą polecenia cmdlet New-AzVM środowiska PowerShell (wersja zapoznawcza)

Ten skrypt używa polecenia cmdlet New-AzVM w powłoce chmury w celu utworzenia maszyny wirtualnej platformy Azure z systemem Windows Server 2016. Po uruchomieniu skryptu, można uzyskać dostępu do maszyny wirtualnej za pośrednictwem protokołu RDP. Chmura ma moduł, który zawiera New-AzVM instalowane domyślnie, co ułatwia zapoznaj się z nowych funkcji.

W przykładzie użyto polecenia cmdlet New AzVM, czyli w wersji zapoznawczej. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

## <a name="sample-script"></a>Przykładowy skrypt


```azurepowershell-interactive
New-AzVM -Name myVM `
    -VirtualNetworkName myVNet `
    -Location westeurope `
    -SecurityGroupName myNSG `
    -PublicIpAddressName myPIP 
```


## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myVMResourceGroup
```


## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących modułu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](/powershell/azure/overview).

Przykłady skryptów PowerShell dodatkowe maszyny wirtualnej znajdują się w [dokumentacji maszyny Wirtualnej systemu Windows Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
