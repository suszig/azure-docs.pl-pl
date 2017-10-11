---
title: "Replikowanie maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej programu VMM z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Zawiera omówienie replikowania maszyn wirtualnych funkcji Hyper-V na lokacja dodatkowa programu VMM przy użyciu portalu Azure."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 476ca82a-8f5c-4498-9dcf-e1011d60ed59
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: raynew
ms.openlocfilehash: b422dd2cf23426de2f154a553b38509082536309
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a>Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach VMM do dodatkowej lokacji programu VMM

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Portal klasyczny](site-recovery-vmm-to-vmm-classic.md)
> * [Program PowerShell — model usługi Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Ten artykuł zawiera omówienie kroków wymaganych do replikowania maszyn wirtualnych funkcji Hyper-V lokalnymi (VM) zarządzane w programie System Center Virtual Machine Manager (VMM) chmury do dodatkowej lokalizacji programu VMM, przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) w portalu Azure.

Po przeczytaniu tego artykułu zamieść wszelkie komentarze u dołu lub na [forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="step-1-review-the-scenario-architecture"></a>Krok 1: Przejrzyj architektura scenariusza

Przed rozpoczęciem wdrażania, przejrzyj architektura scenariusza i upewnij się, że znasz wszystkie składniki potrzebne do wdrożenia.

Przejdź do [krok 1: Przejrzyj architektury](vmm-to-vmm-walkthrough-architecture.md).

## <a name="step-2-review-prerequisites-and-limitations"></a>Krok 2: Sprawdź wymagania wstępne i ograniczenia

Upewnij się, że rozumiesz wymagania wstępne dotyczące wdrażania i ograniczenia.

**Wymagania wstępne platformy Azure**: potrzebujesz subskrypcji Microsoft Azure, a magazyn usług odzyskiwania Azure, do organizowania replikacji.
**Serwery lokalne, VMM i hosty funkcji Hyper-V**: Upewnij się, że serwery VMM i hosty funkcji Hyper-V są zgodne i przygotowania się do wdrażania usługi Site Recovery.

Przejdź do [krok 2: Sprawdź wymagania wstępne i ograniczenia](vmm-to-vmm-walkthrough-prerequisites.md).

## <a name="step-3-plan-networking"></a>Krok 3: Planowanie sieci

Należy wykonać niektóre sieci, planowanie upewnić się, że możesz skonfigurować mapowanie sieci między sieciami maszyny Wirtualnej VMM podczas wdrażania tego scenariusza.

Przejdź do [krok 3: Planowanie sieci](vmm-to-vmm-walkthrough-network.md).


## <a name="step-4-prepare-vmm-and-hyper-v"></a>Krok 4: Przygotowanie VMM i funkcji Hyper-V

Przygotowanie serwerów programu VMM i hosty funkcji Hyper-V do wdrażania usługi Site Recovery.

Przejdź do [krok 4: Przygotowanie serwerów lokalnych](vmm-to-vmm-walkthrough-vmm-hyper-v.md).

## <a name="step-5-set-up-a-vault"></a>Krok 5: Konfigurowanie magazynu

Konfigurowanie magazynu usług odzyskiwania. Magazyn zawiera ustawienia konfiguracji i aranżuje replikację.

[Krok 5: Konfigurowanie magazynu](vmm-to-vmm-walkthrough-create-vault.md).

## <a name="step-6-set-up-source-and-target-settings"></a>Krok 6: Konfigurowanie ustawień źródłowa i docelowa

Konfigurowanie lokalizacja źródłowa i docelowa replikacji programu VMM. Dodawanie serwerów programu VMM w magazynie i pobierane pliki instalacyjne dla składników usługi Site Recovery. Uruchom Instalatora dostawcy usługi Azure Site Recovery na serwerze programu VMM. Instalator instaluje dostawcę na serwerze programu VMM i rejestruje serwer w magazynie. Agent usług odzyskiwania Microsoft należy zainstalować na każdym hoście funkcji Hyper-V.

Przejdź do [krok 6: Konfigurowanie ustawień źródłowa i docelowa](vmm-to-vmm-walkthrough-source-target.md).

## <a name="step-7-configure-network-mapping"></a>Krok 7. Konfigurowanie mapowania sieci

Mapowanie sieci maszyny Wirtualnej programu VMM w lokalizacji źródłowej i docelowej. Po przejściu w tryb failover maszyny wirtualne są tworzone w sieci docelowej, który jest mapowany na źródłowej sieci maszyny Wirtualnej, w którym znajduje się źródłowej maszyny Wirtualnej funkcji Hyper-V.

Przejdź do [kroku 7: Konfigurowanie mapowania sieci](vmm-to-vmm-walkthrough-network-mapping.md).


## <a name="step-8-set-up-a-replication-policy"></a>Krok 8: Konfigurowanie zasad replikacji

Określ, jak będą replikowane maszyny wirtualne między lokacjami programu VMM.

Przejdź do [krok 8: Skonfiguruj zasady replikacji](vmm-to-vmm-walkthrough-replication.md).


## <a name="step-9-enable-replication-for-vms"></a>Krok 9: Włączanie replikacji maszyn wirtualnych

Wybierz maszyny wirtualne mają być replikowane. Włączanie replikacji maszyny Wirtualnej wyzwala na lokacji dodatkowej, następuje replikacja różnicowa trwającej replikacji początkowej.

Przejdź do [krok 9: Włączanie replikacji](vmm-to-vmm-walkthrough-enable-replication.md).


## <a name="step-10-run-a-test-failover"></a>Krok 10: Uruchom test trybu failover

Uruchom testowanie trybu failover, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami.

Przejdź do [kroku 10: testować tryb failover](vmm-to-vmm-walkthrough-test-failover.md).
