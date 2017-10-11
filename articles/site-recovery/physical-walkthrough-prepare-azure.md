---
title: "Przygotowanie zasobów platformy Azure w ramach replikacji lokalnych serwerów fizycznych do platformy Azure przy użyciu usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, co jest potrzebne w miejscu na platformie Azure przed rozpoczęciem replikacji lokalnych serwerów na platformę Azure przy użyciu usługi Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 4e320d9b-8bb8-46bb-ba21-77c5d16748ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: raynew
ms.openlocfilehash: b7411fa6aba04ffd34f3f4bd03e706ca75afc9c8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-physical-server-replication-to-azure"></a>Krok 5: Przygotowanie zasobów Azure dla replikacji serwera fizycznego na platformie Azure


Postępuj zgodnie z instrukcjami w tym artykule, aby przygotować zasobów platformy Azure, aby serwerów lokalnych można replikować do platformy Azure przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) usługi.

Opublikuj komentarze i pytania w dolnej części tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Przed rozpoczęciem

Upewnij się, że zostały przeczytane [wymagania wstępne](physical-walkthrough-prerequisites.md).

## <a name="set-up-an-azure-account"></a>Konfigurowanie konta platformy Azure

- Pobierz [konta Microsoft Azure](http://azure.microsoft.com/).
- Możesz rozpocząć od [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
- Sprawdź, czy obsługiwane regiony usługi Site Recovery w obszarze **dotyczącą dostępności geograficznej** w [szczegóły cennika usługi Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
- Dowiedz się więcej o [cenach usługi Site Recovery](site-recovery-faq.md#pricing)i uzyskać [szczegóły cennika](https://azure.microsoft.com/pricing/details/site-recovery/).



## <a name="set-up-an-azure-network"></a>Konfiguracja sieci platformy Azure

- Skonfiguruj sieć platformy Azure. Maszyny wirtualne platformy Azure są umieszczane w tej sieci, gdy są tworzone po pracy awaryjnej.
- Usługi Site Recovery w portalu Azure mogą być używane w [Resource Manager](../resource-manager-deployment-model.md), lub w trybie klasycznym.
- Sieć powinna znajdować się w tym samym regionie co magazyn usługi Recovery Services.
- Dowiedz się więcej o [cennik sieci wirtualnej](https://azure.microsoft.com/pricing/details/virtual-network/).
- Dowiedz się więcej o [łączności maszyny Wirtualnej Azure](physical-walkthrough-network.md) po pracy awaryjnej.


## <a name="set-up-an-azure-storage-account"></a>Konfigurowanie konta usługi Azure Storage

- Usługa Site Recovery replikuje serwerów lokalnych do magazynu Azure. Maszyny wirtualne platformy Azure są tworzone z magazynu po pracy awaryjnej.
- Konfigurowanie [konto magazynu Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account) dla replikowanych danych.
- Usługi Site Recovery w portalu Azure można używać kont magazynu w Menedżerze zasobów lub w trybie klasycznym.
- Konto magazynu może być standard lub [premium](../storage/common/storage-premium-storage.md).
- Skonfigurowanie konta premium, należy również dodatkowe konta standardowego dla danych dziennika.


## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 6: Konfigurowanie magazynu](physical-walkthrough-create-vault.md)
