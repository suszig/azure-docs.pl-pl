<properties
   pageTitle="Łączenie sieci wirtualnych platformy Azure przy użyciu usługi VPN Gateway i programu PowerShell | Microsoft Azure"
   description="W tym artykule przedstawiono procedurę łączenia sieci wirtualnych przy użyciu usługi Azure Resource Manager i programu PowerShell."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>


# Konfigurowanie połączenia między sieciami wirtualnymi usługi Resource Manager przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Resource Manager — program PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Model klasyczny — klasyczny portal](virtual-networks-configure-vnet-to-vnet-connection.md)

W tym artykule opisano kroki tworzenia połączenia między sieciami wirtualnymi w modelu wdrażania usługi Resource Manager przy użyciu usługi VPN Gateway. Sieci wirtualne mogą być zlokalizowane w tych samych lub różnych regionach i mogą funkcjonować w ramach tej samej lub różnych subskrypcji.


![Diagram połączenia między sieciami wirtualnymi (v2v)](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)


### Modele wdrażania i metody nawiązywania połączeń między sieciami wirtualnymi


[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Połączenie między sieciami wirtualnymi można skonfigurować w obu modelach wdrażania i przy użyciu kilku różnych narzędzi. Poniższą tabelę aktualizujemy w miarę pojawiania się nowych artykułów i dodatkowych narzędzi dla tej konfiguracji. Jeśli artykuł jest dostępny, można przejść do niego bezpośrednio z tabeli.<br><br>

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### Komunikacja równorzędna sieci wirtualnych

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## Informacje o połączeniach między sieciami wirtualnymi

Proces nawiązywania połączenia między dwiema sieciami wirtualnymi przebiega podobnie do procesu łączenia sieci wirtualnej z lokacją lokalną. Oba typy połączeń wykorzystują bramę sieci VPN na platformie Azure, aby zapewnić bezpieczny tunel z użyciem protokołu IPsec/IKE. Sieci wirtualne, które mają zostać połączone, mogą być zlokalizowane w różnych regionach. Mogę także należeć do różnych subskrypcji. Można także łączyć połączenia między sieciami wirtualnymi z konfiguracjami obejmującymi wiele lokacji. Pozwala to tworzyć topologie sieci, które łączą wdrożenia obejmujące wiele lokalizacji z połączeniami między sieciami wirtualnymi, jak pokazano na poniższym diagramie.


![Informacje o połączeniach](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)
 
### Dlaczego łączy się sieci wirtualne?

Sieci wirtualne można łączyć z następujących powodów:

- **Niezależna od regionu nadmiarowość i obecność geograficzna**
    - Można tworzyć własne replikacje geograficzne lub przeprowadzać synchronizację z bezpiecznym połączeniem bez przechodzenia przez punkty końcowe dostępne z Internetu.
    - Usługi Azure Traffic Manager i Load Balancer pozwalają skonfigurować obciążenie o wysokiej dostępności z nadmiarowością geograficzną w wielu regionach świadczenia usługi Azure. Istotnym przykładem jest konfiguracja ustawienia SQL Zawsze włączone, w przypadku którego grupy dostępności rozciągają się na wiele regionów świadczenia usługi Azure.

- **Regionalne aplikacje wielowarstwowe z izolacją lub granicami administracyjnymi**
    - W ramach jednego regionu można skonfigurować aplikacje wielowarstwowe z wielu połączonych ze sobą sieci wirtualnych, korzystając z izolacji lub wymagań administracyjnych.


### Często zadawane pytania dotyczące połączeń między sieciami wirtualnymi

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


## Która instrukcje mają zastosowanie w moim przypadku?

W tym artykule przedstawiono dwa różne zestawy kroków. Jeden zestaw dla [sieci wirtualnych znajdujących się w tej samej subskrypcji](#samesub), a drugi dla [sieci wirtualnych znajdujących się w różnych subskrypcjach](#difsub). Kluczowa różnica między wspomnianymi zestawami czynności polega na możliwości bądź braku możliwości konfiguracji i tworzenia wszystkich zasobów sieci wirtualnej i bramy w ramach tej samej sesji programu PowerShell.

Kroki opisane w tym artykule używają zmiennych, które są zadeklarowane na początku każdej sekcji. Jeśli pracujesz z istniejącymi sieciami wirtualnymi, zmodyfikuj zmienne, aby uwzględnić ustawienia we własnym środowisku. 

![Oba rodzaje połączeń](./media/vpn-gateway-vnet-vnet-rm-ps/differentsubscription.png)


## <a name="samesub"></a>Łączenie sieci wirtualnych, które należą do tej samej subskrypcji

![Diagram połączenia między sieciami wirtualnymi (v2v)](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### Przed rozpoczęciem
    
Niezbędne jest zainstalowanie poleceń cmdlet programu PowerShell usługi Azure Resource Manager. Aby uzyskać więcej informacji na temat instalowania poleceń cmdlet programu Azure PowerShell, zobacz artykuł [How to install and configure Azure PowerShell](../powershell-install-configure.md) (Instalowanie i konfigurowanie programu Azure PowerShell).

### <a name="Step1"></a>Krok 1 — planowanie zakresów adresów IP


W poniższych krokach utworzymy dwie sieci wirtualne wraz z odpowiednimi konfiguracjami oraz podsieciami bram. Następnie utworzymy połączenie sieci VPN między dwiema sieciami wirtualnymi. Ważne, aby zaplanować zakresy adresów IP dla konfiguracji sieci. Niezbędne jest upewnienie się, że zakresy sieci wirtualnej ani sieci lokalnej nie zachodzą na siebie w jakikolwiek sposób.

W przykładach stosujemy następujące wartości:

**Wartości dla sieci TestVNet1:**

- Nazwa sieci wirtualnej: TestVNet1
- Grupa zasobów: TestRG1
- Lokalizacja: Wschodnie stany USA
- TestVNet1: 10.11.0.0/16 & 10.12.0.0/16
- FrontEnd: 10.11.0.0/24
- BackEnd: 10.12.0.0/24
- GatewaySubnet: 10.12.255.0/27
- Serwer DNS: 8.8.8.8
- GatewayName: VNet1GW
- Publiczny adres IP: VNet1GWIP
- VPNType: RouteBased
- Connection(1to4): VNet1toVNet4
- Connection(1to5): VNet1toVNet5
- ConnectionType: VNet2VNet


**Wartości dla sieci TestVNet4:**

- Nazwa sieci wirtualnej: TestVNet4
- TestVNet2: 10.41.0.0/16 & 10.42.0.0/16
- FrontEnd: 10.41.0.0/24
- BackEnd: 10.42.0.0/24
- GatewaySubnet: 10.42.255.0/27
- Grupa zasobów: TestRG4
- Lokalizacja: West US
- Serwer DNS: 8.8.8.8
- GatewayName: VNet4GW
- Publiczny adres IP: VNet4GWIP
- VPNType: RouteBased
- Połączenie: VNet4toVNet1
- ConnectionType: VNet2VNet



### <a name="Step2"></a>Krok 2 — tworzenie i konfigurowanie sieci TestVNet1

1. Zadeklarowanie zmiennych

    Rozpocznij od zadeklarowania zmiennych. W poniższym przykładzie zmienne są deklarowane przy użyciu wartości podanych dla tego ćwiczenia. W większości przypadków należy zastąpić wartości własnymi. Można jednak użyć tych zmiennych, aby wykonać opisane kroki w celu zapoznania się z tego typu konfiguracją. Jeśli jest taka konieczność, zmodyfikuj zmienne, a następnie skopiuj je i wklej do konsoli programu PowerShell.

        $Sub1 = "Replace_With_Your_Subcription_Name"
        $RG1 = "TestRG1"
        $Location1 = "East US"
        $VNetName1 = "TestVNet1"
        $FESubName1 = "FrontEnd"
        $BESubName1 = "Backend"
        $GWSubName1 = "GatewaySubnet"
        $VNetPrefix11 = "10.11.0.0/16"
        $VNetPrefix12 = "10.12.0.0/16"
        $FESubPrefix1 = "10.11.0.0/24"
        $BESubPrefix1 = "10.12.0.0/24"
        $GWSubPrefix1 = "10.12.255.0/27"
        $DNS1 = "8.8.8.8"
        $GWName1 = "VNet1GW"
        $GWIPName1 = "VNet1GWIP"
        $GWIPconfName1 = "gwipconf1"
        $Connection14 = "VNet1toVNet4"
        $Connection15 = "VNet1toVNet5"

2. Nawiązywanie połączenia z subskrypcją

    Przełącz program PowerShell do trybu umożliwiającego korzystanie z poleceń cmdlet usługi Resource Manager. Otwórz konsolę programu PowerShell i połącz się ze swoim kontem. Użyj poniższego przykładu w celu łatwiejszego nawiązania połączenia:

        Login-AzureRmAccount

    Sprawdź subskrypcje dostępne na koncie.

        Get-AzureRmSubscription 

    Wskaż subskrypcję, której chcesz użyć.

        Select-AzureRmSubscription -SubscriptionName $Sub1

3. Utworzenie nowej grupy zasobów

        New-AzureRmResourceGroup -Name $RG1 -Location $Location1

4. Utworzenie konfiguracji podsieci dla sieci TestVNet1

    Poniższy przykład pozwala utworzyć sieć wirtualną o nazwie TestVNet1 oraz trzy podsieci noszące kolejno nazwy GatewaySubnet, FrontEnd i Backend. Podczas zastępowania wartości ważne jest, aby podsieć bramy zawsze nosiła nazwę GatewaySubnet. W przypadku nadania jej innej nazwy proces tworzenia bramy zakończy się niepowodzeniem. 

    W poniższym przykładzie użyto zmiennych, które zostały ustawione wcześniej. W tym przykładzie użyto podsieci bramy /27. Choć z technicznego punktu widzenia można utworzyć mniejszą podsieć bramy wykorzystującą wartość /29, to nie zaleca się takiego postępowania. Zalecamy użycie większych rozmiarów, takich jak /27 lub /26. W ten sposób będzie można korzystać z istniejących i przyszłych konfiguracji, które mogą wymagać większych podsieci bramy. 

        $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
        $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
        $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1


5. Utworzenie sieci TestVNet1

        New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

6. Przesłanie żądania dotyczącego publicznego adresu IP

    Następnie zostaje przesłane żądanie przydzielenia publicznego adresu IP do bramy, która zostanie utworzona dla sieci wirtualnej. Należy zauważyć, że metoda AllocationMethod jest dynamiczna. Nie możesz określić adresu IP, którego chcesz użyć. Jest on przydzielany dynamicznie do bramy. 

        $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AllocationMethod Dynamic

7. Utworzenie konfiguracji bramy

    W ramach konfiguracji bramy zostaje zdefiniowana podsieć i publiczny adres IP do użycia. Poniższy przykład umożliwia utworzenie konfiguracji bramy. 

        $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
        $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
        $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
        -Subnet $subnet1 -PublicIpAddress $gwpip1

8. Utworzenie bramy dla sieci TestVNet1

    W tym kroku zostaje utworzona brama sieci wirtualnej TestVNet1. Konfiguracje połączeń między sieciami wirtualnymi wymagają zastosowania wartości RouteBased obiektu VpnType. Tworzenie bramy może potrwać co najmniej 45 minut.

        New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
        -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### Krok 3 — tworzenie i konfigurowanie sieci TestVNet4

Po skonfigurowaniu sieci TestVNet1 utwórz sieć TestVNet4. Wykonaj kroki opisane poniżej, w razie potrzeby zastępując podane wartości własnymi. Ten krok można przeprowadzić w tej samej sesji programu PowerShell ze względu na przynależność do tej samej subskrypcji.

1. Zadeklarowanie zmiennych

    Należy pamiętać o zastąpieniu przykładowych wartości tymi, które mają zostać użyte w danej konfiguracji.

        $RG4 = "TestRG4"
        $Location4 = "West US"
        $VnetName4 = "TestVNet4"
        $FESubName4 = "FrontEnd"
        $BESubName4 = "Backend"
        $GWSubName4 = "GatewaySubnet"
        $VnetPrefix41 = "10.41.0.0/16"
        $VnetPrefix42 = "10.42.0.0/16"
        $FESubPrefix4 = "10.41.0.0/24"
        $BESubPrefix4 = "10.42.0.0/24"
        $GWSubPrefix4 = "10.42.255.0/27"
        $DNS4 = "8.8.8.8"
        $GWName4 = "VNet4GW"
        $GWIPName4 = "VNet4GWIP"
        $GWIPconfName4 = "gwipconf4"
        $Connection41 = "VNet4toVNet1"

    Przed kontynuowaniem upewnij się, że nadal masz połączenie z subskrypcją 1.

2. Utworzenie nowej grupy zasobów

        New-AzureRmResourceGroup -Name $RG4 -Location $Location4

3. Utworzenie konfiguracji podsieci dla sieci TestVNet4

        $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
        $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
        $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4

4. Utworzenie sieci TestVNet4

        New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4

5. Przesłanie żądania dotyczącego publicznego adresu IP

        $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AllocationMethod Dynamic

6. Utworzenie konfiguracji bramy

        $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
        $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
        $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4

7. Utworzenie bramy sieci TestVNet4

        New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
        -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### Krok 4 — połączenie bram

1. Użycie obu bram sieci wirtualnej

    Jako że obie bramy należą do tej samej subskrypcji, w tym przykładzie omawiany krok można wykonać w ramach tej samej sesji programu PowerShell.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
        $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4


2. Utworzenie połączenia z sieci wirtualnej TestVNet1 do sieci wirtualnej TestVNet4

    W tym kroku zostanie utworzone połączenie z sieci wirtualnej TestVNet1 do sieci wirtualnej TestVNet4. W przykładach zastosowano odwołania do klucza współużytkowanego. Możesz wybrać własne wartości dla klucza współużytkowanego. Ważne jest, aby klucz współużytkowany był zgodny z obydwoma połączeniami. Tworzenie połączenia może nieco potrwać.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
        -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

3. Utworzenie połączenia sieci TestVNet4 i TestVNet1

    Ten krok jest podobny do powyższego, jednak w jego przypadku tworzy się połączenie od sieci TestVNet4 do sieci TestVNet1. Upewnij się, że klucze współdzielone są zgodne.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
        -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

    Po kilku minutach połączenie powinno zostać ustanowione.

4. Weryfikowanie połączenia. Zobacz sekcję [Weryfikowanie połączenia](#verify).


## <a name="difsub"></a>Łączenie sieci wirtualnych, które należą do różnych subskrypcji


![Diagram połączenia między sieciami wirtualnymi (v2v)](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

W tym scenariuszu nawiązywane jest połączenie między sieciami wirtualnymi TestVNet1 i TestVNet5. Sieci TestVNet1 i TestVNet5 znajdują się w innych subskrypcjach. Czynności opisane dla tej konfiguracji dodają dodatkowe połączenie między sieciami wirtualnymi, aby można było nawiązać połączenie między sieciami TestVNet1 i TestVNet5. 

W tym przykładzie różnica polega na tym, że w kontekście drugiej subskrypcji część czynności konfiguracyjnych należy wykonać w osobnej sesji programu PowerShell. Szczególnie, jeśli obie subskrypcje należą do różnych organizacji. 

Poniższe instrukcje stanowią ciąg dalszy kroków wymienionych powyżej. Należy wykonać [Krok 1](#Step1) i [Krok 2](#Step2), aby utworzyć i skonfigurować sieć TestVNet1 i bramę VPN Gateway dla tej sieci. Po ukończeniu kroków 1 i 2 kontynuuj krok 5, aby utworzyć sieć wirtualną TestVNet5.

### Krok 5 — sprawdzenie dodatkowych zakresów adresów IP

Należy upewnić się, że przestrzeń adresów IP nowej sieci wirtualnej o nazwie TestVNet5 nie nakłada się na żaden z zakresów sieci wirtualnych ani na żaden z zakresów bramy sieci lokalnej. 

W tym przykładzie sieci wirtualne mogą należeć do różnych organizacji. Na potrzeby tego ćwiczenia można zastosować następujące wartości dla sieci wirtualnej TestVNet5:

**Wartości dla sieci TestVNet5:**

- Nazwa sieci wirtualnej: TestVNet5
- Grupa zasobów: TestRG5
- Lokalizacja: Japan East
- TestVNet5: 10.51.0.0/16 & 10.52.0.0/16
- FrontEnd: 10.51.0.0/24
- BackEnd: 10.52.0.0/24
- GatewaySubnet: 10.52.255.0.0/27
- Serwer DNS: 8.8.8.8
- GatewayName: VNet5GW
- Publiczny adres IP: VNet5GWIP
- VPNType: RouteBased
- Połączenie: VNet5toVNet1
- ConnectionType: VNet2VNet

**Dodatkowe wartości dla sieci TestVNet1:**

- Połączenie: VNet1toVNet5


### Krok 6 — tworzenie i konfigurowanie sieci TestVNet5

Ten krok należy wykonać w kontekście nowej subskrypcji. Tę część procedury może wykonać administrator w innej organizacji, która jest właścicielem subskrypcji.

1. Zadeklarowanie zmiennych

    Należy pamiętać o zastąpieniu przykładowych wartości tymi, które mają zostać użyte w danej konfiguracji.

        $Sub5 = "Replace_With_the_New_Subcription_Name"
        $RG5 = "TestRG5"
        $Location5 = "Japan East"
        $VnetName5 = "TestVNet5"
        $FESubName5 = "FrontEnd"
        $BESubName5 = "Backend"
        $GWSubName5 = "GatewaySubnet"
        $VnetPrefix51 = "10.51.0.0/16"
        $VnetPrefix52 = "10.52.0.0/16"
        $FESubPrefix5 = "10.51.0.0/24"
        $BESubPrefix5 = "10.52.0.0/24"
        $GWSubPrefix5 = "10.52.255.0/27"
        $DNS5 = "8.8.8.8"
        $GWName5 = "VNet5GW"
        $GWIPName5 = "VNet5GWIP"
        $GWIPconfName5 = "gwipconf5"
        $Connection51 = "VNet5toVNet1"

2. Połączenie z subskrypcją 5

    Otwórz konsolę programu PowerShell i połącz się ze swoim kontem. Użyj poniższego przykładu w celu łatwiejszego nawiązania połączenia:

        Login-AzureRmAccount

    Sprawdź subskrypcje dostępne na koncie.

        Get-AzureRmSubscription 

    Wskaż subskrypcję, której chcesz użyć.

        Select-AzureRmSubscription -SubscriptionName $Sub5

3. Utworzenie nowej grupy zasobów

        New-AzureRmResourceGroup -Name $RG5 -Location $Location5

4. Utworzenie konfiguracji podsieci dla sieci TestVNet4
    
        $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
        $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
        $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5

5. Utworzenie sieci wirtualnej TestVNet5

        New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
        -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5

6. Przesłanie żądania dotyczącego publicznego adresu IP

        $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
        -Location $Location5 -AllocationMethod Dynamic


7. Utworzenie konfiguracji bramy

        $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
        $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
        $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5

8. Utworzenie bramy sieci TestVNet5

        New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
        -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard

### Krok 7 — połączenie bram

Jako że bramy należą do różnych subskrypcji, w tym przykładzie zastosowano rozbicie na dwie sesje programu PowerShell oznaczone jako [Subskrypcja 1] i [Subskrypcja 5].

1. **[Subskrypcja 1]** Pobranie bramy sieci wirtualnej dla Subskrypcji 1

    Pamiętaj o zalogowaniu się i połączeniu z subskrypcją 1.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

    Skopiuj dane wyjściowe z następujących elementów i wyślij je do administratora Subskrypcji 5 pocztą e-mail lub w inny sposób.

        $vnet1gw.Name
        $vnet1gw.Id

    Te dwa elementy mają wartości podobne do następujących przykładowych danych wyjściowych:

        PS D:\> $vnet1gw.Name
        VNet1GW
        PS D:\> $vnet1gw.Id
        /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW

2. **[Subskrypcja 5]** Pobranie bramy sieci wirtualnej dla Subskrypcji 5

    Pamiętaj o zalogowaniu się i połączeniu z subskrypcją 5.

        $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5

    Skopiuj dane wyjściowe z następujących elementów i wyślij je do administratora Subskrypcji 1 pocztą e-mail lub w inny sposób.

        $vnet5gw.Name
        $vnet5gw.Id

    Te dwa elementy mają wartości podobne do następujących przykładowych danych wyjściowych:

        PS C:\> $vnet5gw.Name
        VNet5GW
        PS C:\> $vnet5gw.Id
        /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW

3. **[Subskrypcja 1]** Utworzenie połączenia z sieci wirtualnej TestVNet1 do sieci wirtualnej TestVNet5

    W tym kroku zostanie utworzone połączenie z sieci wirtualnej TestVNet1 do sieci wirtualnej TestVNet5. Różnica polega na tym, że wartości $vnet5gw nie można pobrać w sposób bezpośredni, ponieważ należy ona do innej subskrypcji. Należy utworzyć nowy obiekt programu PowerShell z wartościami przekazywanymi z Subskrypcji 1 w sposób przedstawiony w powyższych krokach. Skorzystaj z poniższego przykładu. Zastąp nazwę, identyfikator i klucz współużytkowany własnymi wartościami. Ważne jest, aby klucz współużytkowany był zgodny z obydwoma połączeniami. Tworzenie połączenia może nieco potrwać.

    Połącz się z Subskrypcją 1. 
    
        $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet5gw.Name = "VNet5GW"
        $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
        $Connection15 = "VNet1toVNet5"
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

4. **[Subskrypcja 5]** Utworzenie połączenia z sieci wirtualnej TestVNet5 do sieci wirtualnej TestVNet1

    Ten krok jest podobny do powyższego, jednak w jego przypadku tworzy się połączenie z sieci TestVNet5 do sieci TestVNet1. Ten sam proces tworzenia obiektu programu PowerShell na podstawie wartości uzyskanych z Subskrypcji 1 ma zastosowanie także w tym przypadku. W tym kroku należy upewnić się, czy klucze współużytkowane są zgodne.

    Połącz się z Subskrypcją 5.

        $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet1gw.Name = "VNet1GW"
        $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

## <a name="verify"></a>Jak zweryfikować połączenie

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]


## Następne kroki

- Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Kroki opisano w sekcji [Tworzenie maszyny wirtualnej](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Informacje na temat protokołu BGP można znaleźć w artykułach [BGP Overview](vpn-gateway-bgp-overview.md) (Omówienie protokołu BGP) i [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (Konfigurowanie protokołu BGP). 




<!--HONumber=Oct16_HO1-->


