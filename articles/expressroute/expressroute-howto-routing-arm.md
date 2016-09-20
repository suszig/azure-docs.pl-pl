<properties
   pageTitle="Jak skonfigurować routing dla obwodu usługi ExpressRoute | Microsoft Azure"
   description="Ten artykuł zawiera instrukcje tworzenia i inicjowania obsługi komunikacji równorzędnej prywatnej, publicznej i firmy Microsoft obwodu usługi ExpressRoute. W tym artykule opisano również, jak aktualizować i usuwać komunikację równoległą dla obwodu oraz sprawdzać jej stan."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/29/2016"
   ms.author="ganesr"/>

# Tworzenie i modyfikowanie routingu dla obwodu usługi ExpressRoute


> [AZURE.SELECTOR]
[Azure Portal — model usługi Resource Manager](expressroute-howto-routing-portal-resource-manager.md)
[Program PowerShell — model usługi Resource Manager](expressroute-howto-routing-arm.md)
[PowerShell — model klasyczny](expressroute-howto-routing-classic.md)



Ten artykuł zawiera instrukcje tworzenia konfiguracji routingu i zarządzania nią dla obwodu usługi ExpressRoute przy użyciu programu PowerShell i modelu wdrażania przy użyciu usługi Azure Resource Manager.  W poniższych krokach opisano również, jak sprawdzać stan komunikacji równorzędnej, aktualizować ją, usuwać i wstrzymywać jej obsługę administracyjną dla obwodu usługi ExpressRoute. 


**Modele wdrażania Azure — informacje**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## Wymagania wstępne dotyczące konfiguracji

- Potrzebna będzie najnowsza wersja modułów programu Azure PowerShell, 1.0 lub nowsza. 
- Pamiętaj, aby przed rozpoczęciem konfiguracji przejrzeć strony z [wymaganiami wstępnymi](expressroute-prerequisites.md), [wymaganiami routingu](expressroute-routing.md) oraz [przepływami pracy](expressroute-workflows.md).
- Musisz mieć aktywny obwód usługi ExpressRoute. Zanim przejdziesz dalej, postępuj zgodnie z instrukcjami, aby [utworzyć obwód usługi ExpressRoute](expressroute-howto-circuit-arm.md), który powinien zostać włączony przez dostawcę połączenia. Obwód usługi ExpressRoute musi być zainicjowany i włączony, aby można było uruchamiać polecenia cmdlet opisane poniżej.

Te instrukcje dotyczą tylko obwodów utworzonych przy pomocy dostawców oferujących usługi łączności warstwy 2. Jeśli korzystasz z usług dostawcy oferującego zarządzane usługi warstwy 3 (zwykle IPVPN, np. MPLS), dostawca połączenia skonfiguruje routing i będzie nim zarządzać.

>[AZURE.IMPORTANT] Obecnie nie anonsujemy komunikacji równorzędnej skonfigurowanej przez dostawców usług w portalu zarządzania usługami. Pracujemy nad tym, by wkrótce włączyć tę funkcję. Sprawdź, czy oferuje ją dostawca usług, przed skonfigurowaniem komunikacji równorzędnej BGP.

Można skonfigurować jedną komunikację równorzędną, dwie lub trzy (prywatną Azure, publiczną Azure i Microsoft) dla obwodu usługi ExpressRoute. Możesz skonfigurować komunikację równorzędną w dowolnej kolejności. Musisz jednak pamiętać, aby kończyć konfiguracje poszczególnych komunikacji równorzędnych pojedynczo. 

## Prywatna komunikacja równorzędna Azure

Ta sekcja zawiera instrukcje dotyczące tworzenia, pobierania, aktualizowania i usuwania konfiguracji prywatnej komunikacji równorzędnej Azure dla obwodu usługi ExpressRoute. 

### Aby utworzyć prywatną komunikację równorzędną

1. Zaimportuj moduł programu PowerShell dla usługi ExpressRoute.
    
    Aby zacząć używać poleceń cmdlet usługi ExpressRoute, musisz zainstalować najnowszą wersję Instalatora programu PowerShell z [galerii programu PowerShell](http://www.powershellgallery.com/) i zaimportować moduły usługi Azure Resource Manager do sesji programu PowerShell. Musisz uruchomić program PowerShell jako administrator.

        Install-Module AzureRM

        Install-AzureRM

    Zaimportuj wszystkie moduły AzureRM.* w ramach znanego semantycznego zakresu wersji.

        Import-AzureRM

    Możesz też po prostu zaimportować wybrany moduł w ramach znanego semantycznego zakresu wersji. 
        
        Import-Module AzureRM.Network 

    Zaloguj się do konta.

        Login-AzureRmAccount

    Wybierz subskrypcję, w ramach której chcesz utworzyć obwód usługi ExpressRoute.
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Utwórz obwód usługi ExpressRoute.
    
    Wypełnij instrukcje, aby utworzyć [obwód usługi ExpressRoute](expressroute-howto-circuit-arm.md), który zostanie zainicjowany przez dostawcę połączenia. 

    Jeśli dostawca połączenia oferuje zarządzane usługi warstwy 3, możesz poprosić go o włączenie prywatnej komunikacji równorzędnej Azure. W takiej sytuacji nie trzeba będzie wykonywać instrukcji wymienionych w następnych sekcjach. Jednak jeśli dostawca połączenia nie zarządza routingiem, po utworzeniu obwodu postępuj zgodnie z poniższymi instrukcjami. 

3. Sprawdź obwód usługi ExpressRoute, aby upewnić się, że jest zainicjowany.

    Musisz najpierw sprawdzić, czy obwód usługi ExpressRoute jest zainicjowany i włączony. Zobacz przykład poniżej.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Odpowiedź będzie przypominać przykład poniżej:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []


4. Skonfiguruj prywatną komunikację równorzędną Azure dla obwodu.

    Zanim przejdziesz do następnych kroków, upewnij się, czy masz następujące elementy:

    - Podsieć /30 dla połączenia podstawowego. Nie może ona być częścią żadnej przestrzeni adresowej zarezerwowanej dla sieci wirtualnych.
    - Podsieć /30 dla połączenia dodatkowego. Nie może ona być częścią żadnej przestrzeni adresowej zarezerwowanej dla sieci wirtualnych.
    - Prawidłowy identyfikator sieci VLAN do ustanowienia tej komunikacji równorzędnej jest włączony. Upewnij się, że żadna inna komunikacja równorzędna w obwodzie nie używa tego samego identyfikatora VLAN.
    - Numer AS do komunikacji równorzędnej. Możesz używać 2-bajtowych i 4-bajtowych numerów AS. Możesz użyć prywatnego numeru AS dla tej komunikacji równorzędnej. Pamiętaj, aby nie używać numeru 65515.
    - Skrót MD5, jeśli zdecydujesz się go użyć. **Jest to opcjonalne**.
    
    Możesz uruchomić następujące polecenie cmdlet, aby skonfigurować prywatną komunikację równorzędną Azure dla obwodu.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Możesz użyć poniższego polecenia cmdlet, jeśli zdecydujesz się używać skrótu MD5.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    >[AZURE.IMPORTANT] Pamiętaj, aby określić numer AS jako ASN komunikacji równorzędnej, a nie ASN klienta.

### Aby wyświetlić szczegóły dotyczące prywatnej komunikacji równorzędnej Azure

Możesz pobrać szczegóły dotyczące konfiguracji przy użyciu następującego polecenia cmdlet.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt   


### Aby zaktualizować konfigurację prywatnej komunikacji równorzędnej Azure

Możesz zaktualizować dowolną część konfiguracji za pomocą następującego polecenia cmdlet. W poniższym przykładzie identyfikator sieci VLAN obwodu jest aktualizowany ze 100 do 500.

    Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### Aby usunąć prywatną komunikację równorzędną Azure

Możesz usunąć konfigurację komunikacji równorzędnej, uruchamiając następujące polecenie cmdlet.

>[AZURE.WARNING] Przed uruchomieniem tego polecenia cmdlet upewnij się, że wszystkie sieci wirtualne zostały odłączone od obwodu usługi ExpressRoute. 

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt



## Publiczna komunikacja równorzędna Azure

Ta sekcja zawiera instrukcje dotyczące tworzenia, pobierania, aktualizowania i usuwania konfiguracji publicznej komunikacji równorzędnej Azure dla obwodu usługi ExpressRoute.

### Aby utworzyć publiczną komunikację równorzędną Azure

1. Zaimportuj moduł programu PowerShell dla usługi ExpressRoute.
    
    Aby zacząć używać poleceń cmdlet usługi ExpressRoute, musisz zainstalować najnowszą wersję Instalatora programu PowerShell z [galerii programu PowerShell](http://www.powershellgallery.com/) i zaimportować moduły usługi Azure Resource Manager do sesji programu PowerShell. Musisz uruchomić program PowerShell jako administrator.

        Install-Module AzureRM

        Install-AzureRM

    Zaimportuj wszystkie moduły AzureRM.* w ramach znanego semantycznego zakresu wersji.

        Import-AzureRM

    Możesz też po prostu zaimportować wybrany moduł w ramach znanego semantycznego zakresu wersji. 
        
        Import-Module AzureRM.Network 

    Zaloguj się do konta.

        Login-AzureRmAccount

    Wybierz subskrypcję, w ramach której chcesz utworzyć obwód usługi ExpressRoute.
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Utwórz obwód usługi ExpressRoute.
    
    Wypełnij instrukcje, aby utworzyć [obwód usługi ExpressRoute](expressroute-howto-circuit-arm.md), który zostanie zainicjowany przez dostawcę połączenia. 

    Jeśli dostawca połączenia oferuje zarządzane usługi warstwy 3, możesz poprosić go o włączenie publicznej komunikacji równorzędnej Azure. W takiej sytuacji nie trzeba będzie wykonywać instrukcji wymienionych w następnych sekcjach. Jednak jeśli dostawca połączenia nie zarządza routingiem, po utworzeniu obwodu postępuj zgodnie z poniższymi instrukcjami.

3. Sprawdź obwód usługi ExpressRoute, aby upewnić się, że jest zainicjowany.

    Musisz najpierw sprawdzić, czy obwód usługi ExpressRoute jest zainicjowany i włączony. Zobacz przykład poniżej.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Odpowiedź będzie przypominać przykład poniżej:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   

4. Skonfiguruj publiczną konfigurację równorzędną Azure dla obwodu.

    Zanim przejdziesz dalej, upewnij się, że masz poniższe informacje.

    - Podsieć /30 dla połączenia podstawowego. Musi to być prawidłowy publiczny prefiks IPv4.
    - Podsieć /30 dla połączenia dodatkowego. Musi to być prawidłowy publiczny prefiks IPv4.
    - Prawidłowy identyfikator sieci VLAN do ustanowienia tej komunikacji równorzędnej jest włączony. Upewnij się, że żadna inna komunikacja równorzędna w obwodzie nie używa tego samego identyfikatora VLAN.
    - Numer AS do komunikacji równorzędnej. Możesz używać 2-bajtowych i 4-bajtowych numerów AS.
    - Skrót MD5, jeśli zdecydujesz się go użyć. **Jest to opcjonalne**.
    
    Możesz uruchomić następujące polecenie cmdlet, aby skonfigurować publiczną komunikację równorzędną Azure dla obwodu.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Możesz użyć poniższego polecenia cmdlet, jeśli zdecydujesz się używać skrótu MD5.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


    >[AZURE.IMPORTANT] Pamiętaj, aby określić numer AS jako ASN komunikacji równorzędnej, a nie ASN klienta.

### Aby wyświetlić szczegóły dotyczące publicznej komunikacji równorzędnej Azure

Możesz pobrać szczegóły dotyczące konfiguracji przy użyciu następującego polecenia cmdlet.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt


### Aby zaktualizować konfigurację publicznej komunikacji równorzędnej Azure

Możesz zaktualizować dowolną część konfiguracji za pomocą następującego polecenia cmdlet.

    Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -Circuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600 

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

W przykładzie powyżej identyfikator sieci VLAN obwodu jest aktualizowany z 200 do 600.

### Aby usunąć publiczną komunikację równorzędną Azure

Możesz usunąć konfigurację komunikacji równorzędnej, uruchamiając następujące polecenie cmdlet.

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## Komunikacja równorzędna firmy Microsoft

Ta sekcja zawiera instrukcje dotyczące tworzenia, pobierania, aktualizowania i usuwania konfiguracji komunikacji równorzędnej Microsoft dla obwodu usługi ExpressRoute. 

### Aby utworzyć komunikację równorzędną Microsoft

1. Zaimportuj moduł programu PowerShell dla usługi ExpressRoute.
    
    Aby zacząć używać poleceń cmdlet usługi ExpressRoute, musisz zainstalować najnowszą wersję Instalatora programu PowerShell z [galerii programu PowerShell](http://www.powershellgallery.com/) i zaimportować moduły usługi Azure Resource Manager do sesji programu PowerShell. Musisz uruchomić program PowerShell jako administrator.

        Install-Module AzureRM

        Install-AzureRM

    Zaimportuj wszystkie moduły AzureRM.* w ramach znanego semantycznego zakresu wersji.

        Import-AzureRM

    Możesz też po prostu zaimportować wybrany moduł w ramach znanego semantycznego zakresu wersji. 
        
        Import-Module AzureRM.Network 

    Zaloguj się do konta.

        Login-AzureRmAccount

    Wybierz subskrypcję, w ramach której chcesz utworzyć obwód usługi ExpressRoute.
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Utwórz obwód usługi ExpressRoute.
    
    Wypełnij instrukcje, aby utworzyć [obwód usługi ExpressRoute](expressroute-howto-circuit-arm.md), który zostanie zainicjowany przez dostawcę połączenia. 

    Jeśli dostawca połączenia oferuje zarządzane usługi warstwy 3, możesz poprosić go o włączenie prywatnej komunikacji równorzędnej Azure. W takiej sytuacji nie trzeba będzie wykonywać instrukcji wymienionych w następnych sekcjach. Jednak jeśli dostawca połączenia nie zarządza routingiem, po utworzeniu obwodu postępuj zgodnie z poniższymi instrukcjami.

3. Sprawdź obwód usługi ExpressRoute, aby upewnić się, że jest zainicjowany.

    Musisz najpierw sprawdzić, czy obwód usługi ExpressRoute jest zainicjowany i włączony. Zobacz przykład poniżej.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Odpowiedź będzie przypominać przykład poniżej:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   
4. Skonfiguruj komunikację równorzędną Microsoft dla obwodu.

    Zanim przejdziesz dalej, upewnij się, że masz poniższe informacje.

    - Podsieć /30 dla połączenia podstawowego. Musi to być prawidłowy publiczny prefiks IPv4, którego jesteś właścicielem, zarejestrowany w RIR/IRR.
    - Podsieć /30 dla połączenia dodatkowego. Musi to być prawidłowy publiczny prefiks IPv4, którego jesteś właścicielem, zarejestrowany w RIR/IRR.
    - Prawidłowy identyfikator sieci VLAN do ustanowienia tej komunikacji równorzędnej jest włączony. Upewnij się, że żadna inna komunikacja równorzędna w obwodzie nie używa tego samego identyfikatora VLAN.
    - Numer AS do komunikacji równorzędnej. Możesz używać 2-bajtowych i 4-bajtowych numerów AS.
    - Anonsowane prefiksy: musisz podać listę wszystkich prefiksów, które planujesz anonsować za pośrednictwem sesji BGP. Akceptowane są tylko prefiksy publicznych adresów IP. Jeśli zamierzasz wysłać zestaw prefiksów, możesz wysłać listę oddzielaną przecinkami. Prefiksy te muszą być zarejestrowane na Ciebie w RIR/IRR.
    - Numer ASN klienta: jeśli anonsujesz prefiksy, które nie są rejestrowane do numeru AS komunikacji równorzędnej, możesz określić numer AS, do którego są rejestrowane. **Jest to opcjonalne**.
    - Nazwa rejestru routingu: możesz określić RIR/IRR, względem którego rejestrowany jest numer AS i prefiksy.
    - Skrót MD5, jeśli zdecydujesz się go użyć. **Jest to opcjonalne.**
    
    Możesz uruchomić następujące polecenie cmdlet, aby skonfigurować komunikację równorzędną firmy Microsoft dla obwodu.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -Circuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### Aby pobrać szczegóły dotyczące komunikacji równorzędnej firmy Microsoft

Możesz pobrać szczegóły dotyczące konfiguracji przy użyciu następującego polecenia cmdlet.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -Circuit $ckt


### Aby zaktualizować konfigurację komunikacji równorzędnej firmy Microsoft

Możesz zaktualizować dowolną część konfiguracji za pomocą następującego polecenia cmdlet.

        Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -Circuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
        

### Aby usunąć komunikację równorzędną firmy Microsoft

Możesz usunąć konfigurację komunikacji równorzędnej, uruchamiając następujące polecenie cmdlet.

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -Circuit $ckt

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## Następne kroki

Następny krok: [Link a VNet to an ExpressRoute circuit](expressroute-howto-linkvnet-arm.md) (Łączenie sieci wirtualnej z obwodem usługi ExpressRoute).

-  Więcej informacji na temat przepływów pracy usługi ExpressRoute znajduje się w artykule [ExpressRoute workflows](expressroute-workflows.md) (Przepływy pracy usługi ExpressRoute).

-  Aby uzyskać więcej informacji o komunikacji równorzędnej obwodu, zobacz artykuł [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (Obwody i domeny routingu usługi ExpressRoute).

-  Więcej informacji na temat pracy z sieciami wirtualnymi znajduje się w artykule [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Omówienie sieci wirtualnych).




<!--HONumber=sep16_HO1-->


