---
title: "Replikowanie maszyn wirtualnych funkcji Hyper-V na platformie Azure za pomocą usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Opisuje sposób organizowania replikacji, trybu failover i odzyskiwania maszyn wirtualnych funkcji Hyper-V lokalnego do platformy Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: efddd986-bc13-4a1d-932d-5484cdc7ad8d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: da10b213bc2543942b5ac77cf5c5d8547c00220c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure"></a>Replikowanie maszyn wirtualnych funkcji Hyper-V (bez VMM) na platformie Azure 

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [Klasyczny portal Azure](site-recovery-hyper-v-site-to-azure-classic.md)
> * [Program PowerShell — model usługi Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

W tym artykule omówiono kroki wymagane do replikowania maszyn wirtualnych funkcji Hyper-V lokalnej na platformie Azure, przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) w portalu Azure. W tym maszyn wirtualnych funkcji Hyper-V wdrożenia nie są zarządzane przez System Center Virtual Machine Manager (VMM).


Po przeczytaniu tego artykułu, post wszelkie komentarze u dołu i zadawaj pytania techniczne na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="step-1-review-architecture-and-prerequisites"></a>Krok 1: Przegląd architektury i wymagania wstępne

Przed rozpoczęciem wdrażania zapoznaj się z architekturą scenariusza i upewnij się, że znasz wszystkie składniki potrzebne do wdrożenia

Przejdź do [krok 1: Przejrzyj architektury](hyper-v-site-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Krok 2: Sprawdź wymagania wstępne dotyczące

Upewnij się, że zostały spełnione wymagania wstępne w przypadku każdego składnika wdrażania:

- **Wymagania wstępne platformy Azure**: wymagane konto Microsoft Azure, sieci platformy Azure i kont magazynu.
- **Wymagania wstępne funkcji Hyper-V lokalnymi**: Upewnij się, że hosty funkcji Hyper-V są przygotowane do wdrażania usługi Site Recovery.
- **Maszyny wirtualne replikowane**: maszyny wirtualne mają być replikowane musi spełniać wymagania dotyczące usługi Azure.

Przejdź do [krok 2: Sprawdź wymagania wstępne i ograniczenia](hyper-v-site-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Krok 3: Planowanie pojemności

Jeśli pełne wdrożenie robimy należy dowiedzieć się, jakie potrzebne zasoby replikacji. Istnieje kilka narzędzi ułatwiających to zrobić. Przejdź do kroku 2. Jeśli podczas wykonywania zestawu szybkiego się do środowiska testowego można pominąć ten krok.

Przejdź do sekcji [Krok 3. Planowanie wydajności](hyper-v-site-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>Krok 4: Planowanie sieci

Należy wykonać niektóre sieci, planowanie upewnić się, czy maszynach wirtualnych platformy Azure są połączone z sieciami po pracy awaryjnej i które mają prawo IP adresy.

Przejdź do [krok 4: Planowanie sieci](hyper-v-site-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>Krok 5: Przygotowanie zasobów platformy Azure

Konfigurowanie sieci platformy Azure i magazynu przed jego uruchomieniem. Można to zrobić podczas wdrażania, ale zaleca się, że można to zrobić przed rozpoczęciem.

Przejdź do [krok 5: przygotowanie Azure](hyper-v-site-walkthrough-prepare-azure.md)


## <a name="step-6-prepare-hyper-v"></a>Krok 6: Przygotowanie funkcji Hyper-V

Upewnij się, że serwery funkcji Hyper-V spełniają wymagania dotyczące wdrażania usługi Site Recovery.

Przejdź do [krok 6: Przygotowanie funkcji Hyper-V](hyper-v-site-walkthrough-prepare-hyper-v.md)

## <a name="step-7-set-up-a-vault"></a>Krok 7: Konfigurowanie magazynu

Należy skonfigurować magazyn usług odzyskiwania do organizowania replikacji. Po skonfigurowaniu magazynu można określić co chcesz replikować, a które chcesz replikować go do.

Przejdź do [kroku 7: Tworzenie magazynu](hyper-v-site-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>Krok 8: Konfigurowanie ustawień źródłowa i docelowa

Skonfiguruj źródłowy i docelowy, który jest używany do replikacji. Konfigurowanie ustawień źródła obejmuje dodawanie hostów funkcji Hyper-V do lokacji funkcji Hyper-V, zainstalowanie agenta dostawcy usługi Site Recovery i usług odzyskiwania na każdym hoście funkcji Hyper-V i rejestrowanie w magazynie usług odzyskiwania lokacji.

Przejdź do [krok 8: Konfigurowanie źródłowych i docelowych](hyper-v-site-walkthrough-source-target.md)

## <a name="step-9-set-up-a-replication-policy"></a>Krok 9: Konfigurowanie zasad replikacji

Aby określić ustawienia replikacji dla maszyn wirtualnych funkcji Hyper-V w magazynie skonfigurować zasadę.

Przejdź do [krok 9: Konfigurowanie zasad replikacji](hyper-v-site-walkthrough-replication.md)


## <a name="step-10-enable-replication"></a>Krok 10: Włączanie replikacji

Po utworzeniu zasad replikacji w miejscu, po włączeniu występuje początkowej replikacji maszyny wirtualnej.

Przejdź do [kroku 10: Włączanie replikacji](hyper-v-site-walkthrough-enable-replication.md)

## <a name="step-11-run-a-test-failover"></a>Krok 11: Uruchom test trybu failover

Po zakończeniu replikacji początkowej, a replikacja różnicowa jest uruchomiony, możesz uruchomić test trybu failover, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami.

Przejdź do [krok 11: testować tryb failover](hyper-v-site-walkthrough-test-failover.md)
