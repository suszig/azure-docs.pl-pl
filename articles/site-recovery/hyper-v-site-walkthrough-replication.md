---
title: "Skonfiguruj zasady replikacji w przypadku replikacji maszyn wirtualnych funkcji Hyper-V (bez programu System Center VMM) na platformie Azure za pomocą usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Zawiera podsumowanie kroków, które należy skonfigurować zasady replikacji podczas replikowania maszyn wirtualnych funkcji Hyper-V do magazynu Azure"
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 1777e0eb-accb-42b5-a747-11272e131a52
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: ca5bec5cf1152e6259b9fe7a869edd2d62b88e1a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="step-9-set-up-a-replication-policy-for-hyper-v-vm-replication-to-azure"></a>Krok 9: Konfigurowanie zasady replikacji w przypadku replikacji maszyn wirtualnych funkcji Hyper-V do platformy Azure

W tym artykule opisano sposób konfigurowania zasad replikacji w przypadku replikacji maszyn wirtualnych funkcji Hyper-V do platformy Azure (bez programu System Center VMM) przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) w portalu Azure.


Opublikuj komentarze i pytania w dolnej części tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="about-snapshots"></a>Temat migawek

Funkcja Hyper-V wykorzystuje dwa typy migawek — standardową migawkę, która jest przyrostową migawką całej maszyny wirtualnej oraz migawkę spójności aplikacji, która wykonuje migawkę danych aplikacji wewnątrz maszyny wirtualnej w danym punkcie w czasie.
    - Migawki spójne z aplikacjami używają usługi Volume Shadow Copy (VSS), aby zapewnić, że aplikacje są w spójnym stanie podczas wykonywania migawki.
    - Jeśli włączysz migawki spójne z aplikacjami, ma wpływ na wydajność aplikacji uruchomionych na źródłowych maszynach wirtualnych. Upewnij się, że ustawiona wartość jest mniejsza od liczby skonfigurowanych dodatkowych punktów odzyskiwania.

## <a name="set-up-a-replication-policy"></a>Konfigurowanie zasad replikacji

1. Aby utworzyć nowe zasady replikacji, kliknij kolejno pozycje **Przygotowanie infrastruktury** > **Ustawienia replikacji** > **+ Utwórz i skojarz**.

    ![Sieć](./media/hyper-v-site-walkthrough-replication/gs-replication.png)
2. W obszarze **Utwórz i skojarz zasady** określ nazwę zasad.
3. W obszarze **Częstotliwość kopiowania** określ, jak często mają być replikowane dane przyrostowe po replikacji początkowej (co 30 sekund, 5 lub 15 minut).

    > [!NOTE]
    > Częstotliwość 30-sekundowa nie jest obsługiwana w przypadku replikowania do usługi Premium Storage. To ograniczenie wynika z liczby migawek na obiekt blob (100) obsługiwanych przez usługę Premium Storage. [Dowiedz się więcej](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).

4. W **przechowywania punktu odzyskiwania**, określ w godzinach, jak długo trwa okna przechowywania dla każdego punktu odzyskiwania. Maszyny wirtualne można odzyskać do dowolnego punktu w tym oknie.
5. W **częstotliwość migawek spójności aplikacji**, określ, jak często (1 – 12 godzin) punkty odzyskiwania zawierające migawki spójne z aplikacjami są tworzone.
6. W **czas rozpoczęcia replikacji początkowej**, określ, kiedy można uruchomić replikacji początkowej. Replikacja odbywa się z wykorzystaniem przepustowości Internetu, dlatego warto zaplanować ją poza najbardziej obciążonymi godzinami. Następnie kliknij przycisk **OK**.

    ![Zasady replikacji](./media/hyper-v-site-walkthrough-replication/gs-replication2.png)

Podczas tworzenia nowych zasad, zostaną one automatycznie skojarzone z lokacji funkcji Hyper-V. Można skojarzyć z wieloma zasadami replikacji w lokacji funkcji Hyper-V (oraz maszyny wirtualne w nim) **replikacji** > Nazwa zasady > **skojarzyć lokacji funkcji Hyper-V**.



## <a name="next-steps"></a>Następne kroki

Przejdź do [kroku 10: Włączanie replikacji](hyper-v-site-walkthrough-enable-replication.md)
