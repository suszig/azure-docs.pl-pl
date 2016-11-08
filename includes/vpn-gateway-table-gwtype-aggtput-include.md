|  | **Przepływność bramy sieci VPN (1)** | **Maksymalna liczba tuneli protokołu IPsec bramy sieci VPN (2)** | **Przepływność bramy ExpressRoute** | **Brama sieci VPN i ExpressRoute współistnieją** |
| --- | --- | --- | --- | --- |
| **Podstawowa jednostka SKU (3)** |100 Mb/s |10 |500 Mb/s |Nie |
| **Standardowa jednostka SKU (4)** |100 Mb/s |10 |1000 Mb/s |Tak |
| **Jednostka SKU wysokiej wydajności (4)** |200 Mb/s |30 |2000 Mb/s |Tak |

* (1) Przepływność sieci VPN została wstępnie oszacowana na podstawie pomiarów między sieciami wirtualnymi w ramach tego samego regionu systemu Azure. Nie jest to gwarantowana przepływność dla połączeń obejmujących wiele lokalizacji za pośrednictwem sieci Internet. Jest to maksymalna możliwa przepustowość.
* (2) Liczba tuneli dotyczy sieci VPN opartych na trasie. Sieć VPN oparta na zasadach może obsługiwać tylko jeden tunel VPN typu lokacja-lokacja.
* (3) Protokół BGP nie jest obsługiwany w ramach podstawowej jednostki SKU.
* (4) Sieci VPN oparte na zasadach nie są obsługiwane w ramach tej jednostki SKU. Są one obsługiwane tylko w ramach podstawowej jednostki SKU.

<!--HONumber=Oct16_HO3-->


