---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 03/01/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 22d37ca30f1319f46a52b96be1c527f6f56719ab
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/13/2018
---
## <a name="supported-distributions-and-drivers"></a>Obsługiwane dystrybucje i sterowniki

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-cuda-drivers"></a>NC, NCv2 NCv3 i serii ND - NVIDIA CUDA sterowniki
| Dystrybucja | Sterownik |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 lub 7.4<br/><br/> CentOS 7.3 lub 7.4 | NVIDIA CUDA 9.1, sterownika gałęzi R390 |

> [!IMPORTANT]
> Upewnij się, czy instalacji lub uaktualnienia do najnowszej wersji sterowników CUDA dla dystrybucji. Sterowniki starsza niż wersja R390 mogą wystąpić problemy z zaktualizowane jądra systemu Linux.
>

### <a name="nv-series---nvidia-grid-drivers"></a>Serii Trwałym - sterowniki NVIDIA siatki

| Dystrybucja | Sterownik |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3<br/><br/>Oparty na systemie CentOS 7.3 | 5.2 siatki NVIDIA, sterownika gałęzi R384|

> [!NOTE]
> Microsoft ponownie dystrybuuje instalatorów sterownika NVIDIA siatki dla maszyn wirtualnych z wirtualizacją sieci. Zainstaluj tylko te sterowniki siatki na maszynach wirtualnych z wirtualizacją sieci platformy Azure. Te sterowniki obejmują licencjonowania siatki wirtualnej oprogramowania procesora GPU na platformie Azure.
>

> [!WARNING] 
> Instalacja oprogramowania innych firm na produktach systemu Red Hat może wpłynąć na warunki wsparcia systemu Red Hat. Zobacz [artykuł bazy wiedzy systemu Red Hat](https://access.redhat.com/articles/1067).
>
