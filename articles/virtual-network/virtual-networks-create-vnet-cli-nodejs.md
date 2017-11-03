---
title: "Utwórz sieć wirtualną przy użyciu 1.0 interfejsu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć sieć wirtualną przy użyciu 1.0 interfejsu wiersza polecenia platformy Azure | Menedżer zasobów."
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.openlocfilehash: f0649c5c8c04dda72d2f147601efb37217f9bade
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Utwórz sieć wirtualną przy użyciu wiersza polecenia platformy Azure

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Platforma Azure ma dwa modele wdrażania: usługa Azure Resource Manager i wersja klasyczna. Firma Microsoft zaleca tworzenie zasobów za pomocą modelu wdrożenia usługi Resource Manager. Aby dowiedzieć się więcej o różnicach między dwoma modelami, zapoznaj się z artykułem [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) (Informacje na temat modeli wdrażania platformy Azure).

## <a name="cli-versions-to-complete-the-task"></a>Wersje interfejsu wiersza polecenia umożliwiające wykonanie zadania
Zadanie można wykonać przy użyciu jednej z następujących wersji interfejsu wiersza polecenia:

- [Interfejs wiersza polecenia platformy Azure w wersji 2.0](virtual-networks-create-vnet-arm-cli.md) — nasz interfejs wiersza polecenia nowej generacji dla modelu wdrażania na potrzeby zarządzania zasobami
- [Azure CLI 1.0](#create-a-virtual-network) — nasze interfejsu wiersza polecenia dla klasycznego i zasobów zarządzania wdrażania modeli (w tym artykule)

 
[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Tworzenie sieci wirtualnej

Aby utworzyć sieć wirtualną przy użyciu wiersza polecenia platformy Azure, wykonaj następujące kroki:

1. Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure, wykonując kroki opisane w [Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure](../cli-install-nodejs.md) artykułu.

2. Utwórz sieć wirtualną i podsieć:

    ```azurecli
    azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    ```

    Oczekiwane dane wyjściowe:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    Użyte parametry:

   * **--vnet**. Nazwa sieci wirtualnej, która zostanie utworzona. W naszym scenariuszu jest to *TestVNet*
   * **-e (lub--przestrzeni adresowej)**. Przestrzeń adresowa sieci wirtualnej. W naszym scenariuszu *192.168.0.0*
   * **-i (lub - cidr)**. Maska sieci w formacie CIDR. W naszym scenariuszu *16*.
   * **-n (lub--nazwy podsieci**). Nazwa pierwszej podsieci. W naszym scenariuszu jest to *FrontEnd*.
   * **-p (lub--podsieci start-ip)**. Początkowy adres IP dla podsieci lub przestrzeni adresowej podsieci. W naszym scenariuszu *192.168.1.0*.
   * **-r (lub--podsieci cidr)**. Maska sieci w formacie CIDR podsieci. W naszym scenariuszu *24*.
   * **-l (lub --location)**. Region platformy Azure, w którym utworzona sieć wirtualna. W naszym scenariuszu *środkowe stany USA*.

3. Utwórz podsieć:

    ```azurecli
    azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    ```
   
    Oczekiwane dane wyjściowe:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    Użyte parametry:

   * **-t (lub--vnet-name**. Nazwa sieci wirtualnej, w której zostanie utworzona podsieć. W naszym scenariuszu jest to *TestVNet*.
   * **-n (lub --name)**. Nazwa nowej podsieci. W naszym scenariuszu *zaplecza*.
   * **-a (lub --address-prefix)**. Blok CIDR podsieci. Cztery naszym scenariuszu *192.168.2.0/24*.
   
4. Aby wyświetlić właściwości nowej sieci wirtualnej:

    ```azurecli
    azure network vnet show
    ```
   
    Oczekiwane dane wyjściowe:
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak połączyć:

- Maszyna wirtualna (VM) do sieci wirtualnej, odczytując [Utwórz Maszynę wirtualną systemu Linux](../virtual-machines/linux/quick-create-cli.md) artykułu. Zamiast tworzyć sieć wirtualną i podsieć, wykonując kroki opisane w artykułach, można wybrać istniejącą sieć wirtualną i podsieć, z którymi zostanie połączona maszyna wirtualna.
- Sieć wirtualną z innymi sieciami wirtualnymi. Odpowiednie informacje możesz znaleźć w artykule [Łączenie sieci wirtualnych](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
- Sieć wirtualną z siecią lokalną za pomocą prywatnej sieci wirtualnej (VPN) typu lokacja-lokacja lub obwodu usługi ExpressRoute. Dowiedz się, jak odczytując [połączyć sieć wirtualną z siecią lokalną przy użyciu sieci VPN lokacja lokacja](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) i [połączyć sieć wirtualną z obwodem usługi ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).