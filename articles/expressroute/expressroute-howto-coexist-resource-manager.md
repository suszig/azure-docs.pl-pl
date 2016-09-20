<properties
   pageTitle="Konfigurowanie połączeń usługi ExpressRoute i sieci VPN typu lokacja-lokacja, które mogą współistnieć, dla modelu wdrażania usługi Resource Manager | Microsoft Azure"
   description="Ten artykuł zawiera instrukcje dotyczące konfigurowania połączeń usługi ExpressRoute oraz sieci VPN typu lokacja-lokacja, które mogą współistnieć, dla modelu usługi Resource Manager."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/19/2016"
   ms.author="charleywen"/>

# Konfigurowanie połączeń usługi ExpressRoute i współistniejących połączeń typu lokacja-lokacja dla modelu wdrażania usługi Resource Manager

> [AZURE.SELECTOR]
- [Program PowerShell — model usługi Resource Manager](expressroute-howto-coexist-resource-manager.md)
- [PowerShell — model klasyczny](expressroute-howto-coexist-classic.md)

Możliwość skonfigurowania sieci VPN typu lokacja-lokacja i usługi ExpressRoute niesie ze sobą pewne korzyści. Sieć VPN typu lokacja-lokacja można skonfigurować jako bezpieczną ścieżkę pracy awaryjnej dla usługi ExpressRoute lub użyć jej do połączenia z witrynami, które nie zostały połączone za pośrednictwem usługi ExpressRoute. Ten artykuł zawiera instrukcje konfiguracji obu scenariuszy. Ten artykuł ma zastosowanie w modelu wdrażania usługi Resource Manager. Ta konfiguracja nie jest dostępna w portalu Azure.


**Modele wdrażania Azure — informacje**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

>[AZURE.IMPORTANT] Przed wykonaniem poniższych instrukcji należy wstępnie skonfigurować obwody usługi ExpressRoute. Przed wykonaniem poniższych kroków należy koniecznie wykonać instrukcje [tworzenia obwodu usługi ExpressRoute](expressroute-howto-circuit-arm.md) i [konfigurowania routingu](expressroute-howto-routing-arm.md).

## Limity i ograniczenia

- **Routing tranzytowy nie jest obsługiwany:** nie można skierować połączenia (przez platformę Azure) między lokalną siecią połączoną za pośrednictwem sieci VPN typu lokacja-lokacja i lokalną siecią połączoną za pośrednictwem usługi ExpressRoute.
- **Nie można włączyć tunelowania wymuszonego dla bramy sieci VPN typu lokacja-lokacja:** można tylko „wymusić” cały ruch związany z Internetem z powrotem do sieci lokalnej za pośrednictwem usługi ExpressRoute. 
- **Tylko bramy standardowe lub o wysokiej wydajności:** należy użyć bramy standardowej lub o wysokiej wydajności zarówno dla bramy usługi ExpressRoute, jak i bramy sieci VPN typu lokacja-lokacja. Informacje o jednostkach SKU bramy znajdują się w artykule [Gateway SKUs](../vpn-gateway/vpn-gateway-about-vpngateways.md) (Jednostki SKU bramy).
- **Tylko brama sieci VPN oparta na trasach:** należy używać bramy sieci VPN opartej na trasach. Informacje na temat bramy sieci VPN opartej na trasach znajdują się w artykule [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) (Brama sieci VPN).
- **Wymaganie trasy statycznej:** jeśli sieć lokalna jest połączona z usługą ExpressRoute oraz siecią VPN typu lokacja-lokacja, aby skierować połączenie sieci VPN typu lokacja-lokacja do publicznego Internetu, trzeba mieć skonfigurowaną trasę statyczną w sieci lokalnej.
- **Najpierw należy skonfigurować bramę usługi ExpressRoute:** przed dodaniem bramy sieci VPN typu lokacja-lokacja należy utworzyć bramę usługi ExpressRoute.


## Projekty konfiguracji

### Konfigurowanie sieci VPN typu lokacja-lokacja jako ścieżki pracy awaryjnej dla usługi ExpressRoute

Połączenie sieci VPN typu lokacja-lokacja można skonfigurować do przechowywania kopii zapasowych dla usługi ExpressRoute. Dotyczy to tylko sieci wirtualnych połączonych ze ścieżką prywatnej sieci równorzędnej Azure. Nie ma rozwiązania pracy awaryjnej opartego na sieci VPN dla usług dostępnych przez publiczne sesje komunikacji równorzędnej platformy Azure ani komunikacji równorzędnej firmy Microsoft. Obwód usługi ExpressRoute jest zawsze połączeniem podstawowym. Dane będą przepływać przez ścieżkę sieci VPN typu lokacja-lokacja tylko w wypadku awarii obwodu usługi ExpressRoute. 

![Współistnienie](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### Konfigurowanie sieci VPN typu lokacja-lokacja do łączenia z witrynami niepołączonymi przez usługę ExpressRoute

Można skonfigurować sieć w taki sposób, by niektóre witryny łączyły się bezpośrednio z platformą Azure za pośrednictwem sieci VPN typu lokacja-lokacja, a niektóre przez usługę ExpressRoute. 

![Współistnienie](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

>[AZURE.NOTE] Nie można skonfigurować sieci wirtualnej jako routera tranzytowego.

## Wybieranie czynności do wykonania

Istnieją dwa różne zestawy procedur do wyboru służące do konfigurowania połączeń, które mogą współistnieć. Wybór procedury konfiguracji będzie zależeć od tego, czy masz istniejącą sieć wirtualną, z którą chcesz się połączyć, czy chcesz utworzyć nową.


- Nie mam sieci wirtualnej i muszę ją utworzyć.
    
    Jeśli nie masz jeszcze sieci wirtualnej, ta procedura zawiera instrukcje tworzenia nowej sieci wirtualnej za pomocą modelu wdrażania usługi Resource Manager i tworzenia nowych połączeń usługi ExpressRoute i sieci VPN typu lokacja-lokacja. Aby przeprowadzić konfigurację, wykonaj kroki opisane w sekcji artykułu [Aby utworzyć nową sieć wirtualną i współistniejące połączenia](#new).

- Mam już sieć wirtualną modelu wdrażania usługi Resource Manager.

    Być może masz już gotową sieć wirtualną z istniejącym połączeniem sieci VPN typu lokacja-lokacja lub połączeniem usługi ExpressRoute. Sekcja [Aby skonfigurować współistniejące połączenia dla istniejącej sieci wirtualnej](#add) zawiera instrukcje usuwania bramy, a następnie tworzenia nowych połączeń usługi ExpressRoute i połączeń VPN typu lokacja-lokacja. Należy pamiętać, że podczas tworzenia nowych połączeń kroki muszą być wykonywane po kolei. Do tworzenia bram i połączeń nie używaj instrukcji z innych artykułów.

    W tej procedurze tworzenie połączeń, które mogą współistnieć, wymaga usunięcia bramy, a następnie skonfigurowania nowych bram. Oznacza to, że podczas usuwania i odtwarzania bramy oraz połączeń wystąpi przestój względem połączeń obejmujących wiele lokalizacji, ale nie trzeba będzie migrować żadnych maszyn wirtualnych ani usług do nowej sieci wirtualnej. Podczas konfigurowania bramy maszyny wirtualne i usługi będą mogły nadal komunikować się za pośrednictwem modułu równoważenia obciążenia, jeżeli zostały w taki sposób skonfigurowane.


## <a name="new"></a>Aby utworzyć nową sieć wirtualną i współistniejące połączenia

Ta procedura zawiera instrukcje tworzenia sieci wirtualnej i połączeń typu lokacja-lokacja oraz usługi ExpressRoute, które będą współistnieć.
    
1. Niezbędne jest zainstalowanie najnowszej wersji poleceń cmdlet programu Azure PowerShell.  Aby uzyskać więcej informacji na temat instalowania poleceń cmdlet programu Azure PowerShell, zobacz artykuł [How to install and configure Azure PowerShell](../powershell-install-configure.md) (Instalowanie i konfigurowanie programu Azure PowerShell). Pamiętaj, że polecenia cmdlet, które zostaną użyte do tej konfiguracji, mogą trochę różnić się od tych, które znasz. Koniecznie użyj poleceń cmdlet podanych w tych instrukcjach.

2. Zaloguj się do konta i skonfiguruj środowisko.
    
        login-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
        $location = "Central US"
        $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location

3. Utwórz sieć wirtualną, w tym podsieć bramy. Więcej informacji na temat konfigurowania sieci wirtualnej znajduje się w temacie [Azure Virtual Network configuration](../virtual-network/virtual-networks-create-vnet-arm-ps.md) (Konfigurowanie sieci wirtualnej Azure).

    >[AZURE.IMPORTANT] Wartość podsieci bramy musi wynosić /27; prefiks może też być krótszy (np. /26 lub /25).
    
    Utwórz nową sieć wirtualną.

        $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16" 

    Dodaj podsieci.

        Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
        Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"

    Zapisz konfigurację sieci wirtualnej.

        $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

4. <a name="gw"></a>Utwórz bramę usługi ExpressRoute. Więcej informacji na temat konfigurowania bramy usługi ExpressRoute znajduje się w artykule [ExpressRoute gateway configuration](expressroute-howto-add-gateway-resource-manager.md) (Konfigurowanie bramy usługi ExpressRoute). Opcja GatewaySKU musi mieć wartość *Standard* lub *HighPerformance*.

        $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
        $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
        $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard 

5. Połącz bramę usługi ExpressRoute z obwodem usługi ExpressRoute. Po ukończeniu tego kroku zostanie nawiązane połączenie między siecią lokalną i platformą Azure za pośrednictwem usługi ExpressRoute. Więcej informacji na temat operacji łączenia znajduje się w artykule [Link VNets to ExpressRoute](expressroute-howto-linkvnet-arm.md) (Łączenie sieci wirtualnych z usługą ExpressRoute).

        $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
        New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute

6. <a name="vpngw"></a>Następnie utwórz bramę sieci VPN typu lokacja-lokacja. Więcej informacji dotyczących konfigurowania bramy sieci VPN znajduje się w temacie [Configure a VNet to VNet connection](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) (Konfigurowanie sieci wirtualnej do połączenia sieci wirtualnej). Opcja GatewaySKU musi mieć wartość *Standard* lub *HighPerformance*. Parametr VpnType musi mieć wartość *RouteBased*.

        $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
        $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
        New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"

7. Utwórz obiekt bramy sieci VPN witryny lokalnej. To polecenie nie powoduje skonfigurowania bramy lokalnej sieci VPN. Umożliwia raczej zapewnienie ustawień bramy lokalnej, np. publicznego adresu IP i lokalnej przestrzeni adresowej, aby brama sieci VPN Azure mogła się z nimi połączyć. 
    >[AZURE.NOTE] Jeżeli sieć lokalna ma wiele tras, możesz je wszystkie przekazać w postaci tablicy.  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")  

        $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix '10.100.0.0/16'

8. Skonfiguruj lokalne urządzenie sieci VPN do połączenia z nową bramą sieci VPN Azure. Więcej informacji na temat konfigurowania urządzenia VPN znajduje się w artykule [VPN Device Configuration](../vpn-gateway/vpn-gateway-about-vpn-devices.md) (Konfigurowanie urządzenia VPN).

9. Połącz bramę sieci VPN typu lokacja-lokacja na platformie Azure z bramą lokalną.

        $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
        New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>


## <a name="add"></a>Aby skonfigurować współistniejące połączenia dla istniejącej sieci wirtualnej

Jeśli masz istniejącą sieć wirtualną, sprawdź rozmiar podsieci bramy. Jeśli podsieć bramy ma wartość /28 lub /29, musisz najpierw usunąć bramę sieci wirtualnej i zwiększyć rozmiar podsieci bramy. W krokach w tej sekcji przedstawiono, jak to zrobić.

Jeśli podsieć bramy ma wartość /27 lub większą, a sieć wirtualna jest połączona za pośrednictwem usługi ExpressRoute, możesz pominąć poniższe kroki i przejść do tematu [„Krok 6 — tworzenie bramy sieci VPN typu lokacja-lokacja”](#vpngw) w poprzedniej sekcji. 

>[AZURE.NOTE] Po usunięciu istniejącej bramy podczas pracy nad tą konfiguracją lokalizacja miejscowa straci połączenie z siecią wirtualną. 

1. Niezbędne jest zainstalowanie najnowszej wersji poleceń cmdlet programu Azure PowerShell.  Aby uzyskać więcej informacji na temat instalowania poleceń cmdlet programu Azure PowerShell, zobacz artykuł [How to install and configure Azure PowerShell](../powershell-install-configure.md) (Instalowanie i konfigurowanie programu Azure PowerShell). Pamiętaj, że polecenia cmdlet, które zostaną użyte do tej konfiguracji, mogą trochę różnić się od tych, które znasz. Koniecznie użyj poleceń cmdlet podanych w tych instrukcjach. 

2. Usuń istniejącą bramę usługi ExpressRoute lub sieci VPN typu lokacja-lokacja. 

        Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>

3. Usuń podsieć bramy.
        
        $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> 
        Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet

4. Dodaj podsieć bramy o wartości /27 lub większej.
    >[AZURE.NOTE] Jeśli nie masz wystarczającej liczby adresów IP w sieci wirtualnej, aby zwiększyć rozmiar podsieci bramy, dodaj więcej przestrzeni adresowej IP.

        $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
        Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"

    Zapisz konfigurację sieci wirtualnej.

        $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

5. Na tym etapie będziesz mieć sieć wirtualną bez bram. W celu utworzenia nowych bram i wykonania połączeń wykonaj instrukcje z części [Krok 4 — tworzenie bramy usługi ExpressRoute](#gw) znajdujące się w poprzednim zestawie kroków.

## Aby dodać konfigurację typu punkt-lokacja do bramy sieci VPN
Możesz wykonać poniższe kroki, aby dodać konfigurację typu punkt-lokacja do bramy sieci VPN w konfiguracji współistnienia.

1. Dodaj pulę adresów klienta sieci VPN. 

        $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
        Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"

2. Przekaż certyfikat główny sieci VPN do platformy Azure dla bramy sieci VPN. W tym przykładzie zakłada się, że certyfikat główny jest przechowywany na komputerze lokalnym, na którym są uruchamiane następujące polecenia cmdlet programu PowerShell. 

        $p2sCertFullName = "RootErVpnCoexP2S.cer"
        $p2sCertMatchName = "RootErVpnCoexP2S"
        $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName}
        if ($p2sCertToUpload.count -eq 1){
            write-host "cert found"
        } else {
            write-host "cert not found"
            exit
        } 
        $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData)
        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData

Więcej informacji na temat sieci VPN typu punkt-lokacja znajduje się w artykule [Configure a Point-to-Site connection](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md) (Konfigurowanie połączenia typu punkt-lokacja).

## Następne kroki

Więcej informacji na temat usługi ExpressRoute znajduje się w artykule [ExpressRoute FAQ](expressroute-faqs.md) (Usługa ExpressRoute — często zadawane pytania).



<!--HONumber=sep16_HO1-->


