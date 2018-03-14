---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: virtual-machines-windows
author: dlepow
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 03/01/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 506c2a4cf675a347dc4c45c9ccf8bce95de2f6fc
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/13/2018
---
## <a name="supported-operating-systems-and-drivers"></a>Obsługiwane systemy operacyjne i sterowniki

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-tesla-drivers"></a>NC, NCv2, NCv3 i serii ND - NVIDIA tesla — sterowniki

| System operacyjny | Sterownik |
| -------- |------------- |
| Windows Server 2016 | [385.54](http://us.download.nvidia.com/Windows/Quadro_Certified/385.54/385.54-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [385.54](http://us.download.nvidia.com/Windows/Quadro_Certified/385.54/385.54-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |

> [!NOTE]
> Linki pobierania sterownika Tesla są aktualne w momencie publikacji. Aby uzyskać najnowsze sterowniki, odwiedź witrynę internetową firmy [NVIDIA](http://www.nvidia.com/).
>

### <a name="nv-series---nvidia-grid-drivers"></a>Serii Trwałym - sterowniki NVIDIA siatki

| System operacyjny | Sterownik |
| -------- |------------- |
| Windows Server 2016 | [Siatka 5.2 (386.09)](https://go.microsoft.com/fwlink/?linkid=836843) (.exe) |
| Windows Server 2012 R2 | [Siatka 5.2 (386.09)](https://go.microsoft.com/fwlink/?linkid=836844) (.exe)  |

> [!NOTE]
> Microsoft ponownie dystrybuuje instalatorów sterownika NVIDIA siatki dla maszyn wirtualnych z wirtualizacją sieci. Zainstaluj tylko te sterowniki siatki na maszynach wirtualnych z wirtualizacją sieci platformy Azure. Te sterowniki obejmują licencjonowania siatki wirtualnej oprogramowania procesora GPU na platformie Azure.
>