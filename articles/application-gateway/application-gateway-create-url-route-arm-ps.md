---
title: "Tworzenie bramy aplikacji przy użyciu reguł routingu adresów URL | Dokumentacja firmy Microsoft"
description: "Ta strona zawiera instrukcje dotyczące tworzenia i skonfiguruj bramę aplikacji Azure przy użyciu reguł routingu adresów URL."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: d141cfbb-320a-4fc9-9125-10001c6fa4cf
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: davidmu
ms.openlocfilehash: f2797864d7f0bda35d4d84ee78b157879451f889
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-application-gateway-by-using-path-based-routing"></a>Utwórz bramę aplikacji przy użyciu routingu opartego na ścieżce

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager — program PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](application-gateway-create-url-route-cli.md)

Na podstawie ścieżki routingu kojarzy tras na podstawie ścieżki adresu URL żądania HTTP. Sprawdza, czy istnieje trasa do puli zaplecza skonfigurowane dla adresu URL, w bramie aplikacji, a następnie wysyła ruchu sieciowego do określonych puli zaplecza. Użycia routingu opartego na adres URL jest w celu zrównoważenia obciążenia żądaniami dla różnych typów zawartości do innego serwera zaplecza pul.

Brama aplikacji Azure ma dwa typy zasad: podstawowe i na podstawie ścieżki routingu. Basic udostępnia usługę okrężnego dla pul zaplecza. Wzorzec ścieżki adresu URL żądania routingu opartego na ścieżkę, oprócz rozdzielanie, używa również wybrać puli zaplecza.

## <a name="scenario"></a>Scenariusz

W poniższym przykładzie brama aplikacji w pełni ruchu dla domeny contoso.com dwie pule serwera zaplecza: pulę serwerów wideo i puli serwerów obrazu.

Żądania dla http://contoso.com/image * są kierowane do puli serwerów obrazu (**pool1**), a żądania dla http://contoso.com/video * są kierowane do puli serwerów wideo (**pool2**). Jeśli żadna z wzorców ścieżki nie zgadza, domyślna pula serwera (**pool1**) jest zaznaczone.

![trasy adresu URL](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a>Przed rozpoczęciem

1. Zainstaluj najnowszą wersję poleceń cmdlet programu Azure PowerShell za pomocą Instalatora platformy sieci Web. Najnowszą wersję można pobrać i zainstalować z sekcji **Windows PowerShell** strony [Pliki do pobrania](https://azure.microsoft.com/downloads/).
2. Utwórz sieć wirtualną i podsieć dla bramy aplikacji. Upewnij się, że nie maszyny wirtualne lub wdrożenia chmury używać tej podsieci. Brama aplikacji musi znajdować się w podsieci sieci wirtualnej.
3. Upewnij się, że istnieje serwerów dodanych do puli zaplecza bramy aplikacji, lub że ich punkty końcowe utworzone w sieci wirtualnej lub z publiczną przypisanych adresów IP i VIP.

## <a name="requirements-to-create-an-application-gateway"></a>Wymagania, aby utworzyć bramę aplikacji

* **Pula serwerów zaplecza**: Lista adresów IP serwerów zaplecza. Na liście adresów IP powinien albo należy do podsieci sieci wirtualnej, albo publicznego adresu IP/VIP.
* **Ustawienia puli serwera zaplecza**: port, protokół i koligacji na podstawie plików cookie. Są one powiązane z pulą i zastosowane do wszystkich serwerów w puli.
* **Port frontonu**: port publiczny, który jest otwarty na bramie aplikacji. Ruch trafienia tego portu, a następnie przekierowuje do jednego z serwerów zaplecza.
* **Odbiornik**: odbiornika ma portów frontonu, protokół (Http lub Https, który jest rozróżniana wielkość liter) i nazwę certyfikatu SSL (jeśli odciążania Konfigurowanie protokołu SSL).
* **Reguła**: reguła wiąże odbiornika i puli serwerów zaplecza i definiuje do puli, które powinny być kierowane go trafienia odbiornik ruchu.

## <a name="create-an-application-gateway"></a>Tworzenie bramy aplikacji

Różnica między przy użyciu klasycznego portalu Azure i usługi Azure Resource Manager jest kolejność, w którym utworzyć bramę aplikacji i elementy, które trzeba skonfigurować.

W usłudze Resource Manager wszystkie elementy składające się na bramę aplikacji są konfigurowane osobno, a następnie składane w celu utworzenia zasobu bramy aplikacji.

Wykonaj następujące kroki, aby utworzyć bramę aplikacji:

1. Utworzenie grupy zasobów dla usługi Resource Manager.
2. Utworzenie sieci wirtualnej, podsieci i publicznego adresu IP bramy aplikacji.
3. Utworzenie obiektu konfiguracji bramy aplikacji.
4. Utworzenie zasobu bramy aplikacji.

## <a name="create-a-resource-group-for-resource-manager"></a>Tworzenie grupy zasobów dla usługi Resource Manager

Upewnij się, że używasz najnowszej wersji programu Azure PowerShell. Znajdź więcej informacji na [przy użyciu programu Windows PowerShell z usługą Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

Logowanie do platformy Azure.

```powershell
Login-AzureRmAccount
```

Zostanie wyświetlony monit o uwierzytelniania przy użyciu poświadczeń.<BR>

### <a name="step-2"></a>Krok 2

Sprawdź subskrypcje dostępne na koncie.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>Krok 3

Wybierz subskrypcję platformy Azure do użycia. <BR>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Krok 4

Utwórz grupę zasobów. (Pomiń ten krok, jeśli używasz istniejącej grupy zasobów).

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US"
```

Alternatywnie można tworzyć tagi dla grupy zasobów dla bramy aplikacji:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 
```

Usługa Azure Resource Manager wymaga, aby grupy zasobów określały domyślnej lokalizacji, która jest stosowana do wszystkich zasobów w tej grupie. Upewnij się, że wszystkie polecenia, aby utworzyć bramę aplikacji używać tej samej grupie zasobów.

W poprzednim przykładzie możemy utworzyć grupę zasobów o nazwie "zarządcy zasobów appgw" i użyć lokalizacji "Zachodnie stany USA".

> [!NOTE]
> Jeśli trzeba skonfigurować niestandardowe sondowania dla bramy aplikacji, przejdź do [Utwórz bramę aplikacji z niestandardowego sond przy użyciu programu PowerShell](application-gateway-create-probe-ps.md). Zobacz [ omówienie monitorowania kondycji aplikacji bramy](application-gateway-probe-overview.md) Aby uzyskać więcej informacji.
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Tworzenie sieci wirtualnej i podsieci dla bramy aplikacji

W poniższym przykładzie pokazano, jak utworzyć sieć wirtualną przy użyciu usługi Resource Manager. W tym przykładzie tworzy sieć wirtualną bramy aplikacji. Brama aplikacji wymaga jego własnej podsieci. Z tego powodu podsieci utworzone dla bramy aplikacji jest mniejszy niż przestrzeń adresową sieci wirtualnej. Umożliwia innych zasobów, w tym, ale nie jest ograniczone do serwerów sieci web można skonfigurować w tej samej sieci wirtualnej.

### <a name="step-1"></a>Krok 1

Przypisz zakres adresów 10.0.0.0/24 do zmiennej podsieci służącej do tworzenia sieci wirtualnej.  Spowoduje to utworzenie obiektu konfiguracji podsieci bramy aplikacji, która jest używana w następnym przykładzie.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Krok 2

Tworzenie sieci wirtualnej o nazwie **appgwvnet** w grupie zasobów **zarządcy zasobów appgw** dla regionu zachodnie stany USA, za pomocą 10.0.0.0/16 prefiks podsieci 10.0.0.0/24. Na tym kończy się konfigurację sieci wirtualnej z pojedynczą podsiecią bramy aplikacji mogą teraz być przechowywane.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Krok 3

Przypisanie zmiennej podsieci dla następnych kroków. To jest przekazywana do `New-AzureRMApplicationGateway` polecenia cmdlet w przyszłości kroku.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Tworzenie publicznego adresu IP dla konfiguracji frontonu

Utwórz zasób publicznego adresu IP **publicIP01** w grupie zasobów **appgw-rg** dla regionu Zachodnie stany USA. Brama aplikacji w służy do odbierania żądań do równoważenia obciążenia publiczny adres IP i adres IP wewnętrznego.  W tym przykładzie używane tylko do publicznego adresu IP. W poniższym przykładzie nie DNS skonfigurowano nazwy dla tworzenie publicznego adresu IP, ponieważ bramy aplikacji nie obsługuje niestandardowych nazw DNS na publiczne adresy IP.  Jeśli nazwy niestandardowego jest wymagana dla publicznego punktu końcowego, należy utworzyć rekord CNAME, aby wskazywał automatycznie generowanych nazwy DNS dla publicznego adresu IP.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Adres IP jest przypisywany do bramy aplikacji w chwili uruchamiania usługi.

## <a name="create-the-application-gateway-configuration"></a>Tworzenie konfiguracji bramy aplikacji

Przed utworzeniem bramy aplikacji można skonfigurować wszystkich elementów konfiguracji. Poniższe kroki utworzyć elementy konfiguracji wymagane dla zasobu bramy aplikacji.

### <a name="step-1"></a>Krok 1

Utwórz konfigurację adresu IP bramy aplikacji o nazwie **gatewayIP01**. Po uruchomieniu bramy aplikacji przejmuje on z podsieci skonfigurowanego adresu IP i kieruje ruchem do adresów IP w puli adresów IP zaplecza sieciowym. Pamiętaj, że każde wystąpienie będzie mieć jeden adres IP.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Krok 2

Skonfiguruj pulę adresów IP zaplecza o nazwie **pool1** i **pool2** z adresami IP **pool1** i **pool2**. Są to adresy IP zasobów, których hostem aplikacji sieci web, które mają być chronione przez bramę aplikacji. Te elementy członkowskie puli zaplecza wszystkie sprawdzania poprawności należy dobrej kondycji przez sond podstawowego lub niestandardowego. Następnie jest do nich kierowany ruch, gdy nadchodzą żądania do bramy aplikacji. Pule zaplecza może służyć przez wiele reguł w ramach bramy aplikacji. Oznacza to, że jedna pula zaplecza może służyć do wielu aplikacji sieci web, które znajdują się na tym samym hoście.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

W tym przykładzie dwie pule zaplecza kierować ruchem sieciowym na podstawie ścieżki adresu URL. Jedna pula odbiera ruch z ścieżkę adresu URL "/ wideo," i innych puli odbiera ruch z ścieżka "/ obrazu." Zastąp powyższe adresy IP, aby dodać własne punkty końcowe adresów IP aplikacji. 

### <a name="step-3"></a>Krok 3

Konfiguruj ustawienia bramy aplikacji **poolsetting01** i **poolsetting02** dla ruchu sieciowego z równoważeniem obciążenia w puli zaplecza. W tym przykładzie możesz skonfigurować ustawienia innej puli zaplecza dla pul zaplecza. Każda pula zaplecza może mieć własnych ustawień.  Reguły używają ustawienia HTTP zaplecza można kierować ruchem do elementów członkowskich puli zaplecza poprawne. Określa protokół i port, który jest używany do wysyłania ruchu do elementów członkowskich puli zaplecza. Na podstawie pliku cookie sesji również są określane przez ustawienia HTTP zaplecza. Włączenie koligacji na podstawie plików cookie sesji wysyła ruch do tej samej zaplecza w poprzednim żądania dla każdego pakietu.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>Krok 4

Skonfiguruj adres IP frontonu publicznego adresu IP punktów końcowych. Odbiornik używany obiekt konfiguracji IP frontonu do powiązania skierowany na zewnątrz adres IP przy użyciu odbiornika.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>Krok 5

Skonfiguruj port frontonu dla bramy aplikacji. Odbiornik używa obiektu konfiguracji portów frontonu w celu zdefiniowania jakie portu bramy aplikacji nasłuchuje ruchu na odbiornika.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a>Krok 6

Skonfiguruj odbiornik publiczny adres IP i port używany do odbierania przychodzącego ruchu sieciowego. Poniższy przykład przyjmuje wcześniej skonfigurowane konfiguracji IP frontonu, Konfiguracja portów frontonu i protokołu (Http lub Https, który jest rozróżniana wielkość liter) i konfiguruje odbiornika. W tym przykładzie odbiornik nasłuchuje ruchu HTTP na porcie 80 publicznego adresu IP, który został utworzony wcześniej.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a>Krok 7

Konfiguruj adres URL reguły ścieżki dla pul zaplecza. Ten krok umożliwia skonfigurowanie ścieżki względnej, używany przez bramę aplikacji i definiuje mapowanie między ścieżkę adresu URL i puli zaplecza, który został przypisany do obsługi ruchu przychodzącego.

> [!IMPORTANT]
> Każda ścieżka musi rozpoczynać się od "/", a gwiazdka jest dozwolona tylko na końcu. Nieprawidłowa przykładów /xyz, /xyz*, lub /xyz/*. Ciąg przekazywani do dopasowania ścieżki nie zawiera żadnego tekstu po pierwszym "?" lub "#", a te znaki są niedozwolone. 

Poniższy przykład tworzy dwie reguły: jeden dla "/ obrazu /" ścieżkę routingu ruchu do zaplecza **pool1**i drugi dla "/ wideo /" ścieżkę routingu ruchu do zaplecza **pool2**. Te reguły upewnij się, że ruch dla każdego zestawu adresów URL jest kierowany do wewnętrznej. Na przykład http://contoso.com/image/figure1.jpg przechodzi do **pool1** i http://contoso.com/video/example.mp4 przechodzi do **pool2**.

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

Jeśli ścieżka nie odpowiada żadnemu z reguły ścieżki wstępnie zdefiniowane, Konfiguracja mapowania ścieżki reguły konfiguruje również domyślna pula adresów zaplecza. Na przykład http://contoso.com/shoppingcart/test.html przechodzi do **pool1** , ponieważ jest on zdefiniowany jako domyślna pula niedopasowane ruchu.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02
```

### <a name="step-8"></a>Krok 8

Utwórz ustawienie reguły. Ten krok obejmuje skonfigurowanie bramy aplikacji, aby korzystać z routingu na podstawie ścieżki adresu URL. `$urlPathMap` Zmiennej zdefiniowanej w poprzednim kroku teraz służy do tworzenia reguł opartych na ścieżki. W tym kroku powiązane reguły z odbiornik i mapowanie ścieżki adresu URL utworzony wcześniej.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-9"></a>Krok 9

Skonfiguruj liczbę wystąpień i rozmiar bramy aplikacji.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
```

## <a name="create-an-application-gateway"></a>Tworzenie bramy aplikacji

Utwórz bramę aplikacji ze wszystkimi obiektami konfiguracji z powyższych kroków.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-an-application-gateway-dns-name"></a>Pobierz nazwę DNS bramy aplikacji

Po utworzeniu bramy, należy skonfigurować frontonu dla komunikacji. Korzystając z publicznym adresem IP, brama aplikacji wymaga przypisywany dynamicznie nazwy DNS, który nie jest przyjazną. Aby zapewnić, że klienci mogą trafień bramy aplikacji, umożliwia rekord CNAME wskaż publiczny punkt końcowy bramy aplikacji. Aby uzyskać więcej informacji, zobacz [Konfigurowanie niestandardowej nazwy domeny dla usługi w chmurze Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).

Aby skonfigurować rekord IP CNAME frontonu, należy pobrać szczegółów bramy aplikacji i skojarzonej z nią nazwy IP DNS przy użyciu elementu publicznego adresu IP dołączony na bramie aplikacji. Użyj nazwy DNS bramy aplikacji, aby utworzyć rekord CNAME. Nie zaleca się używanie rekordu, ponieważ adres VIP zmiany mogą pojawiać się ponownego uruchomienia bramy aplikacji.

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

Jeśli chcesz dowiedzieć się więcej o odciążania protokołu Secure Sockets Layer (SSL), zobacz [skonfigurować bramę aplikacji dla odciążania protokołu SSL przy użyciu usługi Azure Resource Manager](application-gateway-ssl-arm.md).

