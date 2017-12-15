---
title: "Region usługi Azure Container Instances i dostępność zasobów"
description: "Dowiedz się, które regiony platformy Azure obsługują wdrażanie wystąpień kontenerów, i poznaj limity dotyczące procesora CPU oraz pamięci dla tych wystąpień."
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: overview
ms.date: 08/31/2017
ms.author: marsma
ms.openlocfilehash: ace4eb6b284f2c1b2caeb54c1d686e68cacb1725
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
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