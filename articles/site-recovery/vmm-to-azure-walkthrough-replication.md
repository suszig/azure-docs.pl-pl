---
title: "Skonfiguruj zasady replikacji w przypadku replikacji maszyn wirtualnych funkcji Hyper-V (w programie VMM) na platformie Azure z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Zawiera opis sposobu konfigurowania zasady replikacji w przypadku replikacji maszyn wirtualnych funkcji Hyper-V (w programie VMM) na platformie Azure z usługą Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ee808b20-324b-4198-b831-edb65b95e8b7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 592e1c3f647e5b1f1d9aa776657e8f89b60349e1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="step-10-set-up-a-replication-policy-for-hyper-v-vm-replication-with-vmm-to-azure"></a>Krok 10: Skonfiguruj zasady replikacji w przypadku replikacji maszyn wirtualnych funkcji Hyper-V (w programie VMM) na platformie Azure


Po skonfigurowaniu [mapowania sieci](vmm-to-azure-walkthrough-network-mapping.md), użyj w tym artykule, aby skonfigurować zasady replikacji T\to replikowania lokalnych maszyn wirtualnych funkcji Hyper-V zarządzane w chmurach programu System Center Virtual Machine Manager (VMM) na platformie Azure, przy użyciu [Usługi azure Site Recovery](site-recovery-overview.md) w portalu Azure.

Po przeczytaniu tego artykułu zamieść wszelkie komentarze u dołu lub na [forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="create-a-policy"></a>Tworzenie zasad

1. Aby utworzyć nowe zasady replikacji, kliknij kolejno pozycje **Przygotowanie infrastruktury** > **Ustawienia replikacji** > **+ Utwórz i skojarz**.

    ![Sieć](./media/vmm-to-azure-walkthrough-replication/gs-replication.png)
2. W obszarze **Utwórz i skojarz zasady** określ nazwę zasad.
3. W obszarze **Częstotliwość kopiowania** określ, jak często mają być replikowane dane przyrostowe po replikacji początkowej (co 30 sekund, 5 lub 15 minut).

    > [!NOTE]
    >  Częstotliwość 30-sekundowa nie jest obsługiwana w przypadku replikowania do usługi Premium Storage. To ograniczenie wynika z liczby migawek na obiekt blob (100) obsługiwanych przez usługę Premium Storage. [Dowiedz się więcej](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. W obszarze **Przechowywanie punktu odzyskiwania** określ w godzinach, jak długie będzie okno przechowywania dla każdego punktu odzyskiwania. Chronione maszyny można odzyskać do dowolnego punktu w tym oknie.
5. W obszarze **Częstotliwość migawek spójności aplikacji** określ, jak często (1–12 godzin) będą tworzone punkty odzyskiwania zawierające migawki spójne z aplikacjami. Funkcja Hyper-V wykorzystuje dwa typy migawek — standardową migawkę, która jest przyrostową migawką całej maszyny wirtualnej oraz migawkę spójności aplikacji, która wykonuje migawkę danych aplikacji wewnątrz maszyny wirtualnej w danym punkcie w czasie. Migawki spójne z aplikacjami używają usługi Volume Shadow Copy (VSS), aby zapewnić, że aplikacje są w spójnym stanie podczas wykonywania migawki. Należy pamiętać, że jeśli migawki spójne z aplikacjami zostaną włączone, będzie to miało wpływ na wydajność aplikacji uruchomionych na źródłowych maszynach wirtualnych. Upewnij się, że ustawiona wartość jest mniejsza od liczby skonfigurowanych dodatkowych punktów odzyskiwania.
6. W obszarze **Czas rozpoczęcia replikacji początkowej** określ, kiedy rozpoczyna się replikacja początkowa. Replikacja odbywa się z wykorzystaniem przepustowości Internetu, dlatego warto zaplanować ją poza najbardziej obciążonymi godzinami.
7. W obszarze **Szyfrowanie danych przechowywanych na platformie Azure** określ, czy należy szyfrować dane w stanie spoczynku w usłudze Azure Storage. Następnie kliknij przycisk **OK**.

    ![Zasady replikacji](./media/vmm-to-azure-walkthrough-replication/gs-replication2.png)
8. Jeśli utworzysz nowe zasady, zostaną one automatycznie skojarzone z chmurą VMM. Kliknij przycisk **OK**. Możesz skojarzyć dodatkowe chmury VMM (oraz maszyny wirtualne w tych chmurach) z zasadami replikacji w obszarze **Replikacja** > nazwa_zasad > **Skojarz chmurę VMM**.

    ![Zasady replikacji](./media/vmm-to-azure-walkthrough-replication/policy-associate.png)



## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 11: Włączanie replikacji](vmm-to-azure-walkthrough-enable-replication.md)
