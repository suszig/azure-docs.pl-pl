## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>Wdrażanie szablonu ARM przy użyciu interfejsu wiersza polecenia platformy Azure
Aby wdrożyć pobrany szablon ARM przy użyciu interfejsu wiersza polecenia (CLI) Azure, wykonaj poniższe kroki.

1. Jeśli po raz pierwszy używasz interfejsu wiersza polecenia Azure, zobacz artykuł [Instalowanie i konfigurowania interfejsu wiersza polecenia Azure](../articles/cli-install-nodejs.md) i postępuj zgodnie z instrukcjami aż do punktu, w którym należy wybrać konto platformy Azure i subskrypcję.
2. Uruchom polecenie **`azure config mode`**, aby włączyć tryb Resource Manager, jak pokazano poniżej.
   
        azure config mode arm
   
    Oto oczekiwane dane wyjściowe po wprowadzeniu powyższego polecenia:
   
        info:    New mode is arm
3. W razie potrzeby uruchom polecenie **`azure group create`**, aby utworzyć nową grupę zasobów, jak pokazano poniżej. Zwróć uwagę na dane wyjściowe polecenia. Lista wyświetlana po danych wyjściowych zawiera opis używanych parametrów. Aby uzyskać więcej informacji na temat grup zasobów, zobacz [Omówienie usługi Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md).
   
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
4. Uruchom polecenie cmdlet **`azure group deployment create`**, aby wdrożyć nową sieć wirtualną przy użyciu uprzednio pobranego i zmodyfikowanego pliku szablonu i pliku parametrów. Lista wyświetlana po danych wyjściowych zawiera opis używanych parametrów.
   
        azure group deployment create -g TestRG -n TestVNetDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json
   
    Oto oczekiwane dane wyjściowe po wprowadzeniu powyższego polecenia:
   
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestVNetDeployment"
        + Registering providers
        info:    Registering provider microsoft.network
        + Waiting for deployment to complete
        data:    DeploymentName     : TestVNetDeployment
        data:    ResourceGroupName  : TestRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-08-14T21:56:11.152759Z
        data:    Mode               : Incremental
        data:    Name           Type    Value
        data:    -------------  ------  --------------
        data:    location       String  Central US
        data:    vnetName       String  TestVNet
        data:    addressPrefix  String  192.168.0.0/16
        data:    subnet1Prefix  String  192.168.1.0/24
        data:    subnet1Name    String  FrontEnd
        data:    subnet2Prefix  String  192.168.2.0/24
        data:    subnet2Name    String  BackEnd
        info:    group deployment create command OK
   
   * **-g (lub --resource-group)**. Nazwa grupy zasobów, w której zostanie utworzona nowa sieć wirtualna.
   * **-f (lub --template-file)**. Ścieżka do pliku szablonu ARM.
   * **-e (lub --parameters-file)**. Ścieżka do pliku parametrów ARM.
5. Uruchom polecenie **`azure network vnet show`**, aby wyświetlić właściwości nowej sieci wirtualnej, jak pokazano poniżej.
   
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

