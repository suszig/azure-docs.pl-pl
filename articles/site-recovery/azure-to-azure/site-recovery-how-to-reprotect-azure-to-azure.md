---
title: "Jak ponownej ochrony od przejścia w tryb failover maszyn wirtualnych platformy Azure do regionu Azure podstawowego | Dokumentacja firmy Microsoft"
description: "Po pracy awaryjnej maszyn wirtualnych z jednego regionu Azure do innego można użyć usługi Azure Site Recovery do ochrony maszyn w odwrotnym kierunku. Jak kroki przeprowadzenie ponownej ochrony przed ponownie trybu failover."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 0004323fee44c1433cdd3c39fc1fa7dad965dbf7
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2017
---
# <a name="reprotect-azure-vms-back-to-the-primary-region"></a>Włącz ponownie ochronę maszyn wirtualnych platformy Azure do regionu podstawowego



>[!NOTE]
>
> Replikacja lokacji odzyskiwania maszyn wirtualnych platformy Azure (VM) jest obecnie w przeglądzie.


Gdy użytkownik [awaryjnie](../site-recovery-failover.md) maszyn wirtualnych z jednego regionu Azure do drugiego, w trybie Failover maszyny wirtualne są w stanie niechronionym. Aby przywrócić je do regionu podstawowego, należy najpierw uruchom replikowanie maszyn wirtualnych, a następnie przełączyć się ponownie. Nie ma żadnej różnicy w tryb failover w jednym kierunku lub innych. Podobnie po włączeniu replikacji maszyny Wirtualnej nie ma różnic między przełączonej po pracy w trybie failover i powrotu po awarii post.

Aby objaśniają proces zastosowania, na przykład, że przyjęto założenie, lokacja główna chronionych maszyn wirtualnych jest regionu Azja Wschodnia, czy lokacja odzyskiwania jest południowo Azja Wschodnia. Podczas pracy awaryjnej należy przełączyć maszyny wirtualne do regionu Azja Wschodnia południowo. Aby powrócić, musisz replikowanie maszyn wirtualnych z południowo Azja Wschodnia, wróć do Azja Wschodnia. W tym artykule opisano kroki niezbędne do zastosowania.

> [!WARNING]
> Jeśli ukończeniu migracji i przenieść maszynę wirtualną do innej grupy zasobów (lub usunąć maszyny wirtualnej platformy Azure) nie można przerwać ponownie.

Po zakończeniu przełączonej i replikowania maszyn wirtualnych, można zainicjować trybu failover na maszynach wirtualnych, aby przywrócić je do regionu Azja Wschodnia.

## <a name="prerequisites"></a>Wymagania wstępne
1. Maszyna wirtualna powinna zostały przekazane.
2. W docelowej lokacji (Azja Wschodnia) powinny być dostępne, i powinno być możliwe do dostępu/tworzenia nowych zasobów w tym regionie.

## <a name="reprotect"></a>Włącz ponownie ochronę

Wykonaj następujące kroki, aby Włącz ponownie ochronę maszyny Wirtualnej przy użyciu ustawień domyślnych.

1. W **magazynu** > **elementy replikowane**, kliknij prawym przyciskiem myszy maszynę Wirtualną, która przejścia w tryb failover, a następnie wybierz **ponownego włączenia ochrony**. Możesz również kliknij maszynę i wybierz **ponownego włączenia ochrony** z przyciski poleceń.

  ![Ponowne włączenie ochrony](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Sprawdź, czy kierunek replikacji **Azja południowo-wschodnia, Azja Wschodnia**, jest zaznaczone.

  ![Włącz ponownie ochronę](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotectblade.png)

3. Przegląd **grupy zasobów, sieci, magazynu i dostępności zestawy** informacje, a następnie kliknij przycisk **OK**. Jeśli istnieją wszystkie zasoby oznaczone (nowy), są one tworzone podczas zastosowania.

Spowoduje to zainicjowanie zadania przełączonej nasiona lokacji docelowej przy użyciu najnowszych danych, a po zakończeniu replikuje wystąpiły przed nie zostanie ponownie Azja południowo-wschodnia.

### <a name="reprotect-customization"></a>Włącz ponownie ochronę dostosowania
Jeśli chcesz wybrać wyodrębniania konta magazynu lub sieci podczas ponownej ochrony, możesz to zrobić za pomocą opcji Dostosuj dostępnym na stronie zastosowania.

![Dostosowywanie opcji](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

Następujące właściwości docelowej maszyny wirtualnej można dostosować podczas zastosowania.

![Dostosowywanie](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Właściwość |Uwagi  |
|---------|---------|
|Docelowa grupa zasobów     | Można zmienić docelowa grupa zasobów, w którym zostanie utworzona th maszyny wirtualnej. W ramach ponownej ochrony docelowa maszyna wirtualna zostanie usunięta, dlatego można wybrać nową grupę zasobów, w którym można utworzyć pracy awaryjnej post maszyny Wirtualnej         |
|Docelowy sieci wirtualnej     | Nie można zmienić sieci podczas zastosowania. Aby zmienić sieci, ponów mapowania sieci.         |
|Magazyn docelowy     | Można zmienić konta magazynu, do której maszyna wirtualna zostanie utworzona post trybu failover.         |
|Pamięć podręczna     | Można określić konto magazynu pamięci podręcznej, która będzie używana podczas replikacji. Przejście z ustawieniami domyślnymi nowego konta magazynu pamięci podręcznej zostanie utworzony, jeśli jeszcze nie istnieje.         |
|Zestaw dostępności     |Jeśli maszyna wirtualna w Azja Wschodnia jest częścią zestawu dostępności, możesz wybrać zbiór dostępności dla docelowej maszyny wirtualnej w Azji Południowo-Wschodnia. Ustawienia domyślne znajdzie istniejącego zestawu dostępności SEA i ponownie. W trakcie dostosowywania realizowanego można określić zupełnie nowy zestaw AV.         |


### <a name="what-happens-during-reprotect"></a>Co się dzieje podczas ponownej ochrony?

Tak jak po pierwszym włączyć ochronę, poniżej przedstawiono artefaktów tworzonych, korzystając z ustawień domyślnych.
1. Konto magazynu pamięci podręcznej pobiera utworzona w regionie, Azja Wschodnia.
2. Jeśli docelowe konto magazynu (oryginalnego konta magazynu maszyny Wirtualnej Azja południowo-wschodnia) nie istnieje, zostanie utworzony nowy. Nazwa jest konto magazynu maszyny wirtualnej Azja Wschodnia sufiks z "asr".
3. Jeśli nie istnieje zestaw docelowy AV i wykryć wartości domyślne, które należy utworzyć nowy zestaw AV, a następnie zostanie utworzony jako część zadania zastosowania. Jeśli dostosowano przełączonej wybranego zestawu AV będzie używany.


Poniżej przedstawiono listę czynności, które się zdarzyć, gdy użytkownik zainicjuje zadanie zastosowania. Jest to w przypadku, gdy istnieje po stronie docelowej maszyny wirtualnej.

1. Artefakty wymagane są tworzone w ramach ponownej ochrony. Jeśli już istnieją, a następnie są używane ponownie.
2. (Azja południowo-wschodnia) po stronie docelowej maszyny wirtualnej jest najpierw wyłączony, jeśli została uruchomiona.
3. Dysk maszyny wirtualnej po stronie docelowej jest kopiowane za pomocą usługi Azure Site Recovery do kontenera obiektu blob inicjatora.
4. Następnie jest usuwany po stronie docelowej maszyny wirtualnej.
5. Inicjatora obiektu blob jest używany przez bieżącej maszyny wirtualnej (Azja Wschodnia) po stronie źródła replikacji. Dzięki temu, że są replikowane tylko różnice.
6. Istotne zmiany między dysku źródłowego i inicjatora obiektu blob są synchronizowane. Może to zająć trochę czasu.
7. Po zakończeniu zadania przełączonej rozpoczyna się replikacja różnicowa tworzy punkt odzyskiwania zgodnie z zasadami.

> [!NOTE]
> Nie można włączyć ochrony na poziomie planu odzyskiwania. Może być tylko Wznów w na poziomie maszyny Wirtualnej.

Po przełączonej zakończy się powodzeniem, maszyny wirtualnej przechodzą w stan chronionych.

## <a name="next-steps"></a>Następne kroki

Po wprowadzeniu chronionych stan maszyny wirtualnej można zainicjować trybu failover. Tryb failover Zamknij maszynę wirtualną w regionie Azure Azja Wschodnia i następnie utworzy i rozruchu maszyny wirtualnej regionu Azja południowo-wschodnia. Dlatego jest mała przestoju dla aplikacji. Tak wybierz czas pracy w trybie Failover, gdy aplikacja może tolerować przestoju. Zaleca się, że najpierw uruchomić testowy tryb failover maszyny wirtualnej, aby upewnić się, jego jest powtarzający się prawidłowo, przed rozpoczęciem pracy awaryjnej.

-   [Czynności, aby zainicjować testowanie trybu failover maszyny wirtualnej](../site-recovery-test-failover-to-azure.md)

-   [Czynności, aby zainicjować trybu failover maszyny wirtualnej](../site-recovery-failover.md)
