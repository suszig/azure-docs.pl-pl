<properties
    pageTitle="W jaki sposób działa usługa Site Recovery? | Microsoft Azure"
    description="Ten artykuł zawiera omówienie architektury usługi Site Recovery"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/27/2016"
    ms.author="raynew"/>

# W jaki sposób działa usługa Azure Site Recovery?

Przeczytaj ten artykuł, aby zrozumieć podstawową architekturę usługi Azure Site Recovery i składników, dzięki którym działa. 

Zamieść wszelkie komentarze lub pytania pod tym artykułem lub na [forum Usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## Omówienie

Organizacje wymagają strategii zapewniającej ciągłość biznesową i odzyskiwanie po awarii (BCDR, business continuity and disaster recovery), która określa, w jaki sposób aplikacje, obciążenia i dane pozostają dostępne podczas planowanych lub nieplanowanych przestojów oraz jak są przywracane do normalnych warunków roboczych możliwie jak najszybciej.

Usługa Site Recovery jest usługą platformy Azure, która wspiera strategię zapewniającą ciągłość biznesową i odzyskiwanie po awarii poprzez organizowanie replikacji lokalnych serwerów fizycznych i maszyn wirtualnych do chmury (Azure) lub lokacji dodatkowej. W przypadku wystąpienia awarii w lokacji głównej następuje przejście w tryb failover do lokacji dodatkowej, dzięki czemu aplikacje i obciążenia są nadal dostępne. Powrót po awarii może mieć miejsce do lokalizacji głównej, gdy powróci ona do normalnego działania.

Usługa Site Recovery może zostać wdrożona w celu organizowania replikacji w wielu scenariuszach:

- **Replikowanie maszyn wirtualnych VMware**: lokalne maszyny wirtualne VMware można replikować na platformę [Azure](site-recovery-vmware-to-azure-classic.md) lub do [dodatkowego centrum danych](site-recovery-vmware-to-vmware.md).
- **Replikowanie maszyn fizycznych**: maszyny fizyczne z systemem Windows lub Linux można replikować na platformę [Azure](site-recovery-vmware-to-azure-classic.md) lub do [dodatkowego centrum danych](site-recovery-vmware-to-vmware.md).
- **Replikowanie maszyn wirtualnych funkcji Hyper-V zarządzanych w chmurach programu System Center VMM**: maszyny wirtualne funkcji Hyper-V w chmurach VMM można replikować na platformę [Azure](site-recovery-vmm-to-azure.md) lub do [dodatkowego centrum danych](site-recovery-vmm-to-vmm.md). 
- **Replikowanie maszyn wirtualnych funkcji Hyper-V (bez programu VMM)**: maszyny wirtualne funkcji Hyper-V, które nie są zarządzane przez program VMM, można replikować na platformę [Azure](site-recovery-hyper-v-site-to-azure.md).
- **Migrowanie maszyn wirtualnych**: usługi Site Recovery można użyć do [przeprowadzenia migracji maszyn wirtualnych IaaS platformy Azure](site-recovery-migrate-azure-to-azure.md) między regionami lub do [przeprowadzenia migracji wystąpień systemu Windows usług AWS](site-recovery-migrate-aws-to-azure.md) do maszyn wirtualnych IaaS platformy Azure. Obecnie obsługiwana jest tylko migracja, co oznacza, że możliwe jest przełączanie tych maszyn wirtualnych do trybu failover, ale nie jest możliwy ich powrót po awarii.

Usługa Site Recovery może replikować większość aplikacji uruchomionych na tych maszynach wirtualnych lub serwerach fizycznych. Aby uzyskać pełne podsumowanie obsługiwanych aplikacji, zobacz [What workloads can Azure Site Recovery protect?](site-recovery-workload.md) (Jakie obciążenia może chronić usługa Azure Site Recovery?).

## Replikowanie lokalnych maszyn wirtualnych VMware lub serwerów fizycznych na platformę Azure

Dostępne są obecnie dwie różne architektury służące do replikowania maszyn wirtualnych VMware lub fizycznych serwerów z systemem Windows lub Linux na platformę Azure:

- [Architektura starsza](site-recovery-vmware-to-azure-classic-legacy.md): tej architektury nie należy używać w przypadku nowych wdrożeń. 
- [Architektura rozszerzona](site-recovery-vmware-to-azure-classic.md): jest to najnowsza architektura i powinna być używana w przypadku wszystkich nowych wdrożeń. Jeśli ten scenariusz został już wdrożony przy użyciu starszej architektury, [uzyskaj informacje na temat migracji](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment) do rozszerzonego wdrożenia.

W ramach wdrożenia rozszerzonego należy skonfigurować lokalny serwer zarządzania ze wszystkimi składnikami usługi Site Recovery. Na każdej maszynie, która ma być chroniona, należy automatycznie przeprowadzić instalację wypychaną lub ręcznie zainstalować usługę Mobility. Po początkowej replikacji usługa Mobility na maszynie wysyła dane replikacji przyrostowej na serwer przetwarzania, który optymalizuje je przed wysłaniem do magazynu Azure.

![Rozszerzone](./media/site-recovery-components/arch-enhanced.png)
![Rozszerzone](./media/site-recovery-components/arch-enhanced2.png)

### Lokalnie
Oto, co należy zapewnić lokalnie:

- **Serwer zarządzania**: należy posiadać maszynę z systemem Windows Server 2012 R2, która będzie działać jako serwer zarządzania. Na tym serwerze za pomocą pojedynczego pliku instalacji zostaną zainstalowane wszystkie składniki usługi Site Recovery:

    - **Składnik serwera konfiguracji**: służy do koordynowania komunikacji między środowiskiem lokalnym i platformą Azure oraz do zarządzania replikacją i odzyskiwaniem danych.
    - **Składnik serwera przetwarzania**: działa jako brama replikacji. Odbiera dane replikacji z chronionych maszyn źródłowych, optymalizuje je przy użyciu pamięci podręcznej, kompresji i szyfrowania, a następnie wysyła dane do magazynu Azure. Obsługuje także instalację wypychaną usługi Mobility na chronionych maszynach i przeprowadza automatyczne odnajdywanie maszyn wirtualnych VMware. Wraz z rozwojem wdrożenia możliwe będzie dodawanie dodatkowych oddzielnych dedykowanych serwerów przetwarzania w celu obsługi coraz większej ilości danych związanych z ruchem replikacji.
    - **Składnik głównego serwera docelowego**: służy do obsługi replikacji danych podczas powrotu po awarii z platformy Azure. 
- **Hosty VMware ESX/ESXi i serwer vCenter**: konieczny jest co najmniej jeden serwer hostów ESX/ESXi, na którym są uruchamiane maszyny wirtualne VMware. Zaleca się wdrożenie serwera vCenter do zarządzania tymi hostami. **Uwaga:** **nawet w przypadku replikowania serwerów fizycznych należy przeprowadzić ich powrót po awarii do programu VMware**. Po replikowaniu serwera fizycznego będzie on działał jako maszyna wirtualna platformy Azure w przypadku przejścia w tryb failover na platformę Azure. Powróci on po awarii do środowiska lokalnego jako maszyna wirtualna VMware. 
    
- **Maszyny wirtualne/serwery fizyczne**: na każdej maszynie, która ma zostać zreplikowana na platformę Azure, konieczne jest zainstalowanie składnika usługi Mobility. Ta usługa służy do przechwytywania zapisów danych na maszynie i przekazywania ich do serwera przetwarzania. Ten składnik można zainstalować ręcznie lub można go wypchnąć i zainstalować automatycznie za pomocą serwera przetwarzania po włączeniu replikacji dla maszyny.

### Azure

Oto, co jest potrzebne w infrastrukturze Azure:     - **konto platformy Azure**: konieczne będzie konto Microsoft Azure.
    - **Magazyn Azure**: konieczne będzie konto magazynu Azure do przechowywania replikowanych danych. Replikowane dane są przechowywane w magazynie Azure, a w przypadku przejścia w tryb failover uruchamiane są maszyny wirtualne platformy Azure. 
    - **Sieć platformy Azure**: wymagana będzie sieć wirtualna Azure, z którą uruchomione maszyny wirtualne platformy Azure będą nawiązywać połączenie w przypadku przejścia w tryb failover. 
    
    
### Powrót po awarii

Powrót po awarii do środowiska lokalnego odbywa się zawsze do maszyn wirtualnych VMware, nawet jeśli miało miejsce przejście w tryb failover serwera fizycznego. Oto, co jest potrzebne:

- **Tymczasowy serwer przetwarzania na platformie Azure**: jeśli chcesz powrócić po awarii z platformy Azure po przejściu w tryb failover, konieczne będzie skonfigurowanie maszyny wirtualnej platformy Azure jako serwera przetwarzania do obsługi replikacji z platformy Azure. Po zakończeniu powrotu po awarii można usunąć tę maszynę wirtualną.
- **Połączenie VPN**: na potrzeby powrotu po awarii będzie potrzebne połączenie VPN (lub usługa Azure ExpressRoute) skonfigurowane z sieci platformy Azure do lokacji lokalnej.
- **Oddzielny lokalny główny serwer docelowy**: lokalny główny serwer docelowy obsługuje powrót po awarii. Główny serwer docelowy jest instalowany domyślnie na serwerze zarządzania, ale w przypadku powrotu po awarii większych ilości ruchu należy skonfigurować oddzielny lokalny główny serwer docelowy. 

![Rozszerzony powrót po awarii](./media/site-recovery-components/enhanced-failback.png)

[Dowiedz się więcej](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) o wymaganiach dotyczących wdrożenia rozszerzonego.
[Dowiedz się więcej](site-recovery-failback-azure-to-vmware-classic.md) o powrocie po awarii dla wdrożenia rozszerzonego.




## Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach VMM na platformę Azure

Aby wdrożyć ten scenariusz, podczas wdrażania usługi Site Recovery zainstaluj dostawcę usług Azure Site Recovery na serwerze VMM. Dostawca koordynuje i organizuje replikację za pomocą usługi Site Recovery przez Internet. Agent Usług odzyskiwania Azure jest instalowany podczas wdrażania usługi Site Recovery na serwerze hosta funkcji Hyper-V, a dane są replikowane między nim a magazynem Azure za pośrednictwem protokołu HTTPS 443. Komunikacja zarówno ze strony dostawcy, jak i agenta, jest bezpieczna i szyfrowana. Zreplikowane dane w magazynie Azure również są szyfrowane.

- Lokalnie: 
    - **Serwer VMM**: co najmniej jeden serwer VMM z co najmniej jedną chmurą prywatną VMM. Na serwerze powinien być uruchomiony program System Center 2012 R2 i serwer powinien mieć połączenie z Internetem. Jeśli chcesz upewnić się, że maszyny wirtualne platformy Azure są połączone z siecią po przejściu w tryb failover, konieczne będzie skonfigurowanie mapowania sieci. Aby to zrobić, źródłowe maszyny wirtualne powinny być połączone z siecią maszyn wirtualnych VMM. Ta sieć maszyn wirtualnych powinna być połączona z siecią logiczną skojarzoną z chmurą.
    - **Serwer funkcji Hyper-V**: co najmniej jeden serwer hosta funkcji Hyper-V znajdujący się w chmurze VMM. Na hostach funkcji Hyper-V powinien działać system Windows Server 2012 R2.
    - **Chronione maszyny**: źródłowy serwer hosta funkcji Hyper-V powinien mieć co najmniej jedną maszynę wirtualną, która ma być chroniona.
    
- Azure: 
    - **Konto platformy Azure**: konieczne będzie konto platformy Microsoft Azure.
    - **Magazyn Azure**: konieczne będzie konto magazynu Azure do przechowywania replikowanych danych. Replikowane dane są przechowywane w magazynie Azure, a w przypadku przejścia w tryb failover uruchamiane są maszyny wirtualne platformy Azure.
    - **Sieć platformy Azure**: jeśli chcesz skonfigurować mapowanie sieci w taki sposób, aby maszyny wirtualne platformy Azure były połączone z sieciami po przejściu w tryb failover, konieczne będzie skonfigurowanie sieci platformy Azure.

    ![Z programu VMM na platformę Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

Dowiedz się więcej o dokładnych [wymaganiach dotyczących wdrożenia](site-recovery-vmm-to-azure.md#before-you-start).

## Replikowanie maszyn wirtualnych VMware lub serwerów fizycznych do lokacji dodatkowej

Aby zreplikować maszyny wirtualne VMware lub serwery fizyczne z systemem Windows lub Linux do lokacji dodatkowej, konieczne będzie pobranie programu InMage Scout, który jest dołączony do subskrypcji usługi Azure Site Recovery. Następnie należy skonfigurować serwery składników w każdej lokacji (konfiguracja, przetwarzanie, główny docelowy) oraz zainstalować program Unified Agent na maszynach, które mają zostać zreplikowane. Po początkowej replikacji agent na każdej maszynie wysyła zmiany replikacji przyrostowej na serwer przetwarzania. Serwer przetwarzania optymalizuje dane i transferuje je na główny serwer docelowy w lokacji dodatkowej. Serwer konfiguracji zarządza procesem replikacji.

![Z programu VMware do programu VMware](./media/site-recovery-components/vmware-to-vmware.png)

### Lokalna lokacja główna

- **Serwer przetwarzania**: składnik serwera przetwarzania należy skonfigurować w lokacji głównej w celu obsługi pamięci podręcznej, kompresji i optymalizacji danych. Umożliwia on również obsługę instalacji wypychanej programu Unified Agent na maszynach, które chcesz chronić. 
- **Host VMware ESX/ESXi i serwer vCenter**: jeśli chronisz maszyny wirtualne VMware, konieczne będzie użycie funkcji hypervisor hostów VMware EXS/ESXi i opcjonalnie serwera VMware vCenter do zarządzania funkcją hypervisor.
- **Maszyny wirtualne/serwery fizyczne**: na maszynach wirtualnych VMware lub serwerach fizycznych z systemem Windows lub Linux, które mają być chronione, konieczne będzie zainstalowanie programu Unified Agent. Program Unified Agent jest również instalowany na maszynach działających jako główny serwer docelowy. Agent działa jako dostawca komunikacji między wszystkimi składnikami. 
    
### Lokalna lokacja dodatkowa
 
- **Serwer konfiguracji**: serwer konfiguracji jest pierwszym instalowanym składnikiem. Jest on instalowany w lokacji dodatkowej w celu konfigurowania i monitorowania wdrożenia oraz zarządzania nim za pomocą witryny sieci Web zarządzania lub konsoli vContinuum. W ramach wdrożenia istnieje tylko jeden serwer konfiguracji i musi on zostać zainstalowany na maszynie z systemem Windows Server 2012 R2.
- **Serwer vContinuum**: jest instalowany w tej samej lokalizacji (lokacja dodatkowa) co serwer konfiguracji. Zapewnia on konsolę do monitorowania chronionego środowiska i zarządzania nim. W domyślnej instalacji serwer vContinuum jest pierwszym głównym serwerem docelowym i jest na nim zainstalowany program Unified Agent.
- **Główny serwer docelowy**: na głównym serwerze docelowym przechowywane są zreplikowane dane. Odbiera on dane z serwera przetwarzania i tworzy maszynę repliki w lokacji dodatkowej, a także znajdują się na nim punkty przechowywania danych. Liczba potrzebnych głównych serwerów docelowych zależy od liczby chronionych maszyn. Aby powrócić po awarii do lokacji głównej, konieczne będzie również posiadanie w tej sieci głównego serwera docelowego. 

### Azure

Ten scenariusz jest wdrażany przy użyciu programu InMage Scout. Aby go uzyskać, konieczna jest subskrypcja platformy Azure. Po utworzeniu magazynu usługi Site Recovery należy pobrać program InMage Scout i zainstalować najnowsze aktualizacje w celu skonfigurowania wdrożenia.


## Replikowanie maszyn wirtualnych funkcji Hyper-V na platformę Azure (bez programu VMM)

Aby przeprowadzić replikację maszyn wirtualnych funkcji Hyper-V, które nie są zarządzane w chmurach VMM, na platformę Azure, podczas wdrażania usługi Site Recovery zainstaluj dostawcę usług Azure Site Recovery i agenta Usług odzyskiwania Azure na hoście funkcji Hyper-V. Dostawca koordynuje i organizuje replikację za pomocą usługi Site Recovery przez Internet. Agent obsługuje replikację danych za pośrednictwem protokołu HTTPS 443. Komunikacja zarówno ze strony dostawcy, jak i agenta, jest bezpieczna i szyfrowana. Zreplikowane dane w magazynie Azure również są szyfrowane.

![Z lokacji funkcji Hyper-V na platformę Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

### Lokalnie

- **Serwer funkcji Hyper-V**: co najmniej jeden serwer hosta funkcji Hyper-V. Na hostach funkcji Hyper-V powinien działać system Windows Server 2012 R2.
- **Chronione maszyny**: źródłowy serwer hosta funkcji Hyper-V powinien mieć co najmniej jedną maszynę wirtualną, która ma być chroniona.
    
### Azure

- **Konto platformy Azure**: konieczne będzie konto platformy Microsoft Azure.
- **Magazyn Azure**: konieczne będzie konto magazynu Azure do przechowywania replikowanych danych. Replikowane dane są przechowywane w magazynie Azure, a w przypadku przejścia w tryb failover uruchamiane są maszyny wirtualne platformy Azure.

[Dowiedz się więcej](site-recovery-hyper-v-site-to-azure.md#before-you-start) o wymaganiach dotyczących wdrożenia.


## Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach VMM na platformę Azure

Aby wdrożyć ten scenariusz, podczas wdrażania usługi Site Recovery zainstaluj dostawcę usług Azure Site Recovery na serwerze VMM oraz agenta Usług odzyskiwania Azure na hoście funkcji Hyper-V. Dostawca koordynuje i organizuje replikację za pomocą usługi Site Recovery przez Internet. Agent obsługuje replikację danych za pośrednictwem protokołu HTTPS 443. Komunikacja zarówno ze strony dostawcy, jak i agenta, jest bezpieczna i szyfrowana. Zreplikowane dane w magazynie Azure (magazynowane) również są szyfrowane.

![Z programu VMM na platformę Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

### Lokalnie

- **Serwer VMM**: co najmniej jeden serwer VMM z co najmniej jedną chmurą prywatną VMM. Na serwerze powinien być uruchomiony program System Center 2012 R2 i serwer powinien mieć połączenie z Internetem. Jeśli chcesz upewnić się, że maszyny wirtualne platformy Azure są połączone z siecią po przejściu w tryb failover, konieczne będzie skonfigurowanie mapowania sieci. W tym celu należy połączyć źródłowe maszyny wirtualne z siecią maszyn wirtualnych VMM. Ta sieć powinna być połączona z siecią logiczną skojarzoną z chmurą.
- **Serwer funkcji Hyper-V**: co najmniej jeden serwer hosta funkcji Hyper-V znajdujący się w chmurze VMM. Na hostach funkcji Hyper-V powinien działać system Windows Server 2012 R2.
- **Chronione maszyny**: źródłowy serwer hosta funkcji Hyper-V powinien mieć co najmniej jedną maszynę wirtualną, która ma być chroniona.
    
### Azure

- **Konto platformy Azure**: konieczne będzie konto platformy Microsoft Azure.
- **Magazyn Azure**: konieczne będzie konto magazynu Azure do przechowywania replikowanych danych. Replikowane dane są przechowywane w magazynie Azure, a w przypadku przejścia w tryb failover uruchamiane są maszyny wirtualne platformy Azure.
- **Sieć platformy Azure**: aby zapewnić, że maszyny wirtualne platformy Azure będą połączone z siecią po przejściu w tryb failover, konieczne będzie skonfigurowanie mapowania sieci. W tym celu konieczna będzie sieć platformy Azure.

[Dowiedz się więcej](site-recovery-vmm-to-azure.md#before-you-start) o wymaganiach dotyczących wdrożenia.

## Replikowanie maszyn wirtualnych funkcji Hyper-V do dodatkowego centrum danych

Aby wdrożyć ten scenariusz, podczas wdrażania usługi Site Recovery zainstaluj dostawcę usług Azure Site Recovery na serwerze VMM. Dostawca koordynuje i organizuje replikację za pomocą usługi Site Recovery przez Internet. Dane są replikowane między głównymi i dodatkowymi serwerami hosta funkcji Hyper-V za pośrednictwem sieci LAN albo sieci VPN korzystającej z protokołu Kerberos lub uwierzytelniania certyfikatu. Komunikacja zarówno ze strony dostawcy, jak i między serwerami hosta funkcji Hyper-V, jest bezpieczna i szyfrowana. 

![Ze środowiska lokalnego do środowiska lokalnego](./media/site-recovery-components/arch-onprem-onprem.png)

### Lokalnie

- **Serwer VMM**: zalecany jest jeden serwer VMM w lokacji głównej i jeden w lokacji dodatkowej, każdy z nich zawierający co najmniej jedną prywatną chmurę VMM. Na serwerze powinien działać program System Center w wersji co najmniej 2012 SP1 z najnowszymi aktualizacjami i powinien on mieć połączenie z Internetem. Dla chmur należy ustawić profil funkcji Hyper-V.
- **Serwer funkcji Hyper-V**: serwery hosta funkcji Hyper-V powinny znajdować się w głównych i dodatkowych chmurach VMM. Na serwerze hosta powinien działać program System Center w wersji co najmniej 2012 z najnowszymi aktualizacjami i powinien on mieć połączenie z Internetem.
- **Chronione maszyny**: źródłowy serwer hosta funkcji Hyper-V powinien mieć co najmniej jedną maszynę wirtualną, która ma być chroniona.
    
### Azure

Konieczna jest subskrypcja platformy Azure.

[Dowiedz się więcej](site-recovery-vmm-to-vmm.md#before-you-start) o wymaganiach dotyczących wdrożenia.


## Replikowanie maszyn wirtualnych funkcji Hyper-V do dodatkowego centrum danych przy użyciu replikacji sieci SAN

W tym scenariuszu podczas wdrażania usługi Site Recovery zainstaluj dostawcę usług Azure Site Recovery na serwerach VMM. Dostawca koordynuje i organizuje replikację za pomocą usługi Site Recovery przez Internet. Dane są replikowane między tablicami magazynów podstawowych i pomocniczych przy użyciu synchronicznej replikacji sieci SAN.

![Replikacja sieci SAN](./media/site-recovery-components/arch-onprem-onprem-san.png)

### Lokalnie

- **Tablica sieci SAN**: [obsługiwana tablica sieci SAN](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) zarządzana przez podstawowy serwer VMM. Sieć SAN udostępnia infrastrukturę sieciową innej tablicy sieci SAN w lokacji dodatkowej.
- **Serwer VMM**: zalecany jest jeden serwer VMM w lokacji głównej i jeden w lokacji dodatkowej, każdy z nich zawierający co najmniej jedną prywatną chmurę VMM. Na serwerze powinien działać program System Center w wersji co najmniej 2012 SP1 z najnowszymi aktualizacjami i powinien on mieć połączenie z Internetem. Dla chmur należy ustawić profil funkcji Hyper-V.
- **Serwer funkcji Hyper-V**: serwery hosta funkcji Hyper-V znajdujące się w głównych i dodatkowych chmurach VMM. Na serwerze hosta powinien działać program System Center w wersji co najmniej 2012 z najnowszymi aktualizacjami i powinien on mieć połączenie z Internetem.
- **Chronione maszyny**: źródłowy serwer hosta funkcji Hyper-V powinien mieć co najmniej jedną maszynę wirtualną, która ma być chroniona.
    
### Azure

Konieczna jest subskrypcja platformy Azure.  

[Dowiedz się więcej](site-recovery-vmm-san.md#before-you-start) o wymaganiach dotyczących wdrożenia.


## Cykl życia ochrony funkcji Hyper-V

Ten przepływ pracy przedstawia proces ochrony, replikowania i przechodzenia w tryb failover maszyn wirtualnych funkcji Hyper-V. 

1. **Włącz ochronę**: przeprowadzana jest konfiguracja magazynu usługi Site Recovery, konfiguracja ustawień replikacji dla chmury VMM lub lokacji funkcji Hyper-V oraz włączana jest ochrona maszyn wirtualnych. Inicjowane jest zadanie o nazwie **Włączanie ochrony**, które można monitorować na karcie **Zadania**. Zadanie sprawdza, czy maszyna spełnia wymagania wstępne, a następnie wywołuje metodę [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), która konfiguruje replikację na platformę Azure za pomocą określonych ustawień. Zadanie **Włączanie ochrony** wywołuje również metodę [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) w celu zainicjowania pełnej replikacji maszyny wirtualnej.
2. **Replikacja początkowa**: wykonywana jest migawka maszyny wirtualnej, a wirtualne dyski twarde są replikowane pojedynczo, dopóki wszystkie nie zostaną skopiowane na platformę Azure lub do dodatkowego centrum danych. Czas potrzebny na zakończenie tego procesu zależy od rozmiaru maszyny wirtualnej, przepustowości sieci i metody replikacji początkowej. Jeśli podczas replikacji początkowej będzie miała miejsce zmiana dysku, składnik Replica Replication Tracker funkcji Hyper-V będzie śledził te zmiany jako dzienniki replikacji funkcji Hyper-V (hrl), które znajdują się w tym samym folderze co dyski. Z każdym dyskiem jest skojarzony plik hrl, który zostanie wysłany do magazynu pomocniczego. Należy pamiętać, że pliki migawki i dziennika zużywają zasoby dysku w trakcie replikacji początkowej. Po zakończeniu replikacji początkowej migawka maszyny wirtualnej zostanie usunięta, a przyrostowe zmiany dysków w dzienniku zostaną zsynchronizowane i scalone.
3. **Finalizowanie ochrony**: po zakończeniu replikacji początkowej zadanie **Finalizowanie ochrony** konfiguruje ustawienia sieciowe i inne ustawienia po replikacji, aby maszyna wirtualna była chroniona. W przypadku przeprowadzania replikacji na platformę Azure może być konieczne dostosowanie ustawień maszyny wirtualnej w taki sposób, aby była gotowa do przejścia w tryb failover. W tym momencie możesz uruchomić testowe przejście w tryb failover w celu sprawdzenia, czy wszystko działa zgodnie z oczekiwaniami.
4. **Replikacja**: po przeprowadzeniu synchronizacji początkowej rozpoczynana jest replikacja przyrostowa zgodnie z ustawieniami replikacji. 
    - **Niepowodzenie replikacji**: jeśli replikacja przyrostowa nie powiedzie się, a pełna replikacja będzie kosztowna pod względem przepustowości lub czasu, to zostanie przeprowadzona ponowna synchronizacja. Jeśli na przykład pliki hrl będą zajmować 50% rozmiaru dysku, to maszyna wirtualna zostanie oznaczona do ponownej synchronizacji. Ponowna synchronizacja minimalizuje ilość wysyłanych danych dzięki obliczaniu sum kontrolnych źródłowych i docelowych maszyn wirtualnych oraz wysyłaniu tylko danych przyrostowych. Po ukończeniu ponownej synchronizacji replikacja przyrostowa zostanie wznowiona. Domyślnie ponowna synchronizacja jest zaplanowana do automatycznego uruchamiania poza godzinami pracy, ale możliwe jest uruchomienie ponownej synchronizacji maszyny wirtualnej ręcznie.
    - **Błąd replikacji**: jeśli wystąpi błąd replikacji, to może zostać użyty wbudowany mechanizm ponawiania. Jeśli jest to błąd nieodwracalny, taki jak błąd uwierzytelniania lub autoryzacji, lub jeśli maszyna repliki jest w nieprawidłowym stanie, nie będzie podejmowana żadna próba ponowienia. Jeśli jest to błąd odwracalny, taki jak błąd sieci lub błąd dotyczący małej ilości miejsca/pamięci, to wtedy podejmowana jest próba ponowienia w rosnących odstępach czasu między kolejnymi próbami (1, 2, 4, 8, 10, a następnie co 30 minut).
4. **Planowane lub nieplanowane przejście w tryb failover**: w razie potrzeby można uruchomić planowane lub nieplanowane przejścia w tryb failover. Jeśli zostanie uruchomione planowane przejście w tryb failover, to źródłowe maszyny wirtualne zostaną wyłączone w celu zapewnienia, że nie będzie miała miejsca utrata danych. Po utworzeniu maszyny wirtualne replik mają stan oczekiwania na zatwierdzenie. Należy zatwierdzić je, aby zakończyć przejście w tryb failover, chyba że ma miejsce replikacja sieci SAN — w takim przypadku zatwierdzanie odbywa się automatycznie. Po skonfigurowaniu i uruchomieniu lokacji głównej może zostać wykonany powrót po awarii. W przypadku replikacji na platformę Azure odwrotna replikacja przebiega automatycznie. W przeciwnym razie należy rozpocząć replikację odwrotną ręcznie.
 

![przepływ pracy](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## Następne kroki

[Przygotowanie do wdrożenia](site-recovery-best-practices.md)



<!--HONumber=Jun16_HO2-->


