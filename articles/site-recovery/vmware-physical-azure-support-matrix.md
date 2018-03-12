---
title: "Tabela wsparcia dla replikację maszyn wirtualnych VMware i serwerów fizycznych do platformy Azure z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Podsumowanie obsługiwanych systemów operacyjnych oraz składników do replikowania maszyn wirtualnych VMware i serwerów fizycznych do platformy Azure przy użyciu usługi Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/08/2018
ms.author: raynew
ms.openlocfilehash: 413234204175b9361cd2a837e0b318bf5220f58f
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="support-matrix-for-vmware-and-physical-server-replication-to-azure"></a>Tabela wsparcia dla VMware i replikacja serwerów fizycznych do platformy Azure

Ten artykuł zawiera podsumowanie obsługiwanych składników i ustawienia odzyskiwania po awarii maszyn wirtualnych VMware do platformy Azure przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md).

## <a name="supported-scenarios"></a>Obsługiwane scenariusze

**Scenariusz** | **Szczegóły**
--- | ---
**Maszyny wirtualne VMware** | Można wykonać odzyskiwania po awarii do platformy Azure dla maszyn wirtualnych VMware lokalnymi. Można wdrożyć ten scenariusz, w portalu Azure lub za pomocą programu PowerShell.
**Serwerów fizycznych** | Można wykonać odzyskiwania po awarii do platformy Azure dla lokalnych serwerów fizycznych systemu Windows i Linux. Można wdrożyć ten scenariusz, w portalu Azure.

## <a name="on-premises-virtualizationhost-servers"></a>Host wirtualizacji/serwery lokalne

**Serwer** | **Wymagania** | **Szczegóły**
--- | --- | ---
**VMware** | vCenter Server 6.5 w wersji 6.0 lub 5.5 lub vSphere 6.5, 6.0 lub 5.5 | Firma Microsoft zaleca użycie serwera vCenter.
**Serwerów fizycznych** | ND


## <a name="replicated-machines"></a>Replikowane maszyny

W poniższej tabeli przedstawiono obsługę replikacji maszyn wirtualnych VMware oraz serwerach fizycznych. Usługa Site Recovery obsługuje replikację dowolne obciążenia uruchomione na komputerze z obsługiwanym systemem operacyjnym.

**Składnik** | **Szczegóły**
--- | ---
Ustawienia komputera | Komputery, które są replikowane do platformy Azure musi spełniać [wymagania dotyczące usługi Azure](#failed-over-azure-vm-requirements).
System operacyjny Windows | 64-bitowego systemu Windows Server 2016 (instalacja Server Core, serwer z środowisko pulpitu), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 z na co najmniej z dodatkiem SP1. Windows 2016 Nano Server nie jest obsługiwana.
System operacyjny Linux | Red Hat Enterprise Linux: 5.2-5.11, 6.1-6.9, 7.0 do 7,4 <br/><br/>CentOS: 5.2-5.11, 6.1-6.9, 7.0 do 7,4 <br/><br/>Ubuntu 14.04 LTS serwera[ (obsługiwane wersje jądra)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Ubuntu 16.04 LTS server[ (obsługiwane wersje jądra)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Debian 7/Debian 8<br/><br/>Oracle Linux przedsiębiorstwa 6.4, 6.5 systemem Red Hat jądra zgodny lub podzielenie Enterprise jądra wersji 3 (UEK3) <br/><br/>SUSE Linux Enterprise Server 11 SP3, SUSE Linux Enterprise Server 11 SP4 <br/><br/>Uaktualnienie replikowanych maszyn z dodatkiem SP3 do SP4 nie jest obsługiwane. Aby przeprowadzić uaktualnienie, wyłącz replikacji i włącz ją ponownie po uaktualnieniu.

>[!NOTE]
>
> - Na dystrybucje systemu Linux obsługiwane są tylko standardowych jądra, które są częścią dystrybucji wersja pomocnicza wersji/aktualizacji.
>
> - Uaktualnianie chronionych maszyn między Linux główne wersje dystrybucji nie jest obsługiwane. Do uaktualnienia, wyłącz replikację, Uaktualnij system operacyjny, a następnie włącz ponownie replikacji.
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

## <a name="linux-file-systemsguest-storage"></a>Magazyn gościa/systemy plików systemu Linux

**Składnik** | **Obsługiwane**
--- | ---
systemy plików | ext3, ext4, ReiserFS (tylko w systemie Suse Linux Enterprise Server), XFS
Menedżer woluminów | LVM2
Oprogramowanie wielościeżkowego | Mapowanie urządzeń
Urządzenia magazynujące parawirtualnego systemu | Urządzenia eksportowane przez sterowniki parawirtualne nie są obsługiwane.
Blokuj wielu kolejki We/Wy urządzenia | Nieobsługiwane.
Serwery fizyczne z kontrolera magazynu HP CCISS | Nieobsługiwane.
Katalogi | Te katalogi (jeśli skonfigurowany jako osobne partycje /-systemów plików) musi składać się na tym samym dysku systemu operacyjnego na serwerze źródłowym: / (root), / Boot usr, /usr/local, /var, etc.</br></br> / Boot powinien znajdować się na partycji dysku i nie jest woluminem LVM<br/><br/>
Wymagania dotyczące wolnego miejsca| 2 GB na partycji/root <br/><br/> 250 MB na folder instalacji
XFSv5 | Funkcje XFSv5 na XFS systemów plików, takich jak metadanych sumy kontrolnej, są obsługiwane z mobilności wersji usługi 9.10 i jego nowszych wersjach. Użyj narzędzia xfs_info, aby sprawdzić superblock XFS dla partycji. Jeśli ftype jest ustawiona na 1, XFSv5 funkcje są w użyciu.



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
Gość i serwerem dysku o rozmiarze sektora fizycznego logicznych i 4 k 4K | Tak <
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

Lokalnych maszyn wirtualnych, które są replikowane do platformy Azure musi spełniać wymagania maszyny Wirtualnej Azure podsumowaniem w poniższej tabeli. Po uruchomieniu sprawdzania wymagań wstępnych usługi Site Recovery zakończy się niepowodzeniem, jeśli niektórych wymagań nie są spełnione.

**Składnik** | **Wymagania** | **Szczegóły**
--- | --- | ---
**System operacyjny gościa** | Sprawdź [obsługiwanych systemów operacyjnych](#replicated machines). | Sprawdzenie nie powiedzie się, jeśli jest nieobsługiwany. 
**Architektura systemu operacyjnego gościa** | 64-bitowa | Sprawdzenie nie powiedzie się, jeśli jest nieobsługiwany. 
**Rozmiar dysku systemu operacyjnego** | Do 2048 GB | Sprawdzenie nie powiedzie się, jeśli jest nieobsługiwany. 
**Liczba dysków systemu operacyjnego** | 1 | Sprawdzenie nie powiedzie się, jeśli jest nieobsługiwany.  
**Liczba dysków danych** | 64 lub mniej | Sprawdzenie nie powiedzie się, jeśli jest nieobsługiwany.  
**Rozmiar wirtualnego dysku twardego dysku danych** | Do 4,095 GB | Sprawdzenie nie powiedzie się, jeśli jest nieobsługiwany. 
**Karty sieciowe** | Wiele kart sieciowych są obsługiwane. | 
**Udostępniony wirtualny dysk twardy** | Nieobsługiwane. | Sprawdzenie nie powiedzie się, jeśli jest nieobsługiwany. 
**Dysk FC** | Nieobsługiwane. | Sprawdzenie nie powiedzie się, jeśli jest nieobsługiwany. 
**Format dysku twardego** | WIRTUALNEGO DYSKU TWARDEGO <br/><br/> VHDX | VHDX nie jest obecnie obsługiwany na platformie Azure, ale usługa Site Recovery automatycznie konwertuje VHDX do wirtualnego dysku twardego po pracy awaryjnej. Przechodzenia wstecz do lokalnych maszyn wirtualnych w dalszym ciągu korzystać z formatu VHDX.
**BitLocker** | Nieobsługiwane | Przed włączeniem replikacji dla maszyny, należy wyłączyć funkcję BitLocker. | 
**Nazwa maszyny wirtualnej** | Od 1 do 63 znaków<br/><br/> Ograniczone do liter, cyfr i łączników.<br/><br/> Nazwa maszyny musi zaczynać i kończyć literą lub cyfrą. |  Zaktualizuj wartość we właściwościach maszyny w usłudze Site Recovery.
**Typ maszyny wirtualnej** | Generacji 1 i generacji 2 (tylko system Windows) |  Maszyny wirtualne generacji 2 musi mieć podstawowy dysk systemu operacyjnego (w tym lub dwa woluminy danych w formacie VHDX), a mniejszy niż 300 GB miejsca na dysku 
Maszyn wirtualnych systemu Linux generacji 2 nie są obsługiwane. 

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
