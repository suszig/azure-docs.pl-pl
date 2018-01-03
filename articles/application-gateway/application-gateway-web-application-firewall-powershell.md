---
title: "Konfigurowanie zapory aplikacji sieci web: Brama aplikacji w usłudze Azure | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera wskazówki dotyczące sposobu uruchamiania za pomocą zapory aplikacji sieci web w istniejącej lub nowej bramy aplikacji."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: davidmu
ms.openlocfilehash: e8106805d21b325e33fb3ab376db75cd783b9042
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="configure-a-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Konfigurowanie zapory aplikacji sieci web w bramie nowej lub istniejącej aplikacji

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-web-application-firewall-portal.md)
> * [Program PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Interfejs wiersza polecenia platformy Azure](application-gateway-web-application-firewall-cli.md)

Dowiedz się, jak utworzyć zapory aplikacji sieci web (WAF)-włączone bramy aplikacji. Również informacje o sposobie dodawanie zapory aplikacji sieci Web do istniejącej bramy aplikacji.

Zapory aplikacji sieci Web w brama aplikacji w usłudze Azure chroni aplikacje sieci web przed wspólnej ataków opartych na sieci web takich jak iniekcja kodu SQL, ataki skryptów między witrynami i hijacks sesji.

 Brama aplikacji jest modułem równoważenia obciążenia warstwy 7. Zapewnia on trybu failover, wydajności routingu żądań HTTP między różnymi serwerami, czy są one w chmurze lub lokalnie. Brama aplikacji w oferują wiele funkcji kontrolera (ADC) dostarczania aplikacji:

 * Równoważenie obciążenia HTTP
 * Koligacji na podstawie plików cookie sesji
 * Secure Sockets Layer (SSL) odciążania
 * Sondy kondycji niestandardowych
 * Obsługa funkcji wielooddziałowości
 
 Aby uzyskać pełną listę obsługiwanych funkcji, zobacz [brama Omówienie aplikacji](application-gateway-introduction.md).

W tym artykule przedstawiono sposób [Dodawanie zapory aplikacji sieci Web do istniejącej bramy aplikacji](#add-web-application-firewall-to-an-existing-application-gateway). Zawiera także jak [Utwórz bramę aplikacji, która używa zapory aplikacji sieci Web](#create-an-application-gateway-with-web-application-firewall).

![Scenariusz obrazu][scenario]

## <a name="waf-configuration-differences"></a>Różnice w konfiguracji zapory aplikacji sieci Web

Jeśli zostały przeczytane [Utwórz bramę aplikacji przy użyciu programu PowerShell](application-gateway-create-gateway-arm.md), zrozumieć ustawienia jednostki SKU można skonfigurować podczas tworzenia bramy aplikacji. Zapory aplikacji sieci Web udostępnia dodatkowe ustawienia, aby zdefiniować podczas konfigurowania SKU bramy aplikacji. Brak dodatkowych zmian wprowadzonych na bramy aplikacji.

| **Ustawienie** | **Szczegóły**
|---|---|
|**SKU** |Bramy normalne aplikacji bez zapory aplikacji sieci Web obsługuje **standardowe\_małych**, **standardowe\_średni**, i **standardowe\_duży**rozmiary. Wraz z wprowadzeniem zapory aplikacji sieci Web, istnieją dwie dodatkowe jednostki SKU, **WAF\_średni** i **zapory aplikacji sieci Web\_duży**. Zapory aplikacji sieci Web nie jest obsługiwana w bramach małych aplikacji.|
|**Warstwy** | Dostępne wartości to **standardowe** lub **WAF**. Jeśli korzystasz z zapory aplikacji sieci Web, musisz wybrać **WAF**.|
|**Tryb** | To ustawienie jest tryb zapory aplikacji sieci Web. dozwolone wartości to **wykrywania** i **zapobiegania**. Gdy zapory aplikacji sieci Web jest ustawiany w **wykrywania** trybie wszystkie zagrożenia są przechowywane w pliku dziennika. W **zapobiegania** tryb, zdarzenia są nadal rejestrowane, ale osoba atakująca odbiera 403 nieautoryzowany odpowiedzi z bramy aplikacji.|

## <a name="add-a-web-application-firewall-to-an-existing-application-gateway"></a>Dodawanie zapory aplikacji sieci web do istniejącej bramy aplikacji

Upewnij się, że używasz najnowszej wersji programu Azure PowerShell. Aby uzyskać więcej informacji, zobacz [Użyj środowiska Windows PowerShell z usługą Resource Manager](../powershell-azure-resource-manager.md).

1. Zaloguj się do konta platformy Azure.

    ```powershell
    Login-AzureRmAccount
    ```

2. Wybierz subskrypcję do użycia na potrzeby tego scenariusza.

    ```powershell
    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"
    ```

3. Pobierz bramę, której chcesz dodać zapory aplikacji sieci Web.

    ```powershell
    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"
    ```

4. Skonfiguruj SKU zapory aplikacji sieci Web. Dostępne rozmiary są **zapory aplikacji sieci Web\_duży** i **WAF\_średni**. Jeśli korzystasz z zapory aplikacji sieci Web, warstwy musi być **WAF**. Pojemność upewnij się, gdy wartość jednostki SKU.

    ```powershell
    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2
    ```

5. Skonfiguruj ustawienia zapory aplikacji sieci Web, zgodnie z definicją w poniższym przykładzie. Aby uzyskać **FirewallMode**, dostępne wartości to **zapobiegania** i **wykrywania**.

    ```powershell
    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention
    ```

6. Zaktualizuj bramę aplikacji z ustawieniami zdefiniowanych w poprzednim kroku.

    ```powershell
    Set-AzureRmApplicationGateway -ApplicationGateway $gw
    ```

To polecenie aktualizuje bramy aplikacji zapory aplikacji sieci Web. Aby poznać sposób wyświetlania dzienniki bramy aplikacji, zobacz [diagnostyki bramy aplikacji](application-gateway-diagnostics.md). Z powodu zabezpieczeń rodzaj zapory aplikacji sieci Web Przejrzyj dzienniki regularnie, aby poznać strukturę aplikacji sieci web.

## <a name="create-an-application-gateway-with-a-web-application-firewall"></a>Utwórz bramę aplikacji za pomocą zapory aplikacji sieci web

Poniższe kroki przedstawiają cały proces tworzenia bramy aplikacji z zapory aplikacji sieci Web.

Upewnij się, że używasz najnowszej wersji programu Azure PowerShell. Aby uzyskać więcej informacji, zobacz [Użyj środowiska Windows PowerShell z usługą Resource Manager](../powershell-azure-resource-manager.md).

1. Logowanie do platformy Azure, uruchamiając `Login-AzureRmAccount`. Zostanie wyświetlony monit o uwierzytelniania przy użyciu poświadczeń.

2. Sprawdź subskrypcje dla konta, uruchamiając `Get-AzureRmSubscription`.

3. Wybierz subskrypcji Azure, której można użyć.

    ```powershell
    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
    ```

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów dla bramy aplikacji.

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Usługa Azure Resource Manager wymaga, aby wszystkie grupy zasobów określały lokalizację. Ta lokalizacja będzie używana jako domyślna lokalizacja dla zasobów w danej grupie zasobów. Upewnij się, że wszystkie polecenia, aby utworzyć bramę aplikacji używać tej samej grupie zasobów.

W powyższym przykładzie utworzono grupę zasobów o nazwie "appgw-zarządcy zasobów" Lokalizacja "Zachodnie stany USA".

> [!NOTE]
> Jeśli musisz skonfigurować niestandardową sondę bramy aplikacji, zobacz artykuł [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md) (Tworzenie bramy aplikacji z sondami niestandardowymi przy użyciu programu PowerShell). Aby uzyskać więcej informacji, zobacz [monitorowanie kondycji i badania niestandardowe](application-gateway-probe-overview.md).

### <a name="configure-a-virtual-network"></a>Skonfiguruj sieć wirtualną

Brama aplikacji wymaga własnej podsieci. W tym kroku utworzysz sieci wirtualnej przestrzeni adresowej 10.0.0.0/16 z dwoma podsieciami, jeden dla bramy aplikacji i jeden dla członków puli zaplecza.

```powershell
# Create a subnet configuration object for the application gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in the subnet for application gateway instances. With a smaller subnet, you might not be able to add more instances of your application gateway in the future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for the back-end pool members subnet.
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create the virtual network with the previously created subnets.
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="configure-the-public-ip-address"></a>Skonfiguruj publicznego adresu IP

Do obsługi żądań zewnętrznych, bramy aplikacji wymaga publicznego adresu IP. Ten publiczny adres IP nie może mieć `DomainNameLabel` definicja ma być używany przez bramę aplikacji.

```powershell
# Create a public IP address for use with the application gateway. Defining the `DomainNameLabel` during creation is not supported for use with the application gateway.
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

### <a name="configure-the-application-gateway"></a>Konfigurowanie bramy aplikacji

```powershell
# Create an IP configuration to configure which subnet the application gateway uses. When the application gateway starts, it picks up an IP address from the configured subnet and routes network traffic to the IP addresses in the back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

# Create a back-end pool to hold the addresses or NIC handles for the application that the application gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Upload the authentication certificate to be used to communicate with the back-end servers.
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

# Configure the back-end HTTP settings to be used to define how traffic is routed to the back-end pool. The authentication certificate used in the previous step is added to the back-end HTTP settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

# Create a front-end port to be used by the listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

# Create a front-end IP configuration to associate the public IP address with the application gateway.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Configure the certificate for the application gateway. This certificate is used to decrypt and re-encrypt the traffic on the application gateway.
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

# Create the HTTP listener for the application gateway. Assign the front-end IP configuration, port, and SSL certificate to use.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

# Create a load-balancer routing rule that configures the load balancer behavior. In this example, a basic round-robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure the SKU of the application gateway.
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

# Define the SSL policy to use.
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S

# Configure the WAF configuration settings.
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"

# Create the application gateway by using all the previously created configuration objects.
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> Bramy aplikacji utworzonych za pomocą podstawową konfigurację zapory aplikacji sieci Web są skonfigurowane z CRS 3.0 do ochrony.

## <a name="get-an-application-gateway-dns-name"></a>Pobierz nazwę DNS bramy aplikacji

Po utworzeniu bramy, następnym krokiem jest skonfigurowanie frontonu dla komunikacji. Gdy używasz publicznego adresu IP bramy aplikacji wymaga przypisywany dynamicznie nazwy DNS, który nie jest przyjazną. Aby upewnić się, że użytkownicy mogą trafień bramy aplikacji, wskaż publiczny punkt końcowy bramy aplikacji przy użyciu rekordu CNAME. Aby uzyskać więcej informacji, zobacz [Konfigurowanie niestandardowej nazwy domeny dla usługi w chmurze Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Aby skonfigurować alias, należy pobrać szczegółów bramy aplikacji i skojarzonej z nią nazwy IP DNS przy użyciu elementu publicznego adresu IP dołączony na bramie aplikacji. Użyj nazwy DNS bramy aplikacji, aby utworzyć rekord CNAME, wskazujący aplikacji dwie sieci web do tej nazwy DNS. Nie zaleca się przy użyciu rekordów, ponieważ adres VIP może ulec zmianie po ponownym uruchomieniu bramy aplikacji.

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

Aby dowiedzieć się, jak skonfigurować diagnostycznych rejestrowanie do dziennika zdarzeń, które wykryte lub uniemożliwił z zapory aplikacji sieci Web, zobacz [diagnostyki bramy aplikacji](application-gateway-diagnostics.md).

[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png
