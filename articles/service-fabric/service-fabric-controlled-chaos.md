---
title: "Wywołać Chaos w klastrach usługi sieć szkieletowa | Dokumentacja firmy Microsoft"
description: "Zarządzanie za pomocą iniekcji błędów i interfejsów API usługi analizy klastra Chaos w klastrze."
services: service-fabric
documentationcenter: .net
author: motanv
manager: anmola
editor: motanv
ms.assetid: 2bd13443-3478-4382-9a5a-1f6c6b32bfc9
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: motanv
ms.openlocfilehash: 3b3b93bc9ec5ecdcfc289e5b62e84de6aa4172ed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Wywołać kontrolowane Chaos w klastrach sieci szkieletowej usług
Dużych systemów rozproszonych jak infrastruktury chmury jest z założenia gwarantowane. Sieć szkieletowa usług Azure umożliwia deweloperom zapisu niezawodnej usługi rozproszone zawodnych infrastrukturze. Można zapisać niezawodne usługi rozproszone zawodnych infrastrukturze, deweloperzy muszą być możliwe przetestowanie stabilność swoich usług, podczas gdy podstawowej infrastruktury zawodnych przechodzi przez przejść skomplikowane stanu z powodu błędów.

[Iniekcji błędów i usługi klastrowania analizy](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (znaną również jako usługa analiza błędów) umożliwia deweloperom powodować błędy, aby przetestować swoich usług. Tak samo, jak te docelowe symulowane błędów, [ponowne uruchomienie partycji](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), ułatwiają wykonywanie typowych przejścia stanu. Jednak docelowe symulowane błędów są ukierunkowane zgodnie z definicją i w związku z tym może nie usterek przedstawiające się tylko w twardych prognozowania, długich i skomplikowane sekwencji przejścia stanu. Nieobciążonej testowania, możesz użyć Chaos.

Chaos symuluje błędów okresowych, przeplotem (bezpieczne i nieprawidłowego) w całym klastrze przez dłuższy czas. Po skonfigurowaniu Chaos częstotliwość i rodzaju błędów, można uruchomić Chaos za pomocą języka C# lub interfejs API środowiska Powershell w celu rozpoczęcia generowania błędów w klastrze i w usługach. Chaos do uruchomienia w określonym przedziale czasu (na przykład na godzinę), po którym zatrzymuje Chaos można skonfigurować automatycznie lub można wywołać interfejsu API StopChaos, (C# lub programu Powershell) można zatrzymać w dowolnym momencie.

> [!NOTE]
> W postaci bieżącego Chaos wywołuje tylko bezpiecznych błędy, co oznacza, że w przypadku braku błędów zewnętrznych utrata kworum, lub utraty danych nigdy nie występuje.
>

Po uruchomieniu Chaos tworzy różnych zdarzeń, których przechwycony stan w momencie uruchomienia. Na przykład ExecutingFaultsEvent zawiera wszystkie błędy, które Chaos podjęto decyzję o wykonanie w tym iteracji. ValidationFailedEvent zawiera szczegóły niepowodzenia weryfikacji (problemów kondycji lub stabilności) został znaleziony podczas sprawdzania poprawności klastra. Można wywołać GetChaosReport interfejsu API (C# lub programu Powershell) w celu uzyskania raportu Chaos działa. Te zdarzenia uzyskać utrwalone w [niezawodnej słownika](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), której obowiązują zasady obcięcie ustawieniem dwie konfiguracje: **MaxStoredChaosEventCount** (wartość domyślna to 25000) i **StoredActionCleanupIntervalInSeconds** (wartość domyślna to 3600). Co *StoredActionCleanupIntervalInSeconds* Chaos kontroli i wszystkie, ale najnowszej *MaxStoredChaosEventCount* zdarzenia, zostaną usunięte ze słownika wiarygodne.

## <a name="faults-induced-in-chaos"></a>Błędy powstaniu w Chaos
Chaos generuje błędy w ramach całego klastra sieci szkieletowej usług i kompresuje błędów, które są widoczne w miesięcy lub lat do kilku godzin. Kombinacja przeplotem błędów ze wskaźnikiem wysoką odporność znajduje sytuacjach wyjątkowych, które w przeciwnym razie mogą zostać pominięci. Tego ćwiczenia Chaos prowadzi do znacznej poprawy jakości kodu usługi.

Chaos wywołuje usterek z następujących kategorii:

* Uruchom ponownie węzeł
* Ponowne uruchomienie wdrożonego pakietu kodu
* Usuwanie repliki
* Uruchom ponownie repliki
* Przenieś repliki podstawowej (można konfigurować)
* Przenieś repliki pomocniczej (można konfigurować)

Chaos działa w przejść przez wiele iteracji. Każdej iteracji składa się z usterek i weryfikacji klastra w określonym okresie. Można skonfigurować czas dla klastra do utrwalania i Weryfikacja powiodła się. Jeśli błąd zostanie znaleziony w weryfikacji klastra, Chaos generuje i będzie się powtarzał ValidationFailedEvent sygnatury czasowej UTC oraz szczegóły dotyczące błędu. Rozważmy na przykład wystąpienie Chaos, który jest ustawiony na uruchomienie przez godzinę z maksymalnie trzech równoczesnych błędów. Chaos wywołuje trzy błędów, a następnie sprawdza stan klastra. Jego iterację poprzedniego kroku, dopóki nie zostanie jawnie zatrzymana za pośrednictwem interfejsu API StopChaosAsync lub jednogodzinnym przekazuje. Jeśli klaster staje się zła w dowolnym iteracji (to znaczy go nie stabilizacji w MaxClusterStabilizationTimeout przekazany w) Chaos generuje ValidationFailedEvent. To zdarzenie wskazuje, że coś niepowodzenia i może być konieczne dalsze badania.

Można pobrać błędów, które powstaniu Chaos, można użyć API GetChaosReport (programu powershell lub C#). Interfejs API pobiera następny segment Chaos raportu na podstawie token kontynuacji przekazany lub przekazany w czasie zakresie. Można określić ContinuationToken uzyskać następny segment raportu Chaos lub można określić zakres czasu za pośrednictwem StartTimeUtc i EndTimeUtc, ale nie można określić zarówno ContinuationToken, jak i zakres czasu, w tym samym wywołaniu. W przypadku więcej niż 100 zdarzeń Chaos raportu Chaos jest zwracany w segmentów gdy segment zawiera nie więcej niż 100 Chaos zdarzeń.

## <a name="important-configuration-options"></a>Opcje konfiguracji ważne
* **TimeToRun**: suma czasu uruchomionym programem Chaos przed zakończeniem pomyślnie. Można zatrzymać Chaos ma uruchomić w okresie TimeToRun za pośrednictwem interfejsu API StopChaos.

* **MaxClusterStabilizationTimeout**: maksymalną ilość czasu oczekiwania na klaster, aby stała się dobra przed utworzeniem ValidationFailedEvent. Jest to oczekiwania Aby zmniejszyć obciążenie klastra, gdy jest on odzyskiwanie. Testy wykonywane przez to:
  * Jeśli kondycja klastra jest OK
  * Jeśli kondycja usługi jest OK
  * Jeśli rozmiar zestawu replik docelowej uzyskuje się partycji usługi
  * Czy istnieją nie replik w stanie InBuild
* **MaxConcurrentFaults**: Maksymalna liczba jednoczesnych usterek, które są wywołane w każdej iteracji. Jest wyższy Chaos liczba, tym bardziej agresywną i tryb failover i kombinacji przejścia stanu, które przechodzą klastra są również bardziej złożone. 

> [!NOTE]
> Niezależnie od tego jak wysoka wartość *MaxConcurrentFaults* ma Chaos gwarantuje — w przypadku braku błędów zewnętrznych — nie utraty kworum lub utraty danych.
>

* **EnableMoveReplicaFaults**: Włącza lub wyłącza błędów, które powodują repliki podstawowej lub pomocniczej, aby przenieść. Te błędy są domyślnie wyłączone.
* **WaitTimeBetweenIterations**: ilość czasu między iteracji. Oznacza to, że czas Chaos zatrzyma się po wykonaniu round usterek i o zakończeniu odpowiednich sprawdzania poprawności kondycji klastra. Im wyższa wartość dolnej jest wskaźnikiem iniekcji średnią usterek.
* **WaitTimeBetweenFaults**: ilość czasu między dwa kolejne błędy w jednym iteracji. Im większa wartość, tym mniejszy współbieżności (lub nakładanie się między) błędów.
* **ClusterHealthPolicy**: zasad dotyczących kondycji klastra jest używany do sprawdzania poprawności kondycji klastra Between Chaos iteracji. Jeśli stan klastra jest wynikiem błędu lub sytuacji wystąpił nieoczekiwany wyjątek podczas wykonywania błąd Chaos będzie czekać przez 30 minut przed dalej — sprawdzenie kondycji - zapewnienie klastra recuperate trochę czasu.
* **Kontekst**: pary klucz wartość typu kolekcji (ciąg). Mapy może służyć do rejestrowania informacji o przebiegu Chaos. Nie może być więcej niż 100 takie pary, a każdy ciąg (klucz lub wartość) może mieć maksymalnie 4095 znaków. Ta mapa jest ustawiana przez starter milczenia Uruchom na przechowywanie w kontekście o określonym przebiegu.

## <a name="how-to-run-chaos"></a>Jak uruchomić Chaos

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.Add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
