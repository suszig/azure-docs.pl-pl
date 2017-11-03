---
title: Tworzenie migawki wirtualnego dysku twardego na platformie Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak utworzyć kopię dysku VHD na platformie Azure jako kopii zapasowej lub podczas rozwiązywania problemów."
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/09/2017
ms.author: cynthn
ms.openlocfilehash: da00c48f7da5a9be146f4fdb626c93db746c0f9b
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/14/2017
---
# <a name="create-a-snapshot"></a>Utwórz migawkę 

Utwórz migawkę dysku systemu operacyjnego lub danych, który wystawia wirtualnego dysku twardego do utworzenia kopii zapasowej lub rozwiązywania problemów z maszyny Wirtualnej. Migawka jest pełna, tylko do odczytu kopię dysku VHD. 

## <a name="use-azure-cli-20-to-take-a-snapshot"></a>Umożliwia utworzenie migawki 2.0 interfejsu wiersza polecenia platformy Azure

Poniższy przykład wymaga Azure CLI 2.0 zainstalowany, a zalogowany do konta platformy Azure. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

Poniższe kroki przedstawiają sposób wykonania migawki za pomocą `az snapshot create` z `--source-disk` parametru. W poniższym przykładzie założono, że istnieje maszyna wirtualna o nazwie `myVM` utworzone za pomocą zarządzanego dysku systemu operacyjnego w `myResourceGroup` grupy zasobów.

```azure-cli
# take the disk id with which to create a snapshot
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create -g myResourceGroup --source "$osDiskId" --name osDisk-backup
```

Dane wyjściowe powinny wyglądać podobnie jak:

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Copy",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/disks/osdisk_6NexYgkFQU",
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/osDisk-backup",
  "location": "westus",
  "name": "osDisk-backup",
  "osType": "Linux",
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-06T21:27:10.172980+00:00",
  "type": "Microsoft.Compute/snapshots"
}
```

## <a name="use-azure-portal-to-take-a-snapshot"></a>Utworzenie migawki za pomocą portalu Azure 

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Począwszy od lewej górnej kliknij **nowy** i wyszukaj **migawki**.
3. W bloku migawki kliknij **Utwórz**.
4. Wprowadź **nazwa** dla migawki.
5. Wybierz istniejącą [grupę zasobów](../../azure-resource-manager/resource-group-overview.md#resource-groups) lub wprowadź nazwę nowej grupy zasobów. 
6. Wybierz centrum danych Azure lokalizacji.  
7. Dla **dysku źródłowego**, wybierz zarządzany dysk do migawki.
8. Wybierz **typ konta** do przechowywania migawki. Firma Microsoft zaleca **Standard_LRS** chyba że ma być przechowywane na dysku wysokiej wydajności.
9. Kliknij przycisk **Utwórz**.

Jeśli planujesz użyć migawki, aby utworzyć dysk zarządzany i dołączyć maszynę Wirtualną, która musi być wydajnych, użyj parametru `--sku Premium_LRS` z `az snapshot create` polecenia. Spowoduje to utworzenie migawki tak, aby była przechowywana jako dysk zarządzane Premium. Dysków zarządzanych w warstwie Premium działać lepiej, ponieważ są dysków półprzewodnikowych (SSD), ale są droższe niż dyski standardowe (HDD).


## <a name="next-steps"></a>Następne kroki

 Utwórz maszynę wirtualną z migawki, tworząc dysków zarządzanych z migawki, a następnie Trwa dołączanie nowego dysku zarządzanego jako dysk systemu operacyjnego. Aby uzyskać więcej informacji, zobacz [tworzenie maszyny Wirtualnej z migawki](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) skryptu.

