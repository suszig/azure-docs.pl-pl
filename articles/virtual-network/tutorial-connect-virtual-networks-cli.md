---
title: "Uzyskuj dostęp do sieci wirtualnych sieci wirtualnej komunikacji równorzędnej - wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak połączyć sieci wirtualnej z sieci wirtualnej komunikacji równorzędnej."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: bbf2e757e2d9ad76c59394ba0138a61fd4029d15
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-cli"></a>Uzyskuj dostęp do sieci wirtualnych sieci wirtualnej komunikacji równorzędnej przy użyciu wiersza polecenia platformy Azure

Sieci wirtualne można połączyć ze sobą z sieci wirtualnej komunikacji równorzędnej. Po połączyć się za pomocą sieci wirtualnych, zasobów w obie sieci wirtualne są mogły komunikować się ze sobą, z tym samym opóźnienia i przepustowości tak, jakby był zasoby w tej samej sieci wirtualnej. W tym artykule dowiesz się, jak:

> [!div class="checklist"]
> * Utwórz dwie sieci wirtualne
> * Połącz dwie sieci wirtualnej z sieci wirtualnej komunikacji równorzędnej
> * Wdróż maszynę wirtualną (VM) do każdej sieci wirtualnej
> * Komunikację między maszynami wirtualnymi

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli do zainstalowania i używania interfejsu wiersza polecenia lokalnie tego przewodnika Szybki Start, wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.28 lub nowszej. Aby dowiedzieć się, jaka wersja jest używana, uruchom polecenie `az --version`. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli). 

## <a name="create-virtual-networks"></a>Tworzenie sieci wirtualnych

Przed utworzeniem sieci wirtualnej, należy utworzyć grupę zasobów dla sieci wirtualnej i innych zasobów utworzone w tym artykule. Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#az_group_create). Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Utwórz sieć wirtualną za pomocą polecenia [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create). Poniższy przykład tworzy sieć wirtualną o nazwie *myVirtualNetwork1* z prefiksem adresu *10.0.0.0/16*.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.0.0.0/24
```

Tworzenie sieci wirtualnej o nazwie *myVirtualNetwork2* z prefiksem adresu *10.1.0.0/16*:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --address-prefixes 10.1.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.1.0.0/24
```

## <a name="peer-virtual-networks"></a>Sieci wirtualne elementów równorzędnych

Ustanowiono komunikacji równorzędnych między identyfikatory sieci wirtualnej, tak, należy najpierw uzyskać identyfikator każdej sieci wirtualnej z [az sieci vnet show](/cli/azure/network/vnet#az_network_vnet_show) i przechowywać w zmiennej Identyfikatora.

```azurecli-interactive
# Get the id for myVirtualNetwork1.
vNet1Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork1 \
  --query id --out tsv)

# Get the id for myVirtualNetwork2.
vNet2Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork2 \
  --query id \
  --out tsv)
```

Utwórz komunikacji równorzędnej z *myVirtualNetwork1* do *myVirtualNetwork2* z [az sieci vnet równorzędna utworzyć](/cli/azure/network/vnet/peering#az_network_vnet_peering_create). Jeśli `--allow-vnet-access` parametr nie zostanie określony, komunikacja równorzędna została ustanowiona, ale brak komunikacji może przepływać za jego pośrednictwem.

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --remote-vnet-id $vNet2Id \
  --allow-vnet-access
```

W danych wyjściowych zwrócona po wykonaniu poprzedniego polecenia, zobacz który **peeringState** jest *zainicjowano*. Komunikacji równorzędnej pozostaje w *zainicjowano* stanu, dopóki nie zostaną utworzone komunikacji równorzędnej z *myVirtualNetwork2* do *myVirtualNetwork1*. Utwórz komunikacji równorzędnej z *myVirtualNetwork2* do *myVirtualNetwork1*. 

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork2-myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork2 \
  --remote-vnet-id $vNet1Id \
  --allow-vnet-access
```

W danych wyjściowych zwrócona po wykonaniu poprzedniego polecenia, zobacz który **peeringState** jest *połączony*. Azure również zmienić stan komunikacji równorzędnej *myVirtualNetwork1 myVirtualNetwork2* komunikacji równorzędnej do *połączony*. Upewnij się, że stan komunikacji równorzędnej *myVirtualNetwork1 myVirtualNetwork2* równorzędna zmieniony na *połączony* z [az sieci vnet show komunikacji równorzędnej](/cli/azure/network/vnet/peering#az_network_vnet_peering_show).

```azurecli-interactive
az network vnet peering show \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --query peeringState
```

Zasoby w jednej sieci wirtualnej nie może komunikować się z zasobami w innych sieci wirtualnej do **peeringState** dla komunikacji równorzędnych w obu sieci wirtualnych jest *połączony*. 

## <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Tworzenie maszyny Wirtualnej w każdej sieci wirtualnej, dzięki czemu użytkownik może komunikować się między nimi w kolejnym kroku.

### <a name="create-the-first-vm"></a>Tworzenie pierwszej maszyny Wirtualnej

Utwórz maszynę wirtualną za pomocą polecenia [az vm create](/cli/azure/vm#az_vm_create). Poniższy przykład tworzy Maszynę wirtualną o nazwie *myVm1* w *myVirtualNetwork1* sieci wirtualnej. Jeśli kluczy SSH już nie istnieją w domyślnej lokalizacji klucza, polecenie tworzy je. Aby użyć określonego zestawu kluczy, użyj opcji `--ssh-key-value`. `--no-wait` Opcja tworzy maszynę Wirtualną w tle, dzięki czemu można kontynuować do następnego kroku.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork1 \
  --subnet Subnet1 \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>Tworzenie drugiej maszyny Wirtualnej

Utwórz maszynę Wirtualną w *myVirtualNetwork2* sieci wirtualnej.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork2 \
  --subnet Subnet1 \
  --generate-ssh-keys
```

Maszyna wirtualna ma kilka minut na utworzenie. Po utworzeniu maszyny Wirtualnej, interfejsu wiersza polecenia Azure zawiera informacje podobne do poniższego przykładu: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm2",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.1.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

Zwróć uwagę na **publicznego adresu IP**. Ten adres jest używany na dostęp do maszyny Wirtualnej z Internetu w kolejnym kroku.

## <a name="communicate-between-vms"></a>Komunikację między maszynami wirtualnymi

Użyj następującego polecenia, aby utworzyć sesję SSH z *myVm2* maszyny Wirtualnej. Zastąp `<publicIpAddress>` z publicznym adresem IP w sieci maszyny wirtualnej. W poprzednim przykładzie, publiczny adres IP jest *13.90.242.231*.

```bash 
ssh <publicIpAddress>
```

Polecenie ping maszyny Wirtualnej w ramach *myVirtualNetwork1*.

```bash 
ping 10.0.0.4 -c 4
```

Otrzymasz cztery odpowiedzi. 

Zamknąć sesję SSH *myVm2* maszyny Wirtualnej. 

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, użyj [usunięcie grupy az](/cli/azure/group#az_group_delete) można usunąć grupy zasobów i wszystkie zasoby zawiera.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

**<a name="register"></a>Zarejestrować w wersji zapoznawczej komunikacji równorzędnej globalnej sieci wirtualnej**

Łączenie sieci wirtualnych za pomocą komunikacji równorzędnej w tym samym regionie jest ogólnie dostępne. Równorzędna sieci wirtualnych w różnych regionach jest obecnie w przeglądzie. Zobacz [aktualizacje sieci wirtualnej](https://azure.microsoft.com/updates/?product=virtual-network) dla dostępnych regionów. -To-peer sieci wirtualnych w regionach, najpierw należy zarejestrować w preview, wykonując następujące czynności (w ramach subskrypcji, w każdej sieci wirtualnej, który chcesz elementu równorzędnego jest w):

1. Zarejestruj w wersji zapoznawczej, wprowadzając następujące polecenia:

  ```azurecli-interactive
  az feature register --name AllowGlobalVnetPeering --namespace Microsoft.Network
  az provider register --name Microsoft.Network
  ```

2. Upewnij się, że są zarejestrowane dla wersji zapoznawczej, wprowadzając następujące polecenie:

  ```azurecli-interactive
  az feature show --name AllowGlobalVnetPeering --namespace Microsoft.Network
  ```

  Jeśli spróbujesz elementów równorzędnych sieci wirtualnych w różnych regionach przed **RegistrationState** dane wyjściowe po wprowadzeniu poprzedniego polecenia jest wyświetlany **zarejestrowanej** obu subskrypcji, równorzędna kończy się niepowodzeniem .

## <a name="next-steps"></a>Kolejne kroki

W tym artykule przedstawiono sposób połączyć dwie sieci z sieci wirtualnej komunikacji równorzędnej. W tym artykule przedstawiono sposób połączyć dwie sieci w tej samej lokalizacji platformy Azure z sieci wirtualnej komunikacji równorzędnej. Można również elementów równorzędnych sieci wirtualnych w [różnych regionach](#register)w [różnych subskrypcji Azure](create-peering-different-subscriptions.md#portal) i tworzenia [gwiazdy projektów sieci](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) z komunikacji równorzędnej. Przed równorzędna sieci wirtualnych w środowisku produkcyjnym, zalecane jest, że należy dokładnie zapoznać się z [Omówienie komunikacji równorzędnej](virtual-network-peering-overview.md), [Zarządzanie równorzędna](virtual-network-manage-peering.md), i [sieci wirtualnej limity](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

Możesz [połączyć swojego komputera do sieci wirtualnej](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) za pośrednictwem sieci VPN i interakcji z zasobami w sieci wirtualnej lub połączyć za pomocą sieci wirtualnych. Nadal przykłady skryptów dla skryptów wielokrotnego użytku do wykonania wielu zadań omówione w artykułach sieci wirtualnej.

> [!div class="nextstepaction"]
> [Przykłady skryptów sieci wirtualnej](../networking/cli-samples.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
