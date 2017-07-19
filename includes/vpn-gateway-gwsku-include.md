Podczas tworzenia bramy sieci wirtualnej musisz wybrać jednostkę SKU bramy do użycia. Wybierz jednostki SKU, które spełniają wymagania, na podstawie typów obciążeń, przepustowości, funkcji i umowy SLA.

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

###  <a name="workloads"></a>Obciążenia w trybie produkcyjnym *a obciążenia* w środowisku tworzenia i testowania

Z powodu różnic w umowach SLA i zestawach funkcji zaleca się następujące jednostki SKU dla trybu produkcyjnego *i* w środowisku tworzenia i testowania:

| **Obciążenie**                       | **Jednostki SKU**               |
| ---                                | ---                    |
| **Tryb produkcyjny, obciążenia krytyczne** | VpnGw1, VpnGw2, VpnGw3 |
| **Środowisko tworzenia i testowania lub weryfikacja koncepcji**   | Podstawowa                  |
|                                    |                        |

Jeśli używasz starych jednostek SKU, w trybie produkcyjnym zalecane są jednostki SKU Standard i HighPerformance. Aby uzyskać informacje o starych jednostkach SKU, zobacz artykuł [Gateway SKUs (old)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md) (Jednostki SKU bramy (stare)).

###  <a name="feature"></a>Zestawy funkcji jednostek SKU bramy

Nowe jednostki SKU bramy usprawniają zestawy funkcji oferowane w bramach:

| **SKU**| **Funkcje**|
| ---    | ---         |
| VpnGw1<br>VpnGw2<br>VpnGw3|Sieć VPN oparta na trasach obejmująca maksymalnie 30 tuneli* <br>P2S, BGP, aktywne-aktywne, niestandardowe zasady protokołu IPsec/IKE, współistnienie ExpressRoute/VPN <br><br>* Można skonfigurować „PolicyBasedTrafficSelectors” do łączenia bramy sieci VPN opartej na trasach (VpnGw1, VpnGw2, VpnGw3) z wieloma lokalnymi urządzeniami zapory opartymi na zasadach. Aby zapoznać się ze szczegółami, zobacz artykuł [Connect VPN gateways to multiple on-premises policy-based VPN devices using PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) (Połączenie bram sieci VPN z wieloma lokalnymi urządzeniami sieci VPN opartymi na zasadach przy użyciu programu PowerShell). |
|Podstawowa   | Oparta na trasach: 10 tuneli z P2S<br>Oparta na zasadach (IKEv1): 1 tunel; bez P2S|
|        |             |

###  <a name="resize"></a>Zmiana rozmiaru jednostek SKU bramy

1. Można zmienić rozmiar jednostek SKU, wybierając z opcji VpnGw1, VpnGw2 i VpnGw3.
2. Podczas pracy ze starymi jednostkami SKU bramy można zmienić rozmiar, wybierając z opcji Basic, Standard i HighPerformance.
2. **Nie można** zmienić rozmiaru z opcji Basic/Standard/HighPerformance na nowe opcje VpnGw1/VpnGw2/VpnGw3. Zamiast tego trzeba [migrować](#migrate) do nowych jednostek SKU.

###  <a name="migrate"></a>Migrowanie ze starych do nowych jednostek SKU

[!INCLUDE [Migrate SKU](./vpn-gateway-migrate-legacy-sku-include.md)]