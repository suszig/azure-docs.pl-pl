---
title: "Diagnostyka Azure Service Fabric stanowych usług Reliable Services | Dokumentacja firmy Microsoft"
description: "Funkcja diagnostyki dla stanowych usług Reliable Services w sieci szkieletowej usług Azure"
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: ae0e8f99-69ab-4d45-896d-1fa80ed45659
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: edcaaaf8f1619082b33195aedf1fb1abf32e85b1
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2017
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Funkcja diagnostyki dla stanowych usług Reliable Services
Klasa Azure Usługa sieci szkieletowej Stateful niezawodnej usługi StatefulServiceBase emituje [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) zdarzenia, które mogą służyć do debugowania usługi, zapewniają wgląd w sposób działania środowiska uruchomieniowego oraz pomóc w rozwiązywaniu problemów.

## <a name="eventsource-events"></a>Element EventSource zdarzenia
Nazwa źródła zdarzeń dla klasy Stateful niezawodnej usługi StatefulServiceBase jest "Microsoft-ServiceFabric-Services". Zdarzenia z źródłem tego zdarzenia wyświetlane w [zdarzenia diagnostyczne](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) okna, jeśli usługa jest w trakcie [debugowania w programie Visual Studio](service-fabric-debugging-your-application.md).

Narzędzia i technologie, które pomagają w zbierania i/lub wyświetlanie zdarzeń EventSource przykłady [narzędzia PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [diagnostyki Azure](../cloud-services/cloud-services-dotnet-diagnostics.md)i [Microsoft TraceEvent Library](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Zdarzenia
| Nazwa zdarzenia | Identyfikator zdarzenia | Poziom | Opis zdarzenia |
| --- | --- | --- | --- |
| StatefulRunAsyncInvocation |1 |Informacyjny |Wysyłanego po uruchomieniu zadania RunAsync usługi |
| StatefulRunAsyncCancellation |2 |Informacyjny |Podczas anulowania zadania RunAsync usługi |
| StatefulRunAsyncCompletion |3 |Informacyjny |Podczas zadania RunAsync usługi zostało zakończone. |
| StatefulRunAsyncSlowCancellation |4 |Ostrzeżenie |Podczas zadania RunAsync usługi trwa zbyt długo, aby ukończyć anulowania |
| StatefulRunAsyncFailure |5 |Błąd |Podczas zadania RunAsync usługi zgłasza wyjątek |

## <a name="interpret-events"></a>Interpretowanie zdarzenia
Zdarzenia StatefulRunAsyncInvocation, StatefulRunAsyncCompletion i StatefulRunAsyncCancellation są przydatne do usługi składnika zapisywania usługi do informacje o cyklu życia usługi, a także chronometraż podczas usługi uruchamiania, anuluje lub zakończeniu. Te informacje mogą być przydatne, gdy debugowanie problemów dotyczących usługi lub zrozumienie cyklu życia usług.

Moduły zapisujące usługi powinna zwrócić szczególną uwagę na zdarzenia StatefulRunAsyncSlowCancellation i StatefulRunAsyncFailure, ponieważ ich wskazują na problemy z usługą.

StatefulRunAsyncFailure jest emitowany zawsze, gdy zadanie RunAsync() usługi zgłasza wyjątek. Zazwyczaj wyjątek wskazuje błąd lub usterki w usłudze. Ponadto wyjątek powoduje awarię, usługi, więc zostanie przeniesiona do innego węzła. Ta operacja może być kosztowne i może opóźnić żądań przychodzących podczas przenoszenia usługi. Moduły zapisujące usługi należy ustalić przyczynę wyjątku i, jeśli to możliwe, ograniczyć jej.

StatefulRunAsyncSlowCancellation jest emitowany zawsze, gdy żądanie anulowania zadania RunAsync trwa ponad 4 sekundy. Usługi trwa zbyt długo, aby ukończyć anulowania, wpływa na możliwość szybkiego należy ponownie uruchomić w innym węźle usługi. Ten scenariusz może mieć wpływ na ogólnej dostępności usługi.

## <a name="next-steps"></a>Następne kroki
[Element EventSource dostawców w narzędzia PerfView](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
