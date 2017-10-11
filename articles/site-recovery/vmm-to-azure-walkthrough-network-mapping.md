---
title: "Konfigurowanie mapowania sieci replikowania maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM na platformie Azure za pomocą usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Opisuje sposób konfigurowania mapowania sieci podczas replikowania maszyn wirtualnych funkcji Hyper-V w chmurach VMM do platformy Azure z usługą Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: ed6f73d8baea5af0d2aa5f0ae885f305911ccc82
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2017
---
# <a name="step-9-configure-network-mapping-for-hyper-v-replication-with-vmm-to-azure"></a>Krok 9: Konfigurowanie mapowania sieci dla replikacji funkcji Hyper-V (w programie VMM) na platformie Azure

Po skonfigurowaniu [źródłowa i docelowa ustawień replikacji](vmm-to-azure-walkthrough-source-target.md), użyj w tym artykule, aby skonfigurować mapowanie sieci do mapowania między sieciami maszyn wirtualnych VMM lokalnych i sieci platformy Azure.

Opublikuj komentarze i pytania w dolnej części tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Przed rozpoczęciem

- Dowiedz się więcej o [mapowania sieci](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).
- [Przygotowanie programu VMM do mapowania sieci](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping). 
- Sprawdź, czy maszyny wirtualne na serwerze VMM zostały połączone z siecią maszyny wirtualnej i czy utworzono przynajmniej jedną sieć wirtualną platformy Azure. Wiele sieci maszyn wirtualnych można mapować do pojedynczej sieci platformy Azure.

## <a name="configure-mapping"></a>Skonfiguruj mapowanie

Skonfiguruj mapowanie w następujący sposób:

1. W obszarze **Infrastruktura usługi Site Recovery** > **Mapowania sieci** > **Mapowanie sieci** kliknij ikonę **+ Mapowanie sieci**.

    ![Mapowanie sieci](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping1.png)
2. W obszarze **Dodawanie mapowania sieci** wybierz źródłowy serwer programu VMM i ustaw **Azure** jako element docelowy.
3. Sprawdź subskrypcję i model wdrożenia po przejściu do trybu failover.
4. W obszarze **Sieć źródłowa** wybierz lokalną sieć maszyny wirtualnej do mapowania z listy skojarzonej z serwerem VMM.
5. W obszarze **Sieć docelowa** wybierz sieć platformy Azure, w której znajdą się repliki maszyn wirtualnych Azure po ich utworzeniu. Następnie kliknij przycisk **OK**.

    ![Mapowanie sieci](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping2.png)

Oto, co się dzieje po rozpoczęciu mapowania sieci:

* Istniejące maszyny wirtualne w źródłowej sieci maszyn wirtualnych są podłączane do docelowej sieci po rozpoczęciu mapowania. Nowe maszyny wirtualne, połączone ze źródłową siecią maszyn wirtualnych, są łączone z mapowaną siecią platformy Azure po replikacji.
* Jeśli modyfikujesz istniejące mapowanie sieci, repliki maszyn wirtualnych są łączone z wykorzystaniem nowych ustawień.
* Jeśli sieć docelowa ma wiele podsieci i jedna z tych podsieci ma taką samą nazwę, jak podsieć, w której znajduje się źródłowa maszyna wirtualna, replika maszyny wirtualnej jest łączona z tą docelową podsiecią po przejściu do trybu failover.
* Jeśli nie istnieje docelowa podsieć o takiej samej nazwie, maszyna wirtualna jest łączona z pierwszą podsiecią w sieci.



## <a name="next-steps"></a>Następne kroki

Przejdź do [kroku 10: Tworzenie zasad replikacji](vmm-to-azure-walkthrough-replication.md)
