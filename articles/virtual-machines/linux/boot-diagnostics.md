---
title: Rozruch diagnostyki dla maszyn wirtualnych systemu Linux na platformie Azure | Doc firmy Microsoft
description: "Omówienie dwóch funkcji debugowania dla maszyn wirtualnych systemu Linux na platformie Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: Deland-Han
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: delhan
ms.openlocfilehash: 70254d39b5c6326166f7e29fdfc99533835502f9
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-linux-virtual-machines-in-azure"></a>Rozwiązywanie problemów z maszyn wirtualnych systemu Linux na platformie Azure przy użyciu diagnostyki rozruchu

Na platformie Azure jest teraz dostępna obsługa dwóch funkcji debugowania: obsługa danych wyjściowych konsoli i zrzutu ekranu dla modelu wdrażania przy użyciu usługi Azure Virtual Machines Resource Manager. 

Podczas korzystania z własnego obrazu na platformie Azure, a nawet wykonywania rozruchu jednego z obrazów platformy, może wystąpić wiele przyczyn przejścia maszyny wirtualnej do stanu uniemożliwiającego rozruch. Te funkcje umożliwiają łatwe diagnozowanie i odzyskiwanie maszyn wirtualnych po niepowodzeniach rozruchu.

Dla maszyn wirtualnych z systemem Linux można łatwo wyświetlić dane wyjściowe konsoli dziennika za pomocą portalu:

![Azure Portal](./media/boot-diagnostics/screenshot1.png)
 
Jednak zarówno dla maszyn wirtualnych z systemem Linux, jak i Windows, platforma Azure umożliwia również wyświetlenie zrzutu ekranu maszyny wirtualnej za pomocą funkcji hypervisor:

![Błąd](./media/boot-diagnostics/screenshot2.png)

Obie te funkcje są obsługiwane dla maszyn wirtualnych platformy Azure we wszystkich regionach. Należy pamiętać, że może minąć do 10 minut, zanim zrzuty ekranu i dane wyjściowe pojawią się na koncie magazynu.

## <a name="common-boot-errors"></a>Typowe błędy rozruchu

- [Zagadnienia dotyczące systemu plików](https://blogs.msdn.microsoft.com/linuxonazure/2016/09/13/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes/)
- [Problemy z jądra](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-manually-fixing-non-boot-issues-related-to-kernel-problems/)
- [Błędy FSTAB](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/ )

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Włączanie diagnostyki na nowej maszynie wirtualnej
1. Podczas tworzenia nowej maszyny wirtualnej w portalu w wersji zapoznawczej wybierz pozycję **Azure Resource Manager** z listy rozwijanej modelu wdrażania:
 
    ![Resource Manager](./media/boot-diagnostics/screenshot3.jpg)

2. Skonfiguruj opcję Monitorowanie, aby wybrać konto magazynu, w którym chcesz umieścić te pliki diagnostyczne.
 
    ![Tworzenie maszyny wirtualnej](./media/boot-diagnostics/screenshot4.jpg)

3. Jeśli wykonujesz wdrożenie z szablonu usługi Azure Resource Manager, przejdź do zasobu maszyny wirtualnej i dołącz sekcję profilu diagnostyki. Pamiętaj, aby użyć nagłówka wersji interfejsu API „2015-06-15”.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. Profil diagnostyki umożliwia wybranie konta magazynu, na którym chcesz umieścić te dzienniki.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

## <a name="update-an-existing-virtual-machine"></a>Aktualizowanie istniejącej maszyny wirtualnej

Aby włączyć diagnostyki rozruchu za pośrednictwem portalu, należy zaktualizować istniejącej maszyny wirtualnej za pośrednictwem portalu. Wybierz opcję Diagnostyka rozruchu i kliknij ikonę Zapisz. Uruchom ponownie maszynę wirtualną, aby zmiany zaczęły obowiązywać.

![Aktualizowanie istniejącej maszyny wirtualnej](./media/boot-diagnostics/screenshot5.png)