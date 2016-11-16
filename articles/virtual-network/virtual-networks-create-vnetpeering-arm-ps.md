---
title: "Tworzenie wirtualnej sieci równorzędnej przy użyciu poleceń cmdlet programu PowerShell | Microsoft Docs"
description: "Dowiedz się, jak utworzyć sieć wirtualną przy użyciu portalu Azure w usłudze Resource Manager."
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
tags: azure-resource-manager
ms.assetid: dac579bd-7545-461a-bdac-301c87434c84
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: narayanannamalai; annahar
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 348b23b277c80867f600a408736e13b8ceb665f4


---
# <a name="create-vnet-peering-using-powershell-cmdlets"></a>Tworzenie wirtualnej sieci równorzędnej przy użyciu poleceń cmdlet programu Powershell
[!INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[!INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Aby utworzyć wirtualną sieć równorzędną przy użyciu programu PowerShell, wykonaj poniższe kroki:

1. Jeśli nie znasz programu Azure PowerShell, zapoznaj się z artykułem [Instalowanie i konfigurowanie programu Azure PowerShell](../powershell-install-configure.md) i postępuj zgodnie z instrukcjami aż do momentu logowania się w programie Azure i wyboru subskrypcji.

> [!NOTE]
> Polecenie cmdlet programu PowerShell służące do zarządzania wirtualną siecią równorzędną jest dostarczane z programem [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)
> 
> 

1. Odczytaj obiekty sieci wirtualnej:
   
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2
2. Aby ustanowić komunikację równorzędną w sieci wirtualnej, musisz utworzyć dwa połączenia, jedno dla każdego kierunku. Poniższy krok spowoduje utworzenie połączenia komunikacji równorzędnej w sieci wirtualnej z sieci VNet1 do sieci VNet2:
   
        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id
   
    Dane wyjściowe:
   
        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : vnet101
        VirtualNetworkName    : vnet1
        PeeringState        : Initiated
        ProvisioningState    : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic    : False
        AllowGatewayTransit    : False
        UseRemoteGateways    : False
        RemoteGateways        : null
        RemoteVirtualNetworkAddressSpace : null
3. Ten krok spowoduje utworzenie połączenia komunikacji równorzędnej w sieci wirtualnej z sieci VNet2 do sieci VNet1:
   
        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id
   
    Dane wyjściowe:
   
        Name            : LinkToVNet1
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : vnet101
        VirtualNetworkName    : vnet2
        PeeringState        : Connected
        ProvisioningState    : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic    : False
        AllowGatewayTransit    : False
        UseRemoteGateways    : False
        RemoteGateways        : null
        RemoteVirtualNetworkAddressSpace : null
4. Po utworzeniu połączenia komunikacji równorzędnej w sieci wirtualnej stan połączenia jest widoczny poniżej:
   
        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2
   
    Dane wyjściowe:
   
        Name            : LinkToVNet2
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : vnet101
        VirtualNetworkName    : vnet1
        PeeringState        : Connected
        ProvisioningState    : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null
   
    Istnieje kilka właściwości możliwych do skonfigurowania dla połączenia komunikacji równorzędnej sieci wirtualnej:
   
   | Opcja | Opis | Domyślne |
   |:--- |:--- |:--- |
   | AllowVirtualNetworkAccess |Określa, czy przestrzeń adresów wirtualnej sieci równorzędnej ma zostać dołączona do tagu Virtual_network. |Tak |
   | AllowForwardedTraffic |Określa, czy ruch niepochodzący z równorzędnej sieci wirtualnej jest akceptowany, czy pomijany |Nie |
   | AllowGatewayTransit |Umożliwia wirtualnej sieci równorzędnej korzystanie z bramy sieci wirtualnej. |Nie |
   | UseRemoteGateways |Określa, czy ma być używana brama wirtualnej sieci równorzędnej. Wirtualna sieć równorzędna musi mieć skonfigurowaną bramę, a właściwość AllowGatewayTransit musi być wybrana. Tej opcji nie można użyć, jeśli w sieci jest skonfigurowana brama. |Nie |
   
    Każde połączenie w wirtualnej sieci równorzędnej ma zestaw właściwości przedstawionych powyżej. Na przykład można ustawić wartość True dla właściwości AllowVirtualNetworkAccess, aby utworzyć połączenie komunikacji równorzędnej w sieci wirtualnej z sieci VNet1 do sieci VNet2, lub ustawić wartość False, aby utworzyć połączenie komunikacji równorzędnej w sieci wirtualnej w przeciwnym kierunku.
   
        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2
   
    Możesz uruchomić polecenie Get-AzureRmVirtualNetworkPeering, aby sprawdzić wartość właściwości po zmianie. Po uruchomieniu powyższych poleceń cmdlet w danych wyjściowych właściwość AllowForwardedTraffic zostanie zmieniona na wartość True.
   
        Name            : LinkToVNet2
        Id            : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : vnet101
        VirtualNetworkName    : vnet1
        PeeringState        : Connected
        ProvisioningState    : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic        : True
        AllowGatewayTransit        : False
        UseRemoteGateways        : False
        RemoteGateways        : null
        RemoteVirtualNetworkAddressSpace : null
   
    Po ustanowieniu komunikacji równorzędnej w tym scenariuszu powinno być możliwe zainicjowanie połączeń z dowolnej maszyny wirtualnej do dowolnej maszyny wirtualnej w obu sieciach wirtualnych. Domyślnie właściwość AllowVirtualNetworkAccess ma wartość True, a komunikacja równorzędna w sieci wirtualnej zainicjuje odpowiednie listy ACL, aby umożliwić komunikację między sieciami wirtualnymi. Nadal możliwe jest stosowanie reguł grupy zabezpieczeń sieci w celu blokowania łączności między poszczególnymi podsieciami lub maszynami wirtualnymi, aby umożliwić szczegółową kontrolę dostępu między dwiema sieciami wirtualnymi.  Aby uzyskać więcej informacji na temat tworzenia reguł grupy zabezpieczeń sieci, zapoznaj się z tym [artykułem](virtual-networks-create-nsg-arm-ps.md).

[!INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Aby utworzyć wirtualną sieć równorzędną między subskrypcjami przy użyciu programu PowerShell, wykonaj poniższe kroki:

1. Zaloguj się do platformy Azure przy użyciu konta użytkownika A o odpowiednich uprawnieniach dla subskrypcji A i uruchom następujące polecenie cmdlet:
   
        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5
   
    Nie jest to wymagane, ponieważ komunikację równorzędną można nawiązać nawet wtedy, gdy użytkownicy indywidualnie zgłoszą żądania komunikacji równorzędnej dla odpowiednich sieci wirtualnych, o ile zgłoszenia są zgodne. Dodanie użytkownika o odpowiednich uprawnieniach do innej sieci wirtualnej jako użytkownika w lokalnej sieci wirtualnej ułatwia konfigurowanie.
2. Zaloguj się do platformy Azure przy użyciu konta użytkownika B o odpowiednich uprawnieniach dla subskrypcji B i uruchom następujące polecenie cmdlet:
   
        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3
3. W sesji logowania użytkownika A uruchom poniższe polecenie cmdlet:
   
        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3
   
        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscription-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess
4. W sesji logowania użytkownika B uruchom poniższe polecenie cmdlet:
   
        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5
   
        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess
5. Po nawiązaniu komunikacji równorzędnej każda maszyna wirtualna w sieci VNet3 powinna mieć możliwość komunikacji z dowolną maszyną wirtualną w sieci VNet5.

[!INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. W tym scenariuszu możesz uruchomić poniższe polecenia cmdlet programu PowerShell, aby ustanowić połączenie komunikacji równorzędnej w sieci wirtualnej.  Należy ustawić wartość True dla właściwości AllowForwardedTraffic i utworzyć połączenie z sieci VNET1 do sieci HubVNet, które zezwala na ruch przychodzący spoza przestrzeni adresowej równorzędnej sieci wirtualnej.
   
        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
   
        Add-AzureRmVirtualNetworkPeering -Name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.Id -AllowForwardedTraffic
   
        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.Id
2. Po nawiązaniu komunikacji równorzędnej możesz zapoznać się z tym [artykułem](virtual-network-create-udr-arm-ps.md), aby zdefiniować trasę zdefiniowaną przez użytkownika w celu przekierowania ruchu sieci VNet1 za pośrednictwem urządzenia wirtualnego i wykorzystać jego możliwości. Podczas określania adresu następnego skoku w trasie można ustawić go na adres IP urządzenia wirtualnego w wirtualnej sieci równorzędnej HubVNet. Poniżej przedstawiono przykład:
   
        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5
   
        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route
   
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1
   
        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable
   
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

[!INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Aby utworzyć równorzędną sieć wirtualną między klasyczną siecią wirtualną i siecią wirtualną usługi Azure Resource Manager w programie PowerShell, wykonaj następujące czynności:

1. Odczytaj obiekt sieci wirtualnej dla sieci **VNET1** — sieci wirtualnej usługi Azure Resource Manager — w następujący sposób:
   
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
2. Aby ustanowić komunikację równorzędną sieci wirtualnych w ramach tego scenariusza, wymagane jest tylko jedno łącze z sieci **VNET1** do sieci **VNET2**. Ten krok wymaga znajomości identyfikatora zasobu klasycznej sieci wirtualnej. Format identyfikatora grupy zasobów wygląda następująco:
   
        /subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.ClassicNetwork/virtualNetworks/{VirtualNetworkName}
   
    Pamiętaj, aby zastąpić wartości SubscriptionID, ResourceGroupName i VirtualNetworkName odpowiednimi nazwami.
   
    Można to zrobić, wykonując następujące czynności:
   
        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2
3. Po utworzeniu połączenia komunikacji równorzędnej w sieci wirtualnej stan połączenia jest widoczny w danych wyjściowych poniżej:
   
        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## <a name="remove-vnet-peering"></a>Usuwanie wirtualnej sieci równorzędnej
1. Aby usunąć połączenie komunikacji równorzędnej w sieci wirtualnej, należy uruchomić następujące polecenie cmdlet:
   
     Remove-AzureRmVirtualNetworkPeering  
   
     Usuń oba linki za pomocą następujących poleceń:
   
     Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2   Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
2. Po usunięciu połączenia w wirtualnej sieci równorzędnej stan połączenia komunikacji równorzędnej zmieni się na Rozłączono. W tym stanie nie można ponownie utworzyć połączenia, dopóki stan połączenia nie zmieni się na Zainicjowano. Zalecamy usunięcie obu połączeń przed ponownym utworzeniem wirtualnej sieci równorzędnej.




<!--HONumber=Nov16_HO2-->


