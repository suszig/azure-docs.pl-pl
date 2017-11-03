---
title: "Cele dotyczące wydajności i skalowalności magazynu platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej o obiekty docelowe skalowalności i wydajności usługi Azure Storage, w tym pojemności, współczynnik żądań i przepustowości ruchu przychodzącego i wychodzącego dla obu kont magazynu standard i premium. Zrozumienie celów wydajności dla partycji w ramach każdej z usług Azure Storage."
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: be721bd3-159f-40a1-88c1-96418537fe75
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 07/12/2017
ms.author: tamram
ms.openlocfilehash: 805b0eee46846345ee1f33faf0c28393c3e8ebb1
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="azure-storage-scalability-and-performance-targets"></a>Cele usługi Azure Storage dotyczące skalowalności i wydajności
## <a name="overview"></a>Omówienie
W tym temacie opisano tematy dotyczące skalowalności i wydajności dla magazynu Microsoft Azure. Podsumowanie innych limitów Azure, zobacz [subskrypcji platformy Azure i ograniczenia usługi, przydziały i ograniczenia](../../azure-subscription-service-limits.md).

> [!NOTE]
> Wszystkie konta magazynu Uruchom na nową topologię siecią płaską i obsługuje obiekty docelowe skalowalności i wydajności, opisane poniżej, niezależnie od tego, kiedy zostały utworzone. Aby uzyskać więcej informacji na komputerze o architekturze siecią płaską usługi Azure Storage i skalowalność, zobacz [Microsoft Azure Storage: A wysoce dostępna usługa magazynu w chmurze with Strong Consistency](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).
> 
> [!IMPORTANT]
> Obiekty docelowe skalowalności i wydajności wymienione w tym miejscu wysokiej klasy elementów docelowych, ale są osiągalne. Wszystkich przypadkach, współczynnik żądań i przepustowości magazynu konta zależy od rozmiaru obiekty przechowywane, wzorce dostępu do wykorzystania, i wykonuje polecenie Typ obciążenia aplikacji. Należy przetestować usługą w celu ustalenia, czy jego wydajność, spełnia wymagania. Jeśli to możliwe zapobiec nagłym wzrostów w polu szybkość ruchu i sprawdź, czy ruch jest dobrze rozproszone na partycje.
> 
> Gdy aplikacja osiągnie limit co partycji może obsłużyć dla obciążenia, usługi Azure Storage zaczną zwracać kod błędu 503 (serwer jest zajęty) lub kod błędu 500 odpowiedzi (limit czasu operacji). W takim przypadku aplikacji należy używać zasad wykładniczego wycofywania dla ponownych prób. Wykładniczego wycofywania umożliwia obciążenia na partycji zmniejszają i ułatwić limit największego ruchu do tej partycji.
> 
> 

Wymagania aplikacji przekracza wartości docelowe skalowalności konta jednego magazynu, można skompilować aplikację do korzystania z wielu kont magazynu i partycji obiektów danych przez tych kont magazynu. Zobacz [cennik usługi Azure Storage](https://azure.microsoft.com/pricing/details/storage/) informacje dotyczące cennika woluminów.

## <a name="scalability-targets-for-a-storage-account"></a>Wartości docelowe skalowalności dla konta magazynu
[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Azure cele skalowania magazynu obiektów Blob
[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Cele plików Azure w skali
Aby uzyskać więcej informacji na skalowalność i wydajność elementy docelowe dla plików Azure i synchronizacji plików Azure, zobacz [elementy docelowe skalowalności i wydajności usługi pliki Azure](../files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Azure cele skali synchronizacji plików
[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="azure-queue-storage-scale-targets"></a>Skala magazynu Azure kolejki elementów docelowych
[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

## <a name="azure-table-storage-scale-targets"></a>Skala magazynu Azure tabeli obiektów docelowych
[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
## <a name="scalability-targets-for-virtual-machine-disks"></a>Wartości docelowe skalowalności dysków maszyny wirtualnej
[!INCLUDE [azure-storage-limits-vm-disks](../../../includes/azure-storage-limits-vm-disks.md)]

Zobacz [rozmiarów maszyn wirtualnych systemu Windows](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) lub [rozmiarów maszyn wirtualnych systemu Linux](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) dodatkowe szczegóły.

## <a name="managed-virtual-machine-disks"></a>Dyski zarządzane maszyny wirtualnej

[!INCLUDE [azure-storage-limits-vm-disks-managed](../../../includes/azure-storage-limits-vm-disks-managed.md)]

## <a name="unmanaged-virtual-machine-disks"></a>Dyski maszyny wirtualnej niezarządzanej
[!INCLUDE [azure-storage-limits-vm-disks-standard](../../../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../../../includes/azure-storage-limits-vm-disks-premium.md)]

## <a name="see-also"></a>Zobacz też
* [Szczegóły cennika magazynu](https://azure.microsoft.com/pricing/details/storage/)
* [Subskrypcji platformy Azure i usługi limity, przydziały i ograniczenia](../../azure-subscription-service-limits.md)
* [Premium Storage: magazyn o wysokiej wydajności dla obciążeń maszyn wirtualnych platformy Azure](../../virtual-machines/windows/premium-storage.md)
* [Replikacja usługi Azure Storage](../storage-redundancy.md)
* [Wydajność magazynu Microsoft Azure i listę kontrolną skalowalności](../storage-performance-checklist.md)
* [Microsoft Azure Storage: Wysokiej dostępności magazynu usługi w chmurze o wysoki poziom spójności](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

