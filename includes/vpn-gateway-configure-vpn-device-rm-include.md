
Aby skonfigurować urządzenie sieci VPN, potrzebny jest publiczny adres IP bramy sieci wirtualnej do celów konfigurowania lokalnego urządzenia sieci VPN. Uzyskaj określone informacje dotyczące konfiguracji od producenta urządzenia i skonfiguruj urządzenie. Zapoznaj się z tematem [Urządzenia sieci VPN](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md), aby uzyskać więcej informacji o urządzeniach sieci VPN, które dobrze współdziałają z platformą Azure.

Aby znaleźć publiczny adres IP bramy sieci wirtualnej przy użyciu programu PowerShell, skorzystaj z poniższego przykładu:

    Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG

Publiczny adres IP bramy sieci wirtualnej można także wyświetlić, korzystając z witryny Azure Portal. Przejdź do obszaru **Bramy sieci wirtualnej**, a następnie kliknij nazwę bramy.

<!--HONumber=Sep16_HO3-->


