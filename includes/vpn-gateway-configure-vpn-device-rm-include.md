Połączenia typu lokacja-lokacja z siecią lokalną wymagają urządzenia sieci VPN. Nie podajemy kroków konfiguracji dla wszystkich urządzeń sieci VPN, jednak następujące linki mogą zawierać przydatne informacje:

- Aby uzyskać informacje o zgodnych urządzeniach sieci VPN, zobacz [Urządzenia sieci VPN](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md). 
- Aby uzyskać linki do ustawień konfiguracji urządzeń, zobacz [Zweryfikowane urządzenia sieci VPN](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable). Linki do ustawień konfiguracji urządzeń zostały podane na zasadzie największej staranności. Zawsze najlepiej jest skontaktować się z producentem urządzenia, aby uzyskać najnowsze informacje o konfiguracji.
- Aby uzyskać informacje na temat edytowania przykładów konfiguracji urządzeń, zobacz [Edytowanie przykładów](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#editing).
- Aby zapoznać się z parametrami protokołów IPsec/IKE, zobacz [Parametry](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec).
- Przed skonfigurowaniem urządzenia sieci VPN, którego zamierzasz użyć, sprawdź, czy istnieją dla niego [znane problemy dotyczące zgodności urządzeń](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#known).

Podczas konfigurowania urządzenia sieci VPN potrzebne będą:

- Publiczny adres IP bramy sieci wirtualnej.

    -  Aby znaleźć publiczny adres IP za pomocą witryny Azure Portal, przejdź do pozycji **Bramy sieci wirtualnej**, a następnie kliknij nazwę bramy. 
    - Aby znaleźć publiczny adres IP bramy sieci wirtualnej przy użyciu programu PowerShell, skorzystaj z poniższego przykładu, zastępując wartości własnymi.

            Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG
- Klucz wspólny. To ten sam klucz wspólny, który jest określany podczas tworzenia połączenia sieci VPN typu lokacja-lokacja. W naszych przykładach używamy bardzo prostego klucza wspólnego. Do użycia należy wygenerować bardziej złożony klucz.




