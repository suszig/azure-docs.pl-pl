---
title: "Włącz replikację maszyn wirtualnych VMware replikację do platformy Azure z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Zawiera podsumowanie kroków należy włączyć replikację do platformy Azure dla maszyn wirtualnych VMware przy użyciu usługi Azure Site Recovery"
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 519c5559-7032-4954-b8b5-f24f5242a954
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 470b9ddd8df4a4e74ec7174f79020c252323e502
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="step-11-enable-replication-for-vmware-virtual-machines-to-azure"></a>Krok 11: Włączanie replikacji maszyn wirtualnych VMware do platformy Azure


W tym artykule opisano sposób włączania replikacji dla lokalnych maszyn wirtualnych VMware do platformy Azure, przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) w portalu Azure.

Opublikuj komentarze i pytania w dolnej części tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Przed rozpoczęciem

- Maszyny wirtualne VMware muszą mieć [zainstalowanie składnika usługi Mobility](vmware-walkthrough-install-mobility.md). — Jeśli maszyna wirtualna jest gotowy do instalacji w trybie wypychania, serwer przetwarzania automatycznie instaluje usługi mobilności, po włączeniu replikacji.
- Twoje konto użytkownika systemu Azure wymaga określonych [uprawnienia](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) Aby włączyć replikację maszyny wirtualnej na platformie Azure
- Podczas dodawania lub modyfikowania maszyn wirtualnych może potrwać do 15 minut lub dłużej aby zmiany zaczęły obowiązywać, a były widoczne w portalu.
- Możesz sprawdzić czas ostatniego odnalezionych dla maszyn wirtualnych w **serwery konfiguracji** > **ostatniego kontaktu w**.
- Aby dodać maszyn wirtualnych bez oczekiwania na zaplanowanego odnajdywania, zaznacz serwer konfiguracji (nie klikaj pozycji go) i kliknij przycisk **Odśwież**.



## <a name="exclude-disks-from-replication"></a>Wykluczanie dysków z replikacji

Domyślnie wszystkie dyski na komputerze są replikowane. Dyski można wykluczyć z replikacji. Na przykład nie może być mają być replikowane dyski danych tymczasowych lub dane, które odświeżył każdym komputerze lub ponownym uruchomieniem aplikacji (na przykład pagefile.sys lub tempdb serwera SQL). [Dowiedz się więcej](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a>Replikowanie maszyn wirtualnych

Przed rozpoczęciem, Obejrzyj to szybki przegląd wideo

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. Kliknij kolejno pozycje **Krok 2. Replikowanie aplikacji** > **Źródło**.
2. W **źródła**, wybierz serwer konfiguracji.
3. W **komputera typu**, wybierz pozycję **maszyn wirtualnych**.
4. W **vCenter/vSphere Hypervisor**, wybierz serwer vCenter, który zarządza hostem vSphere lub wybierz host.
5. Wybierz serwer przetwarzania. Jeśli nie utworzono żadnych serwerów dodatkowych procesów będzie serwera konfiguracji. Następnie kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/vmware-walkthrough-enable-replication/enable-replication2.png)

6. W **docelowego**, wybierz subskrypcji i grupy zasobów, w którym chcesz utworzyć nieudane przez maszyny wirtualne. Wybierz model wdrażania, który ma być używany na platformie Azure (Zarządzanie zasobu lub classic) dla w trybie Failover maszyny wirtualne.


7. Wybierz konto magazynu Azure, którego chcesz użyć do replikacji danych. Jeśli nie chcesz użyć konta, które zostały już skonfigurowane, można utworzyć nowy.

8. Wybierz sieć platformy Azure i podsieć, z którą nawiążą połączenie maszyny wirtualne platformy Azure, gdy zostaną uruchomione po przejściu do trybu failover. Wybierz opcję **Konfiguruj teraz dla wybranych maszyn**, aby zastosować ustawienia sieci do wszystkich maszyn wybranych do ochrony. Wybierz opcję **Konfiguruj później**, aby wybrać sieć platformy Azure dla poszczególnych maszyn. Jeśli nie chcesz użyć istniejącej sieci, można go utworzyć.

    ![Włączanie replikacji](./media/vmware-walkthrough-enable-replication/enable-rep3.png)
9. W pozycji **Maszyny wirtualne** > **Wybierz maszyny wirtualne** kliknij i zaznacz każdą maszynę, którą chcesz replikować. Możesz wybrać tylko te maszyny, dla których można włączyć replikację. Następnie kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/vmware-walkthrough-enable-replication/enable-replication5.png)
10. W **właściwości** > **skonfigurować właściwości**, wybierz konto, które będzie używane przez serwer przetwarzania Aby automatycznie zainstalować usługi mobilności na maszynie.
11. Domyślnie wszystkie dyski są replikowane. Kliknij przycisk **wszystkie dyski** i wyczyść wszystkie dyski, które nie mają być replikowane. Następnie kliknij przycisk **OK**. Później można ustawić dodatkowe właściwości maszyny Wirtualnej.

    ![Włączanie replikacji](./media/vmware-walkthrough-enable-replication/enable-replication6.png)
11. W **ustawienia replikacji** > **Konfigurowanie ustawień replikacji**, sprawdź, czy jest wybrane zasady replikacji poprawne. Jeśli zmodyfikujesz zasady, zmiany zostaną zastosowane do replikowania maszyny i nowych maszyn.
12. Włącz **spójności wielu maszyn wirtualnych** Jeśli chcesz zebrać maszyn w grupie replikacji i określ nazwę grupy. Następnie kliknij przycisk **OK**. Należy pamiętać, że:

    * Komputery w grupach replikacji replikowane wspólnie i udostępnione punkty odzyskiwania spójna w razie awarii i spójności aplikacji podczas ich w tryb failover.
    * Zaleca się, że użytkownik zbiera maszyn wirtualnych i serwerów fizycznych, aby odzwierciedlały obciążeń. Włączenie spójności wielu maszyn wirtualnych może wpłynąć na wydajność obciążeń i należy używać tylko w przypadku maszyn działają te same obciążenia i wymagana jest spójność.

    ![Włączanie replikacji](./media/vmware-walkthrough-enable-replication/enable-replication7.png)
13. Kliknij przycisk **włączyć replikację**. Możesz śledzić postęp **Włącz ochronę** zadania w **ustawienia** > **zadania** > **zadania usługi Site Recovery**. Po uruchomieniu zadania **Sfinalizuj ochronę** maszyna jest gotowa do przejścia w tryb failover.

## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 12: testować tryb failover](vmware-walkthrough-test-failover.md)
