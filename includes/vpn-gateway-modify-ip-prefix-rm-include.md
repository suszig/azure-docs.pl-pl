### <a name="noconnection"></a>Dodawanie lub usuwanie prefiksów — brak połączenia bramy

- **Aby dodać** dodatkowe prefiksy adresów do bramy sieci lokalnej, która została utworzona, ale nie ma jeszcze połączenia z bramą, skorzystaj z poniższego przykładu. Pamiętaj, aby zastąpić podane wartości swoimi.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

- **Aby usunąć** prefiks adresu z bramy sieci lokalnej, która nie ma połączenia sieci VPN, skorzystaj z poniższego przykładu. Opuść prefiksy, które nie są już potrzebne. W tym przykładzie nie są już potrzebne prefiksy 20.0.0.0/24 (z poprzedniego przykładu), dlatego zostanie zaktualizowana brama sieci lokalnej i zostaną one wykluczone.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>Dodawanie lub usuwanie prefiksów — istniejące połączenie bramy

Jeśli połączenie bramy zostało już utworzone i chcesz dodać lub usunąć prefiksy adresów IP zawarte w bramie Twojej sieci lokalnej, wykonaj kolejno następujące kroki. Spowoduje to pewien przestój połączenia sieci VPN. Podczas aktualizowania prefiksów najpierw należy usunąć połączenie, zmodyfikować prefiksy, a następnie utworzyć nowe połączenie. W poniższych przykładach należy zastąpić podane wartości swoimi.

>[AZURE.IMPORTANT] Nie należy usuwać bramy sieci VPN. W przypadku jej usunięcia należy wrócić do poprzednich kroków, aby utworzyć ją ponownie, a także ponownie skonfigurować router lokalny przy użyciu nowych ustawień.
 
1. Usuń połączenie.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName

2. Zmodyfikuj prefiksy adresów IP bramy Twojej sieci lokalnej.

    Ustaw wartość zmiennej dla klasy LocalNetworkGateway.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName

    Zmodyfikuj prefiksy.

        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. Utwórz połączenie. W tym przykładzie będziemy konfigurować połączenie typu IPsec. Podczas ponownego tworzenia połączenia należy użyć typu połączenia, który został określony dla danej konfiguracji. O dodatkowych typach połączeń można przeczytać na stronie [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) (Polecenia cmdlet programu PowerShell).

    Ustaw wartość zmiennej dla klasy VirtualNetworkGateway.

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName

    Utwórz połączenie. Należy zauważyć, że w przykładzie użyto zmiennej $local, która została ustawiona w poprzednim kroku.


        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'


<!--HONumber=Sep16_HO3-->


