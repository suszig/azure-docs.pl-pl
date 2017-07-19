---
title: "Używanie dysków zarządzanych z zestawami skalowania maszyn wirtualnych platformy Azure | Microsoft Docs"
description: "Informacje o powodach i sposobach używania dysków zarządzanych z zestawami skalowania maszyn wirtualnych"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/01/2017
ms.author: negat
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: f9cc157f25e70a8154614b659fb7e59b7fd06ed1
ms.contentlocale: pl-pl
ms.lasthandoff: 06/01/2017


---
# <a name="azure-vm-scale-sets-and-managed-disks"></a>Zestawy skalowania maszyn wirtualnych platformy Azure i dyski zarządzane

[Zestawy skalowania maszyn wirtualnych](/azure/virtual-machine-scale-sets/) platformy Azure obsługują teraz maszyny wirtualne z dyskami zarządzanymi. Używanie dysków zarządzanych z zestawami skalowania ma między innymi następujące zalety:

* Nie trzeba już wstępnie tworzyć kont magazynów do przechowywania dysków systemów operacyjnych dla maszyn wirtualnych zestawów skalowania ani zarządzać tymi dyskami.

* Do zestawu skalowania można dołączać zarządzane dyski danych.

* W przypadku dysków zarządzanych zestaw skalowania może zawierać do 1000 maszyn wirtualnych opartych na obrazach platformy lub do 100 maszyn wirtualnych opartych na obrazach niestandardowych.

## <a name="get-started"></a>Rozpoczęcie pracy

Prostym sposobem rozpoczęcia pracy z zestawami skalowania dysków zarządzanych jest wdrożenie jednego z nich z witryny Azure Portal. Więcej informacji znajduje się w [tym artykule](./virtual-machine-scale-sets-portal-create.md). Innym prostym sposobem jest wdrożenie zestawu skalowania za pomocą interfejsu [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2). W poniższym przykładzie pokazano, jak utworzyć zestaw skalowania oparty na systemie Ubuntu z 10 maszynami wirtualnymi, z których każda ma dyski danych o pojemności 50 GB i 100 GB:

```azurecli
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

Ewentualnie można poszukać w [repozytorium szablonów z pakietu Azure Quickstart w witrynie GitHub](https://github.com/Azure/azure-quickstart-templates) folderów zawierających element `vmss` w celu wyświetlenia wstępnie utworzonych przykładowych szablonów do wdrażania zestawów skalowania. Aby ustalić, w których szablonach są już używane dyski zarządzane, można skorzystać z [tej listy](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).

## <a name="api-versions"></a>Wersje interfejsu API

Zestawy skalowania z dyskami zarządzanymi wymagają interfejsu API Microsoft.Compute w wersji `2016-04-30-preview` lub nowszej. Zestawy skalowania z dyskami niezarządzanymi będą nadal działać jak obecnie, nawet w nowych wersjach interfejsu API, które obsługują dyski zarządzane. Jednak zestawy skalowania z dyskami niezarządzanymi nie udostępniają korzyści związanych z dyskami zarządzanymi, nawet w tych nowszych wersjach interfejsu API.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej ogólnych informacji o dyskach zarządzanych, zobacz [ten artykuł](../storage/storage-managed-disks-overview.md).

Aby dowiedzieć się, jak przekonwertować szablon usługi Resource Manager w celu zapewnienia obsługi administracyjnej zestawów skalowania z dyskami zarządzanymi, zobacz [ten artykuł](./virtual-machine-scale-sets-convert-template-to-md.md). Te same modyfikacje szablonów usługi Resource Manager mają też zastosowanie w przypadku interfejsu API REST platformy Azure.

Aby dowiedzieć się więcej o używaniu zestawów skalowania z zarządzanymi dyskami danych, zobacz [ten artykuł](./virtual-machine-scale-sets-attached-disks.md).

Aby rozpocząć pracę z dużymi zestawami skalowania, zobacz [ten artykuł](./virtual-machine-scale-sets-placement-groups.md).



