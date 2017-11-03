---
title: "Kontrolowanie routingu i wirtualnych urządzeń przy użyciu 1.0 interfejsu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie kontrolowania routingu i wirtualnych urządzeń przy użyciu 1.0 interfejsu wiersza polecenia platformy Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2017
ms.author: jdial
ms.openlocfilehash: 5f21bc7a4fcd9507ea9d6b2b752a2328a7b834f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-user-defined-routes-udr-using-the-azure-cli-10"></a>Tworzenie trasy zdefiniowane przez użytkownika (przez) przy użyciu 1.0 interfejsu wiersza polecenia platformy Azure

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Interfejs wiersza polecenia platformy Azure](virtual-network-create-udr-arm-cli.md)
> * [Szablon](virtual-network-create-udr-arm-template.md)
> * [PowerShell (klasyczny)](virtual-network-create-udr-classic-ps.md)
> * [Interfejs wiersza polecenia (klasyczny)](virtual-network-create-udr-classic-cli.md)

Utwórz niestandardowy routing i wirtualnych urządzeń przy użyciu wiersza polecenia platformy Azure.

## <a name="cli-versions-to-complete-the-task"></a>Wersje interfejsu wiersza polecenia umożliwiające wykonanie zadania 

Zadanie można wykonać przy użyciu jednej z następujących wersji interfejsu wiersza polecenia: 

- [Azure CLI 1.0](#Create-the-UDR-for-the-front-end-subnet) — nasze interfejsu wiersza polecenia dla klasycznego i zasobów zarządzania wdrażania modeli (w tym artykule)
- [Interfejs wiersza polecenia platformy Azure w wersji 2.0](virtual-network-create-udr-arm-cli.md) — nasz interfejs wiersza polecenia nowej generacji dla modelu wdrażania na potrzeby zarządzania zasobami 


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Poniższe przykładowe polecenia interfejsu wiersza polecenia Azure oczekiwać środowisku niezłożonym już utworzone w zależności od scenariusza powyżej. Jeśli chcesz uruchomić polecenia wyświetlaną w tym dokumencie, wdrażając najpierw utworzyć środowisko testowe [ten szablon](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), kliknij przycisk **wdrażanie na platformie Azure**, Zastąp domyślne wartości parametrów, jeśli to konieczne i postępuj zgodnie z instrukcjami w portalu.


## <a name="create-the-udr-for-the-front-end-subnet"></a>Utwórz przez podsieci frontonu
Aby utworzyć tabelę tras i trasy wymagane do podsieci frontonu oparte na powyższym scenariuszu, wykonaj poniższe kroki.

1. Uruchom następujące polecenie, aby utworzyć tabelę tras dla podsieci frontonu:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest
    ```
   
    Dane wyjściowe:
   
        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK
   
    Parametry:
   
   * **-g (lub --resource-group)**. Nazwa grupy zasobów, w której zostanie utworzona przez. W naszym scenariuszu jest to *TestRG*.
   * **-l (lub --location)**. Region platformy Azure, w której zostanie utworzona przez nowe. W naszym scenariuszu *westus*.
   * **-n (lub --name)**. Nazwa nowej przez. W naszym scenariuszu *frontonu przez*.
2. Uruchom następujące polecenie, aby utworzyć trasę w tabeli tras do wysyłania całego ruchu kierowanego do podsieci zaplecza (192.168.2.0/24), aby **FW1** maszyny Wirtualnej (192.168.0.4):

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4
    ```
   
    Dane wyjściowe:
   
        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK
   
    Parametry:
   
   * **-r (lub--nazwa tabeli tras)**. Nazwa tabeli tras, w którym można dodać trasy. W naszym scenariuszu *frontonu przez*.
   * **-a (lub --address-prefix)**. Prefiks adresu podsieci, gdy pakiety są przeznaczone do. W naszym scenariuszu *192.168.2.0/24*.
   * **-y (lub--następnego przeskoku typu)**. Typ obiektu ruchu zostaną wysłane do. Możliwe wartości to *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, lub *Brak*.
   * **-p (lub--dalej przeskoku — adres ip**). Adres IP następnego przeskoku. W naszym scenariuszu *192.168.0.4*.
3. Uruchom następujące polecenie, aby skojarzyć utworzone powyżej z tabeli tras **frontonu** podsieci:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    Dane wyjściowe:
   
        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK
   
    Parametry:
   
   * **-e (lub--vnet-name)**. Nazwa sieci wirtualnej, w którym znajduje się podsieci. W naszym scenariuszu jest to *TestVNet*.

## <a name="create-the-udr-for-the-back-end-subnet"></a>Utwórz przez podsieci wewnętrznej
Aby utworzyć tabelę tras i trasy wymagane dla podsieci zaplecza opartą na tym scenariuszu powyżej, wykonaj następujące kroki:

1. Uruchom następujące polecenie, aby utworzyć tabelę tras dla podsieci wewnętrznej:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-BackEnd -l westus
    ```

2. Uruchom następujące polecenie, aby utworzyć trasę w tabeli tras do wysyłania całego ruchu kierowanego do podsieci frontonu (192.168.1.0/24), aby **FW1** maszyny Wirtualnej (192.168.0.4):

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4
    ```

3. Uruchom następujące polecenie, aby skojarzyć tabela tras o **zaplecza** podsieci:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a>Włącz przesyłanie dalej IP na FW1
Aby włączyć przesyłanie dalej IP w używany przez kartę Sieciową **FW1**, wykonaj następujące czynności:

1. Uruchom polecenie i zwróć uwagę, wartość **przesyłanie dalej IP włączyć**. Należy wybrać opcję *false*.

    ```azurecli
    azure network nic show -g TestRG -n NICFW1
    ```

    Dane wyjściowe:
   
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK
2. Uruchom następujące polecenie, aby włączyć przesyłanie dalej IP:

    ```azurecli
    azure network nic set -g TestRG -n NICFW1 -f true
    ```
   
    Dane wyjściowe:
   
        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK
   
    Parametry:
   
   * **-f (lub--Włącz przesyłanie dalej ip)**. *wartość true,* lub *false*.

