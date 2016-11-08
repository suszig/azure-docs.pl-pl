## Tworzenie sieci wirtualnej za pomocą interfejsu wiersza polecenia platformy Azure
Interfejsu wiersza polecenia platformy Azure można użyć do zarządzania zasobami Azure z poziomu wiersza polecenia dowolnego komputera z systemem Windows, Linux lub OS X. Aby utworzyć sieć wirtualną przy użyciu interfejsu wiersza polecenia platformy Azure, wykonaj poniższe kroki.

1. Jeśli po raz pierwszy używasz interfejsu wiersza polecenia Azure, zobacz temat [Install and Configure the Azure CLI](../articles/xplat-cli-install.md) (Instalacja i konfiguracja interfejsu wiersza polecenia platformy Azure) i postępuj zgodnie z instrukcjami aż do punktu, w którym należy wybrać konto platformy Azure i subskrypcję.
2. Uruchom polecenie **azure config mode**, aby włączyć tryb Resource Manager, jak pokazano poniżej.
   
        azure config mode arm
   
    Oto oczekiwane dane wyjściowe po wprowadzeniu powyższego polecenia:
   
        info:    New mode is arm
3. W razie potrzeby uruchom polecenie **azure group create**, aby utworzyć nową grupę zasobów, jak pokazano poniżej. Zwróć uwagę na dane wyjściowe polecenia. Lista wyświetlana po danych wyjściowych zawiera opis używanych parametrów. Aby uzyskać więcej informacji na temat grup zasobów, zobacz [Omówienie usługi Azure Resource Manager](../articles/resource-group-overview.md#resource-groups).
   
        azure group create -n TestRG -l centralus
   
    Oto oczekiwane dane wyjściowe po wprowadzeniu powyższego polecenia:
   
        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
   
   * **-n (lub --name)**. Nazwa nowej grupy zasobów. W naszym scenariuszu jest to *TestRG*.
   * **-l (lub --location)**. Region platformy Azure, w którym zostanie utworzona nowa grupa zasobów. W naszym scenariuszu jest to *centralus*.
4. Uruchom polecenie **azure network vnet create**, aby utworzyć sieć wirtualną i podsieć, jak pokazano poniżej. 
   
        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus
   
    Oto oczekiwane dane wyjściowe po wprowadzeniu powyższego polecenia:
   
        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK
   
   * **-g (lub --resource-group)**. Nazwa grupy zasobów, w której zostanie utworzona sieć wirtualna. W naszym scenariuszu jest to *TestRG*.
   * **-n (lub --name)**. Nazwa sieci wirtualnej, która zostanie utworzona. W naszym scenariuszu jest to *TestVNet*
   * **-a (lub --address-prefixes)**. Lista bloków CIDR wykorzystanych w celu zapewnienia przestrzeni adresowej dla sieci wirtualnej. W naszym scenariuszu jest to *192.168.0.0/16*
   * **-l (lub --location)**. Region platformy Azure, w którym zostanie utworzona sieć wirtualna. W naszym scenariuszu jest to *centralus*.
5. Uruchom polecenie **azure network vnet subnet create**, aby utworzyć podsieć, jak pokazano poniżej. Zwróć uwagę na dane wyjściowe polecenia. Lista wyświetlana po danych wyjściowych zawiera opis używanych parametrów.
   
        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24
   
    Oto oczekiwane dane wyjściowe po wprowadzeniu powyższego polecenia:
   
        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK
   
   * **-e (lub --vnet-name)**. Nazwa sieci wirtualnej, w której zostanie utworzona podsieć. W naszym scenariuszu jest to *TestVNet*.
   * **-n (lub --name)**. Nazwa nowej podsieci. W naszym scenariuszu jest to *FrontEnd*.
   * **-a (lub --address-prefix)**. Blok CIDR podsieci. W naszym scenariuszu jest to *192.168.1.0/24*.
6. W razie potrzeby powtórz krok 5 powyżej, aby utworzyć kolejne podsieci. W naszym scenariuszu należy uruchomić poniższe polecenie, aby utworzyć podsieć o nazwie *BackEnd*.
   
        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24
7. Uruchom polecenie **azure network vnet show**, aby wyświetlić właściwości nowej sieci wirtualnej, jak pokazano poniżej.
   
        azure network vnet show -g TestRG -n TestVNet
   
    Oto oczekiwane dane wyjściowe po wprowadzeniu powyższego polecenia:
   
        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK

<!--HONumber=Sep16_HO3-->


