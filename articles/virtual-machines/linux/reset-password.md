---
title: "Jak można zresetować hasła lokalnego systemu Linux na maszynach wirtualnych Azure | Dokumentacja firmy Microsoft"
description: "Wprowadzenie czynności, aby zresetować hasło lokalne systemu Linux na maszynie Wirtualnej Azure"
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: delhan
ms.openlocfilehash: f16f51ced4a54f9d083ac31ff176934a82219670
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="how-to-reset-local-linux-password-on-azure-vms"></a>Jak można zresetować hasła lokalnego systemu Linux na maszynach wirtualnych Azure

W tym artykule przedstawiono kilka metod resetowania haseł usługi lokalnej maszyny wirtualnej systemu Linux (VM). Jeśli konto użytkownika wygasło lub po prostu chcesz utworzyć nowe konto, można użyć następujących metod, Utwórz nowe konto administratora lokalnego i ponownie uzyskać dostęp do maszyny Wirtualnej.

## <a name="symptoms"></a>Objawy

Nie można zalogować się do maszyny Wirtualnej, i pojawi się komunikat informujący o tym, że hasło używane jest nieprawidłowa. Ponadto nie można użyć VMAgent resetowania hasła w portalu Azure. 

## <a name="manual-password-reset-procedure"></a>Procedury ręcznego resetowania hasła

1.  Usuń maszynę Wirtualną i Zachowaj dołączonych dysków.

2.  Dołączanie dysku systemu operacyjnego jako dysku danych do innego danych czasowych maszyny Wirtualnej w tej samej lokalizacji.

3.  Uruchom następujące polecenie SSH na danych czasowych maszyny Wirtualnej, aby stać się nadtypem użytkownika.


    ~~~~
    sudo su
    ~~~~

4.  Uruchom **fdisk -l** lub przyjrzeć się dzienniki systemowe, aby znaleźć nowo dołączonego dysku. Znajdź nazwę dysku do zainstalowania. Na Maszynie wirtualnej danych czasowych, poszukaj w odpowiedni plik dziennika.

    ~~~~
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ~~~~

    Poniżej przedstawiono przykładowe dane wyjściowe polecenia grep:

    ~~~~
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ~~~~

5.  Utwórz punkt instalacji o nazwie **tempmount**.

    ~~~~
    mkdir /tempmount
    ~~~~

6.  Zainstaluj dysk systemu operacyjnego w punkcie instalacji. Zazwyczaj należy sdc1 instalacji lub sdc2. Zależy to hostingu partycji w katalogu/etc z dysku komputera przerwane.

    ~~~~
    mount /dev/sdc1 /tempmount
    ~~~~

7.  Wykonaj kopię zapasową przed wprowadzeniem jakichkolwiek zmian:

    ~~~~
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ~~~~

8.  Resetowania hasła użytkownika, które są potrzebne:

    ~~~~
    passwd <<USER>> 
    ~~~~

9.  Przenieś zmodyfikowane pliki do poprawnej lokalizacji na dysku komputera przerwane.

    ~~~~
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    
10. Go back to the root and unmount the disk.

    ~~~~
    dysk CD / umount /tempmount
    ~~~~

11. Detach the disk from the management portal.

12. Recreate the VM.

## Next steps

* [Troubleshoot Azure VM by attaching OS disk to another Azure VM](http://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: How to delete and re-deploy a VM from VHD](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
