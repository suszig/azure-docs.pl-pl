---
title: Tworzenie, uruchamianie lub usuwanie bramy aplikacji przy użyciu usługi Azure Resource Manager | Microsoft Docs
description: Ta strona zawiera instrukcje dotyczące tworzenia, konfigurowania, uruchamiania i usuwania bramy aplikacji platformy Azure za pomocą usługi Azure Resource Manager
documentationcenter: na
services: application-gateway
author: georgewallace
manager: carmonm
editor: tysonn

ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2016
ms.author: gwallace

---
# Tworzenie, uruchamianie lub usuwanie bramy aplikacji przy użyciu usługi Azure Resource Manager
Azure Application Gateway to moduł równoważenia obciążenia warstwy 7. Udostępnia tryb failover, oparty na wydajności routing żądań HTTP między różnymi serwerami — w chmurze i lokalnymi. Usługa Application Gateway oferuje następujące funkcje dostarczania aplikacji: równoważenie obciążenia HTTP, koligację sesji opartą na plikach cookie oraz odciążanie protokołu SSL (Secure Sockets Layer).

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager — program PowerShell](application-gateway-create-gateway-arm.md)
> * [Klasyczny portal Azure — program PowerShell](application-gateway-create-gateway.md)
> * [Szablon usługi Azure Resource Manager](application-gateway-create-gateway-arm-template.md)
> * [Interfejs wiersza polecenia platformy Azure](application-gateway-create-gateway-cli.md)
> 
> 

<BR>


W tym artykule przedstawiono kroki umożliwiające tworzenie, konfigurowanie, uruchamianie i usuwanie bramy aplikacji.

> [!IMPORTANT]
> Przed rozpoczęciem pracy z zasobami platformy Azure należy pamiętać, że ma ona obecnie dwa modele wdrażania: za pomocą usługi Resource Manager i model klasyczny. Przed rozpoczęciem pracy z dowolnym zasobem platformy Azure należy zapoznać się z [modelami i narzędziami wdrażania](../azure-classic-rm.md). Dokumentację dotyczącą różnych narzędzi można wyświetlić, klikając karty w górnej części artykułu. W tym dokumencie opisano tworzenie bramy aplikacji przy użyciu usługi Azure Resource Manager. Aby korzystać z modelu klasycznego, przejdź do artykułu [Create an application gateway classic deployment by using PowerShell](application-gateway-create-gateway.md) (Tworzenie klasycznego wdrożenia bramy aplikacji przy użyciu programu PowerShell).
> 
> 

## Przed rozpoczęciem
1. Zainstaluj najnowszą wersję poleceń cmdlet programu Azure PowerShell za pomocą Instalatora platformy sieci Web. Najnowszą wersję można pobrać i zainstalować z sekcji **Windows PowerShell** strony [Pliki do pobrania](https://azure.microsoft.com/downloads/).
2. Jeśli masz istniejącą sieć wirtualną, wybierz istniejącą pustą podsieć lub utwórz podsieć w istniejącej sieci wirtualnej wyłącznie do użytku przez tę bramę aplikacji. Nie można wdrożyć bramy aplikacji do innej sieci wirtualnej niż sieć wirtualna zasobów, które zamierzasz wdrożyć za bramą aplikacji. 
3. Serwery konfigurowane do używania bramy aplikacji muszą być umieszczone w sieci wirtualnej lub z przypisanym adresem IP/VIP lub mieć w niej utworzone punkty końcowe.

## Co jest wymagane do utworzenia bramy aplikacji?
* **Pula serwerów zaplecza:** lista adresów IP serwerów zaplecza. Adresy IP na liście powinny należeć do podsieci sieci wirtualnej lub być publicznymi bądź wirtualnymi adresami IP.
* **Ustawienia puli serwerów zaplecza:** każda pula ma ustawienia, takie jak port, protokół i koligacja oparta na plikach cookie. Te ustawienia są powiązane z pulą i są stosowane do wszystkich serwerów w tej puli.
* **Port frontonu:** port publiczny, który jest otwierany w bramie aplikacji. Ruch trafia do tego portu, a następnie jest przekierowywany do jednego z serwerów zaplecza.
* **Odbiornik:** odbiornik ma port frontonu, protokół (Http lub Https, z uwzględnieniem wielkości liter) oraz nazwę certyfikatu SSL (w przypadku konfigurowania odciążania protokołu SSL).
* **Reguła:** reguła wiąże odbiornik z pulą serwerów zaplecza i umożliwia zdefiniowanie, do której puli serwerów zaplecza ma być przekierowywany ruch w przypadku trafienia do określonego odbiornika. 

## Tworzenie bramy aplikacji
Różnica między klasycznym modelem wdrożenia Azure i usługą Azure Resource Manager polega na kolejności tworzenia bramy aplikacji i elementów, które należy skonfigurować.

W usłudze Resource Manager wszystkie elementy składające się na bramę aplikacji są konfigurowane osobno, a następnie składane w celu utworzenia zasobu bramy aplikacji.

Poniżej znajdują się kroki wymagane do utworzenia bramy aplikacji:

## Tworzenie grupy zasobów dla usługi Resource Manager
Upewnij się, że używasz najnowszej wersji programu Azure PowerShell. Więcej informacji znajduje się w artykule [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Używanie programu Windows PowerShell z usługą Resource Manager).

### Krok 1
Zaloguj się do konta Azure (Login-AzureRmAccount)

Zostanie wyświetlony monit o uwierzytelnienie przy użyciu własnych poświadczeń.<BR>

### Krok 2
Sprawdź subskrypcje dostępne na koncie.

        Get-AzureRmSubscription

### Krok 3
Wybierz subskrypcję platformy Azure do użycia. <BR>

        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### Krok 4
Utwórz nową grupę zasobów (ten krok można pominąć, jeśli używasz istniejącej grupy zasobów).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Usługa Azure Resource Manager wymaga, aby wszystkie grupy zasobów określały lokalizację. Ta lokalizacja będzie używana jako domyślna lokalizacja dla zasobów w danej grupie zasobów. Upewnij się, że we wszystkich poleceniach służących do tworzenia bramy aplikacji jest używana ta sama grupa zasobów.

W powyższym przykładzie utworzyliśmy grupę zasobów o nazwie „appgw-RG” i lokalizacji „Zachodnie stany USA”.

> [!NOTE]
> Jeśli musisz skonfigurować niestandardową sondę bramy aplikacji, zobacz artykuł [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md) (Tworzenie bramy aplikacji z sondami niestandardowymi przy użyciu programu PowerShell). Aby dowiedzieć się więcej, zapoznaj się z informacjami na temat [sond niestandardowych i monitorowania kondycji](application-gateway-probe-overview.md).
> 
> 

## Tworzenie sieci wirtualnej i podsieci dla bramy aplikacji
W poniższym przykładzie pokazano, jak utworzyć sieć wirtualną przy użyciu usługi Resource Manager.

### Krok 1
Przypisz zakres adresów 10.0.0.0/24 do zmiennej podsieci służącej do tworzenia sieci wirtualnej.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24


### Krok 2
Utwórz sieć wirtualną o nazwie „appgwvnet” w grupie zasobów „appgw-rg” dla regionu Zachodnie stany USA przy użyciu prefiksu 10.0.0.0/16 i podsieci 10.0.0.0/24.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### Krok 3
Przypisz zmienną podsieci na potrzeby następnych kroków umożliwiających utworzenie bramy aplikacji.

    $subnet=$vnet.Subnets[0]

## Tworzenie publicznego adresu IP dla konfiguracji frontonu
Utwórz zasób publicznego adresu IP „publicIP01” w grupie zasobów „appgw-rg” dla regionu Zachodnie stany USA.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic


## Tworzenie obiektu konfiguracji bramy aplikacji
Musisz skonfigurować wszystkie elementy konfiguracji przed utworzeniem bramy aplikacji. Poniższe kroki umożliwiają utworzenie elementów konfiguracji wymaganych w przypadku zasobu bramy aplikacji.

### Krok 1
Utwórz konfigurację adresu IP bramy aplikacji o nazwie „gatewayIP01”. Uruchomiona usługa Application Gateway wybierze adres IP ze skonfigurowanej podsieci i skieruje ruch sieciowy do adresów IP w puli adresów IP zaplecza. Pamiętaj, że każde wystąpienie będzie mieć jeden adres IP.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### Krok 2
Skonfiguruj pulę adresów IP zaplecza o nazwie „pool01” z adresami IP „134.170.185.46, 134.170.188.221,134.170.185.50”. Będą to adresy IP odbierające ruch sieciowy pochodzący z punktu końcowego adresu IP frontonu. Powyższe adresy IP można zastąpić własnymi dodawanymi punktami końcowymi adresów IP aplikacji.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50



### Krok 3
Skonfiguruj ustawienia bramy aplikacji „poolsetting01” dla ruchu sieciowego ze zrównoważonym obciążeniem w puli zaplecza.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled


### Krok 4
Skonfiguruj port adresu IP frontonu o nazwie „frontendport01” dla punktu końcowego publicznego adresu IP.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

### Krok 5
Utwórz konfigurację adresów IP frontonu o nazwie „fipconfig01” i skojarz publiczny adres IP z konfiguracją adresów IP frontonu.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip


### Krok 6
Utwórz odbiornik o nazwie „listener01” i skojarz port frontonu z konfiguracją adresów IP frontonu.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### Krok 7
Utwórz regułę routingu modułu równoważenia obciążenia o nazwie „rule01”, określającą zachowanie modułu równoważenia obciążenia.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### Krok 8
Skonfiguruj rozmiar wystąpienia bramy aplikacji.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

> [!NOTE]
> Wartość domyślna parametru *InstanceCount* to 2, a wartość maksymalna — 10. Wartość domyślna parametru *GatewaySize* to Medium. Możesz wybrać następujące wartości: Standard_Small, Standard_Medium i Standard_Large.
> 
> 

## Tworzenie bramy aplikacji przy użyciu polecenia New-AzureRmApplicationGateway
Utwórz bramę aplikacji przy użyciu wszystkich elementów konfiguracji z powyższych kroków. W tym przykładzie brama aplikacji ma nazwę „appgwtest”.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

### Krok 9
Pobierz szczegóły adresów DNS i VIP bramy aplikacji z zasobu publicznego adresu IP zasobu dołączonego do bramy aplikacji.

    Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  

    Name                     : publicIP01
    ResourceGroupName        : appgwtest 
    Location                 : westus
    Id                       : /subscriptions/<sub_id>/resourceGroups/appgw-rg/providers/Microsoft.Network/publicIPAddresses/publicIP01
    Etag                     : W/"12302060-78d6-4a33-942b-a494d6323767"
    ResourceGuid             : ee9gd76a-3gf6-4236-aca4-gc1f4gf14171
    ProvisioningState        : Succeeded
    Tags                     : 
    PublicIpAllocationMethod : Dynamic
    IpAddress                : 137.116.26.16
    IdleTimeoutInMinutes     : 4
    IpConfiguration          : {
                                 "Id": "/subscriptions/<sub_id>/resourceGroups/appgw-rg/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIPConfigurations/fipconfig01"
                               }
    DnsSettings              : {
                                 "Fqdn": "ee7aca47-4344-4810-a999-2c631b73e3cd.cloudapp.net"
                               } 



## Usuwanie bramy aplikacji
Aby usunąć bramę aplikacji, wykonaj następujące kroki:

### Krok 1
Pobierz obiekt bramy aplikacji i skojarz go ze zmienną „$getgw”.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### Krok 2
Użyj polecenia cmdlet **Stop-AzureRmApplicationGateway**, aby zatrzymać bramę aplikacji.

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  


Gdy brama aplikacji jest w stanie zatrzymania, użyj polecenia cmdlet **Remove-AzureRmApplicationGateway**, aby usunąć usługę.

    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force



> [!NOTE]
> Przełącznik **-force** umożliwia pomijanie komunikatu potwierdzającego usunięcie.
> 
> 

Aby sprawdzić, czy usługa została usunięta, możesz użyć polecenia cmdlet **Get-AzureRmApplicationGateway**. Ten krok nie jest wymagany.

    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg


## Następne kroki
Jeśli chcesz skonfigurować odciążanie protokołu SSL, zobacz artykuł [Configure an application gateway for SSL offload](application-gateway-ssl.md) (Konfigurowanie bramy aplikacji na potrzeby odciążania protokołu SSL).

Jeśli chcesz skonfigurować bramę aplikacji do użycia z wewnętrznym modułem równoważenia obciążenia, zobacz artykuł [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md) (Tworzenie bramy aplikacji przy użyciu wewnętrznego modułu równoważenia obciążenia).

Więcej ogólnych informacji na temat opcji równoważenia obciążenia możesz znaleźć w następujących artykułach:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

<!--HONumber=sep16_HO1-->


