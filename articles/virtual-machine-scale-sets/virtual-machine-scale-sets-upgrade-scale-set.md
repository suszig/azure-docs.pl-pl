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
ms.openlocfilehash: cdd1015f63e80b7ec51565c18f3440ce1828fb03
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="modify-a-virtual-machine-scale-set"></a>Zmodyfikuj zestaw skali maszyny wirtualnej
W tym artykule opisano sposób modyfikowania istniejącego zestawu skali. Dotyczy to również sposób zmiany konfiguracji skali ustawić zmienić konfigurację aplikacji uruchomionych na skali zestawu, jak zarządzać dostępności i inne.

## <a name="fundamental-concepts"></a>podstawowe pojęcia

### <a name="the-scale-set-model"></a>Modelu zestawu skali

Zestaw skali ma "skali zestaw model" przechwytujący *żądany* ustawiony stan skali jako całość. Aby odpytać modelu dla zestawu skalowania, można użyć:

Interfejs API REST: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` (Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/get))

Środowiska PowerShell: `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}` (Aby uzyskać więcej informacji, zobacz [dokumentacji programu Powershell](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss))

Interfejs wiersza polecenia: `az vmss show -g {resourceGroupName} -n {vmSaleSetName}` (Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show))

Można również użyć [resources.azure.com](https://resources.azure.com) lub [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) do badania modeli dla zestawu skalowania.

Dokładny opis dane wyjściowe zależy od opcji podane polecenia, ale poniżej przedstawiono niektóre przykładowe dane wyjściowe z poziomu interfejsu wiersza polecenia:

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



### <a name="the-scale-set-instance-view"></a>Widok wystąpienia zestawu skali

Skalę, również ustawić się "skali zestaw wystąpienie programu wyświetlić" przechwytujący bieżącego *środowiska uruchomieniowego* ustawiony stan skali jako całość. Aby zbadać widok wystąpienia zestawu skali, można użyć:

Interfejs API REST: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/instanceView?api-version={apiVersion}` (Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getinstanceview))

Środowiska PowerShell: `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceView` (Aby uzyskać więcej informacji, zobacz [dokumentacji programu Powershell](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss))

Interfejs wiersza polecenia: `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName}` (Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view))

Można również użyć [resources.azure.com](https://resources.azure.com) lub [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) zbadać widok wystąpienia zestawu skali.

Dokładny opis dane wyjściowe zależy od opcji podane polecenia, ale poniżej przedstawiono przykładowe dane wyjściowe z poziomu interfejsu wiersza polecenia:

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

Jak widać, właściwości te zapewniają ustawić Podsumowanie bieżącego stanu środowiska uruchomieniowego maszyn wirtualnych w skali, w tym zawiera stan rozszerzenia dotyczą zestaw skalowania (pominąć w przypadku skrócenia).



### <a name="the-scale-set-vm-model-view"></a>Widok modelu maszyny Wirtualnej zestawu skalowania

Podobnie jak zestaw skali ma widok modelu, każdej maszyny Wirtualnej w zestawie skalowania ma własną widok modelu. Aby zbadać widok modelu dla zestawu skalowania, można użyć:

Interfejs API REST: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}?api-version={apiVersion}` (Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/get))

Środowiska PowerShell: `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` (Aby uzyskać więcej informacji, zobacz [dokumentacji programu Powershell](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm))

Interfejs wiersza polecenia: `az vmss show -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` (Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show))

Można również użyć [resources.azure.com](https://resources.azure.com) lub [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) dla maszyny Wirtualnej w zestawie skalowania kwerendy modelu.

Dokładny opis dane wyjściowe zależy od opcji podane polecenia, ale poniżej przedstawiono niektóre przykładowe dane wyjściowe z poziomu interfejsu wiersza polecenia:

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

Jak widać, te właściwości opisano w konfiguracji maszyny Wirtualnej, nie konfiguracji skali ustawić jako całość. Na przykład modelu zestawu skali ma `overprovision` jako właściwość, a nie w modelu dla maszyny Wirtualnej w zestawie skalowania. Ta różnica wynika nadmiarowe Inicjowanie obsługi administracyjnej jest właściwością skali Ustaw jako całości, nie poszczególnych maszyn wirtualnych w zestawie skalowania (Aby uzyskać więcej informacji na temat nadmiarowe Inicjowanie obsługi administracyjnej, zobacz [tej dokumentacji](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview#overprovisioning)).



### <a name="the-scale-set-vm-instance-view"></a>Widok wystąpienia maszyny Wirtualnej zestawu skali

Podobnie jak zestaw skali ma widok wystąpienia każdej maszyny Wirtualnej w zestawie skalowania ma własną widok wystąpienia. Aby zbadać widok wystąpienia zestawu skali, można użyć:

Interfejs API REST: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}/instanceView?api-version={apiVersion}` (Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/getinstanceview))

Środowiska PowerShell: `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -InstanceView` (Aby uzyskać więcej informacji, zobacz [dokumentacji programu Powershell](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm))

Interfejs wiersza polecenia: `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` (Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view))

Można również użyć [resources.azure.com](https://resources.azure.com) lub [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) zbadać widok wystąpienia zestawu skali maszyny wirtualnej.

Dokładny opis dane wyjściowe zależy od opcji podane polecenia, ale poniżej przedstawiono niektóre przykładowe dane wyjściowe z poziomu interfejsu wiersza polecenia:

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

Jak widać, te właściwości opisują bieżący stan środowiska uruchomieniowego samej maszyny Wirtualnej, tym wszystkie rozszerzenia dotyczą zestaw skalowania (pominąć w przypadku skrócenia).




## <a name="how-to-update-global-scale-set-properties"></a>Jak zaktualizować skali globalnej Ustawianie właściwości

Aby zaktualizować właściwości zestawu skali globalnej, należy zaktualizować właściwości w modelu zestawu skali. Można wykonać tej aktualizacji przy użyciu:

Interfejs API REST: `PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` (Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate))

Szablony Menedżera zasobów: Alternatywnie można wdrożyć szablonu usługi Resource Manager, aby zaktualizować właściwości zestawu skali globalnej przy użyciu właściwości z interfejsu API REST.

Środowiska PowerShell: `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -VirtualMachineScaleSet {scaleSetConfigPowershellObject}` (Aby uzyskać więcej informacji, zobacz [dokumentacji programu Powershell](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss))

CLI. Aby zmodyfikować właściwości: `az vmss update --set {propertyPath}={value}`. Aby dodać obiekt do listy właściwości w zestawie skalowania: `az vmss update --add {propertyPath} {JSONObjectToAdd}`. Aby usunąć obiekt z listy właściwości w zestawie skalowania: `az vmss update --remove {propertyPath} {indexToRemove}`. (Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update)). Alternatywnie Jeśli wcześniej wdrożona skali ustawić za pomocą `az vmss create` polecenia, można uruchomić `az vmss create` polecenie ponownie, aby zaktualizować zestaw skali. Aby to zrobić, należy upewnić się, że wszystkie właściwości w `az vmss create` polecenia są takie same jak poprzednio, z wyjątkiem właściwości, którą chcesz zmodyfikować.



Można również użyć [resources.azure.com](https://resources.azure.com) lub [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) zaktualizować skali zestaw modelu.

Po zaktualizowaniu modelu zestawu skali, nowa konfiguracja dotyczy wszelkie nowe maszyny wirtualne utworzone w zestawie skalowania. Jednak modeli dla istniejących maszyn wirtualnych w zestawie skalowania nadal należy doprowadzić instalując najnowsze ogólną skali zestaw modelu. W modelu dla każdej maszyny Wirtualnej jest właściwością logiczną o nazwie `latestModelApplied` wskazująca, czy maszyna wirtualna jest aktualny i najnowszych ogólną skali zestaw modelu (`true` oznacza, że maszyna wirtualna jest aktualny i najnowszego modelu).




## <a name="how-to-bring-vms-up-to-date-with-the-latest-scale-set-model"></a>Sposobu objęcia pomocą najnowszego modelu zestawu skali maszyny wirtualne

Zestawy skalowania ma "zasad uaktualniania" określające, jak zostały podane aktualne z najnowszego modelu zestawu skali maszyny wirtualne. Są trzy tryby dla zasad uaktualniania:

- Automatyczny: W tym trybie zestaw skalowania sprawia, że nie gwarantuje o zamówieniu trwa obniżył maszyn wirtualnych. Zestaw skali może wyłączyć wszystkie maszyny wirtualne w tym samym czasie. 
- Wycofanie: W tym trybie zestaw skalowania wprowadza aktualizacji w partiach o czasie Wstrzymaj opcjonalne między partie.
- Metoda ręczna: W tym trybie, podczas aktualizowania modelu zestawu skali, nic się nie dzieje do istniejących maszyn wirtualnych. Aby zaktualizować istniejące maszyny wirtualne, należy to robić "Ręczne uaktualnianie" każdej istniejącej maszyny Wirtualnej. Możliwość to ręczne uaktualnienie za pomocą:

Interfejs API REST: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/manualupgrade?api-version={apiVersion}` (Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/updateinstances))

Środowiska PowerShell: `Update-AzureRmVmssInstance -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` (Aby uzyskać więcej informacji, zobacz [dokumentacji programu Powershell](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmssinstance))

Interfejs wiersza polecenia: `az vmss update-instances -g {resourceGroupName} -n {vmScaleSetName} --instance-ids {instanceIds}` (Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update_instances)).

Można również użyć [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) celu Ręczne uaktualnianie na maszynie Wirtualnej w zestawie skalowania.

>[!NOTE]
> Klastrów sieci szkieletowej usług można używać tylko trybu automatycznego, ale aktualizacja przebiega inaczej. Aby uzyskać więcej informacji na aktualizacje sieci szkieletowej usług, zobacz [dokumentacji usługi sieć szkieletowa](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade).

>[!NOTE]
> Istnieje jeden typ modyfikacji właściwości zestawu skali globalnej, który nie jest zgodna z zasad uaktualniania. Są to zmiany skali ustawić profil systemu operacyjnego (na przykład nazwa użytkownika i hasło). Te właściwości można tylko zmienione w wersji interfejsu API 2017-12-01 lub nowszej. Te zmiany dotyczą tylko maszyny wirtualne utworzone po zmianie w skali modelu. Aby przenieść istniejące maszyny wirtualne, które są aktualne, należy to robić "odtworzenia z obrazu" każdej istniejącej maszyny wirtualnej. Możesz zrobić to odtworzenia z obrazu za pomocą:

Interfejs API REST: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` (Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage))

Środowiska PowerShell: `Set-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` (Aby uzyskać więcej informacji, zobacz [dokumentacji programu Powershell](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm))

Interfejs wiersza polecenia: `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` (Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_reimage)).

Można również użyć [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) można odtworzyć z obrazu maszyny Wirtualnej w zestawie skalowania.




## <a name="properties-with-restrictions-on-modification"></a>Właściwości z ograniczeń modyfikacji

### <a name="create-time-properties"></a>Właściwości czasu tworzenia

Niektóre właściwości można ustawić tylko podczas tworzenia wstępnie skali zestawu. Te właściwości obejmują:

- Strefy
- Obraz odwołanie wydawcy
- Oferta odwołanie do obrazu

### <a name="properties-that-can-only-be-changed-based-on-the-current-value"></a>Właściwości, które można zmienić tylko na podstawie bieżącej wartości

Niektóre właściwości mogą zmienić wyjątków w zależności od bieżącej wartości. Te właściwości obejmują:

- singlePlacementGroup: Jeśli singlePlacementGroup ma wartość true, mogą zostać zmienione na wartość false. Jednak jeśli singlePlacementGroup ma wartość false, on **nie mogą** można zmodyfikować na wartość true.
- podsieci: podsieci zestawu skali może być modyfikowany, tak długo, jak oryginalny podsieci i nową podsieć znajdują się w tej samej sieci wirtualnej.

### <a name="properties-that-require-deallocation-to-change"></a>Właściwości, które wymagają zmiany cofania alokacji

Niektóre właściwości mogą można zmienić tylko niektóre wartości, jeśli w zestawie skalowania maszyn wirtualnych są alokację. Te właściwości obejmują:

- Nazwa jednostki SKU: Jeśli nowej jednostki SKU maszyny Wirtualnej nie jest obsługiwana na sprzęcie skali zestaw jest aktualnie, musisz cofnąć maszyn wirtualnych w skali ustawiona przed zmodyfikowaniem nazwa jednostki sku. Aby uzyskać więcej informacji na zmianę rozmiaru maszyn wirtualnych, zobacz [to Azure wpis w blogu](https://azure.microsoft.com/blog/resize-virtual-machines/).


## <a name="vm-specific-updates"></a>Aktualizacje specyficzne dla maszyny Wirtualnej

Pewne zmiany mogą być stosowane do określonych maszyn wirtualnych, zamiast właściwości zestawu skali globalnej. Obecnie tylko aktualizacji specyficzne dla maszyny Wirtualnej, która jest obsługiwana jest dołączanie/odłączanie dysków danych do/z maszyn wirtualnych w zestawie skalowania. Ta funkcja jest dostępna w wersji zapoznawczej. Aby uzyskać więcej informacji, zobacz [dokumentację w wersji preview](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).

## <a name="scenarios-application-updates-os-updates-etc"></a>Scenariusze: Aktualizacji aplikacji, aktualizacje systemu operacyjnego itd.

### <a name="application-updates"></a>Aktualizacje aplikacji

Jeśli aplikacja jest wdrażana do skali ustawiana za pośrednictwem rozszerzenia, aktualizowania konfiguracji rozszerzenia powoduje, że aplikacja do aktualizacji zgodnie z zasadami uaktualnienia. Na przykład jeśli masz nową wersję skryptu do uruchomienia w rozszerzeniu niestandardowego skryptu, można zaktualizować właściwości fileUris, aby wskazywały nowy skrypt. W niektórych przypadkach, jednak możesz wymusić aktualizację, nawet jeśli jest bez zmian konfiguracji rozszerzenia (na przykład można zaktualizować skrypt bez zmieniania identyfikator URI skryptu). W takich przypadkach można zmodyfikować wartość forceUpdateTag, aby wymusić aktualizację. Platformy Azure nie ma możliwości interpretowania tej właściwości, aby zmienić jej wartość nie ma wpływu na sposób uruchamiania rozszerzenia. Po prostu modyfikowania jej wymusza rozszerzenia, aby ponownie uruchomić. Aby uzyskać więcej informacji o wartość forceUpdateTag, zobacz [dokumentacja interfejsu API REST dla rozszerzeń](https://docs.microsoft.com/rest/api/compute/virtualmachineextensions/createorupdate).

Jest również wspólne dla aplikacji można wdrożyć za pomocą niestandardowego obrazu. W tym scenariuszu zostało opisane w poniższej sekcji "Aktualizacje systemu operacyjnego"

### <a name="os-updates"></a>Aktualizacje systemu operacyjnego

Jeśli używasz platformy obrazów, można zaktualizować obraz, modyfikując elementu imageReference (więcej informacji w [dokumentacja interfejsu API REST](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate)).

>[!NOTE]
> Obrazy platformy jest często stosowanym rozwiązaniem określ "najnowszej" wersji odwołanie do obrazu. Oznacza to, że podczas skalowania zestaw utworzyć, skalowania w poziomie i odtworzenia z obrazu maszyny wirtualne są tworzone z najnowszej dostępnej wersji. Jednak go **nie** oznacza, że obraz systemu operacyjnego zostanie automatycznie zaktualizowana w czasie jako wydawane są nowe wersje obrazu. Jest to funkcja oddzielne, obecnie w wersji zapoznawczej. Aby uzyskać więcej informacji, zobacz [dokumentacji automatycznych uaktualnień systemu operacyjnego](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade).

Jeśli używasz niestandardowych obrazów, można zaktualizować obraz, aktualizując identyfikator elementu imageReference (więcej informacji w [dokumentacja interfejsu API REST](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate)).

## <a name="examples"></a>Przykłady

### <a name="updating-the-os-image-for-your-scale-set"></a>Aktualizowanie obrazu systemu operacyjnego dla zestawu skalowania

Załóżmy, że masz skali ustawić uruchomiona stara wersja Ubuntu LTS 16.04 i chcesz zaktualizować do nowszej wersji 16.04 LTS Ubuntu (na przykład wersja 16.04.201801090). Właściwość version odwołanie do obrazu nie jest częścią listy, więc można bezpośrednio zmodyfikować te właściwości przy użyciu następujących poleceń:

Środowiska PowerShell: `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -ImageReferenceVersion 16.04.201801090`

INTERFEJS WIERSZA POLECENIA: `az vmss update -g {resourceGroupName} -n {vmScaleSetName} --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090`


### <a name="updating-the-load-balancer-for-your-scale-set"></a>Aktualizowanie usługi równoważenia obciążenia dla zestawu skalowania

Załóżmy, że masz skali ustawić usługi równoważenia obciążenia Azure i chcesz zastąpić modułu równoważenia obciążenia Azure z bramy aplikacji Azure. Obciążenia równoważenia i aplikacji bramy właściwości zestawu skali są częścią listy, dzięki czemu można używać polecenia usuwania i dodawanie elementów listy zamiast bezpośrednie modyfikowanie właściwości:

Środowiska PowerShell: 
```
# get the current model of the scale set and store it in a local powershell object named $vmss
> $vmss=Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName}

# create a local powershell object for the new desired IP configuration, which includes the referencerence to the application gateway
> $ipconf = New-AzureRmVmssIPConfig myNic -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name

# replace the existing IP configuration in the local powershell object (which contains the references to the current Azure Load Balancer) with the new IP configuration
> $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf

# Update the model of the scale set with the new configuration in the local powershell object
> Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName} -virtualMachineScaleSet $vmss

```

INTERFEJS WIERSZA POLECENIA: 
```
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0 # remove the load balancer backend pool from the scale set model
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0 # remove the load balancer backend pool from the scale set model; only necessary if you have NAT pools configured on the scale set
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}' # add the application gateway backend pool to the scale set model
```

>[!NOTE]
> Tych poleceniach założono, że w zestawie skali jest tylko jeden IP konfiguracji i obciążenia modułu równoważenia. Jeśli dostępnych jest wiele, może być konieczne użycie indeksu listy, innego niż 0.