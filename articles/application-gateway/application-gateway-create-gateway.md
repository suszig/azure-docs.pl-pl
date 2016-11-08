---
title: Tworzenie, uruchamianie i usuwanie bramy aplikacji | Microsoft Docs
description: Ta strona zawiera instrukcje dotyczące tworzenia, konfigurowania, uruchamiania i usuwania bramy aplikacji na platformie Azure
documentationcenter: na
services: application-gateway
author: georgewallace
manager: jdial
editor: tysonn

ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/02/2016
ms.author: gwallace

---
# Tworzenie, uruchamianie i usuwanie bramy aplikacji
Usługa Azure Application Gateway to moduł równoważenia obciążenia warstwy 7. Udostępnia tryb failover, oparty na wydajności routing żądań HTTP między różnymi serwerami — w chmurze i lokalnymi. Usługa Application Gateway oferuje następujące funkcje dostarczania aplikacji: równoważenie obciążenia HTTP, koligację sesji opartą na plikach cookie oraz odciążanie protokołu SSL (Secure Sockets Layer).

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager — program PowerShell](application-gateway-create-gateway-arm.md)
> * [Klasyczny portal Azure — program PowerShell](application-gateway-create-gateway.md)
> * [Szablon usługi Azure Resource Manager](application-gateway-create-gateway-arm-template.md)
> * [Interfejs wiersza polecenia platformy Azure](application-gateway-create-gateway-cli.md)
> 
> 

W tym artykule przedstawiono kroki umożliwiające tworzenie, konfigurowanie, uruchamianie i usuwanie bramy aplikacji.

## Przed rozpoczęciem
1. Zainstaluj najnowszą wersję poleceń cmdlet programu Azure PowerShell za pomocą Instalatora platformy sieci Web. Najnowszą wersję można pobrać i zainstalować z sekcji **Windows PowerShell** strony [Pliki do pobrania](https://azure.microsoft.com/downloads/).
2. Jeśli masz istniejącą sieć wirtualną, wybierz istniejącą pustą podsieć lub utwórz nową podsieć w istniejącej sieci wirtualnej wyłącznie do użytku przez tę bramę aplikacji. Nie można wdrożyć bramy aplikacji do innej sieci wirtualnej niż sieć wirtualna zasobów, które zamierzasz wdrożyć za bramą aplikacji.
3. Sprawdź, czy masz działającą sieć wirtualną z prawidłową podsiecią. Upewnij się, że z podsieci nie korzystają żadne maszyny wirtualne ani wdrożenia w chmurze. Brama aplikacji musi znajdować się w podsieci sieci wirtualnej.
4. Serwery konfigurowane do używania bramy aplikacji muszą być umieszczone w sieci wirtualnej lub z przypisanym adresem IP/VIP lub mieć w niej utworzone punkty końcowe.

## Co jest wymagane do utworzenia bramy aplikacji?
W momencie użycia polecenia **New-AzureApplicationGateway** w celu utworzenia bramy aplikacji nic nie jest jeszcze skonfigurowane i nowo utworzony zasób musi zostać skonfigurowany przy użyciu kodu XML lub obiektu konfiguracji.

Potrzebne wartości:

* **Pula serwerów zaplecza:** lista adresów IP serwerów zaplecza. Adresy IP na liście powinny należeć do podsieci sieci wirtualnej lub być publicznymi bądź wirtualnymi adresami IP.
* **Ustawienia puli serwerów zaplecza:** każda pula ma ustawienia, takie jak port, protokół i koligacja oparta na plikach cookie. Te ustawienia są powiązane z pulą i są stosowane do wszystkich serwerów w tej puli.
* **Port frontonu:** port publiczny, który jest otwierany w bramie aplikacji. Ruch trafia do tego portu, a następnie jest przekierowywany do jednego z serwerów zaplecza.
* **Odbiornik:** odbiornik ma port frontonu, protokół (Http lub Https, z uwzględnieniem wielkości liter) oraz nazwę certyfikatu SSL (w przypadku konfigurowania odciążania protokołu SSL).
* **Reguła:** reguła wiąże odbiornik z pulą serwerów zaplecza i umożliwia zdefiniowanie, do której puli serwerów zaplecza ma być przekierowywany ruch w przypadku trafienia do określonego odbiornika.

## Tworzenie bramy aplikacji
Aby utworzyć bramę aplikacji:

1. Utwórz zasób bramy aplikacji.
2. Utwórz konfiguracyjny plik XML lub obiekt konfiguracji.
3. Przekaż konfigurację aplikacji do nowo utworzonego zasobu bramy aplikacji.

> [!NOTE]
> Jeśli musisz skonfigurować niestandardową sondę bramy aplikacji, zobacz artykuł [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-classic-ps.md) (Tworzenie bramy aplikacji z sondami niestandardowymi przy użyciu programu PowerShell). Aby dowiedzieć się więcej, zapoznaj się z informacjami na temat [sond niestandardowych i monitorowania kondycji](application-gateway-probe-overview.md).
> 
> 

### Tworzenie zasobu bramy aplikacji
Aby utworzyć bramę, użyj polecenia cmdlet **New-AzureApplicationGateway**, zastępując wartości własnymi. Opłaty za bramę nie są jeszcze naliczane. Rozliczanie zaczyna się na późniejszym etapie, po pomyślnym uruchomieniu bramy.

Poniższy przykład tworzy bramę aplikacji przy użyciu sieci wirtualnej „testvnet1” i podsieci „subnet-1”.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 Parametry *Description* (opis), *InstanceCount* (Liczba wystąpień) i *GatewaySize* (Rozmiar bramy) są opcjonalne.

Aby sprawdzić, czy brama została utworzona, możesz użyć polecenia cmdlet **Get-AzureApplicationGateway**.

    Get-AzureApplicationGateway AppGwTest
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Stopped
    VirtualIPs    : {}
    DnsName       :

> [!NOTE]
> Wartość domyślna parametru *InstanceCount* to 2, a wartość maksymalna — 10. Wartość domyślna parametru *GatewaySize* to Medium (Średnia). Do wyboru są wartości Small (Mała), Medium (Średnia) i Large (Duża).
> 
> 

Parametry *VirtualIPs* (Wirtualne adresy IP) i *DnsName* (Nazwa serwera DNS) są wyświetlane jako puste, ponieważ brama nie została jeszcze uruchomiona. Zostaną utworzone, gdy brama zacznie działać.

## Konfigurowanie bramy aplikacji
Bramę aplikacji możesz skonfigurować za pomocą pliku XML lub obiektu konfiguracji.

## Konfigurowanie bramy aplikacji za pomocą pliku XML
W poniższym przykładzie używany jest plik XML, aby skonfigurować wszystkie ustawienia bramy aplikacji i zatwierdzić je w zasobie bramy aplikacji.  

### Krok 1
Skopiuj poniższy tekst do Notatnika.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>(name-of-your-frontend-port)</Name>
                <Port>(port number)</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>(name-of-your-backend-pool)</Name>
                <IPAddresses>
                    <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                    <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>(backend-setting-name-to-configure-rule)</Name>
                <Port>80</Port>
                <Protocol>[Http|Https]</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>(name-of-the-listener)</Name>
                <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
                <Protocol>[Http|Https]</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>(name-of-load-balancing-rule)</Name>
                <Type>basic</Type>
                <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
                <Listener>(name-of-the-listener)</Listener>
                <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>

Edytuj zawarte w nawiasach wartości elementów konfiguracji. Zapisz plik z rozszerzeniem .xml.

> [!IMPORTANT]
> W elemencie Http lub Https jest rozróżniana wielkość liter.
> 
> 

Poniższy przykład przedstawia sposób konfigurowania bramy aplikacji przy użyciu pliku konfiguracyjnego. W tym przykładzie równoważone jest obciążenie ruchu HTTP na publicznym porcie 80, a ruch sieciowy jest rozsyłany do portu 80 zaplecza między dwoma adresami IP.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>80</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>BackendPool1</Name>
                <IPAddresses>
                    <IPAddress>10.0.0.1</IPAddress>
                    <IPAddress>10.0.0.2</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>BackendSetting1</Name>
                <Port>80</Port>
                <Protocol>Http</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>HTTPListener1</Name>
                <FrontendPort>FrontendPort1</FrontendPort>
                <Protocol>Http</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>HttpLBRule1</Name>
                <Type>basic</Type>
                <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
                <Listener>HTTPListener1</Listener>
                <BackendAddressPool>BackendPool1</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


### Krok 2
Następnym etapem jest skonfigurowanie bramy aplikacji. Użyj polecenia cmdlet **Set-AzureApplicationGatewayConfig** z plikiem konfiguracyjnym XML.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## Skonfiguruj bramę aplikacji za pomocą obiektu konfiguracji.
Poniższy przykład przedstawia sposób konfigurowania bramy aplikacji przy użyciu obiektów konfiguracji. Wszystkie elementy konfiguracji muszą być skonfigurowane indywidualnie, a następnie dodane do obiektu konfiguracji bramy aplikacji. Po utworzeniu obiektu konfiguracji użyte zostanie polecenie cmdlet **Set-AzureApplicationGateway**, aby zatwierdzić konfigurację we wcześniej utworzonym zasobie bramy aplikacji.

> [!NOTE]
> Przed przypisaniem wartości do poszczególnych obiektów konfiguracji trzeba zadeklarować rodzaj obiektu używanego przez program PowerShell jako magazyn. Pierwszy wiersz tworzenia pojedynczych elementów definiuje obiekt Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.(nazwa obiektu), który zostanie użyty.
> 
> 

### Krok 1
Utwórz wszystkie poszczególne elementy konfiguracji.

Utwórz adres IP frontonu, jak pokazano w poniższym przykładzie.

    $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    $fip.Name = "fip1"
    $fip.Type = "Private"
    $fip.StaticIPAddress = "10.0.0.5"

Utwórz port frontonu, jak pokazano w poniższym przykładzie.

    $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    $fep.Name = "fep1"
    $fep.Port = 80

Utwórz pulę serwerów zaplecza.

 Zdefiniuj adresy IP, które zostaną dodane do puli serwerów zaplecza, jak pokazano w następnym przykładzie.

    $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    $servers.Add("10.0.0.1")
    $servers.Add("10.0.0.2")

 Użyj obiektu $server, aby dodać wartości do obiektu puli zaplecza ($pool).

    $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    $pool.BackendServers = $servers
    $pool.Name = "pool1"

Utwórz ustawienie puli serwerów zaplecza.

    $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    $setting.Name = "setting1"
    $setting.CookieBasedAffinity = "enabled"
    $setting.Port = 80
    $setting.Protocol = "http"

Utwórz odbiornik.

    $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    $listener.Name = "listener1"
    $listener.FrontendPort = "fep1"
    $listener.FrontendIP = "fip1"
    $listener.Protocol = "http"
    $listener.SslCert = ""

Utwórz regułę.

    $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    $rule.Name = "rule1"
    $rule.Type = "basic"
    $rule.BackendHttpSettings = "setting1"
    $rule.Listener = "listener1"
    $rule.BackendAddressPool = "pool1"

### Krok 2
Przypisz wszystkie poszczególne elementy konfiguracji do obiektu konfiguracji bramy aplikacji ($appgwconfig).

Dodaj do konfiguracji adres IP frontonu.

    $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    $appgwconfig.FrontendIPConfigurations.Add($fip)

Dodaj do konfiguracji port frontonu.

    $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    $appgwconfig.FrontendPorts.Add($fep)

Dodaj do konfiguracji pulę serwerów zaplecza.

    $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    $appgwconfig.BackendAddressPools.Add($pool)  

Dodaj do konfiguracji ustawienia puli serwerów zaplecza.

    $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    $appgwconfig.BackendHttpSettingsList.Add($setting)

Dodaj do konfiguracji odbiornik.

    $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    $appgwconfig.HttpListeners.Add($listener)

Dodaj do konfiguracji regułę.

    $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    $appgwconfig.HttpLoadBalancingRules.Add($rule)

### Krok 3
Przekaż obiekt konfiguracji do zasobu bramy aplikacji za pomocą polecenia cmdlet **Set-AzureApplicationGatewayConfig**.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## Uruchamianie bramy
Po skonfigurowaniu bramy użyj polecenia cmdlet **Start-AzureApplicationGateway**, aby uruchomić bramę. Naliczanie opłat za bramę aplikacji rozpocznie się po pomyślnym uruchomieniu bramy.

> [!NOTE]
> Wykonanie polecenia cmdlet **Start-AzureApplicationGateway** może trwać 15–20 minut.
> 
> 

    Start-AzureApplicationGateway AppGwTest

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## Sprawdzanie stanu bramy
Użyj polecenia cmdlet **Get-AzureApplicationGateway**, aby sprawdzić stan bramy. Jeśli polecenie cmdlet **Start-AzureApplicationGateway** zostało pomyślnie wykonane w poprzednim kroku, atrybut *State* (Stan) powinien mieć wartość Running (Uruchomiono), a atrybuty *Vip* (Wirtualny adres IP) i *DnsName* (Nazwa serwera DNS) powinny zawierać prawidłowe wpisy.

W poniższym przykładzie pokazano bramę aplikacji, która jest włączona, działająca i gotowa do przyjmowania ruchu skierowanego na adres `http://<generated-dns-name>.cloudapp.net`.

    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    Vip           : 138.91.170.26
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## Usuwanie bramy aplikacji
Aby usunąć bramę aplikacji:

1. Użyj polecenia cmdlet **Stop-AzureApplicationGateway**, aby zatrzymać bramę.
2. Użyj polecenia cmdlet **Remove-AzureApplicationGateway**, aby usunąć bramę.
3. Aby sprawdzić, czy brama została usunięta, użyj polecenia cmdlet **Get-AzureApplicationGateway**.

W poniższym przykładzie pierwszy wiersz zawiera polecenie cmdlet **Stop-AzureApplicationGateway**, a kolejne wiersze zawierają dane wyjściowe.

    Stop-AzureApplicationGateway AppGwTest

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Po zatrzymaniu bramy aplikacji użyj polecenia cmdlet **Remove-AzureApplicationGateway**, aby usunąć usługę.

    Remove-AzureApplicationGateway AppGwTest

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

Aby sprawdzić, czy usługa została usunięta, możesz użyć polecenia cmdlet **Get-AzureApplicationGateway**. Ten krok nie jest wymagany.

    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## Następne kroki
Jeśli chcesz skonfigurować odciążanie protokołu SSL, zobacz artykuł [Configure an application gateway for SSL offload](application-gateway-ssl.md) (Konfigurowanie bramy aplikacji na potrzeby odciążania protokołu SSL).

Jeśli chcesz skonfigurować bramę aplikacji do użycia z wewnętrznym modułem równoważenia obciążenia, zobacz artykuł [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md) (Tworzenie bramy aplikacji przy użyciu wewnętrznego modułu równoważenia obciążenia).

Więcej ogólnych informacji na temat opcji równoważenia obciążenia możesz znaleźć w następujących artykułach:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

<!--HONumber=Sep16_HO3-->


