---
title: Uruchom test trybu failover dla replikacji funkcji Hyper-V (bez programu System Center VMM) na platformie Azure | Dokumentacja firmy Microsoft
description: "Zawiera podsumowanie kroków potrzebnych do uruchamiania test trybu failover dla maszyn wirtualnych funkcji Hyper-V replikację do platformy Azure przy użyciu usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c9a4c3ca-84a0-4668-b700-9b0046202299
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 0974b9eda2cb7e3ba54a4a0fad0a768db644caf9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="step-11-run-a-test-failover-for-hyper-v-replication-to-azure"></a>Krok 11: Uruchom test trybu failover dla replikacji funkcji Hyper-V w systemie Azure

W tym artykule opisano, jak uruchomić test trybu failover z lokalnych maszyn wirtualnych funkcji Hyper-V (nie zarządzanych przez program System Center VMM) na platformie Azure, przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) w portalu Azure.

Opublikuj komentarze i pytania w dolnej części tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Przed rozpoczęciem

Aby uruchomić test trybu failover, zalecamy Sprawdź właściwości maszyny Wirtualnej i wprowadź wymagane zmiany należy. można uzyskać dostępu do właściwości maszyny Wirtualnej w **elementy replikowane**. **Essentials** blok zawiera informacje o ustawieniach maszyn i stanu.

## <a name="managed-disk-considerations"></a>Zagadnienia dotyczące dysków zarządzanych

[Dyski zarządzane](../virtual-machines/windows/managed-disks-overview.md) uprościć zarządzanie dyskami na maszynach wirtualnych platformy Azure, zarządzając konta magazynu skojarzone z dyskami maszyny Wirtualnej. 

- Dyski zarządzane są tworzone i dołączony do maszyny Wirtualnej, tylko po przejściu w tryb failover na platformie Azure. Po włączeniu ochrony dane z lokalnych maszyn wirtualnych są replikowane do kont magazynu.
- Dyski zarządzane mogą być tworzone tylko dla maszyn wirtualnych, które są wdrażane za pomocą modelu wdrażania Menedżera zasobów.
- Powrót po awarii z platformy Azure do środowiska lokalnego funkcji Hyper-V nie jest obecnie obsługiwany dla maszyn z dyskami zarządzanych. Należy ustawić tylko **Użyj zarządzanego dysków** do **tak** Jeśli podczas wykonywania migracji tylko (trybu failover do platformy Azure bez powrotu po awarii)
- To ustawienie jest włączone, zestawy dostępności tylko w grupach zasobów, które ma **Użyj zarządzanego dysków** włączona można wybrać. Maszyny wirtualne z dyskami zarządzanych musi być w zestawach dostępności z **Użyj zarządzanego dysków** ustawioną **tak**. Jeśli to ustawienie nie jest włączone dla maszyn wirtualnych, można można wybrać tylko zestawów dostępności w grupach zasobów bez dysków zarządzanych włączone. [Dowiedz się więcej](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).
- - Jeśli konto magazynu, którego można użyć na potrzeby replikacji został zaszyfrowany przy użyciu szyfrowania usługi magazynu, dyski zarządzanego nie można utworzyć podczas pracy awaryjnej. W takim przypadku albo nie zezwalaj na używanie dysków zarządzanych lub Wyłącz ochronę dla maszyny Wirtualnej i ponownie włączyć ją pod kątem używania konta magazynu, który nie ma włączone szyfrowanie. [Dowiedz się więcej](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).

 
## <a name="network-considerations"></a>Zagadnienia dotyczące sieci
    
- Można ustawić docelowy adres IP do użycia dla maszyny Wirtualnej Azure po pracy awaryjnej. Jeśli nie podasz adresu, maszyna w trybie failover będzie używać protokołu DHCP. Jeśli ustawisz adres, który nie jest dostępny w trybie failover, przejście do trybu failover zakończy się niepowodzeniem. Ten sam docelowy adres IP może być użyty do testowania trybu failover, jeśli adres jest dostępny w testowej sieci trybu failover.
- Liczba kart sieciowych zależy od rozmiaru określonego dla docelowej maszyny wirtualnej, zgodnie z poniższym:
    - Jeśli liczba kart sieciowych w maszynie źródłowej jest mniejsza lub równa liczbie kart sieciowych dozwolonych dla rozmiaru maszyny docelowej, maszyna docelowa będzie mieć taką samą liczbę kart sieciowych jak maszyna źródłowa.
    - Jeśli liczba kart sieciowych dla źródłowej maszyny wirtualnej przekracza liczbę dozwolonych kart sieciowych dla rozmiaru maszyny docelowej, zostanie użyta maksymalna liczba kart dla rozmiaru maszyny docelowej.
    - Przykładowo, jeśli maszyna źródłowa ma dwie karty sieciowe, a rozmiar maszyny docelowej obsługuje cztery karty, maszyna docelowa będzie mieć dwie karty sieciowe. Jeśli maszyna źródłowa ma dwie karty sieciowe, ale rozmiar maszyny docelowej obsługuje tylko jedną kartę, maszyna docelowa będzie mieć tylko jedną kartę sieciową.     
- Jeśli maszyna wirtualna ma kilka kart sieciowych, wszystkie będą łączyć się z tą samą siecią.
- Jeśli maszyna wirtualna ma wiele kart sieciowych, staje się pierwszym przedstawionego na liście *domyślne* karty sieciowej na maszynie wirtualnej platformy Azure.


## <a name="view-and-manage-vm-settings"></a>Wyświetl i zarządzaj nimi ustawienia maszyny Wirtualnej

Zalecamy zweryfikowanie właściwości maszyny źródłowej przed uruchomieniem trybu failover.

1. W **chronione elementy**, kliknij przycisk **elementy replikowane**i kliknij maszynę Wirtualną.

    ![Włączanie replikacji](./media/hyper-v-site-walkthrough-test-failover/test-failover1.png)
2. W **elementu zreplikowane** okienku wyświetlane podsumowanie informacji, stan kondycji i najnowsze dostępne punkty odzyskiwania maszyny Wirtualnej. Kliknij przycisk **właściwości** można zobaczyć więcej szczegółów.

    ![Włączanie replikacji](./media/hyper-v-site-walkthrough-test-failover/test-failover2.png)
3. W **obliczenia i sieć**, można wykonywać następujące czynności:
    - Zmodyfikuj nazwę maszyny Wirtualnej platformy Azure. Nazwa musi spełniać [wymagania dotyczące usługi Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
    - Określ failover post [grupy zasobów].
    - Określ rozmiar docelowy dla maszyny Wirtualnej Azure
    - Wybierz [zestawu dostępności](../virtual-machines/windows/tutorial-availability-sets.md).
    - Określ, czy używać [dyskach zarządzanych](#managed-disk-considerations). Wybierz **tak**, jeśli chcesz dołączyć zarządzanych dyski na komputerze, na temat migracji do usługi Azure.
    - Wyświetlanie i modyfikowanie ustawień sieciowych, takich jak sieć/podsieć, w którym maszyny Wirtualnej Azure zostaną umieszczone po pracy awaryjnej i adres IP, który zostanie przypisany do niej.

    ![Włączanie replikacji](./media/hyper-v-site-walkthrough-test-failover/test-failover4.png)
4. W **dysków**, można wyświetlić informacje dotyczące systemu operacyjnego i dysków z danymi na Maszynie wirtualnej.


## <a name="run-a-test-failover"></a>Wykonywanie próby przejścia w tryb failover

Teraz uruchom test trybu failover, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami.

- Jeśli chcesz nawiązać połączenie za pomocą protokołu RDP po przejściu w tryb failover, maszynach wirtualnych platformy Azure [przygotować się do połączenia](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - Do przeprowadzenia pełnego testowania potrzebna jest kopia usług Active Directory i DNS w środowisku testowym. [Dowiedz się więcej](site-recovery-active-directory.md#test-failover-considerations).
 - Aby uzyskać pełne informacje dotyczące testowania trybu failover, przeczytaj [w tym artykule](site-recovery-test-failover-to-azure.md) artykułu.
 
 Teraz można uruchomić trybu failover:

1. Do pracy awaryjnej z jednego komputera, w **elementy replikowane**, kliknij maszynę Wirtualną > **+ testowy tryb Failover** ikony.
2. Aby przenieść plan odzyskiwania do trybu failover, w obszarze **Plany odzyskiwania** kliknij plan prawym przyciskiem myszy > **Testowy tryb failover**. Aby utworzyć plan odzyskiwania, [wykonaj te instrukcje](site-recovery-create-recovery-plans.md).
3. W **testowy tryb Failover**, wybierz sieć platformy Azure, na którą maszyny wirtualne Azure zostaną połączone po pracy awaryjnej.
4. Kliknij przycisk **OK**, aby rozpocząć tryb failover. Możesz śledzić postępy, klikając na maszynie Wirtualnej, aby otworzyć jego właściwości lub na **testowy tryb Failover** zadania w nazwie magazynu > **zadania** > **zadania usługi Site Recovery**.
5. Po zakończeniu trybu failover w pozycji Azure Portal > **Maszyny wirtualne** powinna być widoczna replika maszyny wirtualnej platformy Azure. Upewnij się, że maszyna wirtualna ma prawidłowy rozmiar, jest podłączona do odpowiedniej sieci i jest uruchomiona.
6. Jeśli przeprowadzono przygotowanie do nawiązywania połączeń po przejściu do trybu failover, powinno być możliwe nawiązanie połączenia z maszyną wirtualną platformy Azure.
7. Po zakończeniu kliknij **oczyszczania testowy tryb failover** dla planu odzyskiwania. W obszarze **Uwagi** zarejestruj i zapisz wszelkie obserwacje związane z testem pracy w trybie failover. Spowoduje to usunięcie maszyn wirtualnych utworzonych podczas testu pracy w trybie failover.



## <a name="next-steps"></a>Następne kroki

- [Dowiedz się więcej](site-recovery-failover.md) o różnych typach trybu failover i sposobu ich uruchamiania.
- [Przeczytaj informacje o awarii](site-recovery-failback-from-azure-to-hyper-v.md), aby powrócić i replikowanie maszyn wirtualnych Azure do lokalnej głównej lokacji funkcji Hyper-V.

