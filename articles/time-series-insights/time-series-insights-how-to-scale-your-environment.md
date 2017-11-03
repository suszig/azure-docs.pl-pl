---
title: "Jak skalować środowiska Azure czas serii Insights | Dokumentacja firmy Microsoft"
description: "W tym samouczku opisano sposób skalowania środowiska Azure czas serii Insights"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: ba6bd1ab05bb7e24dd1bc307218e7a772fbde601
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>Jak skalować środowiska Insights serii czasu

W tym samouczku uwzględniono również sposób skalowania środowiska Insights serii czasu.

> [!NOTE]
> Skalowania w górę różnych typów jednostki sku nie jest dozwolone. Nie można przekonwertować w środowisku zawierającym S1 Sku środowisku S2.

## <a name="s1-sku-ingress-rates-and-capacities"></a>Szybkość wejściowych S1 SKU i pojemności

| S1 Pojemność jednostki SKU | Szybkość transferu | Maksymalna pojemność magazynu
| --- | --- | --- |
| 1 | 1 GB (1 mln zdarzeń) | 30 GB (30 milionów zdarzeń) na miesiąc |
| 10 | 10 GB (10 milionów zdarzeń) | 300 GB (300 milionów zdarzeń) na miesiąc |

## <a name="s2-sku-ingress-rates-and-capacities"></a>Szybkość wejściowych s2 jednostki SKU i pojemności

| S2 Pojemność jednostki SKU | Szybkość transferu | Maksymalna pojemność magazynu
| --- | --- | --- |
| 1 | 10 GB (10 milionów zdarzeń) | 300 GB (300 milionów zdarzeń) na miesiąc |
| 10 | 100 GB (100 milionów zdarzeń) | 3 TB (zdarzenia miliard 3) na miesiąc |

Możliwości skalowania liniowo, więc sku S1, o pojemności 2 obsługuje 2 GB (2 milionów) zdarzeń według dnia służąca szybkość i 60 GB (60 miliona zdarzeń) w miesiącu.

## <a name="changing-the-capacity-of-your-environment"></a>Zmiana wydajności środowiska

1. W portalu Azure wybierz środowisko, której pojemność chcesz zmienić.
1. W obszarze Ustawienia kliknij przycisk Konfiguruj.
1. Za pomocą suwaka pojemności wybierz wydajności, który spełnia wymagania ceny wejściowych i pojemności magazynu.

## <a name="next-steps"></a>Następne kroki

* Sprawdź, czy nowe pojemności jest wystarczające, aby zapobiec ograniczania. Aby uzyskać więcej informacji, zobacz *środowisku może być pobieranie ograniczany* sekcji [tutaj](time-series-insights-diagnose-and-solve-problems.md).