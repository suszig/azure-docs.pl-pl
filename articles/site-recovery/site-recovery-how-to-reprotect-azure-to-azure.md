---
title: "Jak ponownej ochrony od przejścia w tryb failover maszyn wirtualnych platformy Azure do regionu Azure podstawowego | Dokumentacja firmy Microsoft"
description: "Po pracy awaryjnej maszyn wirtualnych z jednego regionu Azure do innego można użyć usługi Azure Site Recovery do ochrony maszyn w odwrotnym kierunku. Jak kroki przeprowadzenie ponownej ochrony przed ponownie trybu failover."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: rajanaki
ms.openlocfilehash: 3e614b6c3c8358585f3b502f301cc659d2088e2f
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="reprotect-from-failed-over-azure-region-back-to-primary-region"></a>Ponownej ochrony od przejścia w tryb failover region platformy Azure do regionu podstawowego



>[!NOTE]
>
> Replikacja lokacji odzyskiwania maszyn wirtualnych platformy Azure jest obecnie w przeglądzie.


## <a name="overview"></a>Omówienie
Gdy użytkownik [pracy awaryjnej](site-recovery-failover.md) maszyn wirtualnych z jednego regionu Azure do innego, maszyny wirtualne są w stanie niechronionym. Aby przywrócić je do regionu podstawowego, należy najpierw ponownie ochronę maszyny wirtualnej, a następnie trybu failover. Nie ma żadnej różnicy między jak failover w jednym kierunku lub innych. Podobnie po Włączanie ochrony maszyn wirtualnych, nie ma żadnej różnicy między trybu failover po ponownej ochrony lub post powrotu po awarii.
Aby wyjaśnić w przepływach pracy ponownej ochrony i uniknąć pomyłek, dotyczą chronionych maszyn jako regionu Azja Wschodnia lokacji głównej i lokacji odzyskiwania maszyn jako regionu Azja południowo wschodnia. Podczas pracy w trybie failover maszyny wirtualne zostaną uruchomione w regionie południowo Azja Wschodnia. Przed możesz powrotu po awarii należy włączyć ją ponownie maszyn wirtualnych z Azja południowo-wschodnia, Azja Wschodnia do. W tym artykule opisano kroki dotyczące włączyć ją ponownie.

> [!WARNING]
> Jeśli masz [ukończone migracji](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), przenieść maszynę wirtualną do innej grupy zasobów lub usunąć maszyny wirtualnej Azure, nie Włącz ponownie ochronę lub powrotu po awarii maszyny wirtualnej.

Po zakończeniu ponownej ochrony i replikowania chronionych maszyn wirtualnych, można zainicjować trybu failover w przypadku maszyn wirtualnych, aby przywrócić je do regionu Azja Wschodnia.

Opublikuj komentarze lub pytania na końcu tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Wymagania wstępne
1. Maszyny wirtualne powinny zostać zatwierdzone.
2. W docelowej lokacji — w takim przypadku regionu Azja Wschodnia Azure powinny być dostępne i powinien mieć możliwość dostępu/tworzenia nowych zasobów w tym regionie.

## <a name="steps-to-reprotect"></a>Kroki, aby Wznów

Poniżej przedstawiono kroki, aby Włącz ponownie ochronę maszyny wirtualnej przy użyciu ustawień domyślnych.

1. W **magazynu** > **elementy replikowane**, kliknij prawym przyciskiem myszy maszynę wirtualną, która jest Failover, a następnie wybierz **ponownego włączenia ochrony**. Możesz również kliknij maszynę i wybierz **ponownego włączenia ochrony** z przyciski poleceń.

![Kliknij prawym przyciskiem myszy włączyć ją ponownie](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. W bloku, zwróć uwagę, że kierunek ochrony, **Azja południowo-wschodnia, Azja Wschodnia**, jest już wybrana.

![Włącz ponownie ochronę bloku](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotectblade.png)

3. Przegląd **grupy zasobów, sieci, magazynu i dostępności zestawy** informacje i kliknij przycisk OK. Jeśli istnieją wszystkie zasoby oznaczone (nowy), będzie można utworzyć jako część ponownej ochrony.

Zadanie ponownej ochrony najpierw będzie obsługiwał lokacji docelowej (SEA w tym przypadku) przy użyciu najnowszych danych, a po zakończeniu replikacji może przed trybu failover należy do Azja południowo-wschodnia.

### <a name="reprotect-customization"></a>Włącz ponownie ochronę dostosowania
Jeśli chcesz wybrać wyodrębniania konta magazynu lub sieci podczas ponownej ochrony, możesz to zrobić przy użyciu podanego w bloku ponownej ochrony opcję Dostosuj.

![Dostosowywanie opcji](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

Następujące właściwości he docelowej maszyny wirtualnej można dostosować podczas ponownej ochrony.

![Dostosowywanie bloku](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Właściwość |Uwagi  |
|---------|---------|
|Docelowa grupa zasobów     | Można zmienić docelowa grupa zasobów, w którym zostanie utworzona th maszyny wirtualnej. W ramach ponownej ochrony docelowa maszyna wirtualna zostanie usunięta, dlatego można wybrać nową grupę zasobów, w którym można utworzyć pracy awaryjnej post maszyny Wirtualnej         |
|Docelowy sieci wirtualnej     | Nie można zmienić sieci podczas ponownej ochrony jb. Aby zmienić sieci, ponów mapowania sieci.         |
|Magazyn docelowy     | Można zmienić konta magazynu, do której maszyna wirtualna zostanie utworzona post trybu failover.         |
|Pamięć podręczna     | Można określić konto magazynu pamięci podręcznej, która będzie używana podczas replikacji. Przejście z ustawieniami domyślnymi nowego konta magazynu pamięci podręcznej zostanie utworzony, jeśli jeszcze nie istnieje.         |
|Zestaw dostępności     |Jeśli maszyna wirtualna w Azja Wschodnia jest częścią zestawu dostępności, możesz wybrać zbiór dostępności dla docelowej maszyny wirtualnej w Azji Południowo-Wschodnia. Ustawienia domyślne znajdzie istniejącego zestawu dostępności SEA i ponownie. W trakcie dostosowywania realizowanego można określić zupełnie nowy zestaw AV.         |


### <a name="what-happens-during-reprotect"></a>Co się dzieje podczas ponownej ochrony?

Tak jak po pierwszym włączyć ochronę, poniżej przedstawiono artefaktów tworzonych, korzystając z ustawień domyślnych.
1. Konto magazynu pamięci podręcznej pobiera utworzona w regionie, Azja Wschodnia.
2. Jeśli docelowe konto magazynu (oryginalnego konta magazynu maszyny Wirtualnej Azja południowo-wschodnia) nie istnieje, zostanie utworzony nowy. Nazwa jest konto magazynu maszyny wirtualnej Azja Wschodnia sufiks z "asr".
3. Jeśli nie istnieje zestaw docelowy AV i wykryć wartości domyślne, które należy utworzyć nowy zestaw AV, a następnie zostanie utworzony jako część zadania ponownej ochrony. Jeśli dostosowano ponownej ochrony wybranego zestawu AV będzie używany.
4.

Oto lista czynności, które się zdarzyć, gdy użytkownik zainicjuje zadanie ponownej ochrony. Jest to w przypadku, gdy istnieje po stronie docelowej maszyny wirtualnej.

1. Artefakty wymagane są tworzone w ramach ponownej ochrony. Jeśli już istnieją, a następnie są używane ponownie.
2. (Azja południowo-wschodnia) po stronie docelowej maszyny wirtualnej jest najpierw wyłączony, jeśli została uruchomiona.
3. Dysk maszyny wirtualnej po stronie docelowej jest kopiowane za pomocą usługi Azure Site Recovery do kontenera obiektu blob inicjatora.
4. Następnie jest usuwany po stronie docelowej maszyny wirtualnej.
5. Inicjatora obiektu blob jest używany przez bieżącej maszyny wirtualnej (Azja Wschodnia) po stronie źródła replikacji. Dzięki temu, że są replikowane tylko różnice.
6. Istotne zmiany między dysku źródłowego i inicjatora obiektu blob są synchronizowane. Może to zająć trochę czasu.
7. Po zakończeniu zadania ponownej ochrony rozpoczyna się replikacja różnicowa tworzy punkt odzyskiwania zgodnie z zasadami.

> [!NOTE]
> Nie można włączyć ochrony na poziomie planu odzyskiwania. Może być tylko Wznów w na poziomie maszyny Wirtualnej.

Po ponownej ochrony zadania, maszyny wirtualnej przechodzą w stan chronionych.

## <a name="next-steps"></a>Następne kroki

Po wprowadzeniu chronionych stan maszyny wirtualnej można zainicjować trybu failover. Tryb failover Zamknij maszynę wirtualną w regionie Azure Azja Wschodnia i następnie utworzy i rozruchu maszyny wirtualnej regionu Azja południowo-wschodnia. Dlatego jest mała przestoju dla aplikacji. Tak wybierz czas pracy w trybie Failover, gdy aplikacja może tolerować przestoju. Zaleca się przetestowanie trybu failover maszyny wirtualnej, aby upewnić się, że jej jest powtarzający się prawidłowo, przed rozpoczęciem pracy awaryjnej.

-   [Czynności, aby zainicjować testowanie trybu failover maszyny wirtualnej](site-recovery-test-failover-to-azure.md)

-   [Czynności, aby zainicjować trybu failover maszyny wirtualnej](site-recovery-failover.md)
