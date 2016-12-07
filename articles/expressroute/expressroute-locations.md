---
title: "Lokalizacje usługi ExpressRoute | Microsoft Docs"
description: "Ten artykuł zawiera szczegółowe omówienie lokalizacji, w których są oferowane usługi, i sposobu łączenia z regionami świadczenia usługi Azure."
services: expressroute
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
ms.assetid: c878513a-d594-42ad-8b0e-403efd0c4b25
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/02/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 4aa1e62b4758481b40c4b1c8a632c8bb72ab4ce6


---
# <a name="expressroute-partners-and-peering-locations"></a>Partnerzy i lokalizacje komunikacji równorzędnej usługi ExpressRoute
Tabele w tym artykule zawierają informacje dotyczące dostawców połączenia ExpressRoute, zasięgu geograficznego usługi ExpressRoute, usług w chmurze firmy Microsoft obsługiwanych za pośrednictwem usługi ExpressRoute oraz integratorów systemowych ExpressRoute (SI).

## <a name="a-namepartnersaexpressroute-connectivity-providers"></a><a name="partners"></a>Dostawcy połączenia usługi ExpressRoute
Usługa ExpressRoute jest obsługiwana we wszystkich regionach i lokalizacjach świadczenia usługi Azure. Poniższa mapa zawiera listę regionów świadczenia usługi Azure i lokalizacji usługi ExpressRoute. Lokalizacje usługi ExpressRoute to te, w których firma Microsoft prowadzi komunikację równorzędną z kilkoma dostawcami usług.

![Mapa lokalizacji][0]

Będziesz mieć dostęp do usług Azure we wszystkich regionach regionu geopolitycznego, jeśli połączysz się przynajmniej z jedną lokalizacją usługi ExpressRoute w tym regionie. Poniższa tabela zawiera mapę regionów świadczenia usługi Azure dla lokalizacji usługi ExpressRoute w regionie geopolitycznym.

| **Region geopolityczny** | **Regiony platformy Azure** | **Lokalizacje usługi ExpressRoute** |
| --- | --- | --- |
| **Ameryka Północna** |Wschodnie stany USA, Zachodnie stany USA, Wschodnie stany USA 2, Środkowe stany USA, Południowo-środkowe stany USA, Północno-środkowe stany USA, Kanada Środkowa, Kanada Wschodnia |Atlanta, Chicago, Dallas, Las Vegas, Los Angeles, Nowy Jork, Seattle, Dolina Krzemowa, Waszyngton, Montreal+, Miasto Quebec+, Toronto |
| **Ameryka Południowa** |Brazylia Południowa |Sao Paulo |
| **Europa** |Europa Północna, Europa Zachodnia, Zjednoczone Królestwo (zachód), Zjednoczone Królestwo (południe) |Amsterdam, Dublin, Londyn, Newport (Walia)+, Paryż |
| **Azja** |Azja Wschodnia, Azja Południowo-Wschodnia |Hongkong, Singapur |
| **Japonia** |Japońska Zachodnia, Japonia Wschodnia |Osaka, Tokio |
| **Australia** |Australia Południowo-Wschodnia, Australia Wschodnia |Melbourne, Sydney |
| **Indie** |Indie Zachodnie, Indie Środkowe, Indie Południowe |Madras, Bombaj |

W poniższej tabeli zamieszczono informacje o regionach i granicach geopolitycznych chmur krajowych.

| **Region geopolityczny** | **Regiony platformy Azure** | **Lokalizacje usługi ExpressRoute** |
| --- | --- | --- | --- |
| **Chmura administracji USA** |Administracja USA — Iowa, Administracja USA — Wirginia |Chicago, Dallas, Nowy Jork, Waszyngton |
| **Chiny** |Chiny Północne, Chiny Wschodnie |Pekin, Szanghaj |
| **Niemcy** |Niemcy Środkowe, Niemcy Wschodnie |Berlin, Frankfurt |

Łączność między regionami geopolitycznymi nie jest obsługiwana w standardowej jednostce SKU usługi ExpressRoute. Do obsługi połączeń globalnych trzeba włączyć dodatek Premium usługi ExpressRoute. Łączność z krajowymi środowiskami chmury nie jest obsługiwana. W razie potrzeby można współpracować z dostawcą połączenia.

## <a name="connectivity-provider-locations"></a>Lokalizacje dostawcy połączenia
> [!div class="op_single_selector"]
> [Lokalizacje wg dostawcy](expressroute-locations.md#connectivity-provider-locations)
> [Dostawcy wg lokalizacji](expressroute-locations-providers.md#connectivity-provider-locations)
> 
> 

### <a name="production-azure"></a>Środowisko produkcyjne Azure
| **Dostawca usług** | **Microsoft Azure** | **Office 365 i CRM Online** | **Lokalizacje** |
| --- | --- | --- | --- |
| **AARNet** |Obsługiwane |Obsługiwane |Melbourne, Sydney |
| **[Aryaka Networks](http://www.aryaka.com/)** |Obsługiwane |Obsługiwane |Amsterdam, Dallas, Dolina Krzemowa, Singapur, Tokio, Waszyngton |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Obsługiwane |Obsługiwane |Amsterdam, Chicago, Dallas, Londyn, Dolina Krzemowa, Singapur, Sydney, Waszyngton |
| **[British Telecom](http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** |Obsługiwane |Obsługiwane |Amsterdam, Hongkong, Londyn, Dolina Krzemowa, Singapur, Sydney, Tokio, Waszyngton |
| **CenturyLink** |Wkrótce |Wkrótce |Dolina Krzemowa |
| **China Telecom Global** |Obsługiwane |Nieobsługiwane |Hongkong |
| **[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** |Obsługiwane |Wkrótce |Dallas, Montreal+, Toronto |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Obsługiwane |Obsługiwane |Amsterdam, Dublin, Londyn, Tokio |
| **Comcast** |Obsługiwane |Obsługiwane |Chicago, Dolina Krzemowa, Waszyngton |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** |Obsługiwane |Obsługiwane |Los Angeles |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Obsługiwane |Obsługiwane |Amsterdam, Atlanta, Chicago, Dallas, Hongkong, Londyn, Los Angeles, Melbourne, Nowy Jork, Osaka, Paryż+, Sao Paulo, Seattle, Dolina Krzemowa, Singapur, Sydney, Tokio, Toronto, Waszyngton |
| **euNetworks** |Obsługiwane |Obsługiwane |Amsterdam |
| **GÉANT** |Obsługiwane |Obsługiwane |Amsterdam |
| **[Internet Initiative Japan Inc. — IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |Obsługiwane |Obsługiwane |Osaka, Tokio |
| **[InterCloud](https://www.intercloud.com/)** |Obsługiwane |Obsługiwane |Amsterdam, Londyn, Singapur, Waszyngton |
| **Internet Solutions — Cloud Connect** |Obsługiwane |Obsługiwane |Amsterdam, Londyn |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)** |Obsługiwane |Obsługiwane |Amsterdam, Londyn, Paryż |
| **Jisc** |Obsługiwane |Obsługiwane |Londyn |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Obsługiwane |Obsługiwane |Amsterdam, Chicago, Dallas, Las Vegas+, Londyn, Seattle, Dolina Krzemowa, Waszyngton |
| **Megaport** |Obsługiwane |Obsługiwane |Dallas, Hongkong, Las Vegas, Los Angeles, Melbourne, Nowy Jork, Seattle, Singapur, Sydney, Waszyngton |
| **MTN** |Obsługiwane |Obsługiwane |Londyn |
| **Dane nowej generacji** |Wkrótce |Wkrótce |Newport (Walia) + |
| **NEXTDC** |Obsługiwane |Obsługiwane |Melbourne, Sydney |
| **NTT Communications** |Obsługiwane |Obsługiwane |Londyn, Los Angeles Osaka, Singapur, Tokio, Waszyngton |
| **[Orange](http://www.orange-business.com/en/products/business-vpn-galerie)** |Obsługiwane |Obsługiwane |Amsterdam, Hongkong, Londyn, Dolina Krzemowa, Singapur, Sydney, Waszyngton |
| **PCCW Global Limited** |Obsługiwane |Obsługiwane |Hongkong |
| **SIFY** |Obsługiwane |Obsługiwane |Chennai |
| **[SingTel](http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |Obsługiwane |Obsługiwane |Singapur |
| **Softbank** |Obsługiwane |Obsługiwane |Osaka, Tokio |
| **[Tata Communications](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** |Obsługiwane |Obsługiwane |Amsterdam, Madras, Hongkong, Londyn, Bombaj, Dolina Krzemowa, Singapur, Waszyngton |
| **[TeleCity Group](http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** |Obsługiwane |Obsługiwane |Amsterdam, Dublin, Londyn |
| **Telefonica** |Obsługiwane |Obsługiwane |Sao Paulo |
| **Telenor** |Obsługiwane |Obsługiwane |Amsterdam, Londyn |
| **[Telstra Corporation](http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** |Obsługiwane |Obsługiwane |Melbourne, Sydney |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** |Obsługiwane |Obsługiwane |Amsterdam, Hongkong, Londyn, Dolina Krzemowa, Singapur, Sydney, Tokio, Waszyngton |
| **Vodafone** |Obsługiwane |Nieobsługiwane |Londyn |
| **[Zayo Group](http://www.zayo.com/solutions/industries/connect-to-cloud-data-centers/cloud-connectivity/microsoft-expressroute/)** |Obsługiwane |Obsługiwane |Chicago, Los Angeles, Nowy Jork, Dolina Krzemowa, Toronto, Waszyngton |

 **+** oznacza wkrótce

### <a name="national-cloud-environments"></a>Krajowe środowiska chmury
#### <a name="us-government-cloud"></a>Chmura administracji USA
| **Dostawca usług** | **Microsoft Azure** | **Office 365** | **Lokalizacje** |
| --- | --- | --- | --- |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Obsługiwane |Obsługiwane |Chicago, Waszyngton |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Obsługiwane |Obsługiwane |Chicago, Dallas, Nowy Jork, Waszyngton |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Obsługiwane |Obsługiwane |Chicago, Nowy Jork+, Waszyngton |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** |Obsługiwane |Obsługiwane |Chicago, Dallas, Nowy Jork, Waszyngton |

#### <a name="china"></a>Chiny
| **Dostawca usług** | **Microsoft Azure** | **Office 365** | **Lokalizacje** |
| --- | --- | --- | --- |
| **China Telecom** |Obsługiwane |Nieobsługiwane |Pekin, Szanghaj |

Więcej informacji znajduje się w artykule [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/) (Usługa ExpressRoute w Chinach).

#### <a name="germany"></a>Niemcy
| **Dostawca usług** | **Microsoft Azure** | **Office 365** | **Lokalizacje** |
| --- | --- | --- | --- |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Obsługiwane |Nieobsługiwane |Berlin+, Frankfurt |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Obsługiwane |Nieobsługiwane |Frankfurt |
| **e-shelter** |Obsługiwane |Nieobsługiwane |Berlin |
| **Interxion** |Obsługiwane |Nieobsługiwane |Frankfurt |

## <a name="a-namenonpartnersaconnectivity-through-service-providers-not-listed"></a><a name="nonpartners"></a>Łączność za pośrednictwem dostawców usług niewymienionych na liście
Jeśli dostawca połączenia nie został wymieniony w poprzednich sekcjach, możesz i tak utworzyć połączenie.

* Skontaktuj się z dostawcą połączenia, aby sprawdzić, czy jest on połączony z dowolną wymianą z tabeli powyżej. Użyj poniższych linków, aby zebrać więcej informacji o usługach oferowanych przez dostawców wymiany. Kilku dostawców połączenia jest już połączonych z wymianami sieci Ethernet.
  
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
  * [Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)
  * [NextDC](http://www.nextdc.com/)
  * [CoreSite](http://www.coresite.com/)
  * [Cologix](http://www.cologix.com/)
* Poproś dostawcę połączenia o rozszerzenie sieci o wybraną lokalizację komunikacji równorzędnej.
  * Dopilnuj, by dostawca połączenia rozszerzył łączność w sposób wysoko dostępny, aby nie wystąpiły żadne punkty awarii.
* Zamów obwód usługi ExpressRoute z wymianą jako dostawcą połączenia, aby połączyć się z firmą Microsoft.
  * Wykonaj kroki opisane w artykule [Create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) (Tworzenie obwodu usługi ExpressRoute), aby skonfigurować łączność.

| **Dostawca połączenia** | **Exchange** | **Lokalizacje** |
| --- | --- | --- |
| **[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)** |Equinix |Singapur |
| **Alaska Communications** |Equinix |Seattle |
| **[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure)** |Equinix |Nowy Jork, Waszyngton |
| **[XO Communications](http://www.xo.com/)** |Equinix |Dolina Krzemowa |

## <a name="expressroute-system-integrators"></a>Integratorzy systemu ExpressRoute
Włączanie prywatnej łączności do własnych potrzeb może być wyzwaniem w zależności od skali sieci. Możesz pracować z dowolnymi integratorami systemu wymienionymi w poniższej tabeli, którzy ułatwiają dołączanie do usługi ExpressRoute.

| **Integrator systemu** | **Kontynent** |
| --- | --- |
| **[Avanade Inc.](http://www.avanade.com/)** |Azja, Europa, USA |
| **[Dotnet Solutions](http://www.dotnetsolutions.co.uk/)** |Europa |
| **[Equinix Professional Services](http://www.equinix.com/services/consulting/)** |USA |
| **[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** |Azja |
| **[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** |USA |
| **[Project Leadership](http://www.projectleadership.net/azure)** |USA |

## <a name="next-steps"></a>Następne kroki
* Więcej informacji na temat usługi ExpressRoute znajduje się w artykule [ExpressRoute FAQ](expressroute-faqs.md) (Usługa ExpressRoute — często zadawane pytania).
* Upewnij się, że zostały spełnione wszystkie wymagania wstępne. Zobacz artykuł [ExpressRoute prerequisites](expressroute-prerequisites.md) (Wymagania wstępne usługi ExpressRoute).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mapa lokalizacji"



<!--HONumber=Nov16_HO2-->


