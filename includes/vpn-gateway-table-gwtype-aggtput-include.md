|    | **Przepływność bramy sieci VPN (1)** | **Maksymalna liczba tuneli protokołu IPsec bramy sieci VPN (2)** | **Przepływność bramy ExpressRoute** | **Brama sieci VPN i ExpressRoute współistnieją**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **Podstawowy SKU**              |  100 Mb/s | 10                         |  500 Mb/s                           | Nie   |
| **Standardowy SKU**           |  100 Mb/s | 10                         | 1000 Mb/s                           | Yes  |
| **SKU wysokiej wydajności**   | 200 Mb/s  | 30                         | 2000 Mb/s                           | Yes  |

- (1) Przepływność sieci VPN została wstępnie oszacowana na podstawie pomiarów między sieciami wirtualnymi w ramach tego samego regionu systemu Azure. Nie ma gwarancji, że ta wartość może zostać uzyskana w połączeniach przez Internet między różnymi lokalizacjami, ale powinna ona być używana jako najwyższa możliwa wielkość.
- (2) Liczba tuneli dotyczy poniższej sieci VPN opartej na trasie. Sieć VPN oparta na zasadach może obsługiwać tylko jeden tunel VPN typu „lokacja do lokacji”


<!--HONumber=Jun16_HO2-->


