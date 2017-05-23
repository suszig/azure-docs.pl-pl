---
title: "Co to jest usługa Site Recovery? | Microsoft Docs"
description: "Zawiera omówienie usługi Azure Site Recovery oraz podsumowanie scenariuszy wdrażania."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: e9b97b00-0c92-4970-ae92-5166a4d43b68
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/14/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 89921997f68bd72ee6086d2b635361eeeaba1c28
ms.contentlocale: pl-pl
ms.lasthandoff: 05/11/2017


---
# <a name="what-is-site-recovery"></a>Co to jest usługa Site Recovery?

Usługa Azure Site Recovery — Zapraszamy! Ten artykuł zawiera krótkie omówienie usługi.

Zjawiska naturalne i problemy operacyjne mogą powodować awarie. Twoja organizacja wymaga strategii zapewniającej ciągłość biznesową i odzyskiwanie po awarii (BCDR, Business Continuity and Disaster Recovery), dzięki której podczas planowanych lub nieplanowanych przestojów dane będą bezpieczne, a aplikacje pozostaną dostępne, oraz będą jak najszybciej przywracane do normalnych warunków roboczych.

W celu realizacji strategii BCDR można wykorzystać usługi Azure Recovery Services. Usługa [Azure Backup](https://docs.microsoft.com/en-us/azure/backup/) zapewnia bezpieczeństwo danych i możliwość ich odzyskiwania. Usługa Site Recovery pozwala replikować i odzyskiwać obciążenia oraz przełączać je w tryb failover, co gwarantuje ich dostępność w razie awarii.

## <a name="what-does-site-recovery-provide"></a>Co umożliwia usługa Site Recovery?

- **Odzyskiwanie po awarii w chmurze** — obciążenia działające na maszynach wirtualnych i serwerach fizycznych można replikować na platformę Azure zamiast do lokacji dodatkowej. Pozwala to wyeliminować koszty i ograniczyć złożoność obsługi pomocniczego centrum danych.
- **Elastyczna replikacja w środowiskach hybrydowych** — można replikować dowolne obciążenia uruchomione na obsługiwanych lokalnych maszynach wirtualnych funkcji Hyper-V, maszynach wirtualnych VMware oraz serwerach fizycznych z systemem Windows/Linux.
- **Migracja** — usługi Site Recovery można użyć do przeprowadzenia migracji wystąpień usługi AWS do maszyn wirtualnych platformy Azure lub do przeprowadzenia migracji maszyn wirtualnych platformy Azure między regionami świadczenia usługi Azure.
- **Uproszczenie strategii BCDR** — replikację można wdrożyć z jednej lokalizacji w witrynie Azure Portal.  Można uruchamiać proste tryby failover i funkcję powrotu po awarii dla jednej lub wielu maszyn.
- **Odporność** — usługa Site Recovery organizuje replikację i pracę w trybie failover, ale nie przechwytuje danych aplikacji.
Replikowane dane są przechowywane w usłudze Azure Storage, która zapewnia odpowiednią odporność. W przypadku przejścia do trybu failover maszyny wirtualne platformy Azure są tworzone przy użyciu replikowanych danych.
- **Wydajność replikacji** — usługa Site Recovery umożliwia przeprowadzanie replikacji ciągłej w przypadku maszyn VMware oraz replikacji z częstotliwością wynoszącą nawet 30 sekund w przypadku funkcji Hyper-V. Za pomocą progów celu punktu odzyskiwania (RPO, recovery point objective) można sterować częstotliwością tworzenia punktów odzyskiwania danych. Dzięki automatyzacji procesu odzyskiwania usługi Site Recovery oraz integracji z usługą [Azure Traffic Manager](https://azure.microsoft.com/en-us/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) można zmniejszyć cel czasu odzyskiwania (RTO, recovery time objective).
- **Spójność aplikacji** — maszyny są replikowane przy użyciu migawek spójnych z aplikacjami. Oprócz danych dyskowych migawki spójne z aplikacjami przechwytują wszystkie dane w pamięci i wszystkie trwające transakcje.
- **Przeprowadzanie testów bez zakłóceń** — korzystając z testowych trybów failover, można łatwo przetestować próbne odzyskiwanie po awarii bez wpływu na środowiska produkcyjne.
- **Elastyczne odzyskiwanie i uruchamianie trybu failover** — w przypadku przewidywanych przerw w działaniu można uruchomić planowane tryby failover bez utraty danych, a w przypadku nieoczekiwanych awarii — nieplanowane tryby failover, które mogą wiązać się z minimalną utratą danych (zależnie od częstotliwości replikacji). Kiedy lokacja główna będzie znowu dostępna, można łatwo do niej wrócić.
- **Niestandardowe plany odzyskiwania** — plany odzyskiwania umożliwiają modelowanie i dostosowywanie trybu failover oraz odzyskiwania aplikacji wielowarstwowych, które obejmują wiele maszyn wirtualnych. Można ustalać kolejność grup w planach oraz dodawać skrypty i działania ręczne. Plany odzyskiwania można integrować z elementami runbook usługi Azure Automation.
- **Aplikacje wielowarstwowe** — można tworzyć plany odzyskiwania na potrzeby sekwencji trybu failover i odzyskiwania aplikacji wielowarstwowych. W ramach planu odzyskiwania można pogrupować maszyny w różne warstwy (na przykład bazy danych, sieci Web lub aplikacji) oraz dostosować sposób, w jaki poszczególne grupy przechodzą do trybu failover i są uruchamiane.
* **Integracja z istniejącymi technologiami BCDR** — usługa Site Recovery współdziała z innymi technologiami w ramach strategii BCDR. Usługi Site Recovery można na przykład używać, aby chronić zaplecze obciążeń firmowych programu SQL Server. Usługa oferuje natywne wsparcie funkcji AlwaysOn programu SQL Server, umożliwiając zarządzanie trybem failover dla grup dostępności.
* **Integracja z biblioteką automatyzacji** — bogata biblioteka usługi Azure Automation zapewnia gotowe do produkcji, dopasowane do aplikacji skrypty, które można pobrać i zintegrować z usługą Site Recovery.
* **Proste zarządzanie siecią** — zaawansowane zarządzanie siecią w usłudze Site Recovery i na platformie Azure upraszcza wymagania sieciowe aplikacji, w tym rezerwowanie adresów IP, konfigurowanie równoważenia obciążenia i integrowanie usługi Azure Traffic Manager w celu zapewnienia efektywnych przełączeń sieci.


## <a name="whats-supported"></a>Jakie operacje są obsługiwane?

**Obsługiwane** | **Szczegóły**
--- | ---
**Które regiony są obsługiwane na potrzeby usługi Site Recovery?** | [Obsługiwane regiony](https://azure.microsoft.com/en-us/regions/services/) |
**Co można replikować?** | Lokalne maszyny wirtualne VMware, maszyny wirtualne funkcji Hyper-V oraz serwery fizyczne z systemami Windows i Linux.
**Które systemy operacyjne są potrzebne dla zreplikowanych maszyn?** | [Obsługiwane systemy operacyjne](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) dla maszyn wirtualnych programu VMware<br/><br/> W przypadku maszyn wirtualnych funkcji Hyper-V obsługiwane są wszystkie [systemy operacyjne gościa](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) obsługiwane przez platformę Azure i funkcję Hyper-V.<br/><br/> [Systemy operacyjne](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) serwerów fizycznych
**Gdzie można replikować?** | Do magazynu platformy Azure lub do pomocniczego centrum danych<br/><br/> W przypadku funkcji Hyper-V do pomocniczego centrum danych można replikować tylko maszyny wirtualne znajdujące się na hostach funkcji Hyper-V zarządzanych w chmurach programu System Center VMM.
**Jakie serwery/hosty VMware są potrzebne?** | Maszyny wirtualne VMware, które są przeznaczone do replikacji, mogą być zarządzane przez [obsługiwane hosty vSphere/serwery vCenter](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
**Jakie obciążenia można replikować?** | Można replikować dowolne obciążenia uruchomione na obsługiwanej maszynie replikacji. Dodatkowo zespół usługi Site Recovery wykonał specyficzne dla aplikacji testowanie na potrzeby [wielu aplikacji](site-recovery-workload.md#workload-summary).


## <a name="which-azure-portal"></a>Której witryny Azure Portal użyć?

* Usługę Site Recovery można wdrażać zarówno w nowszej witrynie [Azure Portal](https://portal.azure.com), jak i w [klasycznej witrynie Azure Portal](https://manage.windowsazure.com/).
* W klasycznej witrynie Azure Portal możliwe jest korzystanie z usługi Site Recovery przy użyciu klasycznego modelu zarządzania usługami.
* Witryna Azure Portal obsługuje zarówno model klasyczny, jak i nowszy [model wdrożenia usługi Resource Manager](../azure-resource-manager/resource-manager-deployment-model.md).
- Portalu klasycznego należy używać tylko do obsługi istniejących wdrożeń usługi Site Recovery. Nie można w nim tworzyć nowych magazynów.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [obsłudze obciążeń](site-recovery-workload.md)
* Dowiedz się więcej o [architekturze i składnikach usługi Site Recovery](site-recovery-components.md)

