---
title: "Azure CLI przykładowym skrypcie - ruchu sieciowego maszyn wirtualnych filtr | Dokumentacja firmy Microsoft"
description: "Azure CLI skrypt przykładowy — filtrowania ruchu przychodzącego i wychodzącego ruchu sieciowego maszyny Wirtualnej."
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
ms.openlocfilehash: 68ee013cff4e0be15af30239e0314f779f50177a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>Filtrowanie ruchu przychodzącego i wychodzącego ruchu sieciowego maszyny Wirtualnej

Ten przykładowy skrypt tworzy sieć wirtualną z podsieci frontonu i zaplecza. Ruch sieciowy przychodzący do podsieci frontonu może zawierać maksymalnie HTTP, HTTPS i SSH, gdy ruch wychodzący do Internetu z podsieci wewnętrznej jest niedozwolone. Po uruchomieniu skryptu, ma jedną maszynę wirtualną z dwiema kartami sieciowymi. Każda karta sieciowa jest podłączona do innej podsieci.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt


[!code-azurecli-interactive[główne](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "ruchu sieciowego maszyn wirtualnych filtru")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do utworzenia grupy zasobów, sieć wirtualną i grup zabezpieczeń sieci. Każde polecenie w tabeli łącza do dokumentacji specyficzne dla polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](/cli/azure/group#create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Tworzenie sieci wirtualnej sieci az](/cli/azure/network/vnet#create) | Tworzy sieć wirtualna platformy Azure i podsieci frontonu. |
| [Utwórz podsieć sieci az](/cli/azure/network/vnet/subnet#create) | Tworzy podsieć zaplecza. |
| [Aktualizacja podsieci sieci wirtualnej sieci az](/cli/azure/network/vnet/subnet#update) | Kojarzy grup NSG do podsieci. |
| [Tworzenie sieci az publicznego ip](/cli/azure/network/public-ip#create) | Tworzy publiczny adres IP na dostęp do maszyny Wirtualnej z Internetu. |
| [Utwórz az kart interfejsu sieciowego](/cli/azure/network/nic#create) | Tworzy interfejsy sieci wirtualnej i dołącza je do podsieci sieci wirtualnej frontonu i zaplecza. |
| [Tworzenie grupy nsg sieci az](/cli/azure/network/nsg#create) | Tworzy sieciowej grupy zabezpieczeń (NSG), które są skojarzone z podsieci frontonu i zaplecza. |
| [Tworzenie reguły nsg sieci az](/cli/azure/network/nsg/rule#create) |Tworzy reguły NSG, które zezwala lub blokuje określone porty do określonych podsieci. |
| [Tworzenie maszyny wirtualnej az](/cli/azure/vm#create) | Tworzy maszyny wirtualne i dołącza karty Sieciowej na każdej maszynie Wirtualnej. To polecenie określa również obraz maszyny wirtualnej do użycia i poświadczenia administracyjne. |
| [Usuwanie grupy az](/cli/azure/group#delete) | Usuwa grupę zasobów i wszystkie zasoby, które zawiera. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](/cli/azure/overview).

Dodatkowe przykłady skryptów sieci interfejsu wiersza polecenia można znaleźć w [Azure Przegląd dokumentacji](../cli-samples.md)