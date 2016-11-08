---
title: Tworzenie i konfigurowanie bramy aplikacji przy użyciu wewnętrznego modułu równoważenia obciążenia i usługi Azure Resource Manager | Microsoft Docs
description: Ta strona zawiera instrukcje dotyczące tworzenia, konfigurowania, uruchamiania i usuwania bramy aplikacji platformy Azure za pomocą wewnętrznego modułu równoważenia obciążenia na potrzeby usługi Azure Resource Manager
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
ms.date: 08/19/2016
ms.author: gwallace

---
# Tworzenie i konfigurowanie bramy aplikacji przy użyciu wewnętrznego modułu równoważenia obciążenia i usługi Azure Resource Manager
> [!div class="op_single_selector"]
> * [Kroki dla klasycznego portalu Azure](application-gateway-ilb.md)
> * [Kroki dla programu PowerShell w usłudze Resource Manager](application-gateway-ilb-arm.md)
> 
> 

Usługę Azure Application Gateway można skonfigurować z internetowym wirtualnym adresem IP lub wewnętrznym punktem końcowym niepołączonym z Internetem, znanym także jako punkt końcowy wewnętrznego modułu równoważenia obciążenia. Konfigurowanie bramy przy użyciu wewnętrznego modułu równoważenia obciążenia jest pomocne w przypadku wewnętrznych aplikacji LOB niepołączonych z Internetem. Ta opcja jest również przydatna w przypadku usług i warstw w aplikacji wielowarstwowej, która znajduje się w granicach zabezpieczeń bez połączenia z Internetem, ale nadal wymaga dystrybucji obciążenia z działaniem okrężnym, lepkości sesji lub zakończenia protokołu SSL (Secure Sockets Layer).

W tym artykule przeprowadzimy Cię przez proces konfigurowania bramy aplikacji przy użyciu wewnętrznego modułu równoważenia obciążenia.

## Przed rozpoczęciem
1. Zainstaluj najnowszą wersję poleceń cmdlet programu Azure PowerShell za pomocą Instalatora platformy sieci Web. Najnowszą wersję można pobrać i zainstalować z sekcji **Windows PowerShell** strony [Pliki do pobrania](https://azure.microsoft.com/downloads/).
2. Utworzysz sieć wirtualną i podsieć dla usługi Application Gateway. Upewnij się, że z podsieci nie korzystają żadne maszyny wirtualne ani wdrożenia w chmurze. Usługa Application Gateway musi sama znajdować się w podsieci sieci wirtualnej.
3. Serwery konfigurowane do używania bramy aplikacji muszą być umieszczone w sieci wirtualnej lub z przypisanym adresem IP/VIP lub mieć w niej utworzone punkty końcowe.

## Co jest wymagane do utworzenia bramy aplikacji?
* **Pula serwerów zaplecza:** lista adresów IP serwerów zaplecza. Adresy IP na liście powinny należeć do sieci wirtualnej, ale w innej podsieci bramy aplikacji, lub być publicznymi bądź wirtualnymi adresami IP.
* **Ustawienia puli serwerów zaplecza:** każda pula ma ustawienia, takie jak port, protokół i koligacja oparta na plikach cookie. Te ustawienia są powiązane z pulą i są stosowane do wszystkich serwerów w tej puli.
* **Port frontonu:** port publiczny, który jest otwierany w bramie aplikacji. Ruch trafia do tego portu, a następnie jest przekierowywany do jednego z serwerów zaplecza.
* **Odbiornik:** odbiornik ma port frontonu, protokół (Http lub Https, z uwzględnieniem wielkości liter) oraz nazwę certyfikatu SSL (w przypadku konfigurowania odciążania protokołu SSL).
* **Reguła:** reguła wiąże odbiornik z pulą serwerów zaplecza i umożliwia zdefiniowanie, do której puli serwerów zaplecza ma być przekierowywany ruch w przypadku trafienia do określonego odbiornika. Obecnie jest obsługiwana tylko reguła *podstawowa*. Reguła *podstawowa* to dystrybucja obciążenia z działaniem okrężnym.

## Tworzenie bramy aplikacji
Różnica między klasycznym modelem wdrożenia Azure i usługą Azure Resource Manager polega na kolejności tworzenia bramy aplikacji i elementów, które należy skonfigurować.
W usłudze Resource Manager wszystkie elementy składające się na bramę aplikacji są konfigurowane osobno, a następnie składane w celu utworzenia zasobu bramy aplikacji.

Oto kroki wymagane do utworzenia bramy aplikacji:

1. Tworzenie grupy zasobów dla usługi Resource Manager
2. Tworzenie sieci wirtualnej i podsieci dla bramy aplikacji
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
Utwórz nową grupę zasobów (ten krok można pominąć, jeśli używasz istniejącej grupy zasobów).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Usługa Azure Resource Manager wymaga, żeby wszystkie grupy zasobów miały lokalizację. Będzie ona używana jako domyślna lokalizacja zasobów w danej grupie. Upewnij się, że we wszystkich poleceniach służących do tworzenia bramy aplikacji jest używana ta sama grupa zasobów.

W powyższym przykładzie utworzyliśmy grupę zasobów o nazwie „appgw-rg” i lokalizacji „Zachodnie stany USA”.

## Tworzenie sieci wirtualnej i podsieci dla bramy aplikacji
W poniższym przykładzie pokazano, jak utworzyć sieć wirtualną przy użyciu usługi Resource Manager:

### Krok 1
    $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Umożliwia przypisanie zakresu adresów 10.0.0.0/24 do zmiennej podsieci służącej do tworzenia sieci wirtualnej.

### Krok 2
    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig

Umożliwia utworzenie sieci wirtualnej o nazwie „appgwvnet” w grupie zasobów „appgw-rg” dla regionu Zachodnie stany USA przy użyciu prefiksu 10.0.0.0/16 i podsieci 10.0.0.0/24.

### Krok 3
    $subnet = $vnet.subnets[0]

Umożliwia przypisanie obiektu podsieci do zmiennej $subnet na potrzeby następnych kroków.

## Tworzenie obiektu konfiguracji bramy aplikacji
### Krok 1
    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Umożliwia utworzenie konfiguracji adresu IP bramy aplikacji o nazwie „gatewayIP01”. Uruchomiona usługa Application Gateway wybierze adres IP ze skonfigurowanej podsieci i skieruje ruch sieciowy do adresów IP w puli adresów IP zaplecza. Pamiętaj, że każde wystąpienie będzie mieć jeden adres IP.

### Krok 2
    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Umożliwia skonfigurowanie puli adresów IP zaplecza o nazwie „pool01” z adresami IP „134.170.185.46, 134.170.188.221,134.170.185.50”. Są to adresy IP odbierające ruch sieciowy pochodzący z punktu końcowego adresu IP frontonu. Powyższe adresy IP można zastąpić własnymi dodawanymi punktami końcowymi adresów IP aplikacji.

### Krok 3
    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

Umożliwia skonfigurowanie ustawienia bramy aplikacji „poolsetting01” dla ruchu sieciowego ze zrównoważonym obciążeniem w puli zaplecza.

### Krok 4
    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

Umożliwia skonfigurowanie portu adresu IP frontonu o nazwie „frontendport01” dla wewnętrznego modułu równoważenia obciążenia.

### Krok 5
    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

Umożliwia utworzenie konfiguracji adresów IP zaplecza o nazwie „fipconfig01” i skojarzenie jej z prywatnym adresem IP z bieżącej podsieci sieci wirtualnej.

### Krok 6
    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

Umożliwia utworzenie odbiornika o nazwie „listener01” i skojarzenie portu frontonu z konfiguracją adresów IP frontonu.

### Krok 7
    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Umożliwia utworzenie reguły routingu modułu równoważenia obciążenia o nazwie „rule01”, która określa zachowanie modułu równoważenia obciążenia.

### Krok 8
    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Umożliwia skonfigurowanie rozmiaru wystąpienia bramy aplikacji.

> [!NOTE]
> Wartość domyślna parametru *InstanceCount* to 2, a wartość maksymalna — 10. Wartość domyślna parametru *GatewaySize* to Medium. Możesz wybrać następujące wartości: Standard_Small, Standard_Medium i Standard_Large.
> 
> 

## Tworzenie bramy aplikacji przy użyciu polecenia New-AzureApplicationGateway
Umożliwia utworzenie bramy aplikacji przy użyciu wszystkich elementów konfiguracji z powyższych kroków. W tym przykładzie brama aplikacji ma nazwę „appgwtest”.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

Umożliwia utworzenie bramy aplikacji przy użyciu wszystkich elementów konfiguracji z powyższych kroków. W przykładzie brama aplikacji ma nazwę „appgwtest”.

## Usuwanie bramy aplikacji
Aby usunąć bramę aplikacji, wykonaj kolejno poniższe czynności:

1. Użyj polecenia cmdlet **Stop-AzureRmApplicationGateway**, aby zatrzymać bramę.
2. Użyj polecenia cmdlet **Remove-AzureRmApplicationGateway**, aby usunąć bramę.
3. Sprawdź, czy brama została usunięta, przy użyciu polecenia cmdlet **Get-AzureApplicationGateway**.

### Krok 1
Pobierz obiekt bramy aplikacji i skojarz go ze zmienną „$getgw”.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### Krok 2
Użyj polecenia cmdlet **Stop-AzureRmApplicationGateway**, aby zatrzymać bramę aplikacji. W tym przykładzie pokazano polecenie cmdlet **Stop-AzureRmApplicationGateway** w pierwszym wierszu, a następnie dane wyjściowe.

    PS C:\> Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Gdy brama aplikacji jest w stanie zatrzymania, użyj polecenia cmdlet **Remove-AzureRmApplicationGateway**, aby usunąć usługę.

    PS C:\> Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

> [!NOTE]
> Przełącznik **-force** umożliwia pomijanie komunikatu potwierdzającego usunięcie.
> 
> 

Aby sprawdzić, czy usługa została usunięta, możesz użyć polecenia cmdlet **Get-AzureRmApplicationGateway**. Ten krok nie jest wymagany.

    PS C:\>Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## Następne kroki
Jeśli chcesz skonfigurować odciążanie protokołu SSL, zobacz artykuł [Configure an application gateway for SSL offload](application-gateway-ssl.md) (Konfigurowanie bramy aplikacji na potrzeby odciążania protokołu SSL).

Jeśli chcesz skonfigurować bramę aplikacji do użycia z wewnętrznym modułem równoważenia obciążenia, zobacz artykuł [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md) (Tworzenie bramy aplikacji z wewnętrznym modułem równoważenia obciążenia).

Więcej ogólnych informacji na temat opcji równoważenia obciążenia możesz znaleźć w następujących artykułach:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

<!--HONumber=sep16_HO1-->


