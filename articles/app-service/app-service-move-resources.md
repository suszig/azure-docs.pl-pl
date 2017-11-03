---
title: "Przenoszenie aplikacji sieci Web platformy Azure do innej grupy zasobów | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano scenariusze, w których można przenosić aplikacje sieci Web i aplikacji usługi z jednej grupy zasobów do innego."
services: app-service
documentationcenter: 
author: ZainRizvi
manager: erikre
editor: 
ms.assetid: 22f97607-072e-4d1f-a46f-8d500420c33c
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: zarizvi
ms.openlocfilehash: 4d5d6540dd0b51a4f172649a1fd9a2343efeceee
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/20/2017
---
# <a name="move-an-azure-web-app-to-another-resource-group"></a>Przenoszenie aplikacji sieci Web platformy Azure do innej grupy zasobów
Aplikacja sieci Web i/lub powiązane zasoby można przenieść do innej grupy zasobów w tej samej subskrypcji lub grupy zasobów w innej subskrypcji. Jest to realizowane w ramach zarządzania standardowych zasobów na platformie Azure. Aby uzyskać więcej informacji, zobacz [zasobów Azure przenieść do nowej grupy zasobów lub subskrypcji](../azure-resource-manager/resource-group-move-resources.md).

## <a name="limitations-when-moving-within-the-same-subscription"></a>Ograniczenia w przypadku przenoszenia w obrębie tej samej subskrypcji

Podczas przenoszenia aplikacji sieci Web _w ramach tej samej subskrypcji_, nie można przenieść przekazane certyfikaty SSL. Jednak aplikacji sieci Web można przenieść do nowej grupy zasobów, bez przenoszenia jego przekazano certyfikat SSL, a funkcja SSL aplikacji nadal działa. 

Jeśli chcesz przenieść certyfikatu SSL za pomocą aplikacji sieci Web, wykonaj następujące czynności:

1.  Przekazany certyfikat należy usunąć z aplikacji sieci Web.
2.  Przenoszenie aplikacji sieci Web.
3.  Przekaż certyfikat do przeniesionego aplikacji sieci Web.

## <a name="limitations-when-moving-across-subscriptions"></a>Ograniczenia w przypadku przenoszenia różnych subskrypcji

Podczas przenoszenia aplikacji sieci Web _w subskrypcjach_, obowiązują następujące ograniczenia:

- Docelowej grupy zasobów nie może mieć żadnych istniejących zasobów usługi aplikacji. Zasoby usługi App Service obejmują:
    - Web Apps
    - Plany usługi App Service
    - Przekazany lub zaimportowanych certyfikatów SSL
    - Środowiska usługi App Service
- Wszystkie zasoby usługi aplikacji w grupie zasobów, muszą zostać przeniesione razem.
- Zasoby usługi aplikacji można przenieść tylko z grupy zasobów, w której zostały pierwotnie utworzone. Jeśli zasób usługi aplikacji nie jest już w jego oryginalnej grupa zasobów, go musi zostać przeniesiona z powrotem do tego oryginalnego grupy zasobów najpierw, a następnie mogą zostać przeniesione w subskrypcjach. 
