Aby zmienić adres IP bramy, należy użyć polecenia cmdlet `New-AzureRmVirtualNetworkGatewayConnection`. Jeśli nazwa bramy sieci lokalnej pozostanie taka sama jak nazwa już istniejącej, ustawienia zostaną zastąpione. W tej chwili polecenie cmdlet „Set” nie obsługuje modyfikowania adresu IP bramy.

### <a name="gwipnoconnection"></a>Modyfikowanie adresu IP bramy — brak połączenia bramy
Aby zaktualizować adres IP bramy dla bramy sieci lokalnej, która nie ma jeszcze połączenia, skorzystaj z poniższego przykładu. Jednocześnie można również zaktualizować prefiksy adresów. Nowe ustawienia zastąpią istniejące ustawienia. Pamiętaj, aby użyć istniejącej nazwy bramy sieci lokalnej. Jeśli tego nie zrobisz, zostanie utworzona nowa brama sieci lokalnej, która nie zastąpi istniejącej.

Użyj poniższego przykładu, zastępując wartości swoimi własnymi.

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <a name="gwipwithconnection"></a>Modyfikowanie adresu IP bramy — istniejące połączenie bramy
Jeśli istnieje już połączenie bramy, musisz najpierw usunąć połączenie. Następnie możesz zmodyfikować adres IP bramy i ponownie utworzyć nowe połączenie. Spowoduje to pewien przestój połączenia sieci VPN.

> [!IMPORTANT]
> Nie należy usuwać bramy sieci VPN. Jeśli tak zrobisz, trzeba będzie wrócić do poprzednich kroków, aby utworzyć ją ponownie, a także ponownie skonfigurować router lokalny przy użyciu adresu IP przypisanego nowo utworzonej bramie.
> 
> 

1. Usuń połączenie. Nazwę połączenia można znaleźć przy użyciu polecenia cmdlet `Get-AzureRmVirtualNetworkGatewayConnection`.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName
  ```
2. Zmień wartość klasy GatewayIpAddress. Jeśli to konieczne, można w tej chwili zmodyfikować prefiksy adresów. Pamiętaj, że spowoduje to zastąpienie istniejących ustawień bramy sieci lokalnej. Użyj istniejącej nazwy bramy sieci lokalnej podczas modyfikowania, aby ustawienia zostały zastąpione. Jeśli tego nie zrobisz, zostanie utworzona nowa brama sieci lokalnej, a nie zmodyfikowana ta już istniejąca.

  ```powershell
  New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
  -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
  ```
3. Utwórz połączenie. W tym przykładzie będziemy konfigurować połączenie typu IPsec. Podczas ponownego tworzenia połączenia należy użyć typu połączenia, który został określony dla danej konfiguracji. O dodatkowych typach połączeń można przeczytać na stronie [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) (Polecenia cmdlet programu PowerShell).  Aby uzyskać nazwę bramy VirtualNetworkGateway, można uruchomić polecenie cmdlet `Get-AzureRmVirtualNetworkGateway`.
   
    Ustaw wartości zmiennych:

  ```powershell
  $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
  ```
   
    Utwórz połączenie:

  ```powershell 
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
  -Location "West US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```
