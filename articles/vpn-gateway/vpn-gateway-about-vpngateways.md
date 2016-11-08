---
title: VPN Gateway — informacje | Microsoft Docs
description: Informacje na temat połączeń bramy sieci VPN dla sieci wirtualnych platformy Azure.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: ''
tags: azure-resource-manager,azure-service-management

ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/06/2016
ms.author: cherylmc

---
# VPN Gateway — informacje
Brama sieci wirtualnej służy do wysyłania ruchu sieciowego między sieciami wirtualnymi platformy Azure i lokalizacjami lokalnymi, a także między sieciami wirtualnymi na platformie Azure (połączenia między sieciami wirtualnymi). Podczas konfigurowania bramy VPN Gateway należy utworzyć i skonfigurować bramę sieci wirtualnej oraz połączenie bramy sieci wirtualnej.

W modelu wdrażania usługi Resource Manager podczas tworzenia zasobu bramy sieci wirtualnej można określić kilka ustawień. Jednym z wymaganych ustawień jest „-GatewayType” (typ bramy). Istnieją dwa typy bram sieci wirtualnej: Vpn i ExpressRoute. 

Jeśli ruch sieciowy jest przesyłany w dedykowanym połączeniu prywatnym, należy użyć typu bramy „ExpressRoute”. Ten typ bramy jest również określany jako brama usługi ExpressRoute. Gdy ruch sieciowy jest szyfrowany i przesyłany przez połączenie publiczne, należy użyć typu bramy „Vpn”. Ten typ bramy jest określany jako brama sieci VPN. Wszystkie połączenia typu lokacja-lokacja, punkt-lokacja i połączenia między sieciami wirtualnymi używają bramy sieci VPN.

Każda sieć wirtualna może mieć tylko jedną bramę sieci wirtualnej na typ bramy. Na przykład można mieć jedną bramę sieci wirtualnej, która używa klasy -GatewayType ExpressRoute, oraz jedną, która używa klasy -GatewayType Vpn. Ten artykuł koncentruje się głównie na bramie sieci VPN. Więcej informacji na temat usługi ExpressRoute zawiera artykuł [ExpressRoute technical overview](../expressroute/expressroute-introduction.md) (Opis techniczny usługi ExpressRoute).

## Cennik
[!INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)]

## Jednostki SKU bramy
[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

Sieci VPN typu PolicyBased można tworzyć tylko w przypadku bram korzystających z podstawowych jednostek SKU.
Więcej informacji o jednostkach SKU bramy zawiera artykuł [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku) (Jednostki SKU bramy).

W poniższej tabeli przedstawiono typy bram i szacowaną agregowaną przepływność. Ta tabela ma zastosowanie w obu modelach wdrażania — przy użyciu usługi Resource Manager i klasycznym.

[!INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

## Konfigurowanie bramy sieci VPN
Instrukcje używane podczas konfigurowania bramy sieci VPN będą zależeć od modelu wdrażania użytego w celu utworzenia sieci wirtualnej. Jeśli na przykład sieć wirtualna została utworzona przy użyciu klasycznego modelu wdrożenia, do tworzenia i konfigurowania ustawień bramy sieci VPN należy użyć wskazówek i instrukcji dotyczących klasycznego modelu wdrażania. Aby uzyskać więcej informacji na temat modeli wdrażania, zobacz [Omówienie modelu wdrażania przy użyciu usługi Resource Manager oraz wdrażania klasycznego](../resource-manager-deployment-model.md).

Połączenie bramy sieci VPN bazuje na wielu zasobach konfigurowanych przy użyciu konkretnych ustawień. Większość zasobów można skonfigurować osobno, choć w niektórych przypadkach muszą być one konfigurowane w określonej kolejności. Możesz rozpocząć tworzenie i konfigurowanie zasobów za pomocą jednego narzędzia konfiguracji, takiego jak witryna Azure Portal. Później możesz zdecydować się przełączyć do innego narzędzia, takiego jak program PowerShell, w celu skonfigurowania dodatkowych zasobów lub zmodyfikowania istniejących zasobów, jeśli jest to wymagane. Obecnie nie wszystkie zasoby i ustawienia zasobów można skonfigurować w witrynie Azure Portal. Instrukcje w artykułach dotyczących poszczególnych topologii połączeń określają, kiedy potrzebne jest konkretne narzędzie konfiguracji. Aby uzyskać informacje na temat poszczególnych zasobów i ustawień dla bramy sieci VPN, zobacz [Ustawienia bramy sieci VPN — informacje](vpn-gateway-about-vpn-gateway-settings.md).

W poniższych sekcjach znajdują się tabele, w których przedstawiono następujące informacje:

* dostępny model wdrożenia;
* dostępne narzędzia do konfiguracji;
* linki prowadzące bezpośrednio do artykułu, jeśli jest dostępny.

Przedstawione diagramy i opisy mogą ułatwić wybór topologii połączenia dostosowanej do potrzeb użytkownika. Choć te diagramy przedstawiają podstawowe topologie, możliwe jest utworzenie bardziej złożonych konfiguracji z użyciem tych diagramów jako wskazówek.

## Połączenia typu lokacja-lokacja i połączenia obejmujące wiele lokacji
### Lokacja-lokacja
Połączenie bramy sieci VPN typu lokacja-lokacja to połączenie nawiązywane za pośrednictwem tunelu sieci VPN wykorzystującego protokół IPsec/IKE (IKEv1 lub IKEv2). Ten typ połączenia wymaga, aby urządzenie sieci VPN znajdowało się lokalnie, miało przypisany publiczny adres IP i nie znajdowało się za translatorem adresów sieciowych. Z połączeń typu lokacja-lokacja (S2S) można korzystać w ramach konfiguracji hybrydowych i obejmujących wiele lokalizacji.   

![Połączenie typu lokacja-lokacja](./media/vpn-gateway-about-vpngateways/demos2s.png "site-to-site")

### Obejmujące wiele lokacji
Można utworzyć i skonfigurować połączenie bramy sieci VPN między siecią wirtualną i wieloma sieciami lokalnymi. Podczas pracy z wieloma połączeniami należy użyć sieci VPN typu RouteBased (dynamiczna brama dla klasycznych sieci wirtualnych). Ze względu na to, że sieć wirtualna może mieć tylko jedną bramę sieci VPN, wszystkie połączenia za pośrednictwem bramy współużytkują dostępną przepustowość. Ten typ konfiguracji jest często określany mianem połączenia „obejmującego wiele lokacji”.

![Połączenie obejmujące wiele lokacji](./media/vpn-gateway-about-vpngateways/demomulti.png "multi-site")

### Modele wdrażania i metody nawiązywania połączeń typu lokacja-lokacja i połączeń obejmujących wiele lokacji
[!INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

## Połączenia między sieciami wirtualnymi
Proces nawiązywania połączenia między dwiema sieciami wirtualnymi przebiega podobnie do procesu łączenia sieci wirtualnej z lokacją lokalną. Oba typy połączeń wykorzystują bramę sieci VPN, aby zapewnić bezpieczny tunel z użyciem protokołu IPsec/IKE. Można także łączyć połączenia między sieciami wirtualnymi z konfiguracjami połączeń obejmujących wiele lokacji. Pozwala to tworzyć topologie sieci, które łączą wdrożenia obejmujące wiele lokalizacji z połączeniami między sieciami wirtualnymi.

Sieci wirtualne, między którymi tworzone jest połączenie, mogą:

* znajdować się w tym samym lub różnych regionach,
* należeć do tej samej lub różnych subskrypcji, 
* korzystać z tego samego lub różnych modeli wdrażania.

![Połączenie między sieciami wirtualnymi](./media/vpn-gateway-about-vpngateways/demov2v.png "vnet-to-vnet")

#### Połączenia między modelami wdrażania
Platforma Azure ma obecnie dwa modele wdrażania: klasyczny model wdrażania oraz model wdrażania przy użyciu usługi Resource Manager. Jeśli korzystasz z platformy Azure od pewnego czasu, prawdopodobnie masz maszyny wirtualne i wystąpienia roli platformy Azure działające w klasycznej sieci wirtualnej. Nowsze maszyny wirtualne i wystąpienia roli mogą działać w sieci wirtualnej utworzonej w usłudze Resource Manager. Możesz utworzyć połączenie między tymi sieciami wirtualnymi, aby umożliwić zasobom w jednej sieci wirtualnej bezpośrednie komunikowanie się z zasobami w innej sieci.

#### Komunikacja równorzędna sieci wirtualnych
Można utworzyć połączenie przy użyciu komunikacji równorzędnej sieci wirtualnych pod warunkiem, że sieć wirtualna spełnia określone wymagania. W przypadku komunikacji równorzędnej sieci wirtualnych nie jest używana brama sieci wirtualnej. Aby uzyskać więcej informacji, zobacz temat [Komunikacja równorzędna sieci wirtualnych](../virtual-network/virtual-network-peering-overview.md).

### Modele wdrażania i metody nawiązywania połączeń między sieciami wirtualnymi
[!INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## Punkt-lokacja
Połączenie bramy sieci VPN typu punkt-lokacja (P2S, Point-to-Site) pozwala utworzyć indywidualne bezpieczne połączenie z siecią wirtualną z poziomu komputera klienckiego. Połączenie punkt-lokacja (P2S) to połączenie sieci VPN nawiązywane za pośrednictwem protokołu SSTP (Secure Socket Tunneling Protocol). Połączenia typu punkt-lokacja nie wymagają do prawidłowego działania urządzenia sieci VPN ani publicznego adresu IP. Połączenie z siecią VPN jest nawiązywane przez jego uruchomienie z komputera klienckiego. To rozwiązanie przydaje się w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, np. z domu lub z konferencji, lub gdy konieczne jest połączenie z siecią wirtualną jedynie niewielkiej liczby klientów. Połączenia typu punkt-lokacja mogą być używane razem z połączeniami typu lokacja-lokacja z wykorzystaniem tej samej bramy sieci VPN, pod warunkiem że wszystkie wymagania dotyczące konfiguracji dla obu połączeń są zgodne.

![Połączenie typu punkt-lokacja](./media/vpn-gateway-about-vpngateways/demop2s.png "point-to-site")

### Modele wdrażania i metody nawiązywania połączeń typu punkt-lokacja
[!INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)]

## ExpressRoute
[!INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

W przypadku połączenia usługi ExpressRoute brama sieci wirtualnej jest konfigurowana z typem bramy „ExpressRoute” zamiast „Vpn”. Więcej informacji na temat usługi ExpressRoute zawiera artykuł [ExpressRoute technical overview](../expressroute/expressroute-introduction.md) (Opis techniczny usługi ExpressRoute).

## Współistniejące połączenia typu lokacja-lokacja i ExpressRoute
ExpressRoute to bezpośrednie, specjalne połączenie z usługami firmy Microsoft, w tym z platformą Azure, nawiązane z poziomu sieci WAN użytkownika, a nie z publicznego Internetu. Zaszyfrowany ruch połączenia sieci VPN typu lokacja-lokacja jest przesyłany za pośrednictwem publicznej sieci Internet. Możliwość konfiguracji połączenia sieci VPN typu lokacja-lokacja oraz połączenia ExpressRoute dla tej samej sieci wirtualnej niesie ze sobą pewne korzyści.

Sieć VPN typu lokacja-lokacja może zostać skonfigurowana jako bezpieczna ścieżka trybu failover dla połączenia ExpressRoute. Połączenia sieci VPN typu lokacja-lokacja mogą także zostać użyte w celu połączenia się z lokacjami, które nie są częścią sieci, ale które są połączone metodą ExpressRoute. Należy zauważyć, że wymaga to dwóch bram sieci wirtualnej dla tej samej sieci wirtualnej, z których jedna używa klasy -GatewayType Vpn, a druga klasy -GatewayType ExpressRoute.

![Współistniejące połączenia](./media/vpn-gateway-about-vpngateways/demoer.png "expressroute-site2site")

### Modele wdrażania i metody nawiązywania połączeń typu lokacja-lokacja i połączeń usługi ExpressRoute
[!INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)]

## Następne kroki
Planowanie konfiguracji bramy sieci VPN. Zobacz [Planowanie i projektowanie bramy sieci VPN](vpn-gateway-plan-design.md) i [Łączenie sieci lokalnej z platformą Azure](../guidance/guidance-connecting-your-on-premises-network-to-azure.md).

<!--HONumber=Oct16_HO3-->


