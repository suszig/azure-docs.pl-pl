---
title: "Tworzenie, uruchamianie lub usuwanie bramy aplikacji przy użyciu usługi Azure Resource Manager | Microsoft Docs"
description: "Ta strona zawiera instrukcje dotyczące tworzenia, konfigurowania, uruchamiania i usuwania bramy aplikacji platformy Azure za pomocą usługi Azure Resource Manager"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 866e9b5f-0222-4b6a-a95f-77bc3d31d17b
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/10/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: d4da5dd65b39011ed0abb8bbbd47542d66e73ad6


---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Tworzenie, uruchamianie lub usuwanie bramy aplikacji przy użyciu usługi Azure Resource Manager
> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager — program PowerShell](application-gateway-create-gateway-arm.md)
> * [Klasyczny portal Azure — program PowerShell](application-gateway-create-gateway.md)
> * [Szablon usługi Azure Resource Manager](application-gateway-create-gateway-arm-template.md)
> * [Interfejs wiersza polecenia platformy Azure](application-gateway-create-gateway-cli.md)
> 
> 

Usługa Azure Application Gateway to moduł równoważenia obciążenia warstwy 7. Udostępnia tryb failover, oparty na wydajności routing żądań HTTP między różnymi serwerami — w chmurze i lokalnymi. Usługa Application Gateway zapewnia wiele funkcji kontrolera dostarczania aplikacji (ADC, Application Delivery Controller), w tym między innymi równoważenie obciążenia HTTP, koligację sesji na podstawie plików cookie, odciążanie protokołu Secure Sockets Layer (SSL), niestandardowe sondy kondycji i obsługę wielu witryn. Aby uzyskać pełną listę obsługiwanych funkcji, odwiedź stronę [Application Gateway — omówienie](application-gateway-introduction.md)

W tym artykule przedstawiono kroki umożliwiające tworzenie, konfigurowanie, uruchamianie i usuwanie bramy aplikacji.

> [!IMPORTANT]
> Przed rozpoczęciem pracy z zasobami platformy Azure należy pamiętać, że ma ona obecnie dwa modele wdrażania: za pomocą usługi Resource Manager i model klasyczny. Przed rozpoczęciem pracy z dowolnym zasobem platformy Azure należy zapoznać się z [modelami i narzędziami wdrażania](../azure-classic-rm.md). Dokumentację dotyczącą różnych narzędzi można wyświetlić, klikając karty w górnej części artykułu. W tym dokumencie opisano tworzenie bramy aplikacji przy użyciu usługi Azure Resource Manager. Aby korzystać z modelu klasycznego, przejdź do artykułu [Create an application gateway classic deployment by using PowerShell](application-gateway-create-gateway.md) (Tworzenie klasycznego wdrożenia bramy aplikacji przy użyciu programu PowerShell).
> 
> 

## <a name="before-you-begin"></a>Przed rozpoczęciem
1. Zainstaluj najnowszą wersję poleceń cmdlet programu Azure PowerShell za pomocą Instalatora platformy sieci Web. Najnowszą wersję można pobrać i zainstalować z sekcji **Windows PowerShell** strony [Pliki do pobrania](https://azure.microsoft.com/downloads/).
2. Jeśli masz istniejącą sieć wirtualną, wybierz istniejącą pustą podsieć lub utwórz podsieć w istniejącej sieci wirtualnej wyłącznie do użytku przez tę bramę aplikacji. Nie można wdrożyć bramy aplikacji do innej sieci wirtualnej niż sieć wirtualna zasobów, które zamierzasz wdrożyć za bramą aplikacji.
3. Serwery konfigurowane do używania bramy aplikacji muszą być umieszczone w sieci wirtualnej lub z przypisanym adresem IP/VIP lub mieć w niej utworzone punkty końcowe.

## <a name="what-is-required-to-create-an-application-gateway"></a>Co jest wymagane do utworzenia bramy aplikacji?
* **Pula serwerów zaplecza:** lista adresów IP serwerów zaplecza. Adresy IP na liście powinny należeć do podsieci sieci wirtualnej lub być publicznymi bądź wirtualnymi adresami IP.
* **Ustawienia puli serwerów zaplecza:** każda pula ma ustawienia, takie jak port, protokół i koligacja oparta na plikach cookie. Te ustawienia są powiązane z pulą i są stosowane do wszystkich serwerów w tej puli.
* **Port frontonu:** port publiczny, który jest otwierany w bramie aplikacji. Ruch trafia do tego portu, a następnie jest przekierowywany do jednego z serwerów zaplecza.
* **Odbiornik:** odbiornik ma port frontonu, protokół (Http lub Https, z uwzględnieniem wielkości liter) oraz nazwę certyfikatu SSL (w przypadku konfigurowania odciążania protokołu SSL).
* **Reguła:** reguła wiąże odbiornik z pulą serwerów zaplecza i umożliwia zdefiniowanie, do której puli serwerów zaplecza ma być przekierowywany ruch w przypadku trafienia do określonego odbiornika.

## <a name="create-an-application-gateway"></a>Tworzenie bramy aplikacji
Różnica między klasycznym modelem wdrożenia Azure i usługą Azure Resource Manager polega na kolejności tworzenia bramy aplikacji i elementów, które należy skonfigurować.

W usłudze Resource Manager wszystkie elementy składające się na bramę aplikacji są konfigurowane osobno, a następnie składane w celu utworzenia zasobu bramy aplikacji.

Poniżej znajdują się kroki wymagane do utworzenia bramy aplikacji.

## <a name="create-a-resource-group-for-resource-manager"></a>Tworzenie grupy zasobów dla usługi Resource Manager
Upewnij się, że używasz najnowszej wersji programu Azure PowerShell. Więcej informacji znajduje się w artykule [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Używanie programu Windows PowerShell z usługą Resource Manager).

### <a name="step-1"></a>Krok 1
Zaloguj się do platformy Azure.

```powershell
Login-AzureRmAccount
```

Zostanie wyświetlony monit o uwierzytelnienie przy użyciu własnych poświadczeń.

### <a name="step-2"></a>Krok 2
Sprawdź subskrypcje dostępne na koncie.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>Krok 3
Wybierz subskrypcję platformy Azure do użycia.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Krok 4
Utwórz grupę zasobów (ten krok można pominąć, jeśli używasz istniejącej grupy zasobów).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Usługa Azure Resource Manager wymaga, aby wszystkie grupy zasobów określały lokalizację. Ta lokalizacja będzie używana jako domyślna lokalizacja dla zasobów w danej grupie zasobów. Upewnij się, że we wszystkich poleceniach służących do tworzenia bramy aplikacji jest używana ta sama grupa zasobów.

W powyższym przykładzie utworzyliśmy grupę zasobów o nazwie „appgw-RG” i lokalizacji „Zachodnie stany USA”.

> [!NOTE]
> Jeśli musisz skonfigurować niestandardową sondę bramy aplikacji, zobacz artykuł [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md) (Tworzenie bramy aplikacji z sondami niestandardowymi przy użyciu programu PowerShell). Aby dowiedzieć się więcej, zapoznaj się z informacjami na temat [sond niestandardowych i monitorowania kondycji](application-gateway-probe-overview.md).
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Tworzenie sieci wirtualnej i podsieci dla bramy aplikacji
W poniższym przykładzie pokazano, jak utworzyć sieć wirtualną przy użyciu usługi Resource Manager.

### <a name="step-1"></a>Krok 1
Przypisz zakres adresów 10.0.0.0/24 do zmiennej podsieci służącej do tworzenia sieci wirtualnej.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Krok 2
Utwórz sieć wirtualną o nazwie „appgwvnet” w grupie zasobów „appgw-rg” dla regionu Zachodnie stany USA przy użyciu prefiksu 10.0.0.0/16 i podsieci 10.0.0.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Krok 3
Przypisz zmienną podsieci na potrzeby następnych kroków umożliwiających utworzenie bramy aplikacji.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-the-frontend-configuration"></a>Tworzenie publicznego adresu IP dla konfiguracji frontonu
Utwórz zasób publicznego adresu IP „publicIP01” w grupie zasobów „appgw-rg” dla regionu Zachodnie stany USA.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway-configuration-object"></a>Tworzenie obiektu konfiguracji bramy aplikacji
Musisz skonfigurować wszystkie elementy konfiguracji przed utworzeniem bramy aplikacji. Poniższe kroki umożliwiają utworzenie elementów konfiguracji wymaganych w przypadku zasobu bramy aplikacji.

### <a name="step-1"></a>Krok 1
Utwórz konfigurację adresu IP bramy aplikacji o nazwie „gatewayIP01”. Uruchomiona usługa Application Gateway wybierze adres IP ze skonfigurowanej podsieci i skieruje ruch sieciowy do adresów IP w puli adresów IP zaplecza. Pamiętaj, że każde wystąpienie będzie mieć jeden adres IP.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Krok 2
Skonfiguruj pulę adresów IP zaplecza o nazwie „pool01” z adresami IP „134.170.185.46, 134.170.188.221,134.170.185.50”. Będą to adresy IP odbierające ruch sieciowy pochodzący z punktu końcowego adresu IP frontonu. Powyższe adresy IP można zastąpić własnymi dodawanymi punktami końcowymi adresów IP aplikacji.

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
```

### <a name="step-3"></a>Krok 3
Skonfiguruj ustawienia bramy aplikacji „poolsetting01” dla ruchu sieciowego ze zrównoważonym obciążeniem w puli zaplecza.

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

### <a name="step-4"></a>Krok 4
Skonfiguruj port adresu IP frontonu o nazwie „frontendport01” dla punktu końcowego publicznego adresu IP.

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

### <a name="step-5"></a>Krok 5
Utwórz konfigurację adresów IP frontonu o nazwie „fipconfig01” i skojarz publiczny adres IP z konfiguracją adresów IP frontonu.

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

### <a name="step-6"></a>Krok 6
Utwórz odbiornik o nazwie „listener01” i skojarz port frontonu z konfiguracją adresów IP frontonu.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

### <a name="step-7"></a>Krok 7
Utwórz regułę routingu modułu równoważenia obciążenia o nazwie „rule01”, określającą zachowanie modułu równoważenia obciążenia.

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-8"></a>Krok 8
Skonfiguruj rozmiar wystąpienia bramy aplikacji.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> Wartość domyślna parametru *InstanceCount* to 2, a wartość maksymalna — 10. Wartość domyślna parametru *GatewaySize* to Medium. Możesz wybrać następujące wartości: Standard_Small, Standard_Medium i Standard_Large.
> 
> 

## <a name="create-an-application-gateway-by-using-newazurermapplicationgateway"></a>Tworzenie bramy aplikacji przy użyciu polecenia New-AzureRmApplicationGateway
Utwórz bramę aplikacji przy użyciu wszystkich elementów konfiguracji z powyższych kroków. W tym przykładzie brama aplikacji ma nazwę „appgwtest”.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

### <a name="step-9"></a>Krok 9
Pobierz szczegóły adresów DNS i VIP bramy aplikacji z zasobu publicznego adresu IP zasobu dołączonego do bramy aplikacji.

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  
```

## <a name="delete-an-application-gateway"></a>Usuwanie bramy aplikacji
Aby usunąć bramę aplikacji, wykonaj następujące kroki:

### <a name="step-1"></a>Krok 1
Pobierz obiekt bramy aplikacji i skojarz go ze zmienną „$getgw”.

```powershell
$getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a>Krok 2
Użyj polecenia cmdlet **Stop-AzureRmApplicationGateway**, aby zatrzymać bramę aplikacji.

```powershell
Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  
```

Gdy brama aplikacji jest w stanie zatrzymania, użyj polecenia cmdlet **Remove-AzureRmApplicationGateway**, aby usunąć usługę.

```powershell
Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force
```

> [!NOTE]
> Przełącznik **-force** umożliwia pomijanie komunikatu potwierdzającego usunięcie.
> 
> 

Aby sprawdzić, czy usługa została usunięta, możesz użyć polecenia cmdlet **Get-AzureRmApplicationGateway**. Ten krok nie jest wymagany.

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

## <a name="get-application-gateway-dns-name"></a>Pobieranie nazwy DNS bramy aplikacji
Po utworzeniu bramy następnym krokiem jest skonfigurowanie frontonu na potrzeby komunikacji. Gdy jest używany publiczny adres IP, brama aplikacji wymaga dynamicznie przypisywanej nazwy DNS, która nie jest przyjazna. Aby upewnić się, że użytkownicy końcowi mogą trafić bramę aplikacji, można użyć rekordu CNAME w celu wskazania publicznego punktu końcowego bramy aplikacji. [Konfigurowanie niestandardowej nazwy domeny dla platformy Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Aby to zrobić, pobierz szczegóły bramy aplikacji i skojarzony adres IP oraz nazwę DNS, używając elementu PublicIPAddress dołączonego do bramy aplikacji. Nazwa DNS bramy aplikacji powinna zostać użyta w celu utworzenia rekordu CNAME, który wskazuje dwóm aplikacjom sieci Web tę nazwę DNS. Korzystanie z rekordów A nie jest zalecane, ponieważ adres VIP może ulec zmianie po ponownym uruchomieniu bramy aplikacji.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a>Następne kroki
Jeśli chcesz skonfigurować odciążanie protokołu SSL, zobacz artykuł [Configure an application gateway for SSL offload](application-gateway-ssl.md) (Konfigurowanie bramy aplikacji na potrzeby odciążania protokołu SSL).

Jeśli chcesz skonfigurować bramę aplikacji do użycia z wewnętrznym modułem równoważenia obciążenia, zobacz artykuł [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md) (Tworzenie bramy aplikacji przy użyciu wewnętrznego modułu równoważenia obciążenia).

Więcej ogólnych informacji na temat opcji równoważenia obciążenia możesz znaleźć w następujących artykułach:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)




<!--HONumber=Nov16_HO2-->


