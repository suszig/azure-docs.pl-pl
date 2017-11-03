---
title: "Otwórz porty do maszyny Wirtualnej systemu Linux z 2.0 interfejsu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak otworzyć port / utworzyć punktu końcowego maszyny Wirtualnej systemu Linux przy użyciu modelu wdrażania Menedżera zasobów platformy Azure i 2.0 interfejsu wiersza polecenia platformy Azure"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: eef9842b-495a-46cf-99a6-74e49807e74e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: d176187fe465264b5f433260de5178b48ca9dd4a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="open-ports-and-endpoints-to-a-linux-vm-with-the-azure-cli"></a>Otwórz porty i punktów końcowych do maszyny Wirtualnej systemu Linux z wiersza polecenia platformy Azure
Otwarcie portu lub utworzyć punkt końcowy z maszyną wirtualną (VM) na platformie Azure, tworząc filtr sieci w podsieci lub interfejsu sieciowego maszyny Wirtualnej. Te filtry, które kontrolują ruchu przychodzącego i wychodzącego, można umieścić na grupę zabezpieczeń sieci dołączony do tego zasobu, który odbiera ruch. Użyjmy typowym przykładem ruchu w sieci web na porcie 80. W tym artykule przedstawiono sposób Otwórz port do maszyny Wirtualnej z 2.0 interfejsu wiersza polecenia platformy Azure. Czynności te można również wykonać przy użyciu [interfejsu wiersza polecenia platformy Azure w wersji 1.0](nsg-quickstart-nodejs.md).


## <a name="quick-commands"></a>Szybkie polecenia
Można utworzyć grupy zabezpieczeń sieci i zasad należy najnowszej [Azure CLI 2.0](/cli/azure/install-az-cli2) zainstalowane i zalogowany do konta platformy Azure przy użyciu [logowania az](/cli/azure/#login).

W poniższych przykładach Zastąp przykładowe nazwy parametrów własne wartości. Przykład nazwy parametru zawierają *myResourceGroup*, *myNetworkSecurityGroup*, i *myVnet*.

Utwórz grupę zabezpieczeń sieci z [utworzyć nsg sieci az](/cli/azure/network/nsg#create). Poniższy przykład tworzy sieciową grupę zabezpieczeń o nazwie *myNetworkSecurityGroup* w *eastus* lokalizacji:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Dodaj regułę z [Tworzenie reguły nsg sieci az](/cli/azure/network/nsg/rule#create) umożliwić ruchu HTTP na serwerze sieci Web (lub dostosować własne scenariusz, takich jak SSH łączności dostępu lub bazy danych). Poniższy przykład tworzy reguły o nazwie *myNetworkSecurityGroupRule* zezwalająca na ruch TCP na porcie 80:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```

Skojarzenia sieciowej grupy zabezpieczeń z maszyny Wirtualnej interfejsu sieciowego (NIC) z [aktualizacji kart sieciowych az](/cli/azure/network/nic#update). Poniższy przykład powoduje skojarzenie istniejącej karty NIC o nazwie *myNic* z sieciową grupą zabezpieczeń o nazwie *myNetworkSecurityGroup*:

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

Alternatywnie można skojarzyć swojej grupy zabezpieczeń sieci z podsiecią sieci wirtualnej z [zaktualizować podsieci sieci wirtualnej sieci az](/cli/azure/network/vnet/subnet#update) , a nie tylko do interfejsu sieciowego na jednej maszynie Wirtualnej. Poniższy przykład powoduje skojarzenie istniejącą podsieć o nazwie *mySubnet* w *myVnet* sieci wirtualnej z sieciową grupą zabezpieczeń o nazwie *myNetworkSecurityGroup*:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a>Więcej informacji na temat grup zabezpieczeń sieci
Szybkie polecenia tutaj pozwala rozpocząć pracę z ruchem przepływać do maszyny Wirtualnej. Grupy zabezpieczeń sieci zapewniają wiele funkcje i poziom szczegółowości kontrolowania dostępu do zasobów. Możesz przeczytać dodatkowe informacje [Tworzenie grupy zabezpieczeń sieci i listy ACL zasady tutaj](tutorial-virtual-network.md#secure-network-traffic).

Dla aplikacji sieci web wysokiej dostępności należy umieszczać maszyny wirtualne za usługą równoważenia obciążenia Azure. Moduł równoważenia obciążenia dystrybuuje ruch do maszyn wirtualnych z sieciową grupą zabezpieczeń, który umożliwia filtrowanie ruchu. Aby uzyskać więcej informacji, zobacz [jak załadować saldo maszyn wirtualnych systemu Linux na platformie Azure, aby utworzyć aplikację wysokiej dostępności](tutorial-load-balancer.md).

## <a name="next-steps"></a>Następne kroki
W tym przykładzie utworzono prosta Reguła zezwalająca na ruch HTTP. Można znaleźć informacje dotyczące tworzenia środowisk bardziej szczegółowe zawierają następujące artykuły:

* [Omówienie usługi Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Co to jest sieciowa grupa zabezpieczeń?](../../virtual-network/virtual-networks-nsg.md)
