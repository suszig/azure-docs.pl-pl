---
title: "Tworzenie wewnętrznego modułu równoważenia obciążenia — interfejs wiersza polecenia platformy Azure | Microsoft Docs"
description: "Dowiedz się, jak utworzyć wewnętrzny moduł równoważenia obciążenia przy użyciu interfejsu wiersza polecenia platformy Azure w usłudze Resource Manager"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-resource-manager
ms.assetid: c7a24e92-b4da-43c0-90f2-841c1b7ce489
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 920ddecbf81296fd83606f2908e432f5327d4b7e
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Tworzenie wewnętrznego modułu równoważenia obciążenia przy użyciu interfejsu wiersza polecenia platformy Azure

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [Program PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Interfejs wiersza polecenia platformy Azure](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Szablon](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>Wdrażanie rozwiązania przy użyciu interfejsu wiersza polecenia platformy Azure

Poniższe kroki przedstawiają sposób tworzenia dostępnego z Internetu modułu równoważenia obciążenia przy użyciu wiersza interfejsu polecenia w usłudze Azure Resource Manager. Usługa Azure Resource Manager pozwala tworzyć i konfigurować każdy zasób osobno, a następnie łączyć je ze sobą w celu utworzenia kolejnego zasobu.

Aby wdrożyć moduł równoważenia obciążenia, należy utworzyć i skonfigurować następujące obiekty:

* **Konfiguracja adresu IP frontonu** — publiczne adresy IP dla przychodzącego ruchu sieciowego.
* **Pula adresów zaplecza** — interfejsy sieciowe (karty sieciowe) maszyn wirtualnych odbierających ruch sieciowy z modułu równoważenia obciążenia.
* **Reguły równoważenia obciążenia** — reguły mapowania portu publicznego modułu równoważenia obciążenia na port w puli adresów zaplecza.
* **Reguły NAT ruchu przychodzącego** — reguły mapowania portu publicznego modułu równoważenia obciążenia na port określonej maszyny wirtualnej w puli adresów zaplecza.
* **Sondy** — sondy kondycji używane do sprawdzania dostępności wystąpień maszyn wirtualnych w puli adresów zaplecza.

Aby uzyskać więcej informacji, zobacz artykuł [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Obsługa usługi Azure Resource Manager dla modułu równoważenia obciążenia).

## <a name="set-up-cli-to-use-resource-manager"></a>Konfigurowanie interfejsu wiersza polecenia w celu użycia usługi Resource Manager

1. Jeśli po raz pierwszy korzystasz z interfejsu wiersza polecenia platformy Azure, zobacz artykuł [Install and configure the Azure CLI](../cli-install-nodejs.md) (Instalowanie i konfigurowanie interfejsu wiersza polecenia platformy Azure). Postępuj zgodnie z instrukcjami do momentu wybierania konta i subskrypcji platformy Azure.
2. Uruchom polecenie **azure config mode**, aby włączyć tryb usługi Resource Manager, w następujący sposób:

    ```azurecli
    azure config mode arm
    ```

    Oczekiwane dane wyjściowe:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Tworzenie wewnętrznego modułu równoważenia obciążenia krok po kroku

1. Zaloguj się do platformy Azure.

    ```azurecli
    azure login
    ```

    Po wyświetleniu monitu wprowadź swoje poświadczenia platformy Azure.

2. Przełącz narzędzia polecenia na tryb usługi Azure Resource Manager.

    ```azurecli
    azure config mode arm
    ```

## <a name="create-a-resource-group"></a>Utwórz grupę zasobów

Wszystkie zasoby usługi Azure Resource Manager są kojarzone z grupą zasobów. Jeśli jeszcze nie wykonano tej czynności, utwórz grupę zasobów.

```azurecli
azure group create <resource group name> <location>
```

## <a name="create-an-internal-load-balancer-set"></a>Tworzenie zestawu wewnętrznego modułu równoważenia obciążenia

1. Utwórz wewnętrzny moduł równoważenia obciążenia.

    W poniższym scenariuszu tworzona jest grupa zasobów o nazwie nrprg w regionie Wschodnie stany USA.

    ```azurecli
    azure network lb create --name nrprg --location eastus
    ```

   > [!NOTE]
   > Wszystkie zasoby wewnętrznych modułów równoważenia obciążenia, takie jak sieci wirtualne i podsieci sieci wirtualnych, muszą należeć do tej samej grupy zasobów i znajdować się w tym samym regionie.

2. Utwórz adres IP frontonu dla wewnętrznego modułu równoważenia obciążenia.

    Wybrany adres IP musi należeć do zakresu podsieci sieci wirtualnej.

    ```azurecli
    azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet
    ```

3. Utwórz pulę adresów zaplecza.

    ```azurecli
    azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb
    ```

    Po zdefiniowaniu adresu IP frontonu i puli adresów zaplecza możesz utworzyć reguły modułu równoważenia obciążenia, reguły NAT dla ruchu przychodzącego i dostosowane sondy kondycji.

4. Utwórz regułę modułu równoważenia obciążenia dla wewnętrznego modułu równoważenia obciążenia.

    Po wykonaniu poprzednich kroków polecenie spowoduje utworzenie reguły modułu równoważenia obciążenia do nasłuchiwania w porcie 1433 w puli frontonu i wysyłania ruchu sieciowego ze zrównoważonym obciążeniem do puli adresów zaplecza, również przy użyciu portu 1433.

    ```azurecli
    azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb
    ```

5. Utwórz reguły NAT dla ruchu przychodzącego.

    Reguły NAT dla ruchu przychodzącego są używane do tworzenia punktów końcowych modułu równoważenia obciążenia, które umożliwiają przechodzenie do określonego wystąpienia maszyny wirtualnej. W ramach poprzednich kroków utworzono dwie reguły NAT dla pulpitu zdalnego.

    ```azurecli
    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389
    ```

6. Utwórz sondy kondycji modułu równoważenia obciążenia.

    Sonda kondycji sprawdza wszystkie wystąpienia maszyny wirtualnej, aby upewnić się, że mogą wysyłać ruch sieciowy. Wystąpienie maszyny wirtualnej, w przypadku którego sprawdzanie kondycji za pomocą sondy nie powiodło się, jest usuwane z modułu równoważenia obciążenia do momentu ponownego przejścia do trybu online i pomyślnego sprawdzenia kondycji.

    ```azurecli
    azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4
    ```

    > [!NOTE]
    > Platforma Microsoft Azure używa statycznego, podlegającego publicznemu routingowi adresu IPv4 do różnych scenariuszy administracyjnych. Adres IP to 168.63.129.16. Ten adres IP nie powinien być blokowany przez zapory, ponieważ może to spowodować nieoczekiwane zachowanie.
    > Jeśli chodzi o wewnętrzne równoważenie obciążenia platformy Azure, ten adres IP jest używany przez sondy monitorujące wysyłane z modułu równoważenia obciążenia w celu określenia kondycji maszyn wirtualnych w zestawie o zrównoważonym obciążeniu. Jeśli sieciowa grupa zabezpieczeń jest używana do ograniczania ruchu do maszyn wirtualnych platformy Azure w zestawie o wewnętrznie zrównoważonym obciążeniu lub jest stosowana do podsieci sieci wirtualnej, upewnij się, że dodano regułę zabezpieczeń sieci zezwalającą na ruch z adresu 168.63.129.16.

## <a name="create-nics"></a>Tworzenie kart sieciowych

Musisz utworzyć karty sieciowe (lub zmodyfikować istniejące) i skojarzyć je z regułami NAT, regułami modułu równoważenia obciążenia i sondami.

1. Utwórz kartę sieciową o nazwie *lb-nic1-be* i skojarz ją z regułą NAT *rdp1* oraz pulą adresów zaplecza *beilb*.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus
    ```

    Oczekiwane dane wyjściowe:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Utwórz kartę sieciową o nazwie *lb-nic2-be* i skojarz ją z regułą NAT *rdp2* oraz pulą adresów zaplecza *beilb*.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus
    ```

3. Utwórz maszynę wirtualną o nazwie *DB1* i skojarz ją z kartą sieciową o nazwie *lb-nic1-be*. Konto magazynu o nazwie *web1nrp* zostało utworzone przed uruchomieniem następującego polecenia:

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```
    > [!IMPORTANT]
    > Maszyny wirtualne w module równoważenia obciążenia muszą być w tym samym zestawie dostępności. Użyj polecenia `azure availset create`, aby utworzyć zestaw dostępności.

4. Utwórz maszynę wirtualną o nazwie *DB2* i skojarz ją z kartą sieciową o nazwie *lb-nic2-be*. Konto magazynu o nazwie *web1nrp* zostało utworzone przed uruchomieniem następującego polecenia:

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="delete-a-load-balancer"></a>Usuwanie modułu równoważenia obciążenia

Aby usunąć moduł równoważenia obciążenia, użyj następującego polecenia:

```azurecli
azure network lb delete --resource-group nrprg --name ilbset
```

## <a name="next-steps"></a>Następne kroki

[Configure a load balancer distribution mode by using source IP affinity](load-balancer-distribution-mode.md) (Konfigurowanie trybu dystrybucji modułu równoważenia obciążenia przy użyciu koligacji źródłowych adresów IP)

[Configure idle TCP timeout settings for your load balancer](load-balancer-tcp-idle-timeout.md) (Konfigurowanie ustawień limitu czasu bezczynności protokołu TCP dla modułu równoważenia obciążenia)

