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
ms.date: 01/02/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: bd8082c46ee36c70e372208d1bd15337acc558a1
ms.openlocfilehash: eb97f66901efa336942dee56d9a8a62ade1f6842


---
# <a name="how-does-azure-site-recovery-work"></a>W jaki sposób działa usługa Azure Site Recovery?

Przeczytaj ten artykuł, aby zrozumieć podstawową architekturę usługi Azure Site Recovery i składników, dzięki którym działa.

Organizacje wymagają strategii BCDR, która określa, w jaki sposób aplikacje, obciążenia i dane pozostają uruchomione i dostępne podczas planowanych lub nieplanowanych przerw w pracy oraz są przywracane do normalnych warunków roboczych z możliwie dużą prędkością. Strategia BCDR powinna utrzymywać dane firmowe z zachowaniem bezpieczeństwa i umożliwiać ich odzyskiwanie, a także zapewniać, że obciążenia pozostają stale dostępne w przypadku awarii.

Usługa Site Recovery jest usługą platformy Azure, która wspiera strategię BCDR przez organizowanie replikacji lokalnych serwerów fizycznych i maszyn wirtualnych do chmury (Azure) lub dodatkowego centrum danych. W przypadku wystąpienia awarii w lokalizacji głównej następuje przełączenie w trybie failover do lokalizacji dodatkowej, dzięki czemu aplikacje i obciążenia są nadal dostępne. Powrót po awarii może mieć miejsce do lokalizacji głównej, gdy powróci ona do normalnego działania. Dowiedz się więcej w temacie [Co to jest usługa Site Recovery?](site-recovery-overview.md)

W tym artykule opisano wdrażanie w witrynie [Azure Portal](https://portal.azure.com). [Klasycznej witryny Azure Portal](https://manage.windowsazure.com/) można używać do obsługi istniejących magazynów usługi Site Recovery, ale nie można tworzyć nowych magazynów za jej pomocą.

Komentarze możesz zamieścić na dole tego artykułu. Zadawaj pytania techniczne na [Forum Usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="deployment-scenarios"></a>Scenariusze wdrażania

Usługa Site Recovery może zostać wdrożona w celu organizowania replikacji w wielu scenariuszach:

- **Replikowanie maszyn wirtualnych VMware**: lokalne maszyny wirtualne VMware można replikować na platformę Azure lub do dodatkowego centrum danych.
- **Replikowanie maszyn fizycznych**: maszyny fizyczne (z systemem Windows lub Linux) można replikować na platformę Azure lub do dodatkowego centrum danych. Proces replikowania maszyn fizycznych jest prawie taki sam jak proces replikowania maszyn wirtualnych VMware.
- **Replikowanie maszyn wirtualnych funkcji Hyper-V**: maszyny wirtualne funkcji Hyper-V można replikować na platformę Azure lub do dodatkowej lokacji programu VMM. Jeśli mają one być replikowane do dodatkowej lokacji, muszą być zarządzane w chmurach programu System Center Virtual Machine Manager (VMM).
- **Migrowanie maszyn wirtualnych**: oprócz replikowania (replikacja, tryb failover i powrót po awarii) lokalnych maszyn wirtualnych i serwerów fizycznych do platformy Azure, można również przeprowadzić ich migrację do maszyn wirtualnych platformy Azure (replikacja, tryb failover, bez powrotu po awarii). Można wykonać następujące czynności:
    - Przeprowadzić migrację obciążeń działających na lokalnych maszynach wirtualnych funkcji Hyper-V, maszynach wirtualnych programu VMware i serwerach fizycznych, aby działały na maszynach wirtualnych platformy Azure.
    - Przeprowadzić migrację [maszyn wirtualnych IaaS platformy Azure](site-recovery-migrate-azure-to-azure.md) między regionami platformy Azure. W tym scenariuszu aktualnie jest obsługiwana tylko migracja, co oznacza, że powrót po awarii nie jest obsługiwany.
    - Przeprowadzić migrację [wystąpień usługi AWS dla systemu Windows](site-recovery-migrate-aws-to-azure.md) do maszyn wirtualnych IaaS platformy Azure. W tym scenariuszu aktualnie jest obsługiwana tylko migracja, co oznacza, że powrót po awarii nie jest obsługiwany.

Usługa Site Recovery replikuje aplikacje działające na obsługiwanych maszynach wirtualnych lub serwerach fizycznych. Aby uzyskać pełne podsumowanie obsługiwanych aplikacji, zobacz [What workloads can Azure Site Recovery protect?](site-recovery-workload.md) (Jakie obciążenia może chronić usługa Azure Site Recovery?).

## <a name="replicate-vmware-vmsphysical-servers-to-azure"></a>Replikacja maszyn wirtualnych VMware/serwerów fizycznych do platformy Azure

### <a name="components"></a>Składniki

**Składnik** | **Szczegóły**
--- | ---
**Azure** | W przypadku platformy Azure konieczne jest posiadanie konta platformy Microsoft Azure, konta usługi Azure Storage i sieci platformy Azure.<br/><br/> Konta magazynu i sieci mogą być kontami usługi Resource Manager lub kontami klasycznymi.<br/><br/>  Replikowane dane są przechowywane na koncie magazynu. W przypadku wystąpienia w lokacji lokalnej przejścia w tryb failover maszyny wirtualne platformy Azure są tworzone przy użyciu zreplikowanych danych. Maszyny wirtualne platformy Azure nawiązują połączenie z siecią wirtualną platformy Azure, gdy są tworzone.
**Serwer konfiguracji** | Serwer konfiguracji jest konfigurowany lokalnie na potrzeby koordynowania komunikacji między lokacją lokalną i platformą Azure oraz w celu zarządzania replikacją danych.
**Serwer przetwarzania** | Instalowany domyślnie na lokalnym serwerze konfiguracji.<br/><br/> Działa jako brama replikacji. Odbiera dane replikacji z chronionych maszyn źródłowych, optymalizuje je przy użyciu pamięci podręcznej, kompresji i szyfrowania, a następnie wysyła dane do usługi Azure Storage.<br/><br/> Obsługuje instalację wypychaną usługi Mobility na chronionych maszynach i przeprowadza automatyczne odnajdywanie maszyn wirtualnych VMware.<br/><br/> Wraz z rozwojem wdrożenia możliwe będzie dodawanie dodatkowych oddzielnych dedykowanych serwerów przetwarzania w celu obsługi coraz większej ilości danych związanych z ruchem replikacji.
**Główny serwer docelowy** | Instalowany domyślnie na lokalnym serwerze konfiguracji.<br/><br/> Służy do obsługi replikacji danych podczas powrotu po awarii z platformy Azure. W przypadku dużego ruchu powrotu po awarii można wdrożyć oddzielny główny serwer docelowy na potrzeby obsługi powrotu po awarii.
**Serwery VMware** | Aby replikować maszyny wirtualne VMware, należy dodać serwery vCenter i vSphere do magazynu usługi Recovery Services.<br/><br/> W celu zreplikowania serwerów fizycznych konieczne jest posiadanie lokalnej infrastruktury VMware na potrzeby powrotu po awarii. Nie można powracać po awarii do maszyny fizycznej.
**Zreplikowane maszyny** | Usługa Mobility musi być zainstalowana na każdej maszynie, która ma być replikowana. Można ją zainstalować ręcznie na poszczególnych maszynach lub za pośrednictwem instalacji wypychanej z serwera przetwarzania.

**Rysunek 1: Składniki replikacji z oprogramowania VMware na platformę Azure**

![Składniki](./media/site-recovery-components/arch-enhanced.png)

### <a name="replication-process"></a>Proces replikacji

1. Należy skonfigurować wdrażanie, w tym składniki platformy Azure i magazyn usługi Recovery Services. W magazynie należy określić źródło i cel replikacji, skonfigurować serwer konfiguracji, dodać serwery VMware, utworzyć zasady replikacji, wdrożyć usługę Mobility, włączyć replikację i uruchomić test trybu failover.
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

### <a name="failover-and-failback-process"></a>Proces pracy w trybie failover i podczas powrotu po awarii

1. Nieplanowane przejścia w tryb failover uruchamia się z lokalnych maszyn wirtualnych VMware i serwerów fizycznych do platformy Azure. Planowane przejście w tryb failover nie jest obsługiwane.
2. W tryb failover można przełączyć pojedynczą maszynę lub można utworzyć [plany odzyskiwania](site-recovery-create-recovery-plans.md), aby zarządzać trybem failover na wielu maszynach.
3. Po przejściu w tryb failover na platformie Azure zostaną utworzone repliki maszyn wirtualnych. Po zatwierdzeniu trybu failover można rozpocząć uzyskiwanie dostępu do obciążenia z poziomu repliki maszyny wirtualnej platformy Azure.
4. Po ponownym udostępnieniu głównej lokacji lokalnej można do niej powrócić po awarii. Należy skonfigurować infrastrukturę powrotu po awarii, uruchomić replikację maszyny z lokacji dodatkowej do lokacji głównej i uruchomić nieplanowane przejście w tryb failover z lokacji dodatkowej. Po zatwierdzeniu pracy w trybie failover dane będą znowu dostępne lokalnie i konieczne będzie ponowne włączenie replikacji do platformy Azure. [Dowiedz się więcej](site-recovery-failback-azure-to-vmware.md)

Istnieje kilka wymagań powrotu po awarii:

- **Powrót po awarii z lokalizacji fizycznej do lokalizacji fizycznej nie jest obsługiwany**: oznacza to, że w przypadku przeniesienia serwerów fizycznych do trybu failover na platformie Azure, a następnie ich przywrócenia po awarii, należy przywrócić je do maszyny wirtualnej VMware. Nie można powracać po awarii do serwera fizycznego. Wymagana jest maszyna wirtualna platformy Azure, do której przeprowadzony będzie powrót po awarii. Jeśli nie wdrożono serwera konfiguracji jako maszyny wirtualnej VMware, należy skonfigurować osobny główny serwer docelowy jako maszynę wirtualną VMware. Jest to potrzebne, ponieważ główny serwer docelowy wchodzi w interakcję i łączy się z magazynem VMware, aby przywracać dyski do maszyny wirtualnej VMware.
- **Tymczasowy serwer przetwarzania na platformie Azure**: jeśli chcesz powrócić po awarii z platformy Azure po przejściu w tryb failover, konieczne będzie skonfigurowanie maszyny wirtualnej platformy Azure jako serwera przetwarzania do obsługi replikacji z platformy Azure. Po zakończeniu powrotu po awarii można usunąć tę maszynę wirtualną.
- **Połączenie VPN**: na potrzeby powrotu po awarii będzie potrzebne połączenie VPN (lub usługa Azure ExpressRoute) skonfigurowane z sieci platformy Azure do lokacji lokalnej.
- **Oddzielny lokalny główny serwer docelowy**: lokalny główny serwer docelowy obsługuje powrót po awarii. Główny serwer docelowy jest instalowany domyślnie na serwerze zarządzania, ale w przypadku powrotu po awarii większych ilości ruchu należy skonfigurować oddzielny lokalny główny serwer docelowy.
- **Zasady powrotu po awarii**: aby móc przeprowadzić ponowną replikację do lokacji lokalnej, należy utworzyć zasady powrotu po awarii. Są one tworzone automatycznie podczas tworzenia zasad replikacji.

**Rysunek 3: Powrót po awarii między oprogramowaniem VMware i serwerem fizycznym**

![Powrót po awarii](./media/site-recovery-components/enhanced-failback.png)


## <a name="replicate-vmware-vmsphysical-servers-to-a-secondary-site"></a>Replikacja maszyn wirtualnych VMware/serwerów fizycznych do lokacji dodatkowej

### <a name="components"></a>Składniki

**Składnik** | **Szczegóły**
--- | ---
**Azure** | Ten scenariusz jest wdrażany przy użyciu programu InMage Scout. Aby go uzyskać, konieczna jest subskrypcja platformy Azure.<br/><br/> Po utworzeniu magazynu usługi Recovery Services należy pobrać program InMage Scout i zainstalować najnowsze aktualizacje w celu skonfigurowania wdrożenia.
**Serwer przetwarzania** | Należy wdrożyć składnik serwera przetwarzania w lokacji głównej w celu obsługi pamięci podręcznej, kompresji i optymalizacji danych.<br/><br/> Umożliwia on również obsługę instalacji wypychanej programu Unified Agent na maszynach, które chcesz chronić.
**Program VMware ESX/ESXi i serwer vCenter** |  Aby móc replikować maszyny wirtualne VMware, konieczne jest posiadanie infrastruktury VMware.
**Maszyny wirtualne/serwery fizyczne** |  Na przeznaczonych do replikowania maszynach wirtualnych VMware lub serwerach fizycznych z systemem Windows/Linux należy zainstalować program Unified Agent.<br/><br/> Agent działa jako dostawca komunikacji między wszystkimi składnikami.
**Serwer konfiguracji** | Serwer konfiguracji jest instalowany w lokacji dodatkowej w celu konfigurowania i monitorowania wdrożenia oraz zarządzania nim za pomocą witryny sieci Web zarządzania lub konsoli vContinuum.
**Serwer vContinuum** | Instalowany w tej samej lokalizacji co serwer konfiguracji.<br/><br/> Zapewnia on konsolę do monitorowania chronionego środowiska i zarządzania nim.
**Główny serwer docelowy (lokacja dodatkowa)** | Na głównym serwerze docelowym przechowywane są zreplikowane dane. Odbiera on dane z serwera przetwarzania i tworzy maszynę repliki w lokacji dodatkowej, a także znajdują się na nim punkty przechowywania danych.<br/><br/> Liczba potrzebnych głównych serwerów docelowych zależy od liczby chronionych maszyn.<br/><br/> Aby powrócić po awarii do lokacji głównej, konieczne jest również posiadanie w tej sieci głównego serwera docelowego. Na tym serwerze jest instalowany program Unified Agent.

### <a name="replication-process"></a>Proces replikacji

1. Następnie należy skonfigurować serwery składników w każdej lokacji (konfiguracja, przetwarzanie, główny docelowy) oraz zainstalować program Unified Agent na maszynach, które mają zostać zreplikowane.
2. Po początkowej replikacji agent na każdej maszynie wysyła zmiany replikacji przyrostowej na serwer przetwarzania.
3. Serwer przetwarzania optymalizuje dane i transferuje je na główny serwer docelowy w lokacji dodatkowej. Serwer konfiguracji zarządza procesem replikacji.

**Rysunek 4: Replikacja oprogramowania VMware to VMware**

![Z programu VMware do programu VMware](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="replicate-hyper-v-vms-to-azure"></a>Replikacja maszyn wirtualnych funkcji Hyper-V do platformy Azure


### <a name="components"></a>Składniki

**Składnik** | **Szczegóły**
--- | ---
**Azure** | W przypadku platformy Azure konieczne jest posiadanie konta platformy Microsoft Azure, konta usługi Azure Storage i sieci platformy Azure.<br/><br/> Konta magazynu i sieci mogą być kontami opartymi na usłudze Resource Manager lub kontami klasycznymi.<br/><br/> Replikowane dane są przechowywane na koncie magazynu. W przypadku wystąpienia w lokacji lokalnej przejścia w tryb failover maszyny wirtualne platformy Azure są tworzone przy użyciu zreplikowanych danych.<br/><br/> Maszyny wirtualne platformy Azure nawiązują połączenie z siecią wirtualną platformy Azure, gdy są tworzone.
**Serwer VMM** | Jeśli hosty funkcji Hyper-V znajdują się w chmurach programu VMM, należy skonfigurować sieci logiczne i sieci maszyn wirtualnych na potrzeby konfigurowania [mapowania sieci](site-recovery-network-mapping.md). Sieć maszyn wirtualnych powinna być połączona z siecią logiczną skojarzoną z chmurą.
**Host funkcji Hyper-V** | Wymagany jest co najmniej jeden serwer hosta funkcji Hyper-V.
**Maszyny wirtualne funkcji Hyper-V** | Wymagana jest co najmniej jedna maszyna wirtualna na serwerze hosta funkcji Hyper-V. Dostawca działający na hoście funkcji Hyper-V koordynuje i organizuje replikację za pomocą usługi Site Recovery przez Internet. Agent obsługuje replikację danych za pośrednictwem protokołu HTTPS 443. Komunikacja zarówno ze strony dostawcy, jak i agenta, jest bezpieczna i szyfrowana. Zreplikowane dane w usłudze Azure Storage również są szyfrowane.


## <a name="replication-process"></a>Proces replikacji

1. Należy skonfigurować składniki platformy Azure. Przed rozpoczęciem wdrażania usługi Site Recovery zaleca się skonfigurowanie kont magazynu i sieci.
2. Należy utworzyć magazyn usługi Replication Services na potrzeby usługi Site Recovery i skonfigurować ustawienia magazynu, w tym następujące elementy:
    - Jeśli hosty funkcji Hyper-V nie są zarządzane w chmurze programu VMM, należy utworzyć kontener lokacji funkcji Hyper-V i dodać do niego hosty funkcji Hyper-V.
    - Źródło i cel replikacji. Jeśli hosty funkcji Hyper-V są zarządzane w programie VMM, źródłem jest chmura programu VMM. W przeciwnym razie źródłem jest lokacja funkcji Hyper-V.
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
6. Po ponownym udostępnieniu głównej lokacji lokalnej można do niej powrócić po awarii. Planowane przejście w tryb failover jest rozpoczynane z platformy Azure do lokacji głównej. W przypadku planowanego przejścia w tryb failover można wybrać powrót po awarii do tej samej maszyny wirtualnej lub do lokalizacji alternatywnej i zsynchronizować zmiany między platformą Azure i lokacją lokalną, aby zapobiec utracie danych. Po utworzeniu lokalnych maszyn wirtualnych należy zatwierdzić tryb failover.

**Rysunek 5: Replikacja z lokacji funkcji Hyper-V na platformę Azure**

![Składniki](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Rysunek 6: Replikacja z funkcji Hyper-V w chmurach programu VMM na platformę Azure**

![Składniki](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)



## <a name="replicate-hyper-v-vms-to-a-secondary-site"></a>Replikacja maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej

### <a name="components"></a>Składniki

**Składnik** | **Szczegóły**
--- | ---
**Konto platformy Azure** | Potrzebujesz konta platformy Microsoft Azure.
**Serwer VMM** | Zalecamy, aby jeden serwer VMM znajdował się w lokacji głównej, a jeden w lokacji dodatkowej (połączone z Internetem).<br/><br/> Każdy serwer powinien mieć co najmniej jedną chmurę prywatną programu VMM z ustawionym profilem możliwości funkcji Hyper-V.<br/><br/> Zainstaluj dostawcę usługi Azure Site Recovery na serwerze VMM. Dostawca koordynuje i organizuje replikację za pomocą usługi Site Recovery przez Internet. Komunikacja między dostawcą i platformą Azure jest bezpieczna i szyfrowana.
**Serwer funkcji Hyper-V** |  Potrzebujesz co najmniej jednego serwera hosta funkcji Hyper-V w głównych i dodatkowych chmurach programu VMM. Serwery powinny być połączone z Internetem.<br/><br/> Dane są replikowane między głównymi i dodatkowymi serwerami hosta funkcji Hyper-V za pośrednictwem sieci LAN albo sieci VPN korzystającej z protokołu Kerberos lub uwierzytelniania certyfikatu.  
**Maszyny źródłowe** | Źródłowy serwer hosta funkcji Hyper-V powinien mieć co najmniej jedną maszynę wirtualną, która ma być replikowana.

## <a name="replication-process"></a>Proces replikacji

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

### <a name="failover-and-failback-process"></a>Proces pracy w trybie failover i podczas powrotu po awarii

1. Planowane lub nieplanowane przejście w tryb [failover](site-recovery-failover.md) można uruchomić między lokacjami lokalnymi. Jeśli zostanie uruchomione planowane przejście w tryb failover, źródłowe maszyny wirtualne zostaną wyłączone w celu zapewnienia, że nie będzie miała miejsca utrata danych.
2. W tryb failover można przełączyć pojedynczą maszynę lub można utworzyć [plany odzyskiwania](site-recovery-create-recovery-plans.md), aby zarządzać trybem failover na wielu maszynach.
4. Jeśli wykonywane jest nieplanowane przejście w tryb failover do lokacji dodatkowej, po przejściu w tryb failover na maszynach znajdujących się w lokalizacji dodatkowej nie jest włączona ochrona ani replikacja. Jeśli dokonano planowanego przejścia w tryb failover, po przejściu w tryb failover maszyny znajdujące się w lokalizacji dodatkowej są chronione.
5. Następnie następuje zatwierdzenie trybu failover, aby można było rozpocząć uzyskiwanie dostępu do obciążenia z poziomu repliki maszyny wirtualnej.
6. Gdy lokacja główna będzie znowu dostępna, należy zainicjować replikację odwrotną w celu przeprowadzenia replikacji z lokacji dodatkowej do lokacji głównej. Replikacja odwrotna powoduje przełączenie maszyn wirtualnych do stanu chronionego, ale aktywną lokalizacją jest nadal dodatkowe centrum danych.
7. Aby lokacja główna stała się z powrotem lokalizacją aktywną, należy zainicjować planowane przejście w tryb failover z lokacji dodatkowej do głównej, a następnie przeprowadzić kolejną replikację odwrotną.


### <a name="hyper-v-replication-workflow"></a>Przepływ pracy replikacji funkcji Hyper-V

**Etap przepływu pracy** | **Akcja**
--- | ---
1. **Włączanie ochrony** | Po włączeniu ochrony maszyny wirtualnej funkcji Hyper-V jest inicjowane zadanie **Włączanie ochrony** w celu sprawdzenia zgodności maszyny z wymaganiami wstępnymi. To zadanie wywołuje dwie metody:<br/><br/> [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) w celu skonfigurowania replikacji za pomocą skonfigurowanych ustawień.<br/><br/> [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) w celu zainicjowania pełnej replikacji maszyny wirtualnej.
2. **Replikacja początkowa** |  Wykonywana jest migawka maszyny wirtualnej, a wirtualne dyski twarde są replikowane pojedynczo, dopóki wszystkie nie zostaną skopiowane do lokalizacji dodatkowej.<br/><br/> Czas potrzebny na zakończenie tego procesu zależy od rozmiaru maszyny wirtualnej, przepustowości sieci i metody replikacji początkowej.<br/><br/> Jeśli podczas replikacji początkowej będzie miała miejsce zmiana dysku, składnik Replica Replication Tracker funkcji Hyper-V będzie śledził te zmiany jako dzienniki replikacji funkcji Hyper-V (hrl), które znajdują się w tym samym folderze co dyski.<br/><br/> Z każdym dyskiem jest skojarzony plik hrl, który zostanie wysłany do magazynu pomocniczego.<br/><br/> Pliki migawki i dziennika zużywają zasoby dysku w trakcie replikacji początkowej. Po zakończeniu replikacji początkowej migawka maszyny wirtualnej zostanie usunięta, a przyrostowe zmiany dysków w dzienniku zostaną zsynchronizowane i scalone.
3. **Finalizowanie ochrony** | Po zakończeniu replikacji początkowej zadanie **Finalizowanie ochrony** konfiguruje ustawienia sieciowe i inne ustawienia po replikacji, aby maszyna wirtualna była chroniona.<br/><br/> W przypadku przeprowadzania replikacji na platformę Azure może być konieczne dostosowanie ustawień maszyny wirtualnej w taki sposób, aby była gotowa do przejścia w tryb failover.<br/><br/> W tym momencie możesz uruchomić testowe przejście w tryb failover w celu sprawdzenia, czy wszystko działa zgodnie z oczekiwaniami.
4. **Replikacja** | Po przeprowadzeniu replikacji początkowej rozpoczynana jest synchronizacja przyrostowa zgodnie z ustawieniami replikacji.<br/><br/> **Niepowodzenie replikacji**: jeśli replikacja przyrostowa nie powiedzie się, a pełna replikacja będzie kosztowna pod względem przepustowości lub czasu, to zostanie przeprowadzona ponowna synchronizacja. Jeśli na przykład pliki hrl będą zajmować 50% rozmiaru dysku, to maszyna wirtualna zostanie oznaczona do ponownej synchronizacji. Ponowna synchronizacja minimalizuje ilość wysyłanych danych dzięki obliczaniu sum kontrolnych źródłowych i docelowych maszyn wirtualnych oraz wysyłaniu tylko danych przyrostowych. Po ukończeniu ponownej synchronizacji replikacja przyrostowa zostanie wznowiona. Domyślnie ponowna synchronizacja jest zaplanowana do automatycznego uruchamiania poza godzinami pracy, ale możliwe jest uruchomienie ponownej synchronizacji maszyny wirtualnej ręcznie.<br/><br/> **Błąd replikacji**: jeśli wystąpi błąd replikacji, może zostać użyty wbudowany mechanizm ponawiania. Jeśli jest to błąd nieodwracalny, taki jak błąd uwierzytelniania lub autoryzacji, albo jeśli maszyna repliki jest w nieprawidłowym stanie, nie będzie podejmowana żadna próba ponowienia. Jeśli jest to błąd odwracalny, taki jak błąd sieci lub błąd dotyczący małej ilości miejsca/pamięci, to wtedy podejmowana jest próba ponowienia w rosnących odstępach czasu między kolejnymi próbami (1, 2, 4, 8, 10, a następnie co 30 minut).
5. **Planowane lub nieplanowane przejście w tryb failover** | W razie potrzeby można uruchomić planowane lub nieplanowane przejścia w tryb failover.<br/><br/> Jeśli zostanie uruchomione planowane przejście w tryb failover, źródłowe maszyny wirtualne zostaną wyłączone w celu zapewnienia, że nie będzie miała miejsca utrata danych.<br/><br/> Po utworzeniu maszyny wirtualne replik mają stan oczekiwania na zatwierdzenie. Należy je zatwierdzić, aby ukończyć przejście w tryb failover.<br/><br/> Po uruchomieniu lokacji głównej można powrócić do niej po awarii, kiedy będzie ona dostępna.


**Rysunek 8: Przepływ pracy funkcji Hyper-V**

![przepływ pracy](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Następne kroki

[Przygotowanie do wdrożenia](site-recovery-best-practices.md)



<!--HONumber=Jan17_HO1-->


