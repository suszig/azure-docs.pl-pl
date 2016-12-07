---
title: "Tworzenie wirtualnej sieci równorzędnej przy użyciu szablonów usługi Resource Manager | Microsoft Docs"
description: "Dowiedz się, jak utworzyć wirtualną sieć równorzędną przy użyciu szablonów w usłudze Resource Manager."
services: virtual-network
documentationcenter: 
author: narayanannamalai
manager: jefco
editor: 
tags: azure-resource-manager
ms.assetid: 75f8d10e-23e8-44bd-9972-aab74048cf38
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: narayanannamalai;annahar
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 5af02963f139648d9f1b662f2da913ffa0d6f128


---
# <a name="create-vnet-peering-using-resource-manager-templates"></a>Tworzenie wirtualnej sieci równorzędnej przy użyciu szablonów usługi Resource Manager
[!INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[!INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Aby utworzyć wirtualną sieć równorzędną przy użyciu szablonów Resource Manager, wykonaj poniższe kroki:

1. Jeśli nie znasz programu Azure PowerShell, zapoznaj się z artykułem [Instalowanie i konfigurowanie programu Azure PowerShell](../powershell-install-configure.md) i postępuj zgodnie z instrukcjami aż do momentu logowania się w programie Azure i wyboru subskrypcji.
   
   > [!NOTE]
   > Polecenie cmdlet programu PowerShell służące do zarządzania wirtualną siecią równorzędną jest dostarczane z programem [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)
   > 
   > 
2. Tekst poniżej zawiera definicję połączenia wirtualnych sieci równorzędnych (VNet1 z VNet2) na podstawie powyższego scenariusza. Skopiuj poniższą zawartość i zapisz ją w pliku o nazwie VNetPeeringVNet1.json.
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }
3. Sekcja poniżej zawiera definicję połączenia wirtualnych sieci równorzędnych (VNet2 z VNet1) na podstawie powyższego scenariusza.  Skopiuj poniższą zawartość i zapisz ją w pliku o nazwie VNetPeeringVNet2.json.
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }
   
    Jak widać w powyższym szablonie, istnieje kilka właściwości możliwych do skonfigurowania dla wirtualnej sieci równorzędnej:
   
   | Opcja | Opis | Domyślne |
   |:--- |:--- |:--- |
   | AllowVirtualNetworkAccess |Określa, czy przestrzeń adresów wirtualnej sieci równorzędnej ma zostać dołączona do tagu virtual_network. |Tak |
   | AllowForwardedTraffic |Określa, czy ruch niepochodzący z wirtualnej sieci równorzędnej jest akceptowany czy pomijany. |Nie |
   | AllowGatewayTransit |Umożliwia wirtualnej sieci równorzędnej korzystanie z bramy sieci wirtualnej. |Nie |
   | UseRemoteGateways |Określa, czy ma być używana brama wirtualnej sieci równorzędnej. Wirtualna sieć równorzędna musi mieć skonfigurowaną bramę, a właściwość AllowGatewayTransit musi być wybrana. Tej opcji nie można użyć, jeśli jest skonfigurowana brama. |Nie |
   
    Każde połączenie w wirtualnej sieci równorzędnej ma zestaw właściwości przedstawionych powyżej. Na przykład można ustawić wartość True dla właściwości AllowVirtualNetworkAccess, aby utworzyć połączenie komunikacji równorzędnej w sieci wirtualnej z sieci VNet1 do sieci VNet2, lub ustawić wartość False, aby utworzyć połączenie komunikacji równorzędnej w sieci wirtualnej w przeciwnym kierunku.
4. Aby wdrożyć plik szablonu, możesz uruchomić polecenie cmdlet New-AzureRmResourceGroupDeployment w celu utworzenia lub zaktualizowania wdrożenia. Aby uzyskać więcej informacji na temat korzystania z szablonów usługi Resource Manager, zapoznaj się z tym [artykułem](../resource-group-template-deploy.md).
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all
   
   > [!NOTE]
   > Zamień nazwy grupy zasobów i plik szablonu zasobów zgodnie z wymaganiami.
   > 
   > 
   
    Poniżej znajduje się przykład przygotowany na podstawie powyższego scenariusza:
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all
   
    Dane wyjściowe:
   
        DeploymentName        : VNetPeeringVNet1
        ResourceGroupName    : VNet101
        ProvisioningState        : Succeeded
        Timestamp            : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters            :
        Outputs            :
        DeploymentDebugLogLevel : RequestContent, ResponseContent
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all
   
    Dane wyjściowe:
   
        DeploymentName        : VNetPeeringVNet2
        ResourceGroupName    : VNet101
        ProvisioningState        : Succeeded
        Timestamp            : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters            :
        Outputs            :
        DeploymentDebugLogLevel : RequestContent, ResponseContent
5. Po zakończeniu wdrożenia można uruchomić poniższe polecenie cmdlet poniżej, aby wyświetlić stan komunikacji równorzędnej:
   
        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2
   
    Dane wyjściowe:
   
        Name            : LinkToVNet2
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : VNet101
        VirtualNetworkName    : VNet1
        ProvisioningState        : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null
   
    Po ustanowieniu komunikacji równorzędnej w tym scenariuszu powinno być możliwe zainicjowanie połączeń z dowolnej maszyny wirtualnej do dowolnej maszyny wirtualnej w obu sieciach wirtualnych. Domyślnie właściwość AllowVirtualNetworkAccess ma wartość True, a komunikacja równorzędna w sieci wirtualnej zainicjuje odpowiednie listy ACL, aby umożliwić komunikację między sieciami wirtualnymi. Nadal możliwe jest stosowanie reguł grupy zabezpieczeń sieci w celu blokowania łączności między określonymi podsieciami lub maszynami wirtualnymi, aby umożliwić szczegółową kontrolę dostępu między dwiema sieciami wirtualnymi.  Aby uzyskać więcej informacji na temat tworzenia reguł grupy zabezpieczeń sieci, zapoznaj się z tym [artykułem](virtual-networks-create-nsg-arm-ps.md).

[!INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Aby utworzyć wirtualną sieć równorzędną między subskrypcjami, wykonaj poniższe kroki:

1. Zaloguj się do platformy Azure przy użyciu konta użytkownika A o odpowiednich uprawnieniach dotyczących subskrypcji A i uruchom następujące polecenie cmdlet:
   
        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5
   
    Nie jest to wymagane, ponieważ komunikację równorzędną można nawiązać, nawet jeśli użytkownicy indywidualnie zgłoszą żądania komunikacji równorzędnej dla odpowiednich sieci wirtualnych, o ile zgłoszenia są zgodne. Dodanie użytkowników o odpowiednich uprawnieniach do innej sieci wirtualnej jako użytkowników w lokalnej sieci wirtualnej ułatwia konfigurowanie.
2. Zaloguj się do platformy Azure przy użyciu konta użytkownika B o odpowiednich uprawnieniach dla subskrypcji B i uruchom następujące polecenie cmdlet:
   
        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3
3. W sesji logowania użytkownika A uruchom to polecenie cmdlet:
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all
   
    Oto jak jest zdefiniowany plik JSON.  
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }
4. W sesji logowania użytkownika B uruchom następujące polecenie cmdlet:
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all
   
    Oto jak jest zdefiniowany plik JSON:
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }
   
     Po ustanowieniu komunikacji równorzędnej w tym scenariuszu powinno być możliwe zainicjowanie połączeń z dowolnej maszyny wirtualnej do dowolnej maszyny wirtualnej w obu sieciach wirtualnych w różnych subskrypcjach.

[!INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. W tym scenariuszu możesz wdrożyć ten sam szablon w celu ustanowienia komunikacji równorzędnej w sieci wirtualnej.  Dla właściwości AllowForwardedTraffic trzeba ustawić wartość True, dzięki czemu sieciowe urządzenie wirtualne w wirtualnej sieci równorzędnej będzie mogło wysyłać i odbierać dane.
   
    Oto szablon umożliwiający utworzenie komunikacji równorzędnej w sieci wirtualnej prowadzącej z sieci HubVNet do sieci VNet1. Zwróć uwagę, że parametr AllowForwardedTraffic ma wartość False.
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }
2. Oto szablon umożliwiający utworzenie komunikacji równorzędnej w sieci wirtualnej prowadzącej z sieci VNet1 do sieci HubVNet. Zwróć uwagę, że parametr AllowForwardedTraffic ma wartość True.
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }
3. Po nawiązaniu komunikacji równorzędnej można zapoznać się z tym [artykułem](virtual-network-create-udr-arm-ps.md), aby zdefiniować trasę zdefiniowaną przez użytkownika w celu przekierowania ruchu sieci VNet1 za pośrednictwem urządzenia wirtualnego i wykorzystać jego możliwości. Podczas określania adresu następnego skoku w trasie można ustawić go na adres IP urządzenia wirtualnego w wirtualnej sieci równorzędnej HubVNet.

[!INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Aby utworzyć połączenie komunikacji równorzędnej między sieciami wirtualnymi o różnych modelach wdrażania, wykonaj poniższe kroki:

1. Tekst poniżej zawiera definicję połączenia wirtualnych sieci równorzędnych (VNET1 z VNET2) na podstawie tego scenariusza. Wymagane jest tylko jedno łącze, aby nawiązać komunikację równorzędną między klasyczną siecią wirtualną i siecią wirtualną usługi Azure Resource Manager.
   
    Pamiętaj, aby wprowadzić identyfikator subskrypcji, w której znajduje się klasyczna sieć wirtualna lub sieć VNET2 oraz zmienić nazwę MyResouceGroup na nazwę odpowiedniej grupy zasobów.
   
    {  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",  "contentVersion": "1.0.0.0",  "parameters": {  },  "variables": {  },  "resources": [
   
        {
        "apiVersion": "2016-06-01",
        "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
        "name": "VNET1/LinkToVNET2",
        "location": "[resourceGroup().location]",
        "properties": {
        "allowVirtualNetworkAccess": true,
        "allowForwardedTraffic": false,
        "allowGatewayTransit": false,
        "useRemoteGateways": false,
            "remoteVirtualNetwork": {
            "id": "[resourceId('Microsoft.ClassicNetwork/virtualNetworks', 'VNET2')]"
    }
   
        }
        }
    ]  }
2. Aby wdrożyć plik szablonu, uruchom następujące polecenie cmdlet w celu utworzenia lub zaktualizowania wdrożenia.
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all
   
        Output shows:
   
        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent
3. Po pomyślnym zakończeniu wdrożenia można uruchomić następujące polecenie cmdlet, aby wyświetlić stan komunikacji równorzędnej:
   
        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2
   
        Output shows:
   
        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Po nawiązaniu komunikacji równorzędnej między klasyczną siecią wirtualną i siecią wirtualną menedżera zasobów powinno być możliwe inicjowanie połączeń z dowolnej maszyny wirtualnej w sieci VNET1 do dowolnej maszyny wirtualnej w sieci VNET2 i odwrotnie.




<!--HONumber=Nov16_HO2-->


