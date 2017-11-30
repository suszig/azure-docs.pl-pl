---
title: "Migracja maszyn wirtualnych IaaS platformy Azure między regiony platformy Azure | Dokumentacja firmy Microsoft"
description: "Użyj usługi Azure Site Recovery do migrowania maszyn wirtualnych IaaS platformy Azure z jednego regionu Azure do innego."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8a29e0d9-0010-4739-972f-02b8bdf360f6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: raynew
ms.openlocfilehash: f645150aa7b93ebbd98899cc8fdd495dfeaf3076
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migracja maszyn wirtualnych Azure IaaS między regiony platformy Azure z usługą Azure Site Recovery

Azure Site Recovery — Zapraszamy! W tym artykule należy użyć, jeśli chcesz przeprowadzić migrację maszyn wirtualnych platformy Azure między regiony platformy Azure.
>[!NOTE]
>
> Replikowanie maszyn wirtualnych platformy Azure do innego regionu na potrzeby migracja i odzyskiwania po awarii, można znaleźć w temacie [tego dokumentu](../site-recovery-azure-to-azure.md). Replikacja lokacji odzyskiwania maszyn wirtualnych platformy Azure jest obecnie w przeglądzie.


## <a name="prerequisites"></a>Wymagania wstępne
Oto, co jest potrzebne dla tego wdrożenia:

* **Maszyny wirtualne IaaS**: maszyny wirtualne, które chcesz migrować. Należy przeprowadzić migrację tych maszyn wirtualnych, traktując je jako maszyn fizycznych.

## <a name="deployment-steps"></a>Kroki wdrażania
W tej sekcji opisano kroki wdrażania nowego portalu Azure.

1. [Tworzenie magazynu](../site-recovery-azure-to-azure.md#create-a-recovery-services-vault).
2. [Włącz replikację..] /(Site-Recovery-Azure-to-Azure.MD) dla maszyn wirtualnych, aby dokonać migracji, a następnie wybierz pozycję Azure jako źródła.
  >[!NOTE]
  >
  > Obecnie natywnego replikacji maszyn wirtualnych Azure za pomocą dysków zarządzanych nie są obsługiwane. Możesz użyć opcji "Fizycznych do platformy Azure" w [tego dokumentu](../site-recovery-vmware-to-azure.md) do migracji maszyn wirtualnych z dyskami zarządzanych.
3. [Tryb failover](../site-recovery-failover.md). Po zakończeniu początkowej replikacji można uruchomić trybu failover z jednego regionu Azure do innego. Opcjonalnie możesz utworzyć plan odzyskiwania i tryb failover, aby przeprowadzić migrację wielu maszyn wirtualnych między regionami. [Dowiedz się więcej](../site-recovery-create-recovery-plans.md) o planach odzyskiwania.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o innych scenariuszy replikacji w [co to jest Azure Site Recovery?](../site-recovery-overview.md)
