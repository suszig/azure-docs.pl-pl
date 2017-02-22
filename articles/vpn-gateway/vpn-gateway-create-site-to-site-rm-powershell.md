---
title: "Łączenie sieci lokalnej sieci z siecią wirtualną platformy Azure: sieci VPN typu lokacja-lokacja: PowerShell | Microsoft Docs"
description: "Kroki tworzenia połączenia IPsec z sieci lokalnej do sieci wirtualnej platformy Azure za pośrednictwem publicznego Internetu. Ta procedura jest pomocna podczas tworzenia połączenia usługi VPN Gateway typu lokacja-lokacja obejmującego wiele lokalizacji za pomocą programu PowerShell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fcc2fda5-4493-4c15-9436-84d35adbda8e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: eea00841708212789e14fa8717d83dd81d472bac
ms.openlocfilehash: dad78df8d841dd1d5e014a902fa541044743f7e4


---
# <a name="create-a-vnet-with-a-site-to-site-vpn-connection-using-powershell"></a>Tworzenie sieci wirtualnej za pomocą połączenia sieci VPN typu lokacja-lokacja przy użyciu programu PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager — witryna Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager — program PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Model klasyczny — klasyczny portal](vpn-gateway-site-to-site-create.md)
>
>

W tym artykule opisano proces tworzenia sieci wirtualnej i połączenia bramy sieci VPN typu lokacja-lokacja z siecią lokalną za pomocą modelu wdrażania przy użyciu usługi Azure Resource Manager. Z połączeń typu lokacja-lokacja można korzystać w ramach konfiguracji hybrydowych i obejmujących wiele lokalizacji.

![Diagram połączenia bramy VPN Gateway typu lokacja-lokacja obejmującego wiele lokalizacji](./media/vpn-gateway-create-site-to-site-rm-powershell/site-to-site-connection-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Metody i modele wdrażania używane w kontekście połączeń typu lokacja-lokacja
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

W poniższej tabeli przedstawiono metody i modele wdrażania aktualnie dostępne dla konfiguracji lokacja-lokacja. Jeśli jest dostępny artykuł z krokami konfiguracji, można przejść do niego bezpośrednio z tabeli.

[!INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Dodatkowe konfiguracje
Jeśli chcesz połączyć ze sobą sieci wirtualne, nie tworzysz jednak połączenia z lokalizacją lokalną, zapoznaj się z artykułem [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) (Konfigurowanie połączenia między sieciami wirtualnymi). Jeśli chcesz dodać połączenie lokacja-lokacja do sieci wirtualnej, z którą istnieje już połączenie, zobacz [Add a S2S connection to a VNet with an existing VPN gateway connection](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) (Dodawanie połączenia lokacja-lokacja do sieci wirtualnej, z którą istnieje połączenie bramy sieci VPN).

## <a name="before-you-begin"></a>Przed rozpoczęciem
Przed rozpoczęciem konfiguracji sprawdź, czy dysponujesz następującymi elementami.

* Zgodne urządzenie sieci VPN i osoba, która umie je skonfigurować. Zobacz artykuł [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Urządzenia sieci VPN — informacje). Jeśli nie dysponujesz wiedzą niezbędną do skonfigurowania urządzenia sieci VPN lub nie znasz zakresu adresów IP ujętego w konfiguracji sieci lokalnej, skontaktuj się z osobą, która może podać Ci te dane.
* Dostępny zewnętrznie publiczny adres IP urządzenia sieci VPN. Ten adres IP nie może się znajdować za translatorem adresów sieciowych.
* Subskrypcja platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial).
* Najnowsza wersja poleceń cmdlet programu PowerShell usługi Azure Resource Manager. Aby uzyskać więcej informacji na temat instalowania poleceń cmdlet programu Azure PowerShell, zobacz artykuł [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Instalowanie i konfigurowanie programu Azure PowerShell).

## <a name="a-namelogina1-connect-to-your-subscription"></a><a name="Login"></a>1. Nawiązywanie połączenia z subskrypcją
Upewnij się, że program PowerShell został przełączony do trybu umożliwiającego korzystanie z poleceń cmdlet usługi Resource Manager. Więcej informacji znajduje się w temacie [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Używanie programu Windows PowerShell z usługą Resource Manager).

Otwórz konsolę programu PowerShell i połącz się ze swoim kontem. Użyj poniższego przykładu w celu łatwiejszego nawiązania połączenia:

    Login-AzureRmAccount

Sprawdź subskrypcje dostępne na koncie.

    Get-AzureRmSubscription

Wskaż subskrypcję, której chcesz użyć.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="a-namevneta2-create-a-virtual-network-and-a-gateway-subnet"></a><a name="VNet"></a>2. Tworzenie sieci wirtualnej i podsieci bramy
W przykładach użyto podsieci bramy /28. Chociaż możliwe jest utworzenie małej podsieci bramy /29, zaleca się wybranie podsieci przynajmniej /28 lub /27, aby zawierała więcej adresów. Zapewni to wystarczająco dużo adresów, aby możliwe były dodatkowe konfiguracje, które mogą być potrzebne w przyszłości.

Jeśli masz już sieć wirtualną z podsiecią bramy /29 lub większą, możesz przejść od razu do kroku [Dodawanie bramy sieci lokalnej](#localnet).

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>Aby utworzyć sieć wirtualnej i podsieć bramy
Użyj poniższego przykładu, aby utworzyć sieć wirtualną i podsieć bramy. Zastąp poszczególne wartości własnymi.

Najpierw utwórz grupę zasobów:

    New-AzureRmResourceGroup -Name testrg -Location 'West US'

Następnie utwórz sieć wirtualną. Sprawdź, czy określone przestrzenie adresowe nie nakładają się na żadne inne przestrzenie adresowe w obrębie sieci lokalnej.

Poniższy przykład pozwala utworzyć sieć wirtualną o nazwie *testvnet* oraz dwie podsieci, których nazwy brzmią *GatewaySubnet* i *Subnet1*. Ważne jest, aby jedna z utworzonych podsieci miała nazwę *GatewaySubnet*. W przypadku nadania podsieci innej nazwy proces konfigurowania połączenia zakończy się niepowodzeniem.

Ustaw zmienne.

    $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'

Utwórz sieć wirtualną.

    New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
    -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="a-namegatewaysubnetato-add-a-gateway-subnet-to-a-virtual-network-you-have-already-created"></a><a name="gatewaysubnet"></a>Aby dodać podsieć bramy do utworzonej wcześniej sieci wirtualnej
Ten krok jest wymagany tylko w przypadku, gdy niezbędne jest dodanie podsieci bramy do utworzonej wcześniej sieci wirtualnej.

Podsieć bramy można utworzyć, korzystając z poniższego przykładu. Należy pamiętać o nadaniu podsieci bramy nazwy „GatewaySubnet”. Jeśli zostanie nadana inna nazwa, podsieć zostanie utworzona, ale platforma Azure nie będzie jej traktowała jako podsieci bramy.

Ustaw zmienne.

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet

Utwórz podsieć bramy.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

Ustaw konfigurację.

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## <a name="3-a-namelocalnetaadd-your-local-network-gateway"></a>3. <a name="localnet"></a>Dodanie bramy sieci lokalnej
W sieci wirtualnej brama sieci lokalnej zazwyczaj odwołuje się do lokalizacji lokalnej. Należy tej lokalizacji nadać nazwę, z której platforma Azure będzie korzystać, odwołując się do niej, a także określić prefiks przestrzeni adresowej bramy sieci lokalnej.

Platforma Azure używa prefiksu adresu IP w celu określenia, który ruch danych należy skierować do lokalizacji lokalnej. Oznacza to, że niezbędne jest określenie prefiksu dla każdego adresu, który ma zostać skojarzony z bramą sieci lokalnej. W przypadku zmian w sieci lokalnej prefiksy można łatwo zaktualizować.

Korzystając z przykładów dla programu PowerShell, należy pamiętać o następujących kwestiach:

* *GatewayIPAddress* to adres IP lokalnego urządzenia sieci VPN. Urządzenie sieci VPN nie może znajdować się za translatorem adresów sieciowych.
* *AddressPrefix* oznacza lokalną przestrzeń adresową.

Aby dodać bramę sieci lokalnej z pojedynczym prefiksem adresu:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Aby dodać bramę sieci lokalnej z wieloma prefiksami adresów:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### <a name="to-modify-ip-address-prefixes-for-your-local-network-gateway"></a>Aby zmodyfikować prefiksy adresów IP bramy sieci lokalnej
Zdarza się, że prefiksy bramy sieci lokalnej są zmieniane. Kroki, które należy wykonać w celu zmodyfikowania prefiksów adresów IP, zależą od tego, czy utworzono połączenie bramy sieci VPN. Zapoznaj się z sekcją [Modyfikowanie prefiksów adresów IP bramy sieci lokalnej](#modify) tego artykułu.

## <a name="a-namepublicipa4-request-a-public-ip-address-for-the-vpn-gateway"></a><a name="PublicIP"></a>4. Żądanie publicznego adresu IP dla bramy sieci VPN
Następnie należy przesłać żądanie przydzielenia publicznego adresu IP do bramy sieci VPN sieci wirtualnej platformy Azure. Nie jest to ten sam adres IP, który został przypisany do urządzenia sieci VPN, lecz adres przypisany do samej bramy sieci VPN Azure. Nie można określić adresu IP, którego chcesz użyć. Jest on przydzielany dynamicznie do bramy. Podany adres IP zostanie użyty podczas konfigurowania lokalnego urządzenia sieci VPN w celu nawiązania połączenia z bramą.

Brama sieci VPN Azure dla modelu wdrażania przy użyciu usługi Resource Manager obsługuje obecnie tylko publiczne adresy IP z wykorzystaniem metody dynamicznej alokacji. Nie oznacza to jednak, że adres IP się zmienia. Jedyną sytuacją, w której ma miejsce zmiana adresu IP bramy sieci VPN Azure, jest usunięcie bramy i jej ponowne utworzenie. Publiczny adres IP bramy nie zmienia się w przypadku zmiany rozmiaru, zresetowania ani w przypadku przeprowadzania innych wewnętrznych czynności konserwacyjnych bądź uaktualnień bramy sieci VPN platformy Azure.

Użyj poniższego przykładu z programu PowerShell:

    $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

## <a name="a-namegatewayipconfiga5-create-the-gateway-ip-addressing-configuration"></a><a name="GatewayIPConfig"></a>5. Tworzenie konfiguracji adresowania IP bramy
W ramach konfiguracji bramy zostaje zdefiniowana podsieć i publiczny adres IP do użycia. Poniższy przykład umożliwia utworzenie konfiguracji bramy.

    $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id

## <a name="a-namecreategatewaya6-create-the-virtual-network-gateway"></a><a name="CreateGateway"></a>6. Tworzenie bramy sieci wirtualnej
W tym kroku zostanie utworzona brama sieci wirtualnej. Należy pamiętać, że tworzenie bramy może okazać się czasochłonnym procesem. Często trwa 45 minut lub dłużej.

Wprowadź następujące wartości:

* Wartość *-GatewayType* dla konfiguracji lokacja-lokacja to *Vpn*. Typ bramy zawsze zależy od wdrażanej konfiguracji. Na przykład inne konfiguracje bramy mogą wymagać zastosowania wartości -GatewayType ExpressRoute.
* Dla pozycji *-VpnType* określającej typ sieci VPN można wybrać opcję *RouteBased* (oparta na trasach; w dokumentacji używa się czasem określenia „brama dynamiczna”) lub *PolicyBased* (oparta na zasadach; w dokumentacji używa się czasem określenia „brama statyczna”). Więcej informacji o typach bram sieci VPN można znaleźć w artykule [VPN Gateway — informacje](vpn-gateway-about-vpngateways.md).
* Dla opcji *-GatewaySku* można wybrać wartość *Basic* (Podstawowa), *Standard* (Standardowa) lub *HighPerformance* (Wysoka wydajność).     

        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
        -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

## <a name="a-nameconfigurevpndevicea7-configure-your-vpn-device"></a><a name="ConfigureVPNDevice"></a>7. Konfiguracja urządzenia sieci VPN
Na tym etapie potrzebny jest publiczny adres IP bramy sieci wirtualnej do celów konfigurowania lokalnego urządzenia sieci VPN. Uzyskaj szczegółowe informacje dotyczące konfiguracji od producenta urządzenia. Aby uzyskać więcej informacji, zapoznaj się z tematem [VPN Devices](vpn-gateway-about-vpn-devices.md) (Urządzenia sieci VPN).

Aby znaleźć publiczny adres IP bramy sieci wirtualnej, skorzystaj z poniższego przykładu:

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## <a name="a-namecreateconnectiona8-create-the-vpn-connection"></a><a name="CreateConnection"></a>8. Tworzenie połączenia sieci VPN
Następnie należy utworzyć połączenie sieci VPN typu lokacja-lokacja między bramą sieci wirtualnej i urządzeniem sieci VPN. Przedstawione wartości należy zastąpić własnymi. Klucz wspólny musi odpowiadać wartości użytej podczas konfiguracji urządzenia sieci VPN. Należy pamiętać, że dla połączenia sieci typu lokacja-lokacja wartość `-ConnectionType` to *IPsec*.

Ustaw zmienne.

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

Utwórz połączenie.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

Po chwili zostanie nawiązane połączenie.

## <a name="a-nametoverifyato-verify-a-vpn-connection"></a><a name="toverify"></a>Aby zweryfikować połączenie sieci VPN
Istnieje kilka różnych sposobów sprawdzenia połączenia sieci VPN.

[!INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="a-namemodifyato-modify-ip-address-prefixes-for-a-local-network-gateway"></a><a name="modify"></a>Aby zmodyfikować prefiksy adresów IP bramy sieci lokalnej
Jeśli zajdzie potrzeba zmiany prefiksów bramy sieci lokalnej, należy wykonać czynności opisane poniżej. Poniżej przedstawiono dwa zestawy instrukcji. Należy wybrać odpowiednie instrukcje w zależności od tego, czy utworzono już połączenie bramy sieci VPN.

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="a-namemodifygwipaddressato-modify-the-gateway-ip-address-for-a-local-network-gateway"></a><a name="modifygwipaddress"></a>Aby zmodyfikować adres IP bramy dla bramy sieci lokalnej
[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Następne kroki
*  Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne).
* Informacje na temat protokołu BGP można znaleźć w artykułach [BGP Overview](vpn-gateway-bgp-overview.md) (Omówienie protokołu BGP) i [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (Konfigurowanie protokołu BGP).



<!--HONumber=Jan17_HO4-->


