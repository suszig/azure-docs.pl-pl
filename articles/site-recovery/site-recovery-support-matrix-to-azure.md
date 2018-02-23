---
title: "Azure Site Recovery macierz obsługi replikacji do platformy Azure | Dokumentacja firmy Microsoft"
description: "Podsumowanie obsługiwanych systemów operacyjnych oraz składniki usługi Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: Rajani-Janaki-Ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/06/2018
ms.author: rajanaki
ms.openlocfilehash: a17d0918ea5938daf81c469fd6402a7dc9764831
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="azure-site-recovery-support-matrix-for-replicating-from-on-premises-to-azure"></a>Azure Site Recovery macierz obsługi replikacji z lokalnych do platformy Azure


W tym artykule przedstawiono obsługiwane konfiguracje oraz składniki usługi Azure Site Recovery podczas replikacji i odzyskiwania na platformie Azure. Aby uzyskać więcej informacji o wymaganiach usługi Azure Site Recovery, zobacz [wymagania wstępne](site-recovery-prereq.md).

> [!NOTE]
> Upewnij się, należy zaktualizować najnowsza wersja dostawcy usługi Site Recovery i agent w celu uzyskania zgodności z aktualizacjami w macierzy obsługi.


## <a name="support-for-deployment-options"></a>Obsługa opcje wdrażania

**Wdrożenie** | **Serwer VMware/fizyczne** | **Funkcja Hyper-V (z/bez programu Virtual Machine Manager)** |
--- | --- | ---
**Azure portal** | Lokalnych maszyn wirtualnych VMware do magazynu Azure z usługą Azure Resource Manager lub klasycznego magazynu i sieci.<br/><br/> Tryb failover Resource Manager i klasycznych maszyn wirtualnych. | Lokalnych maszyn wirtualnych funkcji Hyper-V do magazynu Azure Resource Manager lub klasycznego magazynu i sieci.<br/><br/> Tryb failover Resource Manager i klasycznych maszyn wirtualnych.
**Portal klasyczny** | Tylko tryb konserwacji. Nie można utworzyć nowego magazynu. | Tylko tryb konserwacji.
**Program PowerShell** | Obsługiwane | Obsługiwane


## <a name="support-for-datacenter-management-servers"></a>Obsługa serwerów zarządzania Centrum danych

### <a name="virtualization-management-entities"></a>Jednostki zarządzania wirtualizacji

**Wdrożenie** | **Pomoc techniczna**
--- | ---
**Maszyna wirtualna oprogramowania VMware/fizyczne serwera** | vCenter 6.5, 6.0 lub 5.5
**Funkcja Hyper-V (z programu Virtual Machine Manager)** | Program System Center Virtual Machine Manager 2016 i System Center Virtual Machine Manager 2012 R2

  >[!Note]
  > W chmurze programu System Center Virtual Machine Manager 2016 z systemem Windows Server 2016 i 2012 R2 hostów nie jest obecnie obsługiwany.
  > Konfiguracje, które obejmują uaktualniania istniejącego programu SCVMM 2012 R2, aby 2016 nie jest obecnie obsługiwane.
### <a name="host-servers"></a>Serwery hosta

**Wdrożenie** | **Pomoc techniczna**
--- | ---
**Maszyna wirtualna oprogramowania VMware/fizyczne serwera** | vSphere 6.5, 6.0, 5.5
**Funkcja Hyper-V (z/bez programu Virtual Machine Manager)** | Windows Server 2016, Windows Server 2012 R2 z najnowszymi aktualizacjami.<br></br>Jeśli używany jest program SCVMM, hosty z systemem Windows Server 2016 zarządzane w programie SCVMM 2016.


  >[!Note]
  >Lokacji funkcji Hyper-V, która łączy hostach z systemem Windows Server 2016 i 2012 R2 nie jest obecnie obsługiwany. Odzyskiwanie do lokalizacji alternatywnej dla maszyn wirtualnych na hoście systemu Windows Server 2016 nie jest obecnie obsługiwany.

## <a name="support-for-replicated-machine-os-versions"></a>Obsługa wersji systemu operacyjnego zreplikowanej maszyny

Maszyny wirtualne, które są chronione muszą spełniać [wymagania dotyczące usługi Azure](#failed-over-azure-vm-requirements) podczas replikacji do platformy Azure.
W poniższej tabeli przedstawiono obsługę systemu operacyjnego replikowanych w różnych scenariuszy wdrażania podczas korzystania z usługi Azure Site Recovery. Ta obsługa dotyczy dowolne obciążenia uruchomione na wymienione systemu operacyjnego.

 **Serwer VMware/fizyczne** | **Funkcja Hyper-V (lub bez VMM)** |
--- | --- |
64-bitowego systemu Windows Server 2016 (instalacja Server Core, serwer z środowisko pulpitu)\*, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 z na co najmniej z dodatkiem SP1<br/><br/> Red Hat Enterprise Linux: 5.2-5.11, 6.1-6.9, 7.0 do 7,4<br/><br/>CentOS: 5.2-5.11, 6.1-6.9, 7.0 do 7,4 <br/><br/>Ubuntu 14.04 LTS serwera[ (obsługiwane wersje jądra)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Ubuntu 16.04 LTS server[ (obsługiwane wersje jądra)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Debian 7 <br/><br/>Debian 8<br/><br/>Oracle Linux przedsiębiorstwa 6.4, 6.5 systemem Red Hat jądra zgodny lub podzielenie Enterprise jądra wersji 3 (UEK3) <br/><br/>SUSE Linux Enterprise Server 11 SP3 <br/><br/>SUSE Linux Enterprise Server 11 SP4 <br/>(Uaktualnienie replikowanie maszyn z SLES 11 z dodatkiem SP3 do SLES 11 z dodatkiem SP4 nie jest obsługiwane. Jeśli zreplikowanej maszyny został uaktualniony z SLES 11SP3 do SLES 11 z dodatkiem SP4, należy wyłączyć replikację i chronić komputer ponownie post uaktualnienia.) | Wszelkie system operacyjny gościa [obsługiwany przez platformę Azure](https://technet.microsoft.com/library/cc794868.aspx)

>[!NOTE]
>
> \* Windows Server 2016 Nano Server nie jest obsługiwane.
>
> Na dystrybucje systemu Linux obsługiwane są tylko podstawowe jądra będących częścią wersja pomocnicza wersji/aktualizacji dystrybucji.
>
> Uaktualnienia w wersjach głównych dystrybucji systemu Linux na platformie Azure Site Recovery chronione maszyny wirtualnej VMware lub serwera fizycznego nie jest obsługiwane. Podczas uaktualniania systemu operacyjnego w wersji głównych (na przykład CentOS 6.* do CentOS 7.*), wyłącz replikację dla maszyny, Uaktualnij system operacyjny na maszynie, a następnie włącz ponownie replikacji.
>


### <a name="supported-ubuntu-kernel-versions-for-vmwarephysical-servers"></a>Ubuntu wersji jądra programu VMware/serwery fizyczne

**Zlecenia** | **Wersja usługi mobilności** | Wersja jądra |
--- | --- | --- |
14.04 LTS | 9.10 | 3.13.0-24-Generic do 3.13.0-121-generic,<br/>3.16.0-25-Generic do 3.16.0-77-generic,<br/>3.19.0-18-Generic do 3.19.0-80-generic,<br/>4.2.0-18-Generic do 4.2.0-42-generic,<br/>4.4.0-21-Generic do 4.4.0-81-generic |
14.04 LTS | 9.11 | 3.13.0-24-Generic do 3.13.0-128-generic,<br/>3.16.0-25-Generic do 3.16.0-77-generic,<br/>3.19.0-18-Generic do 3.19.0-80-generic,<br/>4.2.0-18-Generic do 4.2.0-42-generic,<br/>4.4.0-21-Generic do 4.4.0-91-generic |
14.04 LTS | 9.12 | 3.13.0-24-Generic do 3.13.0-132-generic,<br/>3.16.0-25-Generic do 3.16.0-77-generic,<br/>3.19.0-18-Generic do 3.19.0-80-generic,<br/>4.2.0-18-Generic do 4.2.0-42-generic,<br/>4.4.0-21-Generic do 4.4.0-96-generic |
14.04 LTS | 9.13 | 3.13.0-24-Generic do 3.13.0-137-generic,<br/>3.16.0-25-Generic do 3.16.0-77-generic,<br/>3.19.0-18-Generic do 3.19.0-80-generic,<br/>4.2.0-18-Generic do 4.2.0-42-generic,<br/>4.4.0-21-Generic do 4.4.0-104-generic |
16.04 LTS | 9.10 | 4.4.0-21-Generic do 4.4.0-81-generic,<br/>4.8.0-34-Generic do 4.8.0-56-generic,<br/>4.10.0-14-Generic do 4.10.0-24-generic |
16.04 LTS | 9.11 | 4.4.0-21-Generic do 4.4.0-91-generic,<br/>4.8.0-34-Generic do 4.8.0-58-generic,<br/>4.10.0-14-Generic do 4.10.0-32-generic |
16.04 LTS | 9.12 | 4.4.0-21-Generic do 4.4.0-96-generic,<br/>4.8.0-34-Generic do 4.8.0-58-generic,<br/>4.10.0-14-Generic do 4.10.0-35-generic |
16.04 LTS | 9.13 | 4.4.0-21-Generic do 4.4.0-104-generic,<br/>4.8.0-34-Generic do 4.8.0-58-generic,<br/>4.10.0-14-Generic do 4.10.0-42-generic |

## <a name="supported-file-systems-and-guest-storage-configurations-on-linux-vmwarephysical-servers"></a>Systemy plików obsługiwanych i konfiguracje magazynu gościa w systemie Linux (VMware/serwery fizyczne)

Poniższe systemy plików i magazynu konfiguracji oprogramowania są obsługiwane na serwerach Linux działających na serwerach programu VMware lub fizycznych:
* Systemy plików: ext3 ext4, ReiserFS (Suse Linux Enterprise Server tylko), XFS
* Menedżer woluminów: LVM2
* Oprogramowanie wielościeżkowego: mapowania urządzenia

Parawirtualnego systemu magazynu (urządzenia wyeksportowane przez sterowniki parawirtualnego systemu) nie są obsługiwane.<br/>
Urządzenia We/Wy bloku wielu kolejki nie są obsługiwane.<br/>
Serwery fizyczne z HP CCISS kontrolera magazynu nie są obsługiwane.<br/>

>[!Note]
> Na serwerach Linux następujące katalogi (jeśli skonfigurowany jako osobne partycje /-systemów plików) musi składać się na tym samym dysku (dysk systemu operacyjnego) na serwerze źródłowym: / (root), / Boot usr, /usr/local, /var, etc; i/Boot powinien znajdować się na partycji dysku i nie jest woluminem LVM<br/><br/>
>


## <a name="support-for-network-configuration"></a>Obsługa konfiguracji sieci
W poniższej tabeli podsumowano Obsługa konfiguracji sieci w różnych scenariuszy wdrażania, które używają usługi Azure Site Recovery można replikować do platformy Azure.

### <a name="host-network-configuration"></a>Konfiguracja sieci hosta

**Konfiguracja** | **Serwer VMware/fizyczne** | **Funkcja Hyper-V (z/bez programu Virtual Machine Manager)**
--- | --- | ---
Tworzenie zespołu kart sieciowych | Yes<br/><br/>Nieobsługiwane, gdy są replikowane maszyny fizycznej| Yes
VLAN | Yes | Yes
Protokół IPv4 | Yes | Yes
Protokół IPv6 | Nie | Nie

### <a name="guest-vm-network-configuration"></a>Konfiguracja sieci maszyny Wirtualnej gościa

**Konfiguracja** | **Serwer VMware/fizyczne** | **Funkcja Hyper-V (z/bez programu Virtual Machine Manager)**
--- | --- | ---
Tworzenie zespołu kart sieciowych | Nie | Nie
Protokół IPv4 | Yes | Yes
Protokół IPv6 | Nie | Nie
Statyczny adres IP (z systemem Windows) | Yes | Yes
Statyczny adres IP (Linux) | Yes <br/><br/>Maszyny wirtualne są skonfigurowane do używania protokołu DHCP w przypadku powrotu po awarii  | Nie
Multi-NIC | Yes | Yes

### <a name="failed-over-azure-vm-network-configuration"></a>Konfiguracja sieci maszyny Wirtualnej Azure przełączona w tryb failover

Sieć platformy Azure | **Serwer VMware/fizyczne** | **Funkcja Hyper-V (z/bez programu Virtual Machine Manager)**
--- | --- | ---
ExpressRoute | Yes | Yes
Wewnętrzny moduł równoważenia obciążenia | Yes | Yes
ELB | Yes | Yes
Traffic Manager | Yes | Yes
Multi-NIC | Yes | Yes
Zastrzeżony adres IP | Yes | Yes
Protokół IPv4 | Yes | Yes
Zachowaj źródłowy adres IP | Yes | Yes
Wirtualne sieci punktów końcowych usług (Azure Storage zapory i sieci wirtualne) | Nie | Nie


## <a name="support-for-storage"></a>Obsługa magazynu
W poniższej tabeli podsumowano Obsługa konfiguracji magazynu w różnych scenariuszy wdrażania, które używają usługi Azure Site Recovery można replikować do platformy Azure.

### <a name="host-storage-configuration"></a>Konfiguracja magazynu hosta

**Konfiguracja** | **Serwer VMware/fizyczne** | **Funkcja Hyper-V (z/bez programu Virtual Machine Manager)**
--- | --- | --- | ---
NFS | Tak, aby VMware<br/><br/> Nie dla serwerów fizycznych | ND
SMB 3.0 | ND | Yes
SAN (ISCSI) | Yes | Yes
Wiele ścieżek (MPIO)<br></br>Przetestowana: moduł DSM firmy Microsoft, EMC PowerPath 5.7 z dodatkiem SP4, EMC PowerPath DSM dla CLARiiON | Yes | Yes

### <a name="guest-or-physical-server-storage-configuration"></a>Gość lub serwerze fizycznym magazynu konfiguracji

**Konfiguracja** | **Serwer VMware/fizyczne** | **Funkcja Hyper-V (z/bez programu Virtual Machine Manager)**
--- | --- | ---
VMDK | Yes | ND
VHD/VHDX | ND | Yes
Gł 2 maszyny Wirtualnej | ND | Yes
EFI/UEFI| Migracja do usługi Azure dla systemu Windows Server 2012 i nowszych maszyny wirtualne VMware tylko. </br></br> ** Zobacz uwagi pod koniec tabeli.  | Yes
Udostępniony dysk klastra | Nie | Nie
Zaszyfrowanego dysku | Nie | Nie
NFS | Nie | ND
SMB 3.0 | Nie | Nie
RDM | Yes<br/><br/> Brak serwerów fizycznych | ND
Na dysku > 1 TB | Yes<br/><br/>Maksymalnie 4095 GB | Yes<br/><br/>Maksymalnie 4095 GB
Dysk o rozmiarze sektora fizycznego logicznych i 4 k 4K | Yes | Nie są obsługiwane w maszynach wirtualnych generacji 1<br/><br/>Nie są obsługiwane w maszynach wirtualnych generacji 2.
Dysk o logicznych 4K i rozmiar sektora fizycznego 512 bajtów | Yes |  Yes
Wolumin dysku rozłożone > 1 TB<br/><br/> Zarządzanie woluminami LVM logiczne | Yes | Yes
Funkcja miejsca do magazynowania | Nie | Yes
Dodaj lub usuń gorących dysku | Nie | Nie
Wykluczanie dysku | Yes | Yes
Wiele ścieżek (MPIO) | ND | Yes

> [!NOTE]
> ** UEFI rozruchu maszyn wirtualnych VMware z systemem Windows Server 2012 lub nowszym można poddać migracji do usługi Azure. Obowiązują następujące ograniczenia.
> - Migracja do platformy Azure. Powrót po awarii do lokalnej lokacji oprogramowania VMware nie jest obsługiwane.
> - Nie więcej niż 4 partycje są obsługiwane na dysku systemu operacyjnego serwera.
> - Wymagana wersja usługi mobilności odzyskiwania lokacji Azure 9.13 lub nowszej.
> - Nie jest obsługiwana na serwerach fizycznych.

**Magazyn platformy Azure** | **Serwer VMware/fizyczne** | **Funkcja Hyper-V (z/bez programu Virtual Machine Manager)**
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
Wirtualne sieci punktów końcowych usługi (usługi Azure Storage zapory i sieci wirtualne) skonfigurowany na docelowy magazyn konta, lub buforować używanych do przechowywania danych replikacji konta magazynu | Nie | Nie
Konta magazynu ogólnego przeznaczenia V2 (zarówno gorącego i chłodnej warstwy) | Nie | Nie


## <a name="support-for-azure-compute-configuration"></a>Obsługa konfiguracji obliczeń platformy Azure

**Obliczanie funkcji** | **Serwer VMware/fizyczne** | **Funkcja Hyper-V (z/bez programu Virtual Machine Manager)**
--- | --- | ---
Zestawy dostępności | Yes | Yes
HUB | Yes | Yes  
Dyski zarządzane | Yes | Yes<br/><br/>Powrót po awarii do środowiska lokalnego z maszyny Wirtualnej platformy Azure z dyskami zarządzanego nie jest obecnie obsługiwane.

## <a name="failed-over-azure-vm-requirements"></a>Wymagania dotyczące maszyny Wirtualnej Azure przełączona w tryb failover

Możesz wdrożyć usługę Site Recovery, aby replikować maszyny wirtualne i serwery fizyczne z dowolnym systemem operacyjnym obsługiwanym przez platformę Azure. Jest to większość wersji systemów Windows i Linux. Lokalna Usługa maszyn wirtualnych, które chcesz replikować muszą być zgodne z następującymi wymaganiami Azure podczas replikacji do platformy Azure.

**Entity** | Wymagania | **Szczegóły**
--- | --- | ---
System operacyjny gościa | Funkcja Hyper-V Azure replikacji: Usługa Site Recovery obsługuje wszystkich systemów operacyjnych, które są [obsługiwany przez platformę Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> VMware i replikacji na serwerze fizycznym: sprawdzanie systemu Windows i Linux [wymagania wstępne](site-recovery-vmware-to-azure-classic.md) | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Architektura systemu operacyjnego gościa** | 64-bitowa | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli nieobsługiwane
**Rozmiar dysku systemu operacyjnego** | 2048 GB, jeśli jest replikowana **maszyn wirtualnych VMware lub serwerów fizycznych do platformy Azure**.<br/><br/>Maksymalnie 2048 GB dla **funkcji Hyper-V generacji 1** maszyn wirtualnych.<br/><br/>Maksymalnie 300 GB dla **maszyn wirtualnych funkcji Hyper-V generacji 2**.  | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli nieobsługiwane
**Liczba dysków systemu operacyjnego** | 1 | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Liczba dysków danych** | Replikowanie 64 lub mniej, jeśli **maszyn wirtualnych VMware do platformy Azure**; 16 lub mniej przypadku replikowania **maszyn wirtualnych funkcji Hyper-V w systemie Azure** | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli nieobsługiwane
**Rozmiar wirtualnego dysku twardego dysku danych** | Do 4095 GB | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli nieobsługiwane
**Karty sieciowe** | Wiele kart sieciowych są obsługiwane. |
**Udostępniony wirtualny dysk twardy** | Nieobsługiwane | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli nieobsługiwane
**FC dysku** | Nieobsługiwane | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli nieobsługiwane
**Format dysku twardego** | WIRTUALNEGO DYSKU TWARDEGO <br/><br/> VHDX | Chociaż VHDX nie jest obecnie obsługiwany na platformie Azure, Usługa Site Recovery automatycznie konwertuje VHDX do wirtualnego dysku twardego, gdy awaryjnie na platformie Azure. Gdy nie powiedzie się do lokalnych maszyn wirtualnych w dalszym ciągu korzystać z formatu VHDX.
**Bitlocker** | Nieobsługiwane | Przed rozpoczęciem ochrony maszyny wirtualnej, należy wyłączyć funkcję BitLocker.
**Nazwa maszyny Wirtualnej** | Od 1 do 63 znaków. Ograniczone do litery, cyfry i łączniki. Nazwa maszyny Wirtualnej musi zaczynać i kończyć literą lub cyfrą. | Zaktualizuj wartość we właściwościach maszyny wirtualnej w usłudze Site Recovery.
**Typu maszyny Wirtualnej** | Generacja 1<br/><br/> Generacja 2 — systemu Windows | Maszyny wirtualne generacji 2 z typem dysku systemu operacyjnego, Basic (zawierającej co najmniej dwa woluminy danych w formacie VHDX) i mniejsza niż 300 GB miejsca na dysku są obsługiwane.<br></br>Maszyn wirtualnych systemu Linux generacji 2 nie są obsługiwane. [Dowiedz się więcej](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="support-for-recovery-services-vault-actions"></a>Wspieranie działań magazyn usług odzyskiwania

**Akcja** | **Serwer VMware/fizyczne** | **Funkcja Hyper-V (nie programu Virtual Machine Manager)** | **Funkcja Hyper-V (z programu Virtual Machine Manager)**
--- | --- | --- | ---
Przenieś magazyn między grupami zasobów<br/><br/> W obrębie subskrypcji oraz | Nie | Nie | Nie
Przenieść magazyn, sieć, maszyn wirtualnych platformy Azure w grupach zasobów<br/><br/> W obrębie subskrypcji oraz | Nie | Nie | Nie


## <a name="support-for-provider-and-agent"></a>Obsługa dostawca i Agent

**Nazwa** | **Opis** | **Najnowsza wersja** | **Szczegóły**
--- | --- | --- | --- | ---
**Dostawca usługi Azure Site Recovery** | Współrzędne komunikacji między serwerami lokalnymi i Azure <br/><br/> Zainstalowana na serwerach programu Virtual Machine Manager lokalnie lub na serwerach funkcji Hyper-V, jeśli serwer programu Virtual Machine Manager nie jest | 5.1.2700.1 (dostępne w portalu) | [Najnowsze funkcje i poprawki](https://aka.ms/latest_asr_updates)
**Usługi Azure Site Recovery Unified Instalatora (VMware do platformy Azure)** | Współrzędne komunikacji między serwerami lokalnymi VMware i Azure <br/><br/> Zainstalowana na lokalnych serwerach VMware | 9.12.4653.1 (dostępne w portalu) | [Najnowsze funkcje i poprawki](https://aka.ms/latest_asr_updates)
**Usługa mobilności** | Koordynuje replikację między lokalnymi VMware serwerów/serwery fizyczne i Azure/dodatkowej lokacji<br/><br/> Zainstalowana na maszynie Wirtualnej VMware lub serwerów fizycznych, które chcesz replikować  | 9.12.4653.1 (dostępne w portalu) | [Najnowsze funkcje i poprawki](https://aka.ms/latest_asr_updates)
**Agent usług odzyskiwania Azure firmy Microsoft (MARS)** | Koordynuje replikację między maszynami wirtualnymi funkcji Hyper-V i platformą Azure<br/><br/> Zainstalowane na serwerach funkcji Hyper-V lokalnej (z lub bez serwera programu Virtual Machine Manager) | Najnowszą wersję agenta (dostępne w portalu) |






## <a name="next-steps"></a>Kolejne kroki
[Sprawdzanie wymagań wstępnych](site-recovery-prereq.md)
