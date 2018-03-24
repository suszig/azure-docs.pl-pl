---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: fa9c27457b1da4d233aaea2a6621af9f5d01149d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
|  | **Point-to-Site** | **Site-to-Site** | **ExpressRoute** |
| --- | --- | --- | --- |
| **Usługi obsługiwane przez platformę Azure** |Usługi Cloud Services i Virtual Machines |Usługi Cloud Services i Virtual Machines |[Lista usług](../articles/expressroute/expressroute-faqs.md#supported-services) |
| **Typowe przepustowości** |Zwykle < 100 Mb/s łącznie |Zwykle < 1 GB/s agregacji |50 Mb/s, 100 Mb/s, 200 Mb/s, 500 Mb/s, 1 Gb/s, 2 Gb/s, 5 Gb/s, 10 Gb/s |
| **Obsługiwane protokoły** |Secure Socket Tunneling Protocol (SSTP) |IPsec |Bezpośrednie połączenie za pośrednictwem sieci VLAN, technologie VPN NSP (MPLS, VPLS itp.) |
| **Routing** |RouteBased (dynamic) |Firma Microsoft obsługuje PolicyBased (routing statyczny) i RouteBased (dynamiczny VPN) |BGP |
| **Elastyczność połączenia** |aktywne/pasywne |aktywny pasywny "lub" aktywny aktywny |aktywne/aktywne |
| **Typowy przypadek użycia** |Tworzenie prototypów, scenariusze laboratorium do tworzenia i testowania dla usług w chmurze i maszyn wirtualnych |Scenariusze laboratorium do tworzenia i testowania oraz obciążenia produkcyjne o małej skali dla usług w chmurze i maszyn wirtualnych |Dostęp do wszystkich usług Azure (zatwierdzona lista), obciążenia o znaczeniu krytycznym oraz klasy korporacyjnej, Backup, dane big data, platforma Azure jako lokacja DR |
| **Umowa SLA** |[Umowa SLA](https://azure.microsoft.com/support/legal/sla/) |[Umowa SLA](https://azure.microsoft.com/support/legal/sla/) |[Umowa SLA](https://azure.microsoft.com/support/legal/sla/) |
| **Cennik** |[Cennik](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Cennik](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Cennik](https://azure.microsoft.com/pricing/details/expressroute/) |
| **Dokumentacja techniczna** |[Dokumentacja usługi VPN Gateway](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[Dokumentacja usługi VPN Gateway](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[Dokumentacja usługi ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) |
| **Często zadawane pytania** |[VPN Gateway — często zadawane pytania](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[VPN Gateway — często zadawane pytania](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[Usługa ExpressRoute — często zadawane pytania](../articles/expressroute/expressroute-faqs.md) |
