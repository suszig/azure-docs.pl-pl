---
title: "Inteligentne wykrywania - niskiego poziomu wykorzystania zasobów chmury wykryta przez usługę Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Monitorowanie aplikacji za pomocą usługi Azure Application Insights dla niskiego poziomu wykorzystania zasobów chmury."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: mbullwin
ms.openlocfilehash: 4c852d07d771a1eef0e6c2e4ef27cd36f31d8ddd
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2017
---
# <a name="low-utilization-of-cloud-resources-preview"></a>Niskiego poziomu wykorzystania zasobów chmury (wersja zapoznawcza)

Usługi Application Insights analizuje użycie Procesora przez każde wystąpienie roli w aplikacji i automatycznie wykrywa wystąpienia przy niskim obciążeniu procesorów. Wykrywanie umożliwia zmniejszenie zasobów platformy Azure i zmniejszenie kosztów, zmniejszając liczbę wystąpień roli, który korzysta z poszczególnymi rolami lub zmniejszenie liczby ról.

Ta funkcja wymaga specjalnych ustawień, inne niż [Konfigurowanie liczników wydajności](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-performance-counters) dla aplikacji. Jest aktywna, kiedy aplikacja generuje za mało telemetrii licznika wydajności procesora CPU (% czasu procesora).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Gdy otrzyma ten typ powiadomienia inteligentne wykrywanie?
Powiadomienie o typowych występuje, gdy wiele wystąpienia roli sieć Web/proces roboczy mogą zachowywać niskiego poziomu wykorzystania Procesora.

## <a name="does-my-app-definitely-consume-too-many-resources"></a>Aplikacja my uwielbiamy korzystać zbyt wiele zasobów?
Nie, powiadomienia nie oznacza, że aplikacja ostatecznie zużywa zbyt wiele zasobów. Chociaż takie wzorce niskiego poziomu wykorzystania Procesora wskazują, czy można zmniejszyć zużycie zasobów, to zachowanie może być typowe dla określonej roli użytkownika, lub można mieć naturalne biznesowego wyjaśnienia i można zignorować. Na przykład może być, że wiele wystąpień są wymagane przez inne zasoby, takie jak pamięci i sieci, a nie procesora CPU.

## <a name="how-do-i-fix-it"></a>Jak rozwiązać ten problem?
Powiadomienia obejmują informacje diagnostyczne do obsługi w procesie diagnostyki:
1. **Klasyfikacji.** Powiadomienie zawiera role w aplikacji wykazujących niskiego poziomu wykorzystania Procesora. Może to ułatwić można przypisać priorytet problemu.
2. **Zakres.** Jak wiele ról wystawiane niskiego poziomu wykorzystania Procesora i liczbę wystąpień w każdej roli wykorzystania Procesora niski? Te informacje można uzyskać powiadomienia.
3. **Diagnozowanie.** Wykrywanie zawiera procent wykorzystania Procesora, przedstawiający użycie procesora CPU przez każde wystąpienie w czasie. Aby zdiagnozować problem, można użyć elementy pokrewne i raporty łączenie się dodatkowe informacje, takie jak percentylu użycia procesora CPU.
