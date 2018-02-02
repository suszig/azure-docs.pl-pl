---
title: Tworzenie sieci wirtualnej - wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się szybko utworzyć sieć wirtualną przy użyciu wiersza polecenia platformy Azure. Sieć wirtualna umożliwia wiele typów zasobów platformy Azure do prywatnie komunikują się ze sobą."
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
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: b2fc8a622549a9858c6c769a7e648fe07a3d01c1
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Utwórz sieć wirtualną przy użyciu wiersza polecenia platformy Azure

W tym artykule należy Dowiedz się, jak utworzyć sieć wirtualną. Po utworzeniu sieci wirtualnej, wdrożenie dwóch maszyn wirtualnych w sieci wirtualnej i prywatnie komunikacji między nimi.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten przewodnik szybkiego startu będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Aby znaleźć zainstalowanej wersji, uruchom `az --version`. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#az_group_create). Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. 

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*. Wszystkie zasoby platformy Azure są tworzone w lokalizacji platformy Azure (lub regionu).

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-network"></a>Tworzenie sieci wirtualnej

Tworzenie sieci wirtualnej z [tworzenie sieci wirtualnej sieci az](/cli/azure/network/vnet#az_network_vnet_create) polecenia. Poniższy przykład tworzy domyślną sieci wirtualnej o nazwie *myVirtualNetwork* z jednej podsieci o nazwie *domyślne*. Ponieważ lokalizacja nie jest określona, platforma Azure tworzy sieci wirtualnej w tej samej lokalizacji co grupy zasobów.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default
```

Po utworzeniu sieci wirtualnej, stanowi część zwrócone informacje ma następującą składnię.

```azurecli
"newVNet": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/16"
```

Wszystkie sieci wirtualne mają co najmniej jeden prefiksy adresów do nich przypisany. Ponieważ prefiks adresu nie została określona podczas tworzenia sieci wirtualnej, Azure zdefiniowane przestrzeni adresowej 10.0.0.0/16 domyślnie. Przestrzeń adresowa jest określone w notacji CIDR. 10.0.0.0/16 przestrzeni adresowej obejmuje 10.0.0.0-10.0.255.254.

Zwrócony inną część informacji **prefiks adresu** z *10.0.0.0/24* dla *domyślne* podsieci określone w poleceniu. Sieć wirtualna zawiera zero lub więcej podsieci. To polecenie utworzyło pojedynczej podsieci o nazwie *domyślne*, ale nie podano żadnych prefiks adresu podsieci. Jeśli dla sieci wirtualnej lub podsieć nie jest określona prefiks adresu, Azure definiuje 10.0.0.0/24 jako prefiksu adresu dla pierwszej podsieci domyślnie. W związku z tym podsieci obejmuje 10.0.0.0-10.0.0.254, ale tylko 10.0.0.4-10.0.0.254 są dostępne, ponieważ Azure rezerwuje pierwsze cztery adresów (0-3) oraz ostatni adres w każdej podsieci.

## <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Sieć wirtualna umożliwia kilka typów zasobów platformy Azure do prywatnie komunikują się ze sobą. Jeden typ zasobów, które można wdrożyć w sieci wirtualnej jest maszyną wirtualną. Utwórz dwie maszyny wirtualne w sieci wirtualnej, aby można było zweryfikować i zrozumieć sposób działania komunikacji między maszynami wirtualnymi w sieci wirtualnej w kolejnym kroku.

Utwórz maszynę wirtualną z [tworzenia maszyny wirtualnej az](/cli/azure/vm#az_vm_create) polecenia. Poniższy przykład tworzy maszynę wirtualną o nazwie *myVm1*. Jeśli kluczy SSH już nie istnieją w domyślnej lokalizacji klucza, polecenie tworzy je. Aby użyć określonego zestawu kluczy, użyj opcji `--ssh-key-value`. `--no-wait` Opcja tworzy maszynę wirtualną w tle, dzięki czemu można kontynuować do następnego kroku.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --no-wait
```

Azure automatycznie tworzy maszynę wirtualną w *domyślne* podsieć *myVirtualNetwork* wirtualnych sieci, ponieważ sieć wirtualna nie istnieje w grupie zasobów, a nie sieci wirtualnej lub podsieć jest określone w poleceniu. Azure DHCP automatycznie przypisać 10.0.0.4 do maszyny wirtualnej podczas tworzenia, ponieważ jest pierwszy dostępny adres w *domyślne* podsieci. Lokalizacja, w której utworzono maszynę wirtualną w musi być lokalizację, w której sieć wirtualna istnieje w. Maszyna wirtualna nie jest wymagane w tej samej grupie zasobów co maszyny wirtualnej, chociaż w tym artykule.

Tworzenie drugiej maszyny wirtualnej. Domyślnie program Azure tworzy także tej maszyny wirtualnej w *domyślne* podsieci.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Maszyna wirtualna ma kilka minut na utworzenie. Po utworzeniu maszyny wirtualnej Azure CLI zwraca dane wyjściowe podobne do poniższego przykładu: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm1",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

W tym przykładzie widać, że **elementu privateIpAddress** jest *10.0.0.5*. Azure DHCP automatycznie przypisywany *10.0.0.5* do maszyny wirtualnej powodu następnego dostępnego adresu w *domyślne* podsieci. Zwróć uwagę na **publicznego adresu IP**. Ten adres jest używany na dostęp do maszyny wirtualnej z Internetu w kolejnym kroku. Publiczny adres IP nie jest przypisany od w ramach sieci wirtualnej lub prefiksy adresów podsieci. Publiczne adresy IP są przypisywane z [puli adresów przypisanych do każdego regionu Azure](https://www.microsoft.com/download/details.aspx?id=41653). Azure wie, który publiczny adres IP jest przypisany do maszyny wirtualnej, system operacyjny działający na maszynie wirtualnej nie ma informacji o żadnych publicznego adresu IP, które są przypisane do niej.

## <a name="connect-to-a-virtual-machine"></a>Połącz z maszyną wirtualną

Użyj następującego polecenia, aby utworzyć sesję SSH z *myVm2* maszyny wirtualnej. Zastąp `<publicIpAddress>` z publicznym adresem IP maszyny wirtualnej. W powyższym przykładzie adres IP jest *40.68.254.142*.

```bash 
ssh <publicIpAddress>
```

## <a name="validate-communication"></a>Sprawdź poprawność komunikacji

Użyj następującego polecenia, aby potwierdzić komunikację z *myVm1* z *myVm2*:

```bash
ping myVm1 -c 4
```

Otrzymasz cztery odpowiedzi z *10.0.0.4*. Użytkownik może komunikować się z *myVm1* z *myVm2*, ponieważ obie maszyny wirtualne mają przypisane z prywatnych adresów IP *domyślne* podsieci. Mogą na polecenie ping przez hosta o nazwie, ponieważ platforma Azure automatycznie udostępnia rozpoznawanie nazw DNS dla wszystkich hostów w sieci wirtualnej.

Aby potwierdzić wychodzącego komunikację z Internetem, użyj następującego polecenia:

```bash
ping bing.com -c 4
```

Otrzymasz cztery odpowiedzi z bing.com. Domyślnie ruch wychodzący do Internetu może komunikować się żadnej maszyny wirtualnej w sieci wirtualnej.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, można użyć [usunięcie grupy az](/cli/azure/group#az_group_delete) polecenie, aby usunąć grupę zasobów i wszystkie zasoby zawiera. Zakończyć sesję SSH do maszyny Wirtualnej, a następnie usunąć zasoby.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Kolejne kroki

W tym artykule została wdrożona domyślna sieci wirtualnej z jedną podsiecią i dwie maszyny wirtualne. Aby dowiedzieć się, jak utworzyć sieć wirtualną niestandardowe z wieloma podsieciami i wykonywać zadania zarządzania podstawowe, nadal samouczek dotyczący tworzenia niestandardowych sieci wirtualnej i zarządzanie nią.


> [!div class="nextstepaction"]
> [Tworzenie niestandardowych sieci wirtualnej i zarządzanie nim](virtual-networks-create-vnet-arm-pportal.md#azure-cli)
