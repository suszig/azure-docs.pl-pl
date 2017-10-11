---
title: "Konfigurowanie magazynu dla replikacji maszyn wirtualnych VMware do platformy Azure przy użyciu usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Zawiera podsumowanie kroków, które należy skonfigurować magazynu dla replikacji maszyn wirtualnych VMware do platformy Azure przy użyciu usługi Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8bce940e-f19f-4418-8360-aee7b073519a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: dca95ad46b8de587140c3573ba6ed5702a122032
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="step-7-set-up-a-vault-for-vmware-replication-to-azure"></a>Krok 7: Konfigurowanie magazynu dla replikacji maszyn wirtualnych VMware do platformy Azure


W tym artykule opisano sposób konfigurowania magazynu i określ, co chcesz replikować z tej lokalizacji lokalnego do platformy Azure przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) w portalu Azure.


Opublikuj komentarze i pytania w dolnej części tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu Usług odzyskiwania

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Wybierz cel ochrony

Wybierz, co chcesz replikować, i miejsce, do którego chcesz przeprowadzać replikację.

1. Kliknij przycisk **Magazyny usług odzyskiwania** > magazynu.
2. W Menu zasobów kliknij **usługi Site Recovery** > **przygotowanie infrastruktury** > **cel ochrony**.
3. W **cel ochrony**, wybierz pozycję **do platformy Azure** > **tak, z programem VMware vSphere Hypervisor**.



## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 8: Konfigurowanie źródłowa i docelowa](vmware-walkthrough-source-target.md)
