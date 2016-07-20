### Dodawanie lub usuwanie prefiksów, gdy jeszcze nie utworzono połączenia bramy sieci VPN

- **Aby dodać** dodatkowe prefiksy adresów do bramy sieci lokalnej, która została już utworzona, ale nie ma jeszcze połączenia z bramą sieci VPN, skorzystaj z poniższego przykładu.

        $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')


- **Aby usunąć** prefiks adresu z bramy sieci lokalnej, która nie ma połączenia sieci VPN, skorzystaj z poniższego przykładu. Opuść prefiksy, które nie są już potrzebne. W tym przykładzie nie są już potrzebne prefiksy 20.0.0.0/24 (z poprzedniego przykładu), dlatego zostanie zaktualizowana brama sieci lokalnej i zostaną one wykluczone.

        $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### Dodawanie lub usuwanie prefiksów, gdy zostało już utworzone połączenie bramy sieci VPN

Jeśli połączenie VPN już jest utworzone i chcesz dodać lub usunąć prefiksy adresów IP zawarte w bramie sieci lokalnej, wykonaj kolejno następujące czynności. Spowoduje to pewien przestój połączenia sieci VPN.

>[AZURE.IMPORTANT] Nie należy usuwać bramy sieci VPN. W przypadku jej usunięcia należy wrócić do poprzednich kroków, aby utworzyć ją ponownie, a także ponownie skonfigurować router lokalny przy użyciu nowych ustawień.
 
1. Usuń połączenie protokołu IPsec. 
2. Zmodyfikuj prefiksy dla bramy sieci lokalnej. 
3. Utwórz nowe połączenie protokołu IPsec. 

Poniższego przykładu można użyć jako wskazówki.

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

    Remove-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg

    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
    Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
    
    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'



<!--HONumber=Jun16_HO2-->


