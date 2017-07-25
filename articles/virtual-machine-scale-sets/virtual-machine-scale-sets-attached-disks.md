---
title: "Zestawy skalowania maszyn wirtualnych platformy Azure — dołączone dyski danych | Microsoft Docs"
description: "Informacje o używaniu dołączonych dysków danych z zestawami skalowania maszyn wirtualnych"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/25/2017
ms.author: guybo
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: 451d3c956b863ab90f86509fd80a5c96e27525ce
ms.contentlocale: pl-pl
ms.lasthandoff: 07/19/2017

---
# <a name="azure-vm-scale-sets-and-attached-data-disks"></a>Zestawy skalowania maszyn wirtualnych platformy Azure i dołączone dyski danych
[Zestawy skalowania maszyn wirtualnych](/azure/virtual-machine-scale-sets/) platformy Azure obsługują teraz maszyny wirtualne z dołączonymi dyskami danych. Dla zestawów skalowania utworzonych przy użyciu usługi Azure Managed Disks można zdefiniować dyski danych w profilu magazynu. Wcześniej jedynymi opcjami bezpośrednio dołączonego magazynu dostępnymi dla maszyn wirtualnych w zestawach skalowania były dysk systemu operacyjnego oraz dyski tymczasowe.

> [!NOTE]
>  Podczas tworzenia zestawu skalowania ze zdefiniowanymi dołączonymi dyskami danych nadal konieczne jest zainstalowanie i sformatowanie tych dysków z poziomu maszyny wirtualnej w celu ich użycia (podobnie jak w przypadku autonomicznych maszyn wirtualnych platformy Azure). Wygodnym sposobem wykonania tych czynności jest użycie rozszerzenia niestandardowego skryptu, które wywołuje skrypt standardowy w celu podzielenia na partycje i sformatowania wszystkich dysków danych na maszynie wirtualnej.

## <a name="create-a-scale-set-with-attached-data-disks"></a>Tworzenie zestawu skalowania z dołączonymi dyskami danych
Prostym sposobem utworzenia zestawu skalowania z dyskami dołączonymi jest użycie polecenia _vmss create_ [interfejsu wiersza polecenia platformy Azure](https://github.com/Azure/azure-cli). W poniższym przykładzie zostaje utworzona grupa zasobów platformy Azure oraz zestaw skalowania maszyn wirtualnych obejmujący 10 maszyn wirtualnych z systemem Ubuntu, z których każda ma 2 dołączone dyski danych o pojemności 50 GB i 100 GB.
```bash
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```
Zwróć uwagę, że polecenie _vmss create_ powoduje ustawienie pewnych domyślnych wartości konfiguracji, jeśli nie zostaną określone. Aby wyświetlić dostępne opcje, które można przesłonić, spróbuj użyć polecenia:
```bash
az vmss create --help
```
Innym sposobem tworzenia zestawu skalowania z dołączonymi dyskami danych jest zdefiniowanie zestawu skalowania w szablonie usługi Azure Resource Manager, uwzględnienie sekcji _dataDisks_ w obszarze _storageProfile_ i wdrożenie szablonu. Powyższe przykładowe dyski o pojemności 50 GB i 100 GB zostałyby w tym szablonie zdefiniowane następująco:
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

## <a name="adding-a-data-disk-to-an-existing-scale-set"></a>Dodawanie dysku danych do istniejącego zestawu skalowania
> [!NOTE]
>  Dyski danych można dołączać tylko do zestawu skalowania, który został utworzony za pomocą usługi [Azure Managed Disks](./virtual-machine-scale-sets-managed-disks.md).

Dysk danych można dodać do zestawu skalowania maszyn wirtualnych przy użyciu polecenia _az vmss disk attach_ interfejsu wiersza polecenia platformy Azure. Należy pamiętać o określeniu właściwości lun, która nie jest jeszcze używana. W poniższym przykładzie z interfejsu wiersza polecenia do właściwości lun 3 zostaje dodany dysk o rozmiarze 50 GB:
```bash
az vmss disk attach -g dsktest -n dskvmss --size-gb 50 --lun 3
```

W poniższym przykładzie przy użyciu programu PowerShell do właściwości lun 3 zostaje dodany dysk o rozmiarze 50 GB:
```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myvmssrg -VMScaleSetName myvmss
$vmss = Add-AzureRmVmssDataDisk -VirtualMachineScaleSet $vmss -Lun 3 -Caching 'ReadWrite' -CreateOption Empty -DiskSizeGB 50 -StorageAccountType StandardLRS
Update-AzureRmVmss -ResourceGroupName myvmssrg -Name myvmss -VirtualMachineScaleSet $vmss
```

> [!NOTE]
> Różne rozmiary maszyn wirtualnych mają różne limity dotyczące obsługiwanej przez nie liczby dysków dołączonych. Przed dodaniem nowego dysku zapoznaj się z [charakterystyką rozmiarów maszyn wirtualnych](../virtual-machines/windows/sizes.md).

Dysk można również dodać przez dodanie nowego wpisu do właściwości _dataDisks_ w obszarze _storageProfile_ definicji zestawu skalowania i przez zastosowanie tej zmiany. Aby to przetestować, odszukaj istniejącą definicję zestawu skalowania w [Eksploratorze zasobów Azure](https://resources.azure.com/). Wybierz pozycję _Edytuj_ i dodaj nowy dysk do listy dysków danych. Na przykład przy użyciu powyższego przykładu:
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



