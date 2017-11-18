---
title: Temat dysku magazynu dla maszyn wirtualnych systemu Linux programu Microsoft Azure | Dokumentacja firmy Microsoft
description: "Poznaj podstawy magazynu dysków i wirtualne dyski twarde dla maszyn wirtualnych systemu Linux na platformie Azure."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 7be8dd52-98f7-4187-9b78-55197915bc9b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: robinsh
ms.openlocfilehash: ad55898806024a9f0562b32e7bdd990fd7dfd8d2
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="about-disk-storage-for-azure-linux-vms"></a>Temat dysku magazynu dla maszyn wirtualnych systemu Linux platformy Azure
Podobnie jak dowolnego innego komputera maszynach wirtualnych platformy Azure używać dysków jako miejsce do przechowywania systemu operacyjnego, aplikacji i danych. Wszystkie maszyny wirtualne platformy Azure są co najmniej dwa dyski — dysk systemu operacyjnego Linux i dysku tymczasowym. Dysk systemu operacyjnego jest tworzony z obrazu, a zarówno dysku systemu operacyjnego i obrazu są faktycznie wirtualnych dysków twardych (VHD) przechowywane w koncie magazynu platformy Azure. Maszyny wirtualne mogą także mieć co najmniej jeden dysk danych, które są także przechowywane jako wirtualne dyski twarde. 

W tym artykule firma Microsoft będzie porozmawiać na temat różnych zastosowań dysków, a następnie omówiono różne typy dysków, można tworzyć i używać. W tym artykule jest również dostępny do [maszyn wirtualnych Windows](../windows/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Dysków używanych przez maszyny wirtualne

Spójrzmy na jak dyski są używane przez maszyny wirtualne.

## <a name="operating-system-disk"></a>Dysk systemu operacyjnego
Co maszyna wirtualna ma jeden dysk systemu operacyjnego podłączonego. Jest zarejestrowany jako dysk SATA, a etykietą /dev/sda domyślnie. Ten dysk ma maksymalną pojemność 2048 gigabajtów (GB). 

## <a name="temporary-disk"></a>Tymczasowe dysku
Każda maszyna wirtualna zawiera dysku tymczasowym. Dysku tymczasowym zapewnia krótkoterminowy magazyn dla aplikacji i procesów i jest przeznaczona tylko przechowywania danych, takich jak pliki strony lub wymiany. Dane na dysku tymczasowym mogą zostać utracone podczas [zdarzenia konserwacji](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) lub gdy użytkownik [ponownego wdrażania maszyny Wirtualnej](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Podczas standardowego ponownego uruchomienia maszyny wirtualnej ma utrwalić danych na dysku tymczasowym.

W przypadku maszyn wirtualnych systemu Linux, dysk jest zwykle **/dev/sdb** jest sformatowany i zainstalowany **katalogu/mnt** przez agenta systemu Linux platformy Azure. Rozmiar dysku tymczasowym różni się zależnie od rozmiaru maszyny wirtualnej. Aby uzyskać więcej informacji, zobacz [rozmiary maszyn wirtualnych systemu Linux](../windows/sizes.md).

Aby uzyskać więcej informacji o używaniu dysku tymczasowym Azure, zobacz [opis dysku tymczasowym na maszynach wirtualnych Azure firmy Microsoft](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Dysk z danymi
Dysk z danymi jest wirtualnego dysku twardego, który jest dołączony do maszyny wirtualnej do przechowywania danych aplikacji lub innych danych, które należy zachować. Dyski danych są rejestrowane jako dyski SCSI i są oznaczone literą, którą można wybrać. Każdy dysk danych ma maksymalną pojemność 4095 GB. Rozmiar maszyny wirtualnej Określa, jak wiele dysków z danymi można dołączyć do jego i typ magazynu służy do obsługi dysków.

> [!NOTE]
> Aby uzyskać więcej informacji o pojemności maszyn wirtualnych, zobacz [rozmiary maszyn wirtualnych systemu Linux](../windows/sizes.md).
> 

Azure tworzy dysk systemu operacyjnego, podczas tworzenia maszyny wirtualnej z obrazu. Jeśli używasz obrazu, który zawiera dyski danych Azure tworzy także dysków danych podczas tworzenia maszyny wirtualnej. W przeciwnym razie możesz dodać dysk danych, po utworzeniu maszyny wirtualnej.

Można dodać dysków z danymi do maszyny wirtualnej w dowolnym momencie przez **dołączanie** dysku do maszyny wirtualnej. Można użyć wirtualnego dysku twardego, który został przekazany lub kopiowane do konta magazynu lub taki, który tworzy Azure. Dołączenie dysku danych skojarzenie pliku wirtualnego dysku twardego z maszyną Wirtualną, umieszczając dzierżawy na dysku VHD nie może być usunięty z magazynu nadal będzie dołączony.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Następne kroki
* [Dołączenie dysku](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) dodać dodatkowy magazyn dla maszyny Wirtualnej.
* [Utwórz migawkę](snapshot-copy-managed-disk.md).
* [Konwertuj na dyskach zarządzanych](convert-unmanaged-to-managed-disks.md).

