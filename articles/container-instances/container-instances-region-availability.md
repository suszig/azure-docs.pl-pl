---
title: "Azure wystąpień kontenera regionu i zasobu dostępności | Dokumentacja platformy Azure"
description: "Odnajdywanie, w których regionach platformy Azure obsługuje wdrażanie wystąpień kontenera i limity czasu Procesora i pamięci dla tych wystąpień."
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
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="region-availability-for-azure-container-instances"></a>Dostępność wystąpień kontenera platformy Azure w danym regionie

Podczas udostępniania wersji zapoznawczej wystąpień kontenera platformy Azure są dostępne w następujących regionach, przy użyciu określonego procesora CPU i limity pamięci.

| Lokalizacja | System operacyjny | Procesor CPU | Pamięć (GB) |
| -------- | -- | :---: | :-----------: |
| Europa Zachodnia, zachodnie stany USA, wschodnie stany USA | Linux | 2 | 7 |
| Europa Zachodnia, zachodnie stany USA, wschodnie stany USA | Windows | 2 | 3,5 |

## <a name="resource-availability"></a>Dostępność zasobów

Wystąpień kontenera utworzone w tych granicach zasobów podlegają dostępności w ramach regionu wdrożenia. Jeśli region jest mocno obciążony, może wystąpić błąd podczas wdrażania wystąpienia.

Aby uniknąć niepowodzenia wdrożenia, spróbuj wdrażanie wystąpień z niższym Procesora i pamięci wirtualnej, lub wdrożenia w późniejszym czasie.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na rozwiązywanie problemów z wdrażaniem wystąpienia kontenera, zobacz [Rozwiązywanie problemów dotyczących wdrożenia z wystąpień kontenera Azure](container-instances-troubleshooting.md).