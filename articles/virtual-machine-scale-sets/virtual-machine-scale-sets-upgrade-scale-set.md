---
title: Uaktualnij zestaw skali maszyny wirtualnej platformy Azure | Dokumentacja firmy Microsoft
description: Uaktualnij zestaw skali maszyny wirtualnej platformy Azure
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
ms.date: 05/30/2017
ms.author: gunegatybo
ms.openlocfilehash: fbdc9d40173a40f35eee60cadfdd258293509d53
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/20/2017
---
# <a name="upgrade-a-virtual-machine-scale-set"></a>Uaktualnij zestaw skali maszyny wirtualnej
W tym artykule opisano, jak można wdrożeniem aktualizacji systemu operacyjnego do skali maszyny wirtualnej platformy Azure, bez żadnych przestojów. W tym kontekście aktualizacji systemu operacyjnego obejmuje zmiana wersji lub wersji systemu operacyjnego lub zmiana identyfikatora URI obrazu niestandardowego. Aktualizowanie bez przestojów aktualizowanie maszyn wirtualnych co pojedynczo lub w grupach (na przykład w jednej domenie błędów w czasie) zamiast jednocześnie. W ten sposób można zachować uruchomiona żadnych maszyn wirtualnych, które nie jest uaktualniany.

Aby uniknąć niejednoznaczności, ta funkcja pozwala odróżnić cztery typy aktualizacji systemu operacyjnego, które można wykonać:

* Zmiana wersji lub jednostka SKU obrazu platformy. Na przykład zmiana Ubuntu wersji 14.04.2-LTS z 14.04.201506100 14.04.201507060, lub zmianę 16.04.0-LTS/latest Ubuntu 15.10/najnowszej wersji. W tym scenariuszu są omówione w tym artykule.
* Zmiana identyfikatora URI, który wskazuje do nowej wersji niestandardowego obrazu wbudowane (**właściwości** > **virtualMachineProfile** > **storageProfile**  >  **osDisk** > **obrazu** > **uri**). W tym scenariuszu są omówione w tym artykule.
* Zmiana odwołanie do obrazu zestawu skalowania, który został utworzony przy użyciu dysków zarządzanych platformy Azure.
* Stosowanie poprawek systemu operacyjnego z poziomu maszyny wirtualnej (to przykłady instalowania poprawki zabezpieczeń i uruchamiania usługi Windows Update). Ten scenariusz jest obsługiwany, ale nie zostały omówione w tym artykule.

Zestawy skalowania maszyn wirtualnych, które są wdrażane w ramach [sieć szkieletowa usług Azure](https://azure.microsoft.com/services/service-fabric/) klastra nie zostały omówione w tym miejscu. Aby uzyskać więcej informacji na temat stosowania poprawek sieci szkieletowej usług, zobacz [poprawka systemu operacyjnego Windows w klastrze usługi sieć szkieletowa](https://docs.microsoft.com/azure/service-fabric/service-fabric-patch-orchestration-application)

Zmiana wersji systemu operacyjnego/SKU obrazu platformy lub identyfikator URI niestandardowego obrazu podstawowego sekwencji wygląda następująco:

1. Pobierz modelu zestawu skali maszyny wirtualnej.
2. Zmiana wersji jednostki SKU, odwołanie do obrazu albo wartość identyfikatora URI w modelu.
3. Aktualizowanie modelu.
4. Czy *manualUpgrade* wywołania w przypadku maszyn wirtualnych w zestawie skalowania. Ten krok ma zastosowanie tylko jeśli *upgradePolicy* ustawiono **ręcznego** w zestawie skalowania użytkownika. Jeśli wartość jest ustawiona na **automatyczne**, wszystkie maszyny wirtualne są uaktualniane na raz, powodując przestoje.

Dzięki tym informacjom pamiętać Zobaczmy, jak można zaktualizować wersji skali, ustaw w programie PowerShell i przy użyciu interfejsu API REST. Poniższe przykłady obejmuje również obrazu platformy, ale ten artykuł zawiera za mało informacji w celu dostosowania tego procesu jako obraz niestandardowy.

## <a name="powershell"></a>PowerShell
W tym przykładzie aktualizuje skalowania maszyny wirtualnej systemu Windows, ustaw (Tworzenie do nowej wersji 4.0.20160229. Po zaktualizowaniu modelu, robi aktualizacji jedno wystąpienie maszyny wirtualnej w czasie.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Aktualizowania identyfikatora URI dla niestandardowego obrazu zamiast zmiana wersji obrazu platformy, Zamień polecenia, która aktualizuje obrazu źródłowego identyfikatora URI "Ustawianie nowej wersji" wiersza. Na przykład jeśli zestaw skalowania utworzono bez użycia dysków zarządzanych Azure, aktualizacja może wyglądać następująco:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```

Jeśli zestaw niestandardowych skali opartej na obrazie został utworzony za pomocą dysków zarządzanych Azure, czy można zaktualizować odwołanie do obrazu. Na przykład:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.id = $newImageReference
```

## <a name="the-rest-api"></a>Interfejs API REST
Oto kilka przykładów Python, których wdrożenie aktualizacji wersji systemu operacyjnego za pomocą interfejsu API REST Azure. Oba rozwiązania używają niewielka [azurerm](https://pypi.python.org/pypi/azurerm) biblioteki interfejsu API REST Azure otoki funkcji celu GET na skali ustawić modelu, następuje PUT z zaktualizowanym modelu. Obejrzyj one również widoków wystąpień maszyny wirtualnej do identyfikacji maszyn wirtualnych za pomocą domeny aktualizacji.

### <a name="vmssupgrade"></a>Vmssupgrade
 [Vmssupgrade](https://github.com/gbowerman/vmsstools) jest skrypt w języku Python, który umożliwia wdrażanie uaktualnienia systemu operacyjnego uruchomionego skali maszyny wirtualnej ustawić jednej domeny aktualizacji w czasie.

![Skrypt Vmssupgrade dotyczące wybierania maszyn wirtualnych lub domeny aktualizacji](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Ten skrypt umożliwia wybranie określonych maszyn wirtualnych, aby zaktualizować lub określ domenę aktualizacji. Obsługuje ona zmiana wersji obrazu platformy lub zmiana identyfikatora URI obrazu niestandardowego.

### <a name="vmsseditor"></a>Vmsseditor
[Vmsseditor](https://github.com/gbowerman/vmssdashboard) jest Edytor ogólnego przeznaczenia dla zestawy skalowania maszyn wirtualnych, które pokazuje wirtualnej maszynie stan jako heatmap gdzie jeden wiersz odpowiada jednej domeny aktualizacji. Między innymi można zaktualizować model dla zestawu skalowania nowej wersji, jednostki SKU lub obraz niestandardowy identyfikator URI, a następnie wybierz domen błędów w celu uaktualnienia. Po wykonaniu wszystkich maszyn wirtualnych w tej domenie aktualizacji są uaktualniane do nowego modelu. Alternatywnie można wykonać uaktualnienia stopniowego zależnie od rozmiaru partii wybranych przez użytkownika.  

Poniższy zrzut ekranu przedstawia model dla Ubuntu 14.04-2LTS wersji 14.04.201507060 skali. Wiele więcej opcji zostały dodane do tego narzędzia, ponieważ został przełączony do tego zrzutu ekranu.

![Model Vmsseditor skali, ustaw dla Ubuntu 14.04-2LTS](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Po kliknięciu **uaktualnienia** , a następnie **Pobierz szczegóły**, maszyn wirtualnych w UD 0 start do aktualizacji.

![Trwa aktualizacja przedstawiający Vmsseditor](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)

