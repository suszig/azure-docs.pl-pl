## <a name="how-to-create-a-vnet-using-powershell"></a>Tworzenie sieci wirtualnej za pomocą programu PowerShell
Aby utworzyć sieć wirtualną przy użyciu programu PowerShell, wykonaj poniższe kroki.

1. Jeśli nie znasz programu Azure PowerShell, zapoznaj się z artykułem [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azureps-cmdlets-docs) i postępuj zgodnie z instrukcjami aż do momentu logowania się w programie Azure i wyboru subskrypcji.
2. W razie potrzeby utwórz nową grupę zasobów, jak pokazano poniżej. Na potrzeby tego scenariusza wymagane jest utworzenie grupy zasobów o nazwie *TestRG*. Aby uzyskać więcej informacji na temat grup zasobów, zobacz temat [Omówienie usługi Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md).
   
        New-AzureRmResourceGroup -Name TestRG -Location centralus
   
    Oczekiwane dane wyjściowe:
   
        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG    
3. Utwórz nową sieć wirtualną o nazwie *TestVNet*, jak pokazano poniżej.
   
        New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
            -AddressPrefix 192.168.0.0/16 -Location centralus    
   
    Oczekiwane dane wyjściowe:
   
        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                   : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState          : Succeeded
        Tags                       : 
        AddressSpace               : {
                                   "AddressPrefixes": [
                                     "192.168.0.0/16"
                                   ]
                                  }
        DhcpOptions                : {}
        Subnets                    : []
        VirtualNetworkPeerings     : []
4. Zapisz obiekt sieci wirtualnej w ramach zmiennej, jak pokazano poniżej.
   
        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
   
   > [!TIP]
   > Można połączyć kroki 3 i 4, uruchamiając polecenie **$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus**.
   > 
   > 
5. Dodaj podsieć do nowej zmiennej sieci wirtualnej, jak pokazano poniżej.
   
        Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd `
            -VirtualNetwork $vnet -AddressPrefix 192.168.1.0/24
   
    Oczekiwane dane wyjściowe:
   
        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                  : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState     : Succeeded
        Tags                  :
        AddressSpace          : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions           : {}
        Subnets             : [
                                  {
                                    "Name": "FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24"
                                  }
                                ]
        VirtualNetworkPeerings     : []
6. Powtórz krok 5 opisany powyżej dla każdej podsieci, którą chcesz utworzyć. Poniższe polecenie pozwala utworzyć w naszym scenariuszu podsieć o nazwie *BackEnd*.
   
        Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd `
            -VirtualNetwork $vnet -AddressPrefix 192.168.2.0/24
7. Chociaż w toku tego procesu są tworzone podsieci, to na tym etapie istnieją one tylko w zmiennej lokalnej używanej do pobierania sieci wirtualnej, która została utworzona w kroku 4. Aby zapisać zmiany na platformie Azure, uruchom polecenie cmdlet **Set-AzureRmVirtualNetwork**, jak pokazano poniżej.
   
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet    
   
    Oczekiwane dane wyjściowe:
   
        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                  : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState     : Succeeded
        Tags                  :
        AddressSpace          : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions           : {
                                  "DnsServers": []
                                }
        Subnets               : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  },
                                  {
                                    "Name": "BackEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                    "AddressPrefix": "192.168.2.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  }
                                ]
        VirtualNetworkPeerings : []



<!--HONumber=Jan17_HO3-->


