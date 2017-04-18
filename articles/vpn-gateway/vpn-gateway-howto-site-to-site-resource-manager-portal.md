---
title: "Łączenie sieci lokalnej sieci z siecią wirtualną platformy Azure: sieci VPN typu lokacja-lokacja: portal | Microsoft Docs"
description: "Kroki tworzenia połączenia IPsec z sieci lokalnej do sieci wirtualnej platformy Azure za pośrednictwem publicznego Internetu. Kroki te są pomocne podczas tworzenia obejmującego wiele lokalizacji połączenia bramy sieci VPN typu lokacja-lokacja za pomocą portalu."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 827a4db7-7fa5-4eaf-b7e1-e1518c51c815
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: d6f4caebeeced1286f24dd5fcb4f5fc7d8591785
ms.lasthandoff: 04/12/2017


---
# <a name="create-a-site-to-site-connection-in-the-azure-portal"></a>Tworzenie połączenia typu lokacja-lokacja w witrynie Azure Portal

Połączenie bramy sieci VPN typu lokacja-lokacja to połączenie nawiązywane za pośrednictwem tunelu sieci VPN wykorzystującego protokół IPsec/IKE (IKEv1 lub IKEv2). Ten typ połączenia wymaga, aby urządzenie sieci VPN znajdowało się lokalnie, miało przypisany publiczny adres IP i nie znajdowało się za translatorem adresów sieciowych. Z połączeń typu lokacja-lokacja można korzystać w ramach konfiguracji hybrydowych i obejmujących wiele lokalizacji.

![Diagram połączenia bramy VPN Gateway typu lokacja-lokacja obejmującego wiele lokalizacji](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

W tym artykule opisano proces tworzenia sieci wirtualnej i połączenia bramy sieci VPN typu lokacja-lokacja z siecią lokalną z wykorzystaniem modelu wdrażania usługi Azure Resource Manager i witryny Azure Portal. Tę konfigurację można również utworzyć przy użyciu innych narzędzi wdrażania lub, w przypadku klasycznego modelu wdrażania, wybierając inną opcję z następującej listy:

> [!div class="op_single_selector"]
> * [Resource Manager — witryna Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager — program PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Klasyczny — witryna Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klasyczny — klasyczny portal](vpn-gateway-site-to-site-create.md)
>
>


#### <a name="additional-configurations"></a>Dodatkowe konfiguracje
Jeśli chcesz połączyć ze sobą sieci wirtualne, nie tworzysz jednak połączenia z lokalizacją lokalną, zapoznaj się z artykułem [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) (Konfigurowanie połączenia między sieciami wirtualnymi). Jeśli chcesz dodać połączenie lokacja-lokacja do sieci wirtualnej, z którą istnieje już połączenie, zobacz [Add a S2S connection to a VNet with an existing VPN gateway connection](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) (Dodawanie połączenia lokacja-lokacja do sieci wirtualnej, z którą istnieje połączenie bramy sieci VPN).

## <a name="before-you-begin"></a>Przed rozpoczęciem

[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Przed rozpoczęciem konfiguracji sprawdź, czy dysponujesz następującymi elementami.

* Zgodne urządzenie sieci VPN i osoba, która umie je skonfigurować. Zobacz artykuł [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Urządzenia sieci VPN — informacje).
* Jeśli nie znasz przestrzeni adresowych IP w swojej sieci lokalnej, skontaktuj się z osobą, która może podać Ci te dane. Połączenia typu lokacja-lokacja nie mogą mieć nakładających się przestrzeni adresowych.
* Dostępny zewnętrznie publiczny adres IP urządzenia sieci VPN. Ten adres IP nie może się znajdować za translatorem adresów sieciowych.
* Subskrypcja platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) lub utworzyć [bezpłatne konto](http://azure.microsoft.com/pricing/free-trial).

### <a name="values"></a>Przykładowe wartości
Korzystając z tych kroków w charakterze ćwiczenia, można użyć następujących wartości przykładowych:

* **Nazwa sieci wirtualnej:** TestVNet1
* **Przestrzeń adresowa:** 
    * 10.11.0.0/16
    * 10.12.0.0/16 (opcjonalnie na potrzeby tego ćwiczenia)
* **Podsieci:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24 (opcjonalnie na potrzeby tego ćwiczenia)
  * GatewaySubnet: 10.11.255.0/27
* **Grupa zasobów:** TestRG1
* **Lokalizacja:** Wschodnie stany USA
* **Serwer DNS:** adres IP serwera DNS
* **Nazwa bramy sieci wirtualnej:** VNet1GW
* **Publiczny adres IP:** VNet1GWIP
* **Typ sieci VPN:** oparta na trasach
* **Typ połączenia:** lokacja-lokacja (IPsec)
* **Typ bramy:** VPN
* **Nazwa bramy sieci lokalnej:** Site2
* **Nazwa połączenia:** VNet1toSite2

## <a name="CreatVNet"></a>1. Tworzenie sieci wirtualnej

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-s2s-rm-portal-include.md)]

## <a name="dns"></a>2. Określanie serwera DNS
Serwer DNS nie jest wymagany w przypadku połączeń typu lokacja-lokacja. Jeśli jednak chcesz korzystać z funkcji rozpoznawania nazw dla zasobów, które zostały wdrożone w Twojej sieci wirtualnej, określ serwer DNS. To ustawienie umożliwia określenie serwera DNS, który ma być używany do rozpoznawania nazw dla tej sieci wirtualnej. Nie powoduje ono jednak utworzenia serwera DNS.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>3. Tworzenie podsieci bramy
Musisz utworzyć podsieć dla bramy sieci VPN. Podsieć bramy zawiera adresy IP, z których korzystają usługi bramy sieci VPN. Jeśli to możliwe, utwórz podsieć bramy przy użyciu bloku CIDR /28 lub /27. Zapewni to wystarczającą liczbę adresów IP na potrzeby możliwych przyszłych funkcji bramy.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-s2s-rm-portal-include.md)]

## <a name="VNetGateway"></a>4. Tworzenie bramy sieci wirtualnej

[!INCLUDE [vpn-gateway-add-gw-s2s-rm-portal](../../includes/vpn-gateway-add-gw-s2s-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>5. Tworzenie bramy sieci lokalnej
Brama sieci lokalnej odwołuje się do lokalizacji lokalnej. Ustawienia określane dla bramy sieci lokalnej definiują przestrzeń adresową kierowaną do Twojego lokalnego urządzenia sieci VPN.

[!INCLUDE [vpn-gateway-add-lng-s2s-rm-portal](../../includes/vpn-gateway-add-lng-s2s-rm-portal-include.md)]

## <a name="VPNDevice"></a>6. Konfiguracja urządzenia sieci VPN
[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>7. Tworzenie połączenia sieci VPN typu lokacja-lokacja

W tym kroku zostanie utworzone połączenie sieci VPN typu lokacja-lokacja między bramą sieci wirtualnej i lokalnym urządzeniem sieci VPN. Przed rozpoczęciem wykonywania czynności opisanych w tej sekcji należy sprawdzić, czy zakończono tworzenie bramy sieci wirtualnej i bram sieci lokalnej.

[!INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include.md)]

## <a name="VerifyConnection"></a>8. Sprawdzenie połączenia sieci VPN

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="next-steps"></a>Następne kroki
*  Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne).
*  Informacje na temat protokołu BGP można znaleźć w artykułach [BGP Overview](vpn-gateway-bgp-overview.md) (Omówienie protokołu BGP) i [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (Konfigurowanie protokołu BGP).


