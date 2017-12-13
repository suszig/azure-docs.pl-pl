---
title: "Inteligentne wykrywania - potencjalny wyciek pamięci wykryta przez usługę Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Monitorowanie aplikacji za pomocą usługi Azure Application Insights dla potencjalnych przecieki pamięci."
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
ms.openlocfilehash: 452d0a9d0231e54df2a7f1df76c3c2c0fcd94d87
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2017
---
# <a name="memory-leak-detection-preview"></a>Wykrywanie przecieków pamięci (wersja zapoznawcza)

Usługi Application Insights automatycznie analizuje zmniejszenie zużycia pamięci każdego procesu w aplikacji i może zostać wyświetlone ostrzeżenie o potencjalnych przecieki pamięci lub zużycie pamięci.

Ta funkcja wymaga specjalnych ustawień, inne niż [Konfigurowanie liczników wydajności](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-performance-counters) dla aplikacji. Jest aktywna, kiedy aplikacja generuje telemetrii liczniki wydajności za mało pamięci (na przykład Bajty prywatne).


## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Gdy otrzyma ten typ powiadomienia inteligentne wykrywanie?
Typowy powiadomienie będzie śledzić spójne wzrost zużycia pamięci przez dłuższy czas (w ciągu kilku godzin) w co najmniej jednego procesu i/lub co najmniej jednej maszyny, które są częścią aplikacji.
Algorytmów uczenia maszynowego są używane do wykrywania zużycia pamięci, która jest zgodna z wzorcem przeciek pamięci, w przeciwieństwie do zużycia pamięci z powodu naturalnie zwiększenie użycia aplikacji.

## <a name="does-my-app-definitely-have-a-problem"></a>Aplikacja my uwielbiamy ma problem?
Nie, powiadomienia nie oznacza, że aplikacja ma ostatecznie problem. Mimo że wzorce przeciek pamięci zazwyczaj wskazują problem aplikacji, te wzorce może mieć typowe dla określonego procesu, lub mogą mieć naturalne biznesowego wyjaśnienia i można zignorować.

## <a name="how-do-i-fix-it"></a>Jak rozwiązać ten problem?
Powiadomienia obejmują informacje diagnostyczne do obsługi w procesie diagnostyczne analizy:
1. **Klasyfikacji.** Powiadomienie zawiera można zwiększyć ilość pamięci (w GB), a zakres czasu, w którym wzrosło pamięć. Może to ułatwić można przypisać priorytet problemu.
2. **Zakres.** Liczbę maszyn wystawiane wzorzec przeciek pamięci? Jak wiele wyjątków wyzwolone podczas potencjalny wyciek pamięci? Te informacje można uzyskać powiadomienia.
3. **Diagnozowanie.** Wykrywanie zawiera wzorzec przeciek pamięci, przedstawiający zużycie pamięci procesu w czasie. Umożliwia także powiązane elementy i raporty łączenie się dodatkowe informacje ułatwiające dalsze zdiagnozować problem.
