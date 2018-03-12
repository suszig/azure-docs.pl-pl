---
title: "Tabela wsparcia dla replikacji maszyn wirtualnych VMware lub serwerów fizycznych do lokacji dodatkowej VMware z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Podsumowanie obsługę replikacji serwera VMware/fizycznych do lokacji dodatkowej z usługą Azure Site Recovery"
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: raynew
ms.openlocfilehash: b51a4573ad7a8461b7261f08d94639d2030492d9
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="support-matrix-for-replication-of-vmware-vms-and-physical-servers-to-a-secondary-site"></a>Tabela wsparcia dla replikacji maszyn wirtualnych VMware i serwerów fizycznych do lokacji dodatkowej

Ten artykuł zawiera podsumowanie, co jest obsługiwana w przypadku użycia [usługi Azure Site Recovery](site-recovery-overview.md) usługi do replikowania maszyn wirtualnych VMware lub systemem Windows lub Linux serwerów fizycznych do lokacji dodatkowej VMware.

- Jeśli chcesz replikować maszyny wirtualne VMware lub serwerów fizycznych do platformy Azure, przejrzyj [tej macierzy obsługi](vmware-physical-azure-support-matrix.md).
- Jeśli chcesz replikować maszyny wirtualne funkcji Hyper-V do lokacji dodatkowej, przejrzyj [tej macierzy obsługi](hyper-v-azure-support-matrix.md).

> [!NOTE]
> Replikacja maszyn wirtualnych VMware lokalnymi i serwerów fizycznych są udostępniane przez InMage Scout. InMage Scout znajduje się w subskrypcji usługi Azure Site Recovery.


## <a name="host-servers"></a>Serwery hosta

**System operacyjny** | **Szczegóły**
--- | ---
Serwer vCenter | vCenter 5.5, 6.0 i 6.5<br/><br/> Po uruchomieniu 6.0 lub 6.5, należy pamiętać, że tylko 5.5 funkcje są obsługiwane.


## <a name="replicated-vm-support"></a>Replikacja maszyny Wirtualnej pomocy technicznej

W poniższej tabeli przedstawiono obsługę systemu operacyjnego maszyny replikowane z usługą Site Recovery. Każde obciążenie może być uruchomiony na obsługiwany system operacyjny.

**System operacyjny** | **Szczegóły**
--- | ---
Windows Server | 64-bitowego systemu Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 z na co najmniej z dodatkiem SP1.
Linux | Red Hat Enterprise Linux 6.7, 6.8, 6.9, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6, 6.7, 6.8, 6.9, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, 6.8 systemem Red Hat jądra zgodny lub podzielenie Enterprise jądra wersji 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3, 11 SP4 


## <a name="linux-machine-storage"></a>Magazyn maszyny systemu Linux

Mogą być replikowane tylko maszyn z systemem Linux z magazynem następujące:

- Plik systemu (EXT3, ETX4, ReiserFS, XFS).
- Mapowanie urządzeń wielościeżkowych oprogramowania.
- Menedżer woluminów (LVM2).
- Serwery fizyczne z HP CCISS kontrolera magazynu nie są obsługiwane.
- System plików ReiserFS jest obsługiwany tylko w systemie SUSE Linux Enterprise Server 11 z dodatkiem SP3.

## <a name="network-configuration---hostguest-vm"></a>Konfiguracja sieci - Host/gościa maszyny Wirtualnej

**Konfiguracja** | **Obsługiwane**  
--- | --- 
Host - zespołu kart interfejsu sieciowego | Yes 
Host - sieci VLAN | Yes 
Host - IPv4 | Yes 
Host - IPv6 | Nie 
Maszyna wirtualna - gościa zespołu kart interfejsu sieciowego | Nie
Gość maszyny Wirtualnej — IPv4 | Yes
Gość maszyny Wirtualnej — protokół IPv6 | Nie
Maszyna wirtualna Gues — Windows/Linux — statyczny adres IP | Yes
Gość maszyny Wirtualnej — wieloma kartami Sieciowymi | Yes


## <a name="storage"></a>Magazyn

### <a name="host-storage"></a>Magazyn hosta

**Magazyn (hosta)** | **Obsługiwane** 
--- | --- 
NFS | Yes 
SMB 3.0 | ND 
SAN (ISCSI) | Yes 
Wiele ścieżek (MPIO) | Yes 

### <a name="guest-or-physical-server-storage"></a>Gość lub magazynu serwera fizycznego

**Konfiguracja** | **Obsługiwane** 
--- | --- 
VMDK | Yes 
VHD/VHDX | ND 
Gł 2 maszyny Wirtualnej | ND 
Udostępniony dysk klastra | Yes 
Zaszyfrowanego dysku | Nie 
UEFI| Yes 
NFS | Nie 
SMB 3.0 | Nie 
RDM | Yes 
Na dysku > 1 TB | Yes 
Wolumin dysku rozłożone > 1 TB<br/><br/> LVM | Yes 
Funkcja miejsca do magazynowania | Nie 
Dodaj lub usuń gorących dysku | Yes 
Wykluczanie dysku | Yes 
Wiele ścieżek (MPIO) | ND 

## <a name="vaults"></a>magazynów

**Akcja** | **Obsługiwane** 
--- | --- 
Przenieś magazynów między grupami zasobów (lub wielu subskrypcji) | Nie 
Przenieść magazyn, sieć, maszyn wirtualnych platformy Azure w grupach zasobów (lub wielu subskrypcji) | Nie 

## <a name="mobility-service-and-updates"></a>Usługa mobilności i aktualizacji

Usługa mobilności koordynuje replikację między serwerami lokalnymi VMware lub serwerów fizycznych i lokacji dodatkowej. Po skonfigurowaniu replikacji, upewnij się, że masz najnowszą wersję usługi mobilności i innych składników.

**Aktualizacja** | **Szczegóły** 
--- | --- 
Scout aktualizacji | [Dowiedz się więcej o i Pobierz](/vmware-physical-secondary-disaster-recovery.md#updates) najnowsze aktualizacje Scout | Scout aktualizacje są aktualizacjami zbiorczymi.
Aktualizacje składników | Aktualizacje Scout obejmują aktualizacje dla wszystkich składników, w tym RX serwera, serwer konfiguracji, proces i głównych serwerów docelowych, vContinuum serwerów i serwerów źródłowych, które chcesz chronić.<br/><br/> [Dowiedz się więcej](vmware-physical-secondary-disaster-recovery.md#download-and-install-component-updates).


## <a name="next-steps"></a>Kolejne kroki

Pobierz [InMage Scout Podręcznik użytkownika](https://aka.ms/asr-scout-user-guide)

- [Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach VMM do lokacji dodatkowej](tutorial-vmm-to-vmm.md)
- [Replikacja maszyn wirtualnych VMware i serwerów fizycznych do lokacji dodatkowej](tutorial-vmware-to-vmware.md)
