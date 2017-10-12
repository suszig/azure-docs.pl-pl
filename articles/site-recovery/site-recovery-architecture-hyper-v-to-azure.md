---
title: "Jak działa replikacja funkcji Hyper-V do platformy Azure w usłudze Azure Site Recovery? | Microsoft Docs"
description: "Ten artykuł zawiera omówienie składników i architektury używanych podczas replikowania lokalnych maszyn wirtualnych funkcji Hyper-V do platformy Azure za pomocą usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/23/2017
ms.author: raynew
ms.openlocfilehash: 28f775afaf72b11eec0c22f755e4dbd6a485c895
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-does-hyper-v-replication-to-azure-work-in-site-recovery"></a>Jak działa replikacja funkcji Hyper-V do platformy Azure w usłudze Site Recovery?


Ten artykuł zawiera opis składników i procesów związanych z replikacją lokalnych maszyn wirtualnych funkcji Hyper-V do platformy Azure za pomocą usługi [Azure Site Recovery](site-recovery-overview.md).

Usługa Site Recovery może replikować maszyny wirtualne funkcji Hyper-V w klastrach funkcji Hyper-V i na hostach autonomicznych, które są zarządzane za pomocą programu System Center Virtual Machine Manager (VMM) lub bez tego programu.

Zamieść wszelkie komentarze pod tym artykułem lub na [forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="architectural-components"></a>Składniki architektury

Podczas replikowania maszyn wirtualnych funkcji Hyper-V do platformy Azure zaangażowanych jest wiele składników.

**Składnik** | **Lokalizacja** | **Szczegóły**
--- | --- | ---
**Azure** | W przypadku platformy Azure konieczne jest posiadanie konta platformy Microsoft Azure, konta usługi Azure Storage i sieci platformy Azure. | Replikowane dane są przechowywane na koncie magazynu. W przypadku wystąpienia w lokacji lokalnej przejścia w tryb failover maszyny wirtualne platformy Azure są tworzone przy użyciu zreplikowanych danych.<br/><br/> Maszyny wirtualne platformy Azure nawiązują połączenie z siecią wirtualną platformy Azure, gdy są tworzone.
**Serwer VMM** | Hosty funkcji Hyper-V znajdują się w chmurach programu VMM | Jeśli hosty funkcji Hyper-V są zarządzane w chmurach programu VMM, należy zarejestrować serwer VMM w magazynie usługi Recovery Services.<br/><br/> Na serwerze VMM należy zainstalować dostawcę usługi Site Recovery w celu organizowania replikacji z platformą Azure.<br/><br/> Aby skonfigurować mapowanie sieci, trzeba dysponować sieciami logicznymi i maszyn wirtualnych. Sieć maszyn wirtualnych powinna być połączona z siecią logiczną skojarzoną z chmurą.
**Host funkcji Hyper-V** | Hosty i klastry funkcji Hyper-V można wdrożyć z użyciem serwera VMM lub bez niego. | Jeśli nie ma serwera VMM, dostawca usługi Site Recovery jest instalowany na hoście w celu organizowania replikacji z usługą Site Recovery przez Internet. W przypadku użycia serwera VMM dostawca jest instalowany na nim, a nie na hoście.<br/><br/> Agent usługi Recovery Services jest instalowany na hoście w celu obsługi replikacji danych.<br/><br/> Komunikacja zarówno ze strony dostawcy, jak i agenta, jest bezpieczna i szyfrowana. Zreplikowane dane w usłudze Azure Storage również są szyfrowane.
**Maszyny wirtualne funkcji Hyper-V** | Wymagana jest co najmniej jedna maszyna wirtualna na serwerze hosta funkcji Hyper-V. | Niczego nie trzeba jawnie instalować na maszynach wirtualnych.

Dowiedz się więcej o wymaganiach wstępnych dotyczących wdrożenia i wymaganiach dla każdego z tych składników z [macierzy obsługi](site-recovery-support-matrix-to-azure.md).

**Rysunek 1: Replikacja z lokacji funkcji Hyper-V do platformy Azure**

![Składniki](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Rysunek 2: Replikacja z funkcji Hyper-V w chmurach programu VMM do platformy Azure**

![Składniki](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)


## <a name="replication-process"></a>Proces replikacji

**Rysunek 3: Proces replikacji i odzyskiwania w przypadku replikacji funkcji Hyper-V do platformy Azure**

![przepływ pracy](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

### <a name="enable-protection"></a>Włączanie ochrony

1. Po włączeniu ochrony dla maszyny wirtualnej funkcji Hyper-V, w witrynie Azure Portal lub środowisku lokalnym, zostanie uruchomione zadanie **Włącz ochronę**.
2. To zadanie sprawdza, czy maszyna spełnia wymagania wstępne, a następnie wywołuje metodę [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), aby skonfigurować replikację za pomocą określonych ustawień.
3. Zadanie uruchamia replikację początkową, wywołując metodę [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx), aby zainicjować pełną replikację maszyny wirtualnej i wysłać dyski wirtualne maszyny wirtualnej na platformę Azure.
4. To zadanie możesz monitorować na karcie **Zadania**.      ![Lista zadań](media/site-recovery-hyper-v-azure-architecture/image1.png) ![Szczegóły zadania Włącz ochronę](media/site-recovery-hyper-v-azure-architecture/image2.png)

### <a name="replicate-the-initial-data"></a>Replikowanie danych początkowych

1. Po wyzwoleniu replikacji początkowej tworzona jest [migawka maszyny wirtualnej funkcji Hyper-V](https://technet.microsoft.com/library/dd560637.aspx).
2. Wirtualne dyski twarde są replikowane pojedynczo, dopóki wszystkie nie zostaną skopiowane na platformę Azure. Może to trochę potrwać, w zależności od rozmiaru maszyny wirtualnej i przepustowości sieci. Aby zoptymalizować użycie sieci, zobacz [Jak zarządzać użyciem przepustowości sieci przez ochronę za pomocą replikacji zasobów lokalnych do platformy Azure](https://support.microsoft.com/kb/3056159).
3. Jeśli podczas replikacji początkowej będzie miała miejsce zmiana dysku, składnik Replica Replication Tracker funkcji Hyper-V będzie śledził te zmiany jako dzienniki replikacji funkcji Hyper-V (hrl). Te pliki znajdują się w tym samym folderze co dyski. Z każdym dyskiem jest skojarzony plik hrl, który zostanie wysłany do magazynu pomocniczego.
4. Pliki migawki i dziennika zużywają zasoby dysku w trakcie replikacji początkowej.
5. Po zakończeniu replikacji początkowej migawka maszyny wirtualnej jest usuwana. Różnicowe zmiany dysku w dzienniku są synchronizowane i scalane z dyskiem nadrzędnym.


### <a name="finalize-protection"></a>Finalizowanie ochrony

1. Po zakończeniu replikacji początkowej zadanie **Finalizuj ochronę na maszynie wirtualnej** konfiguruje ustawienia sieciowe i inne ustawienia po replikacji, aby maszyna wirtualna była chroniona.
    ![Zadanie Finalizuj ochronę](media/site-recovery-hyper-v-azure-architecture/image3.png)
2. W przypadku przeprowadzania replikacji na platformę Azure może być konieczne dostosowanie ustawień maszyny wirtualnej w taki sposób, aby była gotowa do przejścia w tryb failover. W tym momencie możesz uruchomić testowe przejście w tryb failover w celu sprawdzenia, czy wszystko działa zgodnie z oczekiwaniami.

### <a name="replicate-the-delta"></a>Replikowanie danych różnicowych

1. Po przeprowadzeniu replikacji początkowej rozpoczynana jest synchronizacja przyrostowa zgodnie z ustawieniami replikacji.
2. Składnik Replica Replication Tracker funkcji Hyper-V śledzi zmiany na wirtualnym dysku twardym w plikach hrl. Z każdym dyskiem skonfigurowanym pod kątem replikacji jest skojarzony plik hrl. Ten dziennik jest wysyłany do konta magazynu klienta po zakończeniu replikacji początkowej. Gdy plik dziennika jest przesyłany do platformy Azure, zmiany na dysku podstawowym są śledzone w innym pliku dziennika, w tym samym katalogu.
3. Podczas replikacji początkowej i różnicowej możesz monitorować maszynę wirtualną w widoku maszyny wirtualnej. [Dowiedz się więcej](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="synchronize-replication"></a>Synchronizowanie replikacji

1. Jeśli replikacja różnicowa nie powiedzie się, a pełna replikacja byłaby kosztowna pod względem przepustowości lub czasu, maszyna wirtualna jest oznaczana do ponownej synchronizacji. Jeśli na przykład pliki hrl będą zajmować 50% rozmiaru dysku, to maszyna wirtualna zostanie oznaczona do ponownej synchronizacji.
2.  Ponowna synchronizacja minimalizuje ilość wysyłanych danych dzięki obliczaniu sum kontrolnych źródłowych i docelowych maszyn wirtualnych oraz wysyłaniu tylko danych różnicowych. Ponowna synchronizacja używa algorytmu dzielenia na fragmenty o stałym bloku. Za jego pomocą pliki źródłowe i docelowe są dzielone na stałe fragmenty. Dla każdego fragmentu są generowane sumy kontrolne, które następnie są porównywane w celu określenia, które bloki ze źródła mają zostać zastosowane do celu.
3. Po ukończeniu ponownej synchronizacji replikacja różnicowa powinna zostać wznowiona. Domyślnie ponowna synchronizacja jest zaplanowana do automatycznego uruchamiania poza godzinami pracy, ale możliwe jest uruchomienie ponownej synchronizacji maszyny wirtualnej ręcznie. Na przykład możesz wznowić ponowną synchronizację, jeśli wystąpi awaria sieci lub inna awaria. W tym celu wybierz maszynę wirtualną w portalu i wybierz pozycję **Synchronizuj ponownie**.

    ![Ręczna ponowna synchronizacja](media/site-recovery-hyper-v-azure-architecture/image4.png)


### <a name="retry-logic"></a>Logika ponowień

Jeśli wystąpi błąd replikacji, może zostać użyty wbudowany mechanizm ponawiania. Tę logikę można podzielić na dwie kategorie:

**Kategoria** | **Szczegóły**
--- | ---
**Błąd nieodwracalny** | Nie jest podejmowana próba ponowienia. Maszyna wirtualna będzie mieć stan **Krytyczny** i będzie wymagana interwencja administratora. Przykłady błędów tego typu: przerwany łańcuch dysków VHD, nieprawidłowy stan repliki maszyny wirtualnej, błędy uwierzytelniania sieci (błędy autoryzacji), błędy Nie znaleziono maszyny wirtualnej (w przypadku autonomicznych serwerów funkcji Hyper-V)
**Błędy odwracalne** | Ponowne próby są wykonywane co interwał replikacji przy użyciu wycofywania wykładniczego zwiększającego interwał ponawiania prób od początku pierwszej próby o 1, 2, 4, 8 i 10 minut. Jeśli błąd będzie się powtarzać, ponowne próby będą wykonywane co 30 minut. Są to na przykład błędy sieci, błędy małej ilości miejsca na dysku i warunki małej ilości pamięci |



## <a name="failover-and-failback-process"></a>Proces pracy w trybie failover i podczas powrotu po awarii

1. Planowane lub nieplanowane przejście w tryb [failover](site-recovery-failover.md) można uruchomić z maszyn wirtualnych funkcji Hyper-V do platformy Azure. Jeśli zostanie uruchomione planowane przejście w tryb failover, źródłowe maszyny wirtualne zostaną wyłączone w celu zapewnienia, że nie będzie miała miejsca utrata danych.
2. W tryb failover można przełączyć pojedynczą maszynę lub można utworzyć [plany odzyskiwania](site-recovery-create-recovery-plans.md), aby zarządzać trybem failover na wielu maszynach.
4. Po uruchomieniu trybu failover powinno być można zobaczyć utworzone repliki maszyn wirtualnych na platformie Azure. Jeśli jest to wymagane, do maszyny wirtualnej można przypisać publiczny adres IP.
5. Następnie następuje zatwierdzenie trybu failover, aby można było rozpocząć uzyskiwanie dostępu do obciążenia z poziomu repliki maszyny wirtualnej platformy Azure.
6. Po ponownym udostępnieniu lokalnej lokacji głównej można do niej [powrócić po awarii](site-recovery-failback-from-azure-to-hyper-v.md). Planowane przejście w tryb failover jest rozpoczynane z platformy Azure do lokacji głównej. W przypadku planowanego przejścia w tryb failover można wybrać powrót po awarii do tej samej maszyny wirtualnej lub do lokalizacji alternatywnej i zsynchronizować zmiany między platformą Azure i lokacją lokalną, aby zapobiec utracie danych. Po utworzeniu lokalnych maszyn wirtualnych należy zatwierdzić tryb failover.




## <a name="next-steps"></a>Następne kroki

Zapoznaj się z [macierzą obsługi](site-recovery-support-matrix-to-azure.md)
