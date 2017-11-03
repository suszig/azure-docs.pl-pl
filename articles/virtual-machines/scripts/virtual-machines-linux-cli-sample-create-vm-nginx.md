---
title: "Azure CLI skrypt przykładowy — Utwórz Maszynę wirtualną systemu Linux z NGINX | Dokumentacja firmy Microsoft"
description: "Azure CLI skrypt przykładowy — Utwórz Maszynę wirtualną systemu Linux z NGINX"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0a6c33d84f1fab85e6ed2933c47c041ca2e59520
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-vm-with-nginx"></a>Utwórz maszynę Wirtualną z NGINX

Ten skrypt tworzy maszynę wirtualną platformy Azure i stosuje niestandardowe rozszerzenie skryptu maszyny wirtualnej platformy Azure w celu zainstalowania NGINX. Po uruchomieniu skryptu, można uzyskać dostępu do pokaz witryny sieci Web na publiczny adres IP maszyny wirtualnej.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nginx/create-vm-nginx.sh "Quick Create VM")]

## <a name="custom-script-extension"></a>Rozszerzenie niestandardowego skryptu

Rozszerzenie skryptu niestandardowego kopiuje tego skryptu na maszynie wirtualnej. Następnie uruchomienia skryptu, aby zainstalować i skonfigurować serwer sieci web NGINX. 

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do utworzenia grupy zasobów, maszyny wirtualnej i wszystkie powiązane zasoby. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Tworzenie maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Tworzy maszynę wirtualną. To polecenie określa również obraz maszyny wirtualnej ma być używany, a poświadczenia administracyjne.  |
| [open — port az maszyny wirtualnej](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Tworzy reguły grupy zabezpieczeń sieci, aby zezwalać na ruch przychodzący. W tym przykładzie port 80 jest otwarty dla ruchu HTTP. |
| [zestaw rozszerzeń maszyny wirtualnej platformy Azure](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Dodaje i uruchamia rozszerzenie maszyny wirtualnej do maszyny Wirtualnej. W tym przykładzie niestandardowe rozszerzenie skryptu jest używany do zainstalowania NGINX.|
| [Usuwanie grupy az](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Przykłady skryptów CLI dodatkowe maszyny wirtualnej znajdują się w [dokumentacji maszyny Wirtualnej systemu Linux Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
