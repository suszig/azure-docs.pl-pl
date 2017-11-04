## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Tworzenie klasycznej sieci wirtualnej przy użyciu wiersza polecenia platformy Azure
Interfejsu wiersza polecenia platformy Azure można użyć do zarządzania zasobami Azure z poziomu wiersza polecenia dowolnego komputera z systemem Windows, Linux lub OS X. Aby utworzyć sieć wirtualną przy użyciu interfejsu wiersza polecenia platformy Azure, wykonaj poniższe kroki.

1. Jeśli po raz pierwszy używasz interfejsu wiersza polecenia Azure, zobacz artykuł [Instalowanie i konfigurowania interfejsu wiersza polecenia Azure](../articles/cli-install-nodejs.md) i postępuj zgodnie z instrukcjami aż do punktu, w którym należy wybrać konto platformy Azure i subskrypcję.
2. Uruchom polecenie **azure network vnet create**, aby utworzyć sieć wirtualną i podsieć, jak pokazano poniżej. Lista wyświetlana po danych wyjściowych zawiera opis używanych parametrów.
   
            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
   
    Oczekiwane dane wyjściowe:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK
   
   * **--vnet**. Nazwa sieci wirtualnej, która zostanie utworzona. W naszym scenariuszu jest to *TestVNet*
   * **-e (lub--przestrzeni adresowej)**. Przestrzeń adresowa sieci wirtualnej. W naszym scenariuszu *192.168.0.0*
   * **-i (lub - cidr)**. Maska sieci w formacie CIDR. W naszym scenariuszu *16*.
   * **-n (lub--nazwy podsieci**). Nazwa pierwszej podsieci. W naszym scenariuszu jest to *FrontEnd*.
   * **-p (lub--podsieci start-ip)**. Początkowy adres IP dla podsieci lub przestrzeni adresowej podsieci. W naszym scenariuszu *192.168.1.0*.
   * **-r (lub--podsieci cidr)**. Maska sieci w formacie CIDR podsieci. W naszym scenariuszu *24*.
   * **-l (lub --location)**. Region platformy Azure, w którym zostanie utworzona sieć wirtualna. W naszym scenariuszu *środkowe stany USA*.
3. Uruchom polecenie **azure network vnet subnet create**, aby utworzyć podsieć, jak pokazano poniżej. Lista wyświetlana po danych wyjściowych zawiera opis używanych parametrów.
   
            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
   
    Oto oczekiwane dane wyjściowe po wprowadzeniu powyższego polecenia:
   
            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK
   
   * **-t (lub--vnet-name**. Nazwa sieci wirtualnej, w której zostanie utworzona podsieć. W naszym scenariuszu jest to *TestVNet*.
   * **-n (lub --name)**. Nazwa nowej podsieci. W naszym scenariuszu *zaplecza*.
   * **-a (lub --address-prefix)**. Blok CIDR podsieci. Cztery naszym scenariuszu *192.168.2.0/24*.
4. Uruchom polecenie **azure network vnet show**, aby wyświetlić właściwości nowej sieci wirtualnej, jak pokazano poniżej.
   
            azure network vnet show
   
    Oto oczekiwane dane wyjściowe po wprowadzeniu powyższego polecenia:
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

