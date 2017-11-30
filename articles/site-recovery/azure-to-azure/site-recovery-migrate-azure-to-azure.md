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
ms.date: 08/31/2017
ms.author: raynew
ms.openlocfilehash: 805582d89ee906e21fbe588e895ce0fe3a926a66
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migracja maszyn wirtualnych Azure IaaS między regiony platformy Azure z usługą Azure Site Recovery

W tym artykule należy użyć do migracji maszyn wirtualnych platformy Azure (maszyny wirtualne) między regiony platformy Azure przy użyciu [usługi Site Recovery](../site-recovery-overview.md) usługi.

## <a name="prerequisites"></a>Wymagania wstępne

Należy chcesz przeprowadzić migrację maszyn wirtualnych IaaS.

## <a name="deployment-steps"></a>Kroki wdrażania

1. [Tworzenie magazynu](azure-to-azure-tutorial-enable-replication.md#create-a-vault).
2. [Włącz replikację](azure-to-azure-tutorial-enable-replication.md#enable-replication) dla maszyn wirtualnych, aby dokonać migracji, a następnie wybierz pozycję Azure jako źródła. Obecnie natywnego replikacji maszyn wirtualnych Azure za pomocą dysków zarządzanych nie jest obsługiwana.
3. [Tryb failover](../site-recovery-failover.md). Po zakończeniu początkowej replikacji można uruchomić trybu failover z jednego regionu Azure do innego. Opcjonalnie możesz utworzyć plan odzyskiwania i tryb failover, aby przeprowadzić migrację wielu maszyn wirtualnych między regionami. [Dowiedz się więcej](../site-recovery-create-recovery-plans.md) o planach odzyskiwania.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o innych scenariuszy replikacji w [co to jest Azure Site Recovery?](../site-recovery-overview.md)
