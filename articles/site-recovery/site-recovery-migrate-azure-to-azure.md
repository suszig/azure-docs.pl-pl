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
ms.openlocfilehash: 86806c5dbafc1fd88c434dcee6292683d050cd2a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migracja maszyn wirtualnych Azure IaaS między regiony platformy Azure z usługą Azure Site Recovery
## <a name="overview"></a>Omówienie
Azure Site Recovery — Zapraszamy! W tym artykule należy użyć, jeśli chcesz przeprowadzić migrację maszyn wirtualnych platformy Azure między regiony platformy Azure.
>[!NOTE]
>
> Replikowanie maszyn wirtualnych platformy Azure do innego regionu na potrzeby migracja i odzyskiwania po awarii, można znaleźć w temacie [tego dokumentu](site-recovery-azure-to-azure.md). Replikacja lokacji odzyskiwania maszyn wirtualnych platformy Azure jest obecnie w przeglądzie.

Przed rozpoczęciem należy pamiętać, że:

* Platforma Azure ma dwa różne modele wdrażania do tworzenia i pracy z zasobami: Azure Resource Manager i model klasyczny. Platforma Azure ma dwa portale — klasyczny portal Azure, który obsługuje klasyczny model wdrażania, oraz portal Azure, który obsługuje oba modele wdrażania. Podstawowe czynności w przypadku migracji są takie same, czy w przypadku konfigurowania usługi Site Recovery w Menedżerze zasobów lub w klasycznym. Jednak instrukcje interfejsu użytkownika i zrzuty ekranu w tym artykule dotyczą portalu Azure.



Zamieść wszelkie komentarze lub pytania pod tym artykułem lub na [forum Usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Wymagania wstępne
Oto, co jest potrzebne dla tego wdrożenia:

* **Maszyny wirtualne IaaS**: maszyny wirtualne, które chcesz migrować. Należy przeprowadzić migrację tych maszyn wirtualnych, traktując je jako maszyn fizycznych.

## <a name="deployment-steps"></a>Kroki wdrażania
W tej sekcji opisano kroki wdrażania nowego portalu Azure.

1. [Tworzenie magazynu](site-recovery-azure-to-azure.md#create-a-recovery-services-vault).
2. [Włącz replikację](site-recovery-azure-to-azure.md) dla maszyn wirtualnych, aby dokonać migracji, a następnie wybierz pozycję Azure jako źródła.
  >[!NOTE]
  >
  > Obecnie natywnego replikacji maszyn wirtualnych Azure za pomocą dysków zarządzanych nie są obsługiwane. Możesz użyć opcji "Fizycznych do platformy Azure" w [tego dokumentu](site-recovery-vmware-to-azure.md) do migracji maszyn wirtualnych z dyskami zarządzanych.
3. [Tryb failover](site-recovery-failover.md). Po zakończeniu początkowej replikacji można uruchomić trybu failover z jednego regionu Azure do innego. Opcjonalnie możesz utworzyć plan odzyskiwania i tryb failover, aby przeprowadzić migrację wielu maszyn wirtualnych między regionami. [Dowiedz się więcej](site-recovery-create-recovery-plans.md) o planach odzyskiwania.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o innych scenariuszy replikacji w [co to jest Azure Site Recovery?](site-recovery-overview.md)
