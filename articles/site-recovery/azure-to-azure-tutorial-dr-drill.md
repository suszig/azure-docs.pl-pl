---
title: "Uruchom wyszczególniania odzyskiwania po awarii dla maszyn wirtualnych platformy Azure w regionie pomocniczym Azure z usługą Azure Site Recovery (wersja zapoznawcza)"
description: "Dowiedz się, jak uruchomić wyszczególniania odzyskiwania po awarii dla maszyn wirtualnych Azure w regionie pomocniczym Azure przy użyciu usługi Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 5bcd3d64714951508d984c17326e845ae4842670
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="run-a-disaster-recovery-drill-for-azure-vms-to-a-secondary-azure-region-preview"></a>Uruchom wyszczególniania odzyskiwania po awarii dla maszyn wirtualnych platformy Azure do dodatkowej regionu Azure (wersja zapoznawcza)

[Usługi Azure Site Recovery](site-recovery-overview.md) usługi przyczynia się do ciągłości i odzyskiwaniem po awarii (BCDR) odzyskiwania strategią biznesową, zatrzymując aplikacji biznesowych i uruchamiając dostępne podczas planowanych lub nieplanowanych przestojów. Usługa Site Recovery zarządza i organizuje odzyskiwania po awarii maszyn lokalnych i maszyn wirtualnych platformy Azure (maszyny wirtualne), łącznie z replikacji, trybu failover i odzyskiwania.

W tym samouczku przedstawiono sposób uruchamiania wyszczególniania odzyskiwania po awarii dla maszyny Wirtualnej platformy Azure z jednego regionu Azure na inny, test trybu failover. Przechodzenie do szczegółów weryfikuje strategii replikacji bez utraty danych lub czas przestoju i nie ma wpływu na środowisko produkcyjne. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Sprawdź wymagania wstępne
> * Uruchom test trybu failover dla jednej maszyny Wirtualnej

## <a name="prerequisites"></a>Wymagania wstępne

- Przed uruchomieniem test trybu failover, firma Microsoft zaleca, aby zweryfikować właściwości maszyny Wirtualnej, aby upewnić się, że wszystko, co jest zgodne z oczekiwaniami.  Dostęp do właściwości maszyny Wirtualnej w **elementy replikowane**. **Essentials** blok zawiera informacje o ustawieniach maszyn i stanu.
- Firma Microsoft zaleca się, że używasz oddzielnej sieci maszyny Wirtualnej platformy Azure do testowania trybu failover, a nie zostało skonfigurowane po włączeniu replikacji sieci domyślne.


## <a name="run-a-test-failover"></a>Wykonywanie próby przejścia w tryb failover

1. W **ustawienia** > **elementy replikowane**, kliknij maszynę Wirtualną **+ testowy tryb Failover** ikony.

2. W **testowy tryb Failover**, wybierz punkt odzyskiwania do użycia dla trybu failover:

   - **Najnowsze przetworzone**: awaryjnie maszyny Wirtualnej do ostatniego punktu odzyskiwania, który był przetwarzany przez usługę Site Recovery. Sygnatura czasowa jest wyświetlany. Ta opcja nie jest zużywany czas przetwarzania danych, dzięki czemu oferuje RTO niski (celu czasu odzyskiwania)
   - **Najnowsza wersja aplikacji spójne**: Ta opcja nie powiedzie się za pośrednictwem wszystkich maszyn wirtualnych do ostatniego punktu odzyskiwania zapewniających spójność aplikacji. Sygnatura czasowa jest wyświetlany.
   - **Niestandardowe**: wybrać dowolny punkt odzyskiwania.

3. Wybierz docelową Azure sieć wirtualną z którą maszyny wirtualne Azure w regionie pomocniczym zostaną połączone, po pracy awaryjnej.

4. Aby uruchomić tryb failover, kliknij przycisk **OK**. Aby śledzić postęp, kliknij maszynę Wirtualną, aby otworzyć jego właściwości. Możesz też kliknąć przycisk **testowy tryb Failover** zadania w nazwie magazynu > **ustawienia** > **zadania** > **zadania usługi Site Recovery**.
5. Po zakończeniu pracy awaryjnej, replika maszyny Wirtualnej platformy Azure jest wyświetlana w portalu Azure > **maszyn wirtualnych**. Upewnij się, że maszyna wirtualna działa, rozmiar, a połączona z odpowiednią siecią.
6. Aby usunąć maszyn wirtualnych, które zostały utworzone podczas testowania trybu failover, kliknij przycisk **oczyszczania testowy tryb failover** na elemencie replikowanych lub planu odzyskiwania. W **uwagi**, zarejestrować i zapisać wszelkie obserwacje związane z testowania trybu failover.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Tryb failover produkcji](azure-to-azure-tutorial-failover-failback.md)
