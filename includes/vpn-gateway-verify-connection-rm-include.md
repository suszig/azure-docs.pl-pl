### Aby sprawdzić połączenie przy użyciu programu PowerShell

Jest możliwość sprawdzenia, czy połączenie powiodło się, używając polecenia `Get-AzureRmVirtualNetworkGatewayConnection`, z opcją `-Debug` lub bez niej. 

1. Można skorzystać z następującego przykładu użycia polecenia cmdlet, dopasowując wartości do własnych potrzeb. Po wyświetleniu monitu wybierz „A”, aby uruchomić wszystko. W podanym przykładzie opcja `-Name` odnosi się do nazwy utworzonego połączenia, które ma zostać przetestowane.

        Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. Po zakończeniu działania polecenia cmdlet sprawdź wartości. W poniższym przykładzie stan połączenia jest wyświetlany jako „Połączone” i można zobaczyć bajty przychodzące i wychodzące.

        Body:
        {
          "name": "MyGWConnection",
          "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
          "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
            "virtualNetworkGateway1": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
        teways/vnetgw1"
            },
            "localNetworkGateway2": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
        ways/LocalSite"
            },
            "connectionType": "IPsec",
            "routingWeight": 10,
            "sharedKey": "abc123",
            "connectionStatus": "Connected",
            "ingressBytesTransferred": 33509044,
            "egressBytesTransferred": 4142431
          }

### Aby sprawdzić połączenie przy użyciu witryny Azure Portal

Przechodząc w witrynie Azure Portal do połączenia, można sprawdzić stan połączenia. Istnieje wiele sposobów, aby to zrobić. Poniżej znajduje się jeden ze sposobów przechodzenia do połączenia.

1. W witrynie [Azure Portal](http://portal.azure.com) przejdź do **Bramy sieci wirtualnej**. Kliknij nazwę bramy.
2. W okienku w obszarze **Ustawienia** kliknij pozycję **Połączenia**. Zostanie pokazany stan każdego połączenia.
3. Po kliknięciu nazwy połączenia można wyświetlić więcej informacji na jego temat. Na stronie z podstawowymi informacjami o połączeniu należy zwrócić uwagę na **Stan połączenia**. Po pomyślnym nawiązaniu połączenia stan jest wyświetlany jako „Powodzenie” i „Połączono”. Informacje na temat przepływających danych można uzyskać, spoglądając na **Dane wejściowe** i **Dane wyjściowe**.

    W poniższym przykładzie **Stan połączenia** jest określony jako „Nie połączono”. 

    ![Sprawdź połączenie](./media/vpn-gateway-verify-connection-rm-include/connectionverify450.png)

<!--HONumber=Sep16_HO3-->


