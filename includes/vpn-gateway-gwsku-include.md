Podczas tworzenia bramy sieci wirtualnej musisz wybrać jednostkę SKU bramy do użycia. W przypadku wybrania wyższej jednostki SKU bramy bramie przydzielane jest więcej procesorów CPU oraz wyższa przepustowość sieci, dlatego może obsługiwać wyższą przepływność sieci do sieci wirtualnej.

Brama VPN Gateway może używać następujących jednostek SKU:

* Podstawowa
* Standardowa (Standard)
* Wysoka wydajność (HighPerformance)

Brama sieci VPN nie korzysta z jednostki SKU bramy UltraPerformance. Informacje o jednostce SKU UltraPerformance można znaleźć w dokumentacji dotyczącej usługi [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md).

Podczas wybierania jednostki SKU należy wziąć pod uwagę następujące zagadnienia:

* Jeśli chcesz użyć typu sieci VPN PolicyBased, należy użyć podstawowej jednostki SKU. Sieć VPN PolicyBased (nazywana wcześniej routingiem statycznym) nie jest obsługiwana w żadnych innych jednostkach SKU.
* Protokół BGP nie jest obsługiwany w ramach podstawowej jednostki SKU.
* Współistniejące konfiguracje bramy sieci VPN usługi ExpressRoute nie są obsługiwane w ramach podstawowej jednostki SKU.
* Połączenia usługi VPN Gateway typu lokacja do lokacji (S2S) aktywne-aktywne można skonfigurować tylko w ramach jednostki SKU o wysokiej wydajności.



<!--HONumber=Nov16_HO2-->


