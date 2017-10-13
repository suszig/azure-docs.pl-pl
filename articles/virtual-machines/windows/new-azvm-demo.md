---
title: "Tworzenie maszyny wirtualnej z systemem Windows za pomocą polecenia cmdlet New-AzVM usługi Azure Cloud Shell | Microsoft Docs"
description: "Szybka nauka tworzenia maszyn wirtualnych z systemem Windows za pomocą polecenia cmdlet New-AzVM usługi Azure Cloud Shell."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/21/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: 3be46c8c02ad136edb1936fbb39560d479b27277
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-windows-virtual-machine-with-the-new-azvm-preview-in-cloud-shell"></a>Tworzenie maszyny wirtualnej z systemem Windows za pomocą polecenia New-AzVM (w wersji zapoznawczej) usługi Cloud Shell. 

Polecenie cmdlet New-AzVM (w wersji zapoznawczej) pozwala łatwo utworzyć nową maszynę wirtualną przy użyciu programu PowerShell. Ten przewodnik zawiera szczegółowe informacje dotyczące tworzenia na platformie Azure nowej maszyny wirtualnej z systemem Windows Server 2016 za pomocą programu PowerShell w usłudze Azure Cloud Shell udostępniającej wstępnie zainstalowane polecenie cmdlet New-AzVM. Po ukończeniu wdrożenia nawiążemy połączenie z serwerem za pomocą protokołu RDP.  

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).


[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]

## <a name="create-the-vm"></a>Tworzenie maszyny wirtualnej

Polecenie cmdlet **New-AzVM** pozwala utworzyć maszynę wirtualną przy użyciu inteligentnych ustawień domyślnych, umożliwiających na przykład użycie obrazu systemu Windows Server 2016 Datacenter pochodzącego z witryny Azure Marketplace. Uruchomienie polecenia New-AzVM w sposób autonomiczny spowoduje użycie wartości domyślnych jako nazw zasobów. W tym przykładzie parametr **-Nazwa** zostanie ustawiony na wartość *myVM*. Polecenie cmdlet utworzy wszystkie wymagane zasoby, używając ciągu *myVM* jako prefiksu nazwy zasobu. 

Upewnij się, że w usłudze Cloud Shell wybrano program **PowerShell** i wpisz:

```azurepowershell-interactive
New-AzVm -Name myVM
```

Pojawi się monit o utworzenie nazwy użytkownika i hasła dla maszyny wirtualnej. Dane te będą używane do połączenia się z maszyną wirtualną w dalszej części tego tematu. Hasło musi mieć długość od 12 do 123 znaków i spełniać trzy z czterech następujących wymagań dotyczących złożoności: mała litera, wielka litera, cyfra i znak specjalny.

Utworzenie maszyny wirtualnej i skojarzonych zasobów trwa około minutę. Po zakończeniu tej operacji można wyświetlić wszystkie utworzone zasoby przy użyciu polecenia cmdlet [Find-AzureRmResource](/powershell/module/azurerm.resources/find-azurermresource).

```azurepowershell-interactive
Find-AzureRmResource `
    -ResourceGroupNameEquals myVMResourceGroup | Format-Table Name
```

## <a name="connect-to-the-vm"></a>Łączenie z maszyną wirtualną

Po zakończeniu wdrożenia utwórz połączenie pulpitu zdalnego z maszyną wirtualną.

Wróć do publicznego adresu IP maszyny wirtualnej za pomocą polecenia [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Zanotuj ten adres IP.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
    -ResourceGroupName myVMResourceGroup | Select IpAddress
```

Na komputerze lokalnym otwórz wiersz polecenia i użyj polecenia **mstsc**, aby uruchomić sesję usług pulpitu zdalnego z nową maszyną wirtualną. Zamień &lt;publiczny adres IP&gt; na adres IP maszyny wirtualnej. Po wyświetleniu monitu wprowadź nazwę użytkownika i hasło podane podczas tworzenia maszyny wirtualnej.

```
mstsc /v:<publicIpAddress>
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy grupa zasobów, maszyna wirtualna i wszystkie pokrewne zasoby nie będą już potrzebne, można je usunąć za pomocą polecenia [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myVMResourceGroup
```

## <a name="next-steps"></a>Następne kroki

W tym temacie wdrożyliśmy prostą maszynę wirtualną przy użyciu polecenia New-AzVM, a następnie połączyliśmy się z nią za pośrednictwem protokołu RDP. Aby dowiedzieć się więcej o maszynach wirtualnych platformy Azure, przejdź do samouczka dla maszyn wirtualnych z systemem Windows.

> [!div class="nextstepaction"]
> [Samouczki dla maszyny wirtualnej platformy Azure z systemem Windows](./tutorial-manage-vm.md)
