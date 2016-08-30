<properties 
   pageTitle="Topologie połączenia usługi VPN Gateway| Microsoft Azure"
   description="Przegląd topologii połączenia usługi VPN Gateway i dostępnych narzędzi konfiguracji oraz modeli wdrażania."
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
   ms.date="03/18/2016"
   ms.author="cherylmc" />

# Topologie połączenia usługi Azure VPN Gateway

W tym artykule przedstawiono podstawowe topologie połączenia bramy sieci VPN. Przedstawione grafiki i opisy mogą ułatwić wybór topologii konfiguracji dostosowanej do potrzeb użytkownika. Choć artykuł ten jest poświęcony podstawowym topologiom, możliwe jest utworzenie bardziej złożonych topologii z użyciem tych diagramów jako wskazówek.

Dla każdej topologii dostępna jest tabela zawierająca informacje o modelu wdrażania, do którego ma zastosowanie dana topologia, oraz o narzędziach wdrażania, których można użyć do skonfigurowania danej topologii. Tabela ta zawiera także bezpośredni link do odpowiedniego artykułu (jeśli jest dostępny). Tabele są często aktualizowane wraz z pojawianiem się nowych artykułów i narzędzi wdrażania.

Więcej informacji o bramach sieci VPN można znaleźć w artykule [VPN Gateway — informacje](vpn-gateway-about-vpngateways.md)



## Połączenia typu lokacja-lokacja i połączenia obejmujące wiele lokacji

Połączenie typu lokacja-lokacja to połączenie nawiązywane za pośrednictwem tunelu sieci VPN wykorzystującego protokół IPsec/IKE (IKEv1 lub IKEv2). Ten typ połączenia wymaga urządzenia sieci VPN lub lokalnej usługi RRAS systemu Windows Server. Z połączeń typu lokacja-lokacja można korzystać w ramach konfiguracji hybrydowych i obejmujących wiele lokalizacji.   


**Diagram połączenia typu lokacja-lokacja (S2S)**

![Połączenie typu lokacja-lokacja](./media/vpn-gateway-topology/site2site.png "site-to-site")

Korzystając z sieci VPN na platformie Azure opartych na trasie, można utworzyć i skonfigurować więcej niż jedno połączenie sieci VPN S2S dla sieci lokalnych. Ten typ konfiguracji jest często określany mianem połączenia „obejmującego wiele lokacji”.
 

**Diagram połączenia obejmującego wiele lokacji**

![Połączenie obejmujące wiele lokacji](./media/vpn-gateway-topology/multisite.png "multi-site")


**Dostępne metody i modele wdrażania**

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## Połączenia między sieciami wirtualnymi

Proces nawiązywania połączenia między dwiema sieciami wirtualnymi przebiega bardzo podobne do procesu łączenia sieci wirtualnej z lokacją lokalną. Oba typy połączeń wykorzystują bramę sieci VPN na platformie Azure, aby zapewnić bezpieczny tunel z użyciem protokołu IPsec/IKE. Sieci wirtualne, które mają zostać połączone, mogą być zlokalizowane w różnych regionach lub należeć do różnych subskrypcji. Można także łączyć połączenia między sieciami wirtualnymi z konfiguracjami obejmującymi wiele lokacji. Pozwala to tworzyć topologie sieci, które łączą wdrożenia obejmujące wiele lokalizacji z połączeniami między sieciami wirtualnymi.

W ramach platformy Azure są obecnie obsługiwane dwa modele wdrażania: model wdrażania przy użyciu zarządzania usługą platformy Azure (określany jako model klasyczny) i model wdrażania przy użyciu usługi Azure Resource Manager. Użytkownicy, którzy od dłuższego czasu korzystają z platformy Azure, prawdopodobnie używają maszyn wirtualnych i ról wystąpień uruchomionych w ramach klasycznej sieci wirtualnej, natomiast ich nowsze maszyny wirtualne i wystąpienia ról mogą być uruchamiane w ramach sieci wirtualnej utworzonej przy użyciu usługi Azure Resource Manager. Połączenie między sieciami wirtualnymi można utworzyć, nawet jeśli te sieci są powiązane z różnymi modelami wdrażania.


**Diagram połączenia między sieciami wirtualnymi**

![Połączenie między sieciami wirtualnymi](./media/vpn-gateway-topology/vnet2vnet.png "vnet-to-vnet")


**Dostępne metody i modele wdrażania**

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 



## Współistniejące połączenia typu lokacja-lokacja i ExpressRoute

ExpressRoute to bezpośrednie, specjalne połączenie z usługami firmy Microsoft, w tym z platformą Azure, nawiązane z poziomu sieci WAN użytkownika, a nie z publicznego Internetu. Zaszyfrowany ruch połączenia sieci VPN typu lokacja-lokacja jest przesyłany za pośrednictwem publicznej sieci Internet. Możliwość konfiguracji połączenia sieci VPN typu lokacja-lokacja oraz połączenia ExpressRoute dla tej samej sieci wirtualnej niesie ze sobą pewne korzyści. Sieć VPN typu lokacja-lokacja może zostać skonfigurowana jako bezpieczna ścieżka trybu failover dla połączenia ExpressRoute. Połączenia sieci VPN typu lokacja-lokacja mogą także zostać użyte w celu połączenia się z lokacjami, które nie są częścią sieci, ale które są połączone metodą ExpressRoute. 


**Diagram współistniejących połączeń**

![Współistniejące połączenia](./media/vpn-gateway-topology/expressroutes2s.png "expressroute-site2site")


**Dostępne metody i modele wdrażania**

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## Punkt-lokacja

Konfiguracja typu punkt-lokacja pozwala utworzyć indywidualne bezpieczne połączenie z siecią wirtualną z poziomu komputera klienckiego. Połączenie sieci VPN jest nawiązywane poprzez zainicjowanie połączenia z komputera klienckiego. Przedstawione rozwiązanie przydaje się w przypadku, gdy celem użytkownika jest połączenie się z siecią wirtualną z lokalizacji zdalnej, np. z domu lub z konferencji, lub gdy konieczne jest połączenie z siecią wirtualną jedynie niewielkiej liczby klientów. 

Połączenie sieci VPN typu punkt-lokacja jest nawiązywane za pośrednictwem protokołu SSTP (Secure Socket Tunneling Protocol). Połączenia typu punkt-lokacja nie wymagają do prawidłowego działania urządzenia sieci VPN ani publicznego adresu IP. 

**Diagram połączenia typu punkt-lokacja**

![Połączenie typu punkt-lokacja](./media/vpn-gateway-topology/point2site.png "point-to-site")

**Dostępne metody i modele wdrażania**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## Następne kroki

Aby lepiej zrozumieć problematykę bram sieci VPN przed przejściem do planowania i projektowania połączenia, warto zapoznać się z informacjami zawartymi w artykułach [Bramy sieci VPN — informacje](vpn-gateway-about-vpngateways.md) i [Brama sieci VPN — często zadawane pytania](vpn-gateway-vpn-faq.md).





 



<!--HONumber=jun16_HO2-->


