---
title: "Poziom aplikacji sieci szkieletowej usług Azure monitorowania | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat i zdarzenia na poziomie usługi Dzienniki aplikacji i umożliwia monitorowanie i diagnozowanie klastrów sieci szkieletowej usług Azure."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 9b07c7cd256cea31c5654f0c3325e9fb675b39f8
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="application-and-service-level-event-and-log-generation"></a>Generowanie zdarzeń i dzienników na poziomie aplikacji i usług

## <a name="instrumenting-the-code-with-custom-events"></a>Instrumentacja kodu przy użyciu zdarzeń niestandardowych

Kod Instrumentacji stanowi podstawę do większości innych aspektów monitorowania usług. Instrumentacja jest jedynym sposobem wiesz, czy jest coś niewłaściwego i diagnozowania, co wymaga naprawy. Chociaż jest technicznie można połączyć z usługą produkcji debugera, nie jest typowym rozwiązaniem. Tak o szczegółowe dane Instrumentacji jest ważna.

Niektóre produkty automatyczną Instrumentację kodu. Mimo że te rozwiązania mogą również działać, ręczne Instrumentacja prawie zawsze jest wymagana. W końcu musi mieć wystarczającej ilości informacji do całkowitego debugowania aplikacji. W tym dokumencie opisano różne podejścia do Instrumentacji kodu i kiedy należy wybrać jeden z nich zamiast innego.

## <a name="eventsource"></a>Źródła zdarzeń
Po utworzeniu rozwiązania sieci szkieletowej usług z szablonu w programie Visual Studio, **EventSource**-klasy (**ServiceEventSource** lub **ActorEventSource**) jest generowany. Utworzyć szablonu, w którym można dodać zdarzeń dla usługi lub aplikacji. **EventSource** nazwa **musi** być unikatowa i powinny zostać zmienione z domyślnego szablonu ciągu Moja firma -&lt;rozwiązania&gt;-&lt;projektu&gt;. Istnieniem wielu **EventSource** definicje, które korzystają z tej samej nazwy powoduje problem w czasie wykonywania. Każdy zdefiniowane zdarzenie musi mieć unikatowy identyfikator. Jeśli identyfikator nie jest unikatowa, wystąpi błąd czasu wykonywania. Niektóre organizacje preassign zakresów wartości identyfikatorów uniknąć konfliktów między oddzielne zespoły deweloperów. Aby uzyskać więcej informacji, zobacz [zaliczko w blogu](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) lub [dokumentacji MSDN](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

### <a name="using-structured-eventsource-events"></a>W przypadku używania strukturalnych zdarzeń źródła zdarzeń

Każdego zdarzenia w przykładach kodu w tej sekcji są definiowane dla określonych przypadków, na przykład podczas rejestrowania typu usługi. Podczas definiowania wiadomości w przypadku użycia, danych można spakować z tekst błędu i mogą w bardziej łatwo wyszukiwać i filtr na podstawie nazw lub wartości określonej właściwości. Struktura sprawia, że dane wyjściowe Instrumentacji łatwiej wykorzystać, ale wymaga więcej myśl i czasu, aby zdefiniować nowe zdarzenie dla każdego przypadku użycia. Definicje zdarzeń mogą być współużytkowane przez całej aplikacji. Na przykład metoda uruchamiania lub zatrzymywania zdarzeń będzie można używać ponownie w wielu usług w aplikacji. Usługa specyficznego dla domeny, takich jak system zamówień może mieć **CreateOrder** zdarzenie, które ma własną unikatową zdarzeń. Takie podejście może wygenerować wiele zdarzeń i mogą wymagać koordynacji identyfikatorów zespołów projektu. 

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // The instance constructor is private to enforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        // The ServiceTypeRegistered event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
        private const int ServiceTypeRegisteredEventId = 3;
        [Event(ServiceTypeRegisteredEventId, Level = EventLevel.Informational, Message = "Service host process {0} registered service type {1}", Keywords = Keywords.ServiceInitialization)]
        public void ServiceTypeRegistered(int hostProcessId, string serviceType)
        {
            WriteEvent(ServiceTypeRegisteredEventId, hostProcessId, serviceType);
        }

        // The ServiceHostInitializationFailed event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
        private const int ServiceHostInitializationFailedEventId = 4;
        [Event(ServiceHostInitializationFailedEventId, Level = EventLevel.Error, Message = "Service host initialization failed", Keywords = Keywords.ServiceInitialization)]
        public void ServiceHostInitializationFailed(string exception)
        {
            WriteEvent(ServiceHostInitializationFailedEventId, exception);
        }
```

### <a name="using-eventsource-generically"></a>Ogólnie przy użyciu źródła zdarzeń

Definiowanie określonych zdarzeń mogą być trudne, wiele osób zdefiniować kilka zdarzenia ze wspólnego zestawu parametrów, które zwykle wysyłają informacji o ich jako ciąg. Większość strukturalnych aspekt zostaną utracone i jest trudne do wyszukiwania i filtrowania wyników. W takie podejście są zdefiniowane kilka zdarzeń, które zwykle odpowiadają poziomy rejestrowania. Poniższy fragment kodu definiuje debugowania i komunikatu o błędzie:

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // The Instance constructor is private, to enforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        private const int DebugEventId = 10;
        [Event(DebugEventId, Level = EventLevel.Verbose, Message = "{0}")]
        public void Debug(string msg)
        {
            WriteEvent(DebugEventId, msg);
        }

        private const int ErrorEventId = 11;
        [Event(ErrorEventId, Level = EventLevel.Error, Message = "Error: {0} - {1}")]
        public void Error(string error, string msg)
        {
            WriteEvent(ErrorEventId, error, msg);
        }
```

Przy użyciu hybrydowego Instrumentacji strukturalnych i rodzajowy również może działać również. Strukturalne Instrumentacji jest używana do raportowania błędów i metryki. Zdarzenia ogólne może służyć do szczegółowe rejestrowanie, używanej przez inżynierów do rozwiązywania problemów.

## <a name="aspnet-core-logging"></a>Platformy ASP.NET Core rejestrowania

Należy dokładnie zaplanować, jak będzie Instrumentacja kodu. Plan prawym Instrumentacji może pomóc w uniknięciu potencjalnie destabilizing kodzie, a następnie konieczności reinstrument kod. Aby zmniejszyć ryzyko, można wybrać Biblioteka instrumentacji, takie jak [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), który jest częścią pakietu Microsoft ASP.NET Core. Ma platformy ASP.NET Core [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) interfejs, który można używać z dostawcy wybranych przez użytkownika, przy jednoczesnym zmniejszeniu wpływu na istniejący kod. Kod w ASP.NET Core w systemach Windows i Linux, a pełna platformy .NET, więc kodu Instrumentacji jest standardowym. Jest to dodatkowe przedstawione poniżej:

### <a name="using-microsoftextensionslogging-in-service-fabric"></a>Przy użyciu Microsoft.Extensions.Logging w sieci szkieletowej usług

1. Dodaj pakiet Microsoft.Extensions.Logging NuGet do projektu, aby dokumentu. Ponadto Dodaj wszelkie pakiety dostawcy (pakietu innych firm, zobacz poniższy przykład). Aby uzyskać więcej informacji, zobacz [logowanie do platformy ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).
2. Dodaj **przy użyciu** dyrektywy dla Microsoft.Extensions.Logging do pliku usługi.
3. Zdefiniuj zmienną prywatny w klasie usługi.

  ```csharp
  private ILogger _logger = null;

  ```
4. W konstruktorze klasy usługi Dodaj ten kod:

  ```csharp
  _logger = new LoggerFactory().CreateLogger<Stateless>();

  ```
5. Uruchom kod w metody instrumentacji. Oto kilka przykładów:

  ```csharp
  _logger.LogDebug("Debug-level event from Microsoft.Logging");
  _logger.LogInformation("Informational-level event from Microsoft.Logging");

  // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and the duration of the request.
  // Later in the article, we discuss why this step is useful.
  _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);

  ```

## <a name="using-other-logging-providers"></a>Przy użyciu innych dostawców logowania

Niektóre użycia innych dostawców podejścia opisanego w poprzedniej sekcji, w tym [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), i [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging). Możesz podłączyć każdego z nich do platformy ASP.NET Core rejestrowania lub można ich używać oddzielnie. Serilog jest funkcją, która wzbogaca wszystkich wiadomości wysłanych z rejestrator. Ta funkcja może być przydatne do wyjściowego nazwę usługi, typ i informacji o partycji. Aby używać tej funkcji w infrastrukturze podstawowej platformy ASP.NET, wykonaj następujące kroki:

1. Dodawanie pakietów Serilog, Serilog.Extensions.Logging i Serilog.Sinks.Observable NuGet do projektu. Na przykład dalej należy również dodać Serilog.Sinks.Literate. Lepszym rozwiązaniem jest wyświetlany w dalszej części tego artykułu.
2. W Serilog Utwórz LoggerConfiguration i wystąpienia rejestratora.

  ```csharp
  Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
  ```

3. Dodaj Serilog.ILogger argument do konstruktora usługi i podaj nowo utworzony rejestratora.

  ```csharp
  ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
  ```

4. W Konstruktorze usługi, Dodaj następujący kod, który tworzy enrichers właściwości dla **ServiceTypeName**, **ServiceName**, **PartitionId**, i **InstanceId** właściwości usługi. Dodano również enricher właściwości z fabryką rejestrowanie platformy ASP.NET Core, dzięki czemu można używać Microsoft.Extensions.Logging.ILogger w kodzie.

  ```csharp
  public Stateless(StatelessServiceContext context, Serilog.ILogger serilog)
      : base(context)
  {
      PropertyEnricher[] properties = new PropertyEnricher[]
      {
          new PropertyEnricher("ServiceTypeName", context.ServiceTypeName),
          new PropertyEnricher("ServiceName", context.ServiceName),
          new PropertyEnricher("PartitionId", context.PartitionId),
          new PropertyEnricher("InstanceId", context.ReplicaOrInstanceId),
      };

      serilog.ForContext(properties);

      _logger = new LoggerFactory().AddSerilog(serilog.ForContext(properties)).CreateLogger<Stateless>();
  }
  ```

5. Instrumentacja kod na takie same, jak przy użyciu platformy ASP.NET Core bez Serilog.

  >[!NOTE]
  >Firma Microsoft zaleca, czy nie jest używany statyczny Log.Logger z poprzedniego przykładu. Sieć szkieletowa usług może obsługiwać wiele wystąpień tego samego typu usługi w ramach jednego procesu. Jeśli używasz Log.Logger statycznych, ostatniego składnika zapisywania programu enrichers właściwości zostaną wyświetlone wartości dla wszystkich wystąpień, które są uruchomione. Jest jednym z powodów dlaczego zmiennej _logger jest zmienną prywatnego elementu członkowskiego klasy usługi. Ponadto należy _logger dostępne dla wspólnego kod, który może być używane w usługach.

## <a name="choosing-a-logging-provider"></a>Wybieranie dostawcy logowania

Jeśli aplikacja korzysta z wysokiej wydajności, **EventSource** zazwyczaj jest to dobre rozwiązanie. **Element EventSource** *zazwyczaj* wykorzystuje mniej zasobów i lepszą wydajność niż platformy ASP.NET Core rejestrowania lub dowolny z dostępnych rozwiązań innych firm.  To nie jest problemem w przypadku wielu usług, ale jeśli usługa jest ukierunkowane, za pomocą **EventSource** może być lepszym rozwiązaniem. Jednak aby uzyskać te korzyści z strukturę rejestrowania, **EventSource** wymaga większych inwestycji zespołu inżynieryjnego. Jeśli to możliwe czy szybkie prototyp kilka opcji rejestrowania, a następnie wybierz jedną, która najlepiej odpowiada Twoim potrzebom.

## <a name="next-steps"></a>Następne kroki

Po wybraniu dostawcy rejestrowania Instrumentacja aplikacji i usług z dzienników i zdarzeń muszą zagregowane, zanim będzie można wysłać do dowolnej platformie analizy. Przeczytaj informacje o [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) i [WAD](service-fabric-diagnostics-event-aggregation-wad.md) aby lepiej zrozumieć niektóre zalecane opcje.
