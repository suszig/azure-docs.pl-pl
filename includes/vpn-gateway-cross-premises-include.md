|  | **Punkt-lokacja** | **Lokacja-lokacja** | **ExpressRoute** |
| --- | --- | --- | --- |
| **Usługi obsługiwane przez platformę Azure** |Usługi Cloud Services i Virtual Machines |Usługi Cloud Services i Virtual Machines |[Lista usług](../articles/expressroute/expressroute-faqs.md#supported-services) |
| **Typowe przepustowości** |Zwykle < 100 Mb/s łącznie |Zwykle < 100 Mb/s łącznie |50 Mb/s, 100 Mb/s, 200 Mb/s, 500 Mb/s, 1 Gb/s, 2 Gb/s, 5 Gb/s, 10 Gb/s |
| **Obsługiwane protokoły** |Secure Socket Tunneling Protocol (SSTP) |IPsec |Bezpośrednie połączenie za pośrednictwem sieci VLAN, technologie VPN NSP (MPLS, VPLS itp.) |
| **Routing** |Oparty na trasie (dynamiczny) |Firma Microsoft obsługuje routing oparty na zasadach (statyczny) i routing oparty na trasie (dynamiczny VPN) |BGP |
| **Elastyczność połączenia** |aktywne/pasywne |aktywne/pasywne |aktywne/aktywne |
| **Typowy przypadek użycia** |Tworzenie prototypów, scenariusze laboratorium do tworzenia i testowania dla usług w chmurze i maszyn wirtualnych |Scenariusze laboratorium do tworzenia i testowania oraz obciążenia produkcyjne o małej skali dla usług w chmurze i maszyn wirtualnych |Dostęp do wszystkich usług Azure (zatwierdzona lista), obciążenia o znaczeniu krytycznym oraz klasy korporacyjnej, Backup, dane big data, platforma Azure jako lokacja DR |
| **Umowa SLA** |[Umowa SLA](https://azure.microsoft.com/support/legal/sla/) |[Umowa SLA](https://azure.microsoft.com/support/legal/sla/) |[Umowa SLA](https://azure.microsoft.com/support/legal/sla/) |
| **Cennik** |[Cennik](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Cennik](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Cennik](https://azure.microsoft.com/pricing/details/expressroute/) |
| **Dokumentacja techniczna** |[Dokumentacja bramy sieci VPN](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[Dokumentacja bramy sieci VPN](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[Dokumentacja usługi ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) |
| **Często zadawane pytania** |[Brama sieci VPN — często zadawane pytania](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[Brama sieci VPN — często zadawane pytania](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[Usługa ExpressRoute — często zadawane pytania](../articles/expressroute/expressroute-faqs.md) |

<!--HONumber=Jun16_HO2-->


