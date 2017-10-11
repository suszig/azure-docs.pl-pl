---
title: "Włącz replikację serwerów fizycznych replikację do platformy Azure z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Zawiera podsumowanie kroków należy włączyć replikację serwerów fizycznych do platformy Azure przy użyciu usługi Azure Site Recovery"
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
ms.openlocfilehash: 42f35c53eec06a346281fd90c97aecfd2269307d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="step-10-enable-replication-for-physical-servers-to-azure"></a>Krok 10: Włączanie replikacji dla serwerów fizycznych do platformy Azure


W tym artykule opisano sposób włączania replikacji lokalnych serwerów fizycznych systemu Windows i Linux na platformie Azure, przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) w portalu Azure.

Opublikuj komentarze i pytania w dolnej części tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Przed rozpoczęciem

- Serwery muszą mieć [zainstalowanie składnika usługi Mobility](physical-walkthrough-install-mobility.md).
- Jeśli maszyna wirtualna jest gotowy do instalacji w trybie wypychania, serwer przetwarzania automatycznie instaluje usługi mobilności, po włączeniu replikacji.
- Po włączeniu maszyny do replikacji konta użytkownika platformy Azure wymaga określonych [uprawnienia](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) aby upewnić się, możesz użyć usługi Azure storage i Tworzenie maszyn wirtualnych platformy Azure.
- Podczas dodawania lub modyfikowania adresów IP dla serwerów, może potrwać do 15 minut lub dłużej aby zmiany zaczęły obowiązywać, a były widoczne w portalu.


## <a name="exclude-disks-from-replication"></a>Wykluczanie dysków z replikacji

Domyślnie wszystkie dyski na komputerze są replikowane. Dyski można wykluczyć z replikacji. Na przykład nie może być mają być replikowane dyski danych tymczasowych lub dane, które odświeżył każdym komputerze lub ponownym uruchomieniem aplikacji (na przykład pagefile.sys lub tempdb serwera SQL). [Dowiedz się więcej](site-recovery-exclude-disk.md)

## <a name="replicate-servers"></a>Replikacja serwerów

1. Kliknij kolejno pozycje **Krok 2. Replikowanie aplikacji** > **Źródło**.
2. W **źródła**, wybierz serwer konfiguracji.
3. W **komputera typu**, wybierz pozycję **maszyn fizycznych**.
4. Wybierz serwer przetwarzania. Jeśli nie utworzono żadnych serwerów dodatkowych procesów będzie serwera konfiguracji. Następnie kliknij przycisk **OK**.
5. W **docelowego**, wybierz subskrypcji i grupy zasobów, w którym chcesz utworzyć nieudane przez maszyny wirtualne. Wybierz model wdrażania, który ma być używany na platformie Azure (Zarządzanie zasobu lub classic) dla w trybie Failover maszyny wirtualne.
6. Wybierz konto magazynu Azure, którego chcesz użyć do replikacji danych. Jeśli nie chcesz użyć konta, które zostały już skonfigurowane, można utworzyć nowy.
7. Wybierz **sieć platformy Azure** i **podsieci** nawiązać którego maszynach wirtualnych Azure po pracy awaryjnej. Wybierz opcję **Konfiguruj teraz dla wybranych maszyn**, aby zastosować ustawienia sieci do wszystkich maszyn wybranych do ochrony. Wybierz opcję **Konfiguruj później**, aby wybrać sieć platformy Azure dla poszczególnych maszyn. Jeśli nie chcesz użyć istniejącej sieci, można go utworzyć.

    ![Włączanie replikacji](./media/physical-walkthrough-enable-replication/targetsettings.png)

8. W **maszyn fizycznych**, kliknij przycisk **+ komputera fizycznego** , a następnie wprowadź **nazwa** i **adres IP**. Wybierz system operacyjny maszyny, które mają być replikowane. Trwa kilka minut, aż maszyny są odnajdywane i wyświetlane na liście.
9. W **właściwości** > **skonfigurować właściwości**, wybierz konto, które będzie używane przez serwer przetwarzania Aby automatycznie zainstalować usługi mobilności na maszynie.
10. Domyślnie wszystkie dyski są replikowane. Kliknij przycisk **wszystkie dyski** i wyczyść wszystkie dyski, które nie mają być replikowane. Następnie kliknij przycisk **OK**. Później można ustawić dodatkowe właściwości maszyny Wirtualnej.

    ![Włączanie replikacji](./media/physical-walkthrough-enable-replication/enable-replication6.png)
11. W **ustawienia replikacji** > **Konfigurowanie ustawień replikacji**, sprawdź, czy jest wybrane zasady replikacji poprawne. Jeśli zmodyfikujesz zasady, zmiany zostaną zastosowane do replikowania maszyny i nowych maszyn.
12. Włącz **spójności wielu maszyn wirtualnych** Jeśli chcesz zebrać maszyn w grupie replikacji i określ nazwę grupy. Następnie kliknij przycisk **OK**. Należy pamiętać, że:

    * Komputery w grupach replikacji replikowane wspólnie i udostępnione punkty odzyskiwania spójna w razie awarii i spójności aplikacji podczas ich w tryb failover.
    * Zaleca się, że użytkownik zbiera serwerów fizycznych, aby odzwierciedlały obciążeń. Włączenie spójności wielu maszyn wirtualnych może wpłynąć na wydajność obciążeń i należy używać tylko w przypadku maszyn działają te same obciążenia i wymagana jest spójność.

13. Kliknij przycisk **włączyć replikację**. Możesz śledzić postęp **Włącz ochronę** zadania w **ustawienia** > **zadania** > **zadania usługi Site Recovery**. Po uruchomieniu zadania **Sfinalizuj ochronę** maszyna jest gotowa do przejścia w tryb failover.

## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 11: testować tryb failover](physical-walkthrough-test-failover.md)
