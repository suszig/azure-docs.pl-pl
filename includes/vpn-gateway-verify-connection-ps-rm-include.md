Możesz również sprawdzić, czy połączenie zostało pomyślnie nawiązane, przy użyciu polecenia cmdlet **Get-AzureRmVirtualNetworkGatewayConnection** z opcją **-Debug** lub bez niej. 

1. Można skorzystać z następującego przykładu użycia polecenia cmdlet, dopasowując wartości do własnych potrzeb. Po wyświetleniu monitu wybierz „A”, aby uruchomić wszystko. W podanym przykładzie opcja **-Name** odnosi się do nazwy utworzonego połączenia, które ma zostać przetestowane.

  ```powershell
  Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG
  ```
2. Po zakończeniu działania polecenia cmdlet sprawdź wartości. W poniższym przykładzie stan połączenia jest wyświetlany jako „Połączone” i można zobaczyć bajty przychodzące i wychodzące.
   
  ```
  "connectionStatus": "Connected",
  "ingressBytesTransferred": 33509044,
  "egressBytesTransferred": 4142431
  ```
  