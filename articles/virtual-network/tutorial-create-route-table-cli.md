---
title: "Kierować ruchem sieciowym - wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak można kierować ruchem sieciowym z tabelą tras za pomocą wiersza polecenia platformy Azure."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/02/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 3c16c774fa1c8a5c8bf50b4f4f9d0bfb283318e3
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-cli"></a>Kierować ruchem sieciowym z tabelą tras za pomocą wiersza polecenia platformy Azure

Azure automatycznie tras ruchu między wszystkich podsieci w sieci wirtualnej, domyślnie. Można utworzyć trasy do przesłonięcia Azure routing domyślny. Możliwość tworzenia niestandardowych tras jest przydatne, jeśli na przykład chcesz kierować ruchem między podsieciami przez zaporę. W tym artykule dowiesz się, jak:

> [!div class="checklist"]
> * Utwórz tabelę tras
> * Utwórz trasę
> * Skojarz tabelę tras z podsiecią sieci wirtualnej
> * Routing testu
> * Rozwiązywanie problemów z routingiem

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli do zainstalowania i używania interfejsu wiersza polecenia lokalnie tego przewodnika Szybki Start, wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.28 lub nowszej. Aby dowiedzieć się, jaka wersja jest używana, uruchom polecenie `az --version`. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli). 

## <a name="create-a-route-table"></a>Utwórz tabelę tras

Azure tras ruchu między wszystkich podsieci w sieci wirtualnej, domyślnie. Aby dowiedzieć się więcej na temat trasy domyślne platformy Azure, zobacz [tras systemowych](virtual-networks-udr-overview.md). Aby zastąpić domyślny Azure routingu, Utwórz tabelę tras, która zawiera trasy i skojarz tabeli tras z podsiecią sieci wirtualnej.

Przed utworzeniem tabelę tras, Utwórz grupę zasobów o [Tworzenie grupy az](/cli/azure/group#az_group_create) dla wszystkich zasobów utworzone w tym artykule. 

```azurecli-interactive
# Create a resource group.
az group create \
  --name myResourceGroup \
  --location eastus
``` 

Utwórz tabelę tras z [utworzyć tabelę tras az sieci](/cli/azure/network/route#az_network_route_table_create). Poniższy przykład tworzy tabelę tras o nazwie *myRouteTablePublic*. 

```azurecli-interactive 
# Create a route table
az network route-table create \
  --resource-group myResourceGroup \
  --name myRouteTablePublic
```

## <a name="create-a-route"></a>Utwórz trasę

Tabela tras zawiera zero lub więcej trasy. Utwórz trasę w tabeli tras z [utwórz trasę tabeli tras sieciowych az](/cli/azure/network/route-table/route#az_network_route_table_route_create). 

```azurecli-interactive
az network route-table route create \
  --name ToPrivateSubnet \
  --resource-group myResourceGroup \
  --route-table-name myRouteTablePublic \
  --address-prefix 10.0.1.0/24 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address 10.0.2.4
``` 

Cały ruch kierowany do prefiksu adresu 10.0.1.0/24 przez urządzenie wirtualne sieci przy użyciu adresu IP 10.0.2.4 kieruje trasy. Urządzenie wirtualne sieci i podsieci z prefiksem adresu określonego są tworzone w kolejnych krokach. Trasy zastępuje domyślne Azure routingu, który kieruje ruchem między podsieciami bezpośrednio. Każda marszruta Określa typ następnego przeskoku. Typ następnego przeskoku nakazuje Azure sposób kierowania ruchu. W tym przykładzie Typ następnego przeskoku jest *VirtualAppliance*. Aby dowiedzieć się więcej na temat wszystkich dostępnych typów następnego przeskoku platformy Azure i kiedy należy używać ich, zobacz [następnego przeskoku typy](virtual-networks-udr-overview.md#custom-routes).

## <a name="associate-a-route-table-to-a-subnet"></a>Skojarz tabelę tras z podsiecią

Aby móc skojarzyć tabelę tras z podsiecią, należy utworzyć sieć wirtualną i podsieć. Utwórz sieć wirtualną z jedną podsiecią z [tworzenie sieci wirtualnej sieci az](/cli/azure/network/vnet#az_network_vnet_create).

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefix 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

Utworzyć dwa dodatkowe podsieci z [Utwórz podsieć sieci wirtualnej sieci az](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create).

```azurecli-interactive
# Create a private subnet.
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24

# Create a DMZ subnet.
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name DMZ \
  --address-prefix 10.0.2.0/24
```

Możesz skojarzyć tabelę tras na zero lub więcej podsieci. Podsieć może mieć zero lub jedną tabelę tras skojarzony. Ruch wychodzący z podsieci jest kierowany na podstawie trasy domyślne platformy Azure i niestandardowych tras, dodane do tabeli tras, który należy powiązać z podsiecią. Skojarz *myRouteTablePublic* tabeli tras do *publicznego* podsieć o [zaktualizować podsieci sieci wirtualnej sieci az](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update).

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Public \
  --resource-group myResourceGroup \
  --route-table myRouteTablePublic
```

Przed wdrożeniem tabel tras do użytku produkcyjnego, zaleca się, że należy dokładnie zapoznać się z [routing na platformie Azure](virtual-networks-udr-overview.md) i [limity Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="test-routing"></a>Routing testu

Aby przetestować routingu, należy utworzyć maszynę wirtualną, która służy jako urządzenie wirtualne sieci, że trasy, który został utworzony w poprzednim kroku kieruje za pośrednictwem. Po utworzeniu urządzenie wirtualne sieci, będzie wdrożyć maszynę wirtualną do *publicznego* i *prywatnej* podsieci. Następnie będzie kierować ruch z *publicznego* podsieci do *prywatnej* podsieci za pośrednictwem sieci urządzenia wirtualnego.

### <a name="create-a-network-virtual-appliance"></a>Utwórz urządzenie sieci wirtualnej

W poprzednim kroku utworzono trasę określone urządzenie wirtualne sieci jako typ następnego przeskoku. Maszynę wirtualną działającą aplikację sieciową jest często określany jako urządzenie wirtualne sieci. W środowiskach produkcyjnych, którą należy wdrożyć urządzenie wirtualne sieci jest często wstępnie skonfigurowane maszyny wirtualnej. Wiele sieci wirtualnych urządzeń są dostępne z [portalu Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1). W tym artykule podstawowa maszyna wirtualna jest utworzona. 

Utwórz urządzenie wirtualne sieci w *DMZ* podsieć o [tworzenia maszyny wirtualnej az](/cli/azure/vm#az_vm_create). Podczas tworzenia maszyny wirtualnej Azure tworzy i przypisuje publicznego adresu IP do maszyny wirtualnej, domyślnie. `--public-ip-address ""` Parametr nakazuje Azure nie można utworzyć i przypisać publicznego adresu IP do maszyny wirtualnej, ponieważ maszyna wirtualna nie musi być połączony z Internetem. Jeśli kluczy SSH już nie istnieją w domyślnej lokalizacji klucza, polecenie tworzy je. Aby użyć określonego zestawu kluczy, użyj opcji `--ssh-key-value`.

```azure-cli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmNva \
  --image UbuntuLTS \
  --public-ip-address "" \
  --subnet DMZ \
  --vnet-name myVirtualNetwork \
  --generate-ssh-keys
```

Maszyna wirtualna ma kilka minut na utworzenie. Nie należy kontynuować do następnego kroku, dopóki Azure zakończy tworzenie maszyny wirtualnej i zwraca dane wyjściowe dotyczące maszyny wirtualnej. W środowiskach produkcyjnych, którą należy wdrożyć urządzenie wirtualne sieci jest często wstępnie skonfigurowane maszyny wirtualnej. Wiele sieci wirtualnych urządzeń są dostępne z [portalu Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1).

Należy włączyć przesyłanie dalej IP dla każdej platformy Azure [interfejsu sieciowego](virtual-network-network-interface.md) dołączony do maszyny wirtualnej, który przesyła dalej ruch kierowany do dowolnego adresu IP, który nie jest przypisany do interfejsu sieciowego. Podczas tworzenia urządzenia wirtualnego sieci w *DMZ* , Azure automatycznie utworzona podsieć karty sieciowej o nazwie *myVmNvaVMNic*, dołączyć interfejsu sieciowego z maszyną wirtualną i przypisany prywatny adres IP *10.0.2.4* do interfejsu sieciowego. Włącz przesyłanie dalej IP dla karty sieciowej z [aktualizacji kart sieciowych az](/cli/azure/network/nic#az_network_nic_update).

```azurecli-interactive
az network nic update \
  --name myVmNvaVMNic \
  --resource-group myResourceGroup \
  --ip-forwarding true
```

Na maszynie wirtualnej systemu operacyjnego lub aplikacji działających w maszynie wirtualnej muszą także do przesyłania dalej ruchu sieciowego. Podczas wdrażania urządzenia wirtualnego sieci w środowisku produkcyjnym urządzenia zwykle filtry, dzienniki lub wykonuje innej funkcji przed przesłaniem ruchu. W tym artykule, system operacyjny po prostu przekazuje cały ruch, który odbiera. Włącz przesyłanie dalej IP w ramach systemu operacyjnego maszyny wirtualnej z [zestaw rozszerzenia maszyny wirtualnej az](/cli/azure/vm/extension#az_vm_extension_set), który wykonuje polecenia, który umożliwia przesyłanie dalej IP w ramach systemu operacyjnego.

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVmNva \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
```
Polecenie może potrwać kilka minut do wykonania.

### <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Utwórz dwie maszyny wirtualne w sieci wirtualnej, aby móc weryfikować tego ruchu z *publicznego* podsieci jest kierowany do *prywatnej* podsieci za pośrednictwem sieci urządzenia wirtualnego w kolejnym kroku. 

Utwórz maszynę wirtualną w *publicznego* podsieć o [tworzenia maszyny wirtualnej az](/cli/azure/vm#az_vm_create). `--no-wait` Parametru zapewnia Azure można wykonać polecenia w tle, dzięki czemu można kontynuować do następnego polecenia. Aby usprawnić w tym artykule, użyto hasła. Klucze są zazwyczaj używane w przypadku wdrożeń produkcyjnych. Użycie kluczy, należy również skonfigurować agenta przekazywania SSH. Aby uzyskać więcej informacji zobacz dokumentację klienta SSH. Zastąp `<replace-with-your-password>` w poniższym poleceniu hasłem wybrane.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmWeb \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --admin-username azureuser \
  --admin-password $adminPassword \
  --no-wait
```

Utwórz maszynę wirtualną w *prywatnej* podsieci.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmMgmt \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --admin-username azureuser \
  --admin-password $adminPassword
```

Maszyna wirtualna ma kilka minut na utworzenie. Po utworzeniu maszyny wirtualnej, interfejsu wiersza polecenia Azure zawiera informacje podobne do poniższego przykładu: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmMgmt",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.1.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```
Zwróć uwagę na **publicznego adresu IP**. Ten adres jest używany na dostęp do maszyny wirtualnej z Internetu w kolejnym kroku.

### <a name="route-traffic-through-a-network-virtual-appliance"></a>Kierować ruchem przez urządzenie wirtualne sieci

Użyj następującego polecenia, aby utworzyć sesję SSH z *myVmMgmt* maszyny wirtualnej. Zastąp  *<publicIpAddress>*  z publicznym adresem IP maszyny wirtualnej. W powyższym przykładzie adres IP jest *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

Po wyświetleniu monitu o podanie hasła, wprowadź hasło wybranym [Tworzenie maszyn wirtualnych](#create-virtual-machines).

Użyj następującego polecenia, aby zainstalować traceroute na *myVmMgmt* maszyny wirtualnej:

```bash 
sudo apt-get install traceroute
```

Użyj następującego polecenia, aby przetestować routingu ruchu sieciowego do *myVmWeb* maszynę wirtualną z *myVmMgmt* maszyny wirtualnej.

```bash
traceroute myvmweb
```

Odpowiedź jest podobny do poniższego przykładu:

```bash
traceroute to myvmweb (10.0.0.4), 30 hops max, 60 byte packets
1  10.0.0.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```

Widać, że ruch jest kierowany bezpośrednio z *myVmMgmt* maszyny wirtualnej do *myVmWeb* maszyny wirtualnej. Trasy domyślne platformy Azure, kierować ruchem między podsieciami. 

Użyj następującego polecenia do SSH *myVmWeb* maszynę wirtualną z *myVmMgmt* maszyny wirtualnej:

```bash 
ssh azureuser@myVmWeb
```

Użyj następującego polecenia, aby zainstalować traceroute na *myVmWeb* maszyny wirtualnej:

```bash 
sudo apt-get install traceroute
```

Użyj następującego polecenia, aby przetestować routingu ruchu sieciowego do *myVmMgmt* maszynę wirtualną z *myVmWeb* maszyny wirtualnej.

```bash
traceroute myvmmgmt
```

Odpowiedź jest podobny do poniższego przykładu:

```bash
traceroute to myvmmgmt (10.0.1.4), 30 hops max, 60 byte packets
1  10.0.2.4 (10.0.2.4)  0.781 ms  0.780 ms  0.775 ms
2  10.0.1.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```
Widać, że pierwszym przeskoku jest 10.0.2.4, który jest prywatny adres IP urządzenia wirtualnego w sieci. Drugi przeskok jest 10.0.1.4, prywatnego adresu IP *myVmMgmt* maszyny wirtualnej. Trasy dodane do *myRouteTablePublic* tabeli tras i powiązanych z *publicznego* podsieci spowodował Azure, aby kierować ruchem przez analizę NVA, a nie bezpośrednio do *prywatnego* podsieci.

Zamknięcie sesji SSH, zarówno *myVmWeb* i *myVmMgmt* maszyn wirtualnych.

## <a name="troubleshoot-routing"></a>Rozwiązywanie problemów z routingiem

Jak przedstawiono w poprzednich krokach Azure stosuje trasy domyślne, które można opcjonalnie zastąpić własnego trasy. Czasami ruchu mogą być kierowane zgodnie z oczekiwaniami, należy. Użyj [az sieci obserwatora Pokaż następnego przeskoku](/cli/azure/network/watcher#az_network_watcher_show_next_hop) do ustalenia, jaki ruch jest przekierowywany między dwiema maszynami wirtualnymi. Na przykład następujące polecenie testów routingu ruchu z *myVmWeb* (10.0.0.4) maszyny wirtualnej do *myVmMgmt* (10.0.1.4) maszyny wirtualnej:

```azurecli-interactive
# Enable network watcher for east region, if you don't already have a network watcher enabled for the region.
az network watcher configure --locations eastus --resource-group myResourceGroup --enabled true

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 10.0.1.4 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVmWeb \
  --out table
```
Następujące dane wyjściowe są zwracane po krótkim okresie oczekiwania:

```azurecli
NextHopIpAddress    NextHopType       RouteTableId
------------------  ---------------- ---------------------------------------------------------------------------------------------------------------------------
10.0.2.4            VirtualAppliance  /subscriptions/<Subscription-Id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/routeTables/myRouteTablePublic
```

Dane wyjściowe informuje, że następnego przeskoku adresu IP dla ruchu z *myVmWeb* do *myVmMgmt* jest 10.0.2.4 ( *myVmNva* maszyny wirtualnej), czy typ następnego przeskoku jest  *VirtualAppliance*, oraz że tabeli tras, która powoduje występowanie routingu jest *myRouteTablePublic*.

Skuteczne trasy dla każdego interfejsu sieciowego są kombinacją trasy domyślne platformy Azure i wszelkie tras zdefiniowanych. Zobacz wszystkie trasy dla interfejsu sieciowego w maszynie wirtualnej z [az sieci karty sieciowej Pokaż obowiązującej--tabeli tras](/cli/azure/network/nic#az_network_nic_show_effective_route_table). Na przykład, aby wyświetlić skuteczne trasy dla *MyVmWebVMNic* interfejsu sieciowego w *myVmWeb* maszyny wirtualnej, wprowadź następujące polecenie:

```azurecli-interactive
az network nic show-effective-route-table \
  --name MyVmWebVMNic \
  --resource-group myResourceGroup
```

Zwracane są wszystkie trasy domyślnej i trasy dodanym w poprzednim kroku.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, użyj [usunięcie grupy az](/cli/azure/group#az_group_delete) można usunąć grupy zasobów i wszystkie zasoby zawiera.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Kolejne kroki

W tym artykule możesz utworzyć tabelę tras i skojarzone go do podsieci. Utworzono sieci urządzenie wirtualne, które kierowany ruch z publicznego podsieci prywatne podsieci. Podczas wdrażania można wielu zasobów platformy Azure w ramach sieci wirtualnej, zasobów w przypadku niektórych usług Azure PaaS nie można wdrożyć w sieci wirtualnej. Możesz nadal ograniczyć dostęp do zasobów pewnych usług Azure PaaS ruch tylko z podsieci sieci wirtualnej jednak. Przejdź do następnego samouczkiem, aby dowiedzieć się, jak ograniczyć dostęp do sieci Azure PaaS zasobów.

> [!div class="nextstepaction"]
> [Ograniczenie dostępu do sieci do PaaS zasobów](virtual-network-service-endpoints-configure.md#azure-cli)
