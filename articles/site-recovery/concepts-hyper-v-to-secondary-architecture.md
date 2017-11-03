---
title: "Przegląd architektury replikacji funkcji Hyper-V do lokacji dodatkowej za pomocą usługi Azure Site Recovery | Microsoft Docs"
description: "Ten artykuł zawiera omówienie architektury używanej podczas replikowania lokalnych maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej z programem System Center VMM za pomocą usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 26475782-a21a-408a-b089-35382d7e010e
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: raynew
ms.openlocfilehash: a7a493097a4eaacc2c8d8449906b4a57eb411827
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="hyper-v-replication-to-a-secondary-site"></a>Replikacji funkcji Hyper-V do lokacji dodatkowej

Ten artykuł zawiera opis składników i procesów związanych z replikacją lokalnych maszyn wirtualnych funkcji Hyper-V w chmurach programu System Center Virtual Machine Manager (VMM) do lokacji dodatkowej z programem VMM za pomocą usługi [Azure Site Recovery](site-recovery-overview.md) w witrynie Azure Portal.


## <a name="architectural-components"></a>Składniki architektury

Poniższej tabeli i grafiki zapewnia widok ogólny składników używanych w przypadku replikacji funkcji Hyper-V z lokacją dodatkową.

**Składnik** | **Wymaganie** | **Szczegóły**
--- | --- | ---
**Azure** | Subskrypcja platformy Azure | Magazyn usługi Recovery Services tworzy się w subskrypcji platformy Azure, aby organizować replikację między lokalizacjami z programem VMM i nią zarządzać.
**Serwer VMM** | Potrzebujesz podstawowej i dodatkowej lokalizacji z programem VMM. | Zalecamy, aby jeden serwer VMM znajdował się w lokacji głównej, a jeden w lokacji dodatkowej.
**Serwer funkcji Hyper-V** |  Co najmniej jeden serwer hosta funkcji Hyper-V w głównych i dodatkowych chmurach programu VMM. | Dane są replikowane między głównymi i dodatkowymi serwerami hosta funkcji Hyper-V za pośrednictwem sieci LAN albo sieci VPN korzystającej z protokołu Kerberos lub uwierzytelniania certyfikatu.  
**Maszyny wirtualne funkcji Hyper-V** | Na serwerze hosta funkcji Hyper-V. | Źródłowy serwer hosta powinien mieć co najmniej jedną maszynę wirtualną, która ma być replikowana.

**Dane lokalne na lokalnej architektury**

![Ze środowiska lokalnego do środowiska lokalnego](./media/concepts-hyper-v-to-secondary-architecture/arch-onprem-onprem.png)

## <a name="replication-process"></a>Proces replikacji

1. Po wyzwoleniu replikacji początkowej [migawki maszyny Wirtualnej funkcji Hyper-V](https://technet.microsoft.com/library/dd560637.aspx) migawki.
2. Wirtualne dyski twarde na maszynie Wirtualnej są replikowane pojedynczo, do lokalizacji dodatkowej.
3. Jeśli zmiany dysku są wykonywane, gdy Replikacja początkowa jest w toku. 
4. Po zakończeniu replikacji początkowej, rozpoczyna się replikacja różnicowa. Śledzenie replikacji repliki funkcji Hyper-V śledzi zmiany jako dzienniki replikacji funkcji Hyper-V (hrl). Te pliki dziennika znajdują się w tym samym folderze co dyski. Każdy dysk ma skojarzony plik hrl wysyłanym do lokalizacji dodatkowej. Pliki migawki i dziennika zużywają zasoby dysku w trakcie replikacji początkowej.
5. Różnicowe zmiany dysku w dzienniku są synchronizowane i scalane z dyskiem nadrzędnym.
6. 

## <a name="failover-and-failback-process"></a>Proces pracy w trybie failover i podczas powrotu po awarii

1. W tryb failover jednego komputera lub utworzyć plany odzyskiwania, aby organizować tryb failover wiele maszyn.
2. Można uruchomić planowanego lub nieplanowanego trybu failover między lokacjami lokalnymi. Jeśli zostanie uruchomione planowane przejście w tryb failover, źródłowe maszyny wirtualne zostaną wyłączone w celu zapewnienia, że nie będzie miała miejsca utrata danych.
    - W przypadku nieplanowanego trybu failover do lokacji dodatkowej po maszyny pracy awaryjnej w dodatkowej lokalizacji nie są chronione.
    - Jeśli dokonano planowanego przejścia w tryb failover, po przejściu w tryb failover maszyny znajdujące się w lokalizacji dodatkowej są chronione.
3. Po początkowej przejście w tryb failover działa, Zatwierdź go uruchomić, uzyskiwanie dostępu do obciążenia z repliki maszyny Wirtualnej.

W przypadku lokalizacji głównej jest ponownie dostępny, możesz się niepowodzeniem ponownie.

1. Inicjuje replikację odwrotną do rozpoczęcia replikacji z lokacji dodatkowej do podstawowych. Replikacja odwrotna powoduje przełączenie maszyn wirtualnych do stanu chronionego, ale aktywną lokalizacją jest nadal dodatkowe centrum danych.
2. Aby lokacja główna stała się z powrotem lokalizacją aktywną, należy zainicjować planowane przejście w tryb failover z lokacji dodatkowej do głównej, a następnie przeprowadzić kolejną replikację odwrotną.



## <a name="next-steps"></a>Następne kroki

Przejrzyj macierz obsługi czynności opisane w samouczku, aby włączyć replikację funkcji Hyper-V między chmurami VMM.
Uruchom tryb failover i powrotu po awarii.
