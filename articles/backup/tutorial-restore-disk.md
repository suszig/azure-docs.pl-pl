---
title: "Przywracanie dysku maszyny wirtualnej przy użyciu usługi Azure Backup | Microsoft Docs"
description: "Dowiedz się, jak przywrócić dysk i utworzyć odzyskaną maszynę wirtualną na platformie Azure przy użyciu usługi Backup i Recovery Services."
services: backup
documentationcenter: virtual-machines
author: markgalioto
manager: carmonm
editor: 
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: 
ms.service: backup
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 2/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 571d40c46771f43ad5ea78fe92398de09e87393c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="restore-a-disk-and-create-a-recovered-vm-in-azure"></a>Przywracanie dysku i tworzenie odzyskanej maszyny wirtualnej na platformie Azure
Usługa Azure Backup tworzy punkty odzyskiwania przechowywane w geograficznie nadmiarowych magazynach odzyskiwania. Z punktu odzyskiwania można przywrócić całą maszynę wirtualną lub poszczególne pliki. W tym artykule opisano sposób przywracania całej maszyny wirtualnej. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wyświetlanie listy i wybieranie punktów odzyskiwania
> * Przywracanie dysku z punktu odzyskiwania
> * Tworzenie maszyny wirtualnej na podstawie przywróconego dysku

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten samouczek będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.18 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 


## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania tego samouczka jest wymagana maszyna wirtualna z systemem Linux, chroniona przy użyciu usługi Azure Backup. Aby zasymulować proces przypadkowego usunięcia maszyny wirtualnej i jej odzyskania, utworzysz maszynę wirtualną na podstawie dysku w punkcie odzyskiwania. Jeśli potrzebujesz maszyny wirtualnej z systemem Linux chronionej przy użyciu usługi Azure Backup, zobacz [Tworzenie kopii zapasowej maszyny wirtualnej na platformie Azure za pomocą interfejsu wiersza polecenia](quick-backup-vm-cli.md).


## <a name="backup-overview"></a>Omówienie usługi Backup
Po zainicjowaniu tworzenia kopii zapasowej przez platformę Azure rozszerzenie kopii zapasowej na maszynie wirtualnej tworzy migawkę punktu w czasie. Rozszerzenie kopii zapasowej jest instalowane na maszynie wirtualnej w momencie pierwszego żądania utworzenia kopii zapasowej. Usługa Azure Backup również może utworzyć migawkę bazowego magazynu, jeśli maszyna wirtualna nie jest uruchomiona w momencie tworzenia kopii zapasowej.

Domyślnie usługa Azure Backup tworzy kopię zapasową spójną na poziomie systemu plików. Po utworzeniu migawki w usłudze Azure Backup dane są przesyłane do magazynu usługi Recovery Services. Aby zmaksymalizować wydajność, usługa Azure Backup rozpoznaje i przesyła jedynie te bloki danych, które uległy zmianie od czasu utworzenia poprzedniej kopii zapasowej.

Po ukończeniu przesyłania danych migawka jest usuwana, a utworzony zostaje punkt odzyskiwania.


## <a name="list-available-recovery-points"></a>Wyświetlanie listy dostępnych punktów odzyskiwania
Aby przywrócić dysk, należy wybrać punkt odzyskiwania będący źródłem danych do odzyskania. Domyślne zasady przewidują tworzenie punktów odzyskiwania codziennie i przechowywanie ich przez 30 dni, dzięki czemu możesz wybrać określony punkt w czasie do odzyskania z dostępnego zestawu punktów odzyskiwania. 

Aby wyświetlić listę dostępnych punktów odzyskiwania, użyj polecenia [az backup recoverypoint list](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az_backup_recoverypoint_list). Do odzyskiwania dysków używana jest **nazwa** punktu odzyskiwania. W tym samouczku użyjemy najnowszego dostępnego punktu odzyskiwania. Parametr `--query [0].name` wybiera nazwę najnowszego punktu odzyskiwania, jak pokazano poniżej:

```azurecli-interactive
az backup recoverypoint list \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --container-name myVM \
    --item-name myVM \
    --query [0].name \
    --output tsv
```


## <a name="restore-a-vm-disk"></a>Przywracanie dysku maszyny wirtualnej
Aby przywrócić dysk z punktu odzyskiwania, należy najpierw utworzyć konto usługi Azure Storage. Jest to konto magazynu używane do przechowywania przywróconego dysku. Dodatkowe czynności obejmują tworzenie maszyny wirtualnej przy użyciu przywróconego dysku.

1. Aby utworzyć konto magazynu, użyj polecenia [az storage account create](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create). Nazwa konta magazynu może zawierać tylko małe litery i musi być globalnie unikatowa. Zastąp ciąg *mystorageaccount* własną unikatową nazwą:

    ```azurecli-interactive
    az storage account create \
        --resource-group myResourceGroup \
        --name mystorageaccount \
        --sku Standard_LRS
    ```

2. Przywróć dysk z punktu odzyskiwania, uruchamiając polecenie [az backup restore restore-disks](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az_backup_restore_restore_disks). Zastąp ciąg *mystorageaccount* nazwą konta magazynu utworzonego przy użyciu poprzedniego polecenia. Zastąp ciąg *myRecoveryPointName* nazwą punktu odzyskiwania uzyskaną wcześniej przy użyciu polecenia [az backup recoverypoint list](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az_backup_recoverypoint_list):

    ```azurecli-interactive
    az backup restore restore-disks \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --storage-account mystorageaccount \
        --rp-name myRecoveryPointName
    ```


## <a name="monitor-the-restore-job"></a>Monitorowanie zadania przywracania
Aby monitorować stan zadania przywracania, użyj polecenia [az backup job list](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az_backup_job_list):

```azurecli-interactive 
az backup job list \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --output table
```

Dane wyjściowe są podobne do następującego przykładu informującego, że zadanie przywracania jest w toku (*InProgress*):

```
Name      Operation        Status      Item Name    Start Time UTC       Duration
--------  ---------------  ----------  -----------  -------------------  --------------
7f2ad916  Restore          InProgress  myvm         2017-09-19T19:39:52  0:00:34.520850
a0a8e5e6  Backup           Completed   myvm         2017-09-19T03:09:21  0:15:26.155212
fe5d0414  ConfigureBackup  Completed   myvm         2017-09-19T03:03:57  0:00:31.191807
```

Gdy w polu *Status* (Stan) zadania przywracania jest widoczna wartość *Completed* (Ukończono), oznacza to, że dysk został przywrócony na konto magazynu.


## <a name="convert-the-restored-disk-to-a-managed-disk"></a>Konwertowanie przywróconego dysku na dysk zarządzany
Zadanie przywracania tworzy dysk niezarządzany. Aby utworzyć maszynę wirtualną na podstawie tego dysku, należy najpierw przekonwertować go na dysk zarządzany.

1. Aby uzyskać parametry połączenia dla konta magazynu, użyj polecenia [az storage account show-connection-string](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_show_connection_string). Zastąp wartość *mystorageaccount* nazwą konta magazynu, jak pokazano poniżej:
    
    ```azurecli-interactive
    export AZURE_STORAGE_CONNECTION_STRING=$( az storage account show-connection-string \
        --resource-group myResourceGroup \
        --output tsv \
        --name mystorageaccount )
    ```

2. Dysk niezarządzany jest zabezpieczony na koncie magazynu. Poniższe polecenia umożliwiają uzyskanie informacji na temat dysku niezarządzanego i utworzenie zmiennej o nazwie *uri*, używanej w następnym kroku do utworzenia dysku zarządzanego.

    ```azurecli-interactive
    container=$(az storage container list --query [0].name -o tsv)
    blob=$(az storage blob list --container-name $container --query [0].name -o tsv)
    uri=$(az storage blob url --container-name $container --name $blob -o tsv)
    ```

3. Możesz teraz utworzyć dysk zarządzany na podstawie odzyskanego dysku przy użyciu polecenia [az disk create](https://docs.microsoft.com/cli/azure/disk?view=azure-cli-latest#az_disk_create). Zmienna *uri* utworzona w poprzednim kroku jest używana jako źródło dla dysku zarządzanego.

    ```azurecli-interactive
    az disk create \
        --resource-group myResourceGroup \
        --name myRestoredDisk \
        --source $uri
    ```

4. Po utworzeniu dysku zarządzanego na podstawie przywróconego dysku możesz wyczyścić dysk niezarządzany i konto magazynu, używając polecenia [az storage account delete](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_delete). Zastąp wartość *mystorageaccount* nazwą konta magazynu, jak pokazano poniżej:

    ```azurecli-interactive
    az storage account delete \
        --resource-group myResourceGroup \
        --name mystorageaccount
    ```


## <a name="create-a-vm-from-the-restored-disk"></a>Tworzenie maszyny wirtualnej na podstawie przywróconego dysku
Ostatnim krokiem jest utworzenie maszyny wirtualnej przy użyciu dysku zarządzanego.

1. Aby utworzyć maszynę wirtualną przy użyciu dysku zarządzanego, użyj polecenia [az vm create](/cli/azure/vm?view=azure-cli-latest#az_vm_create), jak pokazano poniżej:

    ```azurecli-interactive
    az vm create \
        --resource-group myResourceGroup \
        --name myRestoredVM \
        --attach-os-disk myRestoredDisk \
        --os-type linux
    ```

2. Aby potwierdzić utworzenie maszyny wirtualnej na podstawie odzyskanego dysku, wyświetl listę maszyn wirtualnych w grupie zasobów przy użyciu polecenia [az vm list](/cli/azure/vm?view=azure-cli-latest#az_vm_list), jak pokazano poniżej:

    ```azurecli-interactive
    az vm list --resource-group myResourceGroup --output table
    ```


## <a name="next-steps"></a>Następne kroki
Podczas pracy z tym samouczkiem przywrócono dysk z punktu odzyskiwania i utworzono na jego podstawie maszynę wirtualną. W tym samouczku omówiono:

> [!div class="checklist"]
> * Wyświetlanie listy i wybieranie punktów odzyskiwania
> * Przywracanie dysku z punktu odzyskiwania
> * Tworzenie maszyny wirtualnej na podstawie przywróconego dysku

Przejdź do następnego samouczka, aby dowiedzieć się więcej o przywracaniu poszczególnych plików z punktu odzyskiwania.

> [!div class="nextstepaction"]
> [Przywracanie plików na maszynę wirtualną na platformie Azure](tutorial-restore-files.md)

