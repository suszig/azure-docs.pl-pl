---
title: "Utwórz publiczny obciążenia standardowego równoważenia z strefowo nadmiarowy frontonu adres publiczny adres IP przy użyciu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Jak utworzyć publiczny obciążenia standardowego równoważenia z strefowo nadmiarowy frontonu adres publiczny adres IP przy użyciu wiersza polecenia platformy Azure"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2017
ms.author: kumud
ms.openlocfilehash: 769eb86af3e0506ddf03d1ec616d5a17b7e5f714
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-frontend-using-azure-cli"></a>Utwórz publiczny obciążenia standardowego równoważenia z strefowo nadmiarowy frontonu przy użyciu wiersza polecenia platformy Azure

W tym artykule opisano przez proces tworzenia publiczny [standardowe usługi równoważenia obciążenia](https://aka.ms/azureloadbalancerstandard) z strefowo nadmiarowy frontonu przy użyciu adresu publicznego adresu IP standardowa.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="register-for-availability-zones-load-balancer-standard-and-public-ip-standard-preview"></a>Zarejestruj się, aby Podgląd standardowe strefy dostępności, standardowe usługi równoważenia obciążenia i publicznego adresu IP

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga czy korzystasz z wiersza polecenia platformy Azure w wersji 2.0.17 lub nowszej.  Aby dowiedzieć się, jaka wersja jest używana, uruchom polecenie `az --version`. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

>[!NOTE]
[Standardowy SKU usługi równoważenia obciążenia](https://aka.ms/azureloadbalancerstandard) jest obecnie w przeglądzie. W okresie obowiązywania wersji zapoznawczej ta funkcja może nie oferować dostępności i niezawodności na tym samym poziomie, co funkcje w wersji ogólnodostępnej. Aby uzyskać więcej informacji, zobacz [Dodatkowe warunki użytkowania dotyczące wersji zapoznawczych platformy Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Użyj ogólnie dostępna [podstawowy SKU usługi równoważenia obciążenia](load-balancer-overview.md) dla usług produkcji. 

> [!NOTE]
> Dostępność strefy są w wersji zapoznawczej i są gotowe do programowania i testowania scenariuszy. Obsługa jest dostępna dla wybierz zasobów platformy Azure i regiony i rodziny rozmiar maszyny Wirtualnej. Więcej informacji na temat rozpocząć, które zasobów platformy Azure, regiony i rodziny rozmiar maszyny Wirtualnej można spróbować stref dostępności przy, zobacz [Przegląd stref dostępności](https://docs.microsoft.com/azure/availability-zones/az-overview). Aby uzyskać pomoc techniczną, możesz skorzystać z witryny [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) lub [otworzyć bilet pomocy technicznej platformy Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

Przed wybraniem strefy lub strefowo nadmiarowy opcję frontonu publicznego adresu IP usługi równoważenia obciążenia, należy najpierw wykonać kroki w [zarejestrować w wersji zapoznawczej stref dostępności](https://docs.microsoft.com/azure/availability-zones/az-overview).

Upewnij się, że zainstalowano najnowszą [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)i zalogować się do konta platformy Azure z [logowania az](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest#login).

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów, przy użyciu następującego polecenia:

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

## <a name="create-a-public-ip-standard"></a>Utwórz publiczny Standard IP

Tworzenie publicznego adresu IP standardowe przy użyciu następującego polecenia:

```azurecli-interactive
az network public-ip create --resource-group myResourceGroup --name myPublicIP --sku Standard
```

## <a name="create-a-load-balancer"></a>Tworzenie modułu równoważenia obciążenia

Utwórz publiczny obciążenia standardowego modułu równoważenia o standardowych publicznego adresu IP, utworzony w poprzednim kroku przy użyciu następującego polecenia:

```azurecli-interactive
az network lb create --resource-group myResourceGroup --name myLoadBalancer --public-ip-address myPublicIP --frontend-ip-name myFrontEndPool --backend-pool-name myBackEndPool --sku Standard
```

## <a name="create-an-lb-probe-on-port-80"></a>Utworzyć sondy równoważeniem obciążenia na porcie 80

Utwórz sondy kondycji modułu równoważenia obciążenia za pomocą następującego polecenia:

```azurecli-interactive
az network lb probe create --resource-group myResourceGroup --lb-name myLoadBalancer \
  --name myHealthProbe --protocol tcp --port 80
```

## <a name="create-an-lb-rule-for-port-80"></a>Tworzenie reguły LB dla portu 80

Utwórz regułę modułu równoważenia obciążenia za pomocą następującego polecenia:

```azurecli-interactive
az network lb rule create --resource-group myResourceGroup --lb-name myLoadBalancer --name myLoadBalancerRuleWeb \
  --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-pool-name myBackEndPool --probe-name myHealthProbe
```

## <a name="next-steps"></a>Następne kroki
- Dowiedz się, jak [tworzenie publicznego adresu IP w strefie dostępności](../virtual-network/create-public-ip-availability-zone-cli.md)



