---
title: "Tabela wsparcia dla replikacji do lokacji dodatkowej z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Podsumowanie obsługiwanych systemów operacyjnych oraz składniki usługi Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/22/2018
ms.author: raynew
ms.openlocfilehash: 256bad0c3c06182b6be2b647ae27db90fe69724d
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/24/2018
---
# <a name="support-matrix-for-replication-to-a-secondary-site-with-azure-site-recovery"></a>Tabela wsparcia dla replikacji do lokacji dodatkowej z usługą Azure Site Recovery

Ten artykuł zawiera podsumowanie, co jest obsługiwana w przypadku użycia [usługi Azure Site Recovery](site-recovery-overview.md) usługę, aby replikować do lokacji dodatkowej lokalnymi.

## <a name="supported-scenarios"></a>Obsługiwane scenariusze

**Wdrożenie** | **Szczegóły** 
--- | ---
**VMware do programu VMware** | Odzyskiwania po awarii lokalnych maszyn wirtualnych VMware do lokacji dodatkowej VMware.<br/><br/> Pobierz [InMage Scout Podręcznik użytkownika](https://aka.ms/asr-scout-user-guide)
**Z funkcji Hyper-V do funkcji Hyper-V** | Odzyskiwanie maszyn wirtualnych funkcji Hyper-V lokalnymi w chmurach VMM do dodatkowej chmury VMM.<br></br> Nie jest obsługiwane bez programu VMM.



  

## <a name="host-servers"></a>Serwery hosta

**Wdrożenie** | **Pomoc techniczna**
--- | ---
**Maszyna wirtualna oprogramowania VMware/fizyczne serwera** | vCenter 5.5, 6.0 i 6.5 (Obsługa tylko funkcji 5.5)
**Funkcja Hyper-V w programie VMM** | Windows Server 2016 i Windows Server 2012 R2 z najnowszymi aktualizacjami.<br/><br/> Hosty z systemem Windows Server 2016 powinny być zarządzane przez program VMM 2016.<br/><br/> Chmury VMM 2016 z systemu Windows Server 2016 i 2012 R2 hosty nie są obecnie obsługiwane.<br/><br/> Wdrożenia, które obejmują uaktualnienie istniejącego VMM 2012 R2 do wersji System Center 2016 nie są obecnie obsługiwane.


## <a name="support-for-replicated-machine-os-versions"></a>Obsługa wersji systemu operacyjnego zreplikowanej maszyny

W poniższej tabeli przedstawiono obsługę systemu operacyjnego maszyny replikowane z usługą Site Recovery. Każde obciążenie może być uruchomiony na obsługiwany system operacyjny.

**Serwer VMware/fizyczne** | **Funkcja Hyper-V (w programie VMM)**
--- | ---
64-bitowego systemu Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 z na co najmniej z dodatkiem SP1<br/><br/> Red Hat Enterprise Linux 6.7, 6.8, 6.9, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6, 6.7, 6.8, 6.9, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, 6.8 systemem Red Hat jądra zgodny lub podzielenie Enterprise jądra wersji 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3, 11 SP4  | Gość żadnego systemu operacyjnego [obsługiwane przez funkcję Hyper-V](https://technet.microsoft.com/library/mt126277.aspx)

## <a name="linux-machine-storage"></a>Magazyn maszyny systemu Linux

Mogą być replikowane tylko maszyn z systemem Linux z magazynem następujące:

- Plik systemu (EXT3, ETX4, ReiserFS, XFS).
- Mapowanie urządzeń wielościeżkowych oprogramowania.
- Menedżer woluminów (LVM2).
- Serwery fizyczne z HP CCISS kontrolera magazynu nie są obsługiwane.
- System plików ReiserFS jest obsługiwany tylko w systemie SUSE Linux Enterprise Server 11 z dodatkiem SP3.

## <a name="network-configuration"></a>Konfiguracja sieci

### <a name="hosts"></a>Hosts

**Konfiguracja** | **Serwer VMware/fizyczne** | **Funkcja Hyper-V (w programie VMM)**
--- | --- | ---
Tworzenie zespołu kart sieciowych | Yes | Yes
VLAN | Yes | Yes
Protokół IPv4 | Yes | Yes
Protokół IPv6 | Nie | Nie

### <a name="guest-vms"></a>Maszyny wirtualne gości

**Konfiguracja** | **Serwer VMware/fizyczne** | **Funkcja Hyper-V (w programie VMM)**
--- | --- | ---
Tworzenie zespołu kart sieciowych | Nie | Nie
Protokół IPv4 | Yes | Yes
Protokół IPv6 | Nie | Nie
Statyczny adres IP (z systemem Windows) | Yes | Yes
Statyczny adres IP (Linux) | Yes | Yes
Multi-NIC | Yes | Yes


## <a name="storage"></a>Magazyn

### <a name="host-storage"></a>Magazyn hosta

**Magazyn (hosta)** | **Serwer VMware/fizyczne** | **Funkcja Hyper-V (w programie VMM)**
--- | --- | ---
NFS | Yes | ND
SMB 3.0 | ND | Yes
SAN (ISCSI) | Yes | Yes
Wiele ścieżek (MPIO) | Yes | Yes

### <a name="guest-or-physical-server-storage"></a>Gość lub magazynu serwera fizycznego

**Konfiguracja** | **Serwer VMware/fizyczne** | **Funkcja Hyper-V (w programie VMM)**
--- | --- | ---
VMDK | Yes | ND
VHD/VHDX | ND | Tak (maksymalnie 16 dysków)
Gł 2 maszyny Wirtualnej | ND | Yes
Udostępniony dysk klastra | Yes  | Nie
Zaszyfrowanego dysku | Nie | Nie
UEFI| Yes | ND
NFS | Nie | Nie
SMB 3.0 | Nie | Nie
RDM | Yes | ND
Na dysku > 1 TB | Yes | Yes
Wolumin dysku rozłożone > 1 TB<br/><br/> LVM | Yes | Yes
Funkcja miejsca do magazynowania | Nie | Yes
Dodaj lub usuń gorących dysku | Yes | Nie
Wykluczanie dysku | Yes | Yes
Wiele ścieżek (MPIO) | ND | Yes

## <a name="vaults"></a>magazynów

**Akcja** | **Serwer VMware/fizyczne** | **Funkcja Hyper-V (w programie VMM)**
--- | --- | ---
Przenieś magazynów między grupami zasobów (lub wielu subskrypcji) | Nie | Nie
Przenieść magazyn, sieć, maszyn wirtualnych platformy Azure w grupach zasobów (lub wielu subskrypcji) | Nie | Nie

## <a name="provider-and-agent"></a>Dostawca i agent

**Nazwa** | **Opis** | **Najnowsza wersja** | **Szczegóły**
--- | --- | --- | --- | ---
**Dostawca usługi Azure Site Recovery** | Współrzędne komunikacji między serwerami lokalnymi i Azure <br/><br/> Zainstalowana na lokalnych serwerach VMM lub na serwerach funkcji Hyper-V, jeśli serwer VMM nie jest | 5.1.19 ([dostępna z portalu](http://aka.ms/downloaddra)) | [Najnowsze funkcje i poprawki](https://support.microsoft.com/kb/3155002)
**Usługa mobilności** | Koordynuje replikację między serwerami lokalnymi VMware lub serwerów fizycznych i lokacji dodatkowej<br/><br/> Zainstalowana na maszynie Wirtualnej VMware lub serwerów fizycznych, które chcesz replikować  | N/d (dostępne w portalu) | ND


## <a name="next-steps"></a>Kolejne kroki

- [Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach VMM do lokacji dodatkowej](tutorial-vmm-to-vmm.md)
- [Replikacja maszyn wirtualnych VMware i serwerów fizycznych do lokacji dodatkowej](tutorial-vmware-to-vmware.md)
