- **Typ sieci VPN opartej na zasadach:** sieci VPN oparte na zasadach wcześniej były nazywane statycznymi bramami routingu w klasycznym modelu wdrożenia. Sieci VPN oparte na zasadach szyfrują pakiety i kierują je bezpośrednio przez tunel protokołu IPsec na podstawie zasad protokołu IPsec skonfigurowanych przy użyciu kombinacji prefiksów adresów między siecią lokalną użytkownika i siecią wirtualną platformy Azure. Zasady (lub selektor ruchu) są zazwyczaj zdefiniowane jako lista dostępu w konfiguracji urządzenia sieci VPN. Wartością dla typu sieci VPN opartej na zasadach jest *PolicyBased*.

- **Typ sieci VPN opartej na trasach**: Sieci VPN oparte na trasach wcześniej były nazywane dynamicznymi bramami routingu w klasycznym modelu wdrożenia. Sieci VPN oparte na trasach używają „tras” w funkcji przekazywania pakietów IP lub tabeli routingu do kierowania pakietów do odpowiednich interfejsów tuneli. W dalszej kolejności interfejsy tuneli szyfrują lub odszyfrowują pakiety wchodzące do tuneli lub wychodzące z nich. Zasady (lub selektor ruchu) dla sieci VPN opartych na trasach są skonfigurowane jako każdy z każdym (lub symbole wieloznaczne). Wartością dla typu sieci VPN opartej na trasach jest *RouteBased*.


<!--HONumber=Jun16_HO2-->


