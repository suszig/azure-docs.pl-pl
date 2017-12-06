---
title: "Azure cele wydajności i skalowalności plików | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej o obiekty docelowe skalowalności i wydajności dla plików Azure, w tym pojemności, współczynnik żądań i limity przepustowości dla ruchu przychodzącego i wychodzącego."
services: storage
documentationcenter: na
author: wmgries
manager: klaasl
editor: tamram
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 12/04/2017
ms.author: wgries
ms.openlocfilehash: 381e96a0a777415b916e4093fe55aa0d355782a1
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="azure-files-scalability-and-performance-targets"></a>Azure cele wydajności i skalowalności plików
[Usługa pliki Azure](storage-files-introduction.md) oferuje pełni zarządzanych udziałów plików w chmurze, które są dostępne przy użyciu standardowego protokołu SMB. W tym artykule omówiono cele skalowalności i wydajności dla plików Azure i synchronizacji plików Azure (wersja zapoznawcza).

Obiekty docelowe skalowalności i wydajności wymienione w tym miejscu wysokiej klasy elementów docelowych, ale może wpłynąć na inne zmienne w danym wdrożeniu. Na przykład przepływność dla pliku może również być ograniczone przez dostępną przepustowość sieci, nie tylko serwery obsługujące usługi pliki Azure. Zdecydowanie zaleca się testowanie z wzorca użycia w celu określenia, czy skalowalność i wydajność usługi pliki Azure zgodnie z wymaganiami. Możemy również dużą wagę do zwiększenia tych limitów wraz z upływem czasu. Należy natychmiast Prześlij nam swoją opinię, albo w komentarzach poniżej lub na [UserVoice plików Azure](https://feedback.azure.com/forums/217298-storage/category/180670-files), dotyczących ograniczeń, które chcesz zobaczyć się z nami zwiększyć.

## <a name="azure-storage-account-scale-targets"></a>Obiekty docelowe skalowania konta magazynu platformy Azure
Zasobu nadrzędnego do udziału plików Azure to konto magazynu platformy Azure. Konto magazynu reprezentuje puli magazynu na platformie Azure, który może służyć przez wiele usług magazynu, w tym pliki Azure do przechowywania danych. Inne usługi, które przechowują dane na kontach magazynu są magazynu obiektów Blob platformy Azure, magazyn kolejek Azure i magazynem tabel Azure. Następujące elementy docelowe mają zastosowanie wszystkie usługi Magazyn przechowywania danych na koncie magazynu:

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Użycie konta magazynu z innych usług magazynu ma wpływ na Twoje udziały plików platformy Azure na koncie magazynu. Na przykład przekroczenie maksymalnej pojemności konta z magazynu obiektów Blob platformy Azure, nie będzie można tworzyć nowe pliki w udziale plików platformy Azure, nawet jeśli udział plików Azure będzie poniżej rozmiar maksymalny udziału.

## <a name="azure-files-scale-targets"></a>Cele plików Azure w skali
[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Azure cele skali synchronizacji plików
Z opcją synchronizacji plików Azure ma próbowaliśmy możliwie często projektować pod kątem nieograniczonego użycia, jednak nie jest to zawsze możliwe. Poniżej tabeli wskazuje granice testów i cele, które są rzeczywiście stałych limitów:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="see-also"></a>Zobacz też
- [Planowanie wdrożenia usługi pliki Azure](storage-files-planning.md)
- [Planowanie wdrożenia synchronizacji plików Azure](storage-sync-files-planning.md)
- [Obiekty docelowe skalowalności i wydajności dla innych usług magazynu](../common/storage-scalability-targets.md)