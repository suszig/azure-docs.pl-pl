---
title: "Tworzenie wewnętrznego modułu równoważenia obciążenia przy użyciu programu PowerShell w usłudze Resource Manager | Microsoft Docs"
description: "Dowiedz się, jak utworzyć wewnętrzny moduł równoważenia obciążenia przy użyciu programu PowerShell w usłudze Resource Manager"
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: c6c98981-df9d-4dd7-a94b-cc7d1dc99369
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 02d32ef115a6c2d9b0bb891231f3b45051ef0675


---
# <a name="create-an-internal-load-balancer-using-powershell"></a>Tworzenie wewnętrznego modułu równoważenia obciążenia przy użyciu programu PowerShell
[!INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

<BR>
[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]

[klasyczny model wdrażania](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

Poniższe kroki przedstawiają sposób tworzenia wewnętrznego modułu równoważenia obciążenia w usłudze Azure Resource Manager przy użyciu programu PowerShell. W usłudze Azure Resource Manager elementy przeznaczone do tworzenia wewnętrznego modułu równoważenia obciążenia są konfigurowane indywidualnie, a następnie łączone w celu utworzenia modułu równoważenia obciążenia.

Aby wdrożyć moduł równoważenia obciążenia, należy utworzyć i skonfigurować następujące obiekty:

* Konfiguracja adresu IP frontonu — umożliwia skonfigurowanie prywatnego adresu IP na potrzeby przychodzącego ruchu sieciowego.
* Pula adresów zaplecza — umożliwia skonfigurowanie interfejsów sieciowych, które będą odbierać ruch ze zrównoważonym obciążeniem z puli adresów IP frontonu.
* Reguły równoważenia obciążenia — konfiguracja źródła i portu lokalnego na potrzeby modułu równoważenia obciążenia.
* Sondy — umożliwia skonfigurowanie sondy stanu kondycji wystąpień maszyny wirtualnej.
* Reguły NAT dla ruchu przychodzącego — umożliwiają skonfigurowanie reguł portu w celu uzyskiwania bezpośredniego dostępu do jednego z wystąpień maszyny wirtualnej.

Więcej informacji o składnikach modułu równoważenia obciążenia tworzonego za pomocą usługi Azure Resource Manager można znaleźć w artykule [Azure Resource Manager support for load balancer](load-balancer-arm.md) (Obsługa usługi Azure Resource Manager dla modułu równoważenia obciążenia).

Poniższe kroki przedstawiają sposób konfigurowania modułu równoważenia obciążenia między dwiema maszynami wirtualnymi.

## <a name="setup-powershell-to-use-resource-manager"></a>Konfigurowanie programu PowerShell do korzystania z usługi Resource Manager
Upewnij się, że masz najnowszą wersję produkcyjną modułu platformy Azure dla programu PowerShell oraz że program PowerShell został prawidłowo skonfigurowany do uzyskiwania dostępu do subskrypcji platformy Azure.

### <a name="step-1"></a>Krok 1
        Login-AzureRmAccount

### <a name="step-2"></a>Krok 2
Sprawdź subskrypcje dostępne na koncie.

        Get-AzureRmSubscription

Otrzymasz monit o uwierzytelnienie się przy użyciu swoich poświadczeń.<BR>

### <a name="step-3"></a>Krok 3
Wybierz subskrypcję platformy Azure do użycia. <BR>

        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="create-resource-group-for-load-balancer"></a>Tworzenie grupy zasobów dla modułu równoważenia obciążenia
### <a name="step-4"></a>Krok 4
Utwórz nową grupę zasobów (ten krok można pominąć, jeśli używasz istniejącej grupy zasobów).

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

Usługa Azure Resource Manager wymaga, aby wszystkie grupy zasobów określały lokalizację. Będzie ona używana jako domyślna lokalizacja dla zasobów w danej grupie zasobów. Upewnij się, że we wszystkich poleceniach służących do tworzenia modułu równoważenia obciążenia jest używana ta sama grupa zasobów.

W powyższym przykładzie utworzyliśmy grupę zasobów o nazwie „NRP-RG” i lokalizacji „Zachodnie stany USA”.

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Tworzenie sieci wirtualnej i prywatnego adresu IP dla puli adresów IP frontonu
### <a name="step-1"></a>Krok 1
Umożliwia utworzenie podsieci dla sieci wirtualnej, a następnie przypisanie jej do zmiennej $backendSubnet.

    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24

Utwórz sieć wirtualną:

    $vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

Umożliwia utworzenie sieci wirtualnej, dodanie elementu lb-subnet-be podsieci do sieci wirtualnej NRPVNet oraz przypisanie do zmiennej $vnet.

## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Tworzenie puli adresów IP frontonu i puli adresów zaplecza
Konfigurowanie puli adresów IP frontonu dla przychodzącego ruchu sieciowego modułu obciążenia równoważenia i puli adresów zaplecza w celu odbierania ruchu ze zrównoważonym obciążeniem.

### <a name="step-1"></a>Krok 1
Utwórz pulę adresów IP frontonu, używając prywatnego adresu IP 10.0.2.5 dla podsieci 10.0.2.0/24. Będzie ona punktem końcowym przychodzącego ruchu sieciowego.

    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id

### <a name="step-2"></a>Krok 2
Skonfiguruj pulę adresów zaplecza używaną do odbierania ruchu przychodzącego z puli adresów IP frontonu:

    $beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"


## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>Tworzenie reguł równoważenia obciążenia, reguł NAT, sondy i modułu równoważenia obciążenia
Po utworzeniu puli IP frontonu i puli adresów zaplecza należy utworzyć reguły, które będą należeć do zasobu modułu równoważenia obciążenia:

### <a name="step-1"></a>Krok 1
    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

     $lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80


W powyższym przykładzie są tworzone następujące elementy:

* reguła NAT, zgodnie z którą cały ruch przychodzący do portu 3441 będzie przekazywany do portu 3389;
* druga reguła NAT, zgodnie z którą cały ruch przychodzący do portu 3442 będzie przekazywany do portu 3389;
* reguła modułu równoważenia obciążenia, zgodnie z którą obciążenie całego ruchu przychodzącego do publicznego portu 80 będzie równoważone w lokalnym porcie 80 w puli adresów zaplecza;
* reguła sondy służąca do sprawdzania stanu kondycji dla ścieżki „HealthProbe.aspx”.

### <a name="step-2"></a>Krok 2
Utwórz moduł równoważenia obciążenia, dodając wszystkie obiekty (reguły NAT, reguły modułu równoważenia obciążenia, konfiguracje sondy):

    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe


## <a name="create-network-interfaces"></a>Tworzenie interfejsów sieciowych
Po utworzeniu wewnętrznego modułu równoważenia obciążenia należy zdefiniować interfejsy sieciowe, które będą odbierać przychodzący ruch sieciowy ze zrównoważonym obciążeniem, reguły NAT i sondę. W tym przypadku interfejs sieciowy jest konfigurowany indywidualnie i można go później przypisać do maszyny wirtualnej.

### <a name="step-1"></a>Krok 1
Pobierz podsieć i sieć wirtualną zasobu w celu utworzenia interfejsów sieciowych:

    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet


W ramach tego kroku jest tworzony interfejs sieciowy, który będzie należał do puli zaplecza modułu równoważenia obciążenia, oraz skojarzenie pierwszej reguły NAT dotyczącej protokołu RDP dla tego interfejsu sieciowego:

    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

### <a name="step-2"></a>Krok 2
Utwórz drugi interfejs sieciowej o nazwie LB-Nic2-BE:

W ramach tego kroku jest tworzony drugi interfejs sieciowy przypisywany do tej samej puli zaplecza modułu równoważenia obciążenia i definiujący skojarzenie drugiej reguły NAT utworzonej na potrzeby protokołu RDP:

     $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

Wynik końcowy będzie następujący:

    $backendnic1

Oczekiwane dane wyjściowe:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
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
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Krok 3
Za pomocą polecenia Add-AzureRmVMNetworkInterface przypisz kartę sieciową do maszyny wirtualnej.

Instrukcje krok po kroku dotyczące tworzenia maszyny wirtualnej i przypisywania do karty sieciowej można znaleźć w dokumentacji: [Tworzenie maszyny wirtualnej z systemem Windows przy użyciu usługi Resource Manager i programu PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md).

Jeśli masz już utworzoną maszynę wirtualną, możesz dodać interfejs sieciowy, wykonując następujące kroki:

#### <a name="step-1"></a>Krok 1
Załaduj zasób modułu równoważenia obciążenia do zmiennej (jeśli nie załadowano wcześniej). Używana zmienna ma nazwę $lb i korzysta z nazw z zasobu modułu równoważenia obciążenia utworzonego powyżej.

    $lb= Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG

#### <a name="step-2"></a>Krok 2
Załaduj konfigurację zaplecza do zmiennej.

    $backend= Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

#### <a name="step-3"></a>Krok 3
Załaduj uprzednio utworzony interfejs sieciowy do zmiennej. Używana nazwa zmiennej to $nic. Używana nazwa interfejsu sieciowego jest taka sama jak w powyższym przykładzie.

    $nic=Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG

#### <a name="step-4"></a>Krok 4
Zmień konfigurację zaplecza na interfejsie sieciowym.

    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

#### <a name="step-5"></a>Krok 5
Zapisz obiekt interfejsu sieciowego.

    Set-AzureRmNetworkInterface -NetworkInterface $nic

Po dodaniu interfejsu sieciowego do puli zaplecza modułu równoważenia obciążenia zacznie on odbierać ruch sieciowy na podstawie reguł równoważenia obciążenia dla tego zasobu modułu równoważenia obciążenia.

## <a name="update-an-existing-load-balancer"></a>Aktualizowanie istniejącego modułu równoważenia obciążenia
### <a name="step-1"></a>Krok 1
Używając modułu równoważenia obciążenia z poprzedniego przykładu, przypisz obiekt modułu równoważenia obciążenia do zmiennej $slb za pomocą polecenia Get-AzureRmLoadBalancer.

    $slb=get-azureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

### <a name="step-2"></a>Krok 2
W poniższym przykładzie dodasz nową regułę ruchu przychodzącego NAT — przy użyciu portu 81 w puli frontonu i portu 8181 w puli zaplecza — do istniejącego modułu równoważenia obciążenia.

    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp


### <a name="step-3"></a>Krok 3
Zapisz nową konfigurację przy użyciu polecenia Set-AzureLoadBalancer.

    $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Usuwanie modułu równoważenia obciążenia
Za pomocą polecenia Remove-AzureRmLoadBalancer usuń utworzony wcześniej moduł równoważenia obciążenia o nazwie „NRP-LB” w grupie zasobów o nazwie „NRP-RG”.

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

> [!NOTE]
> Możesz użyć opcjonalnego przełącznika -Force, aby pominąć monit o usunięcie.
> 
> 

## <a name="next-steps"></a>Następne kroki
[Configure a Load balancer distribution mode](load-balancer-distribution-mode.md) (Konfigurowanie trybu dystrybucji modułu równoważenia obciążenia)

[Configure idle TCP timeout settings for your load balancer](load-balancer-tcp-idle-timeout.md) (Konfigurowanie ustawień limitu czasu bezczynności protokołu TCP dla modułu równoważenia obciążenia)




<!--HONumber=Nov16_HO2-->


