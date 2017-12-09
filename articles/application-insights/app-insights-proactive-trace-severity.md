---
title: "Inteligentne wykrywania - spadek wydajności stosunek ważność śledzenia, w usłudze Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Monitorowanie śladów aplikacji z usługą Azure Application Insights dla nietypowe wzorce w danych telemetrycznych śledzenia."
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
ms.date: 11/27/2017
ms.author: mbullwin
ms.openlocfilehash: 29ed195dadb7230df425d6d981a0a482e09ee79f
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2017
---
# <a name="degradation-in-trace-severity-ratio-preview"></a>Spadek wydajności stosunek ważność śledzenia (wersja zapoznawcza)

Ślady są powszechnie używane w aplikacjach, jak pomagają przedstawienie historii elementu co się stanie w tle. W przypadku wystąpienia, ślady zawierają kluczowe wgląd w sekwencji zdarzeń, co może prowadzić do stanu niepożądane. Ślady są zazwyczaj niestrukturalnych, istnieje jeden element, który może być rozpoznawane konkretnie z nich — ich poziom ważności. W stanie stabilności aplikacji, czy oczekujemy proporcje śladów "dobrej" (*informacji* i *pełne*) i ślady "zły" (*ostrzeżenie*, *błąd* i *krytyczny*) pozostaną niezmienione. Zakłada się, że śladów "zły" może się zdarzyć, regularnie w pewnym zakresie z powodu różnych sytuacjach (na przykład w sieci wystawia). Jednak po rozpoczęciu rzeczywistych problemów rośnie zwykle manifesty jako wzrost udział śladów "dobrej" vs śladów "zły". Wykrywanie inteligentny wgląd aplikacji automatycznie analizuje ślady zarejestrowane przez aplikację i może zostać wyświetlone ostrzeżenie o nietypowe wzorce w ważność telemetrii śledzenia.

Ta funkcja wymaga specjalnych ustawień, niż Konfigurowanie rejestrowania śledzenia dla aplikacji (zobacz Konfigurowanie odbiornika dziennika śledzenia dla [.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-trace-logs) lub [Java](https://docs.microsoft.com/azure/application-insights/app-insights-java-trace-logs)). Jest aktywna, gdy aplikacja generuje wystarczającą ilość danych telemetrycznych wyjątku.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Gdy otrzyma ten typ powiadomienia inteligentne wykrywanie?
Może zostać wyświetlony ten typ powiadomienia, jeśli proporcje śladów "dobrej" (ślady zarejestrowane na poziomie *informacji* lub *pełne*) i ślady "zły" (ślady zarejestrowane na poziomie *ostrzeżenie*, * Błąd, lub *błąd krytyczny*) jest pogorszenia jakości związku w określonym dniu, w porównaniu do linii bazowej obliczonego z ostatnich siedmiu dni.

## <a name="does-my-app-definitely-have-a-problem"></a>Aplikacja my uwielbiamy ma problem?
Nie, powiadomienia nie oznacza, że aplikacja ma ostatecznie problem. Mimo że spadek proporcje "dobrej" i "zły" ślady może oznaczać problem aplikacji, ta zmiana współczynnika może być niegroźne. Na przykład zwiększenie może być spowodowane nowego przepływu w aplikację emitowanie więcej śladów "zły" niż przepływów).

## <a name="how-do-i-fix-it"></a>Jak rozwiązać ten problem?
Powiadomienia obejmują informacje diagnostyczne do obsługi w procesie diagnostyki:
1. **Klasyfikacji.** Powiadomienia pokazuje, ile operacji dotyczy problem. Może to ułatwić można przypisać priorytet problemu.
2. **Zakres.** Problem wpływa na cały ruch lub tylko do niektórych operacji? Te informacje można uzyskać powiadomienia.
3. **Diagnozowanie.** Można używać elementów pokrewnych i raporty łączenie się dodatkowe informacje ułatwiające dalsze zdiagnozować problem.


