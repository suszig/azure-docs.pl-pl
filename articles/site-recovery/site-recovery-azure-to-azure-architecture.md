---
title: "Jak działa replikacja maszyny wirtualnej platformy Azure między regiony platformy Azure w usłudze Azure Site Recovery?  | Microsoft Docs"
description: "Ten artykuł zawiera omówienie składników i architektury używane podczas replikowania maszyn wirtualnych platformy Azure między regiony platformy Azure przy użyciu usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/31/2017
ms.author: sujayt
ms.openlocfilehash: 7a3e1eb129db530295cbf28bbce1dbe48575d50d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-does-azure-vm-replication-work-in-site-recovery"></a>Jak działa replikacja maszyny Wirtualnej platformy Azure w usłudze Site Recovery?


W tym artykule opisano składniki i procesy zaangażowane w replikacji i odzyskiwania maszyn wirtualnych platformy Azure (maszyn wirtualnych) z jednego regionu do drugiego za pomocą [usługi Azure Site Recovery](site-recovery-overview.md) usługi.

>[!NOTE]
>Azure replikacji maszyny Wirtualnej za pomocą usługi Site Recovery jest obecnie w przeglądzie.

Wszelkie komentarze umieszczaj pod tym artykułem lub zadawaj pytania na [Forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architectural-components"></a>Składniki architektury

Poniższy diagram przedstawia ogólny widok środowiska maszyny Wirtualnej platformy Azure w określonym regionie (w tym przykładzie lokalizacji wschodnie stany USA). W środowisku maszyny Wirtualnej platformy Azure:
- Aplikacje mogą działać na maszynach wirtualnych z dyskami rozmieszczenie do kont magazynu.
- Maszyny wirtualne mogą zostać włączone w co najmniej jednej podsieci sieci wirtualnej.

![środowisko klienta](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Dowiedz się więcej o wymaganiach wstępnych dotyczących wdrożenia i wymagań w [macierz obsługi](site-recovery-support-matrix-azure-to-azure.md).

## <a name="replication-process"></a>Proces replikacji

### <a name="step-1"></a>Krok 1

Po włączeniu replikacji maszyny Wirtualnej platformy Azure w portalu Azure w region docelowy są automatycznie tworzone zasoby pokazano na poniższym diagramie i tabeli. Domyślnie zasoby są tworzone zgodnie z ustawieniami region źródła. Można dostosować ustawienia obiektu docelowego, zgodnie z wymaganiami. [Dowiedz się więcej](site-recovery-replicate-azure-to-azure.md).

![Włącz proces replikacji, krok 1](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-1.png)

**Zasób** | **Szczegóły**
--- | ---
**Docelowa grupa zasobów** | Grupa zasobów replikowane maszyny wirtualne należą po pracy awaryjnej.
**Docelowy sieci wirtualnej** | Sieć wirtualna, w którym replikowanych maszyn wirtualnych znajdują się po pracy awaryjnej. Mapowanie sieci jest tworzony między sieciami wirtualnymi źródłowym i docelowym i na odwrót.
**Konta magazynu pamięci podręcznej** | Przed Replikacja zmian na maszynach wirtualnych źródłowego na docelowe konto magazynu, są one śledzone i wysyłane do konta magazynu pamięci podręcznej w lokalizacji docelowej. Dzięki temu minimalny wpływ na aplikacje produkcji działające na maszynie Wirtualnej.
**Konta magazynu docelowego**  | Konta magazynu w lokalizacji docelowej, do którego dane są replikowane.
**Docelowy zestawów dostępności**  | Zestawy dostępności, której replikowanych maszyn wirtualnych znajdują się po pracy awaryjnej.

### <a name="step-2"></a>Krok 2

Ponieważ replikacja jest włączona, rozszerzenia usługi Site Recovery usługa mobilności jest automatycznie instalowany na maszynie Wirtualnej. Następujące operacje:

1. Maszyna wirtualna jest zarejestrowany z usługą Site Recovery.

2. Ciągła replikacja jest skonfigurowana dla maszyny Wirtualnej. Zapisuje dane na dyskach maszyny Wirtualnej stale są przenoszone do konta magazynu pamięci podręcznej w lokalizacji źródłowej.

   ![Włącz proces replikacji, krok 2](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-2.png)

   >[!IMPORTANT]
   > Usługi Site Recovery, nigdy nie musi mieć łączność przychodzący do maszyny Wirtualnej. Maszyna wirtualna musi mieć tylko ruchu wychodzącego łączność adresy IP/adresy URL usługi Site Recovery, adresy IP/adresy URL uwierzytelniania usługi Office 365 i adresów IP konta magazynu pamięci podręcznej. Aby uzyskać więcej informacji, zobacz [sieci wskazówki dotyczące replikowanie maszyn wirtualnych platformy Azure](site-recovery-azure-to-azure-networking-guidance.md) artykułu.

## <a name="continuous-replication-process"></a>Proces replikacji ciągłej

Po replikacji ciągłej działa, dysku operacje zapisu natychmiast są przenoszone do konta magazynu pamięci podręcznej. Usługa Site Recovery przetwarza dane i wysyła je do docelowe konto magazynu. Po przetworzeniu danych punkty odzyskiwania są generowane w docelowe konto magazynu, co kilka minut.

## <a name="failover-process"></a>Proces trybu failover

Po zainicjowaniu tryb failover maszyn wirtualnych są tworzone w docelowa grupa zasobów, wirtualne sieci docelowej, podsieci docelowej i zestawu dostępności docelowego. Podczas pracy awaryjnej można użyć dowolnego punktu odzyskiwania.

![Proces trybu failover](./media/site-recovery-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [sieci](site-recovery-azure-to-azure-networking-guidance.md) replikacji maszyny Wirtualnej platformy Azure.
- Wykonaj wskazówki do [replikowanie maszyn wirtualnych Azure.](site-recovery-azure-to-azure.md)
