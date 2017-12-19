---
title: Tworzenie sieci wirtualnej - wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak utworzyć sieć wirtualną przy użyciu wiersza polecenia platformy Azure."
services: virtual-network
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 75966bcc-0056-4667-8482-6f08ca38e77a
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 659a791124eab002290ac0b7f0898cf1c06c2951
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Utwórz sieć wirtualną przy użyciu wiersza polecenia platformy Azure

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Platforma Azure ma dwa modele wdrażania: usługa Azure Resource Manager i wersja klasyczna. Firma Microsoft zaleca tworzenie zasobów za pomocą modelu wdrożenia usługi Resource Manager. Aby dowiedzieć się więcej o różnicach między dwoma modelami, zapoznaj się z artykułem [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) (Informacje na temat modeli wdrażania platformy Azure).

Można również utworzyć sieć wirtualną za pomocą Menedżera zasobów przy użyciu innych narzędzi lub Utwórz sieć wirtualną przy użyciu klasycznego modelu wdrażania, wybierając inną opcję z poniższej listy:

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

Aby utworzyć sieć wirtualną przy użyciu wiersza polecenia platformy Azure, wykonaj następujące kroki:

1. Instalowanie i Konfigurowanie r [Azure CLI 2.0](/cli/azure/install-az-cli2) i zaloguj się do platformy Azure konta przy użyciu [logowania az](/cli/azure/#login).

2. Utwórz grupę zasobów do sieci wirtualnej przy użyciu [Tworzenie grupy az](/cli/azure/group#create) z `--name` i `--location` argumentów:

    ```azurecli
    az group create --name TestRG --location centralus
    ```

3. Utwórz sieć wirtualną i podsieć:

    ```azurecli
    az network vnet create \
    --name TestVNet \
    --resource-group TestRG \
    --location centralus \
    --address-prefix 192.168.0.0/16 \
    --subnet-name FrontEnd \
    --subnet-prefix 192.168.1.0/24
    ```

    Oczekiwane dane wyjściowe:
    
    ```json
    {
        "newVNet": {
            "addressSpace": {
            "addressPrefixes": [
            "192.168.0.0/16"
            ]
            },
            "dhcpOptions": {
            "dnsServers": []
            },
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>",
            "subnets": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                "name": "FrontEnd",
                "properties": {
                "addressPrefix": "192.168.1.0/24",
                "provisioningState": "Succeeded"
                },
                "resourceGroup": "TestRG"
            }
            ]
            }
    }
    ```

    Użyte parametry:

    - `--name TestVNet`: Nazwa sieci wirtualnej, która ma zostać utworzony.
    - `--resource-group TestRG`: # Nazwa grupy zasobów, która kontroluje zasobu. 
    - `--location centralus`: Lokalizacja, w której chcesz wdrożyć.
    - `--address-prefix 192.168.0.0/16`: Prefiks adresu i z tego bloku.  
    - `--subnet-name FrontEnd`: Nazwa podsieci.
    - `--subnet-prefix 192.168.1.0/24`: Prefiks adresu i z tego bloku.

    Aby wyświetlić podstawowe informacje do użycia w następnego polecenia, można badać przy użyciu sieci wirtualnej [Filtr kwerendy](/cli/azure/query-az-cli2):

    ```azurecli
    az network vnet list --query '[?name==`TestVNet`].{Where:location,Name:name,Group:resourceGroup}' -o table
    ```

    Które tworzy następujące dane wyjściowe:

        Where      Name      Group

        centralus  TestVNet  TestRG

4. Utwórz podsieć:

    ```azurecli
    az network vnet subnet create \
    --address-prefix 192.168.2.0/24 \
    --name BackEnd \
    --resource-group TestRG \
    --vnet-name TestVNet
    ```

    Oczekiwane dane wyjściowe:

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid> \"",
    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "TestRG",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```

    Użyte parametry:

    - `--address-prefix 192.168.2.0/24`: Blok CIDR podsieci.
    - `--name BackEnd`: Nazwa nowej podsieci.
    - `--resource-group TestRG`: Grupy zasobów.
    - `--vnet-name TestVNet`: Nazwa właściciela sieci wirtualnej.

5. Kwerendy właściwości nowej sieci wirtualnej:

    ```azurecli
    az network vnet show \
    -g TestRG \
    -n TestVNet \
    --query '{Name:name,Where:location,Group:resourceGroup,Status:provisioningState,SubnetCount:subnets | length(@)}' \
    -o table
    ```

    Oczekiwane dane wyjściowe:

        Name      Where      Group    Status       SubnetCount

        TestVNet  centralus  TestRG   Succeeded              2

6. Kwerendy właściwości podsieci:

    ```azurecli
    az network vnet subnet list \
    -g TestRG \
    --vnet-name testvnet \
    --query '[].{Name:name,CIDR:addressPrefix,Status:provisioningState}' \
    -o table
    ```

    Oczekiwane dane wyjściowe:

        Name      CIDR            Status

        FrontEnd  192.168.1.0/24  Succeeded
        BackEnd   192.168.2.0/24  Succeeded

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak połączyć:

- Maszyna wirtualna (VM) do sieci wirtualnej, odczytując [Utwórz Maszynę wirtualną systemu Linux](../virtual-machines/linux/quick-create-cli.md) artykułu. Zamiast tworzyć sieć wirtualną i podsieć, wykonując kroki opisane w artykułach, można wybrać istniejącą sieć wirtualną i podsieć, z którymi zostanie połączona maszyna wirtualna.
- Sieć wirtualną z innymi sieciami wirtualnymi. Odpowiednie informacje możesz znaleźć w artykule [Łączenie sieci wirtualnych](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
- Sieć wirtualną z siecią lokalną za pomocą prywatnej sieci wirtualnej (VPN) typu lokacja-lokacja lub obwodu usługi ExpressRoute. Dowiedz się, jak odczytując [połączyć sieć wirtualną z siecią lokalną przy użyciu sieci VPN lokacja lokacja](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) i [połączyć sieć wirtualną z obwodem usługi ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).