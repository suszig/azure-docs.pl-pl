---
title: "Azure CLI przykładowym skrypcie — ruch sieciowy przez urządzenie wirtualne sieci | Dokumentacja firmy Microsoft"
description: "Przykładowy skrypt usługi Azure interfejsu wiersza polecenia — ruch trasy przez urządzenie wirtualne zapory w sieci."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: af6e9d55038acba194f3cc631ff1b4e2e371852c
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Kierować ruchem przez urządzenie wirtualne sieci

Ten przykładowy skrypt tworzy sieć wirtualną z podsieci frontonu i zaplecza. Tworzy również Maszynę wirtualną z przekazywanie IP jest włączone, można kierować ruchem między dwiema podsieciami. Po uruchomieniu skryptu można wdrażać oprogramowanie sieci, takie jak aplikacja zapory, do maszyny Wirtualnej.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Przykładowy skrypt


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do utworzenia grupy zasobów, sieć wirtualną i grup zabezpieczeń sieci. Każde polecenie w tabeli łącza do dokumentacji specyficzne dla polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Tworzenie sieci wirtualnej sieci az](/cli/azure/network/vnet#az_network_vnet_create) | Tworzy sieć wirtualna platformy Azure i podsieci frontonu. |
| [Utwórz podsieć sieci az](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Tworzy zaplecza i strefą DMZ podsieci. |
| [Tworzenie sieci az publicznego ip](/cli/azure/network/public-ip#az_network_public_ip_create) | Tworzy publiczny adres IP na dostęp do maszyny Wirtualnej z Internetu. |
| [Utwórz az kart interfejsu sieciowego](/cli/azure/network/nic#az_network_nic_create) | Tworzy interfejs sieci wirtualnej i Włącz przesyłanie dalej IP dla niego. |
| [Tworzenie grupy nsg sieci az](/cli/azure/network/nsg#az_network_nsg_create) | Tworzy sieciową grupę zabezpieczeń (NSG). |
| [Tworzenie reguły nsg sieci az](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Tworzy reguły NSG, które zezwala na portach HTTP i HTTPS ruchu przychodzącego do maszyny Wirtualnej. |
| [Aktualizacja podsieci sieci wirtualnej sieci az](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update)| Powoduje skojarzenie grupy NSG i tabele tras do podsieci. |
| [Utwórz tabelę tras az sieci](/cli/azure/network/route-table#az_network_route_table_create)| Tworzy tabelę tras dla wszystkich tras. |
| [Utwórz trasę tabeli tras sieciowych az](/cli/azure/network/route-table/route#az_network_route_table_route_create)| Tworzy tras można kierować ruchem między podsieciami i Internetem za pośrednictwem maszyny Wirtualnej. |
| [Tworzenie maszyny wirtualnej az](/cli/azure/vm#az_vm_create) | Tworzy maszynę wirtualną i dołącza do karty Sieciowej. To polecenie określa również obraz maszyny wirtualnej do użycia i poświadczenia administracyjne. |
| [Usuwanie grupy az](/cli/azure/group#az_group_delete) | Usuwa grupę zasobów i wszystkie zasoby, które zawiera. |

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](/cli/azure/overview).

Dodatkowe przykłady skryptów sieci interfejsu wiersza polecenia można znaleźć w [Azure Przegląd dokumentacji](../cli-samples.md)