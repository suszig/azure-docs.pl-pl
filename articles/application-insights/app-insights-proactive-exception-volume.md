---
title: "Inteligentne wykrywania - nietypowy wzrost wielkości wyjątek w usłudze Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Monitorowanie wyjątków aplikacji z usługą Azure Application Insights dla nietypowe wzorce w woluminie wyjątku."
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
ms.date: 12/08/2017
ms.author: mbullwin
ms.openlocfilehash: 8030f3331a03170bb265c417a57725544bdc7d3f
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2017
---
# <a name="abnormal-rise-in-exception-volume-preview"></a>Nietypowy wzrost liczby wyjątków woluminu (wersja zapoznawcza)

Usługi Application Insights automatycznie analizuje wyjątki zgłaszane w aplikacji i może zostać wyświetlone ostrzeżenie o nietypowe wzorce w obrębie telemetrii wyjątku.

Ta funkcja wymaga specjalnych ustawień, inne niż [konfigurowanie raportowania wyjątek](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-exceptions#set-up-exception-reporting) dla aplikacji. Jest aktywna, gdy aplikacja generuje wystarczającą ilość danych telemetrycznych wyjątku.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Gdy otrzyma ten typ powiadomienia inteligentne wykrywanie?
Ten typ powiadomienia mogą uzyskać Jeśli aplikacji w ciągu dnia, wykazuje nietypowy wzrost liczby wyjątków o określonym typie, w porównaniu do linii bazowej obliczonego z ostatnich siedmiu dni.
Algorytmów uczenia maszynowego są używane do wykrywania wzrost liczby wyjątków, biorąc pod uwagę fizyczną wzrost korzystania z aplikacji.

## <a name="does-my-app-definitely-have-a-problem"></a>Aplikacja my uwielbiamy ma problem?
Nie, powiadomienia nie oznacza, że aplikacja ma ostatecznie problem. Mimo że nadmierną liczbę wyjątków zwykle wskazuje problem aplikacji, te wyjątki mogą być niegroźne i obsługiwane poprawnie przez aplikację.

## <a name="how-do-i-fix-it"></a>Jak rozwiązać ten problem?
Powiadomienia obejmują informacje diagnostyczne do obsługi w procesie diagnostyki:
1. **Klasyfikacji.** Powiadomienie zawiera liczbę użytkowników lub dotyczy liczbę żądań. Może to ułatwić można przypisać priorytet problemu.
2. **Zakres.** Problem wpływa na cały ruch lub tylko do niektórych operacji? Te informacje można uzyskać powiadomienia.
3. **Diagnozowanie.** Wykrywanie zawiera informacje o metodzie, z którego został zgłoszony wyjątek, oraz typ wyjątku. Umożliwia także powiązane elementy i raporty łączenie się dodatkowe informacje ułatwiające dalsze zdiagnozować problem.