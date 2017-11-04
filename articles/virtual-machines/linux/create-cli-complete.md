---
title: "Utworzenie środowiska systemu Linux 2.0 interfejsu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Tworzenie magazynu, Maszynę wirtualną systemu Linux, sieci wirtualnej i podsieci, usługi równoważenia obciążenia, karty Sieciowej, publicznego adresu IP i sieciową grupę zabezpieczeń, wszystkie od podstaw przy użyciu 2.0 interfejsu wiersza polecenia platformy Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/06/2017
ms.author: iainfou
ms.openlocfilehash: e5c4785428b2150e951923e98079e00808a82d87
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-complete-linux-virtual-machine-with-the-azure-cli"></a>Utwórz pełną maszyny wirtualnej systemu Linux z wiersza polecenia platformy Azure
Aby szybko utworzyć maszynę wirtualną (VM) na platformie Azure, korzystając jednego polecenia interfejsu wiersza polecenia Azure, który można utworzyć wszelkie wymagane zasoby towarzyszące są używane wartości domyślne. Zasoby, takie jak sieć wirtualną, publiczny adres IP i reguły grupy zabezpieczeń sieci są tworzone automatycznie. Aby uzyskać większą kontrolę nad środowiskiem w środowisku produkcyjnym należy używać, może utworzyć te zasoby wcześniejsze i następnie dodać do nich maszyn wirtualnych. W tym artykule prowadzi użytkownika przez proces tworzenia maszyny Wirtualnej i każdy z zasobów pomocniczych jeden po drugim.

Upewnij się, że zainstalowano najnowszą [Azure CLI 2.0](/cli/azure/install-az-cli2) i logowania do konta platformy Azure przy użyciu [logowania az](/cli/azure/#login).

W poniższych przykładach Zastąp przykładowe nazwy parametrów własne wartości. Przykład nazwy parametru zawierają *myResourceGroup*, *myVnet*, i *myVM*.

## <a name="create-resource-group"></a>Tworzenie grupy zasobów
Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. Grupy zasobów musi zostać utworzone przed maszyny wirtualnej i dodatkowe zasoby sieci wirtualnej. Tworzenie grupy zasobów z [Tworzenie grupy az](/cli/azure/group#create). Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroup* w *eastus* lokalizacji:

```azurecli
az group create --name myResourceGroup --location eastus
```

Domyślnie dane wyjściowe polecenia wiersza polecenia platformy Azure jest w formacie JSON (JavaScript Object Notation). Aby zmienić domyślne dane wyjściowe do listy lub tabeli, na przykład użyć [skonfigurować az — dane wyjściowe](/cli/azure/#configure). Można również dodać `--output` do dowolnego polecenia jeden raz Zmień format danych wyjściowych. W poniższym przykładzie przedstawiono dane wyjściowe JSON `az group create` polecenia:

```json                       
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "location": "eastus",
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-a-virtual-network-and-subnet"></a>Tworzenie sieci wirtualnej i podsieci
Dalej przez tworzenie sieci wirtualnej platformy Azure i podsieci do której można utworzyć maszyny wirtualne. Użyj [tworzenie sieci wirtualnej sieci az](/cli/azure/network/vnet#create) można utworzyć sieci wirtualnej o nazwie *myVnet* z *192.168.0.0/16* prefiks adresu. Możesz również dodać podsieć o nazwie *mySubnet* z prefiksem adresu o *192.168.1.0/24*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

Dane wyjściowe zawierają podsieci logicznie utworzone w sieci wirtualnej:

```json
{
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "location": "eastus",
  "name": "myVnet",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "ed62fd03-e9de-430b-84df-8a3b87cacdbb",
  "subnets": [
    {
      "addressPrefix": "192.168.1.0/24",
      "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
      "ipConfigurations": null,
      "name": "mySubnet",
      "networkSecurityGroup": null,
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "resourceNavigationLinks": null,
      "routeTable": null
    }
  ],
  "tags": {},
  "type": "Microsoft.Network/virtualNetworks",
  "virtualNetworkPeerings": null
}
```


## <a name="create-a-public-ip-address"></a>Tworzenie publicznego adresu IP
Teraz Utwórzmy publiczny adres IP z [utworzyć az sieci publicznej ip](/cli/azure/network/public-ip#create). Ten publiczny adres IP umożliwia nawiązywanie połączenia z Internetu do maszyn wirtualnych. Ponieważ domyślnym adresem jest dynamiczny, utworzymy również nazwanego wpisu DNS z `--domain-name-label` opcji. Poniższy przykład tworzy publicznego adresu IP o nazwie *myPublicIP* o nazwie DNS *mypublicdns*. Ponieważ nazwy DNS musi być unikatowa, należy podać własną unikatową nazwę DNS:

```azurecli
az network public-ip create \
    --resource-group myResourceGroup \
    --name myPublicIP \
    --dns-name mypublicdns
```

Dane wyjściowe:

```json
{
  "publicIp": {
    "dnsSettings": {
      "domainNameLabel": "mypublicdns",
      "fqdn": "mypublicdns.eastus.cloudapp.azure.com",
      "reverseFqdn": null
    },
    "etag": "W/\"2632aa72-3d2d-4529-b38e-b622b4202925\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": null,
    "ipConfiguration": null,
    "location": "eastus",
    "name": "myPublicIP",
    "provisioningState": "Succeeded",
    "publicIpAddressVersion": "IPv4",
    "publicIpAllocationMethod": "Dynamic",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "4c65de38-71f5-4684-be10-75e605b3e41f",
    "tags": null,
    "type": "Microsoft.Network/publicIPAddresses"
  }
}
```


## <a name="create-a-network-security-group"></a>Utwórz grupę zabezpieczeń sieci
Sterowanie przepływem ruchu do i z maszyn wirtualnych, należy utworzyć grupę zabezpieczeń sieci. Grupa zabezpieczeń sieci może odnosić się do karty Sieciowej lub podsieci. W poniższym przykładzie użyto [utworzyć nsg sieci az](/cli/azure/network/nsg#create) utworzyć sieciowej grupy zabezpieczeń o nazwie *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

Można zdefiniować reguły, które akceptować lub odrzucać określonego ruchu. Aby zezwolić na połączenia przychodzące do portu 22 (do obsługi protokołu SSH), należy utworzyć regułę ruchu przychodzącego dla sieciowej grupy zabezpieczeń z [Tworzenie reguły nsg sieci az](/cli/azure/network/nsg/rule#create). Poniższy przykład tworzy reguły o nazwie *myNetworkSecurityGroupRuleSSH*:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 22 \
    --access allow
```

Aby zezwolić na połączenia przychodzące na porcie 80 (dla obsługi ruchu w sieci web), Dodaj inną regułę grupy zabezpieczeń sieci. Poniższy przykład tworzy reguły o nazwie *myNetworkSecurityGroupRuleHTTP*:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleWeb \
    --protocol tcp \
    --priority 1001 \
    --destination-port-range 80 \
    --access allow
```

Sprawdź, czy grupy zabezpieczeń sieci i reguły z [Pokaż nsg sieci az](/cli/azure/network/nsg#show):

```azurecli
az network nsg show --resource-group myResourceGroup --name myNetworkSecurityGroup
```

Dane wyjściowe:

```json
{
  "defaultSecurityRules": [
    {
      "access": "Allow",
      "description": "Allow inbound traffic from all VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetInBound",
      "name": "AllowVnetInBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow inbound traffic from azure load balancer",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowAzureLoadBalancerInBou
      "name": "AllowAzureLoadBalancerInBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "AzureLoadBalancer",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all inbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllInBound",
      "name": "DenyAllInBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs to all VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetOutBound",
      "name": "AllowVnetOutBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs to Internet",
      "destinationAddressPrefix": "Internet",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowInternetOutBound",
      "name": "AllowInternetOutBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all outbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllOutBound",
      "name": "DenyAllOutBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
  "location": "eastus",
  "name": "myNetworkSecurityGroup",
  "networkInterfaces": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "47a9964e-23a3-438a-a726-8d60ebbb1c3c",
  "securityRules": [
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "22",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleSSH",
      "name": "myNetworkSecurityGroupRuleSSH",
      "priority": 1000,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "80",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleWeb",
      "name": "myNetworkSecurityGroupRuleWeb",
      "priority": 1001,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "subnets": null,
  "tags": null,
  "type": "Microsoft.Network/networkSecurityGroups"
}
```

## <a name="create-a-virtual-nic"></a>Tworzenie wirtualnej karty Sieciowej
Wirtualne karty sieciowe (NIC) są programowo dostępne, ponieważ reguły można stosować do ich używania. Może także zawierać więcej niż jeden. W następujących [tworzenie kart interfejsu sieciowego az](/cli/azure/network/nic#create) polecenia Utwórz karty Sieciowej o nazwie *myNic* i skojarzyć go z grupy zabezpieczeń sieci. Publiczny adres IP *myPublicIP* jest także powiązany z wirtualnej karty sieciowej.

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --public-ip-address myPublicIP \
    --network-security-group myNetworkSecurityGroup
```

Dane wyjściowe:

```json
{
  "NewNIC": {
    "dnsSettings": {
      "appliedDnsServers": [],
      "dnsServers": [],
      "internalDnsNameLabel": null,
      "internalDomainNameSuffix": "brqlt10lvoxedgkeuomc4pm5tb.bx.internal.cloudapp.net",
      "internalFqdn": null
    },
    "enableAcceleratedNetworking": false,
    "enableIpForwarding": false,
    "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
    "ipConfigurations": [
      {
        "applicationGatewayBackendAddressPools": null,
        "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic/ipConfigurations/ipconfig1",
        "loadBalancerBackendAddressPools": null,
        "loadBalancerInboundNatRules": null,
        "name": "ipconfig1",
        "primary": true,
        "privateIpAddress": "192.168.1.4",
        "privateIpAddressVersion": "IPv4",
        "privateIpAllocationMethod": "Dynamic",
        "provisioningState": "Succeeded",
        "publicIpAddress": {
          "dnsSettings": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
          "idleTimeoutInMinutes": null,
          "ipAddress": null,
          "ipConfiguration": null,
          "location": null,
          "name": null,
          "provisioningState": null,
          "publicIpAddressVersion": null,
          "publicIpAllocationMethod": null,
          "resourceGroup": "myResourceGroup",
          "resourceGuid": null,
          "tags": null,
          "type": null
        },
        "resourceGroup": "myResourceGroup",
        "subnet": {
          "addressPrefix": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
          "ipConfigurations": null,
          "name": null,
          "networkSecurityGroup": null,
          "provisioningState": null,
          "resourceGroup": "myResourceGroup",
          "resourceNavigationLinks": null,
          "routeTable": null
        }
      }
    ],
    "location": "eastus",
    "macAddress": null,
    "name": "myNic",
    "networkSecurityGroup": {
      "defaultSecurityRules": null,
      "etag": null,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
      "location": null,
      "name": null,
      "networkInterfaces": null,
      "provisioningState": null,
      "resourceGroup": "myResourceGroup",
      "resourceGuid": null,
      "securityRules": null,
      "subnets": null,
      "tags": null,
      "type": null
    },
    "primary": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "b3dbaa0e-2cf2-43be-a814-5cc49fea3304",
    "tags": null,
    "type": "Microsoft.Network/networkInterfaces",
    "virtualMachine": null
  }
}
```


## <a name="create-an-availability-set"></a>Tworzenie zestawu dostępności
Zestawy dostępności rozpowszechniania pomocy maszyn wirtualnych w domenach awarii i Aktualizacja domeny. Mimo że można utworzyć tylko jedną maszynę Wirtualną od razu, jest najlepszym rozwiązaniem jest użycie zestawów dostępności, aby ułatwić w przyszłości. 

Domen błędów Definiowanie grup maszyn wirtualnych, które współużytkują wspólne przełącznik źródła i sieci zasilania. Domyślnie maszyny wirtualne, które są skonfigurowane w zestawie dostępności są rozdzielone przez maksymalnie trzy domen błędów. Problemem sprzętowym w jednym z tych domen błędów nie ma wpływu na każdej maszynie Wirtualnej, która jest uruchomiona aplikacja.

Aktualizacja domen wskazują grupy maszyn wirtualnych i podstawowym sprzętem fizycznym, który może zostać uruchomiony ponownie w tym samym czasie. Podczas zaplanowanej konserwacji kolejności w aktualizacji, które są ponownie uruchamiane domen mogą nie być sekwencyjnych, ale tylko jedna aktualizacja domeny ponownego uruchomienia w czasie.

Azure automatycznie dystrybuuje maszyn wirtualnych w domenach awarii i aktualizacji podczas umieszczania ich w zestawie dostępności. Aby uzyskać więcej informacji, zobacz [Zarządzanie dostępność maszyn wirtualnych](manage-availability.md).

Utwórz zbiór dostępności dla maszyny Wirtualnej z [tworzenia maszyny wirtualnej az zestawu dostępności](/cli/azure/vm/availability-set#create). Poniższy przykład tworzy zbiór nazwanego dostępności *myAvailabilitySet*:

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet
```

Domen błędów wyjścia uwagi i domen aktualizacji:

```json
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet",
  "location": "eastus",
  "managed": null,
  "name": "myAvailabilitySet",
  "platformFaultDomainCount": 2,
  "platformUpdateDomainCount": 5,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": null,
    "managed": true,
    "tier": null
  },
  "statuses": null,
  "tags": {},
  "type": "Microsoft.Compute/availabilitySets",
  "virtualMachines": []
}
```


## <a name="create-the-linux-vms"></a>Tworzenie maszyn wirtualnych systemu Linux
Po utworzeniu zasobów sieciowych do obsługi maszyn wirtualnych dostęp do Internetu. Teraz Utwórz maszynę Wirtualną i zabezpiecz ją przy użyciu klucza SSH. W takim przypadku zamierzamy utworzyć Ubuntu oparte na najnowszych LTS maszyny Wirtualnej. Można znaleźć dodatkowe obrazy z [listy obrazów maszyny wirtualnej az](/cli/azure/vm/image#list), zgodnie z opisem w [znajdowanie obrazów maszyn wirtualnych Azure](cli-ps-findimage.md).

Możemy również określić klucza SSH do uwierzytelniania. Jeśli nie masz pary kluczy publicznych SSH, możesz [je utworzyć](mac-create-ssh-keys.md) lub użyj `--generate-ssh-keys` parametr, aby je utworzyć automatycznie. Jeśli możesz już parę kluczy, ten parametr wykorzystuje istniejące klucze w `~/.ssh`.

Tworzenie maszyny Wirtualnej, przełączając naszych zasobów i informacji razem z [tworzenia maszyny wirtualnej az](/cli/azure/vm#create) polecenia. Poniższy przykład tworzy Maszynę wirtualną o nazwie *myVM*:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --availability-set myAvailabilitySet \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH do maszyny Wirtualnej z wpisu DNS podany podczas tworzenia publicznego adresu IP. To `fqdn` jest wyświetlany w danych wyjściowych podczas tworzenia maszyny Wirtualnej:

```json
{
  "fqdns": "mypublicdns.eastus.cloudapp.azure.com",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-13-71-C8",
  "powerState": "VM running",
  "privateIpAddress": "192.168.1.5",
  "publicIpAddress": "13.90.94.252",
  "resourceGroup": "myResourceGroup"
}
```

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

Dane wyjściowe:

```bash
The authenticity of host 'mypublicdns.eastus.cloudapp.azure.com (13.90.94.252)' can't be established.
ECDSA key fingerprint is SHA256:SylINP80Um6XRTvWiFaNz+H+1jcrKB1IiNgCDDJRj6A.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'mypublicdns.eastus.cloudapp.azure.com,13.90.94.252' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.4.0-81-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@myVM:~$
```

Można zainstalować NGINX i widoczne ruchu przepływu do maszyny Wirtualnej. Zainstaluj NGINX w następujący sposób:

```bash
sudo apt-get install -y nginx
```

Aby wyświetlić NGINX domyślnej witryny w akcji, otwórz przeglądarkę sieci web i wprowadzić swoją nazwę FQDN:

![Domyślna witryna NGINX na maszynie Wirtualnej](media/create-cli-complete/nginx.png)

## <a name="export-as-a-template"></a>Eksportowanie jako szablon
Co zrobić, jeśli chcesz teraz Utwórz środowisko rozwoju dodatkowych z takimi samymi parametrami lub w środowisku produkcyjnym, które odpowiadają go? Menedżer zasobów używa szablony JSON, które definiują wszystkie parametry dla danego środowiska. Limit całego środowiska kompilacji, umieszczając odwołanie do tego szablonu JSON. Możesz [ręcznie utworzyć szablony JSON](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lub wyeksportować utworzenie szablonu JSON do istniejącego środowiska. Użyj [eksportowanie grupy az](/cli/azure/group#export) Aby wyeksportować grupy zasobów w następujący sposób:

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

To polecenie tworzy `myResourceGroup.json` plik w bieżącym katalogu roboczym. Podczas tworzenia środowiska z tego szablonu, wyświetlany jest monit dla nazw zasobów. Można go wypełnić te nazwy w pliku szablonu, dodając `--include-parameter-default-value` parametr `az group export` polecenia. Edytuj szablon JSON można określić nazwy zasobu lub [Tworzenie pliku parameters.JSON następującym kodem](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) , który określa nazwy zasobu.

Aby utworzyć środowisko z szablonu, należy użyć [Utwórz wdrożenie grupy az](/cli/azure/group/deployment#create) w następujący sposób:

```azurecli
az group deployment create \
    --resource-group myNewResourceGroup \
    --template-file myResourceGroup.json
```

Warto przeczytać [więcej informacji na temat sposobu wdrażania z szablonów](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Więcej informacji na temat sposobu przyrostowo aktualizacja środowisk, użyj pliku parametrów i dostępu do szablonów z lokalizacji magazynu jednego.

## <a name="next-steps"></a>Następne kroki
Teraz możesz przystąpić do rozpoczęcia pracy z wieloma składnikami sieciowymi i maszyny wirtualne. To środowisko próbki służy do tworzenia aplikacji przy użyciu podstawowe składniki wprowadzone w tym miejscu.
