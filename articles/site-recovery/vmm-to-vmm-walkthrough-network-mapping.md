---
title: "Mapowanie sieci dla maszyny Wirtualnej funkcji Hyper-V replikacji do lokacji dodatkowej z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Opisuje sposób mapowania sieci podczas replikowania maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej programu VMM z usługą Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 461b7c1c-ef61-4005-aeec-2324e727a3d0
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 606e2d3d0e31b9d80200105e812f9d7bbbcf939c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="step-7-map-networks-for-hyper-v-vm-replication-to-a-secondary-site"></a>Krok 7: Mapowanie sieci dla maszyny Wirtualnej funkcji Hyper-V replikacji do lokacji dodatkowej


Po skonfigurowaniu [ustawienia źródłowe i docelowe](vmm-to-vmm-walkthrough-source-target.md) replikowania maszyn wirtualnych funkcji Hyper-V (VM) do lokacji dodatkowej programu System Center Virtual Machine Manager (VMM), użyj w tym artykule, aby skonfigurować mapowanie sieci dla funkcji Hyper-V maszyny wirtualnej ( Replikacja maszyny Wirtualnej) do dodatkowej lokacji przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md).

Po przeczytaniu tego artykułu zamieść wszelkie komentarze u dołu lub na [forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Przed rozpoczęciem

- Dowiedz się więcej o [mapowania sieci](vmm-to-vmm-walkthrough-network.md#network-mapping-overview) przed jego uruchomieniem.
- Sprawdź, czy maszyny wirtualne na serwerach VMM są połączone z siecią maszyny Wirtualnej.

## <a name="configure-network-mapping"></a>Konfiguracja mapowania sieci

1. W **mapowanie sieci** > **mapowania sieci**, kliknij przycisk **+ mapowanie sieci**.
2. W **Dodaj mapowanie sieci** , wybierz źródło i VMM serwery docelowe. Pobierane są wszystkie sieci maszyny Wirtualnej skojarzone z serwerów programu VMM.
3. W **Sieć źródłowa**, wybierz sieć, którego chcesz użyć na liście sieci maszyny Wirtualnej skojarzone z podstawowym serwerem programu VMM.
4. W **Sieć docelowa**, wybierz sieć, którego chcesz użyć na pomocniczym serwerze programu VMM. Następnie kliknij przycisk **OK**.

    ![Mapowanie sieci](./media/vmm-to-vmm-walkthrough-network-mapping/network-mapping2.png)

Oto, co się dzieje po rozpoczęciu mapowania sieci:

* Wszystkie istniejące maszyny wirtualne repliki odpowiadające źródłowej sieci maszyny Wirtualnej zostaną podłączone do sieci docelowej maszyny Wirtualnej.
* Nowe maszyny wirtualne, które są podłączone do źródłowej sieci maszyny Wirtualnej zostanie podłączona do sieci docelowej mapowane po replikacji.
* Jeśli zmodyfikujesz istniejące mapowanie, uwzględniając nową sieć, maszyn wirtualne repliki zostaną podłączone z wykorzystaniem nowych ustawień.
* Jeśli sieć docelowa ma wiele podsieci i jedna z tych podsieci ma taką samą nazwę, jak podsieć, w której znajduje się źródłowa maszyna wirtualna, replika maszyny wirtualnej zostanie podłączona do tej docelowej podsieci po przejściu do trybu failover. Jeśli nie istnieje docelowa podsieć o takiej samej nazwie, maszyna wirtualna zostanie podłączona do pierwszej podsieci w sieci.



## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 8: Skonfiguruj zasady replikacji](vmm-to-vmm-walkthrough-replication.md).