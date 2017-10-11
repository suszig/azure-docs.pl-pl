---
title: "Włącz replikację funkcji Hyper-V do lokacji dodatkowej z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak włączyć replikację dla maszyn wirtualnych funkcji Hyper-V replikację do dodatkowej lokacji programu System Center VMM z usługą Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d8782d14-9fef-4396-8912-ff945efc851b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 6673d192dbc18bfc955d9e7e3c55893512511ffb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="step-9-enable-replication-to-a-secondary-site-for-hyper-v-vms"></a>Krok 9: Włączanie replikacji do lokacji dodatkowej dla maszyn wirtualnych funkcji Hyper-V


Po konfigurowania zasad replikacji, użyj w tym artykule, aby włączyć replikacji do lokacji dodatkowej programu System Center Virtual Machine Manager (VMM) dla maszyn wirtualnych funkcji Hyper-V lokalnymi (VM), za pomocą [usługi Azure Site Recovery](site-recovery-overview.md).

Po przeczytaniu tego artykułu zamieść wszelkie komentarze u dołu lub na [forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="select-vms-to-replicate"></a>Wybierz maszyny wirtualne do replikacji

1. Kliknij kolejno pozycje **Krok 2. Replikowanie aplikacji** > **Źródło**. 

    ![Włączanie replikacji](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication1.png)

2. W **źródła**, wybierz serwer programu VMM oraz chmury, w którym znajdują się hosty funkcji Hyper-V mają być replikowane. Następnie kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication2.png)
3. W **docelowego**, sprawdź pomocniczy serwer programu VMM i w chmurze.
4. W **maszyn wirtualnych**, wybierz maszyny wirtualne, które chcesz chronić z listy.

    ![Włączanie ochrony maszyny wirtualnej](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication5.png)

Możesz śledzić postęp **Włącz ochronę** akcji w **zadania** > **zadania usługi Site Recovery**. Po **zakończenia ochrony** zadanie zostało ukończone, Replikacja początkowa została zakończona i maszyna wirtualna jest gotowa do pracy awaryjnej.

Należy pamiętać, że:

- Można również włączyć ochronę maszyn wirtualnych w konsoli programu VMM. Kliknij przycisk **Włącz ochronę** na pasku narzędzi we właściwościach maszyny wirtualnej > **usługi Azure Site Recovery** kartę.
- Po włączeniu replikacji można wyświetlić właściwości dla maszyny Wirtualnej w ramach **elementy replikowane**. Na **Essentials** pulpitu nawigacyjnego, znajdują się informacje dotyczące zasad replikacji dla maszyny Wirtualnej i jej stanie. Kliknij przycisk **właściwości** więcej szczegółów.

## <a name="onboard-existing-vms"></a>Dodaj istniejące maszyny wirtualne

Jeśli masz istniejące maszyny wirtualne w programie VMM, które są replikowane z funkcji Hyper-V Replica, możesz dołączyć je do replikacji usługi Azure Site Recovery w następujący sposób:

1. Upewnij się, że serwer funkcji Hyper-V hosting istniejącej maszyny Wirtualnej znajduje się w chmurze podstawowej oraz że serwera funkcji Hyper-V obsługującego maszyny wirtualnej repliki znajduje się w chmurze dodatkowej.
2. Upewnij się, że skonfigurowano zasady replikacji dla chmury VMM podstawowej.
3. Włączanie replikacji podstawowej maszyny wirtualnej. Azure Site Recovery i VMM upewnij się, że wykrycia tego samego hosta repliki i maszyny wirtualne i usługi Azure Site Recovery spowoduje ponowne użycie i ponownie ustanowić replikację przy użyciu określonych ustawień.


## <a name="next-steps"></a>Następne kroki

Przejdź do [kroku 10: testować tryb failover](vmm-to-vmm-walkthrough-test-failover.md).
