---
title: "Macierz obsługi replikacji funkcji Hyper-V Azure | Dokumentacja firmy Microsoft"
description: "Podsumowanie obsługiwanych składników oraz wymagania dotyczące replikacji funkcji Hyper-V do platformy Azure z usługą Azure Site Recovery"
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 02/14/2018
ms.author: raynew
ms.openlocfilehash: 58d54c1e0e6aa88878b45400b9211396f5d1b9d5
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/22/2018
---
# <a name="support-matrix-for-hyper-v-replication-to-azure"></a>Macierz obsługi replikacji funkcji Hyper-V w systemie Azure


Ten artykuł zawiera podsumowanie obsługiwanych składników i ustawienia odzyskiwania po awarii maszyn wirtualnych funkcji Hyper-V lokalnej na platformie Azure, przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) usługi.


## <a name="supported-scenarios"></a>Obsługiwane scenariusze

**Scenariusz** | **Szczegóły**
--- | --- 
**Funkcja Hyper-V w programie VMM** | Można wykonać odzyskiwania po awarii do platformy Azure dla maszyn wirtualnych na hostach funkcji Hyper-V zarządzane w sieci szkieletowej programu System Center Virtual Machine Manager (VMM)<br/><br/> Można wdrożyć ten scenariusz, w portalu Azure lub przy użyciu programu PowerShell.<br/><br/> Gdy hosty funkcji Hyper-V są zarządzane przez program VMM, można także przeprowadzić odzyskiwania po awarii do lokacji dodatkowej lokalnymi. Odczyt [w tym artykule](tutorial-vmm-to-vmm.md) Aby dowiedzieć się więcej na temat tego scenariusza.
**Funkcja Hyper-V bez programu VMM** | Można wykonać odzyskiwania po awarii do platformy Azure dla maszyn wirtualnych na hostach funkcji Hyper-V, które nie są zarządzane przez program VMM.<br/><br/> Można wdrożyć ten scenariusz, w portalu Azure lub przy użyciu programu Powershell. 


## <a name="on-premises-servers"></a>Serwery lokalne

**Serwer** | Wymagania | **Szczegóły**
--- | --- | ---
**Funkcji Hyper-V (uruchomiony bez programu VMM)** | Windows Server 2016, Windows Server 2012 R2 z najnowszymi aktualizacjami. | Łączenie hostów z uruchomionym systemem Windows Server 2016 i 2012 R2 nie jest obsługiwana po skonfigurowaniu lokacji funkcji Hyper-V w usłudze Site Recovery.<br/><br/> Dla maszyn wirtualnych znajdujących się na hoście z systemem Windows Server 2016 odzyskiwania do alternatywnej lokalizacji nie jest obsługiwana.
**(Uruchomione z programem VMM) funkcji Hyper-V** | VMM 2016, VMM 2012 R2 | Jeśli używany jest program VMM, hostach z systemem Windows Server 2016 powinny być zarządzane w programie VMM 2016.<br/><br/> Chmur programu VMM, które łączy hostów Hyper-V z systemem Windows Server 2016 lub 2012 R2 nie jest obecnie obsługiwany.<br/><br/> Środowiska, które obejmują uaktualnienie istniejącego serwera programu VMM 2012 R2, aby 2016 nie są obsługiwane.


## <a name="replicated-vms"></a>Replikowane maszyny wirtualne


Poniższa tabela zawiera podsumowanie obsługi maszyny Wirtualnej. Usługa Site Recovery obsługuje dowolne obciążenia uruchomione na obsługiwanym systemie operacyjnym. 

 **Składnik** | **Szczegóły**
--- | ---
Konfiguracja maszyny Wirtualnej | Maszyny wirtualne, które są replikowane do platformy Azure musi spełniać [wymagania dotyczące usługi Azure](#failed-over-azure-vm-requirements).
System operacyjny gościa | Wszelkie system operacyjny gościa [obsługiwany przez platformę Azure](https://technet.microsoft.com/library/cc794868.aspx).<br/><br/> Windows Server 2016 Nano Server nie jest obsługiwana.




## <a name="hyper-v-network-configuration"></a>Konfiguracja sieci funkcji Hyper-V

**Składnik** | **Funkcja Hyper-V w programie VMM** | **Funkcja Hyper-V bez programu VMM**
--- | --- | ---
Host sieci: tworzenie zespołu kart sieciowych. | Yes
Host sieci: sieć VLAN | Yes
Host sieci: IPv4 | Yes
Host sieci: IPv6 | Nie
Maszyna wirtualna gościa sieci: tworzenie zespołu kart sieciowych. | Nie
Maszyna wirtualna gościa sieci: IPv4 | Yes
Maszyna wirtualna gościa sieci: IPv6 | Nie
Maszyna wirtualna gościa sieci: Static IP (z systemem Windows) | Yes
Maszyna wirtualna gościa sieci: Static IP (Linux) | Nie
Sieci maszyny Wirtualnej gościa: Multi-karty Sieciowej | Yes



## <a name="azure-vm-network-configuration-after-failover"></a>Konfiguracji sieci maszyny Wirtualnej platformy Azure (po trybu failover)

**Składnik** | **Funkcja Hyper-V w programie VMM** | **Funkcja Hyper-V bez programu VMM**
--- | --- | ---
ExpressRoute | Yes | Yes
Wewnętrzny moduł równoważenia obciążenia | Yes | Yes
ELB | Yes | Yes
Traffic Manager | Yes | Yes
Multi-NIC | Yes | Yes
Zastrzeżony adres IP | Yes | Yes
Protokół IPv4 | Yes | Yes
Zachowaj źródłowy adres IP | Yes | Yes
Punkty końcowe usługi sieci Wirtualnej<br/><br/> (Usługa azure storage zapory i sieci wirtualne) | Nie | Nie


## <a name="hyper-v-host-storage"></a>Magazyn hosta funkcji Hyper-V

**Storage** | **Funkcja Hyper-V w programie VMM** | Funkcja Hyper-V bez programu VMM
--- | --- | --- | ---
NFS | Nie dotyczy | Nie dotyczy
SMB 3.0 | Yes | Yes
SAN (ISCSI) | Yes | Yes
Wiele ścieżek (MPIO). Przetestowane z:<br></br> Microsoft DSM, EMC PowerPath 5.7 SP4<br/><br/> Program EMC PowerPath DSM dla CLARiiON | Yes | Yes

## <a name="hyper-v-vm-guest-storage"></a>Magazynu gościa maszyny Wirtualnej funkcji Hyper-V

**Storage** | **Funkcja Hyper-V w programie VMM** | Funkcja Hyper-V bez programu VMM
--- | --- | ---
VMDK | Nie dotyczy | Nie dotyczy
VHD/VHDX | Yes | Yes
Generacji 2 maszyn wirtualnych | Yes | Yes
EFI/UEFI| Yes | Yes
Udostępniony dysk klastra | Nie | Nie
Zaszyfrowanego dysku | Nie | Nie
NFS | Nie dotyczy | Nie dotyczy
SMB 3.0 | Nie | Nie
RDM | Nie dotyczy | Nie dotyczy
Na dysku > 1 TB | Tak, do 4095 GB | Tak, do 4095 GB
Dysku: sektor logiczny i fizyczny 4K | Nieobsługiwana: gł 1/Gen 2 | Nieobsługiwana: gł 1/Gen 2
Dysku: logicznych 4K i 512 bajtów sektora fizycznego | Yes |  Yes
Wolumin dysku rozłożone > 1 TB<br/><br/> Zarządzanie woluminami LVM logiczne | Yes | Yes
Funkcja miejsca do magazynowania | Yes | Yes
Dodaj lub usuń gorących dysku | Nie | Nie
Wykluczanie dysku | Yes | Yes
Wiele ścieżek (MPIO) | Yes | Yes

## <a name="azure-storage"></a>Azure Storage

**Składnik** | **Funkcja Hyper-V w programie VMM** | **Hyper-V bez programu VMM)**
--- | --- | ---
LRS | Yes | Yes
GRS | Yes | Yes
RA-GRS | Yes | Yes
Magazynu chłodnego | Nie | Nie
Magazynu gorącego| Nie | Nie
Obiekty BLOB typu Block | Nie | Nie
Szyfrowanie rest(SSE)| Yes | Yes
Premium Storage | Yes | Yes
Import/Eksport usługi | Nie | Nie
Sieć wirtualna punktów końcowych usługi (magazynu Azure zapory i sieci wirtualne), na docelowym kontem magazynu pamięci podręcznej używany do replikacji danych | Nie | Nie


## <a name="azure-compute-features"></a>Funkcje obliczeń platformy Azure

**Funkcja** | **Funkcja Hyper-V w programie VMM** | **Funkcja Hyper-V bez programu VMM**
--- | --- | ---
Zestawy dostępności | Yes | Yes
HUB | Yes | Yes  
Dyski zarządzane | Tak, dla trybu failover<br/><br/> Nie jest obsługiwana w przypadku powrotu po awarii dysków zarządzanych | Tak, dla trybu failover<br/><br/> Nie jest obsługiwana w przypadku powrotu po awarii dysków zarządzanych

## <a name="azure-vm-requirements"></a>Wymagania dotyczące maszyny Wirtualnej platformy Azure

Lokalnych maszyn wirtualnych, które są replikowane do platformy Azure musi spełniać wymagania maszyny Wirtualnej Azure podsumowaniem w poniższej tabeli.

**Składnik** | Wymagania | **Szczegóły**
--- | --- | ---
System operacyjny gościa | Usługa Site Recovery obsługuje wszystkich systemów operacyjnych, które są [obsługiwany przez platformę Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).  | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Architektura systemu operacyjnego gościa** | 64-bitowa | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Rozmiar dysku systemu operacyjnego** | Do 2048 GB dla maszyny wirtualne generacji 1.<br/><br/> Dla maszyn wirtualnych generacji 2 do 300 GB.  | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Liczba dysków systemu operacyjnego** | 1 | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Liczba dysków danych** | 16 lub mniej  | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Rozmiar wirtualnego dysku twardego dysku danych** | Do 4095 GB | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Karty sieciowe** | Wiele kart sieciowych są obsługiwane. |
**Udostępniony wirtualny dysk twardy** | Nieobsługiwane | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli jest nieobsługiwany.
**FC dysku** | Nieobsługiwane | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Format dysku twardego** | WIRTUALNEGO DYSKU TWARDEGO <br/><br/> VHDX | Usługa Site Recovery automatycznie konwertuje VHDX do wirtualnego dysku twardego, gdy awaryjnie na platformie Azure. Gdy nie powiedzie się do lokalnych maszyn wirtualnych w dalszym ciągu korzystać z formatu VHDX.
**Bitlocker** | Nieobsługiwane | Przed włączeniem replikacji dla maszyny Wirtualnej, należy wyłączyć funkcję BitLocker.
**Nazwa maszyny Wirtualnej** | Od 1 do 63 znaków. Ograniczone do litery, cyfry i łączniki. Nazwa maszyny Wirtualnej musi zaczynać i kończyć literą lub cyfrą. | Zaktualizuj tę wartość właściwości maszyny Wirtualnej w usłudze Site Recovery.
**Typu maszyny Wirtualnej** | Generacja 1<br/><br/> Generacja 2 — systemu Windows | Maszyny wirtualne generacji 2 z typem dysku systemu operacyjnego, Basic (zawierającej co najmniej dwa woluminy danych w formacie VHDX) i mniejsza niż 300 GB miejsca na dysku są obsługiwane.<br></br>Maszyn wirtualnych systemu Linux generacji 2 nie są obsługiwane. [Dowiedz się więcej](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="recovery-services-vault-actions"></a>Akcje magazyn usług odzyskiwania

**Akcja** |  **Funkcja Hyper-V w programie VMM** | **Funkcja Hyper-V bez programu VMM**
--- | --- | --- 
Przenieś magazyn między grupami zasobów<br/><br/> W obrębie subskrypcji oraz | Nie | Nie 
Przenieść magazyn, sieć, maszyn wirtualnych platformy Azure w grupach zasobów<br/><br/> W obrębie subskrypcji oraz | Nie | Nie 


## <a name="provider-and-agent"></a>Dostawca i Agent

Aby upewnić się, że wdrożenie jest zgodny z ustawieniami w tym artykule, upewnij się, że jest uruchomiona najnowsza wersja dostawcy i wersje agentów.

**Nazwa** | **Opis** | **Szczegóły**
--- | --- | --- | --- | ---
**Dostawca usługi Azure Site Recovery** | Współrzędne komunikacji między serwerami lokalnymi i Azure <br/><br/> Funkcja Hyper-V z programem VMM: zainstalowane na serwerach VMM<br/><br/> Funkcja Hyper-V bez programu VMM: zainstalowanych na hostach funkcji Hyper-V| Najnowsza wersja: 5.1.2700.1 (dostępne w portalu)<br/><br/> [Najnowsze funkcje i poprawki](https://aka.ms/latest_asr_updates)
**Agent usług odzyskiwania Azure firmy Microsoft (MARS)** | Koordynuje replikację między maszynami wirtualnymi funkcji Hyper-V i platformą Azure<br/><br/> Zainstalowane na serwerach funkcji Hyper-V lokalnej (z lub bez programu VMM) | Najnowsza wersja agenta dostępna z portalu






## <a name="next-steps"></a>Kolejne kroki
Dowiedz się, jak [przygotowanie Azure](tutorial-prepare-azure.md) odzyskiwania po awarii maszyn wirtualnych funkcji Hyper-V lokalnymi. 
