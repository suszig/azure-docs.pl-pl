---
title: "Przegląd architektury replikacji maszyn wirtualnych Azure między regiony platformy Azure | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera omówienie składników i architektury używane podczas replikowania maszyn wirtualnych platformy Azure między regiony platformy Azure przy użyciu usługi Azure Site Recovery."
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
ms.date: 09/10/2017
ms.author: raynew
ms.openlocfilehash: a449969e627c67660973ccf17123958678dd8844
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2017
---
# <a name="azure-to-azure-replication-architecture"></a>Architektura replikacji Azure do platformy Azure


W tym artykule opisano architektura i procesy stosowane podczas replikacji, pracy awaryjnej i odzyskiwania maszyn wirtualnych platformy Azure (maszyny wirtualne) między regiony platformy Azure przy użyciu [usługi Azure Site Recovery](../site-recovery-overview.md) usługi.

>[!NOTE]
>Azure replikacji maszyny Wirtualnej za pomocą usługi Site Recovery jest obecnie w przeglądzie.



## <a name="architectural-components"></a>Składniki architektury

Poniższa ilustracja przedstawia ogólny widok środowiska maszyny Wirtualnej platformy Azure w określonym regionie (w tym przykładzie lokalizacji wschodnie stany USA). W środowisku maszyny Wirtualnej platformy Azure:
- Aplikacje mogą działać na maszynach wirtualnych z dyskami rozmieszczenie do kont magazynu.
- Maszyny wirtualne mogą zostać włączone w co najmniej jednej podsieci sieci wirtualnej.


**Replikacja usługi Azure do platformy Azure**

![środowisko klienta](./media/concepts-azure-to-azure-architecture/source-environment.png)

## <a name="replication-process"></a>Proces replikacji

### <a name="step-1"></a>Krok 1

Po włączeniu replikacji maszyny Wirtualnej platformy Azure, zasobów, pokazano poniżej są tworzone automatycznie w obszarze docelowym na podstawie ustawień regionu źródła. Można dostosować ustawienia zasobów obiektu docelowego zgodnie z potrzebami. 

![Włącz proces replikacji, krok 1](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

**Zasób** | **Szczegóły**
--- | ---
**Docelowa grupa zasobów** | Grupa zasobów replikowane maszyny wirtualne należą po pracy awaryjnej.
**Docelowy sieci wirtualnej** | Sieć wirtualna, w którym replikowanych maszyn wirtualnych znajdują się po pracy awaryjnej. Mapowanie sieci jest tworzony między sieciami wirtualnymi źródłowym i docelowym i na odwrót.
**Konta magazynu pamięci podręcznej** | Replikacja zmiany maszyn wirtualnych źródłowego na docelowe konto magazynu, są śledzone i wysyłane do konta magazynu pamięci podręcznej w lokalizacji docelowej. Dzięki temu minimalny wpływ na aplikacje produkcji działające na maszynie Wirtualnej.
**Konta magazynu docelowego**  | Konta magazynu w lokalizacji docelowej, do którego dane są replikowane.
**Docelowy zestawów dostępności**  | Zestawy dostępności, której replikowanych maszyn wirtualnych znajdują się po pracy awaryjnej.

### <a name="step-2"></a>Krok 2

Ponieważ replikacja jest włączona, rozszerzenia usługi Site Recovery usługi mobilności jest automatycznie instalowany na Maszynie wirtualnej:

1. Maszyna wirtualna jest zarejestrowany z usługą Site Recovery.

2. Ciągła replikacja jest skonfigurowana dla maszyny Wirtualnej. Zapisuje dane na dyskach maszyny Wirtualnej stale są przenoszone do konta magazynu pamięci podręcznej, w lokalizacji źródłowej.

   ![Włącz proces replikacji, krok 2](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)

  
 Usługi Site Recovery, nigdy nie musi mieć łączność przychodzący do maszyny Wirtualnej. Tylko łączność wychodząca jest potrzebny, adresy IP/adresy URL usługi Site Recovery, adresy IP/adresy URL uwierzytelniania usługi Office 365 i adresy IP konta magazynu pamięci podręcznej.

### <a name="step-3"></a>Krok 3

Po ciągłej replikacji jest w toku, dysku operacje zapisu są natychmiast przenoszone do konta magazynu pamięci podręcznej. Usługa Site Recovery przetwarza dane i wysyła go do docelowe konto magazynu. Po przetworzeniu danych punkty odzyskiwania są generowane w docelowe konto magazynu, co kilka minut.

## <a name="failover-process"></a>Proces trybu failover

Po zainicjowaniu tryb failover maszyn wirtualnych są tworzone w docelowej sieci wirtualnej, docelowa grupa zasobów, docelowej podsieci i w zestawie dostępności docelowego. Podczas pracy awaryjnej można użyć dowolnego punktu odzyskiwania.

![Proces trybu failover](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Następne kroki

Postępuj zgodnie z [szybkiego startu](azure-to-azure-quickstart.md) Aby włączyć replikację maszyny Wirtualnej platformy Azure w regionie pomocniczym.

