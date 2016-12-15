---
title: "Tworzenie modułu równoważenia obciążenia mającego połączenie z Internetem w usłudze Resource Manager za pomocą interfejsu wiersza polecenia platformy Azure | Microsoft Docs"
description: "Dowiedz się, jak utworzyć dostępny z Internetu moduł równoważenia obciążenia w usłudze Resource Manager za pomocą interfejsu wiersza polecenia Azure"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a8bcdd88-f94c-4537-8143-c710eaa86818
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: f3e06e802e8a5b98afc96747dee0db4173da40f0
ms.openlocfilehash: 9129a1941b138e6f3e9b2d0359015a20fd819565

---
# <a name="creating-an-internet-load-balancer-using-the-azure-cli"></a>Tworzenie internetowego modułu równoważenia obciążenia za pomocą interfejsu wiersza polecenia platformy Azure

> [!div class="op_single_selector"]
> * [Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [Program PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Interfejs wiersza polecenia platformy Azure](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Szablon](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

W tym artykule opisano model wdrażania usługi Resource Manager. Możesz też zapoznać się z artykułem na temat [tworzenia modułu równoważenia obciążenia dostępnego z Internetu przy użyciu wdrażania klasycznego](load-balancer-get-started-internet-classic-portal.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>Wdrażanie rozwiązania za pomocą interfejsu wiersza polecenia Azure

W poniższych krokach opisano, jak utworzyć dostępny z Internetu moduł równoważenia obciążenia przy użyciu usługi Azure Resource Manager z interfejsem wiersza polecenia. Usługa Azure Resource Manager pozwala tworzyć i konfigurować każdy zasób osobno, a następnie łączyć je ze sobą w jeden zasób.

Aby wdrożyć moduł równoważenia obciążenia, należy utworzyć i skonfigurować poniższe obiekty:

* Konfiguracja IP frontonu — publiczne adresy IP dla przychodzącego ruchu sieciowego.
* Pula adresów zaplecza — interfejsy sieciowe (NIC) maszyn wirtualnych odbierających ruch sieciowy z modułu równoważenia obciążenia.
* Reguły równoważenia obciążenia — reguły mapowania portu publicznego modułu równoważenia obciążenia na port w puli adresów zaplecza.
* Reguły NAT ruchu przychodzącego — reguły mapowania portu publicznego modułu równoważenia obciążenia na port określonej maszyny wirtualnej w puli adresów zaplecza.
* Sondy — sondy kondycji używane do sprawdzania dostępności wystąpień maszyn wirtualnych w puli adresów zaplecza.

Aby uzyskać więcej informacji, zobacz artykuł [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Obsługa usługi Azure Resource Manager dla modułu równoważenia obciążenia).

## <a name="set-up-cli-to-use-resource-manager"></a>Konfigurowanie interfejsu wiersza polecenia w celu użycia usługi Resource Manager

1. Jeśli po raz pierwszy używasz interfejsu wiersza polecenia Azure, zobacz artykuł [Instalowanie i konfigurowania interfejsu wiersza polecenia Azure](../xplat-cli-install.md) i postępuj zgodnie z instrukcjami aż do punktu, w którym należy wybrać konto platformy Azure i subskrypcję.
2. Uruchom polecenie **azure config mode**, aby włączyć tryb Resource Manager, jak pokazano poniżej.

    ```azurecli
        azure config mode arm
    ```

    Oczekiwane dane wyjściowe:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Tworzenie sieci wirtualnej oraz publicznego adresu IP dla puli adresów IP frontonu

1. Utwórz sieć wirtualną (VNet) o nazwie *NRPVnet* w lokalizacji Wschodnie stany USA za pomocą grupy zasobów o nazwie *NRPRG*.

    ```azurecli
        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16
    ```

    Utwórz podsieć o nazwie *NRPVnetSubnet* z blokiem CIDR 10.0.0.0/24 w sieci *NRPVnet*.

    ```azurecli
        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24
    ```

2. Utwórz publiczny adres IP o nazwie *NRPPublicIP*, który będzie używany przez pulę adresów IP frontonu z serwerem DNS o nazwie *loadbalancernrp.eastus.cloudapp.azure.com*. Poniższe polecenie wykorzystuje alokację typu statycznego i limit czasu bezczynności trwający 4 min.

    ```azurecli
        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4
    ```

   > [!IMPORTANT]
   > Moduł równoważenia obciążenia będzie używać etykiety domeny publicznego adresu IP jako nazwy FQDN. Różni się to od wdrożenia klasycznego, które wykorzystuje usługę w chmurze jako nazwę FQDN (Fully Qualified Domain Name) modułu równoważenia obciążenia.
   > W tym przykładzie FQDN to *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Tworzenie modułu równoważenia obciążenia

Następujące polecenie spowoduje utworzenie modułu równoważenia obciążenia o nazwie *NRPlb* w grupie zasobów *NRPRG* w lokalizacji Azure *Wschodnie stany USA*.

    ```azurecli
    azure network lb create NRPRG NRPlb eastus
    ```

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Tworzenie puli adresów IP frontonu i puli adresów zaplecza
W tym przykładzie opisano sposób utworzenia puli adresów IP frontonu, która odbiera ruch sieciowy przychodzący do modułu równoważenia obciążenia, oraz puli adresów zaplecza, do której pula adresów IP frontonu przesyła ruch sieciowy o zrównoważonym obciążeniu.

1. Utwórz pulę adresów IP frontonu skojarzoną z publicznym adresem IP utworzonym w poprzednim kroku oraz moduł równoważenia obciążenia.

    ```azurecli
        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip
    ```

2. Skonfiguruj pulę adresów zaplecza używaną do odbierania ruchu przychodzącego z puli adresów IP frontonu.

    ```azurecli
        azure network lb address-pool create NRPRG NRPlb NRPbackendpool
    ```

## <a name="create-lb-rules-nat-rules-and-probe"></a>Tworzenie reguł równoważenia obciążenia, reguł NAT oraz sond

W tym przykładzie opisano tworzenie następujących elementów:

* Reguła NAT do translacji całego ruchu przychodzącego do portu 21 na port 22<sup>1</sup>.
* Reguła NAT do translacji całego ruchu przychodzącego do portu 23 na port 22.
* Reguła modułu równoważenia obciążenia do równoważenia całego ruchu przychodzącego do portu 80 na port 80 adresów w puli zaplecza.
* Reguła sondy do sprawdzania kondycji na stronie o nazwie *HealthProbe.aspx*.

<sup>1</sup> Reguły NAT są powiązane z konkretnym wystąpieniem maszyny wirtualnej za modułem równoważenia obciążenia. Ruch sieciowy przychodzący do portu 21 jest wysyłany do danej maszyny wirtualnej na port 22 powiązany z daną regułą NAT. Musisz określić protokół (UDP lub TCP) dla reguły NAT. Nie można przypisać obu protokołów do tego samego portu.

1. Utwórz reguły NAT.

    ```azurecli
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22
    ```

2. Utwórz regułę modułu równoważenia obciążenia.

    ```azurecli
        azure network lb rule create --resource-group nrprg nrplb --lb-name lbrule --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name NRPfrontendpool --backend-address-pool-name NRPbackendpool
    ```

3. Utwórz sondę kondycji.

    ```azurecli
        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4
    ```

4. Sprawdź ustawienia.

    ```azurecli
        azure network lb show nrprg nrplb
    ```

    Oczekiwane dane wyjściowe:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>Tworzenie kart sieciowych

Musisz utworzyć karty sieciowe (lub zmodyfikować istniejące) i skojarzyć je z regułami NAT, regułami modułu równoważenia obciążenia i sondami.

1. Utwórz kartę sieciową o nazwie *lb-nic1-be* i powiąż ją z regułą NAT *rdp1* oraz pulą adresów zaplecza *NRPbackendpool*.

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus
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

2. Utwórz kartę sieciową o nazwie *lb-nic2-be* i powiąż ją z regułą NAT *rdp2* oraz pulą adresów zaplecza *NRPbackendpool*.

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus
    ```

3. Utwórz maszynę wirtualną (VM) o nazwie *web1* i powiąż ją z kartą sieciową o nazwie *lb-nic1-be*. Konto magazynu o nazwie *web1nrp* zostało utworzone przed uruchomieniem poniższego polecenia.

    ```azurecli
        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

    > [!IMPORTANT]
    > Maszyny wirtualne w module równoważenia obciążenia muszą być w tym samym zestawie dostępności. Użyj polecenia `azure availset create`, aby utworzyć zestaw dostępności.

    Dane wyjściowe będą podobne do następujących:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    > [!NOTE]
    > Może zostać wyświetlony komunikat **This is a NIC without publicIP configured** (Jest to karta sieciowa bez ustawionego publicznego adresu IP), ponieważ utworzona karta sieciowa modułu równoważenia obciążenia łączy się z Internetem za pośrednictwem publicznego adresu IP tego modułu.

    Ponieważ karta sieciowa *lb-nic1-be* jest powiązana z regułą NAT *rdp1*, możesz połączyć się z maszyną wirtualną *web1* za pomocą protokołu RDP przez port 3441 modułu równoważenia obciążenia.

4. Utwórz maszynę wirtualną (VM) o nazwie *web2* i powiąż ją z kartą sieciową o nazwie *lb-nic2-be*. Konto magazynu o nazwie *web1nrp* zostało utworzone przed uruchomieniem poniższego polecenia.

    ```azurecli
        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="update-an-existing-load-balancer"></a>Aktualizowanie istniejącego modułu równoważenia obciążenia
Możesz dodać reguły odwołujące się do istniejącego modułu równoważenia obciążenia. W następnym przykładzie opisano dodawanie nowej reguły równoważenia obciążenia do istniejącego modułu równoważenia obciążenia **NRPlb**

```azurecli
azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool
```

## <a name="delete-a-load-balancer"></a>Usuwanie modułu równoważenia obciążenia
Użyj poniższego polecenia, aby usunąć moduł równoważenia obciążenia:

```azurecli
azure network lb delete --resource-group nrprg --name nrplb
```

## <a name="next-steps"></a>Następne kroki
[Get started configuring an internal load balancer](load-balancer-get-started-ilb-arm-cli.md) (Wprowadzenie do konfigurowania wewnętrznego modułu równoważenia obciążenia)

[Configure a load balancer distribution mode](load-balancer-distribution-mode.md) (Konfigurowanie trybu dystrybucji modułu równoważenia obciążenia)

[Configure idle TCP timeout settings for your load balancer](load-balancer-tcp-idle-timeout.md) (Konfigurowanie ustawień limitu czasu bezczynności protokołu TCP dla modułu równoważenia obciążenia)



<!--HONumber=Dec16_HO1-->


