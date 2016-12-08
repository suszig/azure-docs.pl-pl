---
title: "Tworzenie sieci wirtualnej za pomocą połączenia sieci VPN typu lokacja-lokacja przy użyciu usługi Azure Resource Manager i witryny Azure Portal | Microsoft Docs"
description: "Tworzenie sieci wirtualnej przy użyciu modelu wdrażania usługi Azure Resource Manager i nawiązywanie połączenia z siecią lokalną za pomocą połączenia typu lokacja-lokacja bramy sieci VPN."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 827a4db7-7fa5-4eaf-b7e1-e1518c51c815
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/14/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: d269d9a76ff4ccd973eee70d2d5b54a7262383ef
ms.openlocfilehash: f0491df77418c4d7c79beff87302b64ddc3fa9be


---
# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal"></a>Tworzenie sieci wirtualnej za pomocą połączenia typu lokacja-lokacja przy użyciu witryny Azure Portal
> [!div class="op_single_selector"]
> * [Resource Manager — witryna Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager — program PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Model klasyczny — klasyczny portal](vpn-gateway-site-to-site-create.md)
> 
> 

W tym artykule opisano proces tworzenia sieci wirtualnej i połączenia bramy sieci VPN typu lokacja-lokacja z siecią lokalną z wykorzystaniem modelu wdrażania usługi Azure Resource Manager i witryny Azure Portal. Z połączeń typu lokacja-lokacja można korzystać w ramach konfiguracji hybrydowych i obejmujących wiele lokalizacji.

![Diagram](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)

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
* Subskrypcja platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) lub utworzyć [bezpłatne konto](http://azure.microsoft.com/pricing/free-trial).

### <a name="a-namevaluesasample-configuration-values-for-this-exercise"></a><a name="values"></a>Przykładowe wartości konfiguracji dla tego ćwiczenia
Korzystając z tych kroków w charakterze ćwiczenia, można użyć przykładowych wartości konfiguracji:

* **Nazwa sieci wirtualnej:** TestVNet1
* **Przestrzeń adresowa:** 10.11.0.0/16 i 10.12.0.0/16
* **Podsieci:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24
  * GatewaySubnet: 10.12.255.0/27
* **Grupa zasobów:** TestRG1
* **Lokalizacja:** Wschodnie stany USA
* **Serwer DNS:** 8.8.8.8
* **Nazwa bramy:** VNet1GW
* **Publiczny adres IP:** VNet1GWIP
* **Typ sieci VPN:** oparta na trasach
* **Typ połączenia:** lokacja-lokacja (IPsec)
* **Typ bramy:** VPN
* **Nazwa bramy sieci lokalnej:** Site2
* **Nazwa połączenia:** VNet1toSite2

## <a name="a-namecreatvneta1-create-a-virtual-network"></a><a name="CreatVNet"></a>1. Tworzenie sieci wirtualnej
Jeśli masz już sieć wirtualną, sprawdź, czy ustawienia są zgodne z projektem bramy sieci VPN. Zwróć szczególną uwagę na wszelkie podsieci, które mogą pokrywać się z innymi sieciami. Obecność nakładających się podsieci spowoduje, że połączenie nie będzie działać prawidłowo. Jeśli sieć wirtualną skonfigurowano poprawnie, można rozpocząć wykonywanie kroków z sekcji [Określanie serwera DNS](#dns).

### <a name="to-create-a-virtual-network"></a>Aby utworzyć sieć wirtualną
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="a-namesubnetsa2-add-additional-address-space-and-subnets"></a><a name="subnets"></a>2. Dodawanie dodatkowej przestrzeni adresowej i podsieci
Po utworzeniu sieci wirtualnej można dodać do niej dodatkową przestrzeń adresową oraz podsieci.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="a-namednsa3-specify-a-dns-server"></a><a name="dns"></a>3. Określanie serwera DNS
### <a name="to-specify-a-dns-server"></a>Określenie serwera DNS
[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="a-namegatewaysubneta4-create-a-gateway-subnet"></a><a name="gatewaysubnet"></a>4. Tworzenie podsieci bramy
Przed połączeniem sieci wirtualnej z bramą należy najpierw utworzyć podsieć bramy sieci wirtualnej, z którą ma zostać nawiązane połączenie. Jeśli to możliwe, najlepiej utworzyć podsieć bramy przy użyciu bloku CIDR /28 lub /27 w celu zapewnienia wystarczającej liczby adresów IP, aby uwzględnić wymagania dotyczące dodatkowych przyszłych konfiguracji.

Jeśli tworzysz tę konfigurację w ramach ćwiczenia, użyj tych [wartości](#values) podczas tworzenia podsieci bramy.

### <a name="to-create-a-gateway-subnet"></a>Aby utworzyć podsieć bramy
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="a-namevnetgatewaya5-create-a-virtual-network-gateway"></a><a name="VNetGateway"></a>5. Tworzenie bramy sieci wirtualnej
Jeśli tworzysz tę konfigurację w ramach ćwiczenia, skorzystaj z [przykładowych wartości konfiguracji](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Aby utworzyć bramę sieci wirtualnej
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="a-namelocalnetworkgatewaya6-create-a-local-network-gateway"></a><a name="LocalNetworkGateway"></a>6. Tworzenie bramy sieci lokalnej
Brama sieci lokalnej zazwyczaj odwołuje się do lokalizacji lokalnej. Bramie sieci lokalnej należy nadać nazwę, za pomocą której platforma Azure będzie mogła się do niej odwoływać. 

Jeśli tworzysz tę konfigurację w ramach ćwiczenia, skorzystaj z [przykładowych wartości konfiguracji](#values).

### <a name="to-create-a-local-network-gateway"></a>Aby utworzyć bramę sieci lokalnej
[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="a-namevpndevicea7-configure-your-vpn-device"></a><a name="VPNDevice"></a>7. Konfiguracja urządzenia sieci VPN
[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="a-namecreateconnectiona8-create-a-site-to-site-vpn-connection"></a><a name="CreateConnection"></a>8. Tworzenie połączenia sieci VPN typu lokacja-lokacja
Następnie należy utworzyć połączenie sieci VPN typu lokacja-lokacja między bramą sieci wirtualnej i urządzeniem sieci VPN. Przedstawione wartości należy zastąpić własnymi. Klucz wspólny musi odpowiadać wartości użytej podczas konfiguracji urządzenia sieci VPN. 

Przed rozpoczęciem wykonywania czynności opisanych w tej sekcji należy sprawdzić, czy zakończono tworzenie bramy sieci wirtualnej i bram sieci lokalnej. Jeśli tworzysz tę konfigurację w ramach ćwiczenia, użyj tych [wartości](#values) podczas tworzenia połączenia.

### <a name="to-create-the-vpn-connection"></a>Aby utworzyć połączenie sieci VPN
[!INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## <a name="a-nameverifyconnectiona9-verify-the-vpn-connection"></a><a name="VerifyConnection"></a>9. Sprawdzenie połączenia sieci VPN
Połączenie sieci VPN można sprawdzić, korzystając z portalu lub z programu PowerShell.

[!INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Następne kroki
*  Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne).
*  Informacje na temat protokołu BGP można znaleźć w artykułach [BGP Overview](vpn-gateway-bgp-overview.md) (Omówienie protokołu BGP) i [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (Konfigurowanie protokołu BGP).




<!--HONumber=Nov16_HO4-->


