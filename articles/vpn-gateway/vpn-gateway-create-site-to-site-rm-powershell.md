<properties
   pageTitle="Tworzenie sieci wirtualnej za pomocą połączenia sieci VPN typu lokacja-lokacja przy użyciu usługi Azure Resource Manager i programu PowerShell | Microsoft Azure"
   description="W tym artykule opisano procedurę tworzenia sieci wirtualnej za pomocą modelu usługi Azure Resource Manager i jej łączenia z siecią lokalną przy użyciu połączenia bramy sieci VPN S2S."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/13/2016"
   ms.author="cherylmc"/>

# Tworzenie sieci wirtualnej za pomocą połączenia sieci VPN typu lokacja-lokacja przy użyciu programu PowerShell i usługi Azure Resource Manager

> [AZURE.SELECTOR]
- [Portal Azure](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Klasyczny portal Azure](vpn-gateway-site-to-site-create.md)
- [Program PowerShell — model usługi Resource Manager](vpn-gateway-create-site-to-site-rm-powershell.md)

W tym artykule opisano proces tworzenia wirtualnej sieci i połączenia sieci VPN typu lokacja-lokacja z siecią lokalną z wykorzystaniem modelu wdrażania przy użyciu usługi Azure Resource Manager. Z połączeń typu lokacja-lokacja można korzystać w ramach konfiguracji hybrydowych i obejmujących wiele lokalizacji. 


**Modele wdrażania Azure — informacje**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## Diagram połączenia 

![Diagram połączenia typu lokacja-lokacja](./media/vpn-gateway-create-site-to-site-rm-powershell/site2site.png "site-to-site")

**Modele wdrażania i narzędzia używane w kontekście połączeń typu lokacja-lokacja**

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

Jeśli chcesz połączyć ze sobą sieci wirtualne, nie tworzysz jednak połączenia z lokalizacją lokalną, zapoznaj się z artykułem [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) (Konfigurowanie połączenia między sieciami wirtualnymi). Jeśli potrzebujesz innego typu konfiguracji połączenia, zobacz artykuł [VPN Gateway connection topologies](vpn-gateway-topology.md) (Topologie połączenia bramy sieci VPN).


## Przed rozpoczęciem

Przed rozpoczęciem konfiguracji sprawdź, czy dysponujesz następującymi elementami.

- Zgodne urządzenie sieci VPN i osoba, która umie je skonfigurować. Zobacz artykuł [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Urządzenia sieci VPN — informacje). Jeśli nie dysponujesz wiedzą niezbędną do skonfigurowania urządzenia sieci VPN lub nie znasz zakresu adresów IP ujętego w konfiguracji sieci lokalnej, skontaktuj się z osobą, która dysponuje tymi danymi.

- Zewnętrzny publiczny adres IP urządzenia sieci VPN. Ten adres IP nie może się znajdować za translatorem adresów sieciowych.
    
- Subskrypcja platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).
    
- Niezbędne jest zainstalowanie najnowszej wersji poleceń cmdlet programu PowerShell usługi Azure Resource Manager. Aby uzyskać więcej informacji na temat instalowania poleceń cmdlet programu Azure PowerShell, zobacz artykuł [How to install and configure Azure PowerShell](../powershell-install-configure.md) (Instalowanie i konfigurowanie programu Azure PowerShell).


## 1. Nawiązywanie połączenia z subskrypcją 

Upewnij się, że program PowerShell został przełączony do trybu umożliwiającego korzystanie z poleceń cmdlet usługi Resource Manager. Więcej informacji znajduje się w temacie [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Używanie programu Windows PowerShell z usługą Resource Manager).

Otwórz konsolę programu PowerShell i połącz się ze swoim kontem. Użyj poniższego przykładu w celu łatwiejszego nawiązania połączenia:

    Login-AzureRmAccount

Sprawdź subskrypcje dostępne na koncie.

    Get-AzureRmSubscription 

Wskaż subskrypcję, której chcesz użyć.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## 2. Tworzenie sieci wirtualnej i podsieci bramy

W poniższych przykładach pokazano podsieć bramy /28. Choć jest możliwe utworzenie małej podsieci bramy (/29), nie zaleca się korzystania z takiego rozwiązania. Zaleca się utworzenie podsieci bramy /27 lub większej (/26, /25 itp.) w celu uwzględnienia wymagań dodatkowych funkcji. 

Jeśli masz już sieć wirtualną z podsiecią bramy /29 lub większą, możesz przejść od razu do kroku [Dodawanie bramy sieci lokalnej](#localnet).


[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]  

### Aby utworzyć sieć wirtualnej i podsieć bramy

Użyj poniższego przykładu, aby utworzyć sieć wirtualną i podsieć bramy. Zastąp poszczególne wartości własnymi. 

Najpierw utwórz grupę zasobów:
    
    New-AzureRmResourceGroup -Name testrg -Location 'West US'

Następnie utwórz sieć wirtualną. Sprawdź, czy określone przestrzenie adresowe nie nakładają się na żadne inne przestrzenie adresowe w obrębie sieci lokalnej.

Poniższy przykład pozwala utworzyć sieć wirtualną o nazwie *testvnet* oraz dwie podsieci, których nazwy brzmią *GatewaySubnet* i *Subnet1*. Ważne jest, aby jedna z utworzonych podsieci miała nazwę *GatewaySubnet*. W przypadku nadania podsieci innej nazwy proces konfigurowania połączenia zakończy się niepowodzeniem. 

    $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'
    New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="gatewaysubnet"></a>Aby dodać podsieć bramy do utworzonej wcześniej sieci wirtualnej

Ten krok jest wymagany tylko w przypadku, gdy niezbędne jest dodanie podsieci bramy do utworzonej wcześniej sieci wirtualnej.

Podsieć bramy można utworzyć, korzystając z poniższego przykładu. Należy pamiętać o nadaniu podsieci bramy nazwy „GatewaySubnet”. Jeśli podsieci zostanie nadana inna nazwa, podsieć zostanie utworzona, ale usługa Azure nie będzie jej traktowała jako podsieci bramy.

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet
    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

Następnie ustaw konfigurację. 

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## 3. <a name="localnet"></a>Dodanie bramy sieci lokalnej

W sieci wirtualnej brama sieci lokalnej zazwyczaj odwołuje się do lokalizacji lokalnej. Nadasz tej lokalizacji nazwę, z której usługa Azure będzie korzystać, odwołując się do niej, a także określisz prefiks przestrzeni adresowej bramy sieci lokalnej. 

Usługa Azure będzie używać prefiksu adresu IP w celu określenia, który ruch danych należy skierować do lokalizacji lokalnej. Oznacza to, że niezbędne będzie określenie prefiksu dla każdego adresu, który ma zostać skojarzony z bramą sieci lokalnej. W przypadku zmian w sieci lokalnej prefiksy można łatwo zaktualizować. 

Korzystając z przykładów dla programu PowerShell, należy pamiętać o następujących kwestiach:
    
- *GatewayIPAddress* to adres IP lokalnego urządzenia sieci VPN. Urządzenie sieci VPN nie może znajdować się za translatorem adresów sieciowych. 
- *AddressPrefix* oznacza lokalną przestrzeń adresową.

Aby dodać bramę sieci lokalnej z pojedynczym prefiksem adresu:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Aby dodać bramę sieci lokalnej z wieloma prefiksami adresów:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### Aby zmodyfikować prefiksy adresów IP bramy sieci lokalnej

Zdarza się, że prefiksy bramy sieci lokalnej są zmieniane. Kroki, które należy wykonać w celu zmodyfikowania prefiksów adresów IP, zależą od tego, czy utworzono połączenie bramy sieci VPN. Zapoznaj się z sekcją [Modyfikowanie prefiksów adresów IP bramy sieci lokalnej](#modify) tego artykułu.


## 4. Żądanie publicznego adresu IP dla bramy sieci VPN

Następnie należy przesłać żądanie przydzielenia publicznego adresu IP bramie sieci VPN sieci wirtualnej Azure. Nie jest to ten sam adres IP, który został przypisany do urządzenia sieci VPN, lecz adres przypisany do samej bramy sieci VPN Azure. Nie można określić adresu IP, który ma zostać użyty; adres jest przydzielany bramie w sposób dynamiczny. Ten adres IP zostanie użyty podczas konfigurowania lokalnego urządzenia sieci VPN w celu nawiązania połączenia z bramą.

Użyj poniższego przykładu konfiguracji z programu PowerShell. Dla tego adresu należy wybrać dynamiczną metodę alokacji. 

    $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

>[AZURE.NOTE] Brama sieci VPN Azure dla modelu wdrażania przy użyciu usługi Resource Manager obsługuje obecnie tylko publiczne adresy IP z wykorzystaniem metody dynamicznej alokacji. Nie oznacza to jednak, że adres IP się zmienia. Jedyną sytuacją, w której ma miejsce zmiana adresu IP bramy sieci VPN Azure, jest usunięcie bramy i jej ponowne utworzenie. Publiczny adres IP bramy nie zmienia się w przypadku zmiany rozmiaru, zresetowania ani w przypadku przeprowadzania innych wewnętrznych czynności konserwacyjnych bądź uaktualnień bramy sieci VPN Azure.

## 5. Tworzenie konfiguracji adresowania IP bramy

W ramach konfiguracji bramy zostaje zdefiniowana podsieć i publiczny adres IP do użycia. Poniższy przykład umożliwia utworzenie konfiguracji bramy. 

    $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## 6. Tworzenie bramy sieci wirtualnej

W tym kroku zostanie utworzona brama sieci wirtualnej. Należy pamiętać, że tworzenie bramy może okazać się czasochłonnym procesem. Często trwa 20 minut lub dłużej. 

Wprowadź następujące wartości:

- Wartość *-GatewayType* dla konfiguracji lokacja-lokacja to *Vpn*. Typ bramy zawsze zależy od wdrażanej konfiguracji. Na przykład inne konfiguracje bramy mogą wymagać zastosowania wartości -GatewayType ExpressRoute. 

- Dla pozycji *-VpnType* określającej typ sieci VPN można wybrać opcję *RouteBased* (oparta na trasach; w dokumentacji używa się czasem określenia „brama dynamiczna”) lub *PolicyBased* (oparta na zasadach; w dokumentacji używa się czasem określenia „brama statyczna”). Więcej informacji o typach bram sieci VPN można znaleźć w artykule [About VPN Gateways](vpn-gateway-about-vpngateways.md#vpntype) (Informacje o bramach sieci VPN).
- Dla opcji *-GatewaySku* można wybrać wartość *Basic* (Podstawowa), *Standard* (Standardowa) lub *HighPerformance* (Wysoka wydajność).   

        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard

## 7. Konfiguracja urządzenia sieci VPN

Na tym etapie potrzebny jest publiczny adres IP bramy sieci wirtualnej do celów konfigurowania lokalnego urządzenia sieci VPN. Uzyskaj szczegółowe informacje dotyczące konfiguracji od producenta urządzenia. Zapoznaj się także z tematem [VPN Devices](vpn-gateway-about-vpn-devices.md) (Urządzenia sieci VPN), aby uzyskać więcej informacji.

Aby znaleźć publiczny adres IP bramy sieci wirtualnej, skorzystaj z poniższego przykładu:

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## 8. Tworzenie połączenia sieci VPN

Następnie należy utworzyć połączenie sieci VPN typu lokacja-lokacja między bramą sieci wirtualnej i urządzeniem sieci VPN. Przedstawione wartości należy zastąpić własnymi. Klucz wspólny musi odpowiadać wartości użytej podczas konfiguracji urządzenia sieci VPN. Należy pamiętać, że dla połączenia sieci typu lokacja-lokacja wartość `-ConnectionType` to *IPsec*. 

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

Po chwili zostanie nawiązane połączenie. 

## 9. Sprawdzenie połączenia sieci VPN

Istnieje kilka różnych sposobów sprawdzenia połączenia sieci VPN. Poniżej opisano sposób przeprowadzenia podstawowej weryfikacji za pomocą portalu Azure i programu PowerShell.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="modify"></a>Aby zmodyfikować prefiksy adresów IP bramy sieci lokalnej

Jeśli zajdzie potrzeba zmiany prefiksów bramy sieci lokalnej, należy wykonać czynności opisane poniżej.  Poniżej przedstawiono dwa zestawy instrukcji. Należy wybrać odpowiednie instrukcje w zależności od tego, czy utworzono już połączenie bramy sieci VPN. 

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]


## Następne kroki

- Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Kroki opisano w sekcji [Tworzenie maszyny wirtualnej](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

- Informacje na temat protokołu BGP można znaleźć w artykułach [BGP Overview](vpn-gateway-bgp-overview.md) (Omówienie protokołu BGP) i [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (Konfigurowanie protokołu BGP).




<!--HONumber=jun16_HO2-->


