---
title: "Azure CLI skrypt przykładowy — Utwórz Maszynę wirtualną z wirtualnego dysku twardego | Dokumentacja firmy Microsoft"
description: "Azure CLI skrypt przykładowy — Utwórz Maszynę wirtualną przy użyciu wirtualnego dysku twardego."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/09/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: 6234473d9f7f0eb18ea85e52273eb82a9ce04da5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a>Utwórz maszynę Wirtualną z wirtualnym dyskiem twardym

W tym przykładzie jest tworzony przy użyciu dysku VHD maszyny wirtualnej.
Tworzy grupę zasobów, konto magazynu i kontener, a następnie tworzy Maszynę wirtualną, przekazując wirtualny dysk twardy do kontenera.
Zastępuje ssh publicznego klucza z klucza publicznego, więc musisz mieć dostęp do maszyny Wirtualnej.

Będziesz potrzebować rozruchowy wirtualny dysk twardy.
Możesz pobrać dysku VHD, które zostały użyte z https://azclisamples.blob.core.windows.net/vhds/sample.vhd, lub użyć własnych wirtualnego dysku twardego. Skrypt jest szuka `~/sample.vhd`.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do utworzenia grupy zasobów, maszyny wirtualnej zestawu dostępności, usługi równoważenia obciążenia i wszystkie powiązane zasoby. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Lista kont magazynu az](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_list) | Listy kont magazynu |
| [Sprawdź — nazwa az konta magazynu](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_check_name) | Sprawdza, czy nazwa konta magazynu jest prawidłowa i że jeszcze nie istnieje |
| [Lista kluczy kont magazynu az](https://docs.microsoft.com/cli/azure/storage/account/keys#az_storage_account_keys_list) | Wyświetla listę kluczy dla konta magazynu |
| [istnieje az obiektu blob magazynu](https://docs.microsoft.com/cli/azure/storage/blob#az_storage_blob_exists) | Sprawdza, czy istnieje obiektu blob |
| [Tworzenie kontenera magazynu az](https://docs.microsoft.com/cli/azure/storage/container#az_storage_container_create) | Tworzy kontener na koncie magazynu. |
| [Przekazywanie obiektu blob magazynu az](https://docs.microsoft.com/cli/azure/storage/blob#az_storage_blob_upload) | Tworzy obiekt blob w kontenerze, przekazując wirtualny dysk twardy. |
| [AZ listy maszyny wirtualnej](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | Używane z `--query` Sprawdź, czy nazwa maszyny Wirtualnej jest w użyciu. | 
| [Tworzenie maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | Tworzy maszyny wirtualne. |
| [AZ wirtualna dostęp zestaw linux użytkownika](https://docs.microsoft.com/cli/azure/vm/access#az_vm_access_set_linux_user) | Resetuje klucz SSH, aby dać dostęp bieżącego użytkownika do maszyny Wirtualnej. |
| [AZ vm--adresy ip](https://docs.microsoft.com/cli/azure/vm#az_vm_list-ip-addresses) | Pobiera adres IP maszyny wirtualnej, który został utworzony. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Przykłady skryptów CLI dodatkowe maszyny wirtualnej znajdują się w [dokumentacji maszyny Wirtualnej systemu Linux Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
