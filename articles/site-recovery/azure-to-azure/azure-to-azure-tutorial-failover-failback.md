---
title: "Tryb failover, a kończyć się niepowodzeniem z powrotem maszynach wirtualnych platformy Azure, replikowane do dodatkowej region platformy Azure z usługą Azure Site Recovery (wersja zapoznawcza)"
description: "Dowiedz się, jak działają i niepowodzenie wstecz replikacji maszyn wirtualnych platformy Azure w regionie pomocniczym Azure z usługą Azure Site Recovery"
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 5f37a7b3534102a06f6d5dc6fb91d3abee7c0522
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2017
---
# <a name="fail-over-and-fail-back-azure-vms-between-azure-regions-preview"></a>Tryb failover i niepowodzeniem ponownie Azure maszyn wirtualnych między regiony platformy Azure (wersja zapoznawcza)

[Usługi Azure Site Recovery](../site-recovery-overview.md) usługi przyczynia się do strategii odzyskiwania po awarii poprzez organizowanie replikacji, trybu failover i powrotu po awarii maszyn lokalnych i Azure maszynach wirtualnych (VM) i zarządzanie nimi.

W tym samouczku opisano, jak niepowodzenie za pośrednictwem jednej maszyny Wirtualnej platformy Azure w regionie pomocniczym platformy Azure. Po przez Ciebie nie powiodła się, możesz powrót po awarii do regionu podstawowego, gdy jest ona dostępna. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tryb failover maszyna wirtualna platformy Azure
> * Włącz ponownie ochronę dodatkowej maszyny Wirtualnej Azure, dzięki czemu replikowanych do regionu podstawowego
> * Nastąpił powrót po awarii dodatkowej maszyny Wirtualnej
> * Włącz ponownie ochronę głównej maszyny Wirtualnej do regionu pomocniczego

## <a name="prerequisites"></a>Wymagania wstępne

- Upewnij się, że zostały wykonane [wyszczególniania odzyskiwania po awarii](azure-to-azure-tutorial-dr-drill.md) Sprawdź wszystko działa zgodnie z oczekiwaniami.
- Sprawdź właściwości maszyny Wirtualnej, przed uruchomieniem testowania trybu failover. Maszyna wirtualna musi być zgodne z [wymagania dotyczące usługi Azure](../site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="run-a-failover-to-the-secondary-region"></a>Uruchom tryb failover w regionie pomocniczym

1. W **elementy replikowane**, wybierz maszynę Wirtualną, która ma być awaryjnie > **trybu Failover**

   ![Tryb failover](./media/azure-to-azure-tutorial-failover-failback/failover.png)

2. W **pracy awaryjnej**, wybierz pozycję **punkt odzyskiwania** do trybu failover. Można użyć jednej z następujących opcji:

   * **Najnowsze** (domyślnie): Ta opcja przetwarza wszystkie dane w usłudze Site Recovery i udostępnia najniższy cel punktu odzyskiwania (RPO).
   * **Najnowsze przetworzone**: Ta opcja przywraca maszynę wirtualną do ostatniego punktu odzyskiwania, które zostały przetworzone przez usługę Site Recovery.
   * **Niestandardowe**: Użyj tej opcji do trybu failover do danego punktu. Ta opcja jest przydatna do wykonywania test trybu failover.

3. Wybierz **Zamknij maszynę przed rozpoczęciem pracy awaryjnej** Jeśli chcesz, aby usługi Site Recovery, aby spróbować wykonać zamykania maszyn wirtualnych źródła przed wyzwolenie pracy awaryjnej. Tryb failover trwa nawet w przypadku zamknięcia nie powiedzie się.

4. Śledź postęp trybu failover w **zadania** strony.

5. Po przejściu w tryb failover Sprawdź poprawność maszyny wirtualnej po zalogowaniu się do niego. Jeśli chcesz przejść do innego punktu odzyskiwania dla maszyny wirtualnej, a następnie można użyć **zmienić punktu odzyskiwania** opcji.

6. Po uzyskaniu nieudane przez maszynę wirtualną, możesz **zatwierdzić** pracy awaryjnej.
   Zatwierdzanie powoduje usunięcie wszystkich punktów odzyskiwania dostępnych w usłudze. **Zmienić punktu odzyskiwania** opcja nie jest już dostępny.

## <a name="reprotect-the-secondary-vm"></a>Włącz ponownie ochronę dodatkowej maszyny Wirtualnej

Po pracy w trybie failover maszyny Wirtualnej musisz zacznij ponownie chronić tak, aby replikowane do regionu podstawowego.

1. Upewnij się, że maszyna wirtualna jest w **pracy awaryjnej zatwierdzone** stan i sprawdź, czy jest dostępna w regionie podstawowym i możesz utworzyć i zapisać w nim nowych zasobów.
2. W **magazynu** > **elementy replikowane**, kliknij prawym przyciskiem myszy maszynę Wirtualną, która jest Failover, a następnie wybierz **ponownego włączenia ochrony**.

   ![Kliknij prawym przyciskiem myszy, włączyć ją ponownie](./media/azure-to-azure-tutorial-failover-failback/reprotect.png)

2. Zwróć uwagę, kierunek ochrony pomocniczej do regionu podstawowego, jest już zaznaczone.
3. Przegląd **grupy zasobów, sieci, magazynu i dostępności zestawy** informacji. Wszystkie zasoby oznaczone (nowy) są tworzone w ramach operacji ponownej ochrony.
4. Kliknij przycisk **OK** do wyzwalania zadania ponownej ochrony. To zadanie nasion lokacji docelowej przy użyciu najnowszych danych. Następnie może replikowanych do regionu podstawowego. Maszyna wirtualna jest teraz w stanie chronionym.

## <a name="fail-back-to-the-primary-region"></a>Powrót po awarii do regionu podstawowego

Po maszyny wirtualne są przełączona do trybu, możesz w trybie do regionu podstawowego na potrzeby. Aby to zrobić, wykonaj [pracy awaryjnej](#run-a-failover) instrukcje.
