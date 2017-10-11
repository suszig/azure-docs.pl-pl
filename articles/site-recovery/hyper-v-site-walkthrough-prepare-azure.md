---
title: "Przygotowanie zasobów platformy Azure do replikowania maszyn wirtualnych funkcji Hyper-V (bez programu System Center VMM) na platformie Azure przy użyciu usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, co jest potrzebne w miejscu na platformie Azure przed rozpoczęciem replikacji maszyn wirtualnych funkcji Hyper-V (bez VMM) na platformie Azure przy użyciu usługi Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 28fa722c-675e-4637-98eb-7ccbf3806d69
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 1a30cadaab7e053184f0be133f1da5bfddc1fd91
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-to-azure"></a>Krok 5: Przygotowanie zasobów Azure dla replikacji funkcji Hyper-V do platformy Azure

Wykonaj instrukcje w tym artykule, aby przygotować zasobów platformy Azure, dzięki czemu można replikować maszyny wirtualne funkcji Hyper-V lokalnymi (bez programu System Center VMM) do platformy Azure przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) usługi.

Po przeczytaniu tego artykułu, post wszelkie komentarze u dołu i zadawaj pytania techniczne na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Przed rozpoczęciem

Upewnij się, że zostały przeczytane [wymagania wstępne](hyper-v-site-walkthrough-prerequisites.md)

## <a name="set-up-an-azure-account"></a>Konfigurowanie konta platformy Azure

- Pobierz [konta Microsoft Azure](http://azure.microsoft.com/).
- Możesz rozpocząć od [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
- Sprawdź, czy obsługiwane regiony usługi Site Recovery, w obszarze dotyczącą dostępności geograficznej w [szczegóły cennika usługi Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
- Dowiedz się więcej o [cenach usługi Site Recovery](site-recovery-faq.md#pricing)i uzyskać [szczegóły cennika](https://azure.microsoft.com/pricing/details/site-recovery/).


## <a name="set-up-an-azure-network"></a>Konfiguracja sieci platformy Azure

- Skonfiguruj sieć platformy Azure. Maszyny wirtualne platformy Azure są umieszczane w tej sieci, gdy są tworzone po pracy awaryjnej.
- Sieć powinna znajdować się w tym samym regionie co magazyn usług odzyskiwania
- Usługi Site Recovery w portalu Azure mogą być używane w [Resource Manager](../resource-manager-deployment-model.md), lub w trybie klasycznym.
- Zaleca się skonfigurowanie sieci przed rozpoczęciem dalszych działań. Jeśli tego nie zrobisz, konfigurację będzie trzeba przeprowadzić podczas wdrażania usługi Site Recovery.
- Dowiedz się więcej o [cennik sieci wirtualnej](https://azure.microsoft.com/pricing/details/virtual-network/).


## <a name="set-up-an-azure-storage-account"></a>Konfigurowanie konta usługi Azure Storage

- Usługa Site Recovery replikuje maszyny lokalnej do magazynu Azure. Maszyny wirtualne platformy Azure są tworzone z magazynu po pracy awaryjnej.
- Konfigurowanie standard/premium [konto magazynu Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account) do przechowywania danych replikowanych do platformy Azure.
- [Magazyn w warstwie Premium](../storage/common/storage-premium-storage.md) jest zazwyczaj używana w przypadku maszyn wirtualnych, które wymagają wysoką wydajność We/Wy i małe opóźnienia do intensywnych obciążeń we/wy hosta.
- Jeśli używasz konta Premium do przechowywania replikowanych danych, potrzebujesz też standardowego konta magazynu do przechowywania dzienników replikacji, które przechwytują zachodzące zmiany w danych lokalnych.
- W zależności od modelu zasobu, którego chcesz użyć dla nie powiodło się na maszynach wirtualnych platformy Azure, należy skonfigurować konto w [tryb usługi Resource Manager](../storage/common/storage-create-storage-account.md), lub [trybu klasycznego](../storage/common/storage-create-storage-account.md).
- Zalecamy skonfigurowanie konta magazynu przed rozpoczęciem. Jeśli tego nie zrobisz trzeba przeprowadzić podczas wdrażania usługi Site Recovery. Konta muszą być w tym samym regionie co magazyn usług odzyskiwania.
- Nie można przenieść kont magazynu używane przez usługę Site Recovery, grupy zasobów w ramach tej samej subskrypcji lub różnych subskrypcji.


## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 6: Przygotowanie funkcji Hyper-V zasobów](hyper-v-site-walkthrough-prepare-hyper-v.md)
