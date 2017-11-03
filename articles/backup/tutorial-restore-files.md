---
title: "Przywracanie plików do maszyny Wirtualnej w usłudze Azure Backup | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak przeprowadzić przywracania na poziomie pliku na maszynie Wirtualnej platformy Azure z kopii zapasowej i odzyskiwania usługi."
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
ms.date: 09/29/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: e1bbae56b70c50fcf691db47efd9dc587686e7da
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="restore-files-to-a-virtual-machine-in-azure"></a>Przywracanie plików na maszynę wirtualną na platformie Azure
Kopia zapasowa Azure tworzy punkty odzyskiwania, które są przechowywane w magazynach odzyskiwania z magazynu geograficznie nadmiarowego. Po przywróceniu z punktu odzyskiwania, można przywrócić całej maszyny Wirtualnej lub poszczególnych plików. W tym artykule szczegółowo sposób przywracania poszczególnych plików. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Listy i punktów odzyskiwania wybierz
> * Połączenie z maszyną wirtualną punkt odzyskiwania
> * Przywróć pliki z punktu odzyskiwania

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.18 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli). 


## <a name="prerequisites"></a>Wymagania wstępne
Ten samouczek wymaga systemu Linux maszyny Wirtualnej chronionej przy użyciu kopii zapasowej Azure. Aby symulować pliku przypadkowego usunięcia i proces odzyskiwania, w przypadku usunięcia strony z serwera sieci web. Jeśli potrzebujesz Maszynę wirtualną systemu Linux uruchamia serwer sieci Web, który został objęty ochroną z usługi Kopia zapasowa Azure, zobacz [kopię zapasową maszyny wirtualnej na platformie Azure z poziomu interfejsu wiersza polecenia](quick-backup-vm-cli.md).


## <a name="backup-overview"></a>Omówienie usługi Backup
Gdy Azure inicjuje kopii zapasowej, kopii zapasowej rozszerzenia na maszynie Wirtualnej, wykonuje migawkę punktu w czasie. Zapasowy numer wewnętrzny jest zainstalowany na maszynie Wirtualnej zleconą pierwszej kopii zapasowej. Kopia zapasowa Azure można również wykonać migawki powiązany magazyn, jeśli maszyna wirtualna nie jest uruchomiona, podczas tworzenia kopii zapasowej.

Domyślnie kopia zapasowa Azure ma kopia zapasowa spójna systemu plików. Gdy kopia zapasowa Azure przyjmuje migawki, dane są przesyłane do magazynu usług odzyskiwania. Aby zmaksymalizować wydajność, kopia zapasowa Azure identyfikuje i transferuje tylko bloki danych, które uległy zmianie od czasu poprzedniej kopii zapasowej.

Po ukończeniu transferu danych migawki zostaną usunięte i utworzenia punktu odzyskiwania.


## <a name="delete-a-file-from-a-vm"></a>Usuń plik z maszyny Wirtualnej
Jeśli przypadkowo usunięte lub wprowadzić zmiany w pliku, można przywrócić pojedyncze pliki z punktu odzyskiwania. Ten proces umożliwia przeglądanie plików z kopii zapasowej w punkt odzyskiwania i przywrócić tylko te pliki, które są potrzebne. W tym przykładzie mamy usuwanie pliku z serwera sieci web, aby zademonstrować proces odzyskiwania na poziomie pliku.

1. Aby nawiązać połączenie z maszyną Wirtualną, Uzyskaj adres IP maszyny wirtualnej z [az maszyny wirtualnej pokazu](/cli/azure/vm?view=azure-cli-latest#az_vm_show):

     ```azurecli-interactive
     az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
     ```

2. Aby upewnić się, że witryny sieci web jest obecnie obsługiwane, otwórz przeglądarkę sieci web do publicznego adresu IP maszyny Wirtualnej. Pozostaw otwarte okno przeglądarki sieci web.

    ![Domyślna strona sieci web NGINX](./media/tutorial-restore-files/nginx-working.png)

3. Połączenie z maszyną Wirtualną przy użyciu protokołu SSH. Zastąp *publicznego adresu IP* z publicznym adresem IP, które zostały uzyskane w poprzednim poleceniu:

    ```bash
    ssh publicIpAddress
    ```

4. Usuń z serwera sieci web w domyślnej strony */var/www/html/index.nginx-debian.html* w następujący sposób:

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```

5. W przeglądarce sieci web Odśwież stronę sieci web. Witryna sieci web nie jest już ładuje strony, jak pokazano w poniższym przykładzie:

    ![Witryna sieci web NGINX ładuje już domyślną stronę](./media/tutorial-restore-files/nginx-broken.png)

6. Zamknij sesję SSH do maszyny Wirtualnej w następujący sposób:

    ```bash
    exit
    ```


## <a name="generate-file-recovery-script"></a>Generowanie pliku skryptu odzyskiwania
Aby przywrócić pliki, kopia zapasowa Azure udostępnia skrypt uruchamiany na maszynie Wirtualnej łączący ten punkt odzyskiwania, jako dysk lokalny. Można przeglądać tego dysku lokalnego, Przywracanie plików do samej maszyny Wirtualnej, a następnie odłącz punktu odzyskiwania. Kopia zapasowa Azure w dalszym ciągu kopii zapasowej danych na podstawie zasad przypisany harmonogram i przechowywania.

1. Do listy punktów odzyskiwania dla maszyny Wirtualnej, użyj [az recoverypoint kopii zapasowej listy](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az_backup_recoverypoint_list). W tym przykładzie mamy wybierz najnowszy punkt odzyskiwania dla maszyny Wirtualnej o nazwie *myVM* która jest chroniona w *myRecoveryServicesVault*:

    ```azurecli-interactive
    az backup recoverypoint list \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --query [0].name \
        --output tsv
    ```

2. Aby uzyskać skrypt, który łączy lub instaluje punkt odzyskiwania do maszyny Wirtualnej, użyj [az przywracania kopii zapasowej plików instalacji rp](https://docs.microsoft.com/cli/azure/backup/restore/files?view=azure-cli-latest#az_backup_restore_files_mount_rp). Poniższy przykład uzyskuje skryptu dla maszyny Wirtualnej o nazwie *myVM* która jest chroniona w *myRecoveryServicesVault*.

    Zastąp *myRecoveryPointName* o nazwie punkt odzyskiwania, które zostały uzyskane w poprzednim poleceniu:

    ```azurecli-interactive
    az backup restore files mount-rp \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --rp-name myRecoveryPointName
    ```

    Skrypt jest pobierany i hasło jest wyświetlane, jak w poniższym przykładzie:

    ```
    File downloaded: myVM_we_1571974050985163527.sh. Use password c068a041ce12465
    ```

3. Aby przesłać skrypt do maszyny Wirtualnej, użyj Secure kopiowania (SCP). Podaj nazwę skryptu pobrane i Zastąp *publicznego adresu IP* z publicznym adresem IP w sieci maszyny wirtualnej. Upewnij się, że możesz wpisać kreskę końcową `:` na końcu punkt połączenia usługi polecenia w następujący sposób:

    ```bash
    scp myVM_we_1571974050985163527.sh 52.174.241.110:
    ```


## <a name="restore-file-to-your-vm"></a>Przywracanie plików do maszyny Wirtualnej
Przy użyciu skryptu odzyskiwania kopiowane do maszyny Wirtualnej można połączyć z punktu odzyskiwania i przywracania plików.

1. Połączenie z maszyną Wirtualną przy użyciu protokołu SSH. Zastąp *publicznego adresu IP* z publicznym adresem IP w sieci maszyny wirtualnej w następujący sposób:

    ```bash
    ssh publicIpAddress
    ```

2. Aby umożliwić skrypt, aby działała poprawnie, należy dodać uprawnienia z wykonywania **chmod**. Wprowadź nazwę własnego skryptu:

    ```bash
    chmod +x myVM_we_1571974050985163527.sh
    ```

3. Aby zainstalować punkt odzyskiwania, uruchom skrypt. Wprowadź nazwę własnego skryptu:

    ```bash
    ./myVM_we_1571974050985163527.sh
    ```

    Jak skrypt jest uruchamiany, wyświetlany jest monit o wprowadzenie hasła w celu uzyskania dostępu do punktu odzyskiwania. Wprowadź hasło wyświetlany w danych wyjściowych z poprzedniej [az przywracania kopii zapasowej plików instalacji rp](https://docs.microsoft.com/cli/azure/backup/restore/files?view=azure-cli-latest#az_backup_restore_files_mount_rp) polecenia, który wygenerował skryptu odzyskiwania.

    Dane wyjściowe skryptu zawiera ścieżkę dla punktu odzyskiwania. Następujące przykładowe dane wyjściowe pokazuje, że punkt odzyskiwania został zainstalowany w */home/azureuser/myVM-20170919213536/Volume1*:

    ```
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    Please enter the password as shown on the portal to securely connect to the recovery point. : c068a041ce12465
    
    Connecting to recovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of the recovery point to this machine...
    
    ************ Volumes of the recovery point and their mount paths on this machine ************
    
    Sr.No.  |  Disk  |  Volume  |  MountPath
    
    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170919213536/Volume1
    
    ************ Open File Explorer to browse for files. ************
    ```

4. Użyj **cp** skopiuj NGINX domyślnej strony sieci web z punktu odzyskiwania zainstalowanego z powrotem do oryginalnej lokalizacji pliku. Zastąp */home/azureuser/myVM-20170919213536/Volume1* punktu z własnej lokalizacji instalacji:

    ```bash
    sudo cp /home/azureuser/myVM-20170919213536/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```

6. W przeglądarce sieci web Odśwież stronę sieci web. Witryna sieci web teraz ładuje poprawnie ponownie, jak pokazano w poniższym przykładzie:

    ![Witryna sieci web NGINX teraz ładuje poprawnie](./media/tutorial-restore-files/nginx-restored.png)

7. Zamknij sesję SSH do maszyny Wirtualnej w następujący sposób:

    ```bash
    exit
    ```

8. Odinstaluj punkt odzyskiwania z maszyny Wirtualnej z [az przywracania kopii zapasowej plików odinstalować rp](https://docs.microsoft.com/cli/azure/backup/restore/files?view=azure-cli-latest#az_backup_restore_files_unmount_rp). Poniższy przykład odinstalowuje punkt odzyskiwania z maszyny Wirtualnej o nazwie *myVM* w *myRecoveryServicesVault*.

    Zastąp *myRecoveryPointName* o nazwie ten punkt odzyskiwania, uzyskany w powyższych poleceń:
    
    ```azurecli-interactive
    az backup restore files unmount-rp \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --rp-name myRecoveryPointName
    ```

## <a name="next-steps"></a>Następne kroki
W tym samouczku połączony z maszyną Wirtualną punkt odzyskiwania i przywróconych plików dla serwera sieci web. W tym samouczku omówiono:

> [!div class="checklist"]
> * Listy i punktów odzyskiwania wybierz
> * Połączenie z maszyną wirtualną punkt odzyskiwania
> * Przywróć pliki z punktu odzyskiwania

Przejście do następnym samouczku, aby dowiedzieć się więcej o tym, jak wykonać kopię zapasową serwera systemu Windows Azure.

> [!div class="nextstepaction"]
> [Utwórz kopię zapasową serwera systemu Windows Azure](tutorial-backup-windows-server-to-azure.md)

