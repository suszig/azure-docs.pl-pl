---
title: Zmodyfikuj zestaw skali maszyny wirtualnej platformy Azure | Dokumentacja firmy Microsoft
description: Zmodyfikuj zestaw skali maszyny wirtualnej platformy Azure
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: negat
ms.openlocfilehash: fcca912a8120a51d2f0a454ef0a6341cd5882015
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="modify-a-virtual-machine-scale-set"></a>Zmodyfikuj zestaw skali maszyny wirtualnej
W tym artykule opisano sposób modyfikowania istniejącego zestawu skalowania maszyny wirtualnej. Zadania obejmują jak zmienić konfigurację skali ustawić zmienić konfigurację aplikacji uruchomionych na skali zestawu, jak zarządzać dostępności i inne.

## <a name="fundamental-concepts"></a>podstawowe pojęcia

### <a name="scale-set-model"></a>Modelu zestawu skali

Zestaw skali ma modelu, który przechwytuje *żądany* ustawiony stan skali jako całość. Aby odpytać modelu dla zestawu skalowania, można użyć:

* INTERFEJS API REST: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` 
   
  Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/get).

* Program PowerShell:

  `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}`
   
  Aby uzyskać więcej informacji, zobacz [dokumentacji programu PowerShell](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss).

* Azure CLI: 

  `az vmss show -g {resourceGroupName} -n {vmSaleSetName}` 
   
  Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show).

Można również użyć [Eksploratora zasobów Azure (wersja zapoznawcza)](https://resources.azure.com) lub [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) do badania modeli dla zestawu skalowania.

Dokładny opis dane wyjściowe zależy od opcji, które świadczą polecenia. Oto przykładowe dane wyjściowe z wiersza polecenia platformy Azure:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
{
  "location": "westus",
  "overprovision": true,
  "plan": null,
  "singlePlacementGroup": true,
  "sku": {
    "additionalProperties": {},
    "capacity": 1,
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
  .
  .
  .
}
```

Jak widać, te właściwości są stosowane do skalowania, ustawić jako całość.



### <a name="scale-set-instance-view"></a>Widok wystąpienia zestawu skalowania

Widok wystąpienia, który przechwytuje bieżącego ma również zestaw skalowania *środowiska uruchomieniowego* ustawiony stan skali jako całość. Aby zbadać widok wystąpienia zestawu skali, można użyć:

* INTERFEJS API REST: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/instanceView?api-version={apiVersion}` 
   
  Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getinstanceview).

* Program PowerShell: 

  `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceView` 
  
  Aby uzyskać więcej informacji, zobacz [dokumentacji programu PowerShell](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss).

* Azure CLI: 

  `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName}` 
   
  Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view).

Można również użyć [Eksploratora zasobów Azure (wersja zapoznawcza)](https://resources.azure.com) lub [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) zbadać widok wystąpienia zestawu skali.

Dokładny opis dane wyjściowe zależy od opcji, które świadczą polecenia. Oto przykładowe dane wyjściowe z wiersza polecenia platformy Azure:

```
$ az vmss get-instance-view -g {resourceGroupName} -n {virtualMachineScaleSetName}
{
  "statuses": [
    {
      "additionalProperties": {},
      "code": "ProvisioningState/succeeded",
      "displayStatus": "Provisioning succeeded",
      "level": "Info",
      "message": null,
      "time": "{time}"
    }
  ],
  "virtualMachine": {
    "additionalProperties": {},
    "statusesSummary": [
      {
        "additionalProperties": {},
        "code": "ProvisioningState/succeeded",
        "count": 1
      }
    ]
  }
  .
  .
  .
}
```

Jak widać, właściwości te zapewniają Podsumowanie bieżącego stanu środowiska uruchomieniowego w zestawie skalowania maszyn wirtualnych. Podsumowanie zawiera stan rozszerzenia stosowane do skalowania, ustaw (pominięty w celu jego skrócenia).



### <a name="scale-set-vm-model-view"></a>Widok modelu maszyny Wirtualnej zestawu skalowania

Podobnie jak zestaw skali ma widok modelu, każdej maszyny Wirtualnej w zestawie skalowania ma własną widok modelu. Aby zbadać widok modelu dla zestawu skalowania, można użyć:

* INTERFEJS API REST: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}?api-version={apiVersion}` 
  
  Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/get).

* Program PowerShell: 

  `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` 
  
  Aby uzyskać więcej informacji, zobacz [dokumentacji programu PowerShell](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm).

* Azure CLI: 

  `az vmss show -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` 
  
  Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show).

Można również użyć [Eksploratora zasobów Azure (wersja zapoznawcza)](https://resources.azure.com) lub [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) dla maszyny Wirtualnej w zestawie skalowania kwerendy modelu.

Dokładny opis dane wyjściowe zależy od opcji, które świadczą polecenia. Oto przykładowe dane wyjściowe z wiersza polecenia platformy Azure:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
{
  "location": "westus",
  "name": "{name}",
  "sku": {
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
  .
  .
  .
}
```

Jak widać, te właściwości opisano w konfiguracji maszyny Wirtualnej, nie konfiguracji skali ustawić jako całość. Na przykład modelu zestawu skali ma `overprovision` jako właściwość, należy ustawić modelu dla maszyny Wirtualnej w skali nie. Przyczyna ta różnica jest nadmiarowe Inicjowanie obsługi administracyjnej właściwość skali Ustaw jako całości, nie poszczególnych maszyn wirtualnych w zestawie skalowania. (Aby uzyskać więcej informacji na temat nadmiarowe Inicjowanie obsługi administracyjnej, zobacz [zagadnienia dotyczące zestawy skalowania projektowania](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview#overprovisioning).)



### <a name="scale-set-vm-instance-view"></a>Widok wystąpienia maszyny Wirtualnej zestawu skalowania

Podobnie jak zestaw skali ma widok wystąpienia każdej maszyny Wirtualnej w zestawie skalowania ma własną widok wystąpienia. Aby zbadać widok wystąpienia zestawu skali, można użyć:

* INTERFEJS API REST: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}/instanceView?api-version={apiVersion}` 
 
  Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/getinstanceview).

* Program PowerShell: 

  `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -InstanceView` 
  
  Aby uzyskać więcej informacji, zobacz [dokumentacji programu PowerShell](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm).

* Azure CLI: 

  `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` 
  
  Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view).

Można również użyć [Eksploratora zasobów Azure (wersja zapoznawcza)](https://resources.azure.com) lub [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) zbadać widok wystąpienia zestawu skali maszyny wirtualnej.

Dokładny opis dane wyjściowe zależy od opcji, które świadczą polecenia. Oto przykładowe dane wyjściowe z wiersza polecenia platformy Azure:

```
$ az vmss get-instance-view -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}
{
  "additionalProperties": {
    "osName": "ubuntu",
    "osVersion": "16.04"
  },
  "disks": [
    {
      "name": "{name}",
      "statuses": [
        {
          "additionalProperties": {},
          "code": "ProvisioningState/succeeded",
          "displayStatus": "Provisioning succeeded",
          "time": "{time}"
        }
      ]
    }
  ],
  "statuses": [
    {
      "additionalProperties": {},
      "code": "ProvisioningState/succeeded",
      "displayStatus": "Provisioning succeeded",
      "time": "{time}"
    },
    {
      "additionalProperties": {},
      "code": "PowerState/running",
      "displayStatus": "VM running"
    }
  ],
  "vmAgent": {
    "statuses": [
      {
        "additionalProperties": {},
        "code": "ProvisioningState/succeeded",
        "displayStatus": "Ready",
        "level": "Info",
        "message": "Guest Agent is running",
        "time": "{time}"
      }
    ],
    "vmAgentVersion": "{version}"
  },
  .
  .
  .
}
```

Jak widać, te właściwości opisują bieżący stan środowiska uruchomieniowego samej maszyny Wirtualnej. Stan obejmuje wszystkie rozszerzenia stosowane do skalowania, ustaw (pominięty w celu jego skrócenia).




## <a name="techniques-for-updating-global-scale-set-properties"></a>Techniki aktualizowanie skali globalnej Ustawianie właściwości

Aby zaktualizować właściwości zestawu skali globalnej, należy zaktualizować właściwości w modelu zestawu skali. Można wykonać tej aktualizacji przy użyciu:

* INTERFEJS API REST: 

  `PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` 
  
  Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate).

  Można też wdrożyć szablonu usługi Azure Resource Manager przy użyciu właściwości z interfejsu API REST, aby zaktualizować właściwości zestawu skali globalnej.

* Program PowerShell: 

  `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -VirtualMachineScaleSet {scaleSetConfigPowershellObject}` 
  
  Aby uzyskać więcej informacji, zobacz [dokumentacji programu PowerShell](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss).

* Azure CLI:

  * Aby zmodyfikować właściwości: `az vmss update --set {propertyPath}={value}` 
  
  * Aby dodać obiekt do listy właściwości w zestawie skali: `az vmss update --add {propertyPath} {JSONObjectToAdd}` 
  
  * Aby usunąć obiekt z listy właściwości w zestawie skali: `az vmss update --remove {propertyPath} {indexToRemove}` 
  
  Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update). 
  
  Alternatywnie Jeśli wcześniej wdrożona skali skonfigurowane za pomocą `az vmss create` polecenia, można uruchomić `az vmss create` polecenie ponownie, aby zaktualizować zestawu skalowania. Aby to zrobić, upewnij się, że wszystkie właściwości w `az vmss create` polecenia są takie same jak poprzednio, z wyjątkiem właściwości, które chcesz zmodyfikować.



Można również użyć [Eksploratora zasobów Azure (wersja zapoznawcza)](https://resources.azure.com) lub [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) zaktualizować skali zestaw modelu.

Po zaktualizowaniu modelu zestawu skali nowej konfiguracji dotyczy wszelkie nowe maszyny wirtualne utworzone w zestawie skalowania. Jednak modeli dla istniejących maszyn wirtualnych w zestawie skalowania musi nadal można przełączyć na bieżąco najnowsze ogólną skali zestaw modelu. W modelu dla każdej maszyny Wirtualnej o nazwie właściwości typu Boolean `latestModelApplied` wskazuje, czy maszyna wirtualna jest bieżąco najnowsze ogólną skali zestaw modelu. (Wartość `true` oznacza, że maszyna wirtualna jest aktualne najnowszego modelu.)




## <a name="techniques-for-bringing-vms-up-to-date-with-the-latest-scale-set-model"></a>Techniki za aktualny najnowszego modelu zestawu skali maszyny wirtualne

Zestawy skalowania ma *uaktualnienia zasad* określający, jak zostały podane na bieżąco z najnowszego modelu zestawu skali maszyny wirtualne. Są trzy tryby dla zasad uaktualniania:

- **Automatyczne**: W tym trybie zestaw skalowania sprawia, że nie gwarantuje o zamówieniu maszyn wirtualnych, które są obniżył. Zestaw skali może wyłączyć wszystkie maszyny wirtualne w tym samym czasie. 
- **Wycofanie**: W tym trybie zestaw skalowania zbiera i wydaje aktualizację w partiach z czasu wstrzymania opcjonalne między partie.
- **Ręczne**: W tym trybie, podczas aktualizowania modelu zestawu skali, nic się nie dzieje do istniejących maszyn wirtualnych. Aby zaktualizować istniejące maszyny wirtualne, należy ręcznie uaktualnić każdej z nich. Możliwość to ręczne uaktualnienie za pomocą:

  - INTERFEJS API REST: 
  
    `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/manualupgrade?api-version={apiVersion}` 
    
    Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/updateinstances).

  - Program PowerShell: 
  
    `Update-AzureRmVmssInstance -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` 
    
    Aby uzyskać więcej informacji, zobacz [dokumentacji programu PowerShell](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmssinstance).

  - Azure CLI: 
  
    `az vmss update-instances -g {resourceGroupName} -n {vmScaleSetName} --instance-ids {instanceIds}` 
    
    Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update_instances).

  Można również użyć [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) należy ręcznie uaktualnić maszyny Wirtualnej w zestawie skalowania.

>[!NOTE]
> Klastrów sieci szkieletowej usług Azure można używać tylko trybu automatycznego, ale aktualizacja różni się. Aby uzyskać więcej informacji o aktualizacjach usługi sieć szkieletowa usług, zobacz [dokumentacji usługi sieć szkieletowa](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade).

Jeden typ modyfikacji właściwości zestawu skali globalnej nie jest zgodna z zasad uaktualniania: zmiany w skali Ustaw profil systemu operacyjnego. (Przykłady są nazwa użytkownika i hasło). Te właściwości mogą być zmienione tylko w wersji interfejsu API 2017-12-01 lub nowszej. Te zmiany dotyczą tylko maszyny wirtualne utworzone po zmianie w skali modelu. Aby przenieść istniejące maszyny wirtualne na bieżąco, możesz odtworzyć każdej istniejącej maszyny Wirtualnej. Można odtworzyć z obrazu maszyny Wirtualnej za pomocą:

* INTERFEJS API REST: 

  `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` 
  
  Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage).

* Program PowerShell: 

  `Set-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` 
  
  Aby uzyskać więcej informacji, zobacz [dokumentacji programu PowerShell](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm).

* Azure CLI: 

  `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` 
  
  Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_reimage).

Można również użyć [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) można odtworzyć z obrazu maszyny Wirtualnej w zestawie skalowania.




## <a name="properties-with-restrictions-on-modification"></a>Właściwości z ograniczeń modyfikacji

### <a name="create-time-properties"></a>Właściwości czasu tworzenia

Niektóre właściwości można ustawić tylko wtedy, gdy tworzysz początkowo zestaw skali. Te właściwości obejmują:

- Strefy
- Obraz odwołanie wydawcy
- Oferta odwołanie do obrazu

### <a name="properties-that-can-be-changed-based-on-the-current-value-only"></a>Właściwości, które można zmieniać w oparciu o tylko bieżącą wartość

Niektóre właściwości można zmieniać, poza wyjątkami, w zależności od bieżącej wartości. Te właściwości obejmują:

- `singlePlacementGroup`: Jeśli `singlePlacementGroup` ma wartość true, można zmodyfikować na wartość false. Jednak jeśli `singlePlacementGroup` ma wartość false, jego *nie* można zmodyfikować na wartość true.
- `subnet`: Tak długo, jak oryginalny podsieci i nową podsieć znajdują się w tej samej sieci wirtualnej można zmodyfikować podsieci zestaw skali.

### <a name="properties-that-require-deallocation-to-change"></a>Właściwości, które wymagają zmiany cofania alokacji

Niektóre właściwości można zmienić na określone wartości tylko wtedy, gdy w zestawie skalowania maszyn wirtualnych są alokację. Te właściwości obejmują:

- `sku name`: Nowe jednostki SKU maszyny Wirtualnej nie jest obsługiwana na sprzęcie, który zestaw skalowania jest obecnie włączona, należy cofnąć maszyn wirtualnych w skali ustawiona przed zmodyfikowaniem `sku name`. Aby uzyskać więcej informacji na zmianę rozmiaru maszyn wirtualnych, zobacz [to Azure wpis w blogu](https://azure.microsoft.com/blog/resize-virtual-machines/).


## <a name="vm-specific-updates"></a>Aktualizacje specyficzne dla maszyny Wirtualnej

Pewne zmiany można zastosować do określonej maszyn wirtualnych, zamiast właściwości zestawu skali globalnej. Obecnie tylko aktualizacji specyficzne dla maszyny Wirtualnej, która jest obsługiwana jest dołączanie/odłączanie dysków danych do/z maszyn wirtualnych w zestawie skalowania. Ta funkcja jest dostępna w wersji zapoznawczej. Aby uzyskać więcej informacji, zobacz [dokumentację w wersji preview](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).

## <a name="scenarios"></a>Scenariusze

### <a name="application-updates"></a>Aktualizacje aplikacji

Jeśli aplikacja jest wdrażana do skali ustawiana za pośrednictwem rozszerzenia, aktualizowania konfiguracji rozszerzenia powoduje, że aplikacja do zaktualizowania zgodnie z zasadami uaktualnienia. Na przykład, jeśli masz nową wersję skryptu do uruchomienia w rozszerzeniu niestandardowego skryptu, może zaktualizować `fileUris` właściwości, aby wskazywały nowy skrypt. 

W niektórych przypadkach można wymusić aktualizację, nawet jeśli konfiguracji rozszerzenia jest bez zmian. (Na przykład możesz zaktualizować skrypt bez zmieniania identyfikator URI skryptu.) W takich przypadkach można modyfikować `forceUpdateTag` Aby wymusić aktualizację. Platformy Azure nie ma możliwości interpretowania tej właściwości, aby zmienić jej wartość nie ma wpływu na sposób uruchamiania rozszerzenia. Po prostu modyfikowania jej wymusza rozszerzenia, aby ponownie uruchomić. 

Aby uzyskać więcej informacji na temat `forceUpdateTag`, zobacz [dokumentacja interfejsu API REST dla rozszerzeń](https://docs.microsoft.com/rest/api/compute/virtualmachineextensions/createorupdate).

Jest również wspólne dla aplikacji można wdrożyć za pomocą niestandardowego obrazu. W tym scenariuszu zostało opisane w poniższej sekcji.

### <a name="os-updates"></a>Aktualizacje systemu operacyjnego

Jeśli używasz platformy obrazów, można zaktualizować obrazy, modyfikując `imageReference`. Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate).

>[!NOTE]
> Obrazy platformy jest często stosowanym rozwiązaniem określ "najnowszej" wersji odwołanie do obrazu. Oznacza to, że po utworzeniu zestawy skalowania skalowanej i ponownie, maszyn wirtualnych są tworzone z najnowszej dostępnej wersji. Jednak go *nie* oznacza, że obraz systemu operacyjnego zostanie automatycznie zaktualizowana w czasie jako wydawane są nowe wersje obrazu. Jest to funkcja oddzielne, obecnie w wersji zapoznawczej. Aby uzyskać więcej informacji, zobacz [uaktualnienia systemu operacyjnego automatyczne](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade).

Jeśli używasz niestandardowych obrazów, można zaktualizować obrazy, aktualizując `imageReference` identyfikatora. Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate).

## <a name="examples"></a>Przykłady

### <a name="update-the-os-image-for-your-scale-set"></a>Aktualizowanie obrazu systemu operacyjnego dla zestawu skalowania

Załóżmy, że masz skali ustawić uruchomiona stara wersja Ubuntu LTS 16.04. Chcesz zaktualizować do nowszej wersji 16.04 LTS Ubuntu (na przykład wersja 16.04.201801090). Właściwość version odwołanie do obrazu nie jest częścią listy, więc te właściwości można bezpośrednio modyfikować za pomocą tych poleceń:

* Program PowerShell: 

  `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -ImageReferenceVersion 16.04.201801090`

* Azure CLI: 

  `az vmss update -g {resourceGroupName} -n {vmScaleSetName} --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090`


### <a name="update-the-load-balancer-for-your-scale-set"></a>Aktualizacja usługi równoważenia obciążenia dla zestawu skalowania

Załóżmy, że masz skali ustawić usługi równoważenia obciążenia Azure i mają zostać zamienione usługi równoważenia obciążenia z bramy aplikacji Azure. Obciążenia równoważenia i aplikacji bramy właściwości zestawu skali są częścią listy. Tak można użyć polecenia usuwania i dodawanie elementów listy zamiast bezpośrednie modyfikowanie właściwości.

Program PowerShell:
```
# Get the current model of the scale set and store it in a local PowerShell object named $vmss
> $vmss=Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName}

# Create a local PowerShell object for the new desired IP configuration, which includes the reference to the application gateway
> $ipconf = New-AzureRmVmssIPConfig myNic -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name

# Replace the existing IP configuration in the local PowerShell object (which contains the references to the current Azure load balancer) with the new IP configuration
> $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf

# Update the model of the scale set with the new configuration in the local PowerShell object
> Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName} -virtualMachineScaleSet $vmss

```

Azure CLI:
```
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0 # Remove the load balancer back-end pool from the scale set model
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0 # Remove the load balancer back-end pool from the scale set model; only necessary if you have NAT pools configured on the scale set
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}' # Add the application gateway back-end pool to the scale set model
```

>[!NOTE]
> Tych poleceniach założono, że w zestawie skali jest tylko jeden IP konfiguracji i obciążenia modułu równoważenia. Dostępnych jest wiele, konieczne może użyć indeks listy innych niż 0.