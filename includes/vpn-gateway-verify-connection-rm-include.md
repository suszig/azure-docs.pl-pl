Można sprawdzić połączenie sieci VPN w portalu Azure, przechodząc do opcji **Bramy sieci wirtualnych** **>** *** i klikając nazwę swojej bramy*** **>** **Ustawienia** **>** **Połączenia**. Po wybraniu nazwy połączenia można wyświetlić więcej informacji o połączeniu. W poniższym przykładzie połączenie nie jest aktywne i żadne dane nie przepływają.


![Sprawdź połączenie](./media/vpn-gateway-verify-connection-rm-include/connectionverify450.png)


### Aby sprawdzić połączenie przy użyciu programu PowerShell

Istnieje również możliwość sprawdzenia, czy połączenie powiodło się, przy użyciu polecenia `Get-AzureRmVirtualNetworkGatewayConnection –Debug`. Można skorzystać z następującego przykładu użycia polecenia cmdlet, dopasowując wartości do własnych potrzeb. Po wyświetleniu monitu wybierz „A”, aby uruchomić wszystkie.

    Get-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Debug

 Po zakończeniu działania polecenia cmdlet przewiń, aby wyświetlić wartości. W poniższym przykładzie stan połączenia jest wyświetlany jako *Połączone* i można zobaczyć bajty przychodzące i wychodzące.

    Body:
    {
      "name": "localtovon",
      "id":
    "/subscriptions/086cfaa0-0d1d-4b1c-9455-f8e3da2a0c7789/resourceGroups/testrg/providers/Microsoft.Network/connections/loca
    ltovon",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
        "virtualNetworkGateway1": {
          "id":
    "/subscriptions/086cfaa0-0d1d-4b1c-9455-f8e3da2a0c7789/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworkGa
    teways/vnetgw1"
        },
        "localNetworkGateway2": {
          "id":
    "/subscriptions/086cfaa0-0d1d-4b1c-9455-f8e3da2a0c7789/resourceGroups/testrg/providers/Microsoft.Network/localNetworkGate
    ways/LocalSite"
        },
        "connectionType": "IPsec",
        "routingWeight": 10,
        "sharedKey": "abc123",
        "connectionStatus": "Connected",
        "ingressBytesTransferred": 33509044,
        "egressBytesTransferred": 4142431
      }


<!--HONumber=Jun16_HO2-->


