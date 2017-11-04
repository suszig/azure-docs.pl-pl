---
title: "Azure CLI skryptu przykład — wdrażanie stosu światła w zestawie skalowania równoważeniem obciążenia Machin wirtualna | Dokumentacja firmy Microsoft"
description: "Użyć niestandardowego rozszerzenia skryptu do wdrożenia stosu światła w przypadku obciążenia = skali maszyny wirtualnej ze zrównoważonym ustawiony na platformie Azure."
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
ms.date: 04/05/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: 23170923d7c05c9b7230cf331725250b2a3c0f09
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-the-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a>Wdrażanie stosu światła w zestawie skalowania maszyn wirtualnych z równoważeniem obciążenia

W tym przykładzie tworzy zestaw skali maszyny wirtualnej i stosuje rozszerzenie, które uruchamia niestandardowego skryptu do wdrożenia stosu światła na każdej maszynie wirtualnej w zestawie skalowania.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]

## <a name="connect"></a>Połączenie

Umożliwia podłączanie maszyn wirtualnych i na skalę tego kodu ustawić.

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access the virtual machine scale set")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, zestaw skali i maszyn wirtualnych, a wszystkie powiązane zasoby.

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do utworzenia grupy zasobów, maszyny wirtualnej zestawu dostępności, usługi równoważenia obciążenia i wszystkie powiązane zasoby. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Utwórz az vmss](https://docs.microsoft.com/cli/azure/vmss#az_vmss_create) | Tworzy zestaw skali maszyny wirtualnej |
| [Utwórz regułę równoważeniem obciążenia sieciowego az](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Dodaj punkt końcowy równoważeniem obciążenia |
| [Ustaw rozszerzenie vmss az](https://docs.microsoft.com/cli/azure/vmss/extension#az_vmss_extension_set) | Tworzenie rozszerzenia, uruchamiany skryptu niestandardowego podczas wdrażania maszyny wirtualnej |
| [Aktualizacja vmss az-wystąpienia](https://docs.microsoft.com/cli/azure/vmss#az_vmss_update_instances) | Uruchom skrypt niestandardowe na wystąpień maszyn wirtualnych, które zostały wdrożone, przed rozszerzeniem została zastosowana do zestawu skalowania. |
| [AZ vmss skali](https://docs.microsoft.com/cli/azure/vmss#az_vmss_scale) | Skalowanie w górę skali, dodając więcej wystąpień maszyny Wirtualnej. Niestandardowe skrypt jest uruchamiany na te, gdy są one wdrażane. |
| [Lista ip publicznej sieci az](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_list) | Uzyskaj adresy IP maszyn wirtualnych utworzonych przez próbki. |
| [Pokaż równoważeniem obciążenia sieciowego az](https://docs.microsoft.com/cli/azure/network/lb#az_network_lb_show) | Pobierz frontonu i zaplecza porty używane przez usługi równoważenia obciążenia. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Przykłady skryptów CLI dodatkowe maszyny wirtualnej znajdują się w [dokumentacji maszyny Wirtualnej systemu Linux Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
