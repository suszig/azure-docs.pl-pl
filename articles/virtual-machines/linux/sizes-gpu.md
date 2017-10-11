---
title: Rozmiar maszyny Wirtualnej systemu Linux platformy Azure - GPU | Dokumentacja firmy Microsoft
description: "Wyświetla różne procesora GPU zoptymalizowanych pod kątem dostępne dla maszyn wirtualnych systemu Linux na platformie Azure."
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
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 5c9bf89feba519147b07f2810fe4da882664e89e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="gpu-linux-vm-sizes"></a>Rozmiary maszyn wirtualnych systemu Linux procesora GPU

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

Sterownik instalacji i weryfikacja kroki opisane w artykule [N-series sterownik Instalatora dla systemu Linux](n-series-driver-setup.md).

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* Nie zaleca się instalowanie X serwera lub innych systemów, które używają sterownika nouveau na maszynach wirtualnych NC Ubuntu. Przed zainstalowaniem wersji sterowników procesora GPU NVIDIA, należy wyłączyć sterownik nouveau.  

## <a name="other-sizes"></a>Innych rozmiarach
- [Zastosowania ogólne](sizes-general.md)
- [Optymalizacja pod kątem obliczeń](sizes-compute.md)
- [Optymalizacja pod kątem pamięci](sizes-memory.md)
- [Optymalizacja pod kątem magazynu](sizes-storage.md)
- [Obliczenia o wysokiej wydajności](sizes-hpc.md)

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej na temat [jednostki (ACU) rozwiązań usługi obliczenia Azure](acu.md) ułatwia porównanie wydajności obliczeniowej różnych jednostki SKU Azure.