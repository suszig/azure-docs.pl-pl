---
title: "Jak działa replikacja funkcji Hyper-V do platformy Azure w usłudze Site Recovery? | Microsoft Docs"
description: "Ten artykuł zawiera omówienie sposobu działania replikacji funkcji Hyper-V w usłudze Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/05/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 9adf266c6a2ac00c3aaa34e2a29aefe34abe2871
ms.lasthandoff: 04/18/2017


---
# <a name="how-does-hyper-v-replication-to-azure-work"></a>Jak działa replikacja funkcji Hyper-V do platformy Azure?

Przeczytaj ten artykuł, aby zapoznać się z architekturą i przepływami pracy replikacji funkcji Hyper-V do platformy Azure przy użyciu usługi [Azure Site Recovery](site-recovery-overview.md).

Zamieść wszelkie komentarze pod tym artykułem lub na [forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Oto co możesz replikować do platformy Azure:
- **Funkcja Hyper-V z programem VMM**: Maszyny wirtualne znajdujące się na lokalnych hostach funkcji Hyper-V zarządzanych w chmurach programu System Center Virtual Machine Manager (VMM). Na hostach może działać dowolny [obsługiwany system operacyjny](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). Replikować możesz maszyny wirtualne funkcji Hyper-V, na których działa dowolny system operacyjny gościa [obsługiwany przez funkcję Hyper-V i platformę Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).
- **Funkcja Hyper-V bez programu VMM**: Lokalne maszyny wirtualne znajdujące się na hostach funkcji Hyper-V, które nie są zarządzane w chmurach programu VMM. Na hostach może działać dowolny [obsługiwany system operacyjny](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions). Replikować możesz maszyny wirtualne funkcji Hyper-V, na których działa dowolny system operacyjny gościa [obsługiwany przez funkcję Hyper-V i platformę Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).


## <a name="architectural-components"></a>Składniki architektury

**Obszar** | **Składnik** | **Szczegóły**
--- | --- | ---
**Azure** | W przypadku platformy Azure konieczne jest posiadanie konta platformy Microsoft Azure, konta usługi Azure Storage i sieci platformy Azure. | Konta magazynu i sieci mogą być kontami opartymi na usłudze Resource Manager lub kontami klasycznymi.<br/><br/> Replikowane dane są przechowywane na koncie magazynu. W przypadku wystąpienia w lokacji lokalnej przejścia w tryb failover maszyny wirtualne platformy Azure są tworzone przy użyciu zreplikowanych danych.<br/><br/> Maszyny wirtualne platformy Azure nawiązują połączenie z siecią wirtualną platformy Azure, gdy są tworzone.
**Serwer VMM** | Hosty funkcji Hyper-V znajdujące się w chmurach programu VMM | Jeśli hosty funkcji Hyper-V są zarządzane w chmurach programu VMM, należy zarejestrować serwer VMM w magazynie usługi Recovery Services.<br/><br/> Na serwerze VMM należy zainstalować dostawcę usługi Site Recovery w celu organizowania replikacji z platformą Azure.<br/><br/> Aby skonfigurować mapowanie sieci, trzeba dysponować sieciami logicznymi i maszyn wirtualnych. Sieć maszyn wirtualnych powinna być połączona z siecią logiczną skojarzoną z chmurą.
**Host funkcji Hyper-V** | Serwery funkcji Hyper-V można wdrożyć z użyciem serwera VMM lub bez niego. | Jeśli nie ma serwera VMM, dostawca usługi Site Recovery jest instalowany na hoście w celu organizowania replikacji z usługą Site Recovery przez Internet. W przypadku użycia serwera VMM dostawca jest instalowany na nim, a nie na hoście.<br/><br/> Agent usługi Recovery Services jest instalowany na hoście w celu obsługi replikacji danych.<br/><br/> Komunikacja zarówno ze strony dostawcy, jak i agenta, jest bezpieczna i szyfrowana. Zreplikowane dane w usłudze Azure Storage również są szyfrowane.
**Maszyny wirtualne funkcji Hyper-V** | Wymagana jest co najmniej jedna maszyna wirtualna na serwerze hosta funkcji Hyper-V. | Niczego nie trzeba jawnie instalować na maszynach wirtualnych

## <a name="deployment-steps"></a>Kroki wdrażania

1. **Azure**: Skonfiguruj składniki platformy Azure. Przed rozpoczęciem wdrażania usługi Site Recovery zaleca się skonfigurowanie kont magazynu i sieci.
2. **Magazyn**: Utwórz magazyn usługi Recovery Services na potrzeby usługi Site Recovery i skonfiguruj ustawienia magazynu, co obejmuje skonfigurowanie ustawień źródła i celu, skonfigurowanie zasad replikacji i włączenie replikacji.
3. **Źródło i cel**:
    - **Hosty funkcji Hyper-V w chmurach programu VMM**: W ramach określania ustawień źródła pobierz i zainstaluj dostawcę usługi Azure Site Recovery na serwerze VMM oraz agenta usługi Azure Site Recovery na każdym hoście funkcji Hyper-V. Źródłem będzie serwer VMM. Celem jest platforma Azure.
    - Hosty funkcji Hyper-V bez programu VMM: W ramach określania ustawień źródła pobierz i zainstaluj dostawcę i agenta na każdym hoście funkcji Hyper-V. Podczas wdrażania należy zebrać hosty w lokacji funkcji Hyper-V i określić tę lokację jako źródło. Celem jest platforma Azure.

    ![Replikacja funkcji Hyper-V/programu VMM do platformy Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)
    ![Replikacja lokacji funkcji Hyper-V do platformy Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


4. **Zasady replikacji**: Utwórz zasady replikacji dla lokacji funkcji Hyper-V lub chmury programu VMM. Te zasady są stosowane do wszystkich maszyn wirtualnych znajdujących się na hostach w lokacji lub w chmurze.
5. **Włącz replikację**: Włącz replikację maszyn wirtualnych funkcji Hyper-V. Początkowa replikacja jest wykonywana zgodnie z ustawieniami zasad replikacji. Zmiany danych są śledzone i po zakończeniu początkowej replikacji rozpoczyna się replikowanie zmian różnicowych do platformy Azure. Śledzone zmiany elementu są przechowywane w pliku hrl.
6. **Testowe przełączenie w tryb failover**: Wykonaj testowe przełączenie w tryb failover, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami.

Dowiedz się więcej o wdrażaniu:
- [Wprowadzenie do replikacji maszyn wirtualnych funkcji Hyper-V do platformy Azure — z programem VMM](site-recovery-vmm-to-azure.md)
- [Get started with Hyper-V VM replication to Azure - without VMM](site-recovery-hyper-v-site-to-azure.md) (Wprowadzenie do replikacji maszyn wirtualnych funkcji Hyper-V do platformy Azure — bez programu VMM)

## <a name="hyper-v-replication-workflow"></a>Przepływ pracy replikacji funkcji Hyper-V

### <a name="enable-protection"></a>Włączanie ochrony

1. Po włączeniu ochrony dla maszyny wirtualnej funkcji Hyper-V, w witrynie Azure Portal lub środowisku lokalnym, zostanie uruchomione zadanie **Włącz ochronę**.
2. To zadanie sprawdza, czy maszyna spełnia wymagania wstępne, a następnie wywołuje metodę [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), aby skonfigurować replikację za pomocą określonych ustawień.
3. Zadanie uruchamia replikację początkową, wywołując metodę [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx), aby zainicjować pełną replikację maszyny wirtualnej i wysłać dyski wirtualne maszyny wirtualnej na platformę Azure.
4. To zadanie możesz monitorować na karcie **Zadania**.
        ![Lista zadań](media/site-recovery-hyper-v-azure-architecture/image1.png)
        ![Szczegóły zadania Włącz ochronę](media/site-recovery-hyper-v-azure-architecture/image2.png)

### <a name="initial-replication"></a>Replikacja początkowa

1. Po wyzwoleniu replikacji początkowej tworzona jest [migawka maszyny wirtualnej funkcji Hyper-V](https://technet.microsoft.com/library/dd560637.aspx).
2. Wirtualne dyski twarde są replikowane pojedynczo, dopóki wszystkie nie zostaną skopiowane na platformę Azure. Może to trochę potrwać, w zależności od rozmiaru maszyny wirtualnej i przepustowości sieci. Aby zoptymalizować użycie sieci, zobacz [Jak zarządzać użyciem przepustowości sieci przez ochronę za pomocą replikacji zasobów lokalnych do platformy Azure](https://support.microsoft.com/kb/3056159).
3. Jeśli podczas replikacji początkowej będzie miała miejsce zmiana dysku, składnik Replica Replication Tracker funkcji Hyper-V będzie śledził te zmiany jako dzienniki replikacji funkcji Hyper-V (hrl). Te pliki znajdują się w tym samym folderze co dyski. Z każdym dyskiem jest skojarzony plik hrl, który zostanie wysłany do magazynu pomocniczego.
4. Pliki migawki i dziennika zużywają zasoby dysku w trakcie replikacji początkowej.
5. Po zakończeniu replikacji początkowej migawka maszyny wirtualnej jest usuwana. Różnicowe zmiany dysku w dzienniku są synchronizowane i scalane z dyskiem nadrzędnym.


### <a name="finalize-protection"></a>Finalizowanie ochrony

1. Po zakończeniu replikacji początkowej zadanie **Finalizuj ochronę na maszynie wirtualnej** konfiguruje ustawienia sieciowe i inne ustawienia po replikacji, aby maszyna wirtualna była chroniona.
    ![Zadanie Finalizuj ochronę](media/site-recovery-hyper-v-azure-architecture/image3.png)
2. W przypadku przeprowadzania replikacji na platformę Azure może być konieczne dostosowanie ustawień maszyny wirtualnej w taki sposób, aby była gotowa do przejścia w tryb failover. W tym momencie możesz uruchomić testowe przejście w tryb failover w celu sprawdzenia, czy wszystko działa zgodnie z oczekiwaniami.

### <a name="delta-replication"></a>Replikacja różnicowa

1. Po przeprowadzeniu replikacji początkowej rozpoczynana jest synchronizacja przyrostowa zgodnie z ustawieniami replikacji.
2. Składnik Replica Replication Tracker funkcji Hyper-V śledzi zmiany na wirtualnym dysku twardym w plikach hrl. Z każdym dyskiem skonfigurowanym pod kątem replikacji jest skojarzony plik hrl. Ten dziennik jest wysyłany do konta magazynu klienta po zakończeniu replikacji początkowej. Gdy plik dziennika jest przesyłany do platformy Azure, zmiany na dysku podstawowym są śledzone w innym pliku dziennika, w tym samym katalogu.
3. Podczas replikacji początkowej i różnicowej możesz monitorować maszynę wirtualną w widoku maszyny wirtualnej. [Dowiedz się więcej](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="replication-synchronization"></a>Synchronizacja replikacji

1. Jeśli replikacja różnicowa nie powiedzie się, a pełna replikacja byłaby kosztowna pod względem przepustowości lub czasu, maszyna wirtualna jest oznaczana do ponownej synchronizacji. Jeśli na przykład pliki hrl będą zajmować 50% rozmiaru dysku, to maszyna wirtualna zostanie oznaczona do ponownej synchronizacji.
2.  Ponowna synchronizacja minimalizuje ilość wysyłanych danych dzięki obliczaniu sum kontrolnych źródłowych i docelowych maszyn wirtualnych oraz wysyłaniu tylko danych różnicowych. Ponowna synchronizacja używa algorytmu dzielenia na fragmenty o stałym bloku. Za jego pomocą pliki źródłowe i docelowe są dzielone na stałe fragmenty. Dla każdego fragmentu są generowane sumy kontrolne, które następnie są porównywane w celu określenia, które bloki ze źródła mają zostać zastosowane do celu.
3. Po ukończeniu ponownej synchronizacji replikacja różnicowa powinna zostać wznowiona. Domyślnie ponowna synchronizacja jest zaplanowana do automatycznego uruchamiania poza godzinami pracy, ale możliwe jest uruchomienie ponownej synchronizacji maszyny wirtualnej ręcznie. Na przykład możesz wznowić ponowną synchronizację, jeśli wystąpi awaria sieci lub inna awaria. W tym celu wybierz maszynę wirtualną w portalu i wybierz pozycję **Synchronizuj ponownie**.

    ![Ręczna ponowna synchronizacja](media/site-recovery-hyper-v-azure-architecture/image4.png)


### <a name="retries"></a>Ponowne próby

Jeśli wystąpi błąd replikacji, może zostać użyty wbudowany mechanizm ponawiania. Tę logikę można podzielić na dwie kategorie:

**Kategoria** | **Szczegóły**
--- | ---
**Błąd nieodwracalny** | Nie jest podejmowana próba ponowienia. Maszyna wirtualna będzie mieć stan **Krytyczny** i będzie wymagana interwencja administratora. Przykłady błędów tego typu: przerwany łańcuch dysków VHD, nieprawidłowy stan repliki maszyny wirtualnej, błędy uwierzytelniania sieci (błędy autoryzacji), błędy Nie znaleziono maszyny wirtualnej (w przypadku autonomicznych serwerów funkcji Hyper-V)
**Błędy odwracalne** | Ponowne próby są wykonywane co interwał replikacji przy użyciu wycofywania wykładniczego zwiększającego interwał ponawiania prób od początku pierwszej próby o 1, 2, 4, 8 i 10 minut. Jeśli błąd będzie się powtarzać, ponowne próby będą wykonywane co 30 minut. Są to na przykład błędy sieci, błędy małej ilości miejsca na dysku i warunki małej ilości pamięci |

## <a name="protection-and-recovery-lifecycle"></a>Cykl życia ochrony i odzyskiwania

![przepływ pracy](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Następne kroki

- [Check deployment prerequisites](site-recovery-prereq.md) (Sprawdzanie wymagań wstępnych dotyczących wdrożenia)
- Rozwiązywanie problemów:
    - [Monitorowanie i rozwiązywanie problemów z ochroną](site-recovery-monitoring-and-troubleshooting.md)
    - [Help from Microsoft support](site-recovery-monitoring-and-troubleshooting.md#reach-out-for-microsoft-support) (Pomoc techniczna od firmy Microsoft)
    - [Common errors and resolutions](site-recovery-monitoring-and-troubleshooting.md#common-azure-site-recovery-errors-and-their-resolutions) (Typowe błędy i rozwiązania)

