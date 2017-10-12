---
title: "Przegląd wymagań wstępnych replikacji funkcji Hyper-V do platformy Azure (w programie System Center VMM) za pomocą usługi Azure Site Recovery | Microsoft Docs"
description: "Opisuje wymagania wstępne dotyczące konfigurowania replikacji, trybu failover i odzyskiwania lokalnych maszyn wirtualnych z funkcją Hyper-V w chmurach programu VMM do platformy Azure za pomocą usługi Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a1c30fd5-c979-473c-af44-4f725ad3e3ba
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/24/2017
ms.author: raynew
ms.openlocfilehash: 47c178c66ec98fe5d333edd725b64465026e73ed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="step-2-review-the-prerequisites-for-hyper-v-with-vmm-to-azure-replication"></a>Krok 2. Przegląd wymagań wstępnych replikacji funkcji Hyper-V (z programem VMM) do platformy Azure

Po przejrzeniu [architektury scenariusza](vmm-to-azure-walkthrough-architecture.md) przeczytaj ten artykuł, aby upewnić się, że rozumiesz wymagania wstępne dotyczące wdrażania. 

## <a name="prerequisites-and-limitations"></a>Wymagania wstępne i ograniczenia

**Wymaganie** | **Szczegóły**
--- | ---
**Konto platformy Azure** | Potrzebujesz [konta platformy Microsoft Azure](http://azure.microsoft.com/).
**Magazyn platformy Azure** | Potrzebujesz konta magazynu usługi Azure Storage do przechowywania replikowanych danych.<br/><br/> Konto magazynu musi znajdować się w tym samym regionie co magazyn usługi Azure Recovery Services.<br/><br/>Możesz używać [magazynu geograficznie nadmiarowego](../storage/common/storage-redundancy.md#geo-redundant-storage) lub magazynu lokalnie nadmiarowego. Zalecamy magazyn geograficznie nadmiarowy. Dzięki magazynowi geograficznie nadmiarowemu dane będą odporne w przypadku regionalnej awarii lub sytuacji, w której nie można odzyskać regionu podstawowego.<br/><br/> Możesz używać standardowego konta usługi Azure Storage lub konta usługi Azure [Premium Storage](../storage/common/storage-premium-storage.md). Usługa Premium Storage może obsługiwać obciążenia intensywnie korzystające z operacji we/wy i zwykle jest używana dla maszyn wirtualnych wymagających spójnej wysokiej wydajności we/wy i małych opóźnień. Jeśli używasz usługi Premium Storage do przechowywania replikowanych danych, potrzebujesz też standardowego konta magazynu. Standardowe konto magazynu służy do przechowywania dzienników replikacji, które przechwytują zachodzące zmiany w danych lokalnych.
**Sieć platformy Azure** | Potrzebujesz [sieci platformy Azure](../virtual-network/virtual-network-get-started-vnet-subnet.md), z którą maszyny wirtualne platformy Azure zostaną połączone po przejściu do trybu failover. Sieć platformy Azure musi znajdować się w tym samym regionie co magazyn usługi Recovery Services.
**Lokalne serwery programu VMM** | Potrzebujesz co najmniej jednego serwera programu VMM z uruchomionym programem System Center 2012 R2 lub nowszym.<br/><br/> Każdy serwer programu VMM musi mieć co najmniej jedną chmurę prywatną. Każda chmura potrzebuje co najmniej jednej grupy hostów.<br/><br/> Serwer programu VMM musi mieć dostęp do Internetu.
**Lokalna funkcja Hyper-V** | Serwery hostów funkcji Hyper-V muszą być uruchomione w systemie Windows Server 2012 R2 z włączoną rolą Hyper-V lub w systemie Microsoft Hyper-V Server 2012 R2. Muszą być zainstalowane najnowsze aktualizacje.<br/><br/> Host funkcji Hyper-V musi znajdować się w grupie hostów programu VMM (znajdującej się w chmurze programu VMM).<br/><br/> Host musi mieć co najmniej jedną maszynę wirtualną, która ma być replikowana.<br/><br/> Hosty funkcji Hyper-V muszą mieć połączenie z Internetem na potrzeby replikacji do platformy Azure, bezpośrednio lub przy użyciu serwera proxy. Serwery funkcji Hyper-V muszą mieć poprawki opisane w artykule [2961977](https://support.microsoft.com/kb/2961977).
**Lokalne maszyny wirtualne funkcji Hyper-V** | Maszyny wirtualne, które mają być replikowane, powinny mieć [obsługiwane systemy operacyjne](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) i spełniać [wymagania wstępne platformy Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). Nazwę maszyny wirtualnej można zmodyfikować po włączeniu replikacji. 




## <a name="next-steps"></a>Następne kroki

Przejdź do sekcji [Krok 3. Planowanie wydajności](vmm-to-azure-walkthrough-capacity.md)
