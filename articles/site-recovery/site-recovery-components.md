---
title: "W jaki sposób działa usługa Site Recovery? | Microsoft Docs"
description: "Ten artykuł zawiera omówienie architektury usługi Site Recovery"
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
ms.date: 03/14/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 4674985363bc1267449e018ab15a53757a8fd32d
ms.lasthandoff: 03/15/2017


---
# <a name="how-does-azure-site-recovery-work"></a>W jaki sposób działa usługa Azure Site Recovery?

W tym artykule opisano podstawową architekturę usługi [Azure Site Recovery](site-recovery-overview.md) i składniki, dzięki którym działa.

Zamieść wszelkie komentarze pod tym artykułem lub na [forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="replicate-to-azure"></a>Replikacja do platformy Azure

Oto co możesz replikować do platformy Azure:

- **VMware**: Lokalne maszyny wirtualne programu VMware działające na [obsługiwanym hoście](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). Replikować możesz maszyny wirtualne programu VMware, na których działają [obsługiwane systemy operacyjne](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)
- **Hyper-V**: Lokalne maszyny wirtualne funkcji Hyper-V działające na [obsługiwanych hostach](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
- **Maszyny fizyczne**: Lokalne serwery fizyczne z systemem Windows lub Linux działające w [obsługiwanych systemach operacyjnych](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions). Replikować możesz maszyny wirtualne funkcji Hyper-V, na których działa dowolny system operacyjny gościa [obsługiwany przez funkcję Hyper-V i platformę Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).

## <a name="vmware-to-azure"></a>Z programu VMware do platformy Azure

Poniżej przedstawiono wymagania dotyczące replikacji maszyn wirtualnych programu VMware do platformy Azure.

Obszar | Składnik | Szczegóły
--- | --- | ---
**Azure** | W przypadku platformy Azure konieczne jest posiadanie konta platformy Azure, konta usługi Azure Storage i sieci platformy Azure. | Konta magazynu i sieci mogą być kontami usługi Resource Manager lub kontami klasycznymi.<br/><br/>  Replikowane dane są przechowywane na koncie magazynu. W przypadku wystąpienia w lokacji lokalnej przejścia w tryb failover maszyny wirtualne platformy Azure są tworzone przy użyciu zreplikowanych danych. Maszyny wirtualne platformy Azure nawiązują połączenie z siecią wirtualną platformy Azure, gdy są tworzone.
**Serwer konfiguracji** | Wszystkie lokalne składniki (serwer konfiguracji, serwer przetwarzania i główny serwer docelowy) działają na jednym serwerze zarządzania (maszynie wirtualnej programu VMware) | Serwer konfiguracji służy do koordynowania komunikacji między środowiskiem lokalnym i platformą Azure oraz do zarządzania replikacją danych.
 **Serwer przetwarzania**:  | Domyślnie instalowany na serwerze konfiguracji. | Działa jako brama replikacji. Odbiera dane replikacji, optymalizuje je przy użyciu pamięci podręcznej, kompresji i szyfrowania, a następnie wysyła je do usługi Azure Storage.<br/><br/> Serwer przetwarzania obsługuje także instalację wypychaną usługi Mobility na chronionych maszynach i przeprowadza automatyczne odnajdywanie maszyn wirtualnych programu VMware.<br/><br/> Wraz z rozwojem wdrożenia możliwe będzie dodawanie dodatkowych oddzielnych dedykowanych serwerów przetwarzania w celu obsługi coraz większej ilości danych związanych z ruchem replikacji.
 **Główny serwer docelowy** | Instalowany domyślnie na lokalnym serwerze konfiguracji. | Służy do obsługi replikacji danych podczas powrotu po awarii z platformy Azure.<br/><br/> W przypadku dużego ruchu powrotu po awarii można wdrożyć oddzielny główny serwer docelowy na potrzeby obsługi powrotu po awarii.
**Serwery VMware** | Maszyny wirtualne programu VMware są hostowane na serwerach vSphere ESXi. Zalecamy użycie serwera vCenter do zarządzania hostami. | Serwery VMware są dodawane do magazynu usługi Recovery Services.<br/><br/> I
**Zreplikowane maszyny** | Usługa Mobility zostanie zainstalowana na każdej maszynie wirtualnej VMware, która ma być replikowana. Można ją zainstalować ręcznie na poszczególnych maszynach lub za pośrednictwem instalacji wypychanej z serwera przetwarzania.

**Rysunek 1: Składniki replikacji z oprogramowania VMware na platformę Azure**

![Składniki](./media/site-recovery-components/arch-enhanced.png)

### <a name="replication-process"></a>Proces replikacji

1. Skonfiguruj wdrożenie, w tym składniki platformy Azure i magazyn usługi Recovery Services. W magazynie należy określić źródło i cel replikacji, skonfigurować serwer konfiguracji, dodać serwery VMware, utworzyć zasady replikacji, wdrożyć usługę Mobility, włączyć replikację i uruchomić test trybu failover.
2.  Maszyny zaczynają się replikować zgodnie z zasadami replikacji. Początkowa kopia danych jest replikowana do usługi Azure Storage.
4. Po zakończeniu początkowej replikacji rozpoczyna się replikowanie zmian różnicowych do platformy Azure. Śledzone zmiany dla maszyny są przechowywane w pliku hrl.
    - Na potrzeby zarządzania replikacją replikowane maszyny komunikują się z serwerem konfiguracji na porcie HTTPS 443 dla danych przychodzących.
    - Replikowane maszyny wysyłają dane replikacji do serwera przetwarzania na porcie HTTPS 9443 dla danych przychodzących (można go skonfigurować).
    - Serwer konfiguracji synchronizuje replikację z platformą Azure za pośrednictwem portu HTTPS 443 dla danych wychodzących.
    - Serwer przetwarzania odbiera dane z maszyn źródłowych, optymalizuje je i szyfruje, a następnie wysyła do usługi Azure Storage za pośrednictwem portu 443 dla danych wychodzących.
    - Jeśli włączono spójność wielu maszyn wirtualnych, maszyny z grupy replikacji komunikują się między sobą na porcie 20004. Funkcja spójności wielu maszyn wirtualnych jest używana, jeśli wiele maszyn zostanie połączonych w grupę replikacji, która współużytkuje punkty odzyskiwania spójne na poziomie awarii i aplikacji. Jest to przydatne, jeśli maszyny obsługują to samo obciążenie i muszą być spójne.
5. Ruch jest replikowany do publicznych punktów końcowych usługi Azure Storage za pośrednictwem Internetu. Alternatywnie można użyć [publicznej komunikacji równorzędnej](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-circuit-peerings#public-peering) usługi Azure ExpressRoute. Replikowanie ruchu za pośrednictwem sieci VPN typu lokacja-lokacja z lokacji lokalnej platformy Azure nie jest obsługiwane.

**Rysunek 2: Replikacja z oprogramowania VMware na platformę Azure**

![Rozszerzone](./media/site-recovery-components/v2a-architecture-henry.png)

### <a name="failover-and-failback"></a>Praca w trybie failover i powrót po awarii

1. Po upewnieniu się, że testowe przełączenie w tryb failover działa zgodnie z oczekiwaniami, możesz uruchamiać niezaplanowane przełączenia w tryb failover na platformę Azure zgodnie z potrzebami. Planowane przejście w tryb failover nie jest obsługiwane.
2. W tryb failover możesz przełączyć pojedynczą maszynę lub możesz utworzyć [plany odzyskiwania](site-recovery-create-recovery-plans.md), aby przełączyć wiele maszyn wirtualnych.
3. Po przejściu w tryb failover na platformie Azure zostaną utworzone repliki maszyn wirtualnych. Po zatwierdzeniu trybu failover można rozpocząć uzyskiwanie dostępu do obciążenia z poziomu repliki maszyny wirtualnej platformy Azure.
4. Po ponownym udostępnieniu głównej lokacji lokalnej można do niej powrócić po awarii. Należy skonfigurować infrastrukturę powrotu po awarii, uruchomić replikację maszyny z lokacji dodatkowej do lokacji głównej i uruchomić nieplanowane przejście w tryb failover z lokacji dodatkowej. Po zatwierdzeniu pracy w trybie failover dane będą znowu dostępne lokalnie i konieczne będzie ponowne włączenie replikacji do platformy Azure. [Dowiedz się więcej](site-recovery-failback-azure-to-vmware.md)

Istnieje kilka wymagań powrotu po awarii:


- **Tymczasowy serwer przetwarzania na platformie Azure**: jeśli chcesz powrócić po awarii z platformy Azure po przejściu w tryb failover, konieczne będzie skonfigurowanie maszyny wirtualnej platformy Azure jako serwera przetwarzania do obsługi replikacji z platformy Azure. Po zakończeniu powrotu po awarii można usunąć tę maszynę wirtualną.
- **Połączenie VPN**: na potrzeby powrotu po awarii będzie potrzebne połączenie VPN (lub usługa Azure ExpressRoute) skonfigurowane z sieci platformy Azure do lokacji lokalnej.
- **Oddzielny lokalny główny serwer docelowy**: lokalny główny serwer docelowy obsługuje powrót po awarii. Główny serwer docelowy jest instalowany domyślnie na serwerze zarządzania, ale w przypadku powrotu po awarii większych ilości ruchu należy skonfigurować oddzielny lokalny główny serwer docelowy.
- **Zasady powrotu po awarii**: aby móc przeprowadzić ponowną replikację do lokacji lokalnej, należy utworzyć zasady powrotu po awarii. Są one tworzone automatycznie podczas tworzenia zasad replikacji.

**Rysunek 3: Powrót po awarii między oprogramowaniem VMware i serwerem fizycznym**

![Powrót po awarii](./media/site-recovery-components/enhanced-failback.png)

## <a name="physical-to-azure"></a>Serwer fizyczny do platformy Azure

W przypadku replikowania serwerów lokalnych do platformy Azure używane są również te same składniki i procesy, co podczas replikowania [z programu VMware do platformy Azure](#vmware-replication-to-azure), ale należy zwrócić uwagę na poniższe różnice:

- Jako serwera konfiguracji możesz użyć serwera fizycznego zamiast maszyny wirtualnej programu VMware.
- Musisz mieć lokalną infrastrukturę VMware na potrzeby powrotu po awarii. Nie można powracać po awarii do maszyny fizycznej.

## <a name="hyper-v-to-azure"></a>Z funkcji Hyper-V do platformy Azure

Poniżej przedstawiono wymagania dotyczące replikowania maszyn wirtualnych funkcji Hyper-V do platformy Azure.

**Obszar** | **Składnik** | **Szczegóły**
--- | --- | ---
**Azure** | W przypadku platformy Azure konieczne jest posiadanie konta platformy Microsoft Azure, konta usługi Azure Storage i sieci platformy Azure. | Konta magazynu i sieci mogą być kontami opartymi na usłudze Resource Manager lub kontami klasycznymi.<br/><br/> Replikowane dane są przechowywane na koncie magazynu. W przypadku wystąpienia w lokacji lokalnej przejścia w tryb failover maszyny wirtualne platformy Azure są tworzone przy użyciu zreplikowanych danych.<br/><br/> Maszyny wirtualne platformy Azure nawiązują połączenie z siecią wirtualną platformy Azure, gdy są tworzone.
**Serwer VMM** | Hosty funkcji Hyper-V znajdujące się w chmurach programu VMM | Jeśli hosty funkcji Hyper-V są zarządzane w chmurach programu VMM, należy zarejestrować serwer VMM w magazynie usługi Recovery Services.<br/><br/> Na serwerze VMM należy zainstalować dostawcę usługi Site Recovery w celu organizowania replikacji z platformą Azure.<br/><br/> Aby skonfigurować mapowanie sieci, trzeba dysponować sieciami logicznymi i maszyn wirtualnych. Sieć maszyn wirtualnych powinna być połączona z siecią logiczną skojarzoną z chmurą.
**Host funkcji Hyper-V** | Serwery funkcji Hyper-V można wdrożyć z użyciem serwera VMM lub bez niego. | Jeśli nie ma serwera VMM, dostawca usługi Site Recovery jest instalowany na hoście w celu organizowania replikacji z usługą Site Recovery przez Internet. W przypadku użycia serwera VMM dostawca jest instalowany na nim, a nie na hoście.<br/><br/> Agent usługi Recovery Services jest instalowany na hoście w celu obsługi replikacji danych.<br/><br/> Komunikacja zarówno ze strony dostawcy, jak i agenta, jest bezpieczna i szyfrowana. Zreplikowane dane w usłudze Azure Storage również są szyfrowane.
**Maszyny wirtualne funkcji Hyper-V** | Wymagana jest co najmniej jedna maszyna wirtualna na serwerze hosta funkcji Hyper-V. | Niczego nie trzeba jawnie instalować na maszynach wirtualnych


### <a name="replication-process"></a>Proces replikacji

1. Należy skonfigurować składniki platformy Azure. Przed rozpoczęciem wdrażania usługi Site Recovery zaleca się skonfigurowanie kont magazynu i sieci.
2. Należy utworzyć magazyn usługi Replication Services na potrzeby usługi Site Recovery i skonfigurować ustawienia magazynu, w tym następujące elementy:
    - Ustawienia źródła i celu. Jeśli hosty funkcji Hyper-V nie są zarządzane w chmurze programu VMM, dla celu utwórz kontener lokacji funkcji Hyper-V i dodaj do niego hosty funkcji Hyper-V. Jeśli hosty funkcji Hyper-V są zarządzane w programie VMM, źródłem jest chmura programu VMM. Celem jest platforma Azure.
    - Instalacja dostawcy usługi Azure Site Recovery i agenta usługi Microsoft Azure Recovery Services. Jeśli korzystasz z programu VMM, dostawca zostanie zainstalowany w tym programie, a agent na każdym hoście funkcji Hyper-V. Jeśli nie masz programu VMM, zarówno dostawca, jak i agent są instalowane na każdym hoście.
    - Należy utworzyć zasady replikacji dla lokacji funkcji Hyper-V lub chmury programu VMM. Te zasady są stosowane do wszystkich maszyn wirtualnych znajdujących się na hostach w lokacji lub w chmurze.
    - Należy włączyć replikację maszyn wirtualnych funkcji Hyper-V. Początkowa replikacja jest wykonywana zgodnie z ustawieniami zasad replikacji.
4. Zmiany danych są śledzone i po zakończeniu początkowej replikacji rozpoczyna się replikowanie zmian różnicowych do platformy Azure. Śledzone zmiany elementu są przechowywane w pliku hrl.
5. Należy uruchomić testowanie trybu failover, aby upewnić się, że wszystkie funkcje działają.

### <a name="failover-and-failback-process"></a>Proces pracy w trybie failover i podczas powrotu po awarii

1. Planowane lub nieplanowane przejście w tryb [failover](site-recovery-failover.md) można uruchomić z maszyn wirtualnych funkcji Hyper-V do platformy Azure. Jeśli zostanie uruchomione planowane przejście w tryb failover, źródłowe maszyny wirtualne zostaną wyłączone w celu zapewnienia, że nie będzie miała miejsca utrata danych.
2. W tryb failover można przełączyć pojedynczą maszynę lub można utworzyć [plany odzyskiwania](site-recovery-create-recovery-plans.md), aby zarządzać trybem failover na wielu maszynach.
4. Po uruchomieniu trybu failover powinno być można zobaczyć utworzone repliki maszyn wirtualnych na platformie Azure. Jeśli jest to wymagane, do maszyny wirtualnej można przypisać publiczny adres IP.
5. Następnie następuje zatwierdzenie trybu failover, aby można było rozpocząć uzyskiwanie dostępu do obciążenia z poziomu repliki maszyny wirtualnej platformy Azure.
6. Po ponownym udostępnieniu lokalnej lokacji głównej można do niej [powrócić po awarii](site-recovery-failback-from-azure-to-hyper-v.md). Planowane przejście w tryb failover jest rozpoczynane z platformy Azure do lokacji głównej. W przypadku planowanego przejścia w tryb failover można wybrać powrót po awarii do tej samej maszyny wirtualnej lub do lokalizacji alternatywnej i zsynchronizować zmiany między platformą Azure i lokacją lokalną, aby zapobiec utracie danych. Po utworzeniu lokalnych maszyn wirtualnych należy zatwierdzić tryb failover.

**Rysunek 4: Replikacja z lokacji funkcji Hyper-V do platformy Azure**

![Składniki](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Rysunek 5: Replikacja z funkcji Hyper-V w chmurach programu VMM do platformy Azure**

![Składniki](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)


## <a name="replicate-to-a-secondary-site"></a>Replikacja do lokacji dodatkowej

Oto co możesz replikować do lokacji dodatkowej:

- **VMware**: Lokalne maszyny wirtualne programu VMware działające na [obsługiwanym hoście](site-recovery-support-matrix-to-sec-site.md#on-premises-servers). Replikować możesz maszyny wirtualne programu VMware, na których działają [obsługiwane systemy operacyjne](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions)
- **Maszyny fizyczne**: Lokalne serwery fizyczne z systemem Windows lub Linux działające w [obsługiwanych systemach operacyjnych](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions).
- **Hyper-V**: Lokalne maszyny wirtualne funkcji Hyper-V działające na [obsługiwanych hostach funkcji Hyper-V](site-recovery-support-matrix-to-sec-site.md#on-premises-servers) zarządzanych w chmurach programu VMM. ([Obsługiwane hosty](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers)). Replikować możesz maszyny wirtualne funkcji Hyper-V, na których działa dowolny system operacyjny gościa [obsługiwany przez funkcję Hyper-V i platformę Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).


## <a name="vmwarephysical-to-a-secondary-site"></a>Replikacja z programu VMware/serwera fizycznego do lokacji dodatkowej

Serwery fizyczne lub maszyny wirtualne VMware są replikowane do lokacji dodatkowej przy użyciu programu InMage Scout.

### <a name="components"></a>Składniki

**Obszar** | **Składnik** | **Szczegóły**
--- | --- | ---
**Azure** | InMage Scout. | Aby uzyskać program InMage Scout, konieczna jest subskrypcja platformy Azure.<br/><br/> Po utworzeniu magazynu usługi Recovery Services należy pobrać program InMage Scout i zainstalować najnowsze aktualizacje w celu skonfigurowania wdrożenia.
**Serwer przetwarzania** | Znajduje się w lokacji głównej | Serwer przetwarzania jest wdrażany w celu obsługi pamięci podręcznej, kompresji i optymalizacji danych.<br/><br/> Umożliwia on również obsługę instalacji wypychanej programu Unified Agent na maszynach, które chcesz chronić.
**Serwer konfiguracji** | Znajduje się w lokacji dodatkowej | Serwer konfiguracji umożliwia konfigurowanie i monitorowanie wdrożenia oraz zarządzanie nim za pomocą witryny sieci Web zarządzania lub konsoli vContinuum.
**Serwer vContinuum** | Opcjonalny. Instalowany w tej samej lokalizacji co serwer konfiguracji. | Zapewnia on konsolę do monitorowania chronionego środowiska i zarządzania nim.
**Główny serwer docelowy** | Znajduje się w lokacji dodatkowej | Na głównym serwerze docelowym przechowywane są zreplikowane dane. Odbiera on dane z serwera przetwarzania i tworzy maszynę repliki w lokacji dodatkowej, a także znajdują się na nim punkty przechowywania danych.<br/><br/> Liczba potrzebnych głównych serwerów docelowych zależy od liczby chronionych maszyn.<br/><br/> Aby powrócić po awarii do lokacji głównej, konieczne jest również posiadanie w tej sieci głównego serwera docelowego. Na tym serwerze jest instalowany program Unified Agent.
**Program VMware ESX/ESXi i serwer vCenter** |  Maszyny wirtualne są hostowane na hostach ESX/ESXi. Hosty są zarządzane za pomocą serwera vCenter | Aby móc replikować maszyny wirtualne VMware, konieczne jest posiadanie infrastruktury VMware.
**Maszyny wirtualne/serwery fizyczne** |  Program Unified Agent zainstalowany na przeznaczonych do replikowania maszynach wirtualnych programu VMware i serwerach fizycznych. | Agent działa jako dostawca komunikacji między wszystkimi składnikami.


### <a name="replication-process"></a>Proces replikacji

1. Następnie należy skonfigurować serwery składników w każdej lokacji (konfiguracja, przetwarzanie, główny docelowy) oraz zainstalować program Unified Agent na maszynach, które mają zostać zreplikowane.
2. Po początkowej replikacji agent na każdej maszynie wysyła zmiany replikacji przyrostowej na serwer przetwarzania.
3. Serwer przetwarzania optymalizuje dane i transferuje je na główny serwer docelowy w lokacji dodatkowej. Serwer konfiguracji zarządza procesem replikacji.

**Rysunek 6: Replikacja z programu VMware do programu VMware**

![Z programu VMware do programu VMware](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="hyper-v-to-a-secondary-site"></a>Z funkcji Hyper-V do lokacji dodatkowej

Poniżej przedstawiono wymagania dotyczące replikowania maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej.


**Obszar** | **Składnik** | **Szczegóły**
--- | --- | ---
**Azure** | Potrzebujesz konta platformy Microsoft Azure. |
**Serwer VMM** | Zalecamy, aby zarówno w lokacji głównej, jak i dodatkowej znajdował się jeden serwer VMM | Oba serwery VMM powinny być połączone z Internetem.<br/><br/> Każdy serwer powinien mieć co najmniej jedną chmurę prywatną programu VMM z ustawionym profilem możliwości funkcji Hyper-V.<br/><br/> Zainstaluj dostawcę usługi Azure Site Recovery na serwerze VMM. Dostawca koordynuje i organizuje replikację za pomocą usługi Site Recovery przez Internet. Komunikacja między dostawcą i platformą Azure jest bezpieczna i szyfrowana.
**Serwer funkcji Hyper-V** |  Co najmniej jeden serwer hosta funkcji Hyper-V w głównych i dodatkowych chmurach programu VMM.<br/><br/> Serwery powinny być połączone z Internetem.<br/><br/> Dane są replikowane między głównymi i dodatkowymi serwerami hosta funkcji Hyper-V za pośrednictwem sieci LAN albo sieci VPN korzystającej z protokołu Kerberos lub uwierzytelniania certyfikatu.  
**Maszyny wirtualne funkcji Hyper-V** | Znajduje się na źródłowym serwerze hosta funkcji Hyper-V. | Źródłowy serwer hosta powinien mieć co najmniej jedną maszynę wirtualną, która ma być replikowana.

### <a name="replication-process"></a>Proces replikacji

1. Należy skonfigurować konto platformy Azure.
2. Należy utworzyć magazyn usługi Replication Services na potrzeby usługi Site Recovery i skonfigurować ustawienia magazynu, w tym następujące elementy:

    - Źródło i cel replikacji (lokacja główna i dodatkowa).
    - Instalacja dostawcy usługi Azure Site Recovery i agenta usługi Microsoft Azure Recovery Services. Dostawca jest instalowany na serwerach VMM, a agent na każdym hoście funkcji Hyper-V.
    - Należy utworzyć zasady replikacji dla źródłowej chmury programu VMM. Te zasady są stosowane do wszystkich maszyn wirtualnych znajdujących się na hostach w chmurze.
    - Należy włączyć replikację maszyn wirtualnych funkcji Hyper-V. Początkowa replikacja jest wykonywana zgodnie z ustawieniami zasad replikacji.
4. Zmiany danych są śledzone i po zakończeniu początkowej replikacji rozpoczyna się replikowanie zmian różnicowych. Śledzone zmiany elementu są przechowywane w pliku hrl.
5. Należy uruchomić testowanie trybu failover, aby upewnić się, że wszystkie funkcje działają.

**Rysunek 7: Replikacja z programu VMM do programu VMM**

![Ze środowiska lokalnego do środowiska lokalnego](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback"></a>Praca w trybie failover i powrót po awarii

1. Planowane lub nieplanowane przejście w tryb [failover](site-recovery-failover.md) można uruchomić między lokacjami lokalnymi. Jeśli zostanie uruchomione planowane przejście w tryb failover, źródłowe maszyny wirtualne zostaną wyłączone w celu zapewnienia, że nie będzie miała miejsca utrata danych.
2. W tryb failover można przełączyć pojedynczą maszynę lub można utworzyć [plany odzyskiwania](site-recovery-create-recovery-plans.md), aby zarządzać trybem failover na wielu maszynach.
4. Jeśli wykonywane jest nieplanowane przejście w tryb failover do lokacji dodatkowej, po przejściu w tryb failover na maszynach znajdujących się w lokalizacji dodatkowej nie jest włączona ochrona ani replikacja. Jeśli dokonano planowanego przejścia w tryb failover, po przejściu w tryb failover maszyny znajdujące się w lokalizacji dodatkowej są chronione.
5. Następnie należy zatwierdzić tryb failover, aby można było rozpocząć uzyskiwanie dostępu do obciążenia z poziomu repliki maszyny wirtualnej.
6. Gdy lokacja główna będzie znowu dostępna, należy zainicjować replikację odwrotną w celu przeprowadzenia replikacji z lokacji dodatkowej do lokacji głównej. Replikacja odwrotna powoduje przełączenie maszyn wirtualnych do stanu chronionego, ale aktywną lokalizacją jest nadal dodatkowe centrum danych.
7. Aby lokacja główna stała się z powrotem lokalizacją aktywną, należy zainicjować planowane przejście w tryb failover z lokacji dodatkowej do głównej, a następnie przeprowadzić kolejną replikację odwrotną.


## <a name="next-steps"></a>Następne kroki

- [Dowiedz się więcej](site-recovery-hyper-v-azure-architecture.md) o przepływie pracy replikacji funkcji Hyper-V.
- [Sprawdzanie wymagań wstępnych](site-recovery-prereq.md)

