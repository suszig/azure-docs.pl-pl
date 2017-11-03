---
title: Przywracanie dysku maszyny Wirtualnej z kopii zapasowej Azure | Dokumentacja firmy Microsoft
description: "Informacje o sposobie przywracania dysku i utworzyć odzyskać Maszynę wirtualną na platformie Azure z kopii zapasowej i odzyskiwania usługi."
services: backup, virtual-machines
documentationcenter: virtual-machines
author: markgalioto
manager: carmonm
editor: 
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: 
ms.service: backup, virtual-machines
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/28/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 9bc6da13786eb9eb6186ceadf0432b3a3ec2c941
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="restore-a-disk-and-create-a-recovered-vm-in-azure"></a>Przywróć dysk i Utwórz odzyskanej maszyny Wirtualnej na platformie Azure
Kopia zapasowa Azure tworzy punkty odzyskiwania, które są przechowywane w magazynach odzyskiwania z magazynu geograficznie nadmiarowego. Po przywróceniu z punktu odzyskiwania, można przywrócić całej maszyny Wirtualnej lub poszczególnych plików. W tym artykule opisano sposób przywracania pełną maszyny Wirtualnej. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Listy i punktów odzyskiwania wybierz
> * Przywróć dysk z punktu odzyskiwania
> * Utwórz maszynę Wirtualną z przywróconą dysku

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.18 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 


## <a name="prerequisites"></a>Wymagania wstępne
Ten samouczek wymaga systemu Linux maszyny Wirtualnej chronionej przy użyciu kopii zapasowej Azure. Symulacji przypadkowego usunięcia maszyny Wirtualnej i procesu odzyskiwania należy utworzyć Maszynę wirtualną z dysku w punkcie odzyskiwania. Jeśli potrzebujesz Maszynę wirtualną systemu Linux, który został objęty ochroną z usługi Kopia zapasowa Azure, zobacz [kopię zapasową maszyny wirtualnej na platformie Azure z poziomu interfejsu wiersza polecenia](quick-backup-vm-cli.md).


## <a name="backup-overview"></a>Omówienie usługi Backup
Gdy Azure inicjuje kopii zapasowej, kopii zapasowej rozszerzenia na maszynie Wirtualnej, wykonuje migawkę punktu w czasie. Zapasowy numer wewnętrzny jest zainstalowany na maszynie Wirtualnej zleconą pierwszej kopii zapasowej. Kopia zapasowa Azure można również wykonać migawki powiązany magazyn, jeśli maszyna wirtualna nie jest uruchomiona, podczas tworzenia kopii zapasowej.

Domyślnie kopia zapasowa Azure ma kopia zapasowa spójna systemu plików. Gdy kopia zapasowa Azure przyjmuje migawki, dane są przesyłane do magazynu usług odzyskiwania. Aby zmaksymalizować wydajność, kopia zapasowa Azure identyfikuje i transferuje tylko bloki danych, które uległy zmianie od czasu poprzedniej kopii zapasowej.

Po ukończeniu transferu danych migawki zostaną usunięte i utworzenia punktu odzyskiwania.


## <a name="list-available-recovery-points"></a>Lista dostępnych punktów odzyskiwania
Aby przywrócić dysku, wybierz punkt odzyskiwania jako źródło danych odzyskiwania. Domyślne zasady tworzy punkt odzyskiwania codziennie i przechowuje je przez 30 dni, można zachować zestaw punktów odzyskiwania, który służy do wybierania określonego punktu w czasie odzyskiwania. 

Aby wyświetlić listę dostępnych punktów odzyskiwania, użyj [az recoverypoint kopii zapasowej listy](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az_backup_recoverypoint_list). Punkt odzyskiwania **nazwa** służy do odzyskania dysków. W tym samouczku chcemy najnowszy dostępny punkt odzyskiwania. `--query [0].name` Parametr wybiera nazwę ostatniego punktu odzyskiwania w następujący sposób:

```azurecli-interactive
az backup recoverypoint list \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --container-name myVM \
    --item-name myVM \
    --query [0].name \
    --output tsv
```


## <a name="restore-a-vm-disk"></a>Przywracanie dysku maszyny Wirtualnej
Do przywrócenia dysku z punktu odzyskiwania, należy najpierw utworzyć konto magazynu platformy Azure. To konto magazynu jest używany do przechowywania przywróconej dysku. Dodatkowe kroki przywróconej dysk jest używany do utworzenia maszyny Wirtualnej.

1. Aby utworzyć konto magazynu, należy użyć [Tworzenie konta magazynu az](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create). Nazwa konta magazynu musi zawierać tylko małe litery i musi być globalnie unikatowe. Zastąp *mojekontomagazynu* unikatową nazwą:

    ```azurecli-interactive
    az storage account create \
        --resource-group myResourceGroup \
        --name mystorageaccount \
        --sku Standard_LRS
    ```

2. Przywróć z punktu odzyskiwania z dysku [kopii zapasowej az przywrócenia dysków przywracania](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az_backup_restore_restore_disks). Zastąp *mojekontomagazynu* o nazwie na koncie magazynu utworzonym w poprzednim poleceniu. Zastąp *myRecoveryPointName* o nazwie punkt odzyskiwania został uzyskany w danych wyjściowych z poprzedniej [az recoverypoint kopii zapasowej listy](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az_backup_recoverypoint_list) polecenia:

    ```azurecli-interactive
    az backup restore restore-disks \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --storage-account mystorageaccount \
        --rp-name myRecoveryPointName
    ```


## <a name="monitor-the-restore-job"></a>Monitor zadania przywracania
Aby monitorować stan zadania przywracania, użyj [az zadanie tworzenia kopii zapasowej listy](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az_backup_job_list):

```azurecli-interactive 
az backup job list \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --output table
```

Wynik jest podobny do poniższego przykładu przedstawia zadania przywracania *w toku*:

```
Name      Operation        Status      Item Name    Start Time UTC       Duration
--------  ---------------  ----------  -----------  -------------------  --------------
7f2ad916  Restore          InProgress  myvm         2017-09-19T19:39:52  0:00:34.520850
a0a8e5e6  Backup           Completed   myvm         2017-09-19T03:09:21  0:15:26.155212
fe5d0414  ConfigureBackup  Completed   myvm         2017-09-19T03:03:57  0:00:31.191807
```

Gdy *stan* raportów zadania przywracania *Ukończono*, dysk został przywrócony do konta magazynu.


## <a name="convert-the-restored-disk-to-a-managed-disk"></a>Konwertuj dysk przywrócone na dysk zarządzany
Zadanie przywracania tworzy dysk niezarządzane. Aby utworzyć Maszynę wirtualną z dysku, jego muszą najpierw zostać przekonwertowane na dysk zarządzany.

1. Uzyskaj informacje o połączeniu dla konta magazynu z [Pokaż parametry konta magazynu az](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_show_connection_string). Zastąp *mojekontomagazynu* o nazwie magazynu konta w następujący sposób:
    
    ```azurecli-interactive
    export AZURE_STORAGE_CONNECTION_STRING=$( az storage account show-connection-string \
        --resource-group myResourceGroup \
        --output tsv \
        --name mystorageaccount )
    ```

2. Niezarządzane dysku jest zabezpieczona na koncie magazynu. Poniższe polecenia Pobierz informacje o dysku niezarządzane i utworzyć zmienną o nazwie *uri* używany w następnym kroku podczas tworzenia dysku zarządzanego.

    ```azurecli-interactive
    container=$(az storage container list --query [0].name -o tsv)
    blob=$(az storage blob list --container-name $container --query [0].name -o tsv)
    uri=$(az storage blob url --container-name $container --name $blob -o tsv)
    ```

3. Teraz można utworzyć dysku zarządzanego z dysku odzyskanych z [Tworzenie dysku az](https://docs.microsoft.com/cli/azure/disk?view=azure-cli-latest#az_disk_create). *Uri* zmienna z poprzedniego kroku jest używana jako źródło dla dysku zarządzanego.

    ```azurecli-interactive
    az disk create \
        --resource-group myResourceGroup \
        --name myRestoredDisk \
        --source $uri
    ```

4. Zgodnie z dysku przywróconej teraz znajduje się dysk zarządzane, wyczyść niezarządzane dysku i konto magazynu z [usunąć konto magazynu az](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_delete). Zastąp *mojekontomagazynu* o nazwie magazynu konta w następujący sposób:

    ```azurecli-interactive
    az storage account delete \
        --resource-group myResourceGroup \
        --name mystorageaccount
    ```


## <a name="create-a-vm-from-the-restored-disk"></a>Utwórz maszynę Wirtualną z przywróconą dysku
Ostatnim krokiem jest tworzenie maszyny Wirtualnej z dysku zarządzanego.

1. Tworzenie maszyny Wirtualnej z dysku zarządzanego z [tworzenia maszyny wirtualnej az](/cli/azure/vm?view=azure-cli-latest#az_vm_create) w następujący sposób:

    ```azurecli-interactive
    az vm create \
        --resource-group myResourceGroup \
        --name myRestoredVM \
        --attach-os-disk myRestoredDisk \
        --os-type linux
    ```

2. Aby upewnić się, że maszyna wirtualna została utworzona z dysku odzyskane, lista maszyn wirtualnych w grupie zasobów z [listy wirtualna az](/cli/azure/vm?view=azure-cli-latest#az_vm_list) w następujący sposób:

    ```azurecli-interactive
    az vm list --resource-group myResourceGroup --output table
    ```


## <a name="next-steps"></a>Następne kroki
W tym samouczku przywrócone z punktu odzyskiwania dysku, a następnie utworzyć Maszynę wirtualną z dysku. W tym samouczku omówiono:

> [!div class="checklist"]
> * Listy i punktów odzyskiwania wybierz
> * Przywróć dysk z punktu odzyskiwania
> * Utwórz maszynę Wirtualną z przywróconą dysku

Przejdź do następnego samouczka, aby dowiedzieć się więcej o przywracaniu poszczególnych plików z punktu odzyskiwania.

> [!div class="nextstepaction"]
> [Przywracanie plików na maszynę wirtualną na platformie Azure](tutorial-restore-files.md)

