---
title: Konfigurowanie bramy aplikacji na potrzeby odciążania protokołu SSL przy użyciu usługi Azure Resource Manager | Microsoft Docs
description: Ta strona zawiera instrukcje dotyczące tworzenia bramy aplikacji na potrzeby odciążania protokołu SSL za pomocą szablonu usługi Azure Resource Manager
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
ms.date: 09/09/2016
ms.author: gwallace

---
# Konfigurowanie bramy aplikacji na potrzeby odciążania protokołu SSL przy użyciu usługi Azure Resource Manager
> [!div class="op_single_selector"]
> -[Portal Azure](application-gateway-ssl-portal.md)
> -[Azure Resource Manager — program PowerShell](application-gateway-ssl-arm.md)
> -[Klasyczny Portal Azure — program PowerShell](application-gateway-ssl.md)
> 
> 

 Usługę Azure Application Gateway można skonfigurować tak, aby przerywała sesję protokołu SSL (Secure Sockets Layer) na poziomie bramy, co pozwoli na uniknięcie wykonywania kosztownych zadań szyfrowania protokołu SSL w kolektywie serwerów sieci Web. Odciążanie protokołu SSL upraszcza również konfigurowanie serwerów frontonu i zarządzanie aplikacją sieci Web.

## Przed rozpoczęciem
1. Zainstaluj najnowszą wersję poleceń cmdlet programu Azure PowerShell za pomocą Instalatora platformy sieci Web. Najnowszą wersję można pobrać i zainstalować z sekcji **Windows PowerShell** strony [Pliki do pobrania](https://azure.microsoft.com/downloads/).
2. Tworzona jest sieć wirtualna i podsieć dla bramy aplikacji. Upewnij się, że z podsieci nie korzystają żadne maszyny wirtualne ani wdrożenia w chmurze. Usługa Application Gateway musi sama znajdować się w podsieci sieci wirtualnej.
3. Serwery konfigurowane do używania bramy aplikacji muszą być umieszczone w sieci wirtualnej lub z przypisanym adresem IP/VIP lub mieć w niej utworzone punkty końcowe.

## Co jest wymagane do utworzenia bramy aplikacji?
* **Pula serwerów zaplecza:** lista adresów IP serwerów zaplecza. Adresy IP na liście powinny należeć do podsieci sieci wirtualnej lub być publicznymi bądź wirtualnymi adresami IP.
* **Ustawienia puli serwerów zaplecza:** każda pula ma ustawienia, takie jak port, protokół i koligacja oparta na plikach cookie. Te ustawienia są powiązane z pulą i są stosowane do wszystkich serwerów w tej puli.
* **Port frontonu:** port publiczny, który jest otwierany w bramie aplikacji. Ruch trafia do tego portu, a następnie jest przekierowywany do jednego z serwerów zaplecza.
* **Odbiornik:** odbiornik ma port frontonu, protokół (Http lub Https, z uwzględnieniem wielkości liter) oraz nazwę certyfikatu SSL (w przypadku konfigurowania odciążania protokołu SSL).
* **Reguła:** reguła wiąże odbiornik z pulą serwerów zaplecza i umożliwia zdefiniowanie, do której puli serwerów zaplecza ma być przekierowywany ruch w przypadku trafienia do określonego odbiornika. Obecnie jest obsługiwana tylko reguła *podstawowa*. Reguła *podstawowa* to dystrybucja obciążenia z działaniem okrężnym.

**Dodatkowe uwagi dotyczące konfiguracji**

W przypadku konfiguracji certyfikatów SSL protokół w polu **HttpListener** należy zmienić na *Https* (z uwzględnieniem wielkości liter). Element **SslCertificate** jest dodawany do odbiornika **HttpListener** z wartością zmiennej skonfigurowaną dla certyfikatu SSL. Port frontonu należy zaktualizować do 443.

**Aby włączyć koligację opartą na plikach cookie**: bramę aplikacji można skonfigurować tak, aby żądanie z sesji klienta było zawsze kierowane do tej samej maszyny wirtualnej w kolektywie serwerów sieci Web. W tym scenariuszu należy wstrzyknąć plik cookie sesji, który umożliwi bramie prawidłowe kierowanie ruchu. Aby włączyć koligację opartą na plikach cookie, ustaw element **CookieBasedAffinity** na wartość *Enabled* w elemencie **BackendHttpSettings**.

## Tworzenie bramy aplikacji
Różnica między klasycznym modelem wdrożenia platformy Azure i usługą Azure Resource Management polega na kolejności tworzenia bramy aplikacji i elementów, które należy skonfigurować.

W usłudze Resource Manager wszystkie składniki bramy aplikacji są konfigurowane osobno, a następnie składane w celu utworzenia zasobu bramy aplikacji.

Oto kroki wymagane do utworzenia bramy aplikacji:

1. Tworzenie grupy zasobów dla usługi Resource Manager
2. Tworzenie sieci wirtualnej, podsieci i publicznego adresu IP bramy aplikacji
3. Tworzenie obiektu konfiguracji bramy aplikacji
4. Tworzenie zasobu bramy aplikacji

## Tworzenie grupy zasobów dla usługi Resource Manager
Upewnij się, że program PowerShell został przełączony do trybu umożliwiającego korzystanie z poleceń cmdlet usługi Azure Resource Manager. Więcej informacji znajduje się w artykule [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Używanie programu Windows PowerShell z usługą Resource Manager).

### Krok 1
    Login-AzureRmAccount

### Krok 2
Sprawdź subskrypcje dostępne na koncie.

    Get-AzureRmSubscription

Zostanie wyświetlony monit o uwierzytelnienie przy użyciu własnych poświadczeń.<BR>

### Krok 3
Wybierz subskrypcję platformy Azure do użycia. <BR>

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### Krok 4
Utwórz grupę zasobów (ten krok można pominąć, jeśli używasz istniejącej grupy zasobów).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Usługa Azure Resource Manager wymaga, aby wszystkie grupy zasobów określały lokalizację. To ustawienie jest używane jako domyślna lokalizacja dla zasobów w danej grupie zasobów. Upewnij się, że we wszystkich poleceniach służących do tworzenia bramy aplikacji jest używana ta sama grupa zasobów.

W powyższym przykładzie utworzyliśmy grupę zasobów o nazwie „appgw-RG” i lokalizacji „Zachodnie stany USA”.

## Tworzenie sieci wirtualnej i podsieci dla bramy aplikacji
W poniższym przykładzie pokazano, jak utworzyć sieć wirtualną przy użyciu usługi Resource Manager:

### Krok 1
    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Ten przykład umożliwia przypisanie zakresu adresów 10.0.0.0/24 do zmiennej podsieci służącej do tworzenia sieci wirtualnej.

### Krok 2
    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

Ten przykład umożliwia utworzenie sieci wirtualnej o nazwie „appgwvnet” w grupie zasobów „appgw-rg” dla regionu Zachodnie stany USA przy użyciu prefiksu 10.0.0.0/16 i podsieci 10.0.0.0/24.

### Krok 3
    $subnet = $vnet.Subnets[0]

Ten przykład umożliwia przypisanie obiektu podsieci do zmiennej $subnet na potrzeby następnych kroków.

## Tworzenie publicznego adresu IP dla konfiguracji frontonu
    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

Ten przykład umożliwia utworzenie zasobu publicznego adresu IP „publicIP01” w grupie zasobów „appgw-rg” dla regionu Zachodnie stany USA.

## Tworzenie obiektu konfiguracji bramy aplikacji
### Krok 1
    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Ten przykład umożliwia utworzenie konfiguracji adresu IP bramy aplikacji o nazwie „gatewayIP01”. Uruchomiona usługa Application Gateway wybierze adres IP ze skonfigurowanej podsieci i skieruje ruch sieciowy do adresów IP w puli adresów IP zaplecza. Pamiętaj, że każde wystąpienie będzie mieć jeden adres IP.

### Krok 2
    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Ten przykład umożliwia skonfigurowanie puli adresów IP zaplecza o nazwie „pool01” z adresami IP „134.170.185.46, 134.170.188.221,134.170.185.50”. Te wartości to adresy IP odbierające ruch sieciowy pochodzący z punktu końcowego adresu IP frontonu. Zastąp adresy IP z poprzedniego przykładu adresami IP punktów końcowych aplikacji sieci Web.

### Krok 3
    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

Ten przykład umożliwia skonfigurowanie ustawienia bramy aplikacji „poolsetting01” dla ruchu sieciowego ze zrównoważonym obciążeniem w puli zaplecza.

### Krok 4
    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

Ten przykład umożliwia skonfigurowanie portu adresu IP frontonu o nazwie „frontendport01” dla punktu końcowego publicznego adresu IP.

### Krok 5
    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

Ten przykład umożliwia skonfigurowanie certyfikatu używanego na potrzeby połączenia SSL. Certyfikat musi być w formacie PFX, a hasło musi składać się z 4–12 znaków.

### Krok 6
    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

Ten przykład umożliwia utworzenie konfiguracji adresów IP frontonu o nazwie „fipconfig01” i skojarzenie publicznego adresu IP z konfiguracją adresów IP frontonu.

### Krok 7
    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


Ten przykład umożliwia utworzenie odbiornika o nazwie „listener01” i skojarzenie portu frontonu z certyfikatem i konfiguracją adresów IP frontonu.

### Krok 8
    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Ten przykład umożliwia utworzenie reguły routingu modułu równoważenia obciążenia o nazwie „rule01”, która służy do konfigurowania zachowania modułu równoważenia obciążenia.

### Krok 9
    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Ten przykład umożliwia skonfigurowanie rozmiaru wystąpienia bramy aplikacji.

> [!NOTE]
> Wartość domyślna parametru *InstanceCount* to 2, a wartość maksymalna — 10. Wartość domyślna parametru *GatewaySize* to Medium. Możesz wybrać następujące wartości: Standard_Small, Standard_Medium i Standard_Large.
> 
> 

## Tworzenie bramy aplikacji przy użyciu polecenia New-AzureApplicationGateway
    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

Ten przykład umożliwia utworzenie bramy aplikacji przy użyciu wszystkich elementów konfiguracji z poprzednich kroków. W przykładzie brama aplikacji ma nazwę „appgwtest”.

## Następne kroki
Jeśli chcesz skonfigurować bramę aplikacji do użycia z wewnętrznego modułem równoważenia obciążenia, zobacz artykuł [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md) (Tworzenie bramy aplikacji przy użyciu wewnętrznego modułu równoważenia obciążenia).

Więcej ogólnych informacji na temat opcji równoważenia obciążenia możesz znaleźć w następujących artykułach:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

<!--HONumber=Sep16_HO3-->


