---
title: "Skrypt programu PowerShell Azure przykładowe — zaszyfrować maszyny Wirtualnej systemu Windows | Dokumentacja firmy Microsoft"
description: "Skrypt programu PowerShell Azure przykładowe — zaszyfrować maszyny Wirtualnej systemu Windows"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: iainfou
ms.openlocfilehash: f405cdaf61d6aaafa8568a9d7f21614071285c17
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2017
---
# <a name="encrypt-a-windows-virtual-machine-with-azure-powershell"></a>Szyfrowanie maszyny wirtualnej systemu Windows, przy użyciu programu Azure PowerShell

Ten skrypt tworzy bezpieczne usługi Azure Key Vault, klucze szyfrowania, nazwy głównej usługi Azure Active Directory i maszyny wirtualnej systemu Windows (VM). Maszyna wirtualna jest następnie szyfrowany przy użyciu klucza szyfrowania z magazynu kluczy i poświadczenia główne usługi.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/virtual-machine/encrypt-vm/encrypt-windows-vm.ps1 "Encrypt VM disks")]

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
| [Nowy AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) | Tworzy magazynu kluczy Azure do przechowywania zabezpieczonych danych, takie jak klucze szyfrowania. |
| [Dodaj AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) | Tworzy klucz szyfrowania magazynu kluczy. |
| [Nowe AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | Tworzy nazwę główną usługi do bezpiecznego uwierzytelniania i kontroli dostępu do kluczy szyfrowania usługi Azure Active Directory. |
| [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) | Ustawia uprawnienia Key Vault w celu udzielenia dostępu główną usługi do kluczy szyfrowania. |
| [Nowe AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Tworzy maszynę wirtualną i podłączony do karty sieciowej, sieci wirtualnej, podsieci i grupy zabezpieczeń sieci. To polecenie również otwiera port 80 i ustawia poświadczenia administracyjne. |
| [Get-AzureRmKeyVault](/powershell/module/azurerm.keyvault/get-azurermkeyvault) | Pobiera wymaganych informacji na temat usługi Key Vault |
| [Zestaw AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) | Włącza szyfrowanie na maszynie Wirtualnej przy użyciu poświadczeń głównej usługi i klucza szyfrowania. |
| [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) | Wyświetla stan procesu szyfrowania maszyny Wirtualnej. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Usuwa grupę zasobów i wszystkie zasoby zawarte w ciągu. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących modułu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](/powershell/azure/overview).

Przykłady skryptów PowerShell dodatkowe maszyny wirtualnej znajdują się w [dokumentacji maszyny Wirtualnej systemu Windows Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
