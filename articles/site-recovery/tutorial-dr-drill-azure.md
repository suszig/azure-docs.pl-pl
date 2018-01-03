---
title: "Uruchom wyszczególniania odzyskiwania po awarii dla lokalnych maszyn do platformy Azure z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej o uruchamianiu wyszczególniania odzyskiwania po awarii z lokalnego do platformy Azure z usługą Azure Site Recovery"
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/31/2017
ms.author: raynew
ms.openlocfilehash: f7dc5e2df95a64685a8b70d25e839c371d4fc2de
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/02/2018
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Uruchamianie próbnego odzyskiwania na platformie Azure

Ten samouczek pokazuje, jak uruchomić wyszczególniania odzyskiwania po awarii na maszynie lokalnej na platformie Azure, przy użyciu testowego trybu failover. Przechodzenie do szczegółów weryfikuje strategii replikacji bez utraty danych. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie sieci izolowanej do testowania trybu failover
> * Przygotowanie do połączenia z maszyny Wirtualnej Azure po trybu failover
> * Uruchom test trybu failover dla pojedynczego komputera

To jest czwarty samouczek w serii. Ten samouczek zakłada, zostały już wykonane zadania w poprzednim samouczki.

1. [Przygotowywanie platformy Azure](tutorial-prepare-azure.md)
2. [Przygotowywanie lokalnego wdrożenia oprogramowania VMware](tutorial-prepare-on-premises-vmware.md)
3. [Konfigurowanie odzyskiwania po awarii](tutorial-vmware-to-azure.md)

## <a name="verify-vm-properties"></a>Sprawdź właściwości maszyny Wirtualnej

Przed uruchomieniem test trybu failover, sprawdź właściwości maszyny Wirtualnej i upewnij się, że maszyna wirtualna jest zgodna z [wymagania dotyczące usługi Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. W **chronione elementy**, kliknij przycisk **elementy replikowane** > maszyny Wirtualnej.
2. W **elementu zreplikowane** okienku znajduje się podsumowanie informacji o maszyny Wirtualnej, stan kondycji i najnowsze dostępne punkty odzyskiwania. Kliknij przycisk **właściwości** można zobaczyć więcej szczegółów.
3. W **obliczenia i sieć**, możesz zmodyfikować nazwę platformy Azure, grupy zasobów, rozmiar docelowy [zestawu dostępności](../virtual-machines/windows/tutorial-availability-sets.md)i ustawień dysku zarządzanych.
   
      >[!NOTE]
      Powrót po awarii do lokalnych maszyn Hyper-V z maszyn wirtualnych platformy Azure z dyskami zarządzanego nie jest obecnie obsługiwany. Dyski zarządzane opcji należy używać w trybie failover tylko, jeśli planujesz migrację maszyn wirtualnych lokalnie na platformie Azure, niezawodnie je ponownie.
   
4. Można wyświetlać i modyfikować ustawień sieciowych, takich jak sieć/podsieć, w którym maszyny Wirtualnej Azure zostaną umieszczone po pracy awaryjnej i adres IP, który zostanie przypisany do niej.
5. W **dysków**, można wyświetlić informacje dotyczące systemu operacyjnego i dysków z danymi na Maszynie wirtualnej.

## <a name="run-a-test-failover-for-a-single-vm"></a>Uruchom test trybu failover dla jednej maszyny Wirtualnej

Po uruchomieniu test trybu failover, wykonywane są następujące czynności:

1. Używa do upewnij się, że wszystkie warunki wymagane do trybu failover znajdują się w miejscu Sprawdzanie wymagań wstępnych.
2. Trybu failover przetwarza dane, dzięki czemu można utworzyć maszyny Wirtualnej platformy Azure. Jeśli wybierz najnowszy punkt odzyskiwania, punkt odzyskiwania jest tworzony z danych.
3. Maszyny Wirtualnej platformy Azure jest tworzony przy użyciu danych przetworzonych w poprzednim kroku.

Uruchom test trybu failover w następujący sposób:

1. W **ustawienia** > **elementy replikowane**, kliknij maszynę Wirtualną > **+ testowy tryb Failover**.
2. Wybierz **najnowsze przetworzone** punkt odzyskiwania, w tym samouczku. To nie powiedzie się przez maszynę Wirtualną do najnowszego dostępnego punktu w czasie. Sygnatura czasowa jest wyświetlany. Ta opcja nie jest czas przetwarzania danych, dzięki czemu oferuje RTO niski (celu czasu odzyskiwania).
3. W **testowy tryb Failover**, wybierz element docelowy Azure sieci, na którą maszyny wirtualne Azure zostaną połączone po pracy awaryjnej.
4. Kliknij przycisk **OK**, aby rozpocząć tryb failover. Możesz śledzić postępy, klikając na maszynie Wirtualnej, aby otworzyć jego właściwości. Można też kliknąć przycisk **testowy tryb Failover** zadania w nazwie magazynu > **ustawienia** > **zadania** >
   **zadania usługi Site Recovery**.
5. Po zakończeniu pracy awaryjnej, replika maszyny Wirtualnej platformy Azure jest wyświetlana w portalu Azure > **maszyn wirtualnych**. Sprawdź, czy maszyna wirtualna jest odpowiedni rozmiar, który został połączony z siecią prawo, i że jest uruchomiona.
6. Teraz można nawiązać połączenia z zreplikowanej maszyny Wirtualnej na platformie Azure.
7. Aby usunąć utworzony podczas testowania trybu failover maszyny wirtualne platformy Azure, kliknij przycisk **oczyszczania testowy tryb failover** na maszynie Wirtualnej. W **uwagi**, zarejestrować i zapisać wszelkie obserwacje związane z testowania trybu failover.

W niektórych scenariuszach pracy awaryjnej wymaga dodatkowego przetwarzania pobierającej około 8-10 minut, aby zakończyć. Można zauważyć już test czasu pracy awaryjnej maszyn z systemem VMware Linux, maszyny wirtualne VMware, które nie mają umożliwia usługi DHCP i maszyny wirtualne VMware, które nie mają następujące sterowniki rozruchu: storvsc, magistralę maszyny wirtualnej, storflt, intelide, atapi.

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Uruchom tryb failover i powrotu po awarii dla lokalnych maszyn wirtualnych VMware](tutorial-vmware-to-azure-failover-failback.md).
