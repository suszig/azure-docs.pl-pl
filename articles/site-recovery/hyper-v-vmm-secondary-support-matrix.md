---
title: "Macierz obsługi replikacji maszyn wirtualnych funkcji Hyper-V w programie VMM chmury do lokacji dodatkowej z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Zawiera podsumowanie obsługę replikacji maszyny Wirtualnej funkcji Hyper-V w chmurach VMM do lokacji dodatkowej z usługą Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: raynew
ms.openlocfilehash: 767b0e76b73c082ddb75374f51700b85272f713e
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="support-matrix-for-replication-of-hyper-v-vms-to-a-secondary-site"></a>Macierz obsługi replikacji maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej

Ten artykuł zawiera podsumowanie, co jest obsługiwana w przypadku użycia [usługi Azure Site Recovery](site-recovery-overview.md) usługi do replikowania maszyn wirtualnych funkcji Hyper-V zarządzane w chmurach programu System Center Virtual Machine Manager (VMM) do lokacji dodatkowej. Jeśli chcesz replikować maszyny wirtualne funkcji Hyper-V do platformy Azure, przejrzyj [tej macierzy obsługi](hyper-v-azure-support-matrix.md).

> [!NOTE]
> Tylko można replikować do lokacji dodatkowej, gdy hosty funkcji Hyper-V są zarządzane w chmurach programu VMM.

  

## <a name="host-servers"></a>Serwery hosta

**System operacyjny** | **Szczegóły**
--- | ---
Windows Server 2012 R2 | Serwery muszą mieć uruchomioną najnowsze aktualizacje.
Windows Server 2016 |  Chmury VMM 2016 z systemu Windows Server 2016 i 2012 R2 hosty nie są obecnie obsługiwane.<br/><br/> Wdrożenia, które uaktualnione z programu System Center 2012 R2 VMM 2012 R2 do programu System Center 2016 nie są obecnie obsługiwane.


## <a name="replicated-vm-support"></a>Replikacja maszyny Wirtualnej pomocy technicznej

W poniższej tabeli przedstawiono obsługę systemu operacyjnego maszyny replikowane z usługą Site Recovery. Każde obciążenie może być uruchomiony na obsługiwany system operacyjny.

**Wersja systemu Windows** | **Funkcja Hyper-V (w programie VMM)**
--- | ---
Windows Server 2016 | Gość żadnego systemu operacyjnego [obsługiwane przez funkcję Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows) w systemie Windows Server 2016 
Windows Server 2012 R2 | Gość żadnego systemu operacyjnego [obsługiwane przez funkcję Hyper-V](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027%28v%3dws.11%29) w systemie Windows Server 2012 R2

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
NFS | ND
SMB 3.0 |  Yes
SAN (ISCSI) | Yes
Wiele ścieżek (MPIO) | Yes

### <a name="guest-or-physical-server-storage"></a>Gość lub magazynu serwera fizycznego

**Konfiguracja** | **Obsługiwane**
--- | --- | 
VMDK |  ND
VHD/VHDX | Tak (maksymalnie 16 dysków)
Gł 2 maszyny Wirtualnej | Yes
Udostępniony dysk klastra | Nie
Zaszyfrowanego dysku | Nie
UEFI| ND
NFS | Nie
SMB 3.0 | Nie
RDM | ND
Na dysku > 1 TB | Yes
Wolumin dysku rozłożone > 1 TB<br/><br/> LVM | Yes
Funkcja miejsca do magazynowania | Yes
Dodaj lub usuń gorących dysku | Nie
Wykluczanie dysku | Yes
Wiele ścieżek (MPIO) | Yes

## <a name="vaults"></a>magazynów

**Akcja** | **Obsługiwane**
--- | --- 
Przenieś magazynów między grupami zasobów (lub wielu subskrypcji) |  Nie
Przenieść magazyn, sieć, maszyn wirtualnych platformy Azure w grupach zasobów (lub wielu subskrypcji) | Nie

## <a name="azure-site-recovery-provider"></a>Azure Site Recovery Provider

Dostawca koordynuje komunikacji między serwerami VMM. 

**Latest** | **Aktualizacje**
--- | --- | --- | --- | ---
5.1.19 ([dostępna z portalu](http://aka.ms/downloaddra) | [Najnowsze funkcje i poprawki](https://support.microsoft.com/kb/3155002)



## <a name="next-steps"></a>Kolejne kroki

[Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach VMM do lokacji dodatkowej](tutorial-vmm-to-vmm.md)

