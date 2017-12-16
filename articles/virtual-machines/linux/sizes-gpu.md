---
title: Rozmiar maszyny Wirtualnej systemu Linux platformy Azure - GPU | Dokumentacja firmy Microsoft
description: "Wyświetla różne procesora GPU zoptymalizowanych pod kątem dostępne dla maszyn wirtualnych systemu Linux na platformie Azure. Wyświetla informacje o liczbie Vcpu, dysków z danymi i karty sieciowe, a także przepustowości przepływności i sieć magazynu rozmiarów w tej serii."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/14/2017
ms.author: jonbeck
ms.openlocfilehash: 9e8b682d8d7e409f97144d344ec58e710327df7a
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/16/2017
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>Procesor GPU zoptymalizowanych pod kątem rozmiarów maszyn wirtualnych

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

Sterownik instalacji i weryfikacja kroki opisane w artykule [N-series sterownik Instalatora dla systemu Linux](n-series-driver-setup.md).

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* Nie należy zainstalować X serwera lub innych systemów, które używają `Nouveau` sterowników na maszynach wirtualnych NC Ubuntu. Przed zainstalowaniem wersji sterowników procesora GPU NVIDIA, musisz wyłączyć `Nouveau` sterownika.  

## <a name="other-sizes"></a>Innych rozmiarach
- [Zastosowania ogólne](sizes-general.md)
- [Optymalizacja pod kątem obliczeń](sizes-compute.md)
- [Optymalizacja pod kątem pamięci](sizes-memory.md)
- [Optymalizacja pod kątem magazynu](sizes-storage.md)
- [Obliczenia o wysokiej wydajności](sizes-hpc.md)

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej na temat [jednostki (ACU) rozwiązań usługi obliczenia Azure](acu.md) ułatwia porównanie wydajności obliczeniowej różnych jednostki SKU Azure.