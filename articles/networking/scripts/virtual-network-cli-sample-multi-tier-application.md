---
title: "Azure CLI skrypt przykładowy — tworzenie sieci dla aplikacji wielowarstwowych | Dokumentacja firmy Microsoft"
description: "Azure CLI skrypt przykładowy — tworzenie sieci wirtualnej dla aplikacji wielowarstwowych."
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
ms.openlocfilehash: de65d820f2d9eea49b58185c81d815675fd76740
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a>Tworzenie sieci dla aplikacji wielowarstwowych

Ten przykładowy skrypt tworzy sieć wirtualną z podsieci frontonu i zaplecza. Ruch do podsieci frontonu jest ograniczona do HTTP i SSH, gdy ruch do podsieci wewnętrznej jest ograniczony do MySQL, port 3306. Po uruchomieniu skryptu, masz dwie maszyny wirtualne, w każdej podsieci, którą można wdrożyć serwera sieci web i MySQL oprogramowania.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Przykładowy skrypt


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]

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
| [Tworzenie sieci az publicznego ip](/cli/azure/network/public-ip#create) | Tworzy publiczny adres IP na dostęp do maszyny Wirtualnej z Internetu. |
| [Utwórz az kart interfejsu sieciowego](/cli/azure/network/nic#create) | Tworzy interfejsy sieci wirtualnej i dołącza je do podsieci sieci wirtualnej frontonu i zaplecza. |
| [Tworzenie grupy nsg sieci az](/cli/azure/network/nsg#create) | Tworzy sieciowej grupy zabezpieczeń (NSG), które są skojarzone z podsieci frontonu i zaplecza. |
| [Tworzenie reguły nsg sieci az](/cli/azure/network/nsg/rule#create) |Tworzy reguły NSG, które zezwala lub blokuje określone porty do określonych podsieci. |
| [Tworzenie maszyny wirtualnej az](/cli/azure/vm#create) | Tworzy maszyny wirtualne i dołącza karty Sieciowej na każdej maszynie Wirtualnej. To polecenie określa również obraz maszyny wirtualnej do użycia i poświadczenia administracyjne. |
| [Usuwanie grupy az](/cli/azure/group#delete) | Usuwa grupę zasobów i wszystkie zasoby, które zawiera. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](/cli/azure/overview).

Dodatkowe przykłady skryptów sieci interfejsu wiersza polecenia można znaleźć w [Azure Przegląd dokumentacji](../cli-samples.md)