---
title: "Azure CLI skrypt przykładowy — zaszyfrować Maszynę wirtualną systemu Linux | Dokumentacja firmy Microsoft"
description: "Azure CLI skrypt przykładowy — zaszyfrować Maszynę wirtualną systemu Linux"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 36bbce089ef7626a3e4c433eb9fb2165e7fdcdd1
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/16/2017
---
# <a name="encrypt-a-linux-virtual-machine-in-azure"></a>Szyfrowanie maszyny wirtualnej systemu Linux na platformie Azure

Ten skrypt tworzy bezpieczne usługi Azure Key Vault, klucze szyfrowania, nazwy głównej usługi Azure Active Directory i maszyny wirtualnej systemu Linux (VM). Maszyna wirtualna jest następnie szyfrowany przy użyciu klucza szyfrowania z magazynu kluczy i poświadczenia główne usługi.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń w celu utworzenia grupy zasobów, usługi Azure Key Vault nazwy głównej usługi, maszyny wirtualnej i wszystkie powiązane zasoby. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Utwórz az keyvault](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_create) | Tworzy magazynu kluczy Azure do przechowywania zabezpieczonych danych, takie jak klucze szyfrowania. |
| [Utwórz klucz keyvault az](https://docs.microsoft.com/cli/azure/keyvault/key#az_keyvault_key_create) | Tworzy klucz szyfrowania magazynu kluczy. |
| [ad az sp utworzyć do rbac](https://docs.microsoft.com/cli/azure/ad/sp#az_ad_sp_create_for_rbac) | Tworzy nazwę główną usługi do bezpiecznego uwierzytelniania i kontroli dostępu do kluczy szyfrowania usługi Azure Active Directory. |
| [keyvault az set-policy.](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_set_policy) | Ustawia uprawnienia Key Vault w celu udzielenia dostępu główną usługi do kluczy szyfrowania. |
| [Tworzenie maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Tworzy maszynę wirtualną i podłączony do karty sieciowej, sieci wirtualnej, podsieci i NSG. To polecenie określa również obraz maszyny wirtualnej ma być używany, a poświadczenia administracyjne.  |
| [Włącz szyfrowanie maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm/encryption#az_vm_encryption_enable) | Włącza szyfrowanie na maszynie Wirtualnej przy użyciu poświadczeń głównej usługi i klucza szyfrowania. |
| [Pokaż szyfrowania maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm/encryption#az_vm_encryption_show) | Wyświetla stan procesu szyfrowania maszyny Wirtualnej. |
| [Usuwanie grupy az](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Przykłady skryptów CLI dodatkowe maszyny wirtualnej znajdują się w [dokumentacji maszyny Wirtualnej systemu Linux Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
