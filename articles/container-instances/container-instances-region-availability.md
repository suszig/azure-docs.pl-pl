---
title: "Region usługi Azure Container Instances i dostępność zasobów | Azure Docs"
description: "Dowiedz się, które regiony platformy Azure obsługują wdrażanie wystąpień kontenerów, i poznaj limity dotyczące procesora CPU oraz pamięci dla tych wystąpień."
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 2b9b1b864bbfd73383759212dd7d91f8e4941544
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="region-availability-for-azure-container-instances"></a>Dostępność regionów dla usługi Azure Container Instances

W okresie zapoznawczym usługa Azure Container Instances będzie dostępna w następujących regionach z określonymi limitami dotyczącymi procesora CPU i pamięci.

| Lokalizacja | System operacyjny | Procesor CPU | Pamięć (GB) |
| -------- | -- | :---: | :-----------: |
| Europa Zachodnia, Zachodnie stany USA, Wschodnie stany USA | Linux | 2 | 7 |
| Europa Zachodnia, Zachodnie stany USA, Wschodnie stany USA | Windows | 2 | 3,5 |

## <a name="resource-availability"></a>Dostępność zasobów

Wystąpienia kontenerów utworzone w ramach tych limitów zasobów podlegają dostępności w obrębie regionu wdrożenia. Gdy region jest mocno obciążony, wdrażanie wystąpień może zakończyć się niepowodzeniem.

Aby uniknąć niepowodzenia wdrażania, spróbuj wdrożyć wystąpienia z niższymi ustawieniami procesora CPU i pamięci lub spróbuj przeprowadzić wdrożenie w późniejszym terminie.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących rozwiązywania problemów z wdrażaniem wystąpienia kontenera, zobacz [Rozwiązywanie problemów z wdrażaniem przy użyciu usługi Azure Container Instances](container-instances-troubleshooting.md).