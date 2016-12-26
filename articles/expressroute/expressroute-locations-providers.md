---
title: "Lokalizacje usługi ExpressRoute | Microsoft Docs"
description: "Ten artykuł zawiera szczegółowe omówienie lokalizacji, w których są oferowane usługi, i sposobu łączenia z regionami świadczenia usługi Azure."
services: expressroute
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
ms.assetid: feb67da3-5abc-4acb-bad4-f78e3c541ded
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/08/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: cbbc9f29fc4dea49807f1093b673bebac628314b
ms.openlocfilehash: e886f9c1984003d1bd1391012b26eea39d4a0b76


---
# <a name="expressroute-partners-and-peering-locations"></a>Partnerzy i lokalizacje komunikacji równorzędnej usługi ExpressRoute
Tabele w tym artykule zawierają informacje dotyczące dostawców połączenia ExpressRoute, zasięgu geograficznego usługi ExpressRoute, usług w chmurze firmy Microsoft obsługiwanych za pośrednictwem usługi ExpressRoute oraz integratorów systemowych ExpressRoute (SI).

## <a name="a-namepartnersaexpressroute-connectivity-providers"></a><a name="partners"></a>Dostawcy połączenia usługi ExpressRoute
Usługa ExpressRoute jest obsługiwana we wszystkich regionach i lokalizacjach świadczenia usługi Azure. Poniższa mapa zawiera listę regionów świadczenia usługi Azure i lokalizacji usługi ExpressRoute. Lokalizacje usługi ExpressRoute to te, w których firma Microsoft prowadzi komunikację równorzędną z kilkoma dostawcami usług.

![Mapa lokalizacji][0]

Będziesz mieć dostęp do usług Azure we wszystkich regionach regionu geopolitycznego, jeśli połączysz się przynajmniej z jedną lokalizacją usługi ExpressRoute w tym regionie. 

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Regiony świadczenia usługi Azure i lokalizacje usługi ExpressRoute w regionach geopolitycznych
Poniższa tabela zawiera mapę regionów świadczenia usługi Azure dla lokalizacji usługi ExpressRoute w regionie geopolitycznym.

| **Region geopolityczny** | **Regiony platformy Azure** | **Lokalizacje usługi ExpressRoute** |
| --- | --- | --- |
| **Ameryka Północna** |Wschodnie stany USA, Zachodnie stany USA, Wschodnie stany USA 2, Środkowe stany USA, Południowo-środkowe stany USA, Północno-środkowe stany USA, Kanada Środkowa, Kanada Wschodnia |Atlanta, Chicago, Dallas, Las Vegas, Los Angeles, Nowy Jork, Seattle, Dolina Krzemowa, Waszyngton, Montreal+, Miasto Quebec+, Toronto |
| **Ameryka Południowa** |Brazylia Południowa |Sao Paulo |
| **Europa** |Europa Północna, Europa Zachodnia, Zjednoczone Królestwo (zachód), Zjednoczone Królestwo (południe) |Amsterdam, Dublin, Londyn, Newport (Walia), Paryż |
| **Azja** |Azja Wschodnia, Azja Południowo-Wschodnia |Hongkong, Singapur |
| **Japonia** |Japońska Zachodnia, Japonia Wschodnia |Osaka, Tokio |
| **Australia** |Australia Południowo-Wschodnia, Australia Wschodnia |Melbourne, Sydney |
| **Indie** |Indie Zachodnie, Indie Środkowe, Indie Południowe |Madras, Bombaj |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Regiony i granice geopolityczne chmur krajowych
W poniższej tabeli zamieszczono informacje o regionach i granicach geopolitycznych chmur krajowych.

| **Region geopolityczny** | **Regiony platformy Azure** | **Lokalizacje usługi ExpressRoute** |
| --- | --- | --- | --- |
| **Chmura administracji USA** |Administracja USA — Iowa, Administracja USA — Wirginia |Chicago, Dallas, Nowy Jork, Waszyngton |
| **Chiny** |Chiny Północne, Chiny Wschodnie |Pekin, Szanghaj |
| **Niemcy** |Niemcy Środkowe, Niemcy Wschodnie |Berlin, Frankfurt |

Łączność między regionami geopolitycznymi nie jest obsługiwana w standardowej jednostce SKU usługi ExpressRoute. Do obsługi połączeń globalnych trzeba włączyć dodatek Premium usługi ExpressRoute. Łączność z krajowymi środowiskami chmury nie jest obsługiwana. W razie potrzeby można współpracować z dostawcą połączenia.

## <a name="a-namelocationsaconnectivity-provider-locations"></a><a name="locations"></a>Lokalizacje dostawców połączenia
> [!div class="op_single_selector"]
> * [Lokalizacje według dostawcy](expressroute-locations.md#locations)
> * [Dostawcy według lokalizacji](expressroute-locations-providers.md#locations)
> 
> 

### <a name="production-azure"></a>Środowisko produkcyjne Azure
| **Lokalizacja** | **Dostawcy usług** |
| --- | --- |
| **Amsterdam** |Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT+, InterCloud, Internet Solutions — Cloud Connect, Interxion, Level 3 Communications, Orange, Tata Communications, TeleCity Group, Telenor, Verizon |
| **Atlanta** |Equinix |
| **Chennai** |SIFY, Tata Communications |
| **Chicago** |AT&T NetBond, Comcast, Equinix, Level 3 Communications, Zayo Group |
| **Dallas** |Aryaka Networks, AT&T NetBond, Cologix, Equinix, Level 3 Communications, Megaport |
| **Dublin** |Colt, Telecity Group |
| **Hongkong** |British Telecom, China Telecom Global, Equinix, Megaport, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **Londyn** |AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions — Cloud Connect, Interxion, Jisc, Level 3 Communications, MTN, NTT Communications, Orange, Tata Communications, Telecity Group, Telenor, Verizon, Vodafone |
| **Las Vegas** |Level 3 Communications+, Megaport |
| **Los Angeles** |CoreSite, Equinix, Megaport, NTT, Zayo Group |
| **Melbourne** |AARNet, Equinix, Megaport, NEXTDC, Telstra Corporation |
| **Nowy Jork** |Equinix, Megaport, Zayo Group |
| **Newport (Walia)** |Dane nowej generacji |
| **Montreal** |Cologix+ |
| **Mumbaj** |Tata Communications |
| **Osaka** |Equinix, Internet Initiative Japan Inc. — IIJ, NTT Communications, Softbank |
| **Paryż** |Interxion, Equinix+ |
| **Sao Paulo** |Equinix, Telefonica |
| **Seattle** |Equinix, Level 3 Communications, Megaport |
| **Dolina Krzemowa** |Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink+, Comcast, Equinix, Level 3 Communications, Orange, Tata Communications, Verizon, Zayo Group |
| **Singapur** |Aryaka Networks, AT&T NetBond, British Telecom, Equinix, InterCloud, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Verizon |
| **Sydney** |AARNet, AT&T NetBond, British Telecom, Equinix, Megaport, NEXTDC, Orange, Telstra Corporation, Verizon |
| **Tokio** |Aryaka Networks, British Telecom, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, Softbank, Verizon |
| **Toronto** |Cologix, Equinix, Megaport, Zayo Group |
| **Waszyngton** |Aryaka Networks, AT&T NetBond, British Telecom, Comcast, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, Verizon, Zayo Group |

 **+** oznacza wkrótce

### <a name="national-cloud-environments"></a>Krajowe środowiska chmury

### <a name="us-government-cloud"></a>Chmura administracji USA
| **Lokalizacja** | **Dostawcy usług** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |Equinix, Verizon |
| **Nowy Jork** |Equinix, Level 3 Communications+, Verizon |
| **Waszyngton** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |

### <a name="china"></a>Chiny
| **Lokalizacja** | **Dostawcy usług** |
| --- | --- |
| **Pekin** |China Telecom |
| **Szanghaj** |China Telecom |

Więcej informacji znajduje się w artykule [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/) (Usługa ExpressRoute w Chinach)

### <a name="germany"></a>Niemcy
| **Lokalizacja** | **Dostawcy usług** |
| --- | --- |
| **Berlin** |Colt+, e-shelter |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="a-namenonpartnersaconnectivity-through-service-providers-not-listed"></a><a name="nonpartners"></a>Łączność za pośrednictwem dostawców usług niewymienionych na liście
Jeśli dostawca połączenia nie został wymieniony w poprzednich sekcjach, możesz i tak utworzyć połączenie.

* Skontaktuj się z dostawcą połączenia, aby sprawdzić, czy jest on połączony z dowolną wymianą z tabeli powyżej. Użyj poniższych linków, aby zebrać więcej informacji o usługach oferowanych przez dostawców wymiany. Kilku dostawców połączenia jest już połączonych z wymianami sieci Ethernet.
  * [Cologix](http://www.cologix.com/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [Interxion](http://www.interxion.com/)
  * [NextDC](http://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* Poproś dostawcę połączenia o rozszerzenie sieci o wybraną lokalizację komunikacji równorzędnej.
  * Dopilnuj, by dostawca połączenia rozszerzył łączność w sposób wysoko dostępny, aby nie wystąpiły żadne punkty awarii.
* Zamów obwód usługi ExpressRoute z wymianą jako dostawcą połączenia, aby połączyć się z firmą Microsoft.
  * Wykonaj kroki opisane w artykule [Create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) (Tworzenie obwodu usługi ExpressRoute), aby skonfigurować łączność.

| **Lokalizacja** | **Exchange** | **Dostawcy połączeń** |
| --- | --- | --- |
| **Londyn** | Equinix | Exponential E |
| **Nowy Jork** |Equinix |Lightower |
| **Seattle** |Equinix |Alaska Communications |
| **Dolina Krzemowa** |Equinix |XO Communications |
| **Singapur** |Equinix |1CLOUDSTAR |
| **Sydney** | Megaport | Macquarie |
| **Waszyngton** |Equinix | Lightower, Masergy |

## <a name="expressroute-system-integrators"></a>Integratorzy systemu ExpressRoute
Włączanie prywatnej łączności do własnych potrzeb może być wyzwaniem w zależności od skali sieci. Możesz pracować z dowolnymi integratorami systemu wymienionymi w poniższej tabeli, którzy ułatwiają dołączanie do usługi ExpressRoute.

| **Kontynent** | **Integratorzy systemów** |
| --- | --- |
| **Azja** |Avanade Inc., OneAs1a |
| **Europa** |Avanade Inc., Dotnet Solutions |
| **USA** |Avanade Inc., Equinix Professional Services, Perficient, Project Leadership |

## <a name="next-steps"></a>Następne kroki
* Więcej informacji na temat usługi ExpressRoute znajduje się w artykule [ExpressRoute FAQ](expressroute-faqs.md) (Usługa ExpressRoute — często zadawane pytania).
* Upewnij się, że zostały spełnione wszystkie wymagania wstępne. Zobacz artykuł [ExpressRoute prerequisites](expressroute-prerequisites.md) (Wymagania wstępne usługi ExpressRoute).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mapa lokalizacji"



<!--HONumber=Dec16_HO2-->


