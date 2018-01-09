---
title: "Wykonaj kopię zapasową maszyn wirtualnych systemu Linux platformy Azure | Dokumentacja firmy Microsoft"
description: "Ochrona maszyn wirtualnych systemu Linux przez tworzenie ich kopii zapasowych za pomocą usługi Kopia zapasowa Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 2eb0958169b175813b0dca775e9250da1cb364d4
ms.sourcegitcommit: 7d4b3cf1fc9883c945a63270d3af1f86e3bfb22a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2018
---
# <a name="back-up-linux--virtual-machines-in-azure"></a>Tworzenie kopii zapasowych maszyn wirtualnych systemu Linux na platformie Azure

Możesz chronić swoje dane, tworząc kopie zapasowe w regularnych odstępach czasu. Kopia zapasowa Azure tworzy punkty odzyskiwania, które są przechowywane w magazynach odzyskiwania z magazynu geograficznie nadmiarowego. Po przywróceniu z punktu odzyskiwania, można przywrócić całej maszyny Wirtualnej lub po prostu określonych plików. W tym artykule opisano sposób przywracania jednego pliku do maszyny Wirtualnej systemu Linux uruchomiony nginx. Jeśli nie masz jeszcze maszynę Wirtualną do obsługi, możesz utworzyć go przy użyciu [szybkiego startu Linux](quick-create-cli.md). Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz kopię zapasową maszyny Wirtualnej
> * Harmonogram tworzenia codziennej kopii zapasowej
> * Przywróć plik z kopii zapasowej



## <a name="backup-overview"></a>Omówienie usługi Backup

Gdy usługa Azure Backup Inicjuje kopii zapasowej, wyzwala zapasowy numer wewnętrzny do tworzenia migawki punktu w czasie. Używa usługi Azure Backup _VMSnapshotLinux_ rozszerzenia w systemie Linux. Rozszerzenie jest zainstalowany podczas pierwszego tworzenia kopii zapasowej maszyny Wirtualnej, jeśli maszyna wirtualna jest uruchomiona. Jeśli maszyna wirtualna nie działa, usługa Kopia zapasowa tworzy migawkę powiązany magazyn (ponieważ nie zapisy aplikacji są wykonywane, gdy maszyna wirtualna zostanie zatrzymana).

Domyślnie kopia zapasowa Azure przyjmuje kopia zapasowa spójna systemu plików dla maszyny Wirtualnej systemu Linux, ale może być skonfigurowana do podjęcia [przy użyciu platformy skryptów przed i po skrypt kopii zapasowej spójnej aplikacji](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent). Gdy usługa Kopia zapasowa Azure przyjmuje migawki, dane są przesyłane do magazynu. Aby zmaksymalizować wydajność, usługa identyfikuje i transferuje tylko bloki danych, które uległy zmianie od czasu poprzedniej kopii zapasowej.

Po ukończeniu transferu danych migawki zostaną usunięte i utworzenia punktu odzyskiwania.


## <a name="create-a-backup"></a>Tworzenie kopii zapasowej
Utwórz prostą, zaplanowaną, codzienną operację tworzenia kopii zapasowych w magazynie usługi Recovery Services. 

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
2. W menu po lewej stronie wybierz pozycję **Maszyny wirtualne**. 
3. Z listy wybierz maszynę wirtualną do utworzenia kopii zapasowej.
4. W bloku maszyny Wirtualnej w **ustawienia** kliknij **kopii zapasowej**. **Kopii zapasowej Włącz** zostanie otwarty blok.
5. W **magazyn usług odzyskiwania i**, kliknij przycisk **Utwórz nowy** i podaj nazwę dla nowego magazynu. Nowy magazyn jest tworzony w tej samej lokalizacji co maszyna wirtualna i grupy zasobów.
6. Kliknij przycisk **kopii zapasowej zasad**. W tym przykładzie należy zachować ustawienia domyślne, a następnie kliknij przycisk **OK**.
7. Na **kopii zapasowej Włącz** bloku, kliknij przycisk **Włącz kopię zapasową**. Spowoduje to utworzenie kopii zapasowej codziennie na podstawie harmonogramu domyślnego.
10. Tworzenie punktu odzyskiwania początkowej na **kopii zapasowej** kliknij bloku **wykonaj kopię zapasową teraz**.
11. Na **Utwórz kopię zapasową teraz** bloku, kliknij ikonę kalendarza, użyj formant kalendarza, aby wybrać ostatni dzień tego punktu odzyskiwania jest zachowywana, a następnie kliknij przycisk **kopii zapasowej**.
12. W **kopii zapasowej** bloku dla maszyny Wirtualnej, zobaczysz liczbę punktów odzyskiwania, które są spełnione.

    ![Punkty odzyskiwania](./media/tutorial-backup-vms/backup-complete.png)

Pierwszej kopii zapasowej trwa około 20 minut. Po zakończeniu tworzenia kopii zapasowej, należy przejść do następnej części tego samouczka.

## <a name="restore-a-file"></a>Przywróć plik

Jeśli przypadkowo usunięte lub wprowadzić zmiany w pliku, możesz użyć odzyskiwania plików, aby odzyskać plik z magazynu kopii zapasowej. Odzyskiwanie plików używa skryptu uruchamianego na maszynie Wirtualnej, należy zainstalować punkt odzyskiwania jako dysk lokalny. Dyski te pozostanie zainstalowanego na 12 godzin, co może kopiować pliki z punktu odzyskiwania i przywrócić je do maszyny Wirtualnej.  

W tym przykładzie zostanie przedstawiony sposób odzyskiwania /var/www/html/index.nginx-debian.html strony sieci web nginx domyślne. Publiczny adres IP w naszym maszyny wirtualnej w tym przykładzie jest *13.69.75.209*. Można znaleźć adres IP za pomocą maszyny wirtualnej:

 ```bash 
 az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
 ```

 
1. Na komputerze lokalnym otwórz przeglądarkę i typ publiczny adres IP sieci maszyny wirtualnej, aby zobaczyć domyślnej strony sieci web nginx.

    ![Domyślna strona sieci web nginx](./media/tutorial-backup-vms/nginx-working.png)

1. Nawiąż połączenie z maszyną Wirtualną.

    ```bash
    ssh 13.69.75.209
    ```
2. Usuń /var/www/html/index.nginx-debian.html.

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```
    
4. Na komputerze lokalnym należy odświeżyć przeglądarkę przez naciśnięcie klawiszy CTRL + F5, aby zobaczyć, że domyślna strona nginx został usunięty.

    ![Domyślna strona sieci web nginx](./media/tutorial-backup-vms/nginx-broken.png)
    
1. Na komputerze lokalnym, zaloguj się do [portalu Azure](https://portal.azure.com/).
6. W menu po lewej stronie wybierz pozycję **Maszyny wirtualne**. 
7. Z listy wybierz maszynę Wirtualną.
8. W bloku maszyny Wirtualnej w **ustawienia** kliknij **kopii zapasowej**. **Kopii zapasowej** zostanie otwarty blok. 
9. Wybierz z menu w górnej części bloku **odzyskiwanie plików**. **Odzyskiwanie plików** zostanie otwarty blok.
10. W **krok 1: Wybierz punkt odzyskiwania**, wybierz punkt odzyskiwania z listy rozwijanej.
11. W **krok 2: Pobieranie skryptu, aby przeglądać i odzyskiwanie plików**, kliknij przycisk **Pobierz plik wykonywalny** przycisku. Zapisz pobrany plik na komputerze lokalnym.
7. Kliknij przycisk **pobrać skryptu** pobrać lokalnie plik skryptu.
8. Otwórz wiersz Bash i wpisz następujące polecenie, zastępując *Linux_myVM_05-05-2017.sh* z poprawną ścieżkę i nazwę pliku skryptu, który został pobrany, *azureuser* z nazwą użytkownika dla maszyny Wirtualnej i *13.69.75.209* z publicznym adresem IP dla maszyny Wirtualnej.
    
    ```bash
    scp Linux_myVM_05-05-2017.sh azureuser@13.69.75.209:
    ```
    
9. Na komputerze lokalnym należy otworzyć połączenie SSH maszyny Wirtualnej.

    ```bash
    ssh 13.69.75.209
    ```
    
10. Na maszynie Wirtualnej, należy dodać uprawnienia do pliku skryptu do wykonywania.

    ```bash
    chmod +x Linux_myVM_05-05-2017.sh
    ```
    
11. Na maszynie Wirtualnej należy uruchomić skrypt w celu punktu odzyskiwania jako system plików instalacji.

    ```bash
    ./Linux_myVM_05-05-2017.sh
    ```
    
12. Dane wyjściowe skryptu zawiera ścieżkę punktu instalacji. Dane wyjściowe wygląda podobnie do następującej:

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
                          
    Connecting to recovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of the recovery point to this machine...
                         
    ************ Volumes of the recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath 

    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170505191055/Volume1

    ************ Open File Explorer to browse for files. ************

    After recovery, to remove the disks and close the connection to the recovery point, please click 'Unmount Disks' in step 3 of the portal.

    Please enter 'q/Q' to exit...
    ```

12. Na maszynie Wirtualnej należy skopiować domyślnej strony sieci web nginx z punktu instalacji, do której usunąć plik.

    ```bash
    sudo cp ~/myVM-20170505191055/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```
    
17. Na komputerze lokalnym otwórz kartę przeglądarki, w której są podłączone do adresu IP maszyny wirtualnej przedstawiający nginx domyślnej strony. Naciśnij klawisze CTRL + F5, aby odświeżyć stronę przeglądarki. Powinna zostać wyświetlona domyślna strona działa ponownie.

    ![Domyślna strona sieci web nginx](./media/tutorial-backup-vms/nginx-working.png)

18. Na komputerze lokalnym, wróć do karty przeglądarki dla portalu Azure i w **krok 3: odinstaluj dyski po odzyskaniu** kliknij **odinstalować dyski** przycisku. Jeśli zapomnisz wykonać ten krok, połączenie punktu instalacji zostaje automatycznie zamknięty po 12 godzinach. Po tych 12 godzin należy pobrać nowe skryptu można utworzyć nowego punktu instalacji.


## <a name="next-steps"></a>Kolejne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz kopię zapasową maszyny Wirtualnej
> * Harmonogram tworzenia codziennej kopii zapasowej
> * Przywróć plik z kopii zapasowej

Przejdź do następnego samouczka, aby dowiedzieć się więcej na temat monitorowania maszyn wirtualnych.

> [!div class="nextstepaction"]
> [Monitorowanie maszyn wirtualnych](tutorial-monitoring.md)

