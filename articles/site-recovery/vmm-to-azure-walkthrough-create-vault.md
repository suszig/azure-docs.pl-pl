---
title: "Konfigurowanie magazynu dla replikacji funkcji Hyper-V (za pomocą programu System Center VMM) na platformie Azure przy użyciu usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Zawiera podsumowanie kroków, które należy skonfigurować Magazyn replikacji funkcji Hyper-V (w programie VMM) na platformie Azure przy użyciu usługi Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: b3cd6f03-c33c-406d-91d4-5cba69f79abd
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: af453ec27ba15ad8c59cf9f544584ad18dc0f74a
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a>Krok 7: Konfigurowanie magazynu dla replikacji funkcji Hyper-V

W tym artykule opisano sposób konfigurowania magazynu i określ, co chcesz replikować z tej lokalizacji lokalnego do platformy Azure przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) w portalu Azure.


Opublikuj komentarze i pytania w dolnej części tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu Usług odzyskiwania

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]



## <a name="select-a-protection-goal"></a>Wybierz cel ochrony

Wybierz, co chcesz replikować, i miejsce, do którego chcesz przeprowadzać replikację.

1. Kliknij przycisk **Magazyny usług odzyskiwania** > magazynu.
2. W Menu zasobów kliknij **usługi Site Recovery** > **przygotowanie infrastruktury** > **cel ochrony**.
3. W **cel ochrony**, wybierz pozycję **do platformy Azure** > **tak, z funkcją Hyper-V**. Wybierz **tak** potwierdzenie jest %1./nOdnosi VMM. 

     ![Wybieranie celów](./media/vmm-to-azure-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 8: Konfigurowanie źródłowa i docelowa](vmm-to-azure-walkthrough-source-target.md)
