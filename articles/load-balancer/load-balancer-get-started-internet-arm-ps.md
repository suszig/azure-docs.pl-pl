---
title: "Tworzenie modułu równoważenia obciążenia dostępnego z Internetu w usłudze Resource Manager za pomocą programu PowerShell | Microsoft Docs"
description: "Dowiedz się, jak utworzyć dostępny z Internetu moduł równoważenia obciążenia w usłudze Resource Manager za pomocą programu PowerShell"
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 8257f548-7019-417f-b15f-d004a1eec826
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 7b74067b2f174e1242f5eb6c3028af4ef7b2f22e


---
# <a name="a-namegetstartedacreating-an-internetfacing-load-balancer-in-resource-manager-by-using-powershell"></a><a name="get-started"></a>Tworzenie dostępnego z Internetu modułu równoważenia obciążenia w usłudze Resource Manager za pomocą programu PowerShell
[!INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

W tym artykule opisano model wdrażania usługi Resource Manager. Możesz też zapoznać się z artykułem na temat [tworzenia modułu równoważenia obciążenia dostępnego z Internetu w klasycznym modelu wdrażania](load-balancer-get-started-internet-classic-cli.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>Wdrażanie rozwiązania przy użyciu programu Azure PowerShell
Poniższe procedury przedstawiają sposób tworzenia dostępnego z Internetu modułu równoważenia obciążenia w usłudze Azure Resource Manager przy użyciu programu PowerShell. Usługa Azure Resource Manager pozwala tworzyć i konfigurować każdy zasób osobno, a następnie łączyć je ze sobą, aby utworzyć moduł równoważenia obciążenia.

Aby wdrożyć moduł równoważenia obciążenia, należy utworzyć i skonfigurować poniższe obiekty:

* Konfiguracja IP frontonu — publiczne adresy IP (PIP) dla przychodzącego ruchu sieciowego.
* Pula adresów zaplecza — interfejsy sieciowe (NIC) maszyn wirtualnych odbierających ruch sieciowy z modułu równoważenia obciążenia.
* Reguły równoważenia obciążenia — reguły mapowania portu publicznego modułu równoważenia obciążenia na port w puli adresów zaplecza.
* Reguły NAT ruchu przychodzącego — reguły mapowania portu publicznego modułu równoważenia obciążenia na port określonej maszyny wirtualnej w puli adresów zaplecza.
* Sondy — sondy kondycji używane do sprawdzania dostępności wystąpień maszyny wirtualnej w puli adresów zaplecza.

Aby uzyskać więcej informacji, zobacz artykuł [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Obsługa usługi Azure Resource Manager dla modułu równoważenia obciążenia).

## <a name="set-up-powershell-to-use-resource-manager"></a>Konfigurowanie programu PowerShell do korzystania z usługi Resource Manager
Upewnij się, że masz najnowszą wersję modułu usługi Azure Resource Manager dla programu PowerShell:

1. Zaloguj się do platformy Azure.

        Login-AzureRmAccount

    Po wyświetleniu monitu wprowadź poświadczenia.
2. Sprawdź subskrypcje dostępne na koncie.

        Get-AzureRmSubscription
3. Wybierz subskrypcję platformy Azure do użycia.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
4. Utwórz grupę zasobów. (Pomiń ten krok, jeśli używasz istniejącej grupy zasobów).

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-frontend-ip-pool"></a>Tworzenie sieci wirtualnej oraz publicznego adresu IP dla puli adresów IP frontonu
1. Utwórz podsieć i sieć wirtualną.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
2. Utwórz zasób publicznego adresu IP Azure o nazwie **PublicIP**, który zostanie użyty przez pulę adresów IP frontonu z serwerem DNS o nazwie **loadbalancernrp.westus.cloudapp.azure.com**. Poniższe polecenie używa alokacji typu statycznego.

        $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' –AllocationMethod Static -DomainNameLabel loadbalancernrp

   > [!IMPORTANT]
   > Moduł równoważenia obciążenia używa etykiety domeny publicznego adresu IP jako prefiksu nazwy FQDN. Różni się to od klasycznego modelu wdrażania, który wykorzystuje usługę w chmurze jako nazwę FQDN modułu równoważenia obciążenia.
   > W tym przykładzie FQDN to **loadbalancernrp.westus.cloudapp.azure.com**.
   >
   >

## <a name="create-a-frontend-ip-pool-and-a-backend-address-pool"></a>Tworzenie puli adresów IP frontonu i puli adresów zaplecza
1. Utwórz pulę adresów IP frontonu o nazwie **LB-Frontend** używającą zasobu **PublicIp**.

        $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP
2. Utwórz pulę adresów zaplecza o nazwie **LB-backend**.

        $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>Tworzenie reguł NAT, reguł modułu równoważenia obciążenia, sondy oraz modułu równoważenia obciążenia
W tym przykładzie opisano tworzenie następujących elementów:

* Reguła NAT do translacji całego ruchu przychodzącego do portu 3441 na port 3389
* Reguła NAT do translacji całego ruchu przychodzącego do portu 3442 na port 3389
* Reguła sondy do sprawdzania kondycji na stronie o nazwie **HealthProbe.aspx**
* Reguła modułu równoważenia obciążenia do równoważenia całego ruchu przychodzącego do portu 80 na port 80 adresów w puli zaplecza
* Modułu równoważenia obciążenia, który korzysta ze wszystkich wymienionych obiektów

Wykonaj następujące kroki:

1. Utwórz reguły NAT.

        $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

        $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389
2. Utwórz sondę kondycji. Istnieją dwie metody konfiguracji sondy:

    Sonda HTTP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    Sonda TCP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
3. Utwórz regułę modułu równoważenia obciążenia.

        $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
4. Utwórz moduł równoważenia obciążenia przy użyciu utworzonych wcześniej obiektów.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe

## <a name="create-nics"></a>Tworzenie kart sieciowych
Utwórz karty sieciowe (lub zmodyfikuj istniejące) i powiąż je z regułami NAT, regułami modułu równoważenia obciążenia i sondami:

1. Uzyskaj sieć wirtualną i podsieć sieci wirtualnej, w której ma zostać utworzona karta sieciowa.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
2. Utwórz kartę sieciową o nazwie **lb-nic1-be** i powiąż ją z pierwszą regułą NAT oraz pierwszą (i jedyną) pulą adresów zaplecza.

        $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
3. Utwórz kartę sieciową o nazwie **lb-nic2-be** i powiąż ją z drugą regułą NAT oraz pierwszą (i jedyną) pulą adresów zaplecza.

        $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
4. Sprawdź karty sieciowe.

        $backendnic1

    Oczekiwane dane wyjściowe:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :
5. Użyj polecenia cmdlet `Add-AzureRmVMNetworkInterface`, aby przypisać karty sieciowe do różnych maszyn wirtualnych.

## <a name="create-a-virtual-machine"></a>Tworzenie maszyny wirtualnej
Wskazówki dotyczące tworzenia maszyny wirtualnej i przypisywania karty sieciowej znajdują się w artykule [Create an Azure VM using PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md) (Tworzenie maszyny wirtualnej Azure za pomocą programu PowerShell).

## <a name="add-the-network-interface-to-the-load-balancer"></a>Dodawanie interfejsu sieciowego do modułu równoważenia obciążenia
1. Uzyskaj moduł równoważenia obciążenia z usługi Azure.

    Załaduj zasób modułu równoważenia obciążenia do zmiennej (jeśli nie załadowano wcześniej). Zmienna ma nazwę **$lb**. Użyj utworzonych wcześniej nazw z zasobu modułu równoważenia obciążenia.

        $lb= get-azurermloadbalancer –name NRP-LB -resourcegroupname NRP-RG
2. Załaduj konfigurację zaplecza do zmiennej.

        $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb
3. Załaduj uprzednio utworzony interfejs sieciowy do zmiennej. Zmienna ma nazwę **$nic**. Nazwa interfejsu sieciowego jest taka sama, jak w poprzednim przykładzie.

        $nic =get-azurermnetworkinterface –name lb-nic1-be -resourcegroupname NRP-RG
4. Zmień konfigurację zaplecza na interfejsie sieciowym.

        $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
5. Zapisz obiekt interfejsu sieciowego.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Po dodaniu interfejsu sieciowego do puli zaplecza modułu równoważenia obciążenia zacznie on odbierać ruch sieciowy na podstawie reguł równoważenia obciążenia dla tego zasobu modułu równoważenia obciążenia.

## <a name="update-an-existing-load-balancer"></a>Aktualizowanie istniejącego modułu równoważenia obciążenia
1. Używając modułu równoważenia obciążenia z poprzedniego przykładu, przypisz obiekt modułu równoważenia obciążenia do zmiennej **$slb** za pomocą polecenia `Get-AzureLoadBalancer`.

        $slb = get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
2. W poniższym przykładzie należy dodać regułę ruchu przychodzącego NAT — przy użyciu portu 81 w puli frontonu i portu 8181 puli zaplecza — do istniejącego modułu równoważenia obciążenia.

        $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP
3. Zapisz nową konfigurację za pomocą polecenia `Set-AzureLoadBalancer`.

        $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Usuwanie modułu równoważenia obciążenia
Za pomocą polecenia `Remove-AzureLoadBalancer` usuń utworzony wcześniej moduł równoważenia obciążenia o nazwie **NRP-LB** w grupie zasobów o nazwie **NRP-RG**.

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

> [!NOTE]
> Możesz użyć opcjonalnego przełącznika **-Force**, aby pominąć monit o usunięcie.
>
>

## <a name="next-steps"></a>Następne kroki
[Get started configuring an internal load balancer](load-balancer-get-started-ilb-arm-ps.md) (Wprowadzenie do konfigurowania wewnętrznego modułu równoważenia obciążenia)

[Configure a load balancer distribution mode](load-balancer-distribution-mode.md) (Konfigurowanie trybu dystrybucji modułu równoważenia obciążenia)

[Configure idle TCP timeout settings for your load balancer](load-balancer-tcp-idle-timeout.md) (Konfigurowanie ustawień limitu czasu bezczynności protokołu TCP dla modułu równoważenia obciążenia)



<!--HONumber=Nov16_HO2-->


