---
title: "Utwórz Maszynę wirtualną systemu Linux zoned z wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Utwórz Maszynę wirtualną systemu Linux w strefie dostępności z wiersza polecenia platformy Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: danlep
ms.custom: 
ms.openlocfilehash: 5e742187295d0bd6dbc0767ee164335fc0cf9f02
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/05/2018
---
# <a name="create-a-linux-virtual-machine-in-an-availability-zone-with-the-azure-cli"></a>Utwórz maszynę wirtualną systemu Linux w strefie dostępności z wiersza polecenia platformy Azure

Ta procedura artykułu przy użyciu wiersza polecenia platformy Azure, aby utworzyć Maszynę wirtualną systemu Linux w strefie dostępności Azure. [Strefa dostępności](../../availability-zones/az-overview.md) to fizycznie oddzielona strefa w regionie świadczenia usługi Azure. Strefy dostępności chronią aplikacje i dane, zmniejszając prawdopodobieństwo wystąpienia awarii lub utraty całego centrum danych.

[!INCLUDE [availability-zones-preview-statement.md](../../../includes/availability-zones-preview-statement.md)]

Upewnij się, że zainstalowano najnowszą [Azure CLI 2.0](/cli/azure/install-az-cli2) i zalogowany do konta platformy Azure z [logowania az](/cli/azure/#login).


## <a name="check-vm-sku-availability"></a>Sprawdź dostępność SKU maszyny Wirtualnej
Dostępność rozmiarów maszyn wirtualnych lub jednostki SKU, może się różnić od regionu i strefy. Aby zaplanować użycie stref dostępności, można wyświetlić dostępne jednostki SKU maszyny Wirtualnej w regionie platformy Azure i strefy. Ta możliwość upewnia się, wybierz odpowiedni rozmiar maszyny Wirtualnej i uzyskać odpowiednią odporności różnych strefach. Aby uzyskać więcej informacji o różnych typach maszyn wirtualnych i rozmiary, zobacz [rozmiarów maszyn wirtualnych — omówienie](sizes.md).

Możesz wyświetlić dostępne jednostki SKU maszyny Wirtualnej z [wirtualna az listy SKU](/cli/azure/vm#az_vm_list_skus) polecenia. Poniższy przykład zawiera listę dostępnych SKU maszyny Wirtualnej w *eastus2* regionu:

```azurecli
az vm list-skus --location eastus2 --output table
```

Wynik jest podobny do poniższego przykładu skrócone pokazuje stref dostępności, w których każdy rozmiar maszyny Wirtualnej są dostępne:

```azurecli
ResourceType      Locations  Name               Tier       Size     Zones
----------------  ---------  -----------------  ---------  -------  -------
virtualMachines   eastus2    Standard_DS1_v2    Standard   DS1_v2   1,2,3
virtualMachines   eastus2    Standard_DS2_v2    Standard   DS2_v2   1,2,3
[...]
virtualMachines   eastus2    Standard_F1s       Standard   F1s      1,2,3
virtualMachines   eastus2    Standard_F2s       Standard   F2s      1,2,3
[...]
virtualMachines   eastus2    Standard_D2s_v3    Standard   D2_v3    1,2,3
virtualMachines   eastus2    Standard_D4s_v3    Standard   D4_v3    1,2,3
[...]
virtualMachines   eastus2    Standard_E2_v3     Standard   E2_v3    1,2,3
virtualMachines   eastus2    Standard_E4_v3     Standard   E4_v3    1,2,3
```


## <a name="create-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#az_group_create).  

Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. Grupy zasobów musi zostać utworzone przed maszyny wirtualnej. W tym przykładzie grupy zasobów o nazwie *myResourceGroupVM* jest tworzony w *eastus2* regionu. East US 2 (Wschodnie stany USA 2) jest jednym z regionów świadczenia usługi Azure, które obsługują strefy dostępności w wersji zapoznawczej.

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus2
```

Grupa zasobów jest określony, podczas tworzenia lub modyfikowania maszyn wirtualnych, które są widoczne w tym artykule.

## <a name="create-virtual-machine"></a>Tworzenie maszyny wirtualnej

Utwórz maszynę wirtualną z [tworzenia maszyny wirtualnej az](/cli/azure/vm#az_vm_create) polecenia. 

Podczas tworzenia maszyny wirtualnej, takich jak obraz systemu operacyjnego, dysku zmiany rozmiaru i administracyjnych poświadczeń jest kilka opcji. W tym przykładzie utworzono maszynę wirtualną o nazwie *myVM* systemem Ubuntu Server. Maszyna wirtualna jest tworzony w strefie dostępności *1*. Domyślnie maszyny Wirtualnej jest tworzona w *Standard_DS1_v2* rozmiar. Ten rozmiar jest obsługiwany w wersji zapoznawczej stref dostępności.

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys --zone 1
```

Może upłynąć kilka minut, aby utworzyć maszynę Wirtualną. Po utworzeniu maszyny Wirtualnej, interfejsu wiersza polecenia Azure generuje informacje o maszynie Wirtualnej. Zwróć uwagę na `zones` wartość, która zawiera strefy dostępności, w którym jest uruchomiona maszyna wirtualna. 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus2",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM",
  "zones": "1"
}
```

## <a name="zone-for-ip-address-and-managed-disk"></a>Strefa adresu IP i dysku zarządzanego

Po wdrożeniu maszyny Wirtualnej w strefie dostępności adresu IP i dysków zarządzanych zasobów są wdrażane w tej samej strefie dostępności. Poniższe przykłady uzyskać informacje na temat tych zasobów.

Należy najpierw użyć [az vm--adresy ip](/cli/azure/vm#az_vm_list_ip_addresses) polecenia, aby zwrócić nazwę publicznego zasobu adresu IP w *myVM*. W tym przykładzie nazwa jest przechowywana w zmiennej używanej w kolejnym kroku.

```azurecli-interactive
ipaddressname=$(az vm list-ip-addresses -g myResourceGroupVM -n myVM --query "[].virtualMachine.network.publicIpAddresses[].name" -o tsv)
```

Teraz można uzyskać informacji o adresie IP:

```azurecli-interactive
az network public-ip show --resource-group myResourceGroupVM --name $ipaddressname
```

Dane wyjściowe zawierają, czy adres IP jest w tej samej strefie dostępności jako maszyny Wirtualnej:

```azurecli-interactive
{
  "dnsSettings": null,
  "etag": "W/\"b7ad25eb-3191-4c8f-9cec-c5e4a3a37d35\"",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/publicIPAddresses/myVMPublicIP",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "52.174.34.95",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/networkInterfaces/myVMVMNic/ipConfigurations/ipconfigmyVM",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "myResourceGroupVM",
    "subnet": null
  },
  "location": "eastUS2",
  "name": "myVMPublicIP",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "myResourceGroupVM",
  "resourceGuid": "8c70a073-09be-4504-0000-000000000000",
  "tags": {},
  "type": "Microsoft.Network/publicIPAddresses",
  "zones": [
    "1"
  ]
}
```

Podobnie Sprawdź, czy dysków zarządzanych w maszyny Wirtualnej jest w strefie dostępności. Użyj [az maszyny wirtualnej pokazu](/cli/azure/vm#az_vm_show) poleceniu, aby uzyskać identyfikator dysku. W tym przykładzie identyfikator dysku są przechowywane w zmiennej używanej w kolejnym kroku. 

```azurecli-interactive
osdiskname=$(az vm show -g myResourceGroupVM -n myVM --query "storageProfile.osDisk.name" -o tsv)
```
Teraz można uzyskać informacji o dysku zarządzanego:

```azurecli-interactive
az disk show --resource-group myResourceGroupVM --name $osdiskname
```

Dane wyjściowe pokazują, że dysk zarządzany znajduje się w tej samej strefie dostępności co maszyna wirtualna:

```azurecli-interactive
{
  "creationData": {
    "createOption": "FromImage",
    "imageReference": {
      "id": "/Subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/Providers/Microsoft.Compute/Locations/westeurope/Publishers/Canonical/ArtifactTypes/VMImage/Offers/UbuntuServer/Skus/16.04-LTS/Versions/latest",
      "lun": null
    },
    "sourceResourceId": null,
    "sourceUri": null,
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/disks/osdisk_761c570dab",
  "location": "eastus2",
  "managedBy": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "name": "osdisk_761c570dab",
  "osType": "Linux",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroupVM",
  "sku": {
    "name": "Premium_LRS",
    "tier": "Premium"
  },
  "tags": {},
  "timeCreated": "2017-09-05T22:16:06.892752+00:00",
  "type": "Microsoft.Compute/disks",
  "zones": [
    "1"
  ]
}
```
 


## <a name="next-steps"></a>Kolejne kroki

W tym artykule przedstawiono sposób tworzenia maszyny wirtualnej w strefie dostępności. Dowiedz się więcej o [regionach i dostępności](regions-and-availability.md) maszyn wirtualnych platformy Azure.




