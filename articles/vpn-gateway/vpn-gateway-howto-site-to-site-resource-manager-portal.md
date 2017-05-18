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
ms.date: 05/02/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: fea9796d8ca03351573f091dce41689743821cad
ms.contentlocale: pl-pl
ms.lasthandoff: 05/08/2017


---
# <a name="create-a-site-to-site-connection-in-the-azure-portal"></a>Tworzenie połączenia typu lokacja-lokacja w witrynie Azure Portal

Ten artykuł pokazuje, jak używać witryny Azure Portal do tworzenia połączenia bramy sieci VPN lokacja-lokacja z sieci lokalnej do sieci wirtualnej. Kroki podane w tym artykule mają zastosowanie do modelu wdrażania przy użyciu usługi Resource Manager. Tę konfigurację możesz również utworzyć przy użyciu innego narzędzia wdrażania lub modelu wdrażania, wybierając inną opcję z następującej listy:

> [!div class="op_single_selector"]
> * [Resource Manager — witryna Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager — program PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Resource Manager — interfejs wiersza polecenia](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Klasyczny — witryna Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klasyczny — klasyczny portal](vpn-gateway-site-to-site-create.md)
> 
>


Połączenie bramy sieci VPN typu lokacja-lokacja umożliwia łączenie sieci lokalnej z siecią wirtualną platformy Azure za pośrednictwem tunelu sieci VPN IPsec/IKE (IKEv1 lub IKEv2). Ten typ połączenia wymaga lokalnego urządzenia sieci VPN z przypisanym publicznym adresem IP dostępnym z zewnątrz. Więcej informacji o bramach sieci VPN można znaleźć w artykule [Informacje dotyczące bram sieci VPN](vpn-gateway-about-vpngateways.md).

![Diagram połączenia bramy VPN Gateway typu lokacja-lokacja obejmującego wiele lokalizacji](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem konfiguracji sprawdź, czy są spełnione następujące kryteria:

* Upewnij się, że chcesz pracować z modelem wdrażania przy użyciu usługi Resource Manager. [!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 
* Zgodne urządzenie sieci VPN i osoba, która umie je skonfigurować. Aby uzyskać więcej informacji o zgodnych urządzeniach sieci VPN i konfiguracji urządzeń, zobacz artykuł [Informacje o urządzeniach sieci VPN](vpn-gateway-about-vpn-devices.md).
* Dostępny zewnętrznie publiczny adres IPv4 urządzenia sieci VPN. Ten adres IP nie może się znajdować za translatorem adresów sieciowych.
* Jeśli nie znasz zakresów adresów IP w konfiguracji swojej sieci lokalnej, skontaktuj się z osobą, która może podać Ci te dane. Tworząc tę konfigurację, musisz określić prefiksy zakresu adresów IP, które platforma Azure będzie kierować do Twojej lokalizacji lokalnej. Żadna z podsieci sieci lokalnej nie może się nakładać na podsieci sieci wirtualnej, z którymi chcesz nawiązać połączenie. 

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

Brama sieci wirtualnej korzysta z podsieci bramy, która zawiera adresy IP używane przez usługi bramy sieci VPN. Podczas tworzenia podsieci bramy musi ona otrzymać nazwę „GatewaySubnet”. W przypadku nadania podsieci innej nazwy proces konfigurowania połączenia zakończy się niepowodzeniem.

Rozmiar określanej podsieci bramy zależy od konfiguracji bramy sieci VPN, którą chcesz utworzyć. Chociaż możliwe jest utworzenie podsieci bramy tak małej, jak /29, zaleca się wybranie większej podsieci /27 lub /28, aby zawierała więcej adresów. Zastosowanie większej podsieci bramy daje wystarczającą liczbę adresów IP, aby uwzględnić możliwe przyszłe konfiguracje.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-s2s-rm-portal-include.md)]


## <a name="VNetGateway"></a>4. Tworzenie bramy sieci VPN

[!INCLUDE [vpn-gateway-add-gw-s2s-rm-portal](../../includes/vpn-gateway-add-gw-s2s-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>5. Tworzenie bramy sieci lokalnej

Brama sieci lokalnej zazwyczaj odwołuje się do lokalizacji lokalnej. Nadaj lokacji nazwę, za pomocą której platforma Azure może odwołać się do niej, a następnie określ adres IP lokalnego urządzenia sieci VPN, z którym będzie tworzone połączenie. Określ również prefiksy adresów IP, które będą kierowane za pośrednictwem bramy sieci VPN do urządzenia sieci VPN. Określone prefiksy adresów są prefiksami znajdującymi się w Twojej sieci lokalnej. W przypadku zmian w sieci lokalnej prefiksy można łatwo zaktualizować.

[!INCLUDE [vpn-gateway-add-lng-s2s-rm-portal](../../includes/vpn-gateway-add-lng-s2s-rm-portal-include.md)]

## <a name="VPNDevice"></a>6. Konfiguracja urządzenia sieci VPN

Połączenia typu lokacja-lokacja z siecią lokalną wymagają urządzenia sieci VPN. W tym kroku konfigurowane jest urządzenie sieci VPN. Podczas konfigurowania urządzenia sieci VPN potrzebne będą:

- Klucz współużytkowany. To ten sam klucz współużytkowany, który jest określany podczas tworzenia połączenia sieci VPN typu lokacja-lokacja. W naszych przykładach używamy podstawowego klucza współużytkowanego. Zalecamy, aby do użycia wygenerować bardziej złożony klucz.
- Publiczny adres IP bramy sieci wirtualnej. Publiczny adres IP można wyświetlić za pomocą witryny Azure Portal, programu PowerShell lub interfejsu wiersza polecenia. Aby znaleźć publiczny adres IP bramy sieci VPN za pomocą witryny Azure Portal, przejdź do pozycji **Bramy sieci wirtualnej**, a następnie kliknij nazwę bramy.


[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>7. Tworzenie połączenia sieci VPN

Utwórz połączenie sieci VPN typu lokacja-lokacja między bramą sieci wirtualnej a lokalnym urządzeniem sieci VPN.

[!INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include.md)]

## <a name="VerifyConnection"></a>8. Sprawdzenie połączenia sieci VPN

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]


## <a name="next-steps"></a>Następne kroki

*  Informacje na temat protokołu BGP można znaleźć w artykułach [BGP Overview](vpn-gateway-bgp-overview.md) (Omówienie protokołu BGP) i [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (Konfigurowanie protokołu BGP).
*  Aby uzyskać informacje o wymuszonym tunelowaniu, zobacz [Informacje o wymuszonym tunelowaniu](vpn-gateway-forced-tunneling-rm.md)
*  Aby uzyskać informacje o połączeniach o wysokiej dostępności typu aktywne-aktywne, zobacz [Połączenia obejmujące wiele lokalizacji i połączenia między sieciami wirtualnymi o wysokiej dostępności](vpn-gateway-highlyavailable.md).
