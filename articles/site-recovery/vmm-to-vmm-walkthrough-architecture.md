---
title: "Przegląd architektury replikacji funkcji Hyper-V do lokacji dodatkowej za pomocą usługi Azure Site Recovery | Microsoft Docs"
description: "Ten artykuł zawiera omówienie architektury używanej podczas replikowania lokalnych maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej z programem System Center VMM za pomocą usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 07099161-4cc7-4f32-8eb9-2a71bbf0750b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: b78cd0d5a5395873afaddc8856004775f447e8ea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="step-1-review-the-architecture-for-hyper-v-replication-to-a-secondary-site"></a>Krok 1. Przegląd architektury replikacji funkcji Hyper-V do lokacji dodatkowej

Ten artykuł zawiera opis składników i procesów związanych z replikacją lokalnych maszyn wirtualnych funkcji Hyper-V w chmurach programu System Center Virtual Machine Manager (VMM) do lokacji dodatkowej z programem VMM za pomocą usługi [Azure Site Recovery](site-recovery-overview.md) w witrynie Azure Portal.

Zamieść wszelkie komentarze pod tym artykułem lub na [forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="architectural-components"></a>Składniki architektury

Poniżej przedstawiono wymagania dotyczące replikowania maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej z programem VMM.

**Składnik** | **Lokalizacja** | **Szczegóły**
--- | --- | ---
**Azure** | Subskrypcja na platformie Azure. | Magazyn usługi Recovery Services tworzy się w subskrypcji platformy Azure, aby organizować replikację między lokalizacjami z programem VMM i nią zarządzać.
**Serwer VMM** | Potrzebujesz podstawowej i dodatkowej lokalizacji z programem VMM. | Zalecamy, aby zarówno w lokacji głównej, jak i dodatkowej znajdował się jeden serwer VMM 
**Serwer funkcji Hyper-V** |  Co najmniej jeden serwer hosta funkcji Hyper-V w głównych i dodatkowych chmurach programu VMM. | Dane są replikowane między głównymi i dodatkowymi serwerami hosta funkcji Hyper-V za pośrednictwem sieci LAN albo sieci VPN korzystającej z protokołu Kerberos lub uwierzytelniania certyfikatu.  
**Maszyny wirtualne funkcji Hyper-V** | Na serwerze hosta funkcji Hyper-V. | Źródłowy serwer hosta powinien mieć co najmniej jedną maszynę wirtualną, która ma być replikowana.

## <a name="replication-process"></a>Proces replikacji

1. Należy skonfigurować konto platformy Azure, utworzyć magazyn usługi Recovery Services i określić elementy do replikacji.
2. Możesz skonfigurować źródłowe i docelowe ustawienia replikacji, co obejmuje instalowanie dostawcy usługi Azure Service Recovery na serwerach VMM oraz agenta usługi Microsoft Azure Recovery Services na każdym hoście funkcji Hyper-V.
3. Należy utworzyć zasady replikacji dla źródłowej chmury programu VMM. Te zasady są stosowane do wszystkich maszyn wirtualnych znajdujących się na hostach w chmurze.
4. Należy włączyć replikację dla każdego programu VMM, po czym nastąpi replikacja początkowa maszyny wirtualnej zgodnie z wybranymi ustawieniami.
5. Po replikacji początkowej rozpocznie się replikacja zmian różnicowych. Śledzone zmiany elementu są przechowywane w pliku hrl.


![Ze środowiska lokalnego do środowiska lokalnego](./media/vmm-to-vmm-walkthrough-architecture/arch-onprem-onprem.png)

## <a name="failover-and-failback-process"></a>Proces pracy w trybie failover i podczas powrotu po awarii

1. Planowane lub nieplanowane przejście w tryb [failover](site-recovery-failover.md) można uruchomić między lokacjami lokalnymi. Jeśli zostanie uruchomione planowane przejście w tryb failover, źródłowe maszyny wirtualne zostaną wyłączone w celu zapewnienia, że nie będzie miała miejsca utrata danych.
2. W tryb failover można przełączyć pojedynczą maszynę lub można utworzyć [plany odzyskiwania](site-recovery-create-recovery-plans.md), aby zarządzać trybem failover na wielu maszynach.
4. Jeśli wykonywane jest nieplanowane przejście w tryb failover do lokacji dodatkowej, po przejściu w tryb failover na maszynach znajdujących się w lokalizacji dodatkowej nie jest włączona ochrona ani replikacja. Jeśli dokonano planowanego przejścia w tryb failover, po przejściu w tryb failover maszyny znajdujące się w lokalizacji dodatkowej są chronione.
5. Następnie należy zatwierdzić tryb failover, aby można było rozpocząć uzyskiwanie dostępu do obciążenia z poziomu repliki maszyny wirtualnej.
6. Gdy lokacja główna będzie znowu dostępna, należy zainicjować replikację odwrotną w celu przeprowadzenia replikacji z lokacji dodatkowej do lokacji głównej. Replikacja odwrotna powoduje przełączenie maszyn wirtualnych do stanu chronionego, ale aktywną lokalizacją jest nadal dodatkowe centrum danych.
7. Aby lokacja główna stała się z powrotem lokalizacją aktywną, należy zainicjować planowane przejście w tryb failover z lokacji dodatkowej do głównej, a następnie przeprowadzić kolejną replikację odwrotną.



## <a name="next-steps"></a>Następne kroki

Przejdź do części [Krok 2. Przegląd wymagań wstępnych i ograniczeń](vmm-to-vmm-walkthrough-prerequisites.md).
