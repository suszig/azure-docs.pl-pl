<properties 
   pageTitle="VPN Gateway — informacje | Microsoft Azure"
   description="Informacje na temat połączeń bramy sieci VPN dla sieci wirtualnych platformy Azure."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="cherylmc" />

# VPN Gateway — informacje


Usługa VPN Gateway jest kolekcją zasobów służącą do wysyłania ruchu sieciowego między sieciami wirtualnymi i lokalizacjami lokalnymi. Bramy służą do obsługi połączeń lokacja-lokacja, punkt-lokacja oraz połączeń ExpressRoute. Brama VPN umożliwia również wysyłanie ruchu między wieloma sieciami wirtualnymi na platformie Azure (sieć wirtualna-sieć wirtualna). 

Aby utworzyć połączenie, należy dodać bramę sieci wirtualnej do sieci wirtualnej i skonfigurować dodatkowe zasoby bramy sieci VPN i ich ustawienia. Każda sieć wirtualna może mieć tylko jedną bramę sieci wirtualnej na typ bramy. Na przykład można mieć jedną bramę sieci wirtualnej, która używa klasy -GatewayType Vpn, oraz jednej, która używa klasy -GatewayType ExpressRoute.

Aby uzyskać informacje o wymaganiach dotyczących bramy, zobacz [Wymagania dotyczące bramy](vpn-gateway-about-vpn-gateway-settings.md#requirements). Aby uzyskać informacje o szacowanej zagregowanej przepływności, zobacz [Informacje o ustawieniach bramy sieci VPN](vpn-gateway-about-vpn-gateway-settings.md#aggthroughput). Aby poznać ceny, zobacz [Cennik bramy sieci VPN](https://azure.microsoft.com/pricing/details/vpn-gateway). Aby uzyskać informacje o subskrypcjach i limitach dotyczących usługi, zobacz [Limity dotyczące sieci](../articles/azure-subscription-service-limits.md#networking-limits).

Instrukcje używane podczas konfigurowania bramy sieci VPN będą zależeć od modelu wdrażania użytego w celu utworzenia sieci wirtualnej. Jeśli na przykład sieć wirtualna została utworzona przy użyciu klasycznego modelu wdrożenia, do tworzenia i konfigurowania ustawień bramy sieci VPN należy użyć wskazówek i instrukcji dotyczących klasycznego modelu wdrażania. Aby uzyskać więcej informacji, zobacz [Omówienie wdrażania przy użyciu usługi Resource Manager i wdrażania klasycznego](../resource-manager-deployment-model.md).

W poniższych sekcjach znajdują się tabele, w których przedstawiono następujące informacje dotyczące konfiguracji:

- dostępny model wdrożenia;
- dostępne narzędzia do konfiguracji;
- linki prowadzące bezpośrednio do artykułu, jeśli jest dostępny.


Przedstawione diagramy i opisy mogą ułatwić wybór topologii konfiguracji dostosowanej do potrzeb użytkownika. Choć te diagramy przedstawiają podstawowe topologie, możliwe jest utworzenie bardziej złożonych konfiguracji z użyciem tych diagramów jako wskazówek. Każda konfiguracja zależy od wybranych ustawień bramy sieci VPN.

### Konfigurowanie ustawień bramy sieci VPN

Ponieważ brama sieci VPN jest kolekcją zasobów, można skonfigurować niektóre zasoby przy użyciu jednego narzędzia, a następnie skonfigurować ustawienia innych zasobów za pomocą innego narzędzia. Obecnie nie można skonfigurować wszystkich ustawień zasobów bramy sieci VPN w portalu Azure. W tych artykułach w instrukcjach dla poszczególnych konfiguracji wskazano, czy jest określone narzędzie. W przypadku pracy przy użyciu klasycznego modelu wdrażania warto skorzystać z klasycznego portalu lub programu PowerShell. Aby uzyskać informacje na temat poszczególnych dostępnych ustawień, zobacz [Ustawienia bramy sieci VPN — informacje](vpn-gateway-about-vpn-gateway-settings.md).



## Połączenia typu lokacja-lokacja i połączenia obejmujące wiele lokacji

### Lokacja-lokacja

Połączenie typu lokacja-lokacja (S2S) to połączenie nawiązywane za pośrednictwem tunelu sieci VPN wykorzystującego protokół IPsec/IKE (IKEv1 lub IKEv2). Ten typ połączenia wymaga, aby urządzenie sieci VPN znajdowało się lokalnie, miało przypisany publiczny adres IP i nie znajdowało się za translatorem adresów sieciowych. Z połączeń typu lokacja-lokacja (S2S) można korzystać w ramach konfiguracji hybrydowych i obejmujących wiele lokalizacji.   

![Połączenie typu lokacja-lokacja](./media/vpn-gateway-about-vpngateways/demos2s.png "site-to-site")


### Obejmujące wiele lokacji

Można utworzyć i skonfigurować połączenie sieci VPN między siecią wirtualną i wieloma sieciami lokalnymi. Podczas pracy z wieloma połączeniami należy użyć typu sieci VPN opartego na trasach (dynamiczna brama dla klasycznych sieci wirtualnych). Ze względu na to, że sieć wirtualna może mieć tylko jedną bramę sieci wirtualnej, wszystkie połączenia za pośrednictwem bramy współużytkują dostępną przepustowość. Ten typ konfiguracji jest często określany mianem połączenia „obejmującego wiele lokacji”.
 

![Połączenie obejmujące wiele lokacji](./media/vpn-gateway-about-vpngateways/demomulti.png "multi-site")

### Modele i metody wdrażania

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## Połączenia między sieciami wirtualnymi

Proces nawiązywania połączenia między dwiema sieciami wirtualnymi przebiega podobnie do procesu łączenia sieci wirtualnej z lokacją lokalną. Oba typy połączeń wykorzystują bramę sieci VPN na platformie Azure, aby zapewnić bezpieczny tunel z użyciem protokołu IPsec/IKE. Można także łączyć połączenia między sieciami wirtualnymi z konfiguracjami obejmującymi wiele lokacji. Pozwala to tworzyć topologie sieci, które łączą wdrożenia obejmujące wiele lokalizacji z połączeniami między sieciami wirtualnymi.

Sieci wirtualne, między którymi tworzone jest połączenie, mogą:

- znajdować się w tym samym lub różnych regionach,
- należeć do tej samej lub różnych subskrypcji, 
- korzystać z tego samego lub różnych modeli wdrażania.



![Połączenie między sieciami wirtualnymi](./media/vpn-gateway-about-vpngateways/demov2v.png "vnet-to-vnet")



### Połączenia między modelami wdrażania

Platforma Azure ma obecnie dwa modele wdrażania: klasyczny model wdrażania oraz model wdrażania przy użyciu usługi Resource Manager. Jeśli korzystasz z platformy Azure od pewnego czasu, prawdopodobnie masz maszyny wirtualne i wystąpienia roli platformy Azure działające w klasycznej sieci wirtualnej. Nowsze maszyny wirtualne i wystąpienia roli mogą działać w sieci wirtualnej utworzonej w usłudze Resource Manager. Możesz utworzyć połączenie między tymi sieciami wirtualnymi, aby umożliwić zasobom w jednej sieci wirtualnej bezpośrednie komunikowanie się z zasobami w innej sieci.


### Modele i metody wdrażania

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

### Komunikacja równorzędna sieci wirtualnych

Można utworzyć połączenie przy użyciu komunikacji równorzędnej sieci wirtualnych pod warunkiem, że konfiguracja sieci wirtualnej spełnia określone wymagania. W przypadku komunikacji równorzędnej sieci wirtualnych nie jest używana brama sieci wirtualnej. [Komunikacja równorzędna sieci wirtualnych](../virtual-network/virtual-network-peering-overview.md) jest obecnie dostępna w wersji zapoznawczej.



## Punkt-lokacja

Konfiguracja typu punkt-lokacja (P2S, Point-to-Site) pozwala utworzyć indywidualne bezpieczne połączenie z siecią wirtualną z poziomu komputera klienckiego. Połączenie punkt-lokacja (P2S) to połączenie sieci VPN nawiązywane za pośrednictwem protokołu SSTP (Secure Socket Tunneling Protocol). Połączenia typu punkt-lokacja nie wymagają do prawidłowego działania urządzenia sieci VPN ani publicznego adresu IP. Połączenie z siecią VPN jest nawiązywane przez jego uruchomienie z komputera klienckiego. Przedstawione rozwiązanie przydaje się w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, np. z domu lub z konferencji, lub gdy konieczne jest połączenie z siecią wirtualną jedynie niewielkiej liczby klientów. 


![Połączenie typu punkt-lokacja](./media/vpn-gateway-about-vpngateways/demop2s.png "point-to-site")

### Modele i metody wdrażania

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

Więcej informacji na temat usługi ExpressRoute zawiera artykuł [ExpressRoute technical overview](../expressroute/expressroute-introduction.md) (Opis techniczny usługi ExpressRoute).


## Współistniejące połączenia typu lokacja-lokacja i ExpressRoute

ExpressRoute to bezpośrednie, specjalne połączenie z usługami firmy Microsoft, w tym z platformą Azure, nawiązane z poziomu sieci WAN użytkownika, a nie z publicznego Internetu. Zaszyfrowany ruch połączenia sieci VPN typu lokacja-lokacja jest przesyłany za pośrednictwem publicznej sieci Internet. Możliwość konfiguracji połączenia sieci VPN typu lokacja-lokacja oraz połączenia ExpressRoute dla tej samej sieci wirtualnej niesie ze sobą pewne korzyści.

Sieć VPN typu lokacja-lokacja może zostać skonfigurowana jako bezpieczna ścieżka trybu failover dla połączenia ExpressRoute. Połączenia sieci VPN typu lokacja-lokacja mogą także zostać użyte w celu połączenia się z lokacjami, które nie są częścią sieci, ale które są połączone metodą ExpressRoute. Ta konfiguracja wymaga dwóch bram sieci wirtualnej dla tej samej sieci wirtualnej, z których jedna używa klasy -GatewayType Vpn, a druga klasy -GatewayType ExpressRoute.


![Współistniejące połączenia](./media/vpn-gateway-about-vpngateways/demoer.png "expressroute-site2site")


### Modele i metody wdrażania

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## Następne kroki

Więcej informacji na temat bram sieci VPN zawiera temat [Brama sieci VPN — często zadawane pytania](vpn-gateway-vpn-faq.md).

Połącz lokalizację lokalną z siecią wirtualną. Zobacz [Tworzenie połączenia typu lokacja-lokacja](vpn-gateway-howto-site-to-site-resource-manager-portal.md).





 



<!--HONumber=sep16_HO1-->


