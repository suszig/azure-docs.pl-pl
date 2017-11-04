---
title: "Próbkowanie danych telemetrii w usłudze Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Jak zapewnić ilość danych telemetrycznych pod kontrolą."
services: application-insights
documentationcenter: windows
author: vgorbenko
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: mbullwin
ms.openlocfilehash: 0ed612a0e2101397fb1074de61db07d2361f8463
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="sampling-in-application-insights"></a>Próbkowanie w usłudze Application Insights


Próbkowanie to funkcja [Azure Application Insights](app-insights-overview.md). Jest zalecanym sposobem ograniczenia ruchu danych telemetrycznych i magazynu, zachowując statystycznie prawidłową analizy danych aplikacji. Filtr wybiera elementy, które są powiązane, dzięki czemu można przechodzić między elementami podczas wykonywania badań diagnostycznych.
Gdy metryki liczby są prezentowane w portalu, są one renormalized w celu uwzględnienia pobierania próbek, aby zminimalizować wpływ na statystyki.

Próbkowania powoduje zmniejszenie kosztów ruchu i dane i pomaga uniknąć ograniczania.

## <a name="in-brief"></a>Krótko mówiąc:
* Próbkowanie zachowuje 1 w  *n*  rejestruje i odrzuca wszystkie pozostałe. Na przykład mogą go zachować zdarzenia 1 do 5, częstotliwość próbkowania, 20%. 
* Próbkowanie odbywa się automatycznie, jeśli aplikacja wyśle dużej ilości danych telemetrii, w aplikacji serwera sieci web ASP.NET.
* Można również ustawić próbkowania ręcznie, albo w portalu na stronie cen; lub w zestawie SDK platformy ASP.NET w pliku .config, aby również zmniejszenie ruchu w sieci.
* Jeśli dziennika zdarzeń niestandardowych, należy się upewnić, że zestaw zdarzeń jest zatrzymany lub odrzucone razem upewnij się, że mają one taką samą wartość OperationId.
* Dzielnik próbkowania  *n*  jest zgłaszany we wszystkich rekordach we właściwości `itemCount`, w wyszukiwaniu widocznego pod przyjazną nazwą "liczbę żądań" lub "liczba zdarzeń". Podczas pobierania próbek nie jest w operacji `itemCount==1`.
* Jeśli piszesz zapytania analityczne, [uwzględnienia próbkowania](app-insights-analytics-tour.md#counting-sampled-data). W szczególności, zamiast po prostu zliczanie rekordów, należy użyć `summarize sum(itemCount)`.

## <a name="types-of-sampling"></a>Typy pobierania próbek
Istnieją trzy metody alternatywne próbkowania:

* **Próbkowanie adaptacyjną** automatycznie dostosowuje ilość danych telemetrycznych wysłanych z zestawu SDK w aplikacji ASP.NET. Domyślnie z 2.0.0-beta3 v zestawu SDK. Obecnie dostępne dla platformy ASP.NET tylko telemetrii po stronie serwera. 
* **Próbkowanie stałej stawki** zmniejsza ilość danych telemetrycznych wysłanych z serwera programu ASP.NET oraz z przeglądarki użytkownika. Możesz ustawić częstotliwość. Klient i serwer będzie synchronizować ich próbkowania tak, w wyszukiwania, można przechodzić między wyświetleń strony powiązane i żądań.
* **Wprowadzanie próbkowania** działa w portalu Azure. Odrzuca niektóre dane telemetryczne, przychodzący z aplikacji, z szybkością ustawieniu. Nie zmniejszenie ruchu telemetrii, ale pomaga zachować w wykorzystaniu całego przydziału miesięcznego. Duży zaletą wprowadzanie próbkowania jest bez ponownego wdrażania aplikacji można ją ustawić, czy działa on jednakowo do wszystkich serwerów i klientów. 

W przypadku adaptacyjną lub stała częstotliwość próbkowania w operacji, wprowadzanie próbek jest wyłączone.

## <a name="ingestion-sampling"></a>Wprowadzanie próbkowania
Ta forma pobierania próbek działa w momencie, gdy dane telemetryczne z serwera sieci web, przeglądarki i urządzenia osiąga punkt końcowy usługi Application Insights. Chociaż nie redukują ruchu danych telemetrycznych wysłanych z aplikacji, Zmniejsz rozmiar przetwarzane i przechowywane (i naliczona opłata za) przez usługę Application Insights.

Użyj tego typu próbkowania, jeśli aplikacja często przechodzi przez jego przydział miesięczny, a nie ma możliwości użycia jednej z tego zestawu SDK typu próbkowania. 

Ustaw częstotliwość próbkowania w przydziały i cenach bloku:

![W bloku Omówienie aplikacji kliknij ustawienia, przydział, próbek, a następnie wybierz częstotliwość próbkowania, a następnie kliknij przycisk Aktualizuj.](./media/app-insights-sampling/04.png)

Podobnie jak inne rodzaje próbkowania algorytm zachowuje elementy powiązane dane telemetryczne. Na przykład gdy jest sprawdzanie telemetrii w wyszukiwania, będziesz mieć możliwość odnaleźć żądania dotyczące określonego wyjątku. Metryka liczby takich jak liczby żądań i szybkość wyjątek poprawnie zostaną zachowane.

Punkty danych, które są odrzucane przez pobieranie próbek nie są dostępne w dowolnej funkcji usługi Application Insights, takich jak [eksportu ciągłego](app-insights-export-telemetry.md).

Wprowadzanie próbkowania nie działa podczas operacji SDK próbkowania adaptacyjną lub stałej stawki. Jeśli częstotliwość próbkowania w zestawie SDK jest mniejsza niż 100%, częstotliwość próbkowania wprowadzanie ustawiona jest ignorowana.

> [!WARNING]
> Wartości wyświetlane na kafelku określa wartość, która zostanie ustawiona wprowadzanie próbkowania. Jeśli jest używany w operacji pobierania zestawu SDK nie reprezentuje rzeczywistego próbkowania.
> 
> 

## <a name="adaptive-sampling-at-your-web-server"></a>Adaptacyjną próbkowania na serwerze sieci web
Adaptacyjną próbkowania jest dostępna dla aplikacji Insights zestawu SDK dla platformy ASP.NET v 2.0.0-beta3 i nowszych i jest domyślnie włączona. 

Próbkowanie adaptacyjną wpływa na ilość danych telemetrycznych wysłanych z aplikacji serwera sieci web usługi Application Insights. Wolumin zostanie automatycznie dopasowana do pozostać w określonym maksymalna szybkość ruchu.

Go nie działa niewielki dane telemetryczne, więc w debugowaniu aplikacji lub witryny sieci Web z użyciem niski nie będzie to mieć wpływ na.

Uzyskanie wolumin docelowy, niektóre dane telemetryczne, generowane jest pomijany. Zachowując podobnie jak inne rodzaje próbkowania algorytm elementy powiązane dane telemetryczne. Na przykład gdy jest sprawdzanie telemetrii w wyszukiwania, będziesz mieć możliwość odnaleźć żądania dotyczące określonego wyjątku. 

Metryka liczby takich jak liczby żądań i szybkość wyjątek są dostosowane do kompensacji próbkowania, tak aby pokazywały około poprawne wartości w Eksploratorze metryki.

**Aktualizacja projektu NuGet** pakietów do najnowszej wersji *wersji wstępnej* wersji usługi Application Insights: kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, wybierz polecenie Zarządzaj pakietami NuGet Sprawdź **Include wstępna** i wyszukaj Microsoft.ApplicationInsights.Web. 

W [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), można dostosować kilku parametrów w `AdaptiveSamplingTelemetryProcessor` węzła. Dane pokazywane są wartościami domyślnymi:

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`
  
    Docelowy szybkość, z której ma adaptacyjną algorytmu dla **na każdym hoście serwera**. Jeśli aplikacja sieci web jest uruchomiony na wielu hostach, zmniejsz tę wartość tak, aby pozostają w szybkość docelowy ruchu w portalu usługi Application Insights.
* `<EvaluationInterval>00:00:15</EvaluationInterval>` 
  
    Interwał, jaką jest ponownie oceniane bieżącej szybkości telemetrii. Ocena jest wykonywane średniej ruchomej. Możesz skrócić ten interwał, jeśli seria nagłym odpowiada telemetrii.
* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`
  
    Podczas pobierania próbek zmiany wartość procentową, jak najszybciej po firma Microsoft mogą obniżyć wartości procentowej pobierania próbek ponownie, aby przechwycić mniejszej ilości danych.
* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`
  
    Podczas pobierania próbek zmiany wartości procent, jak najszybciej po firma Microsoft mogą zwiększyć wartości procentowej pobierania próbek ponownie, aby przechwycić większej ilości danych.
* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`
  
    Jako wartość procentowa próbek różni się co to jest minimalny, które firma Microsoft mogła ustawić.
* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`
  
    Jako wartość procentowa próbek różni się co to jest wartość maksymalna, które firma Microsoft mogła ustawić.
* `<MovingAverageRatio>0.25</MovingAverageRatio>` 
  
    Podczas obliczania średniej ruchomej wagi przypisane do najbardziej aktualną wartość. Użyj wartości równa lub mniejsza niż 1. Mniejsze wartości zmiany algorytm mniej reaguje na gwałtowny.
* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`
  
    Wartość przypisana, gdy aplikacja właśnie zostało uruchomione. Nie zmniejsza to podczas debugowania kodu. 

* `<ExcludedTypes>Trace;Exception</ExcludedTypes>`
  
    Rozdzielany średnikami lista typów, które mają być pobrane. Rozpoznawane są typy: zależność, zdarzenia, wyjątek, widok strony, żądanie, śledzenia. Wszystkie wystąpienia określonych typów są przesyłane; typy, które nie są określone są próbkowane.

* `<IncludedTypes>Request;Dependency</IncludedTypes>`
  
    Rozdzielany średnikami lista typów, które mają być pobrane. Rozpoznawane są typy: zależność, zdarzenia, wyjątek, widok strony, żądanie, śledzenia. Próbkowanych określonych typów; wszystkie wystąpienia innych typów zawsze będą przesyłane.


**Aby wyłączyć** adaptacyjną próbkowania, Usuń węzeł AdaptiveSamplingTelemetryProcessor z applicationinsights-config.

### <a name="alternative-configure-adaptive-sampling-in-code"></a>Alternatywa: Konfigurowanie adaptacyjną próbkowania w kodzie
Zamiast dostosowywania próbkowania w pliku .config, możesz użyć kodu. Dzięki temu można określić funkcję wywołania zwrotnego, które jest wywoływane zawsze, gdy następuje także ponowna ocena próbkowania. Można to, na przykład, aby dowiedzieć się, jakie częstotliwość próbkowania jest używany.

Usuń `AdaptiveSamplingTelemetryProcessor` węzeł z pliku .config.

*C#*

```C#

    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.Channel.Implementation;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var adaptiveSamplingSettings = new SamplingPercentageEstimatorSettings();

    // Optional: here you can adjust the settings from their defaults.

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;

    builder.UseAdaptiveSampling(
         adaptiveSamplingSettings,

        // Callback on rate re-evaluation:
        (double afterSamplingTelemetryItemRatePerSecond,
         double currentSamplingPercentage,
         double newSamplingPercentage,
         bool isSamplingPercentageChanged,
         SamplingPercentageEstimatorSettings s
        ) =>
        {
          if (isSamplingPercentageChanged)
          {
             // Report the sampling rate.
             telemetryClient.TrackMetric("samplingPercentage", newSamplingPercentage);
          }
      });

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Dowiedz się więcej o telemetrii procesorów](app-insights-api-filtering-sampling.md#filtering).)

<a name="other-web-pages"></a>

## <a name="sampling-for-web-pages-with-javascript"></a>Próbkowania dla stron sieci web w języku JavaScript
Można skonfigurować stron sieci web do pobierania próbek stałej stawki z dowolnego serwera. 

Gdy użytkownik [skonfigurować stron sieci web dla usługi Application Insights](app-insights-javascript.md), zmodyfikować fragment kodu, który można pobrać z portalu Application Insights. (W aplikacjach ASP.NET, fragment zazwyczaj znajduje się w _Layout.cshtml.)  Wstaw wiersz, takich jak `samplingPercentage: 10,` przed klucza Instrumentacji:

    <script>
    var appInsights= ... 
    }({ 


    // Value must be 100/N where N is an integer.
    // Valid examples: 50, 25, 20, 10, 5, 1, 0.1, ...
    samplingPercentage: 10, 

    instrumentationKey:...
    }); 

    window.appInsights=appInsights; 
    appInsights.trackPageView(); 
    </script> 

W przypadku wartości procentowej pobierania próbek wybierz wartość procentową, która jest bliski 100/N, gdzie N to liczba całkowita.  Próbkowanie aktualnie nie obsługuje innych wartości.

Włączenie próbkowania stałej stawki na serwerze, klientami a serwerem będzie synchronizować tak, w wyszukiwania, można przechodzić między wyświetleń strony powiązane i żądań.

## <a name="fixed-rate-sampling-for-aspnet-web-sites"></a>Stałej częstotliwość próbkowania dla witryn sieci web ASP.NET
Stały częstotliwość próbkowania zmniejsza ruch wysyłany z serwera sieci web i przeglądarki sieci web. W przeciwieństwie do próbkowania adaptacyjną zmniejsza telemetrii według stałej stawki ustalanej określone przez użytkownika. Również synchronizacji klienta i serwera próbkowania, tak aby elementy powiązane są zachowywane — na przykład, jeśli przyjrzymy widoku strony w wyszukiwaniu mogły zostać odnalezione jego powiązanego żądania.

Algorytm próbkowania zachowuje elementy powiązane. Dla każdego żądania HTTP zdarzeń, go i jego zdarzenia powiązane są odrzucane albo przesyłane. 

W Eksploratorze metryk stawki, takich jak liczby żądań i wyjątków są pomnożona przez współczynnik kompensacji próbkowania, dzięki czemu są one poprawne około.

1. **Aktualizację pakietów NuGet projektu** do najnowszej wersji *wstępną* wersji usługi Application Insights. Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, wybierz polecenie Zarządzaj pakietami NuGet Sprawdź **Uwzględnij wersję wstępną** i wyszukaj Microsoft.ApplicationInsights.Web. 
2. **Wyłącz adaptacyjną próbkowania**: W [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), usunąć lub komentarz `AdaptiveSamplingTelemetryProcessor` węzła.
   
    ```xml
   
    <TelemetryProcessors>
   
    <!-- Disabled adaptive sampling:
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    -->

    ```

1. **Włącz moduł próbkowania stałej stawki.** Dodaj ten fragment kodu dotyczący [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):
   
    ```XML
   
    <TelemetryProcessors>
     <Add  Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
   
      <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
      <SamplingPercentage>10</SamplingPercentage>
      </Add>
    </TelemetryProcessors>
   
    ```

> [!NOTE]
> W przypadku wartości procentowej pobierania próbek wybierz wartość procentową, która jest bliski 100/N, gdzie N to liczba całkowita.  Próbkowanie aktualnie nie obsługuje innych wartości.
> 
> 

### <a name="alternative-enable-fixed-rate-sampling-in-your-server-code"></a>Alternatywa: Włącz stałej częstotliwość próbkowania w kodzie serwera
Zamiast ustawienie parametru próbkowania w pliku .config, możesz użyć kodu. 

*C#*

```C#

    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.GetTelemetryProcessorChainBuilder();
    builder.UseSampling(10.0); // percentage

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Dowiedz się więcej o telemetrii procesorów](app-insights-api-filtering-sampling.md#filtering).)

## <a name="when-to-use-sampling"></a>Kiedy należy używać próbkowania?
Adaptacyjną próbkowania jest włączane automatycznie, jeśli używasz 2.0.0-beta3 wersji zestawu SDK platformy ASP.NET lub nowszym. Niezależnie od tego, jakiego używasz wersja zestawu SDK można użyć próbkowania wprowadzanie (w naszym serwera).

Nie trzeba próbkowania dla większości aplikacji małych i średnich. Najbardziej przydatne informacje diagnostyczne i najbardziej dokładna statystyki są pobierane przez zbierania danych dotyczących wszystkich działań użytkownika. 

Główne zalety próbkowania są następujące:

* Aplikacja Insights usługi porzucania ("ogranicza") danych punktów aplikacji wysyła bardzo wysoki współczynnik dane telemetryczne w skrócie czasu interwału. 
* Aby zachować w [przydziału](app-insights-pricing.md) punktów danych dla warstwy cenowej. 
* Aby zmniejszyć ruch sieciowy z kolekcji telemetrii. 

### <a name="which-type-of-sampling-should-i-use"></a>Jakiego typu próbkowania należy używać?
**Użyj wprowadzanie próbkowania, jeśli:**

* Często przejść przez wykorzystaniu całego przydziału miesięcznego dane telemetryczne.
* Używasz wersji zestawu SDK, który nie obsługuje pobierania próbek — na przykład, zestaw SDK Java lub wersji platformy ASP.NET starszych niż 2.
* Otrzymujesz dużej ilości danych telemetrycznych z przeglądarki sieci web użytkownika.

**Użyj stałej stawki próbkowania, jeśli:**

* Używany jest zestaw SDK usługi Application Insights dla usług sieci web ASP.NET w wersji 2.0.0 lub nowszej, a
* Ma próbkowania synchronizowane między klientem a serwerem, dzięki czemu możesz podczas analizowania zdarzenia w [wyszukiwania](app-insights-diagnostic-search.md), można przechodzić między powiązanych zdarzeń na kliencie i serwerze, na przykład wyświetleń strony i żądań http.
* Upewnieniu się wartości procentowej pobierania próbek dla aplikacji. Powinien być wystarczająco duża, aby uzyskać dokładne metryki, ale poniżej poziomu przekraczający limitu przydziału cenową i limity ograniczania przepustowości. 

**Użyj adaptacyjną próbkowania:**

W przeciwnym razie zalecamy adaptacyjną próbkowania. Ta opcja jest włączona domyślnie w ASP.NET server SDK, 2.0.0-beta3 wersji lub nowszym. Do niektórych minimalna częstotliwość nie redukują ruchu, nie będzie to miało wpływ na lokacji niskiego użycia.

## <a name="how-do-i-know-whether-sampling-is-in-operation"></a>Jak sprawdzić, czy w operacji pobierania próbek
Aby odnaleźć rzeczywiste próbkowania niezależnie od tego, gdzie zostały zastosowane, należy użyć [Analytics query](app-insights-analytics.md) takich jak ta:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

W każdym zachowane rekordu, `itemCount` wskazuje liczbę oryginalnego rekordów, które reprezentuje on równą 1 + Liczba poprzednich odrzuconych rekordów. 

## <a name="how-does-sampling-work"></a>Jak działa pobierania próbek
Stałej stawki i adaptacyjną próbkowania są funkcji zestawu SDK w wersji platformy ASP.NET z 2.0.0 i jego nowszych wersjach. Wprowadzanie próbkowania to funkcja usługi Application Insights i można w operacji, jeśli zestaw SDK nie wykonuje próbkowania. 

Algorytm próbkowania decyduje elementy dane telemetryczne, które można usunąć, a które zachować (zarówno w zestawie SDK i w usłudze Application Insights). Decyzja próbkowania opiera się na kilka reguł, które Staraj się zachować wszystkie punkty danych powiązane ze sobą bez zmian, obsługa diagnostycznych doświadczenie w usłudze Application Insights jest efektywna i niezawodne nawet w przypadku ograniczonego zestawu danych. Na przykład jeśli niepomyślnych żądań aplikacji są wysyłane dodatkowe dane telemetryczne elementy (takie jak wyjątku i ślady zarejestrowane z tym żądaniem), pobierania próbek nie zostaną podzielone tego żądania i inne dane telemetryczne. Ją zachowuje lub porzuca je wszystkie razem. W związku z tym sprawdzając szczegóły żądania w usłudze Application Insights, należy zawsze widzieć wraz z jego elementy skojarzone dane telemetryczne żądania. 

Dla aplikacji, które definiują "użytkownika" (czyli najbardziej typowych aplikacji sieci web), decyzja próbkowania jest oparta na skrót identyfikator użytkownika, co oznacza, że wszystkie dane telemetryczne dla każdego konkretnego użytkownika jest zachowywana lub porzucony. Dla typów aplikacji, które nie określają użytkowników (np. usługi sieci web) decyzja próbkowania jest oparta na identyfikator operacji żądania. Na koniec elementów dane telemetryczne, które nie ma identyfikatora użytkownika ani operacji Ustaw (na przykład elementów dane telemetryczne zgłoszone asynchronicznych wątków nie kontekst http) próbkowania po prostu przechwytuje procent elementów telemetrii każdego typu. 

Przedstawiając telemetrii powrót do usługi Application Insights dostosowywaniu metryk taką samą wartość procentową próbkowania użytej w czasie kolekcji, do kompensacji brakujących punktów danych. W związku z tym podczas przeglądania danych telemetrii w usłudze Application Insights, użytkownicy wyświetlanego statystycznie prawidłową przybliżenia, które są bardzo bliski liczb rzeczywistych.

Dokładność zbliżenia zależy przede wszystkim procent skonfigurowanych próbkowania. Dokładność zwiększa się również, dla aplikacji, które obsługi dużej liczby zazwyczaj podobne żądania z partii użytkowników. Z drugiej strony dla aplikacji, które nie działają z znaczne obciążenie, próbkowania nie jest potrzebna jak te aplikacje zazwyczaj można wysłać ich dane telemetryczne pozostając w ramach limitu przydziału, nie powodując utraty danych z przepustowości. 

Należy pamiętać, że usługi Application Insights nie przykładowe typy telemetrii metryki i sesji od dla tych typów zmniejszenie dokładność można zdecydowanie niepożądane. 

### <a name="adaptive-sampling"></a>Adaptacyjną pobierania próbek
Próbkowanie adaptacyjną dodaje składnik, który monitoruje bieżąca szybkość transmisji z zestawu SDK i dopasowuje wartości procentowej pobierania próbek, aby spróbować pozostać w docelowym maksymalna szybkość. Dostosowania są przeliczane w regularnych odstępach czasu i opierają się na to ruchoma średnia szybkość transmisji wychodzących.

## <a name="sampling-and-the-javascript-sdk"></a>Pobieranie próbek i zestawu SDK języka JavaScript
Po stronie klienta (JavaScript) zestawu SDK uczestniczy w stałej częstotliwość próbkowania w połączeniu z zestawem SDK po stronie serwera. Instrumentacją stron wysyła dane telemetryczne po stronie klienta z tych samych użytkowników, dla których po stronie serwera wprowadzone decyzję "próbki w". Istotą takiej logiki zaprojektowano w celu zachowania integralności sesji użytkownika dla strony klienta i serwera. W związku z tym z dowolnym elemencie określonego telemetrii usługi Application Insights, można znaleźć wszystkie pozostałe elementy danych telemetrycznych dla tego użytkownika lub sesję. 

*Moje klienta i dane telemetryczne po stronie serwera nie pokazuj przykłady skoordynowany sposób, jak opisano powyżej.*

* Sprawdź czy jest włączony próbkowania stałej stawki zarówno na serwerze i kliencie.
* Upewnij się, że zestawu SDK w wersji 2.0 lub nowszej.
* Sprawdź, ustawić tej samej wartości procentowej pobierania próbek w klienta i serwera.

## <a name="frequently-asked-questions"></a>Często zadawane pytania
*Dlaczego nie jest próbkowania proste "zbieranie X procent każdego typu danych telemetrycznych"?*

* Gdy ta metoda pobierania próbek będzie dostarczać bardzo wysokiej precyzji w przybliżenia metryki, go spowoduje przerwanie możliwość powiązania danych diagnostycznych na użytkowników, sesji i żądań, co jest szczególnie ważne dla diagnostyki. W związku z tym pobierania próbek działa lepiej "Zbieraj wszystkie dane telemetryczne elementy X procent użytkowników aplikacji" lub "Zbieraj wszystkie dane telemetryczne dotyczące X Procent żądań aplikacji" logiki. Dla elementów telemetrii nie są skojarzone z żądaniami (na przykład asynchronicznego przetwarzania w tle), jest bazowy "zbieranie X procent wszystkie elementy dla każdego typu danych telemetrycznych." 

*Mogą ulec zmianie wartości procentowej pobierania próbek?*

* Tak, adaptacyjną próbkowania stopniowo zmienia wartości procentowej pobierania próbek, oparte na aktualnie obserwowanych ilość danych telemetrycznych.

*Użycie stałej stawki próbkowania, jak sprawdzić, które próbkowania procent będzie najlepsza dla mojej aplikacji?*

* Jednym ze sposobów jest próbkowania do uruchomienia z adaptacyjną, sprawdź, co Oceń to rozliczy na (zobacz powyżej pytanie), a następnie przejdź do stałej stawki próbkowania przy użyciu tego kursu. 
  
    W przeciwnym razie trzeba odgadnąć. Analizowanie bieżącego użycia telemetrii w AI, przestrzegać wszelkich ograniczania przepustowości występuje i oszacowania ilości zbieranych danych telemetrycznych. Te trzy wejść, wraz z wybranej warstwy cenowej, sugerują, ile można zmniejszyć ilość zbieranych danych telemetrycznych. Jednak wzrost liczby użytkowników lub inne przesunięcia w woluminie danych telemetrycznych może unieważnić oszacować.

*Co się stanie, jeśli można skonfigurować wartości procentowej pobierania próbek zbyt niska?*

* Procent próbkowania zbyt niski (próbkowanie over-aggressive) zmniejsza dokładność przybliżenia, gdy usługi Application Insights próbuje kompensacji wizualizację danych redukcji woluminów danych. Ponadto czynności diagnostycznych może być obniżona, jako część żądania rzadko awarie lub powolne próbkować można się.

*Co się stanie, jeśli można skonfigurować zbyt wysokiej wartości procentowej pobierania próbek?*

* Konfigurowanie próbkowania zbyt wysoki procent (nie agresywne wystarczająco) powoduje niewystarczające zmniejszenia ilości zbieranych danych telemetrycznych. Nadal może wystąpić utrata danych telemetrii dotyczące ograniczania przepustowości i kosztów przy użyciu usługi Application Insights może być większa niż zostanie zaplanowane z powodu nadwyżkowe opłat.

*Na platformach, które mogą używać próbkowania?*

* Wprowadzanie próbkowanie może być spowodowany automatycznie dla dowolnego telemetrii powyżej woluminu zestawu SDK nie wykonuje próbkowania. To będzie działać, na przykład, jeśli aplikacja korzysta z serwera Java lub jeśli używasz starszej wersji zestawu SDK platformy ASP.NET.
* Jeśli używasz zestawu SDK platformy ASP.NET w wersji 2.0.0 lub nowszym (hostowanego na platformie Azure lub na danym serwerze), możesz uzyskać adaptacyjną próbkowania domyślnie, ale możesz przełączyć się do stałej stawki, zgodnie z powyższym opisem. Z włączonym próbkowaniem stałej stawki, przeglądarkę zestawu SDK automatycznie synchronizuje do próbkowania powiązanych zdarzeń. 

*Brak niektórych rzadkich zdarzeń, które można zawsze były wyświetlane. Jak można je poza modułu próbkowania?*

* Inicjowanie oddzielnego wystąpienia TelemetryClient z nowego TelemetryConfiguration (nie domyślnie aktywny). Użyj do wysyłania zdarzeń rzadko.

## <a name="next-steps"></a>Następne kroki
* [Filtrowanie](app-insights-api-filtering-sampling.md) zapewniają więcej ścisłej kontroli wysyła zestawu SDK.

