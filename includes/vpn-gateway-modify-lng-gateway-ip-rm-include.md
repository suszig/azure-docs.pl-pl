Aby zmodyfikować adres IP bramy, użyj polecenia cmdlet „New-AzureRmVirtualNetworkGatewayConnection”. Obecnie polecenie cmdlet „Set” nie obsługuje modyfikowania adresu IP bramy.

### <a name="gwipnoconnection"></a>Modyfikowanie adresu IP bramy — brak połączenia bramy
Aby zmodyfikować adres IP bramy dla bramy sieci lokalnej, która nie ma jeszcze połączenia, skorzystaj z poniższego przykładu. Jednocześnie możesz również zmodyfikować prefiksy adresów. Pamiętaj, aby użyć istniejącej nazwy bramy sieci lokalnej w celu zastąpienia bieżących ustawień. Jeśli tego nie zrobisz, utworzysz nową bramę sieci lokalnej, a nie zastąpisz istniejącej.

Użyj poniższego przykładu, zastępując wartości swoimi własnymi:

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <a name="gwipwithconnection"></a>Modyfikowanie adresu IP bramy — istnieje połączenie bramy
Jeśli istnieje już połączenie bramy, musisz je najpierw usunąć. Po usunięciu połączenia możesz zmodyfikować adres IP bramy i ponownie utworzyć nowe połączenie. Jednocześnie możesz również zmodyfikować prefiksy adresów. Spowoduje to pewien przestój połączenia sieci VPN.

> [!IMPORTANT]
> Nie należy usuwać bramy sieci VPN. Jeśli zostanie ona usunięta, trzeba będzie wrócić do poprzednich kroków, aby utworzyć ją ponownie. Ponadto musisz zaktualizować lokalne urządzenie sieci VPN o nowy adres IP bramy sieci VPN.
> 
> 

1. Usuń połączenie. Nazwę połączenia możesz znaleźć przy użyciu polecenia cmdlet „Get-AzureRmVirtualNetworkGatewayConnection”.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName
  ```
2. Zmodyfikuj wartość klasy „GatewayIpAddress”. Jednocześnie możesz również zmodyfikować prefiksy adresów. Pamiętaj, aby użyć istniejącej nazwy bramy sieci lokalnej w celu zastąpienia bieżących ustawień. Jeśli tego nie zrobisz, utworzysz nową bramę sieci lokalnej, a nie zastąpisz istniejącej.

  ```powershell
  New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
  -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
  ```
3. Utwórz połączenie. W tym przykładzie skonfigurujemy połączenie typu IPsec. Podczas ponownego tworzenia połączenia należy użyć typu połączenia, który został określony dla danej konfiguracji. O dodatkowych typach połączeń można przeczytać na stronie [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) (Polecenia cmdlet programu PowerShell).  Aby uzyskać nazwę bramy VirtualNetworkGateway, można uruchomić polecenie cmdlet „Get-AzureRmVirtualNetworkGateway”.
   
    Ustaw zmienne.

  ```powershell
  $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
  ```
   
    Utwórz połączenie.

  ```powershell 
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
  -Location "West US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```