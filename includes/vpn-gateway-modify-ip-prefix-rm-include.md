### <a name="noconnection"></a>Modyfikowanie prefiksów — brak połączenia bramy

- Aby dodać dodatkowe prefiksy adresów:

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
  ```

- Aby usunąć prefiks adresu:<br>
  Opuść prefiksy, które nie są już potrzebne. W tym przykładzie nie są już potrzebne prefiksy 20.0.0.0/24 (z poprzedniego przykładu), dlatego zaktualizujemy bramę sieci lokalnej, wykluczając ten prefiks.

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')
  ```

### <a name="withconnection"></a>Modyfikowanie prefiksów — istnieje połączenie bramy
Jeśli istnieje już połączenie bramy i chcesz dodać lub usunąć prefiksy adresów IP zawarte w bramie Twojej sieci lokalnej, wykonaj kolejno następujące kroki. Spowoduje to pewien przestój połączenia sieci VPN.

> [!IMPORTANT]
> Nie należy usuwać bramy sieci VPN. Jeśli zostanie ona usunięta, trzeba będzie wrócić do poprzednich kroków, aby utworzyć ją ponownie. Ponadto musisz zaktualizować lokalne urządzenie sieci VPN o nowy adres IP bramy sieci VPN.
> 
> 

1. Usuń połączenie.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName
  ```
2. Zmodyfikuj prefiksy adresów IP bramy Twojej sieci lokalnej.
   
  Ustaw wartość zmiennej dla klasy LocalNetworkGateway.

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName
  ```
   
  Zmodyfikuj prefiksy.
   
  ```powershell
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
  ```
3. Utwórz połączenie. W tym przykładzie skonfigurujemy połączenie typu IPsec. Podczas ponownego tworzenia połączenia należy użyć typu połączenia, który został określony dla danej konfiguracji. O dodatkowych typach połączeń można przeczytać na stronie [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) (Polecenia cmdlet programu PowerShell).
   
  Ustaw wartość zmiennej dla klasy VirtualNetworkGateway.

  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName
  ```
   
  Utwórz połączenie. W tym przykładzie użyto zmiennej $local ustawionej w kroku 2.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName -Location 'West US' `
  -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec `
  -RoutingWeight 10 -SharedKey 'abc123'
  ```