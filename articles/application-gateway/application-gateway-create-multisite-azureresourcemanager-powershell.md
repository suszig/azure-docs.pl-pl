---
title: "Utwórz bramę aplikacji dla wielu witryn | Dokumentacja firmy Microsoft"
description: "Ta strona zawiera instrukcje dotyczące tworzenia, konfigurowania bramy aplikacji Azure obsługi wielu aplikacji sieci web na tej samej bramy."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: b107d647-c9be-499f-8b55-809c4310c783
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2016
ms.author: amsriva
ms.openlocfilehash: d42efa7d359f5c87c14afbfd138328b37c8ae6c2
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="create-an-application-gateway-for-hosting-multiple-web-applications"></a>Utwórz bramę aplikacji do obsługi wielu aplikacji sieci web

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-create-multisite-portal.md)
> * [Azure Resource Manager — program PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Obsługujący wiele lokacji umożliwia wdrożenie więcej niż jednej aplikacji sieci web na tej samej bramy aplikacji. Opiera się na obecność nagłówek hosta w przychodzące żądanie HTTP, aby określić, które odbiornika będzie odbierać dane. Odbiornik następnie kieruje ruch do puli zaplecza odpowiednie zgodnie z konfiguracją w definicji reguły bramy. Brama aplikacji w aplikacji sieci web z włączonym protokołem SSL, opiera się rozszerzenia oznaczenia nazwy serwera (SNI), aby wybrać poprawny odbiornika dla ruchu w sieci web. Zazwyczaj do obsługi wielu lokacji jest używane w celu zrównoważenia obciążenia żądaniami dla domen z innej witryny sieci web do innego serwera zaplecza pul. Podobnie wielu domen podrzędnych tej samej domeny katalogu głównego może być hostowana na tę samą bramę aplikacji.

## <a name="scenario"></a>Scenariusz

W poniższym przykładzie brama aplikacji jest obsługę ruchu dla domeny contoso.com i fabrikam.com z dwóch pul serwerów zaplecza: contoso puli serwerów i puli serwerów firmy fabrikam. Podobnych konfiguracji może posłużyć do hosta poddomen, takich jak app.contoso.com i blog.contoso.com.

![imageURLroute](./media/application-gateway-create-multisite-azureresourcemanager-powershell/multisite.png)

## <a name="before-you-begin"></a>Przed rozpoczęciem

1. Zainstaluj najnowszą wersję poleceń cmdlet programu Azure PowerShell za pomocą Instalatora platformy sieci Web. Najnowszą wersję można pobrać i zainstalować z sekcji **Windows PowerShell** strony [Pliki do pobrania](https://azure.microsoft.com/downloads/).
2. Musi istnieć serwerów dodanych do puli zaplecza, aby użyć bramy aplikacji lub ich punkty końcowe utworzone w sieci wirtualnej w osobnej podsieci lub publicznego adresu IP/VIP przypisane.

## <a name="requirements"></a>Wymagania

* **Pula serwerów zaplecza:** lista adresów IP serwerów zaplecza. Adresy IP na liście powinny należeć do podsieci sieci wirtualnej lub być publicznymi bądź wirtualnymi adresami IP. Można także nazwę FQDN.
* **Ustawienia puli serwerów zaplecza:** każda pula ma ustawienia, takie jak port, protokół i koligacja oparta na plikach cookie. Te ustawienia są powiązane z pulą i są stosowane do wszystkich serwerów w tej puli.
* **Port frontonu:** port publiczny, który jest otwierany w bramie aplikacji. Ruch trafia do tego portu, a następnie jest przekierowywany do jednego z serwerów zaplecza.
* **Odbiornik:** odbiornik ma port frontonu, protokół (Http lub Https, z uwzględnieniem wielkości liter) oraz nazwę certyfikatu SSL (w przypadku konfigurowania odciążania protokołu SSL). Dla bramy aplikacji obsługującej obejmujący wiele lokacji nazwy hosta i wskaźniki SNI są również został dodany.
* **Reguła:** reguły wiąże odbiornika puli serwera zaplecza i definiuje puli serwera zaplecza, których ruch powinny być kierowane do, gdy trafienia w szczególności odbiornika. Reguły są przetwarzane w kolejności, w jakiej występują, a ruch zostanie skierowany przez pierwszą regułę odpowiadającą niezależnie od szczegółowością. Na przykład jeśli utworzono regułę przy użyciu odbiornika podstawowe i regułę przy użyciu odbiornika obejmujący wiele lokacji zarówno w tym samym porcie, reguły z wieloma lokacjami odbiornika musi być wymieniona przed regułę przy użyciu podstawowego odbiornika w kolejności reguły obejmujący wiele lokacji, aby działać zgodnie z oczekiwaniami.

## <a name="create-an-application-gateway"></a>Tworzenie bramy aplikacji

Poniżej przedstawiono kroki potrzebne do utworzenia bramy aplikacji:

1. Utworzenie grupy zasobów dla usługi Resource Manager.
2. Utwórz sieć wirtualną, podsieci i publicznego adresu IP dla bramy aplikacji.
3. Utworzenie obiektu konfiguracji bramy aplikacji.
4. Utworzenie zasobu bramy aplikacji.

## <a name="create-a-resource-group-for-resource-manager"></a>Tworzenie grupy zasobów dla usługi Resource Manager

Upewnij się, że używasz najnowszej wersji programu Azure PowerShell. Więcej informacji znajduje się w temacie [przy użyciu programu Windows PowerShell z usługą Resource Manager](../powershell-azure-resource-manager.md).

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
New-AzureRmResourceGroup -Name appgw-RG -location "West US"
```

Alternatywnie można również utworzyć tagi dla grupy zasobów dla bramy aplikacji:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"}
```

Usługa Azure Resource Manager wymaga, aby wszystkie grupy zasobów określały lokalizację. Ta lokalizacja będzie używana jako domyślna lokalizacja dla zasobów w danej grupie zasobów. Upewnij się, że wszystkie polecenia, aby utworzyć bramę aplikacji używać tej samej grupie zasobów.

W powyższym przykładzie utworzono grupę zasobów o nazwie **zarządcy zasobów appgw** z lokalizacji **zachodnie stany USA**.

> [!NOTE]
> Jeśli musisz skonfigurować niestandardową sondę bramy aplikacji, zobacz artykuł [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md) (Tworzenie bramy aplikacji z sondami niestandardowymi przy użyciu programu PowerShell). Odwiedź stronę [monitorowanie kondycji i badania niestandardowych](application-gateway-probe-overview.md) Aby uzyskać więcej informacji.

## <a name="create-a-virtual-network-and-subnets"></a>Tworzenie sieci wirtualnej i podsieci

W poniższym przykładzie pokazano, jak utworzyć sieć wirtualną przy użyciu usługi Resource Manager. Dwie podsieci są tworzone w tym kroku. Jest pierwszą podsieć dla bramy aplikacji. Brama aplikacji wymaga jego własnej podsieci, aby pomieścić jego wystąpienia. Inne bramy aplikacji można wdrożyć w tej podsieci. Drugi podsieci służy do przechowywania serwerów wewnętrznej bazy danych aplikacji.

### <a name="step-1"></a>Krok 1

Przypisz zakresu adresów 10.0.0.0/24 do zmiennej podsieci ma być używany do przechowywania bramy aplikacji.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -AddressPrefix 10.0.0.0/24
```
### <a name="step-2"></a>Krok 2

Przypisz 10.0.1.0/24 zakres adresów do zmiennej podsieć2 służący do pul zaplecza.

```powershell
$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -AddressPrefix 10.0.1.0/24
```

### <a name="step-3"></a>Krok 3

Tworzenie sieci wirtualnej o nazwie **appgwvnet** w grupie zasobów **zarządcy zasobów appgw** dla regionu zachodnie stany USA, przy użyciu 10.0.0.0/16 prefiks z podsieci 10.0.0.0/24, a 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet,$subnet2
```

### <a name="step-4"></a>Krok 4

Przypisanie zmiennej podsieci dalsze czynności, które tworzy bramę aplikacji.

```powershell
$appgatewaysubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -VirtualNetwork $vnet
$backendsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Tworzenie publicznego adresu IP dla konfiguracji frontonu

Utwórz zasób publicznego adresu IP **publicIP01** w grupie zasobów **appgw-rg** dla regionu Zachodnie stany USA.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Adres IP jest przypisywany do bramy aplikacji w chwili uruchamiania usługi.

## <a name="create-application-gateway-configuration"></a>Tworzenie konfiguracji bramy aplikacji

Musisz skonfigurować wszystkie elementy konfiguracji przed utworzeniem bramy aplikacji. Poniższe kroki umożliwiają utworzenie elementów konfiguracji wymaganych w przypadku zasobu bramy aplikacji.

### <a name="step-1"></a>Krok 1

Utwórz konfigurację adresu IP bramy aplikacji o nazwie **gatewayIP01**. Po uruchomieniu aplikacji bramy przejmuje adresu IP z podsieci skonfigurowane i kierować ruchem sieciowym na adresy IP w puli adresów IP zaplecza. Pamiętaj, że każde wystąpienie będzie mieć jeden adres IP.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnet
```

### <a name="step-2"></a>Krok 2

Skonfiguruj pulę adresów IP zaplecza o nazwie **pool01** i **pool2** z adresami IP **134.170.185.46**, **134.170.188.221**, **134.170.185.50** dla **pool1** i **134.170.186.46**, **134.170.189.221**, **134.170.186.50** dla **pool2**.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.1.100, 10.0.1.101, 10.0.1.102
$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.1.103, 10.0.1.104, 10.0.1.105
```

W tym przykładzie istnieją dwie pule zaplecza, można kierować ruchem sieciowym w oparciu o żądanej witryny. Jedna pula odbiera ruch z lokacji "contoso.com" i innych puli odbiera ruch z lokacji "fabrikam.com". Należy zastąpić poprzedniego adresy IP, aby dodać własne punkty końcowe adresu IP aplikacji. Zamiast adresów IP można również użyć publiczne adresy IP, nazwy FQDN lub wirtualna karta sieciowa dla wystąpień wewnętrznej bazy danych. Aby określić nazwy FQDN, zamiast adresów IP w programie PowerShell Użyj "-BackendFQDNs" parametru.

### <a name="step-3"></a>Krok 3

Skonfiguruj ustawienia bramy aplikacji **poolsetting01** i **poolsetting02** dla ruchu sieciowego z równoważeniem obciążenia w puli zaplecza. W tym przykładzie możesz skonfigurować ustawienia innej puli zaplecza dla pul zaplecza. Każda pula zaplecza może mieć własne ustawienia puli zaplecza.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>Krok 4

Skonfiguruj adres IP frontonu z punktem końcowym publicznego adresu IP.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>Krok 5

Skonfiguruj port frontonu dla bramy aplikacji.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443
```

### <a name="step-6"></a>Krok 6

Skonfiguruj dwa certyfikaty SSL dwie witryny sieci Web w się, że firma Microsoft będzie obsługiwany w tym przykładzie. Jeden certyfikat jest przeznaczona ruchu contoso.com i drugą dla ruchu fabrikam.com. Te certyfikaty należy wystawił certyfikaty dla witryny sieci Web urzędu certyfikacji. Certyfikaty z podpisem własnym są obsługiwane, ale nie jest zalecane dla ruchu w środowisku produkcyjnym.

```powershell
$cert01 = New-AzureRmApplicationGatewaySslCertificate -Name contosocert -CertificateFile <file path> -Password <password>
$cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>
```

### <a name="step-7"></a>Krok 7

W tym przykładzie, należy skonfigurować dwa odbiorniki dla dwóch witryn sieci web. Ten krok obejmuje skonfigurowanie odbiorników dla publicznego adresu IP, portu i hosta używany do odbierania ruchu przychodzącego. Parametr Nazwa hosta jest wymagana do obsługi wielu lokacji i należy ustawić odpowiednie witryny sieci Web, dla którego zostanie odebrany ruch. Element RequireServerNameIndication parametr powinien być ustawiony na wartość true dla witryn sieci Web, że wymagana jest obsługa protokołu SSL w przypadku wielu hostów. Jeśli wymagana jest obsługa protokołu SSL, należy określić certyfikat SSL, który służy do zabezpieczania ruchu dla tej aplikacji sieci web. Kombinacja konfiguracji IP frontonu, elementu FrontendPort oraz nazwa hosta musi być unikatowy w odbiorniku. Każdy odbiornik może obsługiwać jeden certyfikat.

```powershell
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02
```

### <a name="step-8"></a>Krok 8

Utwórz dwa ustawienia reguł dla aplikacji sieci web dwóch w tym przykładzie. Reguła wiąże ze sobą odbiorników, pul zaplecza i ustawienia protokołu http. Ten krok obejmuje skonfigurowanie bramy aplikacji należy używać podstawowe reguły routingu, jeden dla każdej witryny sieci Web. Ruch do każdej witryny sieci Web jest odbierany przez jego skonfigurowany odbiornik i jest następnie przekierowywane do jego puli zaplecza skonfigurowane przy użyciu właściwości określony w elementu BackendHttpSettings.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2
```

### <a name="step-9"></a>Krok 9

Skonfiguruj liczbę wystąpień i rozmiar bramy aplikacji.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a>Utwórz bramę aplikacji

Utwórz bramę aplikacji ze wszystkimi obiektami konfiguracji z powyższych kroków.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02
```

> [!IMPORTANT]
> Inicjowanie obsługi bramy aplikacji jest operacją wymagającą dużo czasu i może zająć trochę czasu.
> 
> 

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

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się, jak chronić witryny sieci Web z [Application Gateway - zapory aplikacji sieci Web](application-gateway-webapplicationfirewall-overview.md)

