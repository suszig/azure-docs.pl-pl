Platforma Azure oferuje następujące jednostki SKU bramy sieci VPN:

|**SKU**   | **Połączenia typu lokacja-lokacja/Połączenia między sieciami wirtualnymi<br>Tunele** | **Połączenia<br>punkt-lokacja** | **Agregacja<br>przepływności** |
|---       | ---                             | ---                    | ---                         |
|**VpnGw1**| Maksymalnie z 30                         | Maksymalnie z 128               | 500 Mb/s                    |
|**VpnGw2**| Maksymalnie z 30                         | Maksymalnie z 128               | 1 Gb/s                      |
|**VpnGw3**| Maksymalnie z 30                         | Maksymalnie z 128               | 1,25 Gb/s                   |
|**Podstawowa** | Maksymalnie z 10                         | Maksymalnie z 128               | 100 Mb/s                    | 
|          |                                 |                        |                             | 

- Przepływność opiera się na pomiarach wielu tuneli zagregowanych za pośrednictwem jednej bramy. Ze względu na warunki ruchu internetowego i zachowania aplikacji nie jest to przepływność gwarantowana.

- Informacje o cenach znajdują się na stronie [Cennik](https://azure.microsoft.com/pricing/details/vpn-gateway).

- Informacje na temat umowy SLA (Service Level Agreement) można znaleźć na stronie [SLA](https://azure.microsoft.com/en-us/support/legal/sla/vpn-gateway/).