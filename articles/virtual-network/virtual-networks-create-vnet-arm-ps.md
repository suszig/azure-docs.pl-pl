---
title: Tworzenie sieci wirtualnej - programu Azure PowerShell | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak utworzyć sieć wirtualną przy użyciu programu PowerShell."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a31f4f12-54ee-4339-b968-1a8097ca77d3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e7072ddf51570d46578111e2e392e3cbea53f2aa
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="create-a-virtual-network-using-powershell"></a>Tworzenie sieci wirtualnej przy użyciu programu PowerShell

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Platforma Azure ma dwa modele wdrażania: usługa Azure Resource Manager i wersja klasyczna. Firma Microsoft zaleca tworzenie zasobów za pomocą modelu wdrożenia usługi Resource Manager. Aby dowiedzieć się więcej o różnicach między dwoma modelami, zapoznaj się z artykułem [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) (Informacje na temat modeli wdrażania platformy Azure).
 
W tym artykule wyjaśniono, jak utworzyć sieć wirtualną przy użyciu modelu wdrażania usługi Resource Manager przy użyciu programu PowerShell. Sieć wirtualną można również utworzyć w usłudze Resource Manager przy użyciu innych narzędzi albo za pośrednictwem klasycznego modelu wdrożenia, wybierając inną opcję z poniższej listy:

> [!div class="op_single_selector"]
> * [Portal](virtual-networks-create-vnet-arm-pportal.md)
> * [Program PowerShell](virtual-networks-create-vnet-arm-ps.md)
> * [Interfejs wiersza polecenia](virtual-networks-create-vnet-arm-cli.md)
> * [Szablon](virtual-networks-create-vnet-arm-template-click.md)
> * [Portal (klasyczny)](virtual-networks-create-vnet-classic-pportal.md)
> * [PowerShell (klasyczny)](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [Interfejs wiersza polecenia (klasyczny)](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Tworzenie sieci wirtualnej

Aby utworzyć sieć wirtualną przy użyciu programu PowerShell, wykonaj następujące kroki:

1. Instalowanie i konfigurowanie programu Azure PowerShell, wykonując kroki opisane w [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview) artykułu.

2. W razie potrzeby utwórz nową grupę zasobów, jak pokazano poniżej. W tym scenariuszu, należy utworzyć grupę zasobów o nazwie *TestRG*. Aby uzyskać więcej informacji na temat grup zasobów, zobacz temat [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

    ```powershell   
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    Oczekiwane dane wyjściowe:

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/[Subscription Id]/resourceGroups/TestRG    
3. Tworzenie nowej sieci wirtualnej o nazwie *TestVNet*:

    ```powershell
    New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
    -AddressPrefix 192.168.0.0/16 -Location centralus
    ```

    Oczekiwane dane wyjściowe:

        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                   : W/"[Id]"
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
4. Obiekt sieci wirtualnej można przechowywać w zmiennej:

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

   > [!TIP]
   > Można połączyć kroki 3 i 4, uruchamiając `$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus`.
   > 

5. Dodaj podsieć do nowej zmiennej sieci wirtualnej:

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.1.0/24
    ```

    Oczekiwane dane wyjściowe:
   
        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                  : W/"[Id]"
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

6. Powtórz krok 5 opisany powyżej dla każdej podsieci, którą chcesz utworzyć. Poniższe polecenie tworzy *zaplecza* podsieci dla tego scenariusza:

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.2.0/24
    ```

7. Chociaż w toku tego procesu są tworzone podsieci, to na tym etapie istnieją one tylko w zmiennej lokalnej używanej do pobierania sieci wirtualnej, która została utworzona w kroku 4. Aby zapisać zmiany na platformie Azure, uruchom następujące polecenie:

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```
   
    Oczekiwane dane wyjściowe:
   
        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                  : W/"[Id]"
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
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  },
                                  {
                                    "Name": "BackEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                    "AddressPrefix": "192.168.2.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  }
                                ]
        VirtualNetworkPeerings : []

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak połączyć:

- Maszyna wirtualna (VM) do sieci wirtualnej, odczytując [utworzyć Maszynę wirtualną systemu Windows](../virtual-machines/virtual-machines-windows-ps-create.md) artykułu. Zamiast tworzyć sieć wirtualną i podsieć, wykonując kroki opisane w artykułach, można wybrać istniejącą sieć wirtualną i podsieć, z którymi zostanie połączona maszyna wirtualna.
- Sieć wirtualną z innymi sieciami wirtualnymi. Odpowiednie informacje możesz znaleźć w artykule [Łączenie sieci wirtualnych](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).
- Sieć wirtualną z siecią lokalną za pomocą prywatnej sieci wirtualnej (VPN) typu lokacja-lokacja lub obwodu usługi ExpressRoute. Odpowiednie informacje znajdziesz w artykułach [Connect a VNet to an on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) (Łączenie sieci wirtualnej z siecią lokalną za pomocą sieci VPN typu lokacja-lokacja) i [Link a VNet to an ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-arm.md) (Łączenie sieci wirtualnej z obwodem usługi ExpressRoute).
