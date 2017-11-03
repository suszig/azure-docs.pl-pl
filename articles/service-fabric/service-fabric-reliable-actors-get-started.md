---
title: "Utwórz Twojego pierwszego mikrousługi Azure na podstawie aktora w języku C# | Dokumentacja firmy Microsoft"
description: "Ten samouczek przedstawia kroki tworzenia, debugowania i wdrażania prostego usługi aktora na podstawie przy użyciu usługi sieci szkieletowej Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d4aebe72-1551-4062-b1eb-54d83297f139
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 3f447e049ccd33c77f422e8aa703ad6646f9ffa2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="getting-started-with-reliable-actors"></a>Wprowadzenie do korzystania z Reliable Actors
> [!div class="op_single_selector"]
> * [C# w systemie Windows](service-fabric-reliable-actors-get-started.md)
> * [Java w systemie Linux](service-fabric-reliable-actors-get-started-java.md)
> 
> 

W tym artykule opisano podstawy usługi Azure Service Fabric Reliable Actors oraz przeprowadzi Cię przez proces tworzenia, debugowania i wdrażanie prostej aplikacji niezawodnego aktora w programie Visual Studio.

## <a name="installation-and-setup"></a>Instalacja i Konfiguracja
Przed rozpoczęciem upewnij się, że masz środowisko projektowe sieci szkieletowej usług na komputerze.
Jeśli musisz je skonfigurować, zobacz szczegółowe instrukcje na [jak skonfigurować środowisko projektowe](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Podstawowe pojęcia
Aby rozpocząć pracę z elementami Reliable Actors, musisz tylko poznać kilka podstawowych pojęć:

* **Usługa aktora**. Elementy Reliable Actors są spakowane w usługach Reliable Services, które można wdrożyć w infrastrukturze usługi Service Fabric. Wystąpienia aktora są aktywowane w nazwanym wystąpieniu usługi.
* **Rejestracja aktora**. Podobnie jak w przypadku usług Reliable Services, usługę Reliable Actor należy zarejestrować w środowisku uruchomieniowym usługi Service Fabric. Ponadto typ aktora należy zarejestrować w środowisku uruchomieniowym aktora.
* **Interfejs aktora**. Interfejs aktora służy do definiowania jednoznacznie określonego interfejsu publicznego aktora. W terminologii modelu usługi Reliable Actor interfejs aktora definiuje typy komunikatów, które może zrozumieć i przetworzyć aktor. Interfejs aktora jest używany przez innych aktorów i aplikacje klienckie do „wysyłania” (asynchronicznie) komunikatów do aktora. W usługi Reliable Actors można zaimplementować wiele interfejsów.
* **Klasa ActorProxy**. Klasa ActorProxy jest używana przez aplikacje klienckie do wywołania metod ujawnianych za pośrednictwem interfejsu aktora. Klasa ActorProxy udostępnia dwie ważne funkcje:
  
  * Rozpoznawanie nazw: jest w stanie zlokalizować aktora w klastrze (znaleźć węzeł klastra, w którym jest on hostowany).
  * Obsługa błędów: może ponawiać próby wywołania metody i ponownie rozpoznawać lokalizację aktora, na przykład po wystąpieniu błędu, który wymaga przeniesienia aktora do innego węzła w klastrze.

Warto wspomnieć o następujących regułach odnoszących się do interfejsów aktora:

* Metod interfejsu aktora nie można przeciążyć.
* Metody interfejsu aktora nie mogą zawierać parametrów out, ref ani parametrów opcjonalnych.
* Interfejsy ogólne nie są obsługiwane.

## <a name="create-a-new-project-in-visual-studio"></a>Utwórz nowy projekt w programie Visual Studio
Uruchom program Visual Studio 2015 lub Visual Studio 2017 jako administrator, a następnie utwórz nowy projekt aplikacji sieci szkieletowej usług:

![Narzędzia usługi sieć szkieletowa usług dla programu Visual Studio — nowy projekt][1]

W oknie dialogowym dalej możesz wybrać typ projektu, który chcesz utworzyć.

![Szablony projektu sieci szkieletowej usług][5]

Dla projektu HelloWorld Użyjmy usługi Reliable Actors sieci szkieletowej usług.

Po utworzeniu rozwiązania powinny być widoczne następujące struktury:

![Struktury projektu sieci szkieletowej usług][2]

## <a name="reliable-actors-basic-building-blocks"></a>Podstawowe bloki konstrukcyjne usługi Reliable Actors
Typowe rozwiązania Reliable Actors składa się z trzech projektów:

* **Projekt aplikacji (MyActorApplication)**. Jest to projekt, który wszystkich usług razem wdrożenia pakietów. Zawiera on *ApplicationManifest.xml* i skrypty programu PowerShell do zarządzania aplikacji.
* **Projekt interfejsu (MyActor.Interfaces)**. Jest to projekt, który zawiera definicję interfejsu aktora. W projekcie MyActor.Interfaces można zdefiniować interfejsów, które będą używane przez złośliwych użytkowników w rozwiązaniu. Interfejsów aktora można zdefiniować w każdym projekcie z żadną nazwą, jednak interfejs definiuje kontrakt aktora, który jest współużytkowany przez implementację aktora i klientów wywołujących aktora, dlatego zazwyczaj warto zdefiniować ją w zestawie, do którego jest oddzielony od implementacji aktora i może być współużytkowane przez wiele innych projektów.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **Projekt usługi aktora (MyActor)**. Jest to projekt używane do definiowania usługi sieci szkieletowej usług, który ma obsługiwać aktora. Zawiera ona wdrożenia aktora. Implementacja aktora jest klasa, która pochodzi z typu podstawowego `Actor` i implementuje interfejsy, które są zdefiniowane w projekcie MyActor.Interfaces. Klasa aktora musi także implementować konstruktora akceptującego `ActorService` wystąpienia i `ActorId` i przekazuje je do podstawy `Actor` klasy. Dzięki temu iniekcji zależności konstruktora zależności platformy.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

Usługę aktora należy zarejestrować za pomocą typu usługi w środowisku uruchomieniowym usługi Service Fabric. Aby usługa aktora uruchamiała wystąpienia aktora, w usłudze aktora należy również zarejestrować typ aktora. Metoda rejestracji `ActorRuntime` wykonuje tę pracę dla aktorów.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Jeśli masz tylko jedną definicję aktora w przypadku uruchomienia nowego projektu programu Visual Studio, rejestracji jest domyślnie włączone w kodzie, który generuje Visual Studio. W przypadku definiowania innych uczestników w usłudze, musisz dodać rejestracji aktora przy użyciu:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [!TIP]
> Środowisko uruchomieniowe usługi sieć szkieletowa podmiotów emituje niektóre [zdarzenia i liczniki wydajności powiązane z metody aktora](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Są one przydatne w Diagnostyka i monitorowanie wydajności.
> 
> 

## <a name="debugging"></a>Debugowanie
Narzędzia usługi sieć szkieletowa usług dla programu Visual Studio obsługuje debugowania na komputerze lokalnym. Aby uruchomić sesję debugowania, naciskać klawisz F5. Visual Studio kompiluje (w razie potrzeby) pakietów. Ponadto wdraża aplikację na lokalny klaster usługi Service Fabric i dołącza debuger.

Podczas procesu wdrażania możesz wyświetlić postęp w **dane wyjściowe** okna.

![Okno danych wyjściowych debugowania sieci szkieletowej usług][3]

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [wykorzystania platformy sieć szkieletowa usług Reliable Actors](service-fabric-reliable-actors-platform.md).

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
