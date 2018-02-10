---
title: "Uruchom wyszczególniania odzyskiwania po awarii do lokacji dodatkowej lokalnymi z usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej o uruchamianiu wyszczególniania odzyskiwania po awarii do lokacji dodatkowej lokalnymi z usługą Azure Site Recovery"
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/07/2018
ms.author: raynew
ms.openlocfilehash: 2e5f8dce1ca2f728d15161622fb9ff2afb4b6c86
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="run-a-disaster-recovery-drill-for-hyper-v-vms-to-a-secondary-on-premises-site"></a>Uruchom wyszczególniania odzyskiwania po awarii dla maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej lokalnymi

[Usługi Azure Site Recovery](site-recovery-overview.md) usługi przyczynia się do strategii odzyskiwania po awarii poprzez organizowanie replikacji, trybu failover i powrotu po awarii maszyn lokalnych i Azure maszynach wirtualnych (VM) i zarządzanie nimi.

W tym samouczku przedstawiono sposób uruchamiania wyszczególniania odzyskiwania po awarii dla maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej lokalnymi. Maszyny wirtualne funkcji Hyper-V musi być zarządzane w chmurze prywatnej System Center Virtual Machine Manager (VMM). Przechodzenie do szczegółów weryfikuje strategii replikacji bez utraty danych lub czas przestoju i nie ma wpływu na wpływu na środowisko produkcyjne. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Przygotowanie wymagań wstępnych dotyczących testowania trybu failover
> * Uruchom test trybu failover dla pojedynczego komputera


## <a name="prerequisites"></a>Wymagania wstępne

- Możesz uruchomić test trybu failover z kilku opcji dla sieci w lokacji dodatkowej. Można uruchomić trybu failover bez sieci, z istniejącej sieci, lub pozwolić Site Recovery automatycznie twórz sieci testowej. 
**Jeśli chcesz użyć istniejącej sieci produkcyjnej do testowania trybu failover:**:
    - Podczas testowania trybu failover podczas wykonywania podstawowej maszyny Wirtualnej powinny zostać wyłączone. W przeciwnym razie dwie maszyny wirtualne o tej samej tożsamości będzie uruchomiony w tej samej sieci, w tym samym czasie. 
    - Jeśli wprowadzisz zmiany, aby przetestować maszyn wirtualnych, zmiany zostaną utracone podczas czyszczenia testowania trybu failover. Zmiany nie są zreplikowane na podstawowej maszynie Wirtualnej.
    - Testowanie sieci produkcyjnej powoduje, że czas przestoju w przypadku obciążeń produkcyjnych. Poproś nie użytkownikom korzystanie z aplikacji pokrewnych, gdy trwa wyszczególniania odzyskiwania po awarii. 
- Sieci testowej repliki nie musi być zgodny typ sieci logicznej VMM, które są używane do testowania trybu failover. Jednak niektóre kombinacje nie działają. Na przykład jeśli replika używa protokołu DHCP i izolacji opartej na sieci VLAN, nie można użyć wirtualizacja sieci systemu Windows do testowej sieci trybu failover, ponieważ wymaga puli adresów IP. 
- Zaleca się, że wybrana sieć nie jest używany do mapowania sieci do testowania trybu failover.


## <a name="run-a-test-failover-for-a-vm"></a>Uruchom test trybu failover dla maszyny Wirtualnej

1. W **elementy replikowane**, kliknij maszynę Wirtualną > **testowy tryb Failover**.
2. W **testowego trybu Failover**, określ, jak test maszyny wirtualne zostaną podłączone do sieci po testowania trybu failover. Firma Microsoft zaleca w tym samouczku, aby automatycznie twórz sieci testowej usługi Site Recovery. [Dowiedz się więcej](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery).
3. Kliknij przycisk **OK**, aby rozpocząć tryb failover. Śledzenie postępów **zadania** kartę.
4. Po zakończeniu trybu failover Sprawdź, czy test maszyn wirtualnych pomyślnie uruchomić.
5. Gdy wszystko będzie gotowe, kliknij przycisk **oczyszczania testowy tryb failover**. Spowoduje to usunięcie testu maszyn wirtualnych i sieciami utworzone podczas testowania trybu failover.
6. W **uwagi**, zarejestrować i zapisać wszelkie obserwacje związane z testowania trybu failover. 


## <a name="next-steps"></a>Kolejne kroki

[Tryb failover produkcji](tutorial-vmm-to-vmm-failover-failback.md)






