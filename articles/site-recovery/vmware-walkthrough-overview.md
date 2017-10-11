---
title: "Replikowanie maszyn wirtualnych VMware do platformy Azure z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Zawiera omówienie kroków replikowania obciążeń uruchomionych na maszynach wirtualnych VMware do platformy Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: db6f5f95929503e82a529dba26b56af1edb0767f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-vmware-vms-to-azure-with-site-recovery"></a>Replikowanie maszyn wirtualnych VMware do platformy Azure z usługą Site Recovery

W tym artykule omówiono kroki wymagane do replikowania maszyn wirtualnych VMware lokalnej na platformie Azure, przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) w portalu Azure.


![Proces wdrażania](./media/vmware-walkthrough-overview/vmware-to-azure-process.png)

**Rysunek 1: Podsumowanie procesu wdrożenia**

## <a name="step-1-review-architecture-and-prerequisites"></a>Krok 1: Przegląd architektury i wymagania wstępne

Przed rozpoczęciem wdrażania zapoznaj się z architekturą scenariusza i upewnij się, że znasz wszystkie składniki potrzebne do wdrożenia

Przejdź do [krok 1: Przejrzyj architektury](vmware-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Krok 2: Sprawdź wymagania wstępne dotyczące

Upewnij się, że zostały spełnione wymagania wstępne w przypadku każdego składnika wdrażania:

- **Wymagania wstępne platformy Azure**: wymagane konto Microsoft Azure, sieci platformy Azure i kont magazynu.
- **Składniki usługi Site Recovery lokalnymi**: należy komputera z uruchomionym systemem lokalnymi składnikami usługi Site Recovery.
- **Lokalne wstępnych VMware**: należy ustawić kont, dzięki czemu usługa Site Recovery mogą uzyskiwać dostęp do serwerów VMware i maszyn wirtualnych.
- **Maszyny wirtualne replikowane**: chcesz replikować maszyny wirtualne muszą spełniać wymagania dotyczące usługi Azure i zainstalowano składnika usługi Mobility.

Przejdź do [krok 2: Przejrzyj wymagania wstępne i ograniczenia](vmware-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Krok 3: Planowanie pojemności

Jeśli pełne wdrożenie robimy należy dowiedzieć się, jakie potrzebne zasoby replikacji. Istnieje kilka narzędzi ułatwiających to zrobić. Przejdź do kroku 2. Jeśli podczas wykonywania zestawu szybkiego się do środowiska testowego można pominąć ten krok.

Przejdź do sekcji [Krok 3. Planowanie wydajności](vmware-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>Krok 4: Planowanie sieci

Należy wykonać niektóre sieci, planowanie upewnić się, czy maszynach wirtualnych platformy Azure są połączone z sieciami po pracy awaryjnej i które mają prawo IP adresy.

Przejdź do [krok 4: Planowanie sieci](vmware-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>Krok 5: Przygotowanie zasobów platformy Azure

Konfigurowanie sieci platformy Azure i magazynu przed jego uruchomieniem. Można to zrobić podczas wdrażania, ale zaleca się, że można to zrobić przed rozpoczęciem.

Przejdź do [krok 5: przygotowanie Azure](vmware-walkthrough-prepare-azure.md)


## <a name="step-6-prepare-vmware"></a>Krok 6: Przygotowanie VMware

Należy skonfigurować konta używających usługi Site Recovery do:

- Dostęp do serwera wirtualizacji VMware, aby automatycznie wykrywać maszyn wirtualnych.
- Dostęp do maszyn wirtualnych do zainstalowania usługi mobilności. Każda maszyna wirtualna, które chcesz replikować musi mieć agent usługi mobilności, zainstalowane, aby można było włączyć dla niego replikację.

Przejdź do [krok 6: przygotowanie VMware](vmware-walkthrough-prepare-vmware.md)

## <a name="step-7-set-up-a-vault"></a>Krok 7: Konfigurowanie magazynu

Należy skonfigurować magazyn usług odzyskiwania do organizowania replikacji. Po skonfigurowaniu magazynu można określić co chcesz replikować, a które chcesz replikować go do.

Przejdź do [kroku 7: Konfigurowanie magazynu](vmware-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>Krok 8: Konfigurowanie ustawień źródłowa i docelowa

Skonfiguruj źródłowy i docelowy, który jest używany do replikacji. Konfigurowanie ustawień źródła obejmuje uruchomione Unified Instalatora, aby zainstalować składniki usługi Site Recovery lokalnymi.

Przejdź do [krok 8: Konfigurowanie źródłowych i docelowych](vmware-walkthrough-source-target.md)

## <a name="step-9-set-up-a-replication-policy"></a>Krok 9: Konfigurowanie zasad replikacji

Aby określić ustawienia replikacji dla maszyn wirtualnych VMware w magazynie skonfigurować zasadę.

Przejdź do [krok 9: Konfigurowanie zasad replikacji](vmware-walkthrough-replication.md)

## <a name="step-10-install-the-mobility-service"></a>Krok 10: Zainstaluj usługę mobilności

Musi być zainstalowana usługa mobilności na każdej maszynie Wirtualnej, którą chcesz replikować. Istnieje kilka sposobów, aby skonfigurować usługę za pomocą wypychania lub ściągania instalacji.

Przejdź do [kroku 10: Zainstaluj usługę mobilności](vmware-walkthrough-install-mobility.md)

## <a name="step-11-enable-replication"></a>Krok 11: Włączanie replikacji

Po uruchomieniu usługi mobilności na maszynie Wirtualnej można włączyć dla niego replikację. Po włączeniu odbywa się Replikacja początkowa maszyny wirtualnej.

Przejdź do [krok 11: Włączanie replikacji](vmware-walkthrough-enable-replication.md)

## <a name="step-12-run-a-test-failover"></a>Krok 12: Uruchom test trybu failover

Po zakończeniu replikacji początkowej, a replikacja różnicowa jest uruchomiony, możesz uruchomić test trybu failover, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami.

Przejdź do [krok 12: testować tryb failover](vmware-walkthrough-test-failover.md)
