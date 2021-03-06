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
ms.openlocfilehash: e31eb02fda7ade027225c428c5b15804ebc6f182
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-linux-virtual-machine-in-an-availability-zone-with-the-azure-cli"></a>Utwórz maszynę wirtualną systemu Linux w strefie dostępności z wiersza polecenia platformy Azure

Ta procedura artykułu przy użyciu wiersza polecenia platformy Azure, aby utworzyć Maszynę wirtualną systemu Linux w strefie dostępności Azure. [Strefa dostępności](../../availability-zones/az-overview.md) to fizycznie oddzielona strefa w regionie świadczenia usługi Azure. Strefy dostępności chronią aplikacje i dane, zmniejszając prawdopodobieństwo wystąpienia awarii lub utraty całego centrum danych.

[!INCLUDE [availability-zones-preview-statement.md](../../../includes/availability-zones-preview-statement.md)]

Upewnij się, że zainstalowano najnowszą [Azure CLI 2.0](/cli/azure/install-az-cli2) i zalogowany do konta platformy Azure z [logowania az](/cli/azure/reference-index#az_login).


## <a name="check-vm-sku-availability"></a>Sprawdzanie dostępności jednostki SKU maszyny wirtualnej
Dostępność rozmiarów maszyn wirtualnych lub jednostek SKU może różnić się w zależności od regionu i strefy. Aby uzyskać pomoc w planowaniu użycia stref dostępności, możesz wyświetlić listę dostępnych jednostek SKU maszyn wirtualnych według regionu i strefy świadczenia usługi Azure. Ta możliwość gwarantuje wybranie odpowiedniego rozmiaru maszyny wirtualnej oraz uzyskanie żądanej odporności w różnych strefach. Aby uzyskać więcej informacji o różnych typach i rozmiarach maszyn wirtualnych, zobacz [Omówienie rozmiarów maszyn wirtualnych](sizes.md).

Możesz wyświetlić dostępne jednostki SKU maszyny Wirtualnej z [wirtualna az listy SKU](/cli/azure/vm#az_vm_list_skus) polecenia. Następujący przykład zawiera listę dostępnych jednostek SKU maszyn wirtualnych w regionie *eastus2*:

```azurecli
az vm list-skus --location eastus2 --output table
```

Rezultat jest podobny do poniższego, skróconego przykładu, który pokazuje strefy dostępności, w których dostępne są poszczególne rozmiary maszyn wirtualnych:

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

Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. Grupę zasobów należy utworzyć przed maszyną wirtualną. W tym przykładzie grupy zasobów o nazwie *myResourceGroupVM* jest tworzony w *eastus2* regionu. East US 2 (Wschodnie stany USA 2) jest jednym z regionów świadczenia usługi Azure, które obsługują strefy dostępności w wersji zapoznawczej.

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus2
```

Grupa zasobów jest określony, podczas tworzenia lub modyfikowania maszyn wirtualnych, które są widoczne w tym artykule.

## <a name="create-virtual-machine"></a>Tworzenie maszyny wirtualnej

Utwórz maszynę wirtualną za pomocą polecenia [az vm create](/cli/azure/vm#az_vm_create). 

Podczas tworzenia maszyny wirtualnej masz dostęp do kilku opcji, takich jak obraz systemu operacyjnego, ustalanie rozmiaru dysku i poświadczenia administracyjne. W tym przykładzie tworzona jest maszyna wirtualna o nazwie *myVM* z systemem Ubuntu Server. Maszyna wirtualna jest tworzony w strefie dostępności *1*. Domyślnie maszyny Wirtualnej jest tworzona w *Standard_DS1_v2* rozmiar. Ten rozmiar jest obsługiwany w wersji zapoznawczej stref dostępności.

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys --zone 1
```

Tworzenie maszyny wirtualnej może potrwać kilka minut. Po utworzeniu maszyny wirtualnej interfejs wiersza polecenia platformy Azure wyświetla informacje o maszynie wirtualnej. Zwróć uwagę na `zones` wartość, która zawiera strefy dostępności, w którym jest uruchomiona maszyna wirtualna. 

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

Podobnie Sprawdź, czy dysków zarządzanych w maszyny Wirtualnej jest w strefie dostępności. Za pomocą polecenia [az vm show](/cli/azure/vm#az_vm_show) wyświetl identyfikator dysku. W tym przykładzie identyfikator dysku są przechowywane w zmiennej używanej w kolejnym kroku. 

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




