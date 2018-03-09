---
title: "Azure Site Recovery macierz obsługi replikowania maszyn wirtualnych VMware i serwerów fizycznych do platformy Azure | Dokumentacja firmy Microsoft"
description: "Podsumowanie obsługiwanych systemów operacyjnych oraz składników do replikowania maszyn wirtualnych VMware do platformy Azure przy użyciu usługi Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/07/2018
ms.author: raynew
ms.openlocfilehash: 10d7db60ef584632a45fd7cdc5877461fa45af03
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="support-matrix-for-vmware-and-physical-server-replication-to-azure"></a>Tabela wsparcia dla VMware i replikacja serwerów fizycznych do platformy Azure


Ten artykuł zawiera podsumowanie obsługiwanych składników i ustawienia odzyskiwania po awarii maszyn wirtualnych VMware do platformy Azure przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md).


## <a name="supported-scenarios"></a>Obsługiwane scenariusze

**Scenariusz** | **Szczegóły**
--- | ---
**Maszyny wirtualne VMware** | Można wykonać odzyskiwania po awarii do platformy Azure dla maszyn wirtualnych VMware lokalnymi. Można wdrożyć ten scenariusz, w portalu Azure lub za pomocą programu PowerShell.
**Serwerów fizycznych** | Można wykonać odzyskiwania po awarii do platformy Azure dla lokalnych serwerów fizycznych systemu Windows i Linux. Można wdrożyć ten scenariusz, w portalu Azure.

## <a name="on-premises-virtualization-servers"></a>Lokalnych serwerów wirtualizacji

**Serwer** | **Wymagania** | **Szczegóły**
--- | --- | ---
**VMware** | vCenter Server 6.5 w wersji 6.0 lub 5.5 lub vSphere 6.5, 6.0 lub 5.5 | Firma Microsoft zaleca użycie serwera vCenter.
**Serwerów fizycznych** | ND


## <a name="replicated-machines"></a>Replikowane maszyny

Poniższa tabela zawiera podsumowanie obsługi replikacji dla maszyny. Usługa Site Recovery obsługuje replikację dowolne obciążenia uruchomione na komputerze z obsługiwanym systemem operacyjnym.

**Składnik** | **Szczegóły**
--- | ---
Konfiguracja maszyny | Komputery, które są replikowane do platformy Azure musi spełniać [wymagania dotyczące usługi Azure](#failed-over-azure-vm-requirements).
System operacyjny maszyny (system Windows) | 64-bitowego systemu Windows Server 2016 (instalacja Server Core, serwer z środowisko pulpitu), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 z na co najmniej z dodatkiem SP1
System operacyjny maszyny (Linux) | Red Hat Enterprise Linux: 5.2-5.11, 6.1-6.9, 7.0 do 7,4 <br/><br/>CentOS: 5.2-5.11, 6.1-6.9, 7.0 do 7,4 <br/><br/>Ubuntu 14.04 LTS serwera[ (obsługiwane wersje jądra)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Ubuntu 16.04 LTS server[ (obsługiwane wersje jądra)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Debian 7 <br/><br/>Debian 8<br/><br/>Oracle Linux przedsiębiorstwa 6.4, 6.5 systemem Red Hat jądra zgodny lub podzielenie Enterprise jądra wersji 3 (UEK3) <br/><br/>SUSE Linux Enterprise Server 11 SP3 <br/><br/>SUSE Linux Enterprise Server 11 SP4 <br/>(Uaktualnienie replikowanie maszyn z SLES 11 z dodatkiem SP3 do SLES 11 z dodatkiem SP4 nie jest obsługiwane. Jeśli zreplikowanej maszyny jest uaktualniany z SLES 11SP3 do SLES 11 z dodatkiem SP4, należy wyłączyć replikację i chronić komputer ponownie post uaktualnienia.)

>[!NOTE]
>
> - Na dystrybucje systemu Linux obsługiwane są tylko podstawowe jądra będących częścią wersja pomocnicza wersji/aktualizacji dystrybucji.
>
> - Nie są obsługiwane uaktualnienia w wersjach głównych dystrybucji systemu Linux na maszyny wirtualnej VMware chronione odzyskiwania lokacji lub serwerze fizycznym. Po uaktualnieniu systemu operacyjnego w wersji głównych (na przykład CentOS 6.* do CentOS 7.*) Wyłącz replikację dla maszyny, Uaktualnij system operacyjny na maszynie, a następnie włącz ponownie replikacji.
>

### <a name="ubuntu-kernel-versions"></a>Ubuntu wersji jądra


**Obsługiwana wersja** | **Wersja usługi mobilności odzyskiwania lokacji Azure** | **Wersja jądra** |
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
systemy plików | ext3 ext4, XFS
Menedżer woluminów | LVM2
Urządzenia magazynujące parawirtualnego systemu | Urządzenia eksportowane przez sterowniki parawirtualne nie są obsługiwane.
Blokuj wielu kolejki We/Wy urządzenia | Nieobsługiwane.
Serwery fizyczne z kontrolera magazynu HP CCISS | Nieobsługiwane.
Katalogi | Te katalogi (jeśli skonfigurowany jako osobne partycje /-systemów plików) musi składać się na tym samym dysku systemu operacyjnego na serwerze źródłowym: / (root), / Boot usr, /usr/local, /var, etc. </br></br> Jeśli / (root) wolumin jest LVM, a następnie/Boot musi znajdować się na innej partycji na tym samym dysku i nie jest woluminem LVM.<br/><br/>
|Wymagania dotyczące wolnego miejsca| 2 GB na partycji/root <br/>250 MB na folder instalacji
XFSv5 | Funkcje XFSv5 na XFS systemów plików, takich jak metadanych sumy kontrolnej, są obsługiwane z 9.10 wersji usługi mobilności i nowszych. Użyj narzędzia xfs_info, aby sprawdzić superblock XFS dla partycji. Jeśli ftype jest ustawiona na 1, XFSv5 funkcje są w użyciu.



## <a name="network"></a>Sieć

**Składnik** | **Obsługiwane**
--- | ---
Tworzenie zespołu kart sieciowych hosta | Obsługiwane w maszynach wirtualnych VMware. <br/><br/>Nie jest obsługiwane dla komputera fizycznego replikacji.
Host sieci VLAN | Yes
Sieci hostów protokołu IPv4 | Yes
Sieci hostów protokołu IPv6 | Nie
Gość serwera sieci zespołu kart interfejsu sieciowego | Nie
Gość serwera sieci IPv4 | Yes
Gość serwera sieci IPv6 | Nie
Gość serwera sieci statycznego adresu IP (z systemem Windows) | Yes
Gość serwera sieci statycznego adresu IP (Linux) | Yes <br/><br/>Maszyny wirtualne są skonfigurowane do korzystania z protokołu DHCP w przypadku powrotu po awarii.  
Wiele kart sieciowych sieci z serwerem gościa | Yes


## <a name="azure-vm-network-after-failover"></a>Sieć maszyny Wirtualnej platformy Azure (po trybu failover)

**Składnik** | **Obsługiwane**
--- | ---
Usługa ExpressRoute systemu Azure | Yes
Wewnętrzny moduł równoważenia obciążenia | Yes
ELB | Yes
Azure Traffic Manager | Yes
Multi-NIC | Yes
Zastrzeżony adres IP | Yes
Protokół IPv4 | Yes
Zachowaj źródłowy adres IP | Yes
Punkty końcowe usługi sieci wirtualnej platformy Azure<br/><br/> (Usługa azure Storage zapory i sieci wirtualne) | Nie

## <a name="storage"></a>Magazyn
**Składnik** | **Obsługiwane**
--- | ---
Host systemu plików NFS | Tak, aby VMware<br/><br/> Nie dla serwerów fizycznych.
Sieć SAN (ISCSI) hosta | Yes
Wiele ścieżek hosta (MPIO) | Tak, poddane DSM firmy Microsoft, EMC PowerPath 5.7 z dodatkiem SP4 EMC PowerPath DSM dla CLARiiON
Gość i serwerem VMDK | Yes
Gość i serwerem interfejsem EFI/UEFI| Partial (migracja do platformy Azure dla systemu Windows Server 2012 i nowszych maszyn wirtualnych VMware tylko) </br></br> Patrz Uwaga na koniec tabeli.
Dysk udostępniony klaster gościa serwera | Nie
Gość i serwerem zaszyfrowanego dysku | Nie
Gość serwera systemu plików NFS | Nie
Gość serwera SMB 3.0 | Nie
Gość i serwerem RDM | Yes<br/><br/> Brak serwerów fizycznych
Gość i serwerem dysku > 1 TB | Yes<br/><br/>Do 4,095 GB
Gość i serwerem dysku o rozmiarze sektora fizycznego logicznych i 4 k 4K | Yes
Dysk serwera gościa z logicznych 4K i rozmiar sektora fizycznego 512 bajtów | Yes
Wolumin serwera gościa z dysku rozłożone > 4 TB <br><br/>Zarządzanie woluminami LVM logiczne | Yes
Gość/server - miejsca do magazynowania | Nie
Gość i serwerem dodawania i usuwania gorących dysku | Nie
Gość/server — Wyklucz dysku | Yes
Gość i serwerem wiele ścieżek (MPIO) | ND

> [!NOTE]
> UEFI rozruchu maszyn wirtualnych VMware z systemem Windows Server 2012 lub nowszym można poddać migracji do usługi Azure. Następujące ograniczenia:

> - Tylko migracja do systemu Azure jest obsługiwana. Powrót po awarii do lokacji lokalnej VMware nie jest obsługiwana.
> - Serwer nie powinien zawierać więcej niż czterech partycji na dysku systemu operacyjnego.
> - Wymagana wersja usługi mobilności 9.13 lub nowszej.
> - Nie jest obsługiwana na serwerach fizycznych.

## <a name="azure-storage"></a>Azure Storage

**Składnik** | **Obsługiwane**
--- | ---
LRS | Yes
GRS | Yes
RA-GRS | Yes
Magazynu chłodnego | Nie
Magazynu gorącego| Nie
Obiekty BLOB typu Block | Nie
Szyfrowanie magazynowanych (SSE)| Yes
Premium Storage | Yes
Import/Eksport usługi | Nie
Punkty końcowe usługi sieci wirtualnej<br/><br/> Zapory magazynu i sieci wirtualne skonfigurowane na docelowe konto magazynu pamięci podręcznej/magazynów (używane do przechowywania danych replikacji) | Nie
Konta magazynu ogólnego przeznaczenia v2 (zarówno gorącego i chłodnego warstw) | Nie

## <a name="azure-compute"></a>Obliczeń platformy Azure

**Funkcja** | **Obsługiwane**
--- | ---
Zestawy dostępności | Yes
HUB | Yes   
Dyski zarządzane | Yes

## <a name="azure-vm-requirements"></a>Wymagania dotyczące maszyny Wirtualnej platformy Azure

Lokalnych maszyn wirtualnych, które są replikowane do platformy Azure musi spełniać wymagania maszyny Wirtualnej Azure podsumowaniem w poniższej tabeli.

**Składnik** | **Wymagania** | **Szczegóły**
--- | --- | ---
**System operacyjny gościa** | Sprawdź [obsługiwanych systemów operacyjnych](#replicated machines). | Sprawdzanie wymagań wstępnych kończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Architektura systemu operacyjnego gościa** | 64-bitowa | Sprawdzanie wymagań wstępnych kończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Rozmiar dysku systemu operacyjnego** | Do 2048 GB | Sprawdzanie wymagań wstępnych kończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Liczba dysków systemu operacyjnego** | 1 | Sprawdzanie wymagań wstępnych kończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Liczba dysków danych** | Liczba to 64 lub mniej Jeśli replikujesz *maszyn wirtualnych VMware do platformy Azure*. Liczba jest 16 lub mniej Jeśli replikujesz *maszyn wirtualnych funkcji Hyper-V w systemie Azure*. | Sprawdzanie wymagań wstępnych kończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Rozmiar wirtualnego dysku twardego dysku danych** | Do 4,095 GB | Sprawdzanie wymagań wstępnych kończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Karty sieciowe** | Wiele kart sieciowych są obsługiwane. |
**Udostępniony wirtualny dysk twardy** | Nieobsługiwane. | Sprawdzanie wymagań wstępnych kończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Dysk FC** | Nieobsługiwane. | Sprawdzanie wymagań wstępnych kończy się niepowodzeniem, jeśli jest nieobsługiwany.
**Format dysku twardego** | WIRTUALNEGO DYSKU TWARDEGO <br/><br/> VHDX | Chociaż pliki VHDX nie są obecnie obsługiwane na platformie Azure, usługa Site Recovery automatycznie konwertuje pliki VHDX na wirtualne dyski twarde w przypadku trybu failover na platformie Azure. Przechodzenia wstecz do lokalnych maszyn wirtualnych w dalszym ciągu korzystać z formatu VHDX.
**BitLocker** | Nieobsługiwane. | Przed włączeniem replikacji dla maszyny, należy wyłączyć funkcję BitLocker.
**Nazwa maszyny wirtualnej** | Od 1 do 63 znaków.<br/><br/> Ograniczone do liter, cyfr i łączników.<br/><br/> Nazwa maszyny musi zaczynać i kończyć literą lub cyfrą. | Zaktualizuj wartość we właściwościach maszyny w usłudze Site Recovery.
**Typ maszyny wirtualnej** | Generacja 1.<br/><br/> Generacja 2 — systemu Windows. | Maszyny wirtualne generacji 2 z typem dysku systemu operacyjnego, Basic (zawierającej co najmniej dwa woluminy danych w formacie VHDX) i mniejsza niż 300 GB miejsca na dysku są obsługiwane.<br></br>Maszyn wirtualnych systemu Linux generacji 2 nie są obsługiwane. Aby dowiedzieć się więcej, zobacz [odzyskiwania po awarii na platformie Azure rozszerzone](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).|

## <a name="vault-tasks"></a>Magazyn zadań

**Akcja** | **Obsługiwane**
--- | ---
Przenieś magazyn między grupami zasobów<br/><br/> W obrębie subskrypcji oraz | Nie
Przenieść magazyn, sieć, maszyn wirtualnych platformy Azure w grupach zasobów<br/><br/> W obrębie subskrypcji oraz | Nie


## <a name="mobility-service"></a>Usługa mobilności

**Nazwa** | **Opis** | **Najnowsza wersja** | **Szczegóły**
--- | --- | --- | --- | ---
**Instalator Unified usługi Azure Site Recovery** | Współrzędne komunikacji między serwerami lokalnymi VMware i Azure <br/><br/> Zainstalowana na lokalnych serwerach VMware | 9.12.4653.1 (dostępne w portalu) | [Najnowsze funkcje i poprawki](https://aka.ms/latest_asr_updates)
**Usługa mobilności** | Koordynuje replikację między lokalnymi VMware serwerów/serwery fizyczne i Azure/dodatkowej lokacji<br/><br/> Zainstalowana na maszynie Wirtualnej VMware lub serwerów fizycznych, które chcesz replikować | 9.12.4653.1 (dostępne w portalu) | [Najnowsze funkcje i poprawki](https://aka.ms/latest_asr_updates)


## <a name="next-steps"></a>Kolejne kroki
[Dowiedz się, jak](tutorial-prepare-azure.md) do przygotowania Azure odzyskiwania po awarii maszyn wirtualnych VMware.
