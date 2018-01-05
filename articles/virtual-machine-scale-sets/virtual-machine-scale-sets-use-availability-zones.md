---
title: "Tworzenie zestawu Azure skali, który używa strefy dostępności (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć zestawy skalowania maszyny wirtualnej platformy Azure, korzystających z dostępności strefy w celu zwiększenia nadmiarowości na awarie"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: iainfou
ms.openlocfilehash: 310fdc68d3eb662906053d2bc0c45e6cfa18d4da
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/04/2018
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones-preview"></a>Tworzenie zestawu skali maszyny wirtualnej, który używa strefy dostępności (wersja zapoznawcza)
Aby chronić Twoje zestawy skalowania maszyny wirtualnej z centrum danych na poziomie awarii, można utworzyć skali w strefie dostępności. Regiony platformy Azure obsługujące stref dostępności ma co najmniej trzech oddzielnych stref, każde z nich własne niezależne od zasilania źródła, sieci i chłodzenia. Aby uzyskać więcej informacji, zobacz [Przegląd stref dostępności](../availability-zones/az-overview.md).

Do korzystania ze stref dostępności, należy utworzyć w zestawie skali [obsługiwany region platformy Azure](../availability-zones/az-overview.md#regions-that-support-availability-zones). Można utworzyć zestaw skalowania, który używa strefy dostępności z jednego z następujących metod:

- [Azure portal](#use-the-azure-portal)
- [Interfejs wiersza polecenia platformy Azure 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Szablony usługi Azure Resource Manager](#use-azure-resource-manager-templates)


## <a name="use-the-azure-portal"></a>Korzystanie z witryny Azure Portal
Proces tworzenia zestawu skali, który używa strefy dostępności jest taka sama, jak określono w [wprowadzenie artykułu](virtual-machine-scale-sets-create-portal.md). Po wybraniu obsługiwanych region platformy Azure można utworzyć skali w jednej ze stref dostępne, jak pokazano w poniższym przykładzie:

![Utwórz skali w jednej strefie dostępności](media/virtual-machine-scale-sets-use-availability-zones/create-portal-single-az.png)

Zestaw skali i pomocnicze zasoby, takie jak usługi równoważenia obciążenia Azure i publiczny adres IP, są tworzone w jednej strefie, który określisz.


## <a name="use-the-azure-cli-20"></a>Użyj Azure CLI 2.0
Proces tworzenia zestawu skali, który używa strefy dostępności jest taka sama, jak określono w [wprowadzenie artykułu](virtual-machine-scale-sets-create-cli.md). Do korzystania ze stref dostępność, należy utworzyć na skalę w obsługiwanym regionie Azure. Dodaj `--zones` parametr [utworzyć az vmss](/cli/azure/vmss#az_vmss_create) polecenia i określić, która strefa do użycia (takich jak strefy *1*, *2*, lub *3*). Poniższy przykład tworzy zestaw o nazwie skalowania *myScaleSet* w strefie *1*:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --zones 1
```

Trwa kilka minut, aby utworzyć i skonfigurować wszystkie zestawu skalowania zasobów i maszyn wirtualnych w strefie, który określisz.


## <a name="use-azure-powershell"></a>Korzystanie z programu Azure PowerShell
Proces tworzenia zestawu skali, który używa strefy dostępności jest taka sama, jak określono w [wprowadzenie artykułu](virtual-machine-scale-sets-create-powershell.md). Do korzystania ze stref dostępność, należy utworzyć na skalę w obsługiwanym regionie Azure. Dodaj `-Zone` parametr [AzureRmVmssConfig nowy](/powershell/module/azurerm.compute/new-azurermvmssconfig) polecenia i określić, która strefa do użycia (takich jak strefy *1*, *2*, lub *3*). Poniższy przykład tworzy config zestaw skali, o nazwie *vmssConfig* w *wschodnie stany USA 2* strefy *1*:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Zone "1"
```

Aby utworzyć zestaw skalowania rzeczywiste, wykonaj dodatkowe kroki zgodnie z opisem w [wprowadzenie artykułu](virtual-machine-scale-sets-create-powershell.md).


## <a name="use-azure-resource-manager-templates"></a>Korzystanie z szablonów usługi Azure Resource Manager
Proces tworzenia zestawu skali, który używa strefy dostępności jest taka sama, zgodnie z opisem w artykule Rozpoczęto pobieranie dla [Linux](virtual-machine-scale-sets-create-template-linux.md) lub [Windows](virtual-machine-scale-sets-create-template-windows.md). Do korzystania ze stref dostępność, należy utworzyć na skalę w obsługiwanym regionie Azure. Dodaj `zones` właściwości *Microsoft.Compute/virtualMachineScaleSets* zasobów typu w szablonie i określić, która strefa do użycia (takich jak strefy *1*, *2*, lub *3*). Poniższy przykład tworzy zestaw o nazwie skalowania Linux *myScaleSet* w *wschodnie stany USA 2* strefy *1*:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "zones": ["1"],
  "sku": {
    "name": "Standard_A1",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
      "storageProfile": {
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage"
        },
        "imageReference":  {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "16.04-LTS",
          "version": "latest"
        }
      },
      "osProfile": {
        "computerNamePrefix": "myvmss",
        "adminUsername": "azureuser",
        "adminPassword": "P@ssw0rd!"
      }
    }
  }
}
```

Aby utworzyć zestaw skalowania rzeczywiste, wykonaj dodatkowe kroki zgodnie z opisem w artykule Rozpoczęto pobieranie dla [Linux](virtual-machine-scale-sets-create-template-linux.md) lub [systemu Windows](virtual-machine-scale-sets-create-template-windows.md)


## <a name="next-steps"></a>Kolejne kroki
Teraz, po utworzeniu skali w strefie dostępności, możesz dowiedzieć się jak [Wdrażaj aplikacje na maszynie wirtualnej skalowanie zestawów](virtual-machine-scale-sets-deploy-app.md) lub [automatycznego skalowania za pomocą zestawów skali maszyny wirtualnej](virtual-machine-scale-sets-autoscale-overview.md).
