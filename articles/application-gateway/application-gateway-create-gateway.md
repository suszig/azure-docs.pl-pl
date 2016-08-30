<properties
   pageTitle="Tworzenie, uruchamianie i usuwanie bramy aplikacji | Microsoft Azure"
   description="Ta strona zawiera instrukcje dotyczące tworzenia, konfigurowania, uruchamiania i usuwania bramy aplikacji na platformie Azure"
   documentationCenter="na"
   services="application-gateway"
   authors="joaoma"
   manager="jdial"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/05/2016"
   ms.author="joaoma"/>

# Tworzenie, uruchamianie i usuwanie bramy aplikacji

Usługa Azure Application Gateway to moduł równoważenia obciążenia warstwy 7. Udostępnia tryb failover, oparty na wydajności routing żądań HTTP między różnymi serwerami — w chmurze i lokalnymi. Usługa Application Gateway oferuje następujące funkcje dostarczania aplikacji: równoważenie obciążenia HTTP, koligację sesji opartą na plikach cookie oraz odciążanie protokołu SSL (Secure Sockets Layer).

> [AZURE.SELECTOR]
- [Klasyczny portal Azure — program PowerShell](application-gateway-create-gateway.md)
- [Azure Resource Manager — program PowerShell](application-gateway-create-gateway-arm.md)
- [Szablon usługi Azure Resource Manager](application-gateway-create-gateway-arm-template.md)


<BR>

W tym artykule przedstawiono kroki umożliwiające tworzenie, konfigurowanie, uruchamianie i usuwanie bramy aplikacji.


## Przed rozpoczęciem

1. Zainstaluj najnowszą wersję poleceń cmdlet programu Azure PowerShell za pomocą Instalatora platformy sieci Web. Najnowszą wersję można pobrać i zainstalować z sekcji **Windows PowerShell** strony [Pliki do pobrania](https://azure.microsoft.com/downloads/).
2. Sprawdź, czy masz działającą sieć wirtualną z prawidłową podsiecią. Upewnij się, że z podsieci nie korzystają żadne maszyny wirtualne ani wdrożenia w chmurze. Brama aplikacji musi znajdować się w podsieci sieci wirtualnej.
3. Serwery, które będziesz konfigurować do używania bramy aplikacji, muszą być umieszczone lub mieć punkty końcowe utworzone w sieci wirtualnej lub z przypisanym adresem IP/VIP.

## Co jest wymagane do utworzenia bramy aplikacji?


W momencie użycia polecenia **New-AzureApplicationGateway** w celu utworzenia bramy aplikacji nic nie jest jeszcze skonfigurowane i nowo utworzony zasób musi zostać skonfigurowany przy użyciu kodu XML lub obiektu konfiguracji.


Potrzebne wartości:

- **Pula serwerów zaplecza:** lista adresów IP serwerów zaplecza. Adresy IP na liście powinny należeć do podsieci sieci wirtualnej lub być publicznymi bądź wirtualnymi adresami IP.
- **Ustawienia puli serwerów zaplecza:** każda pula ma ustawienia, takie jak port, protokół i koligacja oparta na plikach cookie. Te ustawienia są powiązane z pulą i są stosowane do wszystkich serwerów w tej puli.
- **Port frontonu:** port publiczny, który jest otwierany w bramie aplikacji. Ruch trafia do tego portu, a następnie jest przekierowywany do jednego z serwerów zaplecza.
- **Odbiornik:** odbiornik ma port frontonu, protokół (Http lub Https, z uwzględnieniem wielkości liter) oraz nazwę certyfikatu SSL (w przypadku konfigurowania odciążania protokołu SSL).
- **Reguła:** reguła wiąże odbiornik z pulą serwerów zaplecza i definiuje, do której puli serwerów zaplecza ma być przekierowywany ruch w przypadku trafienia do określonego odbiornika.


## Tworzenie nowej bramy aplikacji

Aby utworzyć bramę aplikacji:

1. Utwórz zasób bramy aplikacji.
2. Utwórz konfiguracyjny plik XML lub obiekt konfiguracji.
3. Przekaż konfigurację aplikacji do nowo utworzonego zasobu bramy aplikacji.

>[AZURE.NOTE] Jeśli musisz skonfigurować niestandardową sondę bramy aplikacji, zobacz artykuł [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-classic-ps.md) (Tworzenie bramy aplikacji z sondami niestandardowymi przy użyciu programu PowerShell). Aby dowiedzieć się więcej, zapoznaj się z informacjami na temat [sond niestandardowych i monitorowania kondycji](application-gateway-probe-overview.md).


### Tworzenie zasobu bramy aplikacji

Aby utworzyć bramę, użyj polecenia cmdlet **New-AzureApplicationGateway**, zastępując wartości własnymi. Zauważ, że opłaty za bramę nie są jeszcze naliczane. Rozliczanie zaczyna się na późniejszym etapie, po pomyślnym uruchomieniu bramy.

Poniższy przykład tworzy nową bramę aplikacji przy użyciu sieci wirtualnej „testvnet1” i podsieci „subnet-1”.


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

>[AZURE.NOTE]  Wartość domyślna parametru *InstanceCount* to 2, a wartość maksymalna — 10. Wartość domyślna parametru *GatewaySize* to Medium (Średnia). Do wyboru są wartości Small (Mała), Medium (Średnia) i Large (Duża).


 Parametry *VirtualIPs* (Wirtualne adresy IP) i *DnsName* (Nazwa serwera DNS) są wyświetlane jako puste, ponieważ brama nie została jeszcze uruchomiona. Zostaną utworzone, gdy brama zacznie działać.

## Konfigurowanie bramy aplikacji

Bramę aplikacji możesz skonfigurować za pomocą pliku XML lub obiektu konfiguracji.

## Konfigurowanie bramy aplikacji za pomocą pliku XML

W poniższym przykładzie użyjesz pliku XML, aby skonfigurować wszystkie ustawienia bramy aplikacji i przekazać je do zasobu bramy aplikacji.  

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

>[AZURE.IMPORTANT] W elemencie Http lub Https jest rozróżniana wielkość liter.

Poniższy przykład przedstawia sposób konfigurowania bramy aplikacji przy użyciu pliku konfiguracyjnego, aby równoważyć obciążenie ruchu HTTP na publicznym porcie 80 i rozsyłać ruch sieciowy do portu 80 zaplecza między dwoma adresami IP.

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

Poniższy przykład przedstawia sposób konfigurowania bramy aplikacji przy użyciu obiektów konfiguracji. Wszystkie elementy konfiguracji muszą być skonfigurowane indywidualnie, a następnie dodane do obiektu konfiguracji bramy aplikacji. Po utworzeniu obiektu konfiguracji użyjesz polecenia cmdlet **Set-AzureApplicationGateway**, aby przekazać konfigurację do wcześniej utworzonego zasobu bramy aplikacji.

>[AZURE.NOTE] Przed przypisaniem wartości do poszczególnych obiektów konfiguracji trzeba zadeklarować rodzaj obiektu używanego przez program PowerShell jako magazyn. Pierwszy wiersz tworzenia pojedynczych elementów definiuje obiekt Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.(nazwa obiektu), który będzie używany.

### Krok 1

Utwórz wszystkie poszczególne elementy konfiguracji.

Utwórz adres IP frontonu, jak pokazano w poniższym przykładzie.

    PS C:\> $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    PS C:\> $fip.Name = "fip1"
    PS C:\> $fip.Type = "Private"
    PS C:\> $fip.StaticIPAddress = "10.0.0.5"

Utwórz port frontonu, jak pokazano w poniższym przykładzie.

    PS C:\> $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    PS C:\> $fep.Name = "fep1"
    PS C:\> $fep.Port = 80

Utwórz pulę serwerów zaplecza.

 Zdefiniuj adresy IP, które zostaną dodane do puli serwerów zaplecza, jak pokazano w następnym przykładzie.


    PS C:\> $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    PS C:\> $servers.Add("10.0.0.1")
    PS C:\> $servers.Add("10.0.0.2")

 Użyj obiektu $server, aby dodać wartości do obiektu puli zaplecza ($pool).

    PS C:\> $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    PS C:\> $pool.BackendServers = $servers
    PS C:\> $pool.Name = "pool1"

Utwórz ustawienie puli serwerów zaplecza.

    PS C:\> $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    PS C:\> $setting.Name = "setting1"
    PS C:\> $setting.CookieBasedAffinity = "enabled"
    PS C:\> $setting.Port = 80
    PS C:\> $setting.Protocol = "http"

Utwórz odbiornik.

    PS C:\> $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    PS C:\> $listener.Name = "listener1"
    PS C:\> $listener.FrontendPort = "fep1"
    PS C:\> $listener.FrontendIP = "fip1"
    PS C:\> $listener.Protocol = "http"
    PS C:\> $listener.SslCert = ""

Utwórz regułę.

    PS C:\> $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    PS C:\> $rule.Name = "rule1"
    PS C:\> $rule.Type = "basic"
    PS C:\> $rule.BackendHttpSettings = "setting1"
    PS C:\> $rule.Listener = "listener1"
    PS C:\> $rule.BackendAddressPool = "pool1"

### Krok 2

Przypisz wszystkie poszczególne elementy konfiguracji do obiektu konfiguracji bramy aplikacji ($appgwconfig).

Dodaj do konfiguracji adres IP frontonu.

    PS C:\> $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    PS C:\> $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    PS C:\> $appgwconfig.FrontendIPConfigurations.Add($fip)

Dodaj do konfiguracji port frontonu.

    PS C:\> $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    PS C:\> $appgwconfig.FrontendPorts.Add($fep)

Dodaj do konfiguracji pulę serwerów zaplecza.

    PS C:\> $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    PS C:\> $appgwconfig.BackendAddressPools.Add($pool)  

Dodaj do konfiguracji ustawienia puli serwerów zaplecza.

    PS C:\> $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    PS C:\> $appgwconfig.BackendHttpSettingsList.Add($setting)

Dodaj do konfiguracji odbiornik.

    PS C:\> $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    PS C:\> $appgwconfig.HttpListeners.Add($listener)

Dodaj do konfiguracji regułę.

    PS C:\> $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    PS C:\> $appgwconfig.HttpLoadBalancingRules.Add($rule)

### Krok 3

Przekaż obiekt konfiguracji do zasobu bramy aplikacji za pomocą polecenia cmdlet **Set-AzureApplicationGatewayConfig**.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## Uruchamianie bramy

Po skonfigurowaniu bramy użyj polecenia cmdlet **Start-AzureApplicationGateway**, aby uruchomić bramę. Naliczanie opłat za bramę aplikacji rozpocznie się po pomyślnym uruchomieniu bramy.


> [AZURE.NOTE] Wykonanie polecenia cmdlet **Start-AzureApplicationGateway** może trwać 15–20 minut.



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

- [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)



<!--HONumber=jun16_HO2-->


