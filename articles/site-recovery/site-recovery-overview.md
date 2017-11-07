---
title: "Informacje o usłudze Azure Site Recovery | Microsoft Docs"
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
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 90f9fe5775f493298dad3b12f2be9d6da6cb480e
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="about-site-recovery"></a>Informacje o usłudze Site Recovery

Usługa Azure Site Recovery — Zapraszamy! Ten artykuł zawiera krótkie omówienie usługi.

## <a name="business-continuity-and-disaster-recovery-bcdr-with-azure-recovery-services"></a>Ciągłość działalności biznesowej i odzyskiwanie po awarii (BCDR, Business continuity and disaster recovery) w usługach Azure Recovery Services

Dla organizacji niezbędne jest ustalenie, w jaki sposób zamierza ona zapewnić bezpieczeństwo danych i ciągłość działania aplikacji/obciążeń podczas planowanych i nieplanowanych przestojów.

W celu realizacji strategii BCDR można wykorzystać usługi Azure Recovery Services:

- **Usługa Site Recovery**: usługa Site Recovery pomaga zapewnić ciągłość działalności biznesowej poprzez uruchamianie aplikacji na maszynach wirtualnych i serwerach fizycznych, które są dostępne, jeśli w danej lokacji wystąpi awaria. Usługa Site Recovery replikuje obciążenia uruchomione na maszynach wirtualnych i serwerach fizycznych, dzięki czemu są one nadal dostępne w lokalizacji dodatkowej, jeśli lokacja główna jest niedostępna. Gdy lokacja główna zostanie ponowne uruchomiona, obciążenia zostaną odzyskane w tej lokacji.
- **Usługa kopii zapasowej**: usługa [Azure Backup](https://docs.microsoft.com/azure/backup/) powoduje, że dane są bezpieczne i możliwe do odzyskania dzięki tworzeniu kopii zapasowej w systemie Azure.

Usługa Site Recovery może zarządzać replikacją dla:

- Maszyn wirtualnych platformy Azure replikowanych między regionami świadczenia usługi Azure.
- Lokalnych maszyn wirtualnych i serwerów fizycznych replikowanych do platformy Azure lub lokacji dodatkowej.


## <a name="what-does-site-recovery-provide"></a>Co umożliwia usługa Site Recovery?

**Funkcja** | **Szczegóły**
--- | ---
**Wdrożenie prostego rozwiązania BCDR** | Korzystając z usługi Site Recovery, z jednej lokalizacji w portalu Azure można skonfigurować replikację, pracę w trybie failover i powrót po awarii oraz nimi zarządzać.
**Replikowanie maszyn wirtualnych platformy Azure** | Strategię BCDR można skonfigurować w taki sposób, aby maszyny wirtualne platformy Azure były replikowane między regionami świadczenia usługi Azure.
**Replikowanie lokalnych maszyn wirtualnych poza siedzibą** | Lokalne maszyny wirtualne i serwery fizyczne można replikować do platformy Azure lub do dodatkowej lokalizacji lokalnej. Replikacja do platformy Azure pozwala wyeliminować koszty i złożoność związane z utrzymywaniem dodatkowego centrum danych.
**Replikowanie dowolnego obciążenia** | Można replikować dowolne obciążenia uruchomione na obsługiwanych maszynach wirtualnych platformy Azure, lokalnych maszynach wirtualnych funkcji Hyper-V, maszynach wirtualnych programu VMware oraz serwerach fizycznych z systemem Windows/Linux.
**Zapewnianie bezpieczeństwa danych i odporności na błędy** | Usługa Site Recovery organizuje replikację, ale nie przechwytuje danych aplikacji. Replikowane dane są przechowywane w usłudze Azure Storage, która zapewnia odpowiednią odporność. W przypadku przejścia do trybu failover maszyny wirtualne platformy Azure są tworzone przy użyciu replikowanych danych.
**Realizacja celów RTO i RPO** | Cele czasu odzyskiwania (RTO, recovery time objectives) i cele punktu odzyskiwania (RPO, recovery point objectives) mieszczą się w ramach limitów organizacji. Usługa Site Recovery umożliwia przeprowadzanie replikacji ciągłej w przypadku maszyn wirtualnych platformy Azure oraz maszyn wirtualnych programu VMware oraz replikacji nawet co 30 sekund w przypadku funkcji Hyper-V. Cele czasu odzyskiwania (RTO) można ograniczyć jeszcze bardziej dzięki integracji z usługą [Azure Traffic Manager](https://azure.microsoft.com/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/).
**Utrzymywanie spójności aplikacji w trybie failover** | Punkty odzyskiwania można skonfigurować za pomocą migawek dotyczących spójności aplikacji. Migawki spójne z aplikacjami przechwytują dane dyskowe, wszystkie dane w pamięci oraz wszystkie trwające transakcje.
**Przeprowadzanie testów bez zakłóceń** | Korzystając z testowych trybów failover, można łatwo przetestować odzyskiwanie po awarii bez wpływu na trwającą replikację.
**Uruchamianie elastycznych trybów failover** | W przypadku przewidywanych przerw w działaniu można uruchomić planowane tryby failover bez utraty danych, a w przypadku nieoczekiwanych awarii — nieplanowane tryby failover, które mogą wiązać się z minimalną utratą danych (zależnie od częstotliwości replikacji). Kiedy lokacja główna będzie znowu dostępna, można łatwo do niej wrócić.
**Tworzenie planów odzyskiwania** | Korzystając z planów odzyskiwania można dostosować i uszeregować tryby failover i odzyskiwanie aplikacji wielowarstwowych na wielu maszynach wirtualnych. W ramach planów można łączyć maszyny w grupy oraz dodawać skrypty i działania ręczne. Plany odzyskiwania można integrować z elementami runbook usługi Azure Automation.
**Integracja z istniejącymi technologiami BCDR** | Usługa Site Recovery integruje się z innymi technologiami BCDR. Usługi Site Recovery można na przykład używać, aby chronić zaplecze obciążeń firmowych programu SQL Server. Usługa oferuje natywne wsparcie funkcji AlwaysOn programu SQL Server, umożliwiając zarządzanie trybem failover dla grup dostępności.
**Integracja z biblioteką automatyzacji** | Bogata biblioteka usługi Azure Automation zapewnia gotowe do produkcji skrypty dopasowane do danych aplikacji, które można pobrać i zintegrować z usługą Site Recovery.
**Zarządzanie ustawieniami sieci** | Usługa Site Recovery integruje się z platformą Azure, upraszczając zarządzanie siecią na potrzeby aplikacji, w tym rezerwowanie adresów IP, konfigurowanie równoważenia obciążenia i integrowanie usługi Azure Traffic Manager w celu zapewnienia efektywnych przełączeń sieci.


## <a name="what-can-i-replicate"></a>Co mogę replikować?

**Obsługiwane** | **Szczegóły**
--- | ---
**Co można replikować?** | Maszyny wirtualne platformy Azure między regionami platformy Azure<br/><br/>  Lokalne maszyny wirtualne programu VMware, maszyny wirtualne funkcji Hyper-V oraz serwery fizyczne (z systemami Windows i Linux) do platformy Azure<br/><br/> Lokalne maszyny wirtualne programu VMware, maszyny wirtualne funkcji Hyper-V oraz serwery fizyczne do programu ​Virtual Machine Manager (VMM).
**Które regiony są obsługiwane na potrzeby usługi Site Recovery?** | [Obsługiwane regiony](https://azure.microsoft.com/regions/services/) |
**Które systemy operacyjne są potrzebne dla zreplikowanych maszyn?** | [Wymagania dotyczące maszyny wirtualnej platformy Azure](site-recovery-support-matrix-azure-to-azure.md#support-for-replicated-machine-os-versions)</br></br>[Wymagania dotyczące maszyny wirtualnej programu VMware](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)<br/><br/> W przypadku maszyn wirtualnych funkcji Hyper-V obsługiwane są wszystkie [systemy operacyjne gościa](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) obsługiwane przez platformę Azure i funkcję Hyper-V.<br/><br/> [Wymagania dotyczące serwera fizycznego](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)
**Jakie serwery/hosty VMware są potrzebne?** | Maszyny wirtualne programu VMware mogą znajdować się na [obsługiwanych hostach vSphere/serwerach vCenter](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).

**Jakie obciążenia można replikować?** Możesz replikować dowolne obciążenia uruchomione na obsługiwanej maszynie replikacji. Dodatkowo zespół usługi Site Recovery wykonał specyficzne dla aplikacji testowanie na potrzeby [wielu aplikacji](site-recovery-workload.md#workload-summary).



## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [obsłudze obciążeń](site-recovery-workload.md).
* Zapoznaj się z wprowadzeniem do [replikacji maszyn wirtualnych platformy Azure między regionami](azure-to-azure-quickstart.md). 
