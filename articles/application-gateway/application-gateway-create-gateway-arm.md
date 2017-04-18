---
title: "Tworzenie usługi Azure Application Gateway i zarządzanie nią — PowerShell | Microsoft Docs"
description: "Ta strona zawiera instrukcje dotyczące tworzenia, konfigurowania, uruchamiania i usuwania bramy aplikacji platformy Azure za pomocą usługi Azure Resource Manager"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 866e9b5f-0222-4b6a-a95f-77bc3d31d17b
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 73ee330c276263a21931a7b9a16cc33f86c58a26
ms.openlocfilehash: 11ecfc993f17c89d4ac4431e9a835000d30afe76
ms.lasthandoff: 04/05/2017


---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Tworzenie, uruchamianie lub usuwanie bramy aplikacji przy użyciu usługi Azure Resource Manager

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager — program PowerShell](application-gateway-create-gateway-arm.md)
> * [Klasyczny portal Azure — program PowerShell](application-gateway-create-gateway.md)
> * [Szablon usługi Azure Resource Manager](application-gateway-create-gateway-arm-template.md)
> * [Interfejs wiersza polecenia platformy Azure](application-gateway-create-gateway-cli.md)

Usługa Azure Application Gateway to moduł równoważenia obciążenia warstwy 7. Udostępnia tryb failover, oparty na wydajności routing żądań HTTP między różnymi serwerami — w chmurze i lokalnymi.
Usługa Application Gateway zapewnia wiele funkcji kontrolera dostarczania aplikacji (ADC, Application Delivery Controller), w tym między innymi równoważenie obciążenia HTTP, koligację sesji na podstawie plików cookie, odciążanie protokołu Secure Sockets Layer (SSL), niestandardowe sondy kondycji i obsługę wielu witryn.

Aby uzyskać pełną listę obsługiwanych funkcji, odwiedź stronę [Application Gateway — omówienie](application-gateway-introduction.md)

W tym artykule przedstawiono kroki umożliwiające tworzenie, konfigurowanie, uruchamianie i usuwanie bramy aplikacji.

> [!IMPORTANT]
> Przed rozpoczęciem pracy z zasobami platformy Azure należy pamiętać, że ma ona obecnie dwa modele wdrażania: za pomocą usługi Resource Manager i model klasyczny. Przed rozpoczęciem pracy z dowolnym zasobem platformy Azure należy zapoznać się z [modelami i narzędziami wdrażania](../azure-classic-rm.md). Dokumentację dotyczącą różnych narzędzi można wyświetlić, klikając karty w górnej części artykułu. W tym dokumencie opisano tworzenie bramy aplikacji przy użyciu usługi Azure Resource Manager. Aby korzystać z modelu klasycznego, przejdź do artykułu [Create an application gateway classic deployment by using PowerShell](application-gateway-create-gateway.md) (Tworzenie klasycznego wdrożenia bramy aplikacji przy użyciu programu PowerShell).

## <a name="before-you-begin"></a>Przed rozpoczęciem

1. Zainstaluj najnowszą wersję poleceń cmdlet programu Azure PowerShell za pomocą Instalatora platformy sieci Web. Najnowszą wersję można pobrać i zainstalować z sekcji **Windows PowerShell** strony [Pliki do pobrania](https://azure.microsoft.com/downloads/).
1. Jeśli masz istniejącą sieć wirtualną, wybierz istniejącą pustą podsieć lub utwórz podsieć w istniejącej sieci wirtualnej wyłącznie do użytku przez tę bramę aplikacji. Nie można wdrożyć bramy aplikacji do innej sieci wirtualnej niż sieć wirtualna zasobów, które zamierzasz wdrożyć za bramą aplikacji.
1. Serwery konfigurowane do używania bramy aplikacji muszą być umieszczone w sieci wirtualnej lub z przypisanym adresem IP/VIP lub mieć w niej utworzone punkty końcowe.

## <a name="what-is-required-to-create-an-application-gateway"></a>Co jest wymagane do utworzenia bramy aplikacji?

* **Pula serwerów zaplecza:** lista adresów IP, nazw FQDN i kart sieciowych serwerów zaplecza. Jeśli używane są adresy IP, powinny albo należeć do podsieci sieci wirtualnej, albo być publicznymi adresami IP bądź adresami VIP.
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

W powyższym przykładzie utworzyliśmy grupę zasobów o nazwie **appgw-RG** i lokalizacji **Zachodnie stany USA**.

> [!NOTE]
> Jeśli musisz skonfigurować niestandardową sondę bramy aplikacji, odwiedź: [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md) (Tworzenie bramy aplikacji z sondami niestandardowymi przy użyciu programu PowerShell). Aby dowiedzieć się więcej, zapoznaj się z informacjami na temat [sond niestandardowych i monitorowania kondycji](application-gateway-probe-overview.md).

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Tworzenie sieci wirtualnej i podsieci dla bramy aplikacji

W poniższym przykładzie pokazano, jak utworzyć sieć wirtualną przy użyciu usługi Resource Manager. W tym przykładzie tworzona jest sieć wirtualna dla usługi Application Gateway. Usługa Application Gateway wymaga własnej podsieci, z tego powodu podsieć tworzona dla usługi Application Gateway jest mniejsza niż przestrzeń adresowa sieci wirtualnej. Użycie mniejszych podsieci pozwala, aby inne zasoby, w tym, ale nie tylko, serwery sieci Web, były skonfigurowane w tej samej sieci wirtualnej.

### <a name="step-1"></a>Krok 1

Przypisz zakres adresów 10.0.0.0/24 do zmiennej podsieci służącej do tworzenia sieci wirtualnej. W tym kroku tworzony jest obiekt konfiguracji podsieci dla usługi Application Gateway, który jest używany w kolejnym przykładzie.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Krok 2

Utwórz sieć wirtualną o nazwie **appgwvnet** w grupie zasobów **appgw-rg** dla regionu Zachodnie stany USA przy użyciu prefiksu 10.0.0.0/16 i podsieci 10.0.0.0/24. W tym kroku kończona jest konfiguracja sieci wirtualnej z pojedynczą podsiecią, w której będzie znajdować się usługa Application Gateway.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Krok 3

Przypisz zmienną podsieci dla następnych kroków; ta zmienna jest przekazywana do polecenia cmdlet `New-AzureRMApplicationGateway` w przyszłym kroku.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Tworzenie publicznego adresu IP dla konfiguracji frontonu

Utwórz zasób publicznego adresu IP **publicIP01** w grupie zasobów **appgw-rg** dla regionu Zachodnie stany USA. Do odbierania żądań zrównoważenia obciążenia usługa Application Gateway może korzystać z publicznego adresu IP, wewnętrznego adresu IP lub obu z nich.  W tym przykładzie używany jest tylko publiczny adres IP. W poniższym przykładzie nie jest konfigurowana nazwa DNS na potrzeby utworzenia publicznego adresu IP.  Usługa Application Gateway nie obsługuje niestandardowych nazw DNS dla publicznych adresów IP.  Jeżeli dla publicznego punktu końcowego wymagana jest nazwa niestandardowa, należy utworzyć rekord CNAME wskazujący automatycznie wygenerowaną nazwę DNS dla publicznego adresu IP.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

> [!NOTE]
> Adres IP jest przypisywany do bramy aplikacji w chwili uruchamiania usługi.

## <a name="create-the-application-gateway-configuration-objects"></a>Tworzenie obiektów konfiguracji bramy aplikacji

Wszystkie elementy konfiguracji muszą zostać skonfigurowane przed utworzeniem bramy aplikacji. Poniższe kroki umożliwiają utworzenie elementów konfiguracji wymaganych w przypadku zasobu bramy aplikacji.

### <a name="step-1"></a>Krok 1

Utwórz konfigurację adresu IP bramy aplikacji o nazwie **gatewayIP01**. Uruchomiona usługa Application Gateway wybierze adres IP ze skonfigurowanej podsieci i skieruje ruch sieciowy do adresów IP w puli adresów IP zaplecza. Pamiętaj, że każde wystąpienie będzie mieć jeden adres IP.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Krok 2

Skonfiguruj pulę adresów IP zaplecza o nazwie **pool01** za pomocą adresów IP dla puli **pool1**. Te adresy IP są adresami IP zasobów hostujących aplikację sieci Web, która ma być chroniona przez bramę aplikacji. Kondycja tych wszystkich elementów członkowskich zaplecza jest weryfikowana przez sondy (podstawowe lub niestandardowe).  Następnie jest do nich kierowany ruch, gdy nadchodzą żądania do bramy aplikacji. Pule zaplecza mogą być używane przez wiele reguł w ramach bramy aplikacji, co oznacza, że jednej puli zaplecza można używać dla wielu aplikacji sieci Web znajdujących się na tym samym hoście.

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50
```

W tym przykładzie istnieją dwie pule zaplecza do kierowania ruchu sieciowego na podstawie ścieżki adresu URL. Jedna pula odbiera ruch ze ścieżki adresu URL „/video”, a druga pula odbiera ruch ze ścieżki „/image”. Zastąp powyższe adresy IP, aby dodać własne punkty końcowe adresów IP aplikacji.

### <a name="step-3"></a>Krok 3

Skonfiguruj ustawienia bramy aplikacji **poolsetting01** dla ruchu sieciowego ze zrównoważonym obciążeniem w puli zaplecza. Każda pula zaplecza może mieć własne ustawienia puli zaplecza.  Ustawienia HTTP zaplecza są używane przez reguły do kierowania ruchu do właściwych elementów członkowskich puli zaplecza. Ustawienia HTTP zaplecza określają protokół i port, które są używane podczas wysyłania ruchu do elementów członkowskich puli zaplecza. Sesje bazujące na plikach cookie są też określane przez ustawienia HTTP zaplecza.  Jeśli koligacja sesji bazujących na plikach cookie jest włączona, wysyła ruch do tego samego zaplecza co poprzednie żądania dla każdego pakietu.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
```

### <a name="step-4"></a>Krok 4

Skonfiguruj port frontonu dla bramy aplikacji. Obiekt konfiguracji portu frontonu jest używany przez odbiornik do definiowania, który port usługi Application Gateway nasłuchuje ruchu na odbiorniku.

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

### <a name="step-5"></a>Krok 5

Skonfiguruj adres IP frontonu z punktem końcowym publicznego adresu IP. Obiekt konfiguracji adresu IP frontonu jest używany przez odbiornik do powiązywania widocznego z zewnątrz adresu IP z odbiornikiem.

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

### <a name="step-6"></a>Krok 6

Skonfiguruj odbiornik. W tym kroku dla odbiornika konfigurowany jest publiczny adres IP i port używane do odbierania przychodzącego ruchu sieciowego. W następującym przykładzie odbiornik konfigurowany jest na podstawie wcześniej określonej konfiguracji adresu IP frontonu, konfiguracji portu frontonu i protokołu (http lub https). W tym przykładzie odbiornik nasłuchuje ruchu HTTP na porcie 80 publicznego adresu IP, który został utworzony wcześniej.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

### <a name="step-7"></a>Krok 7

Utwórz regułę routingu modułu równoważenia obciążenia o nazwie **rule01**, określającą zachowanie modułu równoważenia obciążenia. Ustawienia puli zaplecza, odbiornik i pula zaplecza utworzone we wcześniejszych krokach składają się na regułę. Zgodnie ze zdefiniowanymi kryteriami ruch jest kierowany do odpowiedniego zaplecza.

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting01 -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-8"></a>Krok 8

Skonfiguruj liczbę wystąpień i rozmiar bramy aplikacji.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> Wartość domyślna parametru **InstanceCount** to 2, a wartość maksymalna — 10. Wartość domyślna parametru **GatewaySize** to Medium (Średnia). Możesz wybrać następujące wartości: **Standard_Small**, **Standard_Medium** i **Standard_Large**.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Tworzenie bramy aplikacji przy użyciu polecenia New-AzureRmApplicationGateway

Utwórz bramę aplikacji przy użyciu wszystkich elementów konfiguracji z powyższych kroków. W tym przykładzie brama aplikacji ma nazwę **appgwtest**.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

Pobierz szczegóły adresów DNS i VIP bramy aplikacji z zasobu publicznego adresu IP zasobu dołączonego do bramy aplikacji.

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  
```

## <a name="delete-an-application-gateway"></a>Usuwanie bramy aplikacji

Aby usunąć bramę aplikacji, wykonaj następujące kroki:

### <a name="step-1"></a>Krok 1

Pobierz obiekt bramy aplikacji i skojarz go ze zmienną `$getgw`.

```powershell
$getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a>Krok 2

Użyj polecenia `Stop-AzureRmApplicationGateway`, aby zatrzymać bramę aplikacji.

```powershell
Stop-AzureRmApplicationGateway -ApplicationGateway $getgw
```

Po zatrzymaniu bramy aplikacji użyj polecenia cmdlet `Remove-AzureRmApplicationGateway`, aby usunąć usługę.

```powershell
Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force
```

> [!NOTE]
> Przełącznik **-force** umożliwia pomijanie komunikatu potwierdzającego usunięcie.

Aby sprawdzić, czy usługa została usunięta, możesz użyć polecenia cmdlet `Get-AzureRmApplicationGateway`. Ten krok nie jest wymagany.

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

## <a name="get-application-gateway-dns-name"></a>Pobieranie nazwy DNS bramy aplikacji

Po utworzeniu bramy następnym krokiem jest skonfigurowanie frontonu na potrzeby komunikacji. Gdy jest używany publiczny adres IP, brama aplikacji wymaga dynamicznie przypisywanej nazwy DNS, która nie jest przyjazna. Aby upewnić się, że użytkownicy końcowi mogą trafić bramę aplikacji, można użyć rekordu CNAME w celu wskazania publicznego punktu końcowego bramy aplikacji. [Konfigurowanie niestandardowej nazwy domeny dla platformy Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Aby znaleźć dynamicznie utworzoną nazwę DNS, pobierz szczegóły bramy aplikacji i skojarzony adres IP oraz nazwę DNS, używając elementu PublicIPAddress dołączonego do bramy aplikacji. Nazwa DNS bramy aplikacji powinna zostać użyta w celu utworzenia rekordu CNAME, który wskazuje dwóm aplikacjom sieci Web tę nazwę DNS. Korzystanie z rekordów A nie jest zalecane, ponieważ adres VIP może ulec zmianie po ponownym uruchomieniu bramy aplikacji.

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

Jeśli chcesz skonfigurować odciążanie protokołu SSL, odwiedź: [Configure an application gateway for SSL offload](application-gateway-ssl.md) (Konfigurowanie bramy aplikacji na potrzeby odciążania protokołu SSL).

Jeśli chcesz skonfigurować bramę aplikacji do użycia z wewnętrznym modułem równoważenia obciążenia, odwiedź: [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md) (Tworzenie bramy aplikacji przy użyciu wewnętrznego modułu równoważenia obciążenia).

Więcej ogólnych informacji na temat opcji równoważenia obciążenia możesz znaleźć, odwiedzając:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)


