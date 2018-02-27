---
title: "Zestawy skalowania maszyn wirtualnych platformy Azure — dołączone dyski danych | Microsoft Docs"
description: "Informacje o używaniu dołączonych dysków danych z zestawami skalowania maszyn wirtualnych"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/25/2017
ms.author: negat
ms.openlocfilehash: 52ea7e35b941d5b1e45f39203757e4a3644cc9a5
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="azure-virtual-machine-scale-sets-and-attached-data-disks"></a>Zestawy skalowania i dołączone dyski danych maszyn wirtualnych platformy Azure
[Zestawy skalowania maszyn wirtualnych](/azure/virtual-machine-scale-sets/) platformy Azure obsługują teraz maszyny wirtualne z dołączonymi dyskami danych. Dla zestawów skalowania utworzonych przy użyciu usługi Azure Managed Disks można zdefiniować dyski danych w profilu magazynu. Wcześniej jedynymi opcjami bezpośrednio dołączonego magazynu dostępnymi dla maszyn wirtualnych w zestawach skalowania były dysk systemu operacyjnego oraz dyski tymczasowe.

> [!NOTE]
>  Podczas tworzenia zestawu skalowania ze zdefiniowanymi dołączonymi dyskami danych nadal konieczne jest zainstalowanie i sformatowanie tych dysków z poziomu maszyny wirtualnej w celu ich użycia (podobnie jak w przypadku autonomicznych maszyn wirtualnych platformy Azure). Wygodnym sposobem wykonania tych czynności jest użycie rozszerzenia niestandardowego skryptu, które wywołuje skrypt standardowy w celu podzielenia na partycje i sformatowania wszystkich dysków danych na maszynie wirtualnej.

## <a name="create-a-scale-set-with-attached-data-disks"></a>Tworzenie zestawu skalowania z dołączonymi dyskami danych
Prostym sposobem utworzenia zestawu skalowania z dyskami dołączonymi jest użycie polecenia [az vmss create](/cli/azure/vmss#az_vmss_create). W poniższym przykładzie zostaje utworzona grupa zasobów platformy Azure oraz zestaw skalowania maszyn wirtualnych obejmujący 10 maszyn wirtualnych z systemem Ubuntu, z których każda ma 2 dołączone dyski danych o pojemności 50 GB i 100 GB.

```bash
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

Polecenie [az vmss create](/cli/azure/vmss#az_vmss_create) powoduje ustawienie pewnych domyślnych wartości konfiguracji, jeśli nie zostaną określone. Aby wyświetlić dostępne opcje, które można przesłonić, spróbuj użyć polecenia:

```bash
az vmss create --help
```

Innym sposobem tworzenia zestawu skalowania z dołączonymi dyskami danych jest zdefiniowanie zestawu skalowania w szablonie usługi Azure Resource Manager, uwzględnienie sekcji _dataDisks_ w obszarze _storageProfile_ i wdrożenie szablonu. Opisane w poprzednim przykładzie dyski o pojemności 50 GB i 100 GB zostaną zdefiniowane, jak pokazano w poniższym przykładowym szablonie:

```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    }
]
```

Kompletny, gotowy do wdrożenia przykład szablonu zestawu skalowania ze zdefiniowanym dyskiem dołączonym można zobaczyć tutaj: [https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data](https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data).

## <a name="create-a-service-fabric-cluster-with-attached-data-disks"></a>Tworzenie klastra usługi Service Fabric z dołączonymi dyskami danych
Każdy [typ węzła](../service-fabric/service-fabric-cluster-nodetypes.md) w klastrze usługi [Service Fabric](/azure/service-fabric) uruchomiony na platformie Azure jest obsługiwany przez zestaw skalowania maszyn wirtualnych.  Za pomocą szablonu usługi Azure Resource Manager możesz dołączać dyski danych do zestawów skalowania tworzących klaster usługi Service Fabric. Jako punktu startowego możesz użyć [istniejącego szablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates). W szablonie dołącz sekcję _dataDisks_ w obszarze _storageProfile_ zasobów _Microsoft.Compute/virtualMachineScaleSets_, a następnie wdróż szablon. Poniższy przykład służy do dołączania dysku z danymi o rozmiarze 128 GB:

```json
"dataDisks": [
    {
    "diskSizeGB": 128,
    "lun": 0,
    "createOption": "Empty"
    }
]
```

Po wdrożeniu klastra możesz automatycznie tworzyć partycje na dyskach danych, a także formatować je i instalować.  Dodaj rozszerzenie niestandardowego skryptu do sekcji _extensionProfile_ obszaru _virtualMachineProfile_ zestawów skalowania.

Aby automatycznie przygotować dyski danych w klastrze systemu Windows, dodaj następujący fragment kodu:

```json
{
    "name": "customScript",    
    "properties": {    
        "publisher": "Microsoft.Compute",    
        "type": "CustomScriptExtension",    
        "typeHandlerVersion": "1.8",    
        "autoUpgradeMinorVersion": true,    
        "settings": {    
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.ps1"
        ],
        "commandToExecute": "powershell -ExecutionPolicy Unrestricted -File prepare_vm_disks.ps1"
        }
    }
}
```
Aby automatycznie przygotować dyski danych w klastrze systemu Linux, dodaj następujący fragment kodu:
```json
{
    "name": "lapextension",
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"
        ],
        "commandToExecute": "bash prepare_vm_disks.sh"
        }
    }
}
```

## <a name="adding-a-data-disk-to-an-existing-scale-set"></a>Dodawanie dysku danych do istniejącego zestawu skalowania
> [!NOTE]
>  Dyski danych można dołączać tylko do zestawu skalowania, który został utworzony za pomocą usługi [Azure Managed Disks](./virtual-machine-scale-sets-managed-disks.md).

Dysk danych można dodać do zestawu skalowania maszyn wirtualnych przy użyciu polecenia _az vmss disk attach_ interfejsu wiersza polecenia platformy Azure. Należy pamiętać o określeniu właściwości LUN, która nie jest jeszcze używana. W poniższym przykładzie z interfejsu wiersza polecenia do właściwości LUN 3 zostaje dodany dysk o rozmiarze 50 GB:

```bash
az vmss disk attach -g dsktest -n dskvmss --size-gb 50 --lun 3
```

W poniższym przykładzie przy użyciu programu PowerShell do właściwości LUN 3 zostaje dodany dysk o rozmiarze 50 GB:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myvmssrg -VMScaleSetName myvmss
$vmss = Add-AzureRmVmssDataDisk -VirtualMachineScaleSet $vmss -Lun 3 -Caching 'ReadWrite' -CreateOption Empty -DiskSizeGB 50 -StorageAccountType StandardLRS
Update-AzureRmVmss -ResourceGroupName myvmssrg -Name myvmss -VirtualMachineScaleSet $vmss
```

> [!NOTE]
> Różne rozmiary maszyn wirtualnych mają różne limity dotyczące obsługiwanej przez nie liczby dysków dołączonych. Przed dodaniem nowego dysku zapoznaj się z [charakterystyką rozmiarów maszyn wirtualnych](../virtual-machines/windows/sizes.md).

Dysk można również dodać przez dodanie nowego wpisu do właściwości _dataDisks_ w obszarze _storageProfile_ definicji zestawu skalowania i przez zastosowanie tej zmiany. Aby to przetestować, odszukaj istniejącą definicję zestawu skalowania w [Eksploratorze zasobów Azure](https://resources.azure.com/). Wybierz pozycję _Edytuj_ i dodaj nowy dysk do listy dysków danych, tak jak to pokazano w poniższym przykładzie:

```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    },
    {
    "lun": 3,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 20
    }          
]
```

Następnie wybierz pozycję _PUT_, aby zastosować zmiany do zestawu skalowania. Ten przykład sprawdzi się w przypadku korzystania z takiego rozmiaru maszyny wirtualnej, który zapewnia obsługę więcej niż dwóch dołączonych dysków danych.

> [!NOTE]
> Wprowadzenie w definicji zestawu skalowania zmiany, takiej jak dodanie lub usunięcie dysku danych, ma zastosowanie do wszystkich nowo utworzonych maszyn wirtualnych. Jeśli jednak właściwość _upgradePolicy_ jest ustawiona na wartość „Automatic”, zmiana zostaje zastosowana tylko do istniejących maszyn wirtualnych. W przypadku ustawionej wartości „Manual” należy natomiast ręcznie zastosować nowy model do istniejących maszyn wirtualnych. W tym celu można skorzystać z portalu i użyć polecenia _Update-AzureRmVmssInstance_ programu PowerShell bądź polecenia _az vmss update-instances_ interfejsu wiersza polecenia.

## <a name="adding-pre-populated-data-disks-to-an-existent-scale-set"></a>Dodawanie wstępnie wypełnionych dysków danych do istniejącego zestawu skalowania 
> Podczas dodawania dysków do istniejącego modelu zestawu skalowania dysk zawsze jest tworzony jako pusty — jest to celowe. Ten scenariusz obejmuje także nowe wystąpienia utworzone przez zestaw skalowania. To zachowanie jest spowodowane obecnością pustego dysku danych w definicji zestawu skalowania. Aby utworzyć wstępnie wypełnione dyski danych dla istniejącego modelu zestawu skalowania, możesz wybrać dowolną z następujących dwóch opcji:

* Skopiowanie danych z maszyny wirtualnej wystąpienia 0 na dyski danych w innych maszynach wirtualnych przy użyciu skryptu niestandardowego.
* Utworzenie obrazu zarządzanego z dyskiem systemu operacyjnego oraz dyskiem danych (z wymaganymi danymi) i utworzenie nowego zestawu skalowania przy użyciu tego obrazu. W ten sposób każda nowa utworzona maszyna wirtualna ma dysk danych podany w definicji zestawu skalowania. Ponieważ ta definicja odwołuje się do obrazu z dyskiem danych zawierającym dostosowane dane, te zmiany są uwzględniane na każdej maszynie wirtualnej w zestawie skalowania.

> Sposób tworzenia obrazu niestandardowego można znaleźć tutaj: [Tworzenie obrazu zarządzanego uogólnionej maszyny wirtualnej na platformie Azure](/azure/virtual-machines/windows/capture-image-resource/) 

> Użytkownik musi przechwycić maszynę wirtualną wystąpienia 0 zawierającą wymagane dane, a następnie utworzyć definicję obrazu przy użyciu tego wirtualnego dysku twardego.

## <a name="removing-a-data-disk-from-a-scale-set"></a>Usuwanie dysku danych z zestawu skalowania
Dysk danych można usunąć z zestawu skalowania maszyn wirtualnych przy użyciu polecenia _az vmss disk detach_ interfejsu wiersza polecenia platformy Azure. Na przykład następujące polecenie usuwa dysk zdefiniowany z numerem LUN 2:
```bash
az vmss disk detach -g dsktest -n dskvmss --lun 2
```  
Podobnie można również usunąć dysk z zestawu skalowania, usuwając wpis z właściwości _dataDisks_ w obszarze _storageProfile_ i stosując tę zmianę. 

## <a name="additional-notes"></a>Uwagi dodatkowe
Obsługa usługi Azure Managed Disks i dołączonych dysków danych w zestawach skalowania jest dostępna w wersji [_2016-04-30-preview_](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) interfejsu API Microsoft.Compute i w nowszych wersjach.

We wstępnej implementacji obsługi dysków dołączonych dla zestawów skalowania nie można dołączać ani odłączać dysków danych w przypadku pojedynczych maszyn wirtualnych należących do zestawu skalowania.

W witrynie Azure Portal obsługa dołączonych dysków danych w zestawach skalowania jest wstępnie ograniczona. W zależności od wymagań można zarządzać dyskami dołączonymi przy użyciu szablonów platformy Azure, interfejsu wiersza polecenia, programu PowerShell, zestawów SDK oraz interfejsu API REST.


