---
title: "Zaawansowane użycia niezawodne usługi | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat zaawansowanych funkcji usługi sieć szkieletowa usług niezawodnej dodano elastyczność w usługach."
services: Service-Fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: masnider
ms.assetid: f2942871-863d-47c3-b14a-7cdad9a742c7
ms.service: Service-Fabric
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 694d75807d978ece6296b945bf348f08688d3b5d
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="advanced-usage-of-the-reliable-services-programming-model"></a>Zaawansowane użycia niezawodnej model programowania usług
Sieć szkieletowa usług Azure ułatwia pisanie usług oraz zarządzania nimi niezawodnej bezstanowe i stanowe. Ten przewodnik zawiera informacje o zaawansowanych użycia niezawodnych usług, aby uzyskać więcej kontrolę i elastyczność za pośrednictwem usługi. Przed odczytaniem tego przewodnika, zapoznaj się z [modelu programowania usług niezawodnej](service-fabric-reliable-services-introduction.md).

Zarówno stanowe i bezstanowe usługi mają dwa punkty wejścia głównej dla kodu użytkownika:

* `RunAsync(C#) / runAsync(Java)`jest punktem wejścia ogólnego przeznaczenia dla kodu usługi.
* `CreateServiceReplicaListeners(C#)`i `CreateServiceInstanceListeners(C#) / createServiceInstanceListeners(Java)` jest otwierania odbiorników komunikacji do obsługi żądań klientów.

W przypadku większości usług te punkty wejścia dwóch są wystarczające. W rzadkich przypadkach większą kontrolę nad cyklem życia usługi jest wymagane, zdarzenia cyklu życia dodatkowe są dostępne.

## <a name="stateless-service-instance-lifecycle"></a>Cykl życia wystąpienia usługi bezstanowej
Cykl życia usługi bezstanowej jest bardzo prosty. Usługi bezstanowej tylko można otworzyć, zamknięta lub zostało przerwane. `RunAsync`usługi bezstanowej jest wykonywane, gdy wystąpienie usługi jest otwarta i anulowany, gdy wystąpienie usługi jest zamknięty lub zostało przerwane.

Mimo że `RunAsync` powinny być wystarczające w niemal wszystkich przypadkach, otwarte, zamknij i przerwania zdarzeń usługi bezstanowej są także dostępne:

* `Task OnOpenAsync(IStatelessServicePartition, CancellationToken) - C# / CompletableFuture<String> onOpenAsync(CancellationToken) - Java`OnOpenAsync jest wywoływane, gdy wystąpienie usługi bezstanowej ma być używany. W tym momencie można uruchomić zadania inicjowania usługi rozszerzonej.
* `Task OnCloseAsync(CancellationToken) - C# / CompletableFuture onCloseAsync(CancellationToken) - Java`OnCloseAsync jest wywoływane, gdy wystąpienie usługi bezstanowej będzie można bezpiecznie zamknąć. Taka sytuacja może wystąpić, gdy kodu usługi jest uaktualniany, wystąpienie usługi jest przenoszony z powodu równoważenia obciążenia lub wykrycia błędu przejściowego. OnCloseAsync można bezpiecznie zamknąć wszystkie zasoby, Zatrzymaj przetwarzanie w tle, Zakończ zapisywanie stanu zewnętrznych lub zamknięcia w istniejących połączeń.
* `void OnAbort() - C# / void onAbort() - Java`OnAbort jest wywoływane, gdy wystąpienie usługi bezstanowej trwa wymuszone zamykanie. Zazwyczaj jest to nazywane po wykryciu trwałego błędu w węźle lub sieci szkieletowej usług nie można niezawodnie zarządzania cyklem życia wystąpienia usługi z powodu błędów wewnętrznych.

## <a name="stateful-service-replica-lifecycle"></a>Cykl życia repliki usługi stanowej

> [!NOTE]
> Niezawodne usługi stanowej nie są obsługiwane w języku Java jeszcze.
>
>

Cykl życia repliki usługi stanowej jest znacznie bardziej skomplikowane niż wystąpienie usługi bezstanowej. Ponadto aby otworzyć, zamknij i abort zdarzenia, repliki usługi stanowej podlega zmianom roli przez jego okres istnienia. Zmiany roli repliki usługi stanowej `OnChangeRoleAsync` zdarzenie zostanie wyzwolone:

* `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`OnChangeRoleAsync jest wywoływane, gdy replika usługi stanowej jest zmiana roli, na przykład na podstawowy lub pomocniczy. Replik podstawowych podano stanu zapisu (dozwolone są do tworzenia i zapisywania niezawodnej kolekcji). Replik pomocniczych podano stanu (tylko można odczytać z istniejących zbiorach niezawodnej). Większość pracy w usługi stanowej odbywa się repliką podstawową. Replik pomocniczych można wykonać walidacji tylko do odczytu, generowanie raportów, wyszukiwania danych lub innych zadań tylko do odczytu.

W usługi stanowej tylko replika podstawowa ma dostęp do zapisu do stanu i dlatego zazwyczaj gdy usługa wykonuje faktyczną pracę. `RunAsync` Metoda w usługi stanowej jest wykonywana tylko wtedy, gdy replika usługi stanowej jest kluczem podstawowym. `RunAsync` Metody jest anulowany, gdy roli repliki podstawowej zmiany od podstawowego, a także podczas zdarzeń zamknięcia i abort.

Przy użyciu `OnChangeRoleAsync` zdarzeń służy do wykonywania pracy, w zależności od roli repliki, jak również w odpowiedzi na zmiany roli.

Usługa stanowa zapewnia również tego samego zdarzenia cyklu życia cztery jako usługę bezstanową z tej samej semantyki i przypadki użycia:

```csharp
* Task OnOpenAsync(IStatefulServicePartition, CancellationToken)
* Task OnCloseAsync(CancellationToken)
* void OnAbort()
```

## <a name="next-steps"></a>Następne kroki
Aby uzyskać bardziej zaawansowanych tematów dotyczących usługi Service Fabric zobacz następujące artykuły:

* [Konfigurowanie stanowe niezawodne usługi](service-fabric-reliable-services-configuration.md)
* [Wprowadzenie kondycji sieci szkieletowej usług](service-fabric-health-introduction.md)
* [Korzystanie z raportów kondycji systemu do rozwiązywania problemów](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Konfigurowanie usług z usług sieci szkieletowej klastra Menedżera zasobów](service-fabric-cluster-resource-manager-configure-services.md)
