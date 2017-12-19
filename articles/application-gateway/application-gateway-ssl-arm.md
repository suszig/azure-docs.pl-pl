---
title: "Konfigurowanie odciążania SSL - bramy aplikacji Azure — PowerShell | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera instrukcje, aby utworzyć bramę aplikacji przy użyciu protokołu SSL Odciążanie przy użyciu usługi Azure Resource Manager"
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 3c3681e0-f928-4682-9d97-567f8e278e13
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: davidmu
ms.openlocfilehash: ee48ca45ae0d337b5b919dbbb28341caf8af0d45
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Konfigurowanie bramy aplikacji na potrzeby odciążania protokołu SSL przy użyciu usługi Azure Resource Manager

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager — program PowerShell](application-gateway-ssl-arm.md)
> * [Program Azure PowerShell klasycznego](application-gateway-ssl.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](application-gateway-ssl-cli.md)

Usługę Azure Application Gateway można skonfigurować tak, aby przerywała sesję protokołu SSL (Secure Sockets Layer) na poziomie bramy, co pozwoli na uniknięcie wykonywania kosztownych zadań szyfrowania protokołu SSL w kolektywie serwerów sieci Web. Odciążanie protokołu SSL upraszcza również konfigurowanie serwerów frontonu i zarządzanie aplikacją sieci Web.

## <a name="before-you-begin"></a>Przed rozpoczęciem

1. Zainstaluj najnowszą wersję poleceń cmdlet programu Azure PowerShell za pomocą Instalatora platformy sieci Web. Najnowszą wersję można pobrać i zainstalować z sekcji **Windows PowerShell** strony [Pliki do pobrania](https://azure.microsoft.com/downloads/).
2. Utwórz sieć wirtualną i podsieć bramy aplikacji. Upewnij się, że z podsieci nie korzystają żadne maszyny wirtualne ani wdrożenia w chmurze. Brama aplikacji musi znajdować się w podsieci sieci wirtualnej.
3. Serwery, które można skonfigurować do korzystania z bramy aplikacji musi istnieć lub ich punkty końcowe utworzone w sieci wirtualnej lub wirtualnego adresu IP (VIP) przypisany lub publicznego adresu IP.

## <a name="what-is-required-to-create-an-application-gateway"></a>Co jest wymagane do utworzenia bramy aplikacji?

* **Pula serwerów zaplecza**: Lista adresów IP serwerów zaplecza. Na liście adresów IP powinna należeć do podsieci sieci wirtualnej lub powinny być publicznego adresu IP/VIP.
* **Ustawienia puli serwera zaplecza**: co Pula ma ustawienia, takie jak port protokołu i koligacji na podstawie plików cookie. Te ustawienia są powiązane z pulą i są stosowane do wszystkich serwerów w tej puli.
* **Port frontonu**: ten port jest port publiczny, który jest otwarty na bramie aplikacji. Ruch trafia do tego portu, a następnie jest przekierowywany do jednego z serwerów zaplecza.
* **Odbiornik**: odbiornika ma port frontonu, protokół (Http lub Https; tych ustawień jest rozróżniana wielkość liter) oraz nazwę certyfikatu SSL (jeśli odciążania Konfigurowanie protokołu SSL).
* **Reguła**: reguła wiąże odbiornika i puli serwerów zaplecza i określa, które puli serwerów zaplecza umożliwiających kierowanie ruchem do gdy liczba trafień określonego odbiornika. Obecnie jest obsługiwana tylko reguła *podstawowa*. Reguła *podstawowa* to dystrybucja obciążenia z działaniem okrężnym.

**Uwagi dotyczące konfiguracji dodatkowych**

W przypadku konfiguracji certyfikatów SSL protokół w polu **HttpListener** należy zmienić na **Https** (z uwzględnieniem wielkości liter). Dodaj **SslCertificate** elementu **HttpListener** z wartością zmiennej skonfigurowane dla certyfikatu SSL. Port frontonu powinny zostać uaktualnione do **443**.

**Aby włączyć koligacji na podstawie plików cookie**: można skonfigurować bramę aplikacji, aby upewnić się, że żądanie z sesji klienta jest zawsze kierowane do tej samej maszyny Wirtualnej w kolektywie serwerów sieci web. W tym celu należy wstawić umożliwia bramy do kierowania ruchem danych odpowiednio plik cookie sesji. Aby włączyć koligację opartą na plikach cookie, ustaw element **CookieBasedAffinity** na wartość **Enabled** w elemencie **BackendHttpSettings**.

## <a name="create-an-application-gateway"></a>Tworzenie bramy aplikacji

Różnica między przy użyciu modelu klasycznego wdrożenia usługi Azure i usługi Azure Resource Manager jest kolejność, w którym utworzyć bramę aplikacji i elementy, które trzeba skonfigurować.

Usługa Resource Manager wszystkie składniki bramy aplikacji są konfigurowane osobno, a następnie umieść ze sobą w celu utworzenia zasobu bramy aplikacji.

Oto kroki wymagane do utworzenia bramy aplikacji:

1. [Utwórz grupę zasobów dla Menedżera zasobów](#create-a-resource-group-for-resource-manager)
2. [Tworzenie sieci wirtualnej, podsieci i publicznego adresu IP dla bramy aplikacji](#create-virtual-network-subnet-and-public-IP-for-the-application-gateway)
3. [Utwórz obiekt konfiguracji bramy aplikacji](#create-an-application-gateway-configuration-object)
4. [Utwórz zasób bramy aplikacji](#create-an-application-gateway-resource)

## <a name="create-a-resource-group-for-resource-manager"></a>Tworzenie grupy zasobów dla usługi Resource Manager

Upewnij się, że program PowerShell został przełączony do trybu umożliwiającego korzystanie z poleceń cmdlet usługi Azure Resource Manager. Więcej informacji znajduje się w temacie [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Używanie programu Windows PowerShell z usługą Resource Manager).

   1. Wprowadź następujące polecenie:

   ```powershell
   Login-AzureRmAccount
   ```

   2. Aby sprawdzić subskrypcji dla konta, wprowadź następujące polecenia:

   ```powershell
   Get-AzureRmSubscription
   ```

   Zostanie wyświetlony monit o uwierzytelnienie przy użyciu własnych poświadczeń.

   3. Aby wybrać subskrypcję platformy Azure, aby użyć, wprowadź następujące polecenie:

   ```powershell
   Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
   ```

   4. Aby utworzyć grupę zasobów, wpisz następujące polecenie. (Pomiń ten krok, jeśli używasz istniejącej grupy zasobów).

   ```powershell
   New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
   ```

Usługa Azure Resource Manager wymaga, aby wszystkie grupy zasobów określały lokalizację. To ustawienie jest używane jako domyślna lokalizacja dla zasobów w danej grupie zasobów. Upewnij się, że wszystkie polecenia, aby utworzyć bramę aplikacji używać tej samej grupie zasobów.

W poprzednim przykładzie utworzono grupę zasobów o nazwie **zarządcy zasobów appgw** znajduje się **zachodnie stany USA**.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Tworzenie sieci wirtualnej i podsieci dla bramy aplikacji

W poniższym przykładzie pokazano, jak utworzyć sieć wirtualną przy użyciu usługi Resource Manager:

   1. Wprowadź następujące polecenie:

   ```powershell
   $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
   ```

   W tym przykładzie przypisuje zakres adresów **10.0.0.0/24** ze zmienną podsieci ma być używany do tworzenia sieci wirtualnej.

   2. Wprowadź następujące polecenie:

   ```powershell
   $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
   ```

   Ten przykład tworzy sieć wirtualną o nazwie **appgwvnet** w grupie zasobów **zarządcy zasobów appgw** dla **zachodnie stany USA** region przy użyciu prefiksu **10.0.0.0/16** z podsiecią **10.0.0.0/24**.

   3. Wprowadź następujące polecenie:

   ```powershell
   $subnet = $vnet.Subnets[0]
   ```

   W tym przykładzie przypisuje obiektu podsieci do zmiennej **$subnet** dalsze czynności.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Tworzenie publicznego adresu IP dla konfiguracji frontonu

Aby utworzyć publiczny adres IP frontonu konfiguracji, wprowadź następujące polecenie:

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Ten przykład tworzy zasób publicznego adresu IP **publicIP01** w grupie zasobów **zarządcy zasobów appgw** dla **zachodnie stany USA** regionu.

## <a name="create-an-application-gateway-configuration-object"></a>Tworzenie obiektu konfiguracji bramy aplikacji

   1. Aby utworzyć obiekt konfiguracji bramy aplikacji, wprowadź następujące polecenie:

   ```powershell
   $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
   ```

   Ten przykład tworzy konfigurację IP bramy dla aplikacji o nazwie **gatewayIP01**. Po uruchomieniu aplikacji bramy przejmuje adresu IP z podsieci skonfigurowane i kieruje ruchem sieciowym na adresy IP w puli adresów IP zaplecza. Pamiętaj, że każde wystąpienie będzie mieć jeden adres IP.

   2. Wprowadź następujące polecenie:

   ```powershell
   $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
   ```

   Ten przykład konfiguruje puli adresów IP zaplecza, o nazwie **pool01** z adresami IP **134.170.185.46**, **134.170.188.221**, i **134.170.185.50** . Te wartości to adresy IP odbierające ruch sieciowy pochodzący z punktu końcowego adresu IP frontonu. Zastąp adresy IP z poprzedniego przykładu adresami IP punktów końcowych aplikacji sieci Web.

   3. Wprowadź następujące polecenie:

   ```powershell
   $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled
   ```

   Ten przykład konfiguruje ustawienia bramy aplikacji **poolsetting01** Równoważenie obciążenia ruchem sieciowym w puli zaplecza.

   4. Wprowadź następujące polecenie:

   ```powershell
   $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443
   ```

   Ten przykład konfiguruje port IP frontonu, o nazwie **frontendport01** publicznego punktu końcowego adresu IP.

   5. Wprowadź następujące polecenie:

   ```powershell
   $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password "<password>"
   ```

   Ten przykład umożliwia skonfigurowanie certyfikatu używanego na potrzeby połączenia SSL. Ten certyfikat musi mieć format PFX, a hasło musi zawierać od 4 do 12 znaków.

   6. Wprowadź następujące polecenie:

   ```powershell
   $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
   ```

   Ten przykład tworzy konfiguracji IP frontonu o nazwie **fipconfig01** i kojarzy publiczny adres IP z konfiguracji IP frontonu.

   7. Wprowadź następujące polecenie:

   ```powershell
   $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
   ```

   Ten przykład tworzy odbiornik o nazwie **listener01** i kojarzy frontonu port konfiguracji IP frontonu i certyfikatów.

   8. Wprowadź następujące polecenie:

   ```powershell
   $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   ```

   Ten przykład tworzy regułę routingu usługi równoważenia obciążenia o nazwie **rule01** który konfiguruje zachowanie usługi równoważenia obciążenia.

   9. Wprowadź następujące polecenie:

   ```powershell
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
   ```

   Ten przykład umożliwia skonfigurowanie rozmiaru wystąpienia bramy aplikacji.

   > [!NOTE]
   > Wartość domyślna dla **InstanceCount** jest **2**, z maksymalną wartość 10. Wartość domyślna dla **GatewaySize** jest **średni**. Możesz wybrać następujące wartości: Standard_Small, Standard_Medium i Standard_Large.

   10. Wprowadź następujące polecenie:

   ```powershell
   $policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S
   ```

   Ten krok definiuje zasady SSL do używania na bramie aplikacji. Aby uzyskać więcej informacji, zobacz [skonfigurować protokół SSL wersje zasad i mechanizmów szyfrowania w bramie aplikacji](application-gateway-configure-ssl-policy-powershell.md).

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Tworzenie bramy aplikacji przy użyciu polecenia New-AzureApplicationGateway

Wprowadź następujące polecenie:

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

Ten przykład tworzy bramę aplikacji z wszystkich elementów konfiguracji z powyższych kroków. W tym przykładzie nosi nazwę bramy aplikacji **appgwtest**.

## <a name="get-the-application-gateway-dns-name"></a>Pobierz nazwę DNS bramy aplikacji

Po utworzeniu bramy, następnym krokiem jest skonfigurowanie frontonu dla komunikacji. Brama aplikacji wymaga przypisywany dynamicznie nazwy DNS, korzystając z publicznego adresu IP, który nie jest przyjazną. Aby zapewnić, że użytkownicy końcowi mogą trafień bramy aplikacji, umożliwia rekord CNAME wskaż publiczny punkt końcowy bramy aplikacji. Aby uzyskać więcej informacji, zobacz [Konfigurowanie niestandardowej nazwy domeny w usłudze Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Aby uzyskać nazwę DNS bramy aplikacji, pobrać szczegółów bramy aplikacji i skojarzonej z nią nazwy IP DNS przy użyciu **publicznego adresu IP** element dołączony na bramie aplikacji. Użyj nazwy DNS bramy aplikacji, aby utworzyć rekord CNAME, wskazujący aplikacji dwie sieci web do tej nazwy DNS. Firma Microsoft nie zaleca się użycie rekordów A, ponieważ adres VIP, można zmienić na ponowne uruchomienie z bramy aplikacji.


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

Jeśli chcesz skonfigurować bramę aplikacji za pomocą wewnętrznego modułu równoważenia obciążenia, zobacz [Utwórz bramę aplikacji z wewnętrznego modułu równoważenia obciążenia](application-gateway-ilb.md).

Aby uzyskać więcej informacji o równoważeniu obciążenia opcje ogólnie rzecz biorąc, zobacz:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
