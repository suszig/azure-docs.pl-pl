---
title: "Utwórz zoned adres publiczny adres IP z wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Utwórz publiczny adres IP w strefie dostępności z wiersza polecenia platformy Azure."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: ef93d43bbd0c58950027810c8c335d9076574326
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-public-ip-address-in-an-availability-zone-with-the-azure-cli"></a>Tworzenie publicznego adresu IP w strefie dostępności z wiersza polecenia platformy Azure

Można wdrożyć publicznego adresu IP w strefie dostępności Azure (wersja zapoznawcza). Strefa dostępności jest fizycznie oddzielnych stref w regionie platformy Azure. Instrukcje:

> * Tworzenie publicznego adresu IP w strefie dostępności
> * Zidentyfikuj powiązane zasoby utworzone w strefie dostępności

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Jeśli do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku, wymaga czy korzystasz z wersji interfejsu wiersza polecenia Azure nowsza niż wersja 2.0.17. Aby dowiedzieć się, jaka wersja jest używana, uruchom polecenie `az --version`. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

> [!NOTE]
> Dostępność strefy są w wersji zapoznawczej i są gotowe do programowania i testowania scenariuszy. Obsługa jest dostępna dla wybierz zasobów platformy Azure i regiony i rodziny rozmiar maszyny Wirtualnej. Więcej informacji na temat rozpocząć, które zasobów platformy Azure, regiony i rodziny rozmiar maszyny Wirtualnej można spróbować stref dostępności przy, zobacz [Przegląd stref dostępności](https://docs.microsoft.com/azure/availability-zones/az-overview). Aby uzyskać pomoc techniczną, możesz skorzystać z witryny [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) lub [otworzyć bilet pomocy technicznej platformy Azure](../azure-supportability/how-to-create-azure-support-request.md).

## <a name="create-a-zonal-public-ip-address"></a>Utwórz zonal publicznego adresu IP

Utwórz publiczny adres IP w strefie dostępności przy użyciu następującego polecenia:


```azurecli-interactive
az network public-ip create --resource-group myResourceGroup --name myPublicIp --zone 2 --location westeurope
```
> [!NOTE]
> Po przypisaniu standardowego publicznego adresu IP jednostki SKU do interfejsu sieciowego maszyny wirtualnej musisz jawnie zezwolić na ruch do miejsca przeznaczenia przy użyciu [sieciowej grupy zabezpieczeń](security-overview.md#network-security-groups). Próba komunikacji z zasobem będzie kończyć się niepowodzeniem do momentu utworzenia i skojarzenia sieciowej grupy zabezpieczeń, a następnie jawnego zezwolenia na żądany ruch.

## <a name="get-zone-information-about-a-public-ip-address"></a>Pobierz informacje strefy publicznego adresu IP

Uzyskaj informacje o strefie publicznego adresu IP za pomocą następującego polecenia:

```azurecli-interactive
az network public-ip show --resource-group myResourceGroup --name myPublicIp
```

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [stref dostępności](https://docs.microsoft.com/azure/availability-zones/az-overview)
- Dowiedz się więcej o [publicznych adresów IP](../virtual-network/virtual-network-public-ip-address.md) 
