---
title: "Włącz replikację do platformy Azure dla maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Opisuje sposób włączania replikacji do platformy Azure dla maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM z usługą Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 89a2c4fc-7e03-4a86-a2c0-52831ccebc1a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 96a817e43a830e836f2faa4603fc88ed9c0b1828
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="step-11-enable-replication-to-azure-for-hyper-v-vms-in-vmm-clouds"></a>Krok 11: Włączanie replikacji do platformy Azure dla maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM

Po skonfigurowaniu zasad replikacji, użyj w tym artykule Aby włączyć replikację dla maszyn wirtualnych funkcji Hyper-V lokalnymi (VM) zarządzane w chmurach programu System Center Virtual Machine Manager (VMM)), Azure, przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) usługi w portalu Azure.

Opublikuj komentarze i pytania w dolnej części tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Przed rozpoczęciem

Upewnij się, że konto platformy Azure jest prawidłowy [uprawnienia](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)do tworzenia maszyn wirtualnych platformy Azure. [Dowiedz się więcej](../active-directory/role-based-access-built-in-roles.md) o kontroli dostępu opartej na rolach na platformie Azure.

## <a name="exclude-disks-from-replication"></a>Wykluczanie dysków z replikacji

Domyślnie wszystkie dyski na komputerze są replikowane. Dyski można wykluczyć z replikacji. Na przykład nie może być mają być replikowane dyski danych tymczasowych lub dane, które odświeżył każdym komputerze lub ponownym uruchomieniem aplikacji (na przykład pagefile.sys lub tempdb serwera SQL). [Dowiedz się więcej](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a>Replikowanie maszyn wirtualnych

Włącz replikację dla maszyn wirtualnych w następujący sposób:  

1. Kliknij kolejno pozycje **Krok 2. Replikowanie aplikacji** > **Źródło**. Po włączeniu replikacji po raz pierwszy kliknij pozycję **+ Replikuj** w magazynie, aby włączyć replikację dla dodatkowych maszyn.

    ![Włączanie replikacji](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication1.png)
2. W bloku **Źródło** wybierz serwer programu VMM i chmurę, w której znajdują się hosty funkcji Hyper-V. Następnie kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication-source.png)
3. W obszarze **Cel** wybierz subskrypcję, model wdrożenia po przejściu do trybu failover oraz konto magazynu używane do przechowywania replikowanych danych.

    ![Włączanie replikacji](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication-target.png)
4. Wybierz konto magazynu, którego chcesz użyć. Jeśli chcesz użyć konta magazynu innego niż posiadane, możesz [utworzyć konto](#set-up-an-azure-storage-account). Jeśli używasz konta usługi Premium Storage dla replikowanych danych, musisz skonfigurować dodatkowe, standardowe konto magazynu do przechowywania dzienników replikacji, które przechwytują zachodzące zmiany w danych lokalnych. Aby utworzyć konto magazynu za pomocą modelu usługi Resource Manager, kliknij pozycję **Utwórz nowy**. Jeśli chcesz utworzyć konto magazynu przy użyciu modelu klasycznego, należy to zrobić [w witrynie Azure Portal](../storage/common/storage-create-storage-account.md). Następnie kliknij przycisk **OK**.
5. Wybierz sieć platformy Azure i podsieć, z którą nawiążą połączenie maszyny wirtualne platformy Azure, gdy zostaną uruchomione po przejściu do trybu failover. Wybierz opcję **Konfiguruj teraz dla wybranych maszyn**, aby zastosować ustawienia sieci do wszystkich maszyn wybranych do ochrony. Wybierz opcję **Konfiguruj później**, aby wybrać sieć platformy Azure dla poszczególnych maszyn. Jeśli chcesz użyć sieci innej niż posiadana, możesz [utworzyć sieć](#set-up-an-azure-network). Aby utworzyć sieć przy użyciu modelu Resource Manager, kliknij pozycję **Utwórz nowe**. Jeśli chcesz utworzyć sieć przy użyciu modelu klasycznego, zrób to [w witrynie Azure Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Wybierz podsieć, jeśli jest to konieczne. Następnie kliknij przycisk **OK**.
6. W pozycji **Maszyny wirtualne** > **Wybierz maszyny wirtualne** kliknij i zaznacz każdą maszynę, którą chcesz replikować. Możesz wybrać tylko te maszyny, dla których można włączyć replikację. Następnie kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication5.png)

7. W pozycji **Właściwości** > **Konfiguracja właściwości** wybierz system operacyjny dla wybranych maszyn wirtualnych oraz dysk systemu operacyjnego.

    - Sprawdź, czy nazwa maszyny wirtualnej platformy Azure (nazwa docelowa) jest zgodna z [wymaganiami maszyny wirtualnej platformy Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).   
    - Domyślnie do replikacji są wybierane wszystkie dyski maszyny wirtualnej, ale można wyczyścić pozycje dysków, aby je wykluczyć.

        - Dyski można wykluczyć w celu zmniejszenia obciążenia przepustowości replikacji. Możesz na przykład zrezygnować z replikacji dysków z danymi tymczasowymi lub danych odświeżanych podczas każdego ponownego uruchomienia maszyny lub aplikacji (na przykład pliku pagefile.sys lub tempdb programu Microsoft SQL Server). Dysk można wykluczyć z replikacji, cofając jego zaznaczenie.
        - Wykluczyć można tylko dyski podstawowe. Nie można wykluczyć dysków systemu operacyjnego.
        - Nie zalecamy wykluczania dysków dynamicznych. Wewnątrz maszyny wirtualnej gościa usługa Site Recovery nie może ustalić, czy wirtualny dysk twardy jest dyskiem podstawowym, czy dynamicznym. Jeśli wszystkie zależne dyski woluminu dynamicznego nie zostaną wykluczone, chroniony dysk dynamiczny będzie wskazywany jako uszkodzony dysk maszyny wirtualnej w trybie failover, a dane na tym dysku nie będą dostępne.
        - Po włączeniu replikacji nie można dodawać ani usuwać dysków do replikacji. Jeśli chcesz dodać lub wykluczyć dysk, musisz wyłączyć ochronę maszyny wirtualnej, a następnie włączyć ją ponownie.
        - Dyski tworzone ręcznie na platformie Azure nie mają możliwości powrotu po awarii. Jeśli na przykład przeniesiesz trzy dyski do trybu failover i utworzysz dwa bezpośrednio na maszynie wirtualnej platformy Azure, tylko dla trzech dysków przeniesionych do trybu failover nastąpi powrót po awarii z platformy Azure do funkcji Hyper-V. Dysków tworzonych ręcznie nie można uwzględnić podczas powrotu po awarii ani replikacji odwrotnej z funkcji Hyper-V do platformy Azure.
        - Jeśli wykluczasz dysk wymagany do działania aplikacji, po przejściu do trybu failover na platformie Azure musisz ręcznie utworzyć go na tej platformie, aby można było uruchomić replikowaną aplikację. Można również zintegrować usługę Azure Automation w planie odzyskiwania, aby utworzyć dysk podczas pracy maszyny w trybie failover.

    Kliknij przycisk **OK**, aby zapisać zmiany. Później możesz skonfigurować dodatkowe właściwości.

    ![Włączanie replikacji](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

8. W pozycji **Ustawienia replikacji** > **Konfigurowanie ustawień replikacji** wybierz zasady replikacji, które chcesz zastosować dla chronionych maszyn wirtualnych. Następnie kliknij przycisk **OK**. Możesz zmodyfikować zasady replikacji w pozycji **Zasady replikacji** > nazwa_zasad > **Edytuj ustawienia**. Zastosowane zmiany są używane dla obecnie replikowanych i nowych maszyn.

   ![Włączanie replikacji](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication7.png)

Możesz śledzić postępy zadania **Włącz ochronę** w obszarze **Zadania** > **Zadania usługi Site Recovery**. Po uruchomieniu zadania **Sfinalizuj ochronę** maszyna jest gotowa do przejścia w tryb failover.



## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 12: testować tryb failover](vmm-to-azure-walkthrough-test-failover.md)
