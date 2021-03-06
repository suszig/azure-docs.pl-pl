---
title: Azure CLI przykładowym skrypcie - ruchu sieciowego maszyn wirtualnych filtr | Dokumentacja firmy Microsoft
description: Azure CLI skrypt przykładowy — filtrowania ruchu przychodzącego i wychodzącego ruchu sieciowego maszyny Wirtualnej.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: bfc894ea718205ac77be48552f6cb5a076572395
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>Filtrowanie ruchu przychodzącego i wychodzącego ruchu sieciowego maszyny Wirtualnej

Ten przykładowy skrypt tworzy sieć wirtualną z podsieci frontonu i zaplecza. Ruch sieciowy przychodzący do podsieci frontonu może zawierać maksymalnie HTTP, HTTPS i SSH, gdy ruch wychodzący do Internetu z podsieci wewnętrznej jest niedozwolone. Po uruchomieniu skryptu, ma jedną maszynę wirtualną z dwiema kartami sieciowymi. Każda karta sieciowa jest podłączona do innej podsieci.

Skrypt można wykonywać z platformy Azure [powłoki chmury](https://shell.azure.com/bash), lub z lokalną instalacją wiersza polecenia platformy Azure. Jeśli używasz interfejsu wiersza polecenia lokalnie, ten skrypt wymaga, że używasz wersji 2.0.28 lub nowszej. Aby znaleźć zainstalowanej wersji, uruchom `az --version`. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli). Jeśli używasz interfejsu wiersza polecenia lokalnie, należy uruchomić `az login` można utworzyć połączenia z platformą Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby:

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Objaśnienia dla skryptu

Ten skrypt używa następujących poleceń do utworzenia grupy zasobów, sieć wirtualną i grup zabezpieczeń sieci. Każde polecenie w następujących łączy tabeli w dokumentacji konkretnego polecenia:

| Polecenie | Uwagi |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w której są przechowywane wszystkie zasoby. |
| [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) | Tworzy sieć wirtualna platformy Azure i podsieci frontonu. |
| [Utwórz podsieć sieci az](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Tworzy podsieć zaplecza. |
| [Aktualizacja podsieci sieci wirtualnej sieci az](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update) | Kojarzy grup NSG do podsieci. |
| [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) | Tworzy publiczny adres IP na dostęp do maszyny Wirtualnej z Internetu. |
| [az network nic create](/cli/azure/network/nic#az_network_nic_create) | Tworzy interfejsy sieci wirtualnej i dołącza je do podsieci sieci wirtualnej frontonu i zaplecza. |
| [az network nsg create](/cli/azure/network/nsg#az_network_nsg_create) | Tworzy sieciowej grupy zabezpieczeń (NSG), które są skojarzone z podsieci frontonu i zaplecza. |
| [az network nsg rule create](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) |Tworzy reguły NSG, które zezwala lub blokuje określone porty do określonych podsieci. |
| [az vm create](/cli/azure/vm#az_vm_create) | Tworzy maszyny wirtualne i dołącza karty Sieciowej na każdej maszynie Wirtualnej. To polecenie określa również obraz maszyny wirtualnej do użycia i poświadczenia administracyjne. |
| [az group delete](/cli/azure/group#az_group_delete) | Usuwa grupę zasobów i wszystkie zasoby, które zawiera. |

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat interfejsu wiersza polecenia platformy Azure, zobacz [dokumentację interfejsu wiersza polecenia platformy Azure](/cli/azure).

Przykłady skryptów dodatkowe sieci wirtualnej interfejsu wiersza polecenia można znaleźć w [przykłady interfejsu wiersza polecenia sieci wirtualnej](../cli-samples.md).
