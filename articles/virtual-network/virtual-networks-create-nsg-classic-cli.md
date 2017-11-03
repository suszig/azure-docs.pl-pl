---
title: "Sposób tworzenia grup NSG w klasycznym trybie przy użyciu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć i wdrożyć grup NSG w klasycznym trybie przy użyciu wiersza polecenia platformy Azure"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 17d98950-5fbb-4653-bef6-d822ab37541e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 115a1937a4c88ba2b986a40c84b1b759ed5e03b5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a>Sposób tworzenia grup NSG (klasyczne) w wiersza polecenia platformy Azure
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

W tym artykule opisano klasyczny model wdrażania. Możesz również [tworzenia grup NSG w modelu wdrażania usługi Resource Manager](virtual-networks-create-nsg-arm-cli.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Poniższe przykładowe polecenia interfejsu wiersza polecenia Azure oczekiwać środowisku niezłożonym już utworzone w zależności od scenariusza powyżej. Do uruchomienia poleceń wyświetlaną w tym dokumencie, najpierw utworzyć środowisko testowe przez [tworzenia sieci wirtualnej](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Jak utworzyć grupę NSG dla podsieci frontonu
Aby utworzyć grupy NSG o nazwie o nazwie **frontonu NSG** oparte na powyższym scenariuszu, wykonaj poniższe kroki.

1. Jeśli po raz pierwszy używasz interfejsu wiersza polecenia Azure, zobacz artykuł [Instalowanie i konfigurowania interfejsu wiersza polecenia Azure](../cli-install-nodejs.md) i postępuj zgodnie z instrukcjami aż do punktu, w którym należy wybrać konto platformy Azure i subskrypcję.
2. Uruchom  **`azure config mode`**  polecenie, aby przełączyć do trybu klasycznego, jak pokazano poniżej.
   
        azure config mode asm
   
    Oczekiwane dane wyjściowe:
   
        info:    New mode is asm
3. Uruchom  **`azure network nsg create`**  polecenie, aby utworzyć grupy NSG.
   
        azure network nsg create -l uswest -n NSG-FrontEnd
   
    Oczekiwane dane wyjściowe:
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    Parametry:
   
   * **-l (lub --location)**. Region platformy Azure, w którym zostanie utworzona nowa grupa NSG. W naszym scenariuszu *westus*.
   * **-n (lub --name)**. Nazwa nowej grupy NSG. W naszym scenariuszu *frontonu NSG*.
4. Uruchom  **`azure network nsg rule create`**  polecenie, aby utworzyć regułę, która zezwala na dostęp do portu 3389 (RDP) z Internetu.
   
        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    Oczekiwane dane wyjściowe:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
   
    Parametry:
   
   * **-nazwę (lub--nsg —)**. Nazwa grupy NSG, w którym zostaną utworzone reguły. W naszym scenariuszu *frontonu NSG*.
   * **-n (lub --name)**. Nazwa nowej reguły. W naszym scenariuszu *reguły protokołu rdp*.
   * **-c (lub--akcji)**. Poziom dostępu dla reguły (Deny lub Zezwalaj).
   * **-p (lub--protokół)**. Protocol (Tcp, Udp lub *) dla reguły.
   * **-r (lub--typ)**. Kierunek połączenia (przychodzący lub wychodzący).
   * **-y (lub--priorytetu)**. Priorytet reguły.
   * **-f (lub--prefiks adresu źródłowego)**. Prefiks adresu źródłowego CIDR lub przy użyciu znaczników domyślnych.
   * **-o (lub--zakres portów źródłowych)**. Port źródłowy, lub zakresem portów.
   * **-e (lub--prefiks adresu docelowego)**. Prefiks adresu docelowego w CIDR lub przy użyciu znaczników domyślnych.
   * **-u (lub--zakres portów docelowych)**. Port docelowy, lub zakresem portów.
5. Uruchom  **`azure network nsg rule create`**  polecenie, aby utworzyć regułę, która zezwala na dostęp do portu 80 (HTTP) z Internetu.
   
        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    Oczekiwano putput:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK
6. Uruchom  **`azure network nsg subnet add`**  poleceń połączyć grupy NSG do podsieci frontonu.
   
        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   
    Oczekiwane dane wyjściowe:
   
        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>Jak utworzyć grupę NSG podsieci wewnętrznej
Aby utworzyć grupy NSG o nazwie o nazwie *zaplecza NSG* oparte na powyższym scenariuszu, wykonaj poniższe kroki.

1. Uruchom  **`azure network nsg create`**  polecenie, aby utworzyć grupy NSG.
   
        azure network nsg create -l uswest -n NSG-BackEnd
   
    Oczekiwane dane wyjściowe:
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    Parametry:
   
   * **-l (lub --location)**. Region platformy Azure, w którym zostanie utworzona nowa grupa NSG. W naszym scenariuszu *westus*.
   * **-n (lub --name)**. Nazwa nowej grupy NSG. W naszym scenariuszu *frontonu NSG*.
2. Uruchom  **`azure network nsg rule create`**  polecenie, aby utworzyć regułę, która umożliwia dostęp do portu 1433 (SQL) z podsieci frontonu.
   
        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    Oczekiwane dane wyjściowe:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
3. Uruchom  **`azure network nsg rule create`**  polecenie, aby utworzyć regułę, która nie zezwala na dostęp do Internetu.
   
        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   
    Oczekiwano putput:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK
4. Uruchom  **`azure network nsg subnet add`**  poleceń połączyć grupy NSG podsieci wewnętrznej.
   
        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
   
    Oczekiwane dane wyjściowe:
   
        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK

