---
title: "Wprowadzenie do usługi ExpressRoute | Microsoft Docs"
description: "Ta strona zawiera omówienie usługi ExpressRoute, w tym sposobu działania połączenia ExpressRoute."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: fd95dcd5-df1d-41d6-85dd-e91d0091af05
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 3b3e0fd5342c4729d6ffee4858089099b31404a8


---
# <a name="expressroute-technical-overview"></a>ExpressRoute — opis techniczny
Usługa Microsoft Azure ExpressRoute umożliwia rozszerzanie sieci lokalnych na chmurę Microsoft za pośrednictwem specjalnego połączenia prywatnego wspieranego przez dostawcę połączenia. Dzięki usłudze ExpressRoute można ustanowić połączenia z usługami Microsoft w chmurze, np. Microsoft Azure, Office 365 i CRM Online. Połączenie może być z sieci typu dowolna-dowolna (IP VPN), sieci Ethernet typu punkt-punkt lub przy użyciu łączności obejmującej wiele połączeń wirtualnych przez dostawcę połączenia w ramach infrastruktury współlokacji. Połączenia ExpressRoute nie odbywają się za pośrednictwem publicznego Internetu. Dzięki temu oferują one większą niezawodność i szybkość oraz mniejsze opóźnienia i lepsze zabezpieczenia niż typowe połączenia przez Internet.

![](./media/expressroute-introduction/expressroute-basic.png)

**Najważniejsze korzyści to:**

* Łączność w warstwie 3 między siecią lokalną a usługą Microsoft Cloud za pośrednictwem dostawcy połączenia. Połączenie może być z sieci typu dowolna-dowolna (IP VPN), sieci Ethernet typu punkt-punkt lub przy użyciu łączności obejmującej wiele połączeń wirtualnych za pośrednictwem wymiany sieci Ethernet.
* Łączność z usługami w chmurze firmy Microsoft we wszystkich regionach w regionie geopolitycznym.
* Łączność globalna z usługami Microsoft we wszystkich regionach dzięki dodatkowi ExpressRoute Premium.
* Routing dynamiczny między siecią użytkownika i firmą Microsoft za pośrednictwem standardowych protokołów (BGP).
* Wbudowana nadmiarowość w każdej lokalizacji komunikacji równorzędnej w celu uzyskania większej niezawodności.
* Czas sprawnego działania połączenia zgodnie z umową [SLA](https://azure.microsoft.com/support/legal/sla/).
* QoS i obsługa wielu klas usługi dla wielu aplikacji, np. Skype dla firm.

Szczegółowe informacje znajdują się w artykule [ExpressRoute FAQ](expressroute-faqs.md) (Usługa ExpressRoute — często zadawane pytania).

## <a name="a-namehowtoconnectahow-can-i-connect-my-network-to-microsoft-using-expressroute"></a><a name="howtoconnect"></a>Jak połączyć sieć z firmą Microsoft przy użyciu usługi ExpressRoute?
Połączenie między siecią lokalną i chmurą Microsoft można utworzyć na trzy sposoby:

### <a name="colocated-at-a-cloud-exchange"></a>Kolokowanie w ramach wymiany w chmurze
Jeśli użytkownik jest kolokowany w ramach infrastruktury z wymianą w chmurze, może zamówić łączność obejmującą wiele połączeń wirtualnych z chmurą Microsoft za pośrednictwem wymiany sieci Ethernet dostawy kolokacji. Dostawcy kolokacji mogą zaoferować łączność obejmującą wiele połączeń w warstwie 2 lub wiele zarządzanych połączeń w warstwie 3 między infrastrukturą w strukturze kolokacji i chmurą Microsoft.

### <a name="pointtopoint-ethernet-connections"></a>Połączenia Ethernet typu punkt-punkt
Lokalne centra danych/biura można połączyć z chmurą Microsoft za pośrednictwem łączy Ethernet typu punkt-punkt. Dostawcy sieci Ethernet typu punkt-punkt oferują połączenia w warstwie 2 lub zarządzane połączenia w warstwie 3 między witryną użytkownika i chmurą Microsoft.

### <a name="anytoany-ipvpn-networks"></a>Sieci typu dowolna-dowolna (IPVPN)
Sieć WAN można zintegrować z chmurą Microsoft. Dostawcy sieci IPVPN (zwykle MPLS VPN) oferują łączność typu dowolna-dowolna między biurami oddziałów i centrami danych. Chmurę Microsoft można połączyć z siecią WAN w taki sposób, aby wyglądało to tak jak każde inne biuro oddziału. Dostawcy sieci WAN oferują zazwyczaj łączność zarządzaną w warstwie 3. Wszystkie możliwości i funkcje usługi ExpressRoute są identyczne we wszystkich powyższych modelach łączności. 

Dostawcy połączenia oferują po jednym modelu łączności lub większą ich liczbę. Można współpracować z dostawcą połączenia w celu wybrania najlepszego dla siebie modelu.

![](./media/expressroute-introduction/expressroute-connectivitymodels.png)

## <a name="expressroute-features"></a>Funkcje usługi ExpressRoute
ExpressRoute obsługuje następujące funkcje i możliwości: 

### <a name="layer-3-connectivity"></a>Łączność w warstwie 3
Firma Microsoft używa standardowego protokołu routingu dynamicznego (BGP) do wymiany tras między siecią lokalną, wystąpieniami na platformie Azure i publicznymi adresami Microsoft.  Ustanawiamy wiele sesji BGP z siecią dla różnych profilów ruchu. Więcej szczegółowych informacji można znaleźć w artykule [ExpressRoute circuit and routing domains](expressroute-circuit-peerings.md) (Obwód i domeny routingu usługi ExpressRoute).

### <a name="redundancy"></a>Nadmiarowość
Każdy obwód usługi ExpressRoute składa się z dwóch połączeń z dwoma routerami granicznymi Microsoft Enterprise (MSEE) od dostawcy połączenia/z granicy sieci. Firma Microsoft wymaga podwójnego połączenia BGP od dostawcy połączenia/ze strony użytkownika — po jednym do każdego MSEE. Można zdecydować o niewdrażaniu nadmiarowych urządzeń/obwodów sieci Ethernet po swojej stronie. Dostawcy połączenia używają jednak urządzeń nadmiarowych, aby zagwarantować, że połączenia będą przekazywane do firmy Microsoft w sposób nadmiarowy. Konfiguracja łączności nadmiarowej w warstwie 3 jest wymaganiem, które musi być spełnione, aby umowa [SLA](https://azure.microsoft.com/support/legal/sla/) była ważna. 

### <a name="connectivity-to-microsoft-cloud-services"></a>Łączność z usługami w chmurze firmy Microsoft
[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

Połączenia ExpressRoute umożliwiają dostęp do następujących usług:

* Usługi Microsoft Azure
* Usługi Microsoft Office 365
* Usługi Microsoft CRM Online 

Szczegółowa lista usług obsługiwanych za pośrednictwem usługi ExpressRoute znajduje się w artykule [ExpressRoute FAQ](expressroute-faqs.md) (Usługa ExpressRoute — często zadawane pytania).

### <a name="connectivity-to-all-regions-within-a-geopolitical-region"></a>Łączność z wszystkimi regionami w regionie geopolitycznym
Możesz połączyć się z firmą Microsoft w jednej z naszych [lokalizacji komunikacji równorzędnej](expressroute-locations.md) i mieć dostęp do wszystkich regionów w regionie geopolitycznym. 

Na przykład po połączeniu się z firmą Microsoft w Amsterdamie za pośrednictwem usługi ExpressRoute będziesz mieć dostęp do wszystkich usług w chmurze Microsoft obsługiwanych w Europie Północnej i Europie Zachodniej. Zapoznaj się z artykułem [ExpressRoute partners and peering locations](expressroute-locations.md) (Partnerzy i lokalizacje komunikacji równorzędnej usługi ExpressRoute), który zawiera omówienie regionów geopolitycznych, powiązanych regionów chmury Microsoft i odpowiadających im lokalizacji komunikacji równorzędnej usługi ExpressRoute.

### <a name="global-connectivity-with-expressroute-premium-addon"></a>Globalna łączność dzięki dodatkowi ExpressRoute Premium
Możesz włączyć funkcję dodatku ExpressRoute Premium, aby rozszerzyć łączność w ramach granic geopolitycznych. Na przykład po połączeniu się z firmą Microsoft w Amsterdamie za pośrednictwem usługi ExpressRoute będziesz mieć dostęp do wszystkich usług w chmurze Microsoft obsługiwanych we wszystkich regionach na świecie (z wyjątkiem chmur krajowych). Możesz uzyskać dostęp do usług wdrożonych w Ameryce Południowej lub Australii w taki sam sposób, w jaki uzyskujesz dostęp do regionów Europy Północnej i Zachodniej.

### <a name="rich-connectivity-partner-ecosystem"></a>Bogaty ekosystem partnerów łączności
Usługa ExpressRoute ma rosnący ekosystem dostawców połączenia i partnerów SI. Najnowsze informacje znajdują się w artykule [ExpressRoute providers and locations](expressroute-locations.md) (Dostawcy i lokalizacje usługi ExpressRoute).

### <a name="connectivity-to-national-clouds"></a>Łączność z chmurami krajowymi
Firma Microsoft obsługuje izolowane środowiska w chmurze dla określonych regionów geopolitycznych i segmentów klientów. Lista krajowych chmur i dostawców znajduje się w artykule [ExpressRoute providers and locations](expressroute-locations.md) (Dostawcy i lokalizacje usługi ExpressRoute).

### <a name="supported-bandwidth-options"></a>Obsługiwane opcje przepustowości
Możesz zakupić obwody usługi ExpressRoute do szerokiego zakresu przepustowości. Lista obsługiwanych przepustowości znajduje się poniżej. Koniecznie sprawdź u dostawcy połączenia listę obsługiwanych przepustowości, które oferuje.

* 50 Mb/s
* 100 Mb/s
* 200 Mb/s
* 500 Mb/s
* 1 Gb/s
* 2 Gb/s
* 5 Gb/s
* 10 Gb/s

### <a name="dynamic-scaling-of-bandwidth"></a>Dynamiczne skalowanie przepustowości
Istnieje możliwość zwiększania przepustowości obwodu usługi ExpressRoute (w oparciu o optymalne rozwiązanie) bez konieczności zatrzymywania połączeń. 

### <a name="flexible-billing-models"></a>Elastyczne modele rozliczeń
Możesz wybrać najlepszy dla siebie model rozliczeń. Wybierz z modeli rozliczeń wymienionych poniżej. Szczegółowe informacje znajdują się w artykule [ExpressRoute FAQ](expressroute-faqs.md) (Usługa ExpressRoute — często zadawane pytania). 

* **Nieograniczona ilość danych**. Obwód usługi ExpressRoute jest obciążany opłatą co miesiąc. Cały transfer danych przychodzących i wychodzących jest bezpłatny. 
* **Mierzone dane**. Obwód usługi ExpressRoute jest obciążany opłatą co miesiąc. Cały transfer danych przychodzących jest bezpłatny. Transfer danych wychodzących jest obciążany opłatą za GB transferu danych. Stawki transferu danych różnią się zależnie od regionu.
* **Dodatek ExpressRoute Premium**. ExpressRoute Premium jest dodatkiem w obwodzie usługi ExpressRoute. Dodatek ExpressRoute Premium zapewnia następujące możliwości: 
  * Zwiększone limity tras dla publicznej i prywatnej komunikacji równorzędnej Azure z 4000 do 10 000 tras.
  * Globalna łączność dla usług. Obwód usługi ExpressRoute utworzony w dowolnym regionie (z wyłączeniem chmur krajowych) będzie miał dostęp do zasobów we wszystkich innych regionach na świecie. Na przykład dostęp do sieci wirtualnej utworzonej w Europie Zachodniej można uzyskać za pośrednictwem obwodu usługi ExpressRoute zainicjowanego w Dolinie Krzemowej.
  * Zwiększona liczba łączy sieci wirtualnej na obwód usługi ExpressRoute z 10 do większego limitu, w zależności od przepustowości obwodu.

## <a name="next-steps"></a>Następne kroki
* Więcej informacji na temat połączeń ExpressRoute i domen routingu. Zobacz artykuł [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (Obwody i domeny routingu usługi ExpressRoute).
* Znajdź dostawcę usługi. Zobacz artykuł [ExpressRoute partners and peering locations](expressroute-locations.md) (Partnerzy i lokalizacje komunikacji równorzędnej usługi ExpressRoute).
* Upewnij się, że zostały spełnione wszystkie wymagania wstępne. Zobacz artykuł [ExpressRoute prerequisites](expressroute-prerequisites.md) (Wymagania wstępne usługi ExpressRoute).
* Zapoznaj się z wymaganiami dotyczącymi [routingu](expressroute-routing.md), [NAT](expressroute-nat.md) i [QoS](expressroute-qos.md).
* Skonfiguruj połączenie usługi ExpressRoute.
  * [Create an ExpressRoute circuit (Tworzenie obwodu usługi ExpressRoute)](expressroute-howto-circuit-classic.md)
  * [Configure routing (Konfigurowanie routingu)](expressroute-howto-routing-classic.md)
  * [Link a VNet to an ExpressRoute circuit (Łączenie sieci wirtualnej z obwodem usługi ExpressRoute)](expressroute-howto-linkvnet-classic.md)




<!--HONumber=Nov16_HO2-->


