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
ms.date: 10/05/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 5614c39d914d5ae6fde2de9c0d9941e7b93fc10f
ms.openlocfilehash: ef9a7da86e7528d3052f89dbe1eaac6fbb90527c


---
# <a name="how-does-azure-site-recovery-work"></a>W jaki sposób działa usługa Azure Site Recovery?
Przeczytaj ten artykuł, aby zrozumieć podstawową architekturę usługi Azure Site Recovery i składników, dzięki którym działa.

Zamieść wszelkie komentarze lub pytania pod tym artykułem lub na [forum Usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Omówienie
Organizacje wymagają strategii BCDR, która określa, w jaki sposób aplikacje, obciążenia i dane pozostają uruchomione i dostępne podczas planowanych lub nieplanowanych przerw w pracy oraz są przywracane do normalnych warunków roboczych z możliwie dużą prędkością. Strategia BCDR powinna utrzymywać dane firmowe z zachowaniem bezpieczeństwa i umożliwiać ich odzyskiwanie, a także zapewniać, że obciążenia pozostają stale dostępne w przypadku awarii.

Usługa Site Recovery jest usługą platformy Azure, która wspiera strategię BCDR przez organizowanie replikacji lokalnych serwerów fizycznych i maszyn wirtualnych do chmury (Azure) lub dodatkowego centrum danych. W przypadku wystąpienia awarii w lokalizacji głównej następuje przełączenie w trybie failover do lokalizacji dodatkowej, dzięki czemu aplikacje i obciążenia są nadal dostępne. Powrót po awarii może mieć miejsce do lokalizacji głównej, gdy powróci ona do normalnego działania. Dowiedz się więcej w temacie [Co to jest usługa Site Recovery?](site-recovery-overview.md)

## <a name="site-recovery-in-the-azure-portal"></a>Usługa Site Recovery w portalu Azure
Platforma Azure ma dwa różne [modele wdrażania](../resource-manager-deployment-model.md) związane z tworzeniem zasobów i pracą z nimi: Azure Resource Manager i klasyczny model zarządzania usługami. Platforma Azure ma dwa portale — [klasyczną witrynę Azure Portal](https://manage.windowsazure.com/), która obsługuje klasyczny model wdrażania, oraz [witrynę Azure Portal](https://portal.azure.com), która obsługuje oba modele wdrażania.

Usługa Site Recovery jest dostępna zarówno w portalu klasycznym, jak i w portalu Azure. W klasycznym portalu Azure możesz korzystać z usługi Site Recovery przy użyciu klasycznego modelu zarządzania usługami. W portalu Azure możesz korzystać z wdrożeń opartych na modelu klasycznym lub na modelu z obsługą zasobów. [Dowiedz się więcej](site-recovery-overview.md#site-recovery-in-the-azure-portal) o wdrażaniu przy użyciu portalu Azure.

Informacje przedstawione w tym artykule dotyczą wdrożeń klasycznych i przy użyciu portalu Azure. Różnice są odnotowane wedle potrzeby.

## <a name="deployment-scenarios"></a>Scenariusze wdrażania
Usługa Site Recovery może zostać wdrożona w celu organizowania replikacji w wielu scenariuszach:

* **Replikowanie maszyn wirtualnych VMware**: lokalne maszyny wirtualne VMware można replikować na platformę Azure lub do dodatkowego centrum danych.
* * **Replikowanie maszyn fizycznych**: maszyny fizyczne z systemem Windows lub Linux można replikować na platformę Azure lub do dodatkowego centrum danych. Proces replikowania maszyn fizycznych jest prawie taki sam jak proces replikowania maszyn wirtualnych VMware
* **Replikowanie maszyn wirtualnych funkcji Hyper-V (bez programu VMM)**: maszyny wirtualne funkcji Hyper-V, które nie są zarządzane przez program VMM, można replikować na platformę Azure.
* **Replikowanie maszyn wirtualnych funkcji Hyper-V zarządzanych w chmurach programu System Center VMM**: maszyny wirtualne funkcji Hyper-V działające na serwerach hostów funkcji Hyper-V w chmurach VMM można replikować na platformę Azure lub do dodatkowego centrum danych. Można replikować przy użyciu standardowej repliki funkcji Hyper-V lub replikacji sieci SAN.
* **Migrowanie maszyn wirtualnych**: usługi Site Recovery można użyć do [przeprowadzenia migracji maszyn wirtualnych IaaS platformy Azure](site-recovery-migrate-azure-to-azure.md) między regionami lub do [przeprowadzenia migracji wystąpień systemu Windows usług AWS](site-recovery-migrate-aws-to-azure.md) do maszyn wirtualnych IaaS platformy Azure. Obecnie obsługiwana jest tylko migracja, co oznacza, że możliwe jest przełączanie tych maszyn wirtualnych do trybu failover, ale nie jest możliwy ich powrót po awarii.

Usługa Site Recovery może replikować większość aplikacji uruchomionych na tych maszynach wirtualnych lub serwerach fizycznych. Aby uzyskać pełne podsumowanie obsługiwanych aplikacji, zobacz [What workloads can Azure Site Recovery protect?](site-recovery-workload.md) (Jakie obciążenia może chronić usługa Azure Site Recovery?).

## <a name="replicate-to-azure-vmware-virtual-machines-or-physical-windowslinux-servers"></a>Replikacja do platformy Azure: maszyny wirtualne VMware lub fizyczne serwery z systemem Windows/Linux
Istnieje kilka sposobów replikowania maszyn wirtualnych VMware przy użyciu usługi Site Recovery.

* **Za pomocą portalu Azure** — w przypadku wdrożenia usługi Site Recovery w portalu Azure możesz przełączać maszyny wirtualne do trybu failover do klasycznego magazynu menedżera usługi lub do usługi Resource Manager. Replikowanie maszyn wirtualnych VMware w portalu Azure niesie ze sobą wiele korzyści, w tym możliwość replikowania do klasycznego magazynu lub magazynu usługi Resource Manager na platformie Azure. [Dowiedz się więcej](site-recovery-vmware-to-azure.md).
* **Za pomocą portalu klasycznego** — można wdrożyć usługę Site Recovery w portalu klasycznym przy użyciu udoskonalonego środowiska użytkownika. Ta opcja powinna być stosowana dla wszystkich nowych wdrożeń w portalu klasycznym. W tym wdrożeniu można przełączać awaryjnie maszyny wirtualne tylko do klasycznego magazynu na platformie Azure, a nie do magazynu usługi Resource Manager. [Dowiedz się więcej](site-recovery-vmware-to-azure-classic.md). Istnieje również [starsze środowisko](site-recovery-vmware-to-azure-classic-legacy.md) do konfigurowania replikacji VMware w klasycznym portalu. Ta opcja nie powinna być używana w przypadku nowych wdrożeń.  Jeśli przeprowadzono już wdrożenie przy użyciu starszego środowiska użytkownika, [uzyskaj informacje na temat migracji](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment) do rozszerzonego wdrożenia.

Wymagania dotyczące architektury na potrzeby wdrażania usługi Site Recovery w celu replikowania maszyn wirtualnych VMware/serwerów fizycznych w portalu Azure lub klasycznym portalu Azure (rozszerzonym) są podobne, z kilkoma różnicami:

* W przypadku wdrożenia w portalu Azure możesz replikować do magazynu opartego na usłudze Resource Manager i użyć sieci usługi Resource Manager do uzyskania połączenia z maszynami wirtualnymi platformy Azure po przejściu do trybu failover.
* W przypadku wdrożenia w portalu Azure obsługiwany jest magazyn LRS i GRS. W klasycznym portalu wymagany jest magazyn GRS.
* Proces wdrażania jest uproszczony i bardziej przyjazny dla użytkownika w portalu Azure.

Oto, co jest potrzebne:

* **Konto platformy Azure**: konieczne będzie konto platformy Microsoft Azure.
* **Azure Storage**: konieczne będzie konto usługi Azure Storage do przechowywania replikowanych danych. Można używać klasycznego konta magazynu lub konta magazynu usługi Resource Manager. Konto może być magazynem LRS lub GRS w przypadku wdrażania w witrynie Azure Portal. Replikowane dane są przechowywane w usłudze Azure Storage, a w przypadku przejścia w tryb failover uruchamiane są maszyny wirtualne platformy Azure.
* **Sieć platformy Azure**: wymagana będzie sieć wirtualna Azure, z którą maszyny wirtualne platformy Azure będą nawiązywać połączenie w przypadku ich utworzenia w trybie failover. W portalu Azure można utworzyć sieci w klasycznym modelu menedżera usługi lub w modelu opartym na usłudze Resource Manager.
* **Lokalny serwer konfiguracji**: wymagana jest lokalna maszyna z systemem Windows Server 2012 R2, na której działa serwer konfiguracji i inne składniki usługi Site Recovery. Jeśli replikujesz maszyny wirtualne VMware, powinna to być maszyna wirtualna VMware o wysokiej dostępności. Jeśli chcesz replikować serwery fizyczne, maszyna może być fizyczna. Na maszynie zostaną zainstalowane te składniki usługi Site Recovery:
  * **Serwer konfiguracji**: służy do koordynowania komunikacji między środowiskiem lokalnym i platformą Azure oraz do zarządzania replikacją i odzyskiwaniem danych.
  * **Serwer przetwarzania**: działa jako brama replikacji. Odbiera dane replikacji z chronionych maszyn źródłowych, optymalizuje je przy użyciu pamięci podręcznej, kompresji i szyfrowania, a następnie wysyła dane do usługi Azure Storage. Obsługuje także instalację wypychaną usługi Mobility na chronionych maszynach i przeprowadza automatyczne odnajdywanie maszyn wirtualnych VMware. Wraz z rozwojem wdrożenia możliwe będzie dodawanie dodatkowych oddzielnych dedykowanych serwerów przetwarzania w celu obsługi coraz większej ilości danych związanych z ruchem replikacji.
  * **Główny serwer docelowy**: służy do obsługi replikacji danych podczas powrotu po awarii z platformy Azure.
* **Maszyny wirtualne VMware lub serwery fizyczne do replikacji**: na każdej maszynie, która ma zostać zreplikowana na platformę Azure, konieczne jest zainstalowanie składnika usługi Mobility. Ta usługa służy do przechwytywania zapisów danych na maszynie i przekazywania ich do serwera przetwarzania. Ten składnik można zainstalować ręcznie lub można go wypchnąć i zainstalować automatycznie za pomocą serwera przetwarzania po włączeniu replikacji dla maszyny.
* **Hosty vSPhere/serwer vCenter**: konieczny jest co najmniej jeden serwer hosta vSphere, na którym są uruchamiane maszyny wirtualne VMware. Zaleca się wdrożenie serwera vCenter do zarządzania tymi hostami.
* **Powrót po awarii**: oto, co jest potrzebne:
  * **Powrót po awarii z lokalizacji fizycznej do lokalizacji fizycznej nie jest obsługiwany**: oznacza to, że w przypadku przeniesienia serwerów fizycznych do trybu failover na platformie Azure, a następnie ich przywrócenia po awarii, należy przywrócić je do maszyny wirtualnej VMware. Nie można powracać po awarii do serwera fizycznego. Wymagana jest maszyna wirtualna platformy Azure, do której przeprowadzony będzie powrót po awarii. Jeśli nie wdrożono serwera konfiguracji jako maszyny wirtualnej VMware, należy skonfigurować osobny główny serwer docelowy jako maszynę wirtualną VMware. Jest to potrzebne, ponieważ główny serwer docelowy wchodzi w interakcję i łączy się z magazynem VMware, aby przywracać dyski do maszyny wirtualnej VMware.
  * * **Tymczasowy serwer przetwarzania na platformie Azure**: jeśli chcesz powrócić po awarii z platformy Azure po przejściu w tryb failover, konieczne będzie skonfigurowanie maszyny wirtualnej platformy Azure jako serwera przetwarzania do obsługi replikacji z platformy Azure. Po zakończeniu powrotu po awarii można usunąć tę maszynę wirtualną.
  * **Połączenie VPN**: na potrzeby powrotu po awarii będzie potrzebne połączenie VPN (lub usługa Azure ExpressRoute) skonfigurowane z sieci platformy Azure do lokacji lokalnej.
  * **Oddzielny lokalny główny serwer docelowy**: lokalny główny serwer docelowy obsługuje powrót po awarii. Główny serwer docelowy jest instalowany domyślnie na serwerze zarządzania, ale w przypadku powrotu po awarii większych ilości ruchu należy skonfigurować oddzielny lokalny główny serwer docelowy.

**Architektura ogólna**

![Rozszerzone](./media/site-recovery-components/arch-enhanced.png)

**Składniki wdrażania**

![Rozszerzone](./media/site-recovery-components/arch-enhanced2.png)

**Powrót po awarii**

![Rozszerzony powrót po awarii](./media/site-recovery-components/enhanced-failback.png)

* [Dowiedz się więcej](site-recovery-vmware-to-azure.md#azure-prerequisites) o wymaganiach dotyczących wdrożenia w portalu Azure.
* [Dowiedz się więcej](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) o wymaganiach dotyczących wdrożenia rozszerzonego w portalu klasycznym.
* [Dowiedz się więcej](site-recovery-failback-azure-to-vmware.md) o powrocie po awarii w witrynie Azure Portal.
* [Dowiedz się więcej](site-recovery-failback-azure-to-vmware-classic.md) o powrocie po awarii w portalu klasycznym.

## <a name="replicate-to-azure-hyper-v-vms-not-managed-by-vmm"></a>Replikacja do platformy Azure: maszyny wirtualne funkcji Hyper-V niezarządzane przez program VMM
Można replikować maszyny wirtualne funkcji Hyper-V, które nie są zarządzane przez program System Center VMM, do platformy Azure przy użyciu usługi Site Recovery w następujący sposób:

* **Za pomocą portalu Azure** — w przypadku wdrożenia usługi Site Recovery w portalu Azure możesz przełączać awaryjnie maszyny wirtualne do klasycznego magazynu lub do usługi Resource Manager. [Dowiedz się więcej](site-recovery-hyper-v-site-to-azure.md).
* **Za pomocą portalu klasycznego** — można wdrożyć usługę Site Recovery w portalu klasycznym. W tym wdrożeniu można przełączać awaryjnie maszyny wirtualne tylko do klasycznego magazynu na platformie Azure, a nie do magazynu usługi Resource Manager. [Dowiedz się więcej](site-recovery-hyper-v-site-to-azure-classic.md).

Architektura dla obu wdrożeń jest podobna, poza następującymi różnicami:

* W przypadku wdrożenia w portalu Azure możesz replikować do magazynu usługi Resource Manager i użyć sieci usługi Resource Manager do uzyskania połączenia z maszynami wirtualnymi Azure po przejściu w tryb failover.
* Proces wdrażania jest uproszczony i bardziej przyjazny dla użytkownika w portalu Azure.

Oto, co jest potrzebne:

* **Konto platformy Azure**: konieczne będzie konto platformy Microsoft Azure.
* **Azure Storage**: konieczne będzie konto usługi Azure Storage do przechowywania replikowanych danych. W portalu Azure można używać klasycznego konta magazynu lub konta magazynu usługi Resource Manager. W portalu klasycznym można użyć tylko klasycznego konta. W przypadku przejścia do trybu failover replikowane dane są przechowywane w usłudze Azure Storage i tworzone są maszyny wirtualne Azure.
* **Sieć platformy Azure**: wymagana będzie sieć platformy Azure, z którą maszyny wirtualne platformy Azure będą nawiązywać połączenie w przypadku ich utworzenia po przejściu do trybu failover.
* **Host funkcji Hyper-V**: wymagany będzie co najmniej jeden serwer hosta funkcji Hyper-V z systemem Windows Server 2012 R2. Podczas wdrażania usługi Site Recovery na hoście zostanie zainstalowany dostawca usługi Azure Site Recovery oraz agent usług Microsoft Azure Recovery Services.
* **Maszyny wirtualne funkcji Hyper-V**: wymagana będzie co najmniej jedna maszyna wirtualna na serwerze hosta funkcji Hyper-V. Dostawca usługi Azure Site Recovery oraz agent usług Azure Recovery Services na hoście funkcji Hyper-V podczas wdrażania usługi Site Recovery. Dostawca koordynuje i organizuje replikację za pomocą usługi Site Recovery przez Internet. Agent obsługuje replikację danych za pośrednictwem protokołu HTTPS 443. Komunikacja zarówno ze strony dostawcy, jak i agenta, jest bezpieczna i szyfrowana. Zreplikowane dane w usłudze Azure Storage również są szyfrowane.

**Architektura ogólna**

![Z lokacji funkcji Hyper-V na platformę Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

* [Dowiedz się więcej](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) o wymaganiach dotyczących wdrożenia w portalu Azure.
* [Dowiedz się więcej](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) o wymaganiach dotyczących wdrożenia w portalu klasycznym.

## <a name="replicate-to-azure-hyper-v-vms-managed-by-vmm"></a>Replikacja do platformy Azure: maszyny wirtualne funkcji Hyper-V zarządzane przez program VMM
Można replikować maszyny wirtualne funkcji Hyper-V w chmurach VMM na platformę Azure przy użyciu usługi Site Recovery w następujący sposób:

* **Za pomocą portalu Azure** — w przypadku wdrożenia usługi Site Recovery w portalu Azure możesz przełączać awaryjnie maszyny wirtualne do klasycznego magazynu lub do usługi Resource Manager. [Dowiedz się więcej](site-recovery-vmm-to-azure.md).
* **Za pomocą portalu klasycznego** — można wdrożyć usługę Site Recovery w portalu klasycznym. W tym wdrożeniu można przełączać awaryjnie maszyny wirtualne tylko do klasycznego magazynu na platformie Azure, a nie do magazynu usługi Resource Manager. [Dowiedz się więcej](site-recovery-vmm-to-azure-classic.md).

Architektura dla obu wdrożeń jest podobna, poza następującymi różnicami:

* W przypadku wdrożenia w portalu Azure możesz replikować do magazynu opartego na usłudze Resource Manager i użyć sieci usługi Resource Manager do uzyskania połączenia z maszynami wirtualnymi platformy Azure po przejściu do trybu failover.
* Proces wdrażania jest uproszczony i bardziej przyjazny dla użytkownika w portalu Azure.

Oto, co jest potrzebne:

* **Konto platformy Azure**: konieczne będzie konto platformy Microsoft Azure.
* **Azure Storage**: konieczne będzie konto usługi Azure Storage do przechowywania replikowanych danych. W portalu Azure można używać klasycznego konta magazynu lub konta magazynu usługi Resource Manager. W portalu klasycznym można użyć tylko klasycznego konta. W przypadku przejścia do trybu failover replikowane dane są przechowywane w usłudze Azure Storage i tworzone są maszyny wirtualne Azure.
* **Sieć platformy Azure**: należy skonfigurować mapowanie sieci w taki sposób, aby maszyny wirtualne platformy Azure były połączone z odpowiednimi sieciami po ich utworzeniu po przejściu do trybu failover.
* **Serwer VMM**: wymagany będzie co najmniej jeden lokalny serwer VMM z systemem System Center 2012 R2 oraz co najmniej jedna skonfigurowana chmura prywatna. W przypadku wdrożenia w portalu Azure wymagane będzie skonfigurowanie sieci logicznej i maszyn wirtualnych w celu skonfigurowania mapowania sieci. W portalu klasycznym jest to opcjonalne.  Sieć maszyn wirtualnych powinna być połączona z siecią logiczną skojarzoną z chmurą.
* **Host funkcji Hyper-V**: wymagany będzie co najmniej jeden serwer hosta funkcji Hyper-V z systemem Windows Server 2012 R2 w chmurze VMM.
* **Maszyny wirtualne funkcji Hyper-V**: wymagana będzie co najmniej jedna maszyna wirtualna na serwerze hosta funkcji Hyper-V.

**Architektura ogólna**

![Z programu VMM na platformę Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

* [Dowiedz się więcej](site-recovery-vmm-to-azure.md#azure-prerequisites) o wymaganiach dotyczących wdrożenia w portalu Azure.
* [Dowiedz się więcej](site-recovery-vmm-to-azure-classic.md) o wymaganiach dotyczących wdrożenia w portalu klasycznym.

## <a name="replicate-to-a-secondary-site-vmware-virtual-machines-or-physical-servers"></a>Replikowanie do lokacji dodatkowej: replikowanie maszyn wirtualnych VMware lub serwerów fizycznych
Aby zreplikować maszyny wirtualne VMware lub serwery fizyczne do lokacji dodatkowej, konieczne będzie pobranie programu InMage Scout, który jest dołączony do subskrypcji usługi Azure Site Recovery. Można go pobrać z portalu Azure lub klasycznego portalu Azure.

Następnie należy skonfigurować serwery składników w każdej lokacji (konfiguracja, przetwarzanie, główny docelowy) oraz zainstalować program Unified Agent na maszynach, które mają zostać zreplikowane. Po początkowej replikacji agent na każdej maszynie wysyła zmiany replikacji przyrostowej na serwer przetwarzania. Serwer przetwarzania optymalizuje dane i transferuje je na główny serwer docelowy w lokacji dodatkowej. Serwer konfiguracji zarządza procesem replikacji.

Oto, co jest potrzebne:

**Konto Azure**: ten scenariusz jest wdrażany przy użyciu programu InMage Scout. Aby go uzyskać, konieczna jest subskrypcja platformy Azure. Po utworzeniu magazynu usługi Site Recovery należy pobrać program InMage Scout i zainstalować najnowsze aktualizacje w celu skonfigurowania wdrożenia.
**Serwer przetwarzania (lokacja główna)**: składnik serwera przetwarzania należy skonfigurować w lokacji głównej w celu obsługi pamięci podręcznej, kompresji i optymalizacji danych. Umożliwia on również obsługę instalacji wypychanej programu Unified Agent na maszynach, które chcesz chronić.
**Host VMware ESX/ESXi i serwer vCenter (lokacja główna)**: jeśli chronisz maszyny wirtualne VMware, konieczne będzie użycie funkcji hypervisor hostów VMware EXS/ESXi i opcjonalnie serwera VMware vCenter do zarządzania funkcją hypervisor.

* **Maszyny wirtualne/serwery fizyczne (lokacja główna)**: na maszynach wirtualnych VMware lub serwerach fizycznych z systemem Windows lub Linux, które mają być chronione, konieczne będzie zainstalowanie programu Unified Agent. Program Unified Agent jest również instalowany na maszynach działających jako główny serwer docelowy. Agent działa jako dostawca komunikacji między wszystkimi składnikami.
* * **Serwer konfiguracji (lokacja dodatkowa)**: serwer konfiguracji jest pierwszym instalowanym składnikiem. Jest on instalowany w lokacji dodatkowej w celu konfigurowania i monitorowania wdrożenia oraz zarządzania nim za pomocą witryny sieci Web zarządzania lub konsoli vContinuum. W ramach wdrożenia istnieje tylko jeden serwer konfiguracji i musi on zostać zainstalowany na maszynie z systemem Windows Server 2012 R2.
* **Serwer vContinuum (lokacja dodatkowa)**: jest instalowany w tej samej lokalizacji (lokacja dodatkowa) co serwer konfiguracji. Zapewnia on konsolę do monitorowania chronionego środowiska i zarządzania nim. W domyślnej instalacji serwer vContinuum jest pierwszym głównym serwerem docelowym i jest na nim zainstalowany program Unified Agent.
* **Główny serwer docelowy (lokacja dodatkowa)**: na głównym serwerze docelowym przechowywane są zreplikowane dane. Odbiera on dane z serwera przetwarzania i tworzy maszynę repliki w lokacji dodatkowej, a także znajdują się na nim punkty przechowywania danych. Liczba potrzebnych głównych serwerów docelowych zależy od liczby chronionych maszyn. Aby powrócić po awarii do lokacji głównej, konieczne będzie również posiadanie w tej sieci głównego serwera docelowego.

**Architektura ogólna**

![Z programu VMware do programu VMware](./media/site-recovery-components/vmware-to-vmware.png)

## <a name="replicate-to-a-secondary-site-hyper-v-vms-managed-by-vmm"></a>Replikacja do lokacji dodatkowej: maszyny wirtualne funkcji Hyper-V zarządzane przez program VMM
Można replikować maszyny wirtualne funkcji Hyper-V, które są zarządzane przez program System Center VMM, do dodatkowego centrum danych przy użyciu usługi Site Recovery w następujący sposób:

* **Za pomocą portalu Azure** — w przypadku wdrożenia usługi Site Recovery w portalu Azure. [Dowiedz się więcej](site-recovery-hyper-v-site-to-azure.md).
* **Za pomocą portalu klasycznego** — można wdrożyć usługę Site Recovery w portalu klasycznym. [Dowiedz się więcej](site-recovery-hyper-v-site-to-azure-classic.md).

Architektura dla obu wdrożeń jest podobna, poza następującymi różnicami:

* W przypadku wdrożenia w portalu Azure należy skonfigurować mapowanie sieci. W portalu klasycznym jest to opcjonalne.
* Proces wdrażania jest uproszczony i bardziej przyjazny dla użytkownika w portalu Azure.
* * W przypadku wdrożenia w portalu klasycznym Azure [mapowanie sieci](site-recovery-storage-mapping.md) jest dostępne.

Oto, co jest potrzebne:

* **Konto platformy Azure**: konieczne będzie konto platformy Microsoft Azure.
* **Serwer VMM**: zalecany jest jeden serwer VMM w lokacji głównej i jeden w lokacji dodatkowej, każdy z nich zawierający co najmniej jedną prywatną chmurę VMM. Na serwerze powinien działać program System Center w wersji co najmniej 2012 SP1 z najnowszymi aktualizacjami i powinien on mieć połączenie z Internetem. Dla chmur należy ustawić profil funkcji Hyper-V. Zainstaluj dostawcę usługi Azure Site Recovery na serwerze VMM. Dostawca koordynuje i organizuje replikację za pomocą usługi Site Recovery przez Internet. Komunikacja między dostawcą i platformą Azure jest bezpieczna i szyfrowana.
* **Serwer funkcji Hyper-V**: serwery hosta funkcji Hyper-V powinny znajdować się w głównych i dodatkowych chmurach VMM. Na serwerze hosta powinien działać program System Center w wersji co najmniej 2012 z najnowszymi aktualizacjami i powinien on mieć połączenie z Internetem. Dane są replikowane między głównymi i dodatkowymi serwerami hosta funkcji Hyper-V za pośrednictwem sieci LAN albo sieci VPN korzystającej z protokołu Kerberos lub uwierzytelniania certyfikatu.  
* **Chronione maszyny**: źródłowy serwer hosta funkcji Hyper-V powinien mieć co najmniej jedną maszynę wirtualną, która ma być chroniona.

**Architektura ogólna**

![Ze środowiska lokalnego do środowiska lokalnego](./media/site-recovery-components/arch-onprem-onprem.png)

* [Dowiedz się więcej](site-recovery-vmm-to-vmm.md#azure-prerequisites) o wymaganiach dotyczących wdrożenia w portalu Azure.
* * [Dowiedz się więcej](site-recovery-vmm-to-vmm-classic.md#before-you-start) o wymaganiach dotyczących wdrożenia w portalu klasycznym.

## <a name="replicate-to-a-secondary-site-with-san-replication-hyper-v-vms-managed-by-vmm"></a>Replikacja do lokacji dodatkowej z replikacją sieci SAN: maszyny wirtualne funkcji Hyper-V zarządzane przez program VMM
Można replikować maszyny wirtualne funkcji Hyper-V zarządzane w chmurach VMM do lokacji dodatkowej przy użyciu replikacji sieci SAN w portalu klasycznym Azure. W nowym portalu Azure ten scenariusz nie jest obecnie obsługiwany.

W tym scenariuszu podczas wdrażania usługi Site Recovery zainstaluj dostawcę usług Azure Site Recovery na serwerach VMM. Dostawca koordynuje i organizuje replikację za pomocą usługi Site Recovery przez Internet. Dane są replikowane między tablicami magazynów podstawowych i pomocniczych przy użyciu synchronicznej replikacji sieci SAN.

Oto, co jest potrzebne:

**Konto Azure**: konieczna jest subskrypcja platformy Azure

* **Tablica sieci SAN**: [obsługiwana tablica sieci SAN](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) zarządzana przez podstawowy serwer VMM. Sieć SAN udostępnia infrastrukturę sieciową innej tablicy sieci SAN w lokacji dodatkowej.
* **Serwer VMM**: zalecany jest jeden serwer VMM w lokacji głównej i jeden w lokacji dodatkowej, każdy z nich zawierający co najmniej jedną prywatną chmurę VMM. Na serwerze powinien działać program System Center w wersji co najmniej 2012 SP1 z najnowszymi aktualizacjami i powinien on mieć połączenie z Internetem. Dla chmur należy ustawić profil funkcji Hyper-V.
* **Serwer funkcji Hyper-V**: serwery hosta funkcji Hyper-V znajdujące się w głównych i dodatkowych chmurach VMM. Na serwerze hosta powinien działać program System Center w wersji co najmniej 2012 z najnowszymi aktualizacjami i powinien on mieć połączenie z Internetem.
* **Chronione maszyny**: źródłowy serwer hosta funkcji Hyper-V powinien mieć co najmniej jedną maszynę wirtualną, która ma być chroniona.

**Architektura replikacji sieci SAN**

![Replikacja sieci SAN](./media/site-recovery-components/arch-onprem-onprem-san.png)

[Dowiedz się więcej](site-recovery-vmm-san.md#before-you-start) o wymaganiach dotyczących wdrożenia.

### <a name="on-premises"></a>Lokalnie
## <a name="hyper-v-protection-lifecycle"></a>Cykl życia ochrony funkcji Hyper-V
Ten przepływ pracy przedstawia proces ochrony, replikowania i przechodzenia w tryb failover maszyn wirtualnych funkcji Hyper-V.

1. **Włącz ochronę**: przeprowadzana jest konfiguracja magazynu usługi Site Recovery, konfiguracja ustawień replikacji dla chmury VMM lub lokacji funkcji Hyper-V oraz włączana jest ochrona maszyn wirtualnych. Inicjowane jest zadanie o nazwie **Włączanie ochrony**, które można monitorować na karcie **Zadania**. Zadanie sprawdza, czy maszyna spełnia wymagania wstępne, a następnie wywołuje metodę [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), która konfiguruje replikację na platformę Azure za pomocą określonych ustawień. Zadanie **Włączanie ochrony** wywołuje również metodę [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) w celu zainicjowania pełnej replikacji maszyny wirtualnej.
2. **Replikacja początkowa**: wykonywana jest migawka maszyny wirtualnej, a wirtualne dyski twarde są replikowane pojedynczo, dopóki wszystkie nie zostaną skopiowane na platformę Azure lub do dodatkowego centrum danych. Czas potrzebny na zakończenie tego procesu zależy od rozmiaru maszyny wirtualnej, przepustowości sieci i metody replikacji początkowej. Jeśli podczas replikacji początkowej będzie miała miejsce zmiana dysku, składnik Replica Replication Tracker funkcji Hyper-V będzie śledził te zmiany jako dzienniki replikacji funkcji Hyper-V (hrl), które znajdują się w tym samym folderze co dyski. Z każdym dyskiem jest skojarzony plik hrl, który zostanie wysłany do magazynu pomocniczego. Należy pamiętać, że pliki migawki i dziennika zużywają zasoby dysku w trakcie replikacji początkowej. Po zakończeniu replikacji początkowej migawka maszyny wirtualnej zostanie usunięta, a przyrostowe zmiany dysków w dzienniku zostaną zsynchronizowane i scalone.
3. **Finalizowanie ochrony**: po zakończeniu replikacji początkowej zadanie **Finalizowanie ochrony** konfiguruje ustawienia sieciowe i inne ustawienia po replikacji, aby maszyna wirtualna była chroniona. W przypadku przeprowadzania replikacji na platformę Azure może być konieczne dostosowanie ustawień maszyny wirtualnej w taki sposób, aby była gotowa do przejścia w tryb failover. W tym momencie możesz uruchomić testowe przejście w tryb failover w celu sprawdzenia, czy wszystko działa zgodnie z oczekiwaniami.
4. **Replikacja**: po przeprowadzeniu synchronizacji początkowej rozpoczynana jest replikacja przyrostowa zgodnie z ustawieniami replikacji.
   * **Niepowodzenie replikacji**: jeśli replikacja przyrostowa nie powiedzie się, a pełna replikacja będzie kosztowna pod względem przepustowości lub czasu, to zostanie przeprowadzona ponowna synchronizacja. Jeśli na przykład pliki hrl będą zajmować 50% rozmiaru dysku, to maszyna wirtualna zostanie oznaczona do ponownej synchronizacji. Ponowna synchronizacja minimalizuje ilość wysyłanych danych dzięki obliczaniu sum kontrolnych źródłowych i docelowych maszyn wirtualnych oraz wysyłaniu tylko danych przyrostowych. Po ukończeniu ponownej synchronizacji replikacja przyrostowa zostanie wznowiona. Domyślnie ponowna synchronizacja jest zaplanowana do automatycznego uruchamiania poza godzinami pracy, ale możliwe jest uruchomienie ponownej synchronizacji maszyny wirtualnej ręcznie.
   * **Błąd replikacji**: jeśli wystąpi błąd replikacji, to może zostać użyty wbudowany mechanizm ponawiania. Jeśli jest to błąd nieodwracalny, taki jak błąd uwierzytelniania lub autoryzacji, lub jeśli maszyna repliki jest w nieprawidłowym stanie, nie będzie podejmowana żadna próba ponowienia. Jeśli jest to błąd odwracalny, taki jak błąd sieci lub błąd dotyczący małej ilości miejsca/pamięci, to wtedy podejmowana jest próba ponowienia w rosnących odstępach czasu między kolejnymi próbami (1, 2, 4, 8, 10, a następnie co 30 minut).
5. **Planowane lub nieplanowane przejście w tryb failover**: w razie potrzeby można uruchomić planowane lub nieplanowane przejścia w tryb failover. Jeśli zostanie uruchomione planowane przejście w tryb failover, to źródłowe maszyny wirtualne zostaną wyłączone w celu zapewnienia, że nie będzie miała miejsca utrata danych. Po utworzeniu maszyny wirtualne replik mają stan oczekiwania na zatwierdzenie. Należy zatwierdzić je, aby zakończyć przejście w tryb failover, chyba że ma miejsce replikacja sieci SAN — w takim przypadku zatwierdzanie odbywa się automatycznie. Po skonfigurowaniu i uruchomieniu lokacji głównej może zostać wykonany powrót po awarii. W przypadku replikacji na platformę Azure odwrotna replikacja przebiega automatycznie. W przeciwnym razie należy rozpocząć replikację odwrotną ręcznie.

![przepływ pracy](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Następne kroki
[Przygotowanie do wdrożenia](site-recovery-best-practices.md)



<!--HONumber=Nov16_HO2-->


