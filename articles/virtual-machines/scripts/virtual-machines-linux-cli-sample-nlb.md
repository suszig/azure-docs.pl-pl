---
title: "Azure CLI skrypt przykładowy — Utwórz Maszynę wirtualną systemu Linux z równoważeniem obciążenia Sieciowego | Dokumentacja firmy Microsoft"
description: "Azure CLI skrypt przykładowy — Utwórz Maszynę wirtualną systemu Linux z równoważeniem obciążenia Sieciowego"
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
ms.openlocfilehash: fdddf54dceab23394dd26a5c10fa57b921f8cd34
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-highly-available-vm"></a>Tworzenie maszyny Wirtualnej o wysokiej dostępności

Ten przykładowy skrypt tworzy wszystko, co jest potrzebne do uruchomienia wielu maszyn wirtualnych Ubuntu skonfigurowane w wysokiej dostępności i konfiguracji z równoważeniem obciążenia. Po uruchomieniu skryptu ma trzy maszyny wirtualne, dołączony do zestawu dostępności Azure i jest dostępny za pośrednictwem usługi równoważenia obciążenia Azure. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do utworzenia grupy zasobów, maszyny wirtualnej zestawu dostępności, usługi równoważenia obciążenia i wszystkie powiązane zasoby. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Tworzenie sieci wirtualnej sieci az](https://docs.microsoft.com/cli/azure/network/vnet#az_network_vnet_create) | Tworzy sieć wirtualna platformy Azure i podsieć. |
| [Tworzenie sieci az publicznego ip](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_create) | Tworzy publiczny adres IP z statyczny adres IP i skojarzonej nazwy DNS. |
| [Utwórz równoważeniem obciążenia sieciowego az](https://docs.microsoft.com/cli/azure/network/lb#az_network_lb_create) | Tworzy Azure Usługa równoważenia obciążenia sieciowego (NLB). |
| [Utwórz sondowania równoważeniem obciążenia sieciowego az](https://docs.microsoft.com/cli/azure/network/lb/probe#az_network_lb_probe_create) | Tworzy badanie równoważenia obciążenia Sieciowego. Sonda równoważenia obciążenia Sieciowego jest używana do monitorowania każdej maszyny Wirtualnej w zestawie równoważenia obciążenia Sieciowego. Jeśli żadnej maszyny Wirtualnej staje się niedostępny, ruch nie jest kierowany do maszyny Wirtualnej. |
| [Utwórz regułę równoważeniem obciążenia sieciowego az](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Tworzy regułę równoważenia obciążenia Sieciowego. W tym przykładzie jest tworzona reguła dla portu 80. Jak ruchu HTTP dociera Równoważenie obciążenia Sieciowego, jest kierowany do portu 80 jednej z maszyn wirtualnych w zestawie równoważenia obciążenia Sieciowego. |
| [Utwórz az sieci lb ruchu przychodzącego translatora adresów sieciowych — reguł](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#az_network_lb_inbound_nat_rule_create) | Tworzy regułę równoważenia obciążenia Sieciowego translatora adresów sieciowych (NAT).  Reguły NAT portu Równoważenie obciążenia Sieciowego są mapowane na port na maszynie Wirtualnej. W tym przykładzie tworzona jest reguła NAT dla ruchu SSH do każdej maszyny Wirtualnej w zestawie równoważenia obciążenia Sieciowego.  |
| [Tworzenie grupy nsg sieci az](https://docs.microsoft.com/cli/azure/network/nsg#az_network_nsg_create) | Tworzy sieciową grupę zabezpieczeń (NSG), czyli granicę zabezpieczeń między internet i maszyny wirtualnej. |
| [Tworzenie reguły nsg sieci az](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Umożliwia utworzenie reguły NSG zezwalająca na ruch przychodzący. W tym przykładzie port 22 jest otwarty dla ruchu protokołu SSH. |
| [Utwórz az kart interfejsu sieciowego](https://docs.microsoft.com/cli/azure/network/nic#az_network_nic_create) | Tworzy karty sieci wirtualnej i dołącza go do sieci wirtualnej, podsieci i NSG. |
| [Tworzenie maszyny wirtualnej az zestawu dostępności](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Tworzy zestaw dostępności. Zestawy dostępności upewnij się, czas działania aplikacji poprzez rozłożenie zasobów fizycznych maszyny wirtualnej tak, aby w przypadku niepowodzenia cały zestaw nie jest wykonywane. |
| [Tworzenie maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | Tworzy maszynę wirtualną i podłączony do karty sieciowej, sieci wirtualnej, podsieci i NSG. To polecenie określa również obraz maszyny wirtualnej ma być używane i zapewnić poświadczenia administracyjne.  |
| [Usuwanie grupy az](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Przykłady skryptów CLI dodatkowe maszyny wirtualnej znajdują się w [dokumentacji maszyny Wirtualnej systemu Linux Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
