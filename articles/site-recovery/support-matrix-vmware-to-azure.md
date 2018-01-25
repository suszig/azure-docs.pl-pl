---
title: "Azure Site Recovery macierz obsługi replikowania maszyn wirtualnych VMware i serwerów fizycznych do platformy Azure | Dokumentacja firmy Microsoft"
description: "Podsumowanie obsługiwanych systemów operacyjnych oraz składników do replikowania maszyn wirtualnych VMware do platformy Azure z usługą Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 01/11/2018
ms.author: raynew
ms.openlocfilehash: ead133318d8660e8b8f4b3e9c5dddb6d75878b19
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/25/2018
---
# <a name="support-matrix-for-vmware-and-physical-server-replication-to-azure"></a>Tabela wsparcia dla VMware i replikacja serwerów fizycznych do platformy Azure


Ten artykuł zawiera podsumowanie obsługiwanych składników i ustawienia odzyskiwania po awarii maszyn wirtualnych VMware do platformy Azure, przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) usługi.


## <a name="supported-scenarios"></a>Obsługiwane scenariusze

**Scenariusz** | **Szczegóły** 
--- | --- 
**Maszyny wirtualne VMware** | Można wykonać odzyskiwania po awarii do platformy Azure dla maszyn wirtualnych VMware lokalnymi. Można wdrożyć ten scenariusz, w portalu Azure lub przy użyciu programu PowerShell.
**Serwerów fizycznych** | Można wykonać odzyskiwania po awarii do platformy Azure dla lokalnych serwerów fizycznych systemu Windows i Linux. Można wdrożyć ten scenariusz, w portalu Azure.

## <a name="on-premises-virtualization-servers"></a>Lokalnych serwerów wirtualizacji

**Serwer** | **Wymagania** | **Szczegóły**
--- | --- | ---
**VMware** | vCenter Server 6.5 6.0 lub 5.5 lub vSphere 6.5, 6.0, 5.5 | Firma Microsoft zaleca się, że używany jest serwer vCenter
**Serwerów fizycznych** | Nie dotyczy


## <a name="replicated-machines"></a>Replikowane maszyny

Poniższa tabela zawiera podsumowanie obsługi replikacji dla maszyny. Usługa Site Recovery obsługuje replikację dowolne obciążenia uruchomione na komputerze z obsługiwanym systemem operacyjnym.

**Składnik** | **Szczegóły**
--- | ---
Konfiguracja maszyny | Komputery, które są replikowane do platformy Azure musi spełniać [wymagania dotyczące usługi Azure](#failed-over-azure-vm-requirements).
System operacyjny maszyny (system Windows) | 64-bitowego systemu Windows Server 2016 (instalacja Server Core, serwer z środowisko pulpitu)\*, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 z na co najmniej z dodatkiem SP1
System operacyjny maszyny (Linux) | Red Hat Enterprise Linux: 5.2-5.11, 6.1-6.9, 7.0 do 7,4 <br/><br/>CentOS: 5.2-5.11, 6.1-6.9, 7.0 do 7,4 <br/><br/>Ubuntu 14.04 LTS serwera[ (obsługiwane wersje jądra)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Ubuntu 16.04 LTS server[ (obsługiwane wersje jądra)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Debian 7 <br/><br/>Debian 8<br/><br/>Oracle Linux przedsiębiorstwa 6.4, 6.5 systemem Red Hat jądra zgodny lub podzielenie Enterprise jądra wersji 3 (UEK3) <br/><br/>SUSE Linux Enterprise Server 11 SP3 <br/><br/>SUSE Linux Enterprise Server 11 SP4 <br/>(Uaktualnienie replikowanie maszyn z SLES 11 z dodatkiem SP3 do SLES 11 z dodatkiem SP4 nie jest obsługiwane. Jeśli zreplikowanej maszyny został uaktualniony z SLES 11SP3 do SLES 11 z dodatkiem SP4, należy wyłączyć replikację i chronić komputer ponownie post uaktualnienia.)

>[!NOTE]
>
> - Na dystrybucje systemu Linux obsługiwane są tylko podstawowe jądra będących częścią wersja pomocnicza wersji/aktualizacji dystrybucji.
>
> - Uaktualnienia w wersjach głównych dystrybucji systemu Linux na platformie Azure Site Recovery chronione maszyny wirtualnej VMware lub serwera fizycznego nie jest obsługiwane. Podczas uaktualniania systemu operacyjnego w wersji głównych (na przykład CentOS 6.* do CentOS 7.*), wyłącz replikację dla maszyny, Uaktualnij system operacyjny na maszynie, a następnie włącz ponownie replikacji.
>

### <a name="ubuntu-kernel-versions"></a>Ubuntu wersji jądra


**Obsługiwana wersja** | **Wersja usługi mobilności** | **Wersja jądra** |
--- | --- | --- |
14.04 LTS | 9.10 | 3.13.0-24-Generic do 3.13.0-121-generic,<br/>3.16.0-25-Generic do 3.16.0-77-generic,<br/>3.19.0-18-Generic do 3.19.0-80-generic,<br/>4.2.0-18-Generic do 4.2.0-42-generic,<br/>4.4.0-21-Generic do 4.4.0-81-generic |
14.04 LTS | 9.11 | 3.13.0-24-Generic do 3.13.0-128-generic,<br/>3.16.0-25-Generic do 3.16.0-77-generic,<br/>3.19.0-18-Generic do 3.19.0-80-generic,<br/>4.2.0-18-Generic do 4.2.0-42-generic,<br/>4.4.0-21-Generic do 4.4.0-91-generic |
14.04 LTS | 9.12 | 3.13.0-24-Generic do 3.13.0-132-generic,<br/>3.16.0-25-Generic do 3.16.0-77-generic,<br/>3.19.0-18-Generic do 3.19.0-80-generic,<br/>4.2.0-18-Generic do 4.2.0-42-generic,<br/>4.4.0-21-Generic do 4.4.0-96-generic |
14.04 LTS | 9.13 | 3.13.0-24-Generic do 3.13.0-137-generic,<br/>3.16.0-25-Generic do 3.16.0-77-generic,<br/>3.19.0-18-Generic do 3.19.0-80-generic,<br/>4.2.0-18-Generic do 4.2.0-42-generic,<br/>4.4.0-21-Generic do 4.4.0-104-generic |
16.04 LTS | 9.10 | 4.4.0-21-Generic do 4.4.0-81-generic,<br/>4.8.0-34-Generic do 4.8.0-56-generic,<br/>4.10.0-14-Generic do 4.10.0-24-generic |
16.04 LTS | 9.11 | 4.4.0-21-Generic do 4.4.0-91-generic,<br/>4.8.0-34-Generic do 4.8.0-58-generic,<br/>4.10.0-14-Generic do 4.10.0-32-generic |
16.04 LTS | 9.12 | 4.4.0-21-Generic do 4.4.0-96-generic,<br/>4.8.0-34-Generic do 4.8.0-58-generic,<br/>4.10.0-14-Generic do 4.10.0-35-generic |
16.04 LTS | 9.13 | 4.4.0-21-Generic do 4.4.0-104-generic,<br/>4.8.0-34-Generic do 4.8.0-58-generic,<br/>4.10.0-14-Generic do 4.10.0-42-generic |

## <a name="linux-file-systemsguest-storage-configurations"></a>Konfiguracje magazynu systemów/gościa plików systemu Linux

**Składnik** | **Obsługiwane**
--- | ---
systemy plików | ext3, ext4, ReiserFS (tylko w systemie Suse Linux Enterprise Server), XFS
Menedżer woluminów | LVM2
Oprogramowanie wielościeżkowego | Mapowanie urządzeń
Urządzenia magazynujące parawirtualnego systemu | Wyeksportowane przez parawirtualnego systemu sterowników urządzeń nie są obsługiwane.
Blokuj wielu kolejki We/Wy urządzenia | Nieobsługiwane.
Serwery fizyczne z kontrolera magazynu HP CCISS | Nieobsługiwane.
Katalogi | Te katalogi (jeśli skonfigurowany jako osobne partycje /-systemów plików) musi składać się na tym samym dysku systemu operacyjnego na serwerze źródłowym: / (root), / Boot usr, /usr/local, /var, etc </br></br>  Jeśli / (root) wolumin jest LVM, a następnie/Boot musi znajdować się na innej partycji na tym samym dysku i nie jest woluminem LVM.<br/><br/>
XFSv5 | Funkcje XFSv5 na XFS systemów plików, takich jak metadanych sumy kontrolnej są obsługiwane z wersji 9.10 usługi mobilności i jego nowszych wersjach. Użyj narzędzia xfs_info, aby sprawdzić superblock XFS dla partycji. Jeśli ftype jest ustawiona na 1, XFSv5 funkcje są w użyciu.



## <a name="network"></a>Sieć

**Składnik** | **Obsługiwane** 
--- | --- 
Tworzenie zespołu kart sieciowych hosta | Obsługiwane w maszynach wirtualnych VMware <br/><br/>Nieobsługiwane w przypadku replikacji maszyny fizycznej.
Host sieci VLAN | Yes 
Sieci hostów protokołu IPv4 | Yes 
Sieci hostów protokołu IPv6 | Nie 
Gość serwera sieci zespołu kart interfejsu sieciowego | Nie 
Gość serwera sieci IPv4 | Yes 
Gość serwera sieci IPv6 | Nie 
Gość serwera sieci statycznego adresu IP (z systemem Windows) | Yes 
Gość serwera sieci statycznego adresu IP (Linux) | Yes <br/><br/>Maszyny wirtualne są skonfigurowane do używania protokołu DHCP w przypadku powrotu po awarii  
Wiele kart sieciowych sieci z serwerem gościa | Yes 


## <a name="azure-vm-network-after-failover"></a>Sieć maszyny Wirtualnej platformy Azure (po trybu failover)

**Składnik** | **Obsługiwane** 
--- | --- 
ExpressRoute | Yes 
Wewnętrzny moduł równoważenia obciążenia | Yes 
ELB | Yes 
Traffic Manager | Yes 
Multi-NIC | Yes 
Zastrzeżony adres IP | Yes 
Protokół IPv4 | Yes 
Zachowaj źródłowy adres IP | Yes 
Punkty końcowe usługi sieci wirtualnej<br/><br/> (Usługa azure storage zapory i sieci wirtualne) | Nie 


## <a name="storage"></a>Magazyn


**Składnik** | **Obsługiwane** 
--- | --- 
Host systemu plików NFS | Tak, aby VMware<br/><br/> Nie dla serwerów fizycznych 
Sieć SAN (ISCSI) hosta | Yes
Wiele ścieżek hosta (MPIO) | Tak — przetestowana: Microsoft DSM, EMC PowerPath 5.7 z dodatkiem SP4 EMC PowerPath DSM dla CLARiiON
Gość i serwerem VMDK | Yes 
Gość i serwerem interfejsem EFI/UEFI| Partial (migracja do platformy Azure dla systemu Windows Server 2012 i późniejsze). </br></br> ** Zobacz uwagi pod koniec tabeli.
Dysk udostępniony klaster gościa serwera | Nie 
Gość i serwerem zaszyfrowanego dysku | Nie 
Gość serwera systemu plików NFS | Nie 
Gość serwera SMB 3.0 | Nie
Gość i serwerem RDM | Yes<br/><br/> Brak serwerów fizycznych 
Gość i serwerem dysku > 1 TB | Yes<br/><br/>Do 4095 GB 
Gość i serwerem dysku o rozmiarze sektora fizycznego logicznych i 4 k 4K | Yes
Dysk serwera gościa z logicznych 4K i rozmiar sektora fizycznego 512 bajtów | Yes 
Wolumin serwera gościa z dysku rozłożone > 1 TB<br/><br/> Logiczne LVM woluminu gościa/serwer zarządzania — miejsca do magazynowania | Dysku dodawania i usuwania gorących gościa i serwerem | Gość/server - wykluczyć dysku | Tak wielościeżkowe gościa i serwerem (MPIO) | N/D

> [!NOTE]
> ** UEFI rozruchu maszyn wirtualnych VMware lub serwerów fizycznych z systemem Windows Server 2012 lub nowszym można poddać migracji do usługi Azure. Obowiązują następujące ograniczenia.
> - Tylko migracja do systemu Azure jest obsługiwana. Powrót po awarii do lokalnej lokacji oprogramowania VMware nie jest obsługiwane.
> - Serwer nie powinna mieć więcej niż 4 partycji na dysku systemu operacyjnego.
> - Wymagana wersja usługi mobilności odzyskiwania lokacji Azure 9.13 lub nowszej.


## <a name="azure-storage"></a>Azure Storage

**Składnik** | **Obsługiwane** 
--- | --- 
LRS | Yes 
GRS | Yes 
RA-GRS | Yes 
Magazynu chłodnego | Nie 
Magazynu gorącego| Nie 
Obiekty BLOB typu Block | Nie 
Szyfrowanie rest(SSE)| Yes 
Premium Storage | Yes 
Import/Eksport usługi | Nie 
Punkty końcowe usługi sieci wirtualnej<br/><br/> Usługa Azure storage zapory i sieci wirtualne skonfigurowane na docelowe konto magazynu pamięci podręcznej/magazynów (używane do przechowywania danych replikacji) | Nie 
Konta magazynu ogólnego przeznaczenia V2 (zarówno gorącego i chłodnej warstwy) | Nie 


## <a name="azure-compute"></a>Obliczeń platformy Azure

**Featuree** | **Obsługiwane** 
--- | --- 
Zestawy dostępności | Yes 
HUB | Yes   
Dyski zarządzane | Yes 

## <a name="azure-vm-requirements"></a>Wymagania dotyczące maszyny Wirtualnej platformy Azure

Lokalnych maszyn wirtualnych, które są replikowane do platformy Azure musi spełniać wymagania maszyny Wirtualnej Azure podsumowaniem w poniższej tabeli.

**Składnik** | **Wymagania** | **Szczegóły**
--- | --- | ---
**System operacyjny gościa** | Sprawdź [obsługiwanych systemów operacyjnych](#replicated machines) | Sprawdzanie wymagań wstępnych kończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Architektura systemu operacyjnego gościa** | 64-bitowa | Sprawdzanie wymagań wstępnych kończy się niepowodzeniem, jeśli nieobsługiwane
**Rozmiar dysku systemu operacyjnego** | Do 2048 GB | Sprawdzanie wymagań wstępnych kończy się niepowodzeniem, jeśli nieobsługiwane
**Liczba dysków systemu operacyjnego** | 1 | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Liczba dysków danych** | Replikowanie 64 lub mniej, jeśli **maszyn wirtualnych VMware do platformy Azure**; 16 lub mniej przypadku replikowania **maszyn wirtualnych funkcji Hyper-V w systemie Azure** | Sprawdzanie wymagań wstępnych zakończy się niepowodzeniem, jeśli nieobsługiwane
**Rozmiar wirtualnego dysku twardego dysku danych** | Do 4095 GB | Sprawdzanie wymagań wstępnych kończy się niepowodzeniem, jeśli nieobsługiwane
**Karty sieciowe** | Wiele kart sieciowych są obsługiwane. | 
**Udostępniony wirtualny dysk twardy** | Nieobsługiwane | Sprawdzanie wymagań wstępnych kończy się niepowodzeniem, jeśli nieobsługiwane
**FC dysku** | Nieobsługiwane | Sprawdzanie wymagań wstępnych kończy się niepowodzeniem, jeśli nieobsługiwane
**Format dysku twardego** | WIRTUALNEGO DYSKU TWARDEGO <br/><br/> VHDX | Chociaż VHDX nie jest obecnie obsługiwany na platformie Azure, Usługa Site Recovery automatycznie konwertuje VHDX do wirtualnego dysku twardego, gdy awaryjnie na platformie Azure. Gdy nie powiedzie się do lokalnych maszyn wirtualnych w dalszym ciągu korzystać z formatu VHDX.
**Bitlocker** | Nieobsługiwane | Przed włączeniem replikacji dla maszyny, należy wyłączyć funkcję BitLocker.
**Nazwa maszyny Wirtualnej** | 1 – 63 znaków.<br/><br/> Ograniczone do litery, cyfry i łączniki.<br/><br/> Nazwa maszyny musi zaczynać i kończyć literą lub cyfrą. | Zaktualizuj wartość we właściwościach maszyny w usłudze Site Recovery.
**Typu maszyny Wirtualnej** | Generacja 1<br/><br/> Generacja 2 — systemu Windows | Maszyny wirtualne generacji 2 z typem dysku systemu operacyjnego, Basic (zawierającej co najmniej dwa woluminy danych w formacie VHDX) i mniejsza niż 300 GB miejsca na dysku są obsługiwane.<br></br>Maszyn wirtualnych systemu Linux generacji 2 nie są obsługiwane. [Dowiedz się więcej](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="vault-tasks"></a>Magazyn zadań

**Akcja** | **Obsługiwane** 
--- | --- 
Przenieś magazyn między grupami zasobów<br/><br/> W obrębie subskrypcji oraz | Nie 
Przenieść magazyn, sieć, maszyn wirtualnych platformy Azure w grupach zasobów<br/><br/> W obrębie subskrypcji oraz | Nie 


## <a name="mobility-service"></a>Usługa mobilności

**Nazwa** | **Opis** | **Najnowsza wersja** | **Szczegóły**
--- | --- | --- | --- | ---
** Usługi azure Site Recovery Unified Instalatora ** | Współrzędne komunikacji między serwerami lokalnymi VMware i Azure <br/><br/> Zainstalowana na lokalnych serwerach VMware | 9.12.4653.1 (dostępne w portalu) | [Najnowsze funkcje i poprawki](https://aka.ms/latest_asr_updates)
**Usługa mobilności** | Koordynuje replikację między lokalnymi VMware serwerów/serwery fizyczne i Azure/dodatkowej lokacji<br/><br/> Zainstalowana na maszynie Wirtualnej VMware lub serwerów fizycznych, które chcesz replikować  | 9.12.4653.1 (dostępne w portalu) | [Najnowsze funkcje i poprawki](https://aka.ms/latest_asr_updates)


## <a name="next-steps"></a>Kolejne kroki
[Dowiedz się, jak](tutorial-prepare-azure.md) do przygotowania Azure odzyskiwania po awarii maszyn wirtualnych VMware.
