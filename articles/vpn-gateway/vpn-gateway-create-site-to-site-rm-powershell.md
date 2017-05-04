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
ms.date: 04/24/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: 4ec11ffeae94b4a8e5a65566f0f0c067f45a0134
ms.lasthandoff: 04/25/2017


---
# <a name="create-a-vnet-with-a-site-to-site-vpn-connection-using-powershell"></a>Tworzenie sieci wirtualnej za pomocą połączenia sieci VPN typu lokacja-lokacja przy użyciu programu PowerShell

Ten artykuł pokazuje, jak używać programu PowerShell do tworzenia połączenia bramy sieci VPN lokacja-lokacja z Twojej sieci lokalnej do sieci wirtualnej. Kroki podane w tym artykule mają zastosowanie do modelu wdrażania przy użyciu usługi Resource Manager. Tę konfigurację możesz również utworzyć przy użyciu innego narzędzia wdrażania lub modelu wdrażania, wybierając inną opcję z następującej listy:

> [!div class="op_single_selector"]
> * [Resource Manager — witryna Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager — program PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Resource Manager — interfejs wiersza polecenia](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Klasyczny — witryna Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klasyczny — klasyczny portal](vpn-gateway-site-to-site-create.md)
> 
>


![Diagram połączenia bramy VPN Gateway typu lokacja-lokacja obejmującego wiele lokalizacji](./media/vpn-gateway-create-site-to-site-rm-powershell/site-to-site-connection-diagram.png)

Połączenie bramy sieci VPN typu lokacja-lokacja umożliwia łączenie sieci lokalnej z siecią wirtualną platformy Azure za pośrednictwem tunelu sieci VPN IPsec/IKE (IKEv1 lub IKEv2). Ten typ połączenia wymaga lokalnego urządzenia sieci VPN z przypisanym publicznym adresem IP dostępnym z zewnątrz. Więcej informacji o bramach sieci VPN można znaleźć w artykule [Informacje dotyczące bram sieci VPN](vpn-gateway-about-vpngateways.md).

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem konfiguracji sprawdź, czy są spełnione następujące kryteria:

* Upewnij się, że chcesz pracować z modelem wdrażania przy użyciu usługi Resource Manager. [!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 
* Zgodne urządzenie sieci VPN i osoba, która umie je skonfigurować. Aby uzyskać więcej informacji o zgodnych urządzeniach sieci VPN i konfiguracji urządzeń, zobacz artykuł [Informacje o urządzeniach sieci VPN](vpn-gateway-about-vpn-devices.md).
* Dostępny zewnętrznie publiczny adres IPv4 urządzenia sieci VPN. Ten adres IP nie może się znajdować za translatorem adresów sieciowych.
* Jeśli nie znasz zakresów adresów IP w konfiguracji swojej sieci lokalnej, skontaktuj się z osobą, która może podać Ci te dane. Tworząc tę konfigurację, musisz określić prefiksy zakresu adresów IP, które platforma Azure będzie kierować do Twojej lokalizacji lokalnej. Żadna z podsieci sieci lokalnej nie może się nakładać na podsieci sieci wirtualnej, z którymi chcesz nawiązać połączenie.
* Najnowsza wersja poleceń cmdlet programu PowerShell usługi Azure Resource Manager. Aby uzyskać więcej informacji na temat instalowania poleceń cmdlet programu Azure PowerShell, zobacz artykuł [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Instalowanie i konfigurowanie programu Azure PowerShell).

### <a name="example-values"></a>Przykładowe wartości

W przykładach w tym artykule są stosowane następujące wartości. Tych wartości możesz użyć do tworzenia środowiska testowego lub odwoływać się do nich, aby lepiej zrozumieć przykłady w niniejszym artykule.

```
#Example values

VnetName                = testvnet 
ResourceGroup           = testrg 
Location                = West US 
AddressSpace            = 10.0.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.0.1.0/28 
GatewaySubnet           = 10.0.0.0/27
LocalNetworkGatewayName = LocalSite
LNG Public IP           = <VPN device IP address> 
Local Address Prefixes  = 10.0.0.0/24','20.0.0.0/24
Gateway Name            = vnetgw1
PublicIP                = gwpip
Gateway IP Config       = gwipconfig1 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = myGWConnection
```

## <a name="Login"></a>1. Nawiązywanie połączenia z subskrypcją
Upewnij się, że program PowerShell został przełączony do trybu umożliwiającego korzystanie z poleceń cmdlet usługi Resource Manager. Więcej informacji znajduje się w temacie [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Używanie programu Windows PowerShell z usługą Resource Manager).

1. Otwórz konsolę programu PowerShell i połącz się ze swoim kontem. Użyj poniższego przykładu w celu łatwiejszego nawiązania połączenia:

  ```powershell
  Login-AzureRmAccount
  ```
2. Sprawdź subskrypcje dostępne na koncie.

  ```powershell
  Get-AzureRmSubscription
  ```
3. Wskaż subskrypcję, której chcesz użyć.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```

## <a name="VNet"></a>2. Tworzenie sieci wirtualnej i podsieci bramy

Jeśli nie masz jeszcze sieci wirtualnej, utwórz ją. Podczas tworzenia sieci wirtualnej upewnij się, że określone przestrzenie adresowe nie nakładają się na żadne inne przestrzenie adresowe w obrębie sieci lokalnej. Dla tej konfiguracji potrzebna jest również podsieć bramy. Brama sieci wirtualnej korzysta z podsieci bramy, która zawiera adresy IP używane przez usługi bramy sieci VPN. Podczas tworzenia podsieci bramy musi ona otrzymać nazwę „GatewaySubnet”. Jeśli zostanie nadana inna nazwa, podsieć zostanie utworzona, ale platforma Azure nie będzie jej traktowała jako podsieci bramy.

Rozmiar określanej podsieci bramy zależy od konfiguracji bramy sieci VPN, którą chcesz utworzyć. Chociaż możliwe jest utworzenie podsieci bramy tak małej, jak /29, zaleca się wybranie większej podsieci /27 lub /28, aby zawierała więcej adresów. Zastosowanie większej podsieci bramy daje wystarczającą liczbę adresów IP, aby uwzględnić możliwe przyszłe konfiguracje.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>Aby utworzyć sieć wirtualną i podsieć bramy

Ten przykład tworzy sieć wirtualną i podsieć bramy. Jeśli masz już sieć wirtualną, do której potrzebujesz dodać podsieć bramy, zobacz [Aby dodać podsieć bramy do utworzonej wcześniej sieci wirtualnej](#gatewaysubnet).

Utwórz grupę zasobów:

```powershell
New-AzureRmResourceGroup -Name testrg -Location 'West US'
```

Utwórz swoją sieć wirtualną. 

1. Ustaw zmienne.

  ```powershell
  $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/27
  $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'
  ```
2. Utwórz sieć wirtualną.

  ```powershell
  New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
  -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2
  ```

### <a name="gatewaysubnet"></a>Aby dodać podsieć bramy do utworzonej wcześniej sieci wirtualnej

1. Ustaw zmienne.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet
  ```
2. Utwórz podsieć bramy.

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/27 -VirtualNetwork $vnet
  ```
3. Ustaw konfigurację.

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

## 3. <a name="localnet"></a>Tworzenie bramy sieci lokalnej

Brama sieci lokalnej zazwyczaj odwołuje się do lokalizacji lokalnej. Nadaj lokacji nazwę, za pomocą której platforma Azure może odwołać się do niej, a następnie określ adres IP lokalnego urządzenia sieci VPN, z którym będzie tworzone połączenie. Określ również prefiksy adresów IP, które będą kierowane za pośrednictwem bramy sieci VPN do urządzenia sieci VPN. Określone prefiksy adresów są prefiksami znajdującymi się w Twojej sieci lokalnej. W przypadku zmian w sieci lokalnej prefiksy można łatwo zaktualizować.

Wprowadź następujące wartości:

* *GatewayIPAddress* to adres IP lokalnego urządzenia sieci VPN. Urządzenie sieci VPN nie może znajdować się za translatorem adresów sieciowych.
* *AddressPrefix* oznacza lokalną przestrzeń adresową.

- Aby dodać bramę sieci lokalnej z pojedynczym prefiksem adresu:

  ```powershell
  New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
  -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix   '10.0.0.0/24'
  ```

- Aby dodać bramę sieci lokalnej z wieloma prefiksami adresów:

  ```powershell
  New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
  -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')
  ```

- Aby zmodyfikować prefiksy adresów IP bramy sieci lokalnej:<br>
Zdarza się, że prefiksy bramy sieci lokalnej są zmieniane. Kroki, które należy wykonać w celu zmodyfikowania prefiksów adresów IP, zależą od tego, czy utworzono połączenie bramy sieci VPN. Zapoznaj się z sekcją [Modyfikowanie prefiksów adresów IP bramy sieci lokalnej](#modify) tego artykułu.

## <a name="PublicIP"></a>4. Przesłanie żądania dotyczącego publicznego adresu IP

Prześlij żądanie przydzielenia publicznego adresu IP, który zostanie przypisany do Twojej bramy sieci VPN sieci wirtualnej. Jest to adres IP, do łączenia z którym jest konfigurowane Twoje urządzenie sieci VPN.

Brama sieci wirtualnej dla modelu wdrażania przy użyciu usługi Resource Manager obsługuje obecnie tylko publiczne adresy IP z wykorzystaniem metody dynamicznej alokacji. Nie oznacza to jednak, że adres IP się zmienia. Jedyną sytuacją, w której ma miejsce zmiana adresu IP bramy sieci VPN, jest usunięcie bramy i jej ponowne utworzenie. Publiczny adres IP bramy sieci wirtualnej nie zmienia się w przypadku zmiany rozmiaru, zresetowania ani w przypadku przeprowadzania innych wewnętrznych czynności konserwacyjnych bądź uaktualnień bramy sieci VPN.

Użyj poniższego przykładu z programu PowerShell:

```powershell
$gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic
```

## <a name="GatewayIPConfig"></a>5. Tworzenie konfiguracji adresowania IP bramy
W ramach konfiguracji bramy zostaje zdefiniowana podsieć i publiczny adres IP do użycia. Poniższy przykład umożliwia utworzenie własnej konfiguracji bramy:

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```

## <a name="CreateGateway"></a>6. Tworzenie bramy sieci VPN

Utwórz bramę sieci VPN sieci wirtualnej. Tworzenie bramy sieci VPN może potrwać 45 minut lub więcej.

Wprowadź następujące wartości:

* Wartość *-GatewayType* dla konfiguracji lokacja-lokacja to *Vpn*. Typ bramy zawsze zależy od wdrażanej konfiguracji. Na przykład inne konfiguracje bramy mogą wymagać zastosowania wartości -GatewayType ExpressRoute.
* Dla pozycji *-VpnType* określającej typ sieci VPN można wybrać opcję *RouteBased* (oparta na trasach; w dokumentacji używa się czasem określenia „brama dynamiczna”) lub *PolicyBased* (oparta na zasadach; w dokumentacji używa się czasem określenia „brama statyczna”). Więcej informacji o typach bram sieci VPN można znaleźć w artykule [VPN Gateway — informacje](vpn-gateway-about-vpngateways.md).
* Dla opcji *-GatewaySku* można wybrać wartość Basic (Podstawowa), Standard (Standardowa) lub HighPerformance (Wysoka wydajność). Dla niektórych jednostek SKU istnieją ograniczenia konfiguracji. Aby uzyskać więcej informacji, zobacz [Gateway SKUs](vpn-gateway-about-vpngateways.md#gateway-skus) (Jednostki SKU bramy).

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku Standard
```

## <a name="ConfigureVPNDevice"></a>7. Konfiguracja urządzenia sieci VPN

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

Aby znaleźć publiczny adres IP bramy sieci wirtualnej przy użyciu programu PowerShell, skorzystaj z poniższego przykładu:

```powershell
Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG
```

## <a name="CreateConnection"></a>8. Tworzenie połączenia sieci VPN
Następnie należy utworzyć połączenie sieci VPN typu lokacja-lokacja między bramą sieci wirtualnej i urządzeniem sieci VPN. Przedstawione wartości należy zastąpić własnymi. Klucz współużytkowany musi odpowiadać wartości użytej podczas konfiguracji urządzenia sieci VPN. Należy pamiętać, że dla połączenia typu lokacja-lokacja wartość parametru „-ConnectionType” to *IPsec*.

1. Ustaw zmienne.
  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
  $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
  ```

2. Utwórz połączenie.
  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName testrg `
  -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

Po chwili zostanie nawiązane połączenie.

## <a name="toverify"></a>9. Sprawdzenie połączenia sieci VPN
Istnieje kilka różnych sposobów sprawdzenia połączenia sieci VPN.

[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="modify"></a>Aby zmodyfikować prefiksy adresów IP bramy sieci lokalnej
Jeśli zajdzie potrzeba zmiany prefiksów bramy sieci lokalnej, należy wykonać czynności opisane poniżej. Poniżej przedstawiono dwa zestawy instrukcji. Należy wybrać odpowiednie instrukcje w zależności od tego, czy utworzono już połączenie bramy sieci VPN.

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Aby zmodyfikować adres IP bramy dla bramy sieci lokalnej
[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Następne kroki
*  Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne).
* Informacje na temat protokołu BGP można znaleźć w artykułach [BGP Overview](vpn-gateway-bgp-overview.md) (Omówienie protokołu BGP) i [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (Konfigurowanie protokołu BGP).

