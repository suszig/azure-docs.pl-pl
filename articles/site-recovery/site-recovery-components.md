---
title: W jaki sposób działa usługa Site Recovery? | Microsoft Docs
description: Ten artykuł zawiera omówienie architektury usługi Site Recovery
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: cfreeman
editor: ''

ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/13/2016
ms.author: raynew

---
# <a name="how-does-azure-site-recovery-work?"></a>W jaki sposób działa usługa Azure Site Recovery?
Przeczytaj ten artykuł, aby zrozumieć, jak zbudowana jest architektura usługi Azure Site Recovery, i poznać składniki, dzięki którym działa.

Zamieść wszelkie komentarze lub pytania pod tym artykułem lub na [forum Usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Omówienie
Potrzebna jest strategia zapewniająca ciągłość biznesową i odzyskiwanie po awarii (BCDR, business continuity and disaster recovery), w ramach której obciążenia i dane działają i pozostają dostępne podczas planowanych lub nieplanowanych przestojów oraz możliwie jak najszybciej są przywracane normalne warunki robocze. 

Usługa Site Recovery to usługa platformy Azure, która przyczynia się do realizacji strategii BCDR. Organizuje ona replikację lokalnych serwerów fizycznych i maszyn wirtualnych do chmury (platforma Azure) lub pomocniczego centrum danych. W przypadku wystąpienia awarii w lokalizacji głównej następuje przełączenie w trybie failover do lokalizacji dodatkowej, dzięki czemu aplikacje i obciążenia są nadal dostępne. Powrót do lokalizacji głównej następuje wtedy, gdy powróci ona do normalnego działania. Przeczytaj omówienie w temacie [Co to jest usługa Site Recovery?](site-recovery-overview.md)

## <a name="site-recovery-in-the-azure-portal"></a>Usługa Site Recovery w witrynie Azure Portal
Platforma Azure ma dwa różne [modele wdrażania](../resource-manager-deployment-model.md) związane z tworzeniem zasobów i pracą z nimi: Azure Resource Manager i klasyczny model zarządzania usługami. Platforma Azure ma dwa portale — [klasyczną witrynę Azure Portal](https://manage.windowsazure.com/), która obsługuje klasyczny model wdrażania, oraz [witrynę Azure Portal](https://portal.azure.com), która obsługuje oba modele wdrażania.

* Usługa Site Recovery jest dostępna zarówno w klasycznej, jak i w nowej witrynie Azure Portal.
* W klasycznej witrynie Azure Portal możliwe jest korzystanie z usługi Site Recovery przy użyciu klasycznego modelu zarządzania usługami.
* W witrynie Azure Portal możliwe jest korzystanie z wdrożeń opartych na modelu klasycznym lub na modelu z obsługą zasobów. 

Informacje przedstawione w tym artykule dotyczą wdrożeń klasycznych i przy użyciu witryny Azure Portal. Tabela zawiera podsumowanie wszystkich różnic.

| **Replikacja** | **Azure Portal** | **Portal klasyczny** |
| --- | --- | --- |
| **Replikacja maszyny wirtualnej VMware do platformy Azure** |Uproszczony proces wdrażania.<br/><br/> Przełącz maszyny wirtualne w tryb failover do magazynu klasycznego lub magazynu opartego na usłudze Resource Manager.<br/><br/> Przeprowadź replikację do magazynu klasycznego lub do magazynu opartego na usłudze Resource Manager.<br/><br/> Użyj sieci klasycznych lub sieci usługi Resource Manager w celu nawiązania połączenia z maszynami wirtualnymi platformy Azure po przejściu w tryb failover.<br/><br/> Użyj magazynu LRS lub GRS. |Przejdź w tryb failover tylko do magazynu klasycznego.<br/><br/> Użyj sieci klasycznych tylko w celu nawiązania połączenia z maszynami wirtualnymi po przejściu w tryb failover.<br/><br/> Użyj magazynu GR. |
| **Replikacja maszyn wirtualnych funkcji Hyper-V (bez programu VMM) do platformy Azure** |Uproszczony proces wdrażania.<br/><br/> Przełącz maszyny wirtualne w tryb failover do magazynu klasycznego lub magazynu opartego na usłudze Resource Manager.<br/><br/> Przeprowadź replikację do magazynu klasycznego lub do magazynu opartego na usłudze Resource Manager.<br/><br/> Użyj sieci klasycznych lub sieci usługi Resource Manager w celu nawiązania połączenia z maszynami wirtualnymi platformy Azure po przejściu w tryb failover. | |
| **Replikacja maszyn wirtualnych funkcji Hyper-V (z programem VMM) do platformy Azure** |Uproszczony proces wdrażania.<br/><br/> Przełącz maszyny wirtualne w tryb failover do magazynu klasycznego lub magazynu opartego na usłudze Resource Manager.<br/><br/> Przeprowadź replikację do magazynu klasycznego lub do magazynu opartego na usłudze Resource Manager.<br/><br/> Użyj sieci klasycznych lub sieci usługi Resource Manager w celu nawiązania połączenia z maszynami wirtualnymi platformy Azure po przejściu w tryb failover.<br/><br/> Należy skonfigurować mapowanie sieci |Przejdź w tryb failover tylko do magazynu klasycznego.<br/><br/> Użyj sieci klasycznych tylko w celu nawiązania połączenia z maszynami wirtualnymi po przejściu w tryb failover. |
| **Replikacja maszyn wirtualnych funkcji Hyper-V (z programem VMM) do lokacji dodatkowej** |Uproszczony proces wdrażania.<br/><br/> Przełącz maszyny wirtualne w tryb failover do magazynu klasycznego lub magazynu opartego na usłudze Resource Manager.<br/><br/> Przeprowadź replikację do magazynu klasycznego lub do magazynu opartego na usłudze Resource Manager.<br/><br/> Użyj sieci klasycznych lub sieci usługi Resource Manager w celu nawiązania połączenia z maszynami wirtualnymi platformy Azure po przejściu w tryb failover.<br/><br/> Należy skonfigurować mapowanie sieci |Przejdź w tryb failover tylko do magazynu klasycznego.<br/><br/> Użyj sieci klasycznych tylko w celu nawiązania połączenia z maszynami wirtualnymi po przejściu w tryb failover.<br/><br/> Możliwość skonfigurowania mapowania magazynu. <br/><br/> Replikacja sieci SAN nie jest obsługiwana. |

## <a name="deployment-scenarios"></a>Scenariusze wdrażania
Usługa Site Recovery może zostać wdrożona w celu organizowania replikacji w wielu scenariuszach:

* **Replikowanie maszyn wirtualnych VMware**: lokalne maszyny wirtualne VMware można replikować do magazynu platformy Azure lub do dodatkowego centrum danych.
* **Replikowanie maszyn fizycznych**: maszyny fizyczne z systemem Windows lub Linux można replikować do magazynu platformy Azure lub do dodatkowego centrum danych. Proces replikowania maszyn fizycznych jest prawie taki sam jak proces replikowania maszyn wirtualnych VMware.
* **Replikowanie maszyn wirtualnych funkcji Hyper-V**: jeśli hosty funkcji Hyper-V są zarządzane w chmurach programu System Center VMM, maszyny wirtualne można replikować na platformę Azure lub do dodatkowego centrum danych. Jeśli hosty nie są zarządzane przez program VMM, replikację można przeprowadzać tylko na platformę Azure. Maszyny wirtualne funkcji Hyper-V, które nie są zarządzane przez program VMM, można replikować do magazynu platformy Azure.
* **Migrowanie maszyn wirtualnych**: usługi Site Recovery można użyć do [przeprowadzenia migracji maszyn wirtualnych IaaS platformy Azure](site-recovery-migrate-azure-to-azure.md) między regionami lub do [przeprowadzenia migracji wystąpień systemu Windows usług AWS](site-recovery-migrate-aws-to-azure.md) do maszyn wirtualnych IaaS platformy Azure. Pełna replikacja nie jest obecnie obsługiwana. Możliwe jest replikowanie na potrzeby migracji (tryb failover), ale nie ma wtedy możliwości powrotu po awarii. 

Usługa Site Recovery może replikować większość aplikacji uruchomionych na tych maszynach wirtualnych lub serwerach fizycznych. Aby uzyskać pełne podsumowanie obsługiwanych aplikacji, zobacz [What workloads can Azure Site Recovery protect?](site-recovery-workload.md) (Jakie obciążenia może chronić usługa Azure Site Recovery?).

## <a name="replicate-vmware-virtual-machines-to-azure"></a>Replikowanie maszyn wirtualnych programu VMware na platformę Azure
![Rozszerzone](./media/site-recovery-components/arch-enhanced.png)

| **Składnik** | **Szczegóły** |
| --- | --- |
| **Azure** |**Konto**: wymagane jest konto platformy Azure.<br/><br/> **Magazyn**: wymagane jest konto magazynu Azure do przechowywania replikowanych danych. Można używać klasycznego konta magazynu lub konta magazynu usługi Resource Manager. Konto może być magazynem LRS lub GRS w przypadku wdrażania w witrynie Azure Portal. Replikowane dane są przechowywane w usłudze Azure Storage, a w przypadku przejścia w tryb failover uruchamiane są maszyny wirtualne platformy Azure.<br/><br/> **Sieć**: wymagana jest sieć wirtualna Azure, z którą maszyny wirtualne platformy Azure będą nawiązywać połączenie w przypadku ich utworzenia w trybie failover. W witrynie Azure Portal sieci można utworzyć w klasycznym modelu menedżera usługi lub w modelu opartym na usłudze Resource Manager. |
| **Lokalny serwer konfiguracji** |Wymagana jest lokalna maszyna z systemem Windows Server 2012 R2, na której działa serwer konfiguracji i inne składniki usługi Site Recovery.<br/><br/> Powinna to być maszyna wirtualna VMware o wysokiej dostępności.<br/><br/> Składniki na serwerze obejmują:<br/><br/> **Serwer konfiguracji**: służy do koordynowania komunikacji między środowiskiem lokalnym i platformą Azure oraz do zarządzania replikacją i odzyskiwaniem danych.<br/><br/> **Serwer przetwarzania**: działa jako brama replikacji. Odbiera dane replikacji z chronionych maszyn źródłowych, optymalizuje je przy użyciu pamięci podręcznej, kompresji i szyfrowania, a następnie wysyła dane do usługi Azure Storage. Obsługuje także instalację wypychaną usługi Mobility na chronionych maszynach i przeprowadza automatyczne odnajdywanie maszyn wirtualnych VMware. Wraz z rozwojem wdrożenia możliwe będzie dodawanie dodatkowych oddzielnych dedykowanych serwerów przetwarzania w celu obsługi coraz większej ilości danych związanych z ruchem replikacji.<br/><br/> **Główny serwer docelowy**: służy do obsługi replikacji danych podczas powrotu po awarii z platformy Azure. |
| **Lokalne serwery wirtualizacji** |Co najmniej jeden host vSphere. Zalecamy również serwer vCenter służący do zarządzania hostami. |
| **Maszyny wirtualne do replikacji** |Dla każdej maszyny wirtualnej VMware, która ma zostać zreplikowana na platformę Azure, konieczne jest zainstalowanie składnika usługi Mobility. Ta usługa służy do przechwytywania zapisów danych na maszynie i przekazywania ich do serwera przetwarzania. Ten składnik można zainstalować ręcznie lub można go wypchnąć i zainstalować automatycznie za pomocą serwera przetwarzania po włączeniu replikacji dla maszyny. |

* [Dowiedz się więcej](site-recovery-vmware-to-azure.md#azure-prerequisites) o wymaganiach dotyczących wdrożenia w witrynie Azure Portal.
* [Dowiedz się więcej](site-recovery-failback-azure-to-vmware.md) o powrocie po awarii w portalu Azure.

## <a name="replicate-physical-servers-to-azure"></a>Replikacja serwerów fizycznych do platformy Azure
![Rozszerzone](./media/site-recovery-components/arch-enhanced.png)

| **Składnik** | **Szczegóły** |
| --- | --- |
| **Azure** |**Konto**: wymagane jest konto platformy Azure.<br/><br/> **Magazyn**: wymagane jest konto magazynu Azure do przechowywania replikowanych danych. Można używać klasycznego konta magazynu lub konta magazynu usługi Resource Manager. Konto może być magazynem LRS lub GRS w przypadku wdrażania w witrynie Azure Portal. Replikowane dane są przechowywane w usłudze Azure Storage, a w przypadku przejścia w tryb failover uruchamiane są maszyny wirtualne platformy Azure.<br/><br/> **Sieć**: wymagana jest sieć wirtualna Azure, z którą maszyny wirtualne platformy Azure będą nawiązywać połączenie w przypadku ich utworzenia w trybie failover. W witrynie Azure Portal sieci można utworzyć w klasycznym modelu menedżera usługi lub w modelu opartym na usłudze Resource Manager. |
| **Lokalny serwer konfiguracji** |Wymagana jest lokalna maszyna z systemem Windows Server 2012 R2, na której działa serwer konfiguracji i inne składniki usługi Site Recovery.<br/><br/> Serwer może być wirtualny lub fizyczny.<br/><br/> Składniki na serwerze obejmują:<br/><br/> **Serwer konfiguracji**: służy do koordynowania komunikacji między środowiskiem lokalnym i platformą Azure oraz do zarządzania replikacją i odzyskiwaniem danych.<br/><br/> **Serwer przetwarzania**: działa jako brama replikacji. Odbiera dane replikacji z chronionych maszyn źródłowych, optymalizuje je przy użyciu pamięci podręcznej, kompresji i szyfrowania, a następnie wysyła dane do usługi Azure Storage. Obsługuje także instalację wypychaną usługi Mobility na chronionych maszynach i przeprowadza automatyczne odnajdywanie maszyn wirtualnych VMware. Wraz z rozwojem wdrożenia możliwe będzie dodawanie dodatkowych oddzielnych dedykowanych serwerów przetwarzania w celu obsługi coraz większej ilości danych związanych z ruchem replikacji.<br/><br/> **Główny serwer docelowy**: służy do obsługi replikacji danych podczas powrotu po awarii z platformy Azure. |
| **Maszyny do replikacji** |Dla każdej maszyny fizycznej z systemem Windows lub Linux, która ma zostać zreplikowana na platformę Azure, konieczne jest zainstalowanie składnika usługi Mobility. Ta usługa służy do przechwytywania zapisów danych na maszynie i przekazywania ich do serwera przetwarzania. Ten składnik można zainstalować ręcznie lub można go wypchnąć i zainstalować automatycznie za pomocą serwera przetwarzania po włączeniu replikacji dla maszyny. |
| **Powrót po awarii** |Powrót po awarii z lokalizacji fizycznej do lokalizacji fizycznej nie jest obsługiwany. Oznacza to, że w przypadku przejścia serwerów fizycznych w tryb failover na platformę Azure, a następnie ich powrotu po awarii, powrót musi się odbyć na maszynę wirtualną VMware. Nie można powracać po awarii do serwera fizycznego. Wymagana jest maszyna wirtualna platformy Azure, do której przeprowadzony będzie powrót po awarii. Jeśli nie wdrożono serwera konfiguracji jako maszyny wirtualnej VMware, należy skonfigurować osobny główny serwer docelowy jako maszynę wirtualną VMware. Jest to potrzebne, ponieważ główny serwer docelowy wchodzi w interakcję i łączy się z magazynem VMware, aby przywracać dyski do maszyny wirtualnej VMware. |

* [Dowiedz się więcej](site-recovery-vmware-to-azure.md#azure-prerequisites) o wymaganiach dotyczących wdrożenia w witrynie Azure Portal.
* [Dowiedz się więcej](site-recovery-failback-azure-to-vmware.md) o powrocie po awarii w portalu Azure.

## <a name="replicate-hyper-v-vms-not-managed-by-vmm-to-azure"></a>Replikacja maszyn wirtualnych funkcji Hyper-V niezarządzanych przez program VMM
![Z lokacji funkcji Hyper-V na platformę Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

| **Składnik** | **Szczegóły** |
| --- | --- |
| **Azure** |**Konto**: wymagane jest konto platformy Azure.<br/><br/> **Magazyn**: wymagane jest konto magazynu Azure do przechowywania replikowanych danych. Można używać klasycznego konta magazynu lub konta magazynu usługi Resource Manager. To konto musi być kontem GRS. Replikowane dane są przechowywane w usłudze Azure Storage, a w przypadku przejścia w tryb failover uruchamiane są maszyny wirtualne platformy Azure.<br/><br/> **Sieć**: wymagana jest sieć wirtualna Azure, z którą maszyny wirtualne platformy Azure będą nawiązywać połączenie w przypadku ich utworzenia w trybie failover. |
| **Hosty lub maszyny wirtualne funkcji Hyper-V** |Co najmniej jeden host funkcji Hyper-V, na którym działa co najmniej jedna maszyna wirtualna.<br/><br/> Dostawca usługi Site Recovery i agent usługi Recovery Services jest instalowany na każdym hoście podczas wdrożenia. |

* [Dowiedz się więcej](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) o wymaganiach dotyczących wdrożenia w witrynie Azure Portal.
* [Dowiedz się więcej](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) o wymaganiach dotyczących wdrożenia w portalu klasycznym.

## <a name="replicate-hyper-v-vms-managed-by-vmm-to-azure"></a>Replikacja maszyn wirtualnych funkcji Hyper-V zarządzanych przez program VMM na platformę Azure
![Z programu VMM na platformę Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

| **Składnik** | **Szczegóły** |
| --- | --- |
| **Azure** |**Konto**: wymagane jest konto platformy Azure.<br/><br/> **Magazyn**: wymagane jest konto magazynu Azure do przechowywania replikowanych danych. Można używać klasycznego konta magazynu lub konta magazynu usługi Resource Manager. To konto musi być kontem GRS. Replikowane dane są przechowywane w usłudze Azure Storage, a w przypadku przejścia w tryb failover uruchamiane są maszyny wirtualne platformy Azure.<br/><br/> **Sieć**: wymagana jest sieć wirtualna Azure, z którą maszyny wirtualne platformy Azure będą nawiązywać połączenie w przypadku ich utworzenia w trybie failover. |
| **Serwer VMM** |Wymagany jest co najmniej jeden lokalny serwer programu VMM z co najmniej jedną chmurą prywatną. Dostawca usługi Site Recovery jest instalowany na każdym serwerze podczas wdrożenia. |
| **Hosty lub maszyny wirtualne funkcji Hyper-V** |Co najmniej jeden host funkcji Hyper-V, na którym działa co najmniej jedna maszyna wirtualna.<br/><br/> Agent usług Recovery Services jest instalowany na każdym hoście podczas wdrażania. |

* [Dowiedz się więcej](site-recovery-vmm-to-azure.md#azure-requirements) o wymaganiach dotyczących wdrożenia w witrynie Azure Portal.
* [Dowiedz się więcej](site-recovery-vmm-to-azure-classic.md#before-you-start) o wymaganiach dotyczących wdrożenia w portalu klasycznym.

## <a name="replicate-vmware-virtual-machines-or-physical-server-to-a-secondary-site"></a>Replikowanie maszyn wirtualnych VMware lub serwera fizycznego do lokacji dodatkowej
![Z programu VMware do programu VMware](./media/site-recovery-components/vmware-to-vmware.png)

| **Składnik** | **Szczegóły** |
| --- | --- |
| **Azure** |**Konto**: ten scenariusz jest wdrażany przy użyciu programu InMage Scout. Aby go uzyskać, wymagane jest konto platformy Azure.<br/><br/> Po utworzeniu magazynu usługi Site Recovery należy pobrać program InMage Scout i zainstalować najnowsze aktualizacje w celu skonfigurowania wdrożenia. |
| **Lokacja główna** |**Serwer przetwarzania**: składnik serwera przetwarzania należy skonfigurować w lokacji głównej w celu obsługi pamięci podręcznej, kompresji i optymalizacji danych. Umożliwia on również obsługę instalacji wypychanej programu Unified Agent na maszynach, które chcesz chronić.<br/><br/> **Funkcja VMware ESX/ESXi i serwer vCenter**: wymagana jest funkcja hypervisor VMware ESX/ESXi i opcjonalnie serwer VMware vCenter.<br/><br/> **Maszyny wirtualne** |
| **Lokacja dodatkowa** |**Serwer konfiguracji**: serwer konfiguracji jest pierwszym instalowanym składnikiem. Jest on instalowany w lokacji dodatkowej w celu konfigurowania i monitorowania wdrożenia oraz zarządzania nim za pomocą witryny sieci Web zarządzania lub konsoli vContinuum. W ramach wdrożenia istnieje tylko jeden serwer konfiguracji i musi on zostać zainstalowany na maszynie z systemem Windows Server 2012 R2.<br/><br/> **Serwer vContinuum (lokacja dodatkowa)**: jest instalowany w tej samej lokalizacji co serwer konfiguracji. Zapewnia on konsolę do monitorowania chronionego środowiska i zarządzania nim. W domyślnej instalacji serwer vContinuum jest pierwszym głównym serwerem docelowym i jest na nim zainstalowany program Unified Agent.<br/><br/> **Główny serwer docelowy**: na głównym serwerze docelowym przechowywane są zreplikowane dane. Odbiera on dane z serwera przetwarzania i tworzy maszynę repliki w lokacji dodatkowej, a także znajdują się na nim punkty przechowywania danych. Liczba potrzebnych głównych serwerów docelowych zależy od liczby chronionych maszyn. Aby powrócić po awarii do lokacji głównej, konieczne będzie również posiadanie w tej sieci głównego serwera docelowego. |

Aby zreplikować maszyny wirtualne VMware lub serwery fizyczne do lokacji dodatkowej, konieczne będzie pobranie programu InMage Scout, który jest dołączony do subskrypcji usługi Azure Site Recovery. Można go pobrać z witryny Azure Portal lub klasycznej witryny Azure Portal.

Następnie należy skonfigurować serwery składników w każdej lokacji (konfiguracja, przetwarzanie, główny docelowy) oraz zainstalować program Unified Agent na maszynach, które mają zostać zreplikowane. Po początkowej replikacji agent na każdej maszynie wysyła zmiany replikacji przyrostowej na serwer przetwarzania. Serwer przetwarzania optymalizuje dane i transferuje je na główny serwer docelowy w lokacji dodatkowej. Serwer konfiguracji zarządza procesem replikacji.

## <a name="replicate-hyper-v-vms-managed-by-vmm-to-a-secondary-site"></a>Replikacja maszyn wirtualnych funkcji Hyper-V zarządzanych przez program VMM do lokacji dodatkowej
![Ze środowiska lokalnego do środowiska lokalnego](./media/site-recovery-components/arch-onprem-onprem.png)

| **Składnik** | **Szczegóły** |
| --- | --- |
| **Azure** |**Konto**: wymagane jest konto platformy Azure. |
| **Serwer VMM** |Zalecamy, aby jeden serwer VMM znajdował się w lokacji głównej, a jeden w lokacji dodatkowej. Każdy serwer wymaga co najmniej jednej chmury prywatnej.<br/><br/> Podczas wdrożenia na serwerze VMM instalowany jest dostawca usługi Azure Site Recovery. |
| **Hosty lub maszyny wirtualne funkcji Hyper-V** |Co najmniej jeden host funkcji Hyper-V uruchomiony w chmurach programu VMM w lokacji głównej i dodatkowej<br/><br/> Każdy host powinien mieć co najmniej jedną maszynę wirtualną do replikacji.<br/><br/>. Agent usług Recovery Services jest instalowany na każdym hoście podczas wdrażania. |

* [Dowiedz się więcej](site-recovery-vmm-to-vmm.md#azure-prerequisites) o wymaganiach dotyczących wdrożenia w witrynie Azure Portal.
* [Dowiedz się więcej](site-recovery-vmm-to-vmm-classic.md#before-you-start) o wymaganiach dotyczących wdrożenia w klasycznej witrynie Azure Portal.

## <a name="replicate-hyper-v-vms-managed-by-vmm-to-a-secondary-site-using-san-replication"></a>Replikacja maszyn wirtualnych funkcji Hyper-V zarządzanych przez program VMM do lokacji dodatkowej za pomocą replikacji sieci SAN
![Replikacja sieci SAN](./media/site-recovery-components/arch-onprem-onprem-san.png)

Maszyny wirtualne funkcji Hyper-V zarządzane w chmurach programu VMM można replikować do lokacji dodatkowej przy użyciu replikacji sieci SAN w portalu klasycznym Azure. W witrynie Azure Portal ten scenariusz nie jest obecnie obsługiwany.

| **Składnik** | **Szczegóły** |
| --- | --- |
| **Azure** |**Konto**: wymagane jest konto platformy Azure. |
| **Serwer VMM** |Zalecamy, aby jeden serwer VMM znajdował się w lokacji głównej, a jeden w lokacji dodatkowej. Każdy serwer wymaga co najmniej jednej chmury prywatnej.<br/><br/> Podczas wdrożenia na serwerze VMM instalowany jest dostawca usługi Azure Site Recovery. |
| **Sieć SAN** |Obsługiwana tablica sieci SAN zarządzana przez podstawowy serwer VMM.<br/><br/> Sieć SAN powinna udostępniać infrastrukturę sieciową innej tablicy sieci SAN w lokacji dodatkowej. |
| **Hosty lub maszyny wirtualne funkcji Hyper-V** |Co najmniej jeden host funkcji Hyper-V uruchomiony w chmurach programu VMM w lokacji głównej i dodatkowej<br/><br/> Każdy host powinien mieć co najmniej jedną maszynę wirtualną do replikacji.<br/><br/>. Agent usług Recovery Services jest instalowany na każdym hoście podczas wdrażania. |

W tym scenariuszu podczas wdrażania usługi Site Recovery zainstaluj dostawcę usług Azure Site Recovery na serwerach VMM. Dostawca koordynuje i organizuje replikację za pomocą usługi Site Recovery przez Internet. Dane są replikowane między tablicami magazynów podstawowych i pomocniczych przy użyciu synchronicznej replikacji sieci SAN.

[Dowiedz się więcej](site-recovery-vmm-san.md#before-you-start) o wymaganiach dotyczących wdrożenia.

## <a name="hyper-v-protection-lifecycle"></a>Cykl życia ochrony funkcji Hyper-V
Ten przepływ pracy przedstawia proces ochrony, replikowania i przechodzenia w tryb failover maszyn wirtualnych funkcji Hyper-V.

1. **Włącz ochronę**: przeprowadzana jest konfiguracja magazynu usługi Site Recovery, konfiguracja ustawień replikacji dla chmury VMM lub lokacji funkcji Hyper-V oraz włączana jest ochrona maszyn wirtualnych. Inicjowane jest zadanie o nazwie **Włączanie ochrony**, które można monitorować na karcie **Zadania**. Zadanie sprawdza, czy maszyna spełnia wymagania wstępne, a następnie wywołuje metodę [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), która konfiguruje replikację na platformę Azure za pomocą określonych ustawień. Zadanie **Włączanie ochrony** wywołuje również metodę [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) w celu zainicjowania pełnej replikacji maszyny wirtualnej.
2. **Replikacja początkowa**: wykonywana jest migawka maszyny wirtualnej, a wirtualne dyski twarde są replikowane pojedynczo, dopóki wszystkie nie zostaną skopiowane na platformę Azure lub do dodatkowego centrum danych. Czas potrzebny na zakończenie tego procesu zależy od rozmiaru maszyny wirtualnej, przepustowości sieci i metody replikacji początkowej. Jeśli podczas replikacji początkowej będzie miała miejsce zmiana dysku, składnik Replica Replication Tracker funkcji Hyper-V będzie śledził te zmiany jako dzienniki replikacji funkcji Hyper-V (hrl), które znajdują się w tym samym folderze co dyski. Z każdym dyskiem jest skojarzony plik hrl, który zostanie wysłany do magazynu pomocniczego. Należy pamiętać, że pliki migawki i dziennika zużywają zasoby dysku w trakcie replikacji początkowej. Po zakończeniu replikacji początkowej migawka maszyny wirtualnej zostanie usunięta, a przyrostowe zmiany dysków w dzienniku zostaną zsynchronizowane i scalone.
3. **Finalizowanie ochrony**: po zakończeniu replikacji początkowej zadanie **Finalizowanie ochrony** konfiguruje ustawienia sieciowe i inne ustawienia po replikacji, aby maszyna wirtualna była zabezpieczona. W przypadku przeprowadzania replikacji na platformę Azure może być konieczne dostosowanie ustawień maszyny wirtualnej w taki sposób, aby była gotowa do przejścia w tryb failover. W tym momencie możesz uruchomić testowe przejście w tryb failover w celu sprawdzenia, czy wszystko działa zgodnie z oczekiwaniami.
4. **Replikacja**: po przeprowadzeniu synchronizacji początkowej rozpoczynana jest replikacja przyrostowa zgodnie z ustawieniami replikacji.
   * **Niepowodzenie replikacji**: jeśli replikacja przyrostowa nie powiedzie się, a pełna replikacja będzie kosztowna pod względem przepustowości lub czasu, to zostanie przeprowadzona ponowna synchronizacja. Jeśli na przykład pliki hrl będą zajmować 50% rozmiaru dysku, to maszyna wirtualna zostanie oznaczona do ponownej synchronizacji. Ponowna synchronizacja minimalizuje ilość wysyłanych danych dzięki obliczaniu sum kontrolnych źródłowych i docelowych maszyn wirtualnych oraz wysyłaniu tylko danych przyrostowych. Po ukończeniu ponownej synchronizacji replikacja przyrostowa zostanie wznowiona. Domyślnie ponowna synchronizacja jest zaplanowana do automatycznego uruchamiania poza godzinami pracy, ale możliwe jest uruchomienie ponownej synchronizacji maszyny wirtualnej ręcznie.
   * **Błąd replikacji**: jeśli wystąpi błąd replikacji, to może zostać użyty wbudowany mechanizm ponawiania. Jeśli jest to błąd nieodwracalny, taki jak błąd uwierzytelniania lub autoryzacji, albo jeśli maszyna repliki jest w nieprawidłowym stanie, nie będzie podejmowana żadna próba ponowienia. Jeśli jest to błąd odwracalny, taki jak błąd sieci lub błąd dotyczący małej ilości miejsca/pamięci, to wtedy podejmowana jest próba ponowienia w rosnących odstępach czasu między kolejnymi próbami (1, 2, 4, 8, 10, a następnie co 30 minut).
5. **Planowane lub nieplanowane przejście w tryb failover**: w razie potrzeby można uruchomić planowane lub nieplanowane przejścia w tryb failover. Jeśli zostanie uruchomione planowane przejście w tryb failover, to źródłowe maszyny wirtualne zostaną wyłączone w celu zapewnienia, że nie będzie miała miejsca utrata danych. Po utworzeniu maszyny wirtualne replik mają stan oczekiwania na zatwierdzenie. Należy zatwierdzić je, aby zakończyć przejście w tryb failover, chyba że ma miejsce replikacja sieci SAN — w takim przypadku zatwierdzanie odbywa się automatycznie. Po skonfigurowaniu i uruchomieniu lokacji głównej może zostać wykonany powrót po awarii. W przypadku replikacji na platformę Azure odwrotna replikacja przebiega automatycznie. W przeciwnym razie należy rozpocząć replikację odwrotną ręcznie.

![przepływ pracy](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Następne kroki
[Przygotowanie do wdrożenia](site-recovery-best-practices.md)

<!--HONumber=Oct16_HO3-->


