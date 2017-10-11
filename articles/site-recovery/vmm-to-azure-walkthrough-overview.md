---
title: "Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach VMM do platformy Azure z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Zawiera omówienie replikowania maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM na platformie Azure przy użyciu usługi Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: af68d21184c137b2b0f1bb4c1afb9bf507e8332d
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-site-recovery-in-the-azure-portal"></a>Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM do platformy Azure przy użyciu usługi Site Recovery w witrynie Azure Portal
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [Klasyczny portal Azure](site-recovery-vmm-to-azure-classic.md)
> * [Program PowerShell — model usługi Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [PowerShell — model klasyczny](site-recovery-deploy-with-powershell.md)


Ten artykuł zawiera omówienie kroków wymaganych do replikowania maszyn wirtualnych funkcji Hyper-V lokalnymi (VM) zarządzane w programie System Center Virtual Machine Manager (VMM) chmury Azure, przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) w portalu Azure.

Po przeczytaniu tego artykułu zamieść wszelkie komentarze u dołu lub na [forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="step-1-review-the-scenario-architecture"></a>Krok 1: Przejrzyj architektura scenariusza

Przed rozpoczęciem wdrażania, przejrzyj architektura scenariusza i upewnij się, że znasz wszystkie składniki potrzebne do wdrożenia.

Przejdź do [krok 1: Przejrzyj architektury](vmm-to-azure-walkthrough-architecture.md)

## <a name="step-2-review-prerequisites-and-limitations"></a>Krok 2: Sprawdź wymagania wstępne i ograniczenia

Upewnij się, że rozumiesz wymagania wstępne dotyczące wdrażania i ograniczenia.

**Wymagania wstępne platformy Azure**: wymagane konto Microsoft Azure, sieci platformy Azure i kont magazynu.
**Serwery lokalne, VMM i hosty funkcji Hyper-V**: Upewnij się, że serwery VMM i hosty funkcji Hyper-V są zgodne i przygotowania się do wdrażania usługi Site Recovery.
**Maszyny wirtualne replikowane**: Sprawdź, że maszyny wirtualne mają być replikowane spełniają wymagania dotyczące usługi Azure.

Przejdź do [krok 2: Sprawdź wymagania wstępne i ograniczenia](vmm-to-azure-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Krok 3: Planowanie pojemności

Jeśli pełne wdrożenie robimy, należy dowiedzieć się, jakie potrzebne zasoby replikacji. Istnieje kilka narzędzi ułatwiających to zrobić. Jeśli podczas wykonywania szybkiego skonfigurowany do środowiska testowego, możesz pominąć ten krok.

Przejdź do sekcji [Krok 3. Planowanie wydajności](vmm-to-azure-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>Krok 4: Planowanie sieci

Należy wykonać niektóre sieci, planowanie upewnić się, że możesz skonfigurować mapowanie sieci, podczas wdrażania tego scenariusza, że maszyny wirtualne Azure zostaną podłączone do sieci wirtualnych platformy Azure po pracy awaryjnej, oraz że je przypisano odpowiednie IP adresów.

Przejdź do [krok 4: Planowanie sieci](vmm-to-azure-walkthrough-network.md)


## <a name="step-5-prepare-azure-resources"></a>Krok 5: Przygotowanie zasobów platformy Azure

Konfigurowanie konta platformy Azure, sieci i magazynu. Można to zrobić podczas wdrażania, ale zaleca się, że można to zrobić przed rozpoczęciem.

Przejdź do [krok 5: przygotowanie Azure](vmm-to-azure-walkthrough-prepare-azure.md)

## <a name="step-6-prepare-vmm-and-hyper-v"></a>Krok 6: Przygotowanie VMM i funkcji Hyper-V

Przygotuj serwery lokalne, VMM i hosty funkcji Hyper-V do wdrażania usługi Site Recovery.

Przejdź do [krok 6: Przygotowanie serwerów lokalnych](vmm-to-azure-walkthrough-vmm-hyper-v.md)

## <a name="step-7-set-up-a-vault"></a>Krok 7: Konfigurowanie magazynu

Konfigurowanie magazynu usług odzyskiwania. Magazyn zawiera ustawienia konfiguracji i aranżuje replikację.

[Krok 7: Konfigurowanie magazynu](vmm-to-azure-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>Krok 8: Konfigurowanie ustawień źródłowa i docelowa

Konfigurowanie replikacji lokalizacja źródłowa i docelowa. Dodaj serwer programu VMM w magazynie i pobierane pliki instalacyjne dla składników usługi Site Recovery. Uruchom Instalatora dostawcy usługi Azure Site Recovery na serwerze programu VMM. Instalator instaluje dostawcę na serwerze programu VMM i rejestruje serwer w magazynie. Agent usług odzyskiwania Microsoft należy zainstalować na każdym hoście funkcji Hyper-V.

Przejdź do [krok 8: Konfigurowanie ustawień źródłowa i docelowa](vmm-to-azure-walkthrough-source-target.md)

## <a name="step-9-configure-network-mapping"></a>Krok 9: Konfigurowanie mapowania sieci

Mapa lokalnych sieci maszyny Wirtualnej VMM do sieci wirtualnych platformy Azure. Po przejściu w tryb failover maszynach wirtualnych platformy Azure są tworzone w sieci platformy Azure, który jest mapowany na lokalnej sieci maszyny Wirtualnej, w którym znajduje się źródło funkcji Hyper-V.

Przejdź do [krok 9: Konfigurowanie mapowania sieci](vmm-to-azure-walkthrough-network-mapping.md)


## <a name="step-10-set-up-a-replication-policy"></a>Krok 10: Konfigurowanie zasad replikacji

Określ, jak będą replikowane lokalnych maszyn wirtualnych na platformie Azure.

Przejdź do [10 kroku: Konfigurowanie zasad replikacji](vmm-to-azure-walkthrough-replication.md)


## <a name="step-11-enable-replication-for-vms"></a>Krok 11: Włączanie replikacji maszyn wirtualnych

Wybierz maszyny wirtualne mają być replikowane. Włączanie replikacji maszyny Wirtualnej wyzwala replikacji początkowej na platformie Azure, następuje replikacja różnicowa stałe.

Przejdź do [krok 11: Włączanie replikacji](vmm-to-azure-walkthrough-enable-replication.md)


## <a name="step-12-run-a-test-failover"></a>Krok 12: Uruchom test trybu failover

Uruchom testowanie trybu failover, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami.

Przejdź do [krok 12: testować tryb failover](vmm-to-azure-walkthrough-test-failover.md)


