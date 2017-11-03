---
title: "Azure CLI przykładowym skrypcie - dysk systemu operacyjnego instalacji | Dokumentacja firmy Microsoft"
description: "Azure CLI przykładowym skrypcie - instalacji dysk systemu operacyjnego"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c32ea5e6cade34a9c8dac0eab523ebcaa10ef039
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>Rozwiązywanie problemów z dyskiem systemu operacyjnego maszyny wirtualne

Ten skrypt instaluje dysk systemu operacyjnego maszyny wirtualnej nie powiodło się lub problemem jako dysku danych do drugiej maszyny wirtualnej. Może to być przydatne podczas rozwiązywania problemów z dysku problemy lub odzyskiwania danych. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do utworzenia grupy zasobów, maszyny wirtualnej i wszystkie powiązane zasoby. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Pokaż wirtualna az](https://docs.microsoft.com/cli/azure/vm#az_vm_show) | Zwraca listę maszyn wirtualnych. W takim przypadku opcji zapytania służy do zwracania dysku systemu operacyjnego maszyny wirtualnej. Ta wartość jest następnie dodawana do nazwy zmiennej "uri". |
| [AZ usuwania maszyny wirtualnej](https://docs.microsoft.com/cli/azure/vm#az_vm_delete) | Usuwa maszynę wirtualną. |
| [Tworzenie maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Tworzy maszynę wirtualną.  |
| [Dołącz az dysku maszyny wirtualnej](https://docs.microsoft.com/cli/azure/vm/disk#az_vm_disk_attach) | Dołącza dysk do maszyny wirtualnej. |
| [AZ vm--adresy ip](https://docs.microsoft.com/cli/azure/vm#az_vm_list_ip_addresses) | Zwraca adresów IP maszyny wirtualnej. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Przykłady skryptów CLI dodatkowe maszyny wirtualnej znajdują się w [dokumentacji maszyny Wirtualnej systemu Linux Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
