Połączenia typu lokacja-lokacja z siecią lokalną wymagają urządzenia sieci VPN. Istnieje wiele różnych urządzeń sieci VPN, które będą działać z platformą Azure. Aby uzyskać informacje o urządzeniach sieci VPN i ustawienia konfiguracji, zobacz [Urządzenia sieci VPN](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md). Przed skonfigurowaniem urządzenia sieci VPN, którego zamierzasz użyć, sprawdź, czy istnieją dla niego [znane problemy dotyczące zgodności urządzeń](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#known). Konkretne informacje o konfiguracji urządzenia sieci VPN można uzyskać od producenta urządzenia.

Podczas konfigurowania urządzenia sieci VPN potrzebne będą:

- **Publiczny adres IP** bramy sieci wirtualnej.

    -  Aby znaleźć publiczny adres IP za pomocą witryny Azure Portal, przejdź do pozycji **Bramy sieci wirtualnej**, a następnie kliknij nazwę bramy. 

    - Aby znaleźć publiczny adres IP bramy sieci wirtualnej przy użyciu programu PowerShell, skorzystaj z poniższego przykładu, zastępując wartości własnymi.

            Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG
- **Klucz wspólny**. To ten sam klucz wspólny, który zostanie określony podczas tworzenia połączenia sieci VPN typu lokacja-lokacja. W naszych przykładach używamy bardzo prostego klucza wspólnego. Do użycia należy wygenerować bardziej złożony klucz.




