---
title: "Usługa Azure Application Insights migawki debuger dla aplikacji .NET | Dokumentacja firmy Microsoft"
description: "Debugowanie migawki są zbierane automatycznie, gdy wyjątki zostaną zgłoszone w aplikacjach .NET produkcji"
services: application-insights
documentationcenter: 
author: pharring
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: mbullwin
ms.openlocfilehash: 68686e128d7e9528396f338b95f483adf07c3292
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>Debugowanie migawek na wyjątków w aplikacji .NET

Po wystąpieniu wyjątku, można automatycznie zbierać migawki debugowania aplikacji sieci web na żywo. Migawki przedstawia stan kodu źródłowego i zmiennych w momencie utworzenia zgłoszono wyjątek. Debuger migawki (wersja zapoznawcza) w [Azure Application Insights](app-insights-overview.md) monitoruje dane telemetryczne wyjątku z aplikacji sieci web. Zbiera migawki na listy wyjątków zgłaszanie top, dzięki czemu masz informacje potrzebne do diagnozowania problemów w środowisku produkcyjnym. Obejmują [pakietu NuGet modułu zbierającego migawki](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) w aplikacji i opcjonalnie skonfigurować parametry kolekcji w [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Migawki są wyświetlane na [wyjątki](app-insights-asp-net-exceptions.md) w portalu usługi Application Insights.

Migawki debugowania można wyświetlić w portalu w celu sprawdzenia wywołania stosu i sprawdzić zmiennych w każdej ramki stosu wywołań. Aby uzyskać bardziej wydajne działanie debugowania z kodem źródłowym, otwórz migawki z programu Visual Studio Enterprise 2017 przez [pobieranie rozszerzenia migawki debugera programu Visual Studio](https://aka.ms/snapshotdebugger). W programie Visual Studio można również [ustawić Snappoints do interaktywnego migawek](https://aka.ms/snappoint) bez oczekiwania na wyjątek.

Kolekcja migawki jest dostępny dla:
* Aplikacje środowiska .NET framework i program ASP.NET systemem .NET Framework 4.5 lub nowszej.
* .NET core 2.0 i ASP.NET Core 2.0 aplikacji uruchomionych w systemie Windows.

Są obsługiwane w następujących środowiskach:
* Usługa aplikacji Azure.
* Usługa w chmurze Azure programu rodziny systemów operacyjnych, 4 lub nowszego.
* Usług Azure Service Fabric uruchomiony w systemie Windows Server 2012 R2 lub nowszym.
* Azure maszyn wirtualnych z systemem Windows Server 2012 R2 lub nowszym.
* Lokalnych maszyn wirtualnych lub fizycznych z systemem Windows Server 2012 R2 lub nowszym.

> [!NOTE]
> Aplikacje klienckie (na przykład WPF i formularze systemu Windows platformy uniwersalnej systemu Windows) nie są obsługiwane.

### <a name="configure-snapshot-collection-for-aspnet-applications"></a>Konfigurowanie zbierania migawek dla aplikacji ASP.NET

1. [Włącz usługę Application Insights w aplikacji sieci web](app-insights-asp-net.md), jeśli nie jeszcze go jeszcze gotowe.

2. Obejmują [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) pakietu NuGet w aplikacji. 

3. Przejrzyj domyślne opcje dodane do pakietu [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- The default is true, but you can disable Snapshot Debugging by setting it to false -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this to true. -->
        <!-- DeveloperMode is a property on the active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need to see an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>5</ThresholdForSnapshotting>
        <!-- The maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- The maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often to reset problem counters. -->
        <ProblemCounterResetInterval>06:00:00</ProblemCounterResetInterval>
        <!-- The maximum number of snapshots allowed in one minute. -->
        <SnapshotsPerMinuteLimit>2</SnapshotsPerMinuteLimit>
        <!-- The maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>50</SnapshotsPerDayLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. Migawki są zbierane tylko na wyjątki, które są zgłaszane do usługi Application Insights. W niektórych przypadkach (na przykład starszych wersji platformy .NET), konieczne może być [Konfigurowanie zbierania wyjątków](app-insights-asp-net-exceptions.md#exceptions) aby zobaczyć wyjątki z migawkami w portalu.


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a>Konfigurowanie zbierania migawek dla aplikacji platformy ASP.NET Core 2.0

1. [Włącz usługę Application Insights w aplikacji sieci web platformy ASP.NET Core](app-insights-asp-net-core.md), jeśli nie jeszcze go jeszcze gotowe.

> [!NOTE]
> Należy się upewnić, że aplikacja odwołuje się do wersji 2.1.1 lub nowszego, Microsoft.ApplicationInsights.AspNetCore pakietu.

2. Obejmują [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) pakietu NuGet w aplikacji.

3. Modyfikowanie aplikacji `Startup` klasy, aby dodać i skonfigurować migawki modułu zbierającego dane telemetryczne procesora.

   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   using Microsoft.Extensions.Options;
   ...
   class Startup
   {
       private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
       {
           private readonly IServiceProvider _serviceProvider;

           public SnapshotCollectorTelemetryProcessorFactory(IServiceProvider serviceProvider) =>
               _serviceProvider = serviceProvider;

           public ITelemetryProcessor Create(ITelemetryProcessor next)
           {
               var snapshotConfigurationOptions = _serviceProvider.GetService<IOptions<SnapshotCollectorConfiguration>>();
               return new SnapshotCollectorTelemetryProcessor(next, configuration: snapshotConfigurationOptions.Value);
           }
       }

       public Startup(IConfiguration configuration) => Configuration = configuration;

       public IConfiguration Configuration { get; }

       // This method gets called by the runtime. Use this method to add services to the container.
       public void ConfigureServices(IServiceCollection services)
       {
           // Configure SnapshotCollector from application settings
           services.Configure<SnapshotCollectorConfiguration>(Configuration.GetSection(nameof(SnapshotCollectorConfiguration)));

           // Add SnapshotCollector telemetry processor.
           services.AddSingleton<ITelemetryProcessorFactory>(sp => new SnapshotCollectorTelemetryProcessorFactory(sp));

           // TODO: Add other services your application needs here.
       }
   }
   ```

4. Skonfiguruj moduł zbierający migawki przez dodanie sekcji SnapshotCollectorConfiguration do appsettings.json. Na przykład:

   ```json
   {
     "ApplicationInsights": {
       "InstrumentationKey": "<your instrumentation key>"
     },
     "SnapshotCollectorConfiguration": {
       "IsEnabledInDeveloperMode": true
     }
   }
   ```

### <a name="configure-snapshot-collection-for-other-net-applications"></a>Konfigurowanie zbierania migawek dla innych aplikacji .NET

1. Jeśli aplikacja nie jest już instrumentowane przy użyciu usługi Application Insights, rozpoczęcie pracy przez [Włączanie usługi Application Insights i ustawienie klucza Instrumentacji](app-insights-windows-desktop.md).

2. Dodaj [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) pakietu NuGet w aplikacji.

3. Migawki są zbierane tylko na wyjątki, które są zgłaszane do usługi Application Insights. Konieczne może być swój kod, aby zgłosić je modyfikować. Kod obsługi wyjątków jest zależna od struktury aplikacji, ale przykładem jest poniżej:
    ```C#
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle the request.
        }
        catch (Exception ex)
        {
            // Report the exception to Application Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow the exception if desired.
        }
   }
    ```
    
## <a name="grant-permissions"></a>Udzielanie uprawnień

Właściciele subskrypcji platformy Azure można sprawdzić migawki. Inni użytkownicy muszą mieć uprawnienie przez właściciela.

Aby przyznać uprawnienia, Przypisz `Application Insights Snapshot Debugger` roli do użytkowników, którzy będzie przeprowadzał inspekcję migawki. Tę rolę można przypisać do poszczególnych użytkowników lub grup właściciele subskrypcji dla zasobu usługi Application Insights docelowej lub grupy zasobów lub subskrypcji.

1. Otwiera blok kontroli dostępu (IAM).
1. Kliknij przycisk Dodaj +.
1. Wybierz debuger migawki Insights aplikacji z listy rozwijanej ról.
1. Wyszukaj, a następnie wprowadź nazwę użytkownika do dodania.
1. Kliknij przycisk Zapisz, aby dodać użytkownika do roli.


[!IMPORTANT]
    Migawki potencjalnie może zawierać informacje osobiste i innych poufnych w zmiennej i wartości parametrów.

## <a name="debug-snapshots-in-the-application-insights-portal"></a>Debugowanie migawek w portalu usługi Application Insights

Jeśli migawka jest dostępna dla danego wyjątku lub identyfikator problemu **Otwórz migawki debugowania** na pojawi się przycisk [wyjątek](app-insights-asp-net-exceptions.md) w portalu usługi Application Insights.

![Przycisk Otwórz migawki debugowania na wyjątek](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

W widoku debugowania migawek Zobacz stosu wywołań i okienku zmiennych. Po wybraniu ramki stosu wywołań w okienku Stos wywołań, można wyświetlić zmiennych lokalnych i wywołać parametrów dla tej funkcji w okienku zmiennych.

![Widok debugowania migawki w portalu](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

Migawki mogą zawierać poufne informacje, i domyślnie nie są widoczne. Aby wyświetlić migawki, musisz mieć `Application Insights Snapshot Debugger` rola przypisana użytkownikowi.

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a>Migawki z programu Visual Studio Enterprise 2017 debugowania
1. Kliknij przycisk **pobrać migawki** przycisk, aby pobrać `.diagsession` pliku, który można otworzyć w programie Visual Studio Enterprise 2017 r. 

2. Aby otworzyć `.diagsession` pliku, należy najpierw [Pobierz i zainstaluj rozszerzenie migawki debugera programu Visual Studio](https://aka.ms/snapshotdebugger).

3. Po otwarciu pliku migawki, zostanie wyświetlona strona debugowania minizrzutu w programie Visual Studio. Kliknij przycisk **debugowania kodu zarządzanego** można rozpocząć debugowania migawki. Migawka zostanie otwarty wiersz kodu, w której wystąpił wyjątek, aby umożliwić debugowanie bieżący stan procesu.

    ![Wyświetl migawkę debugowania w programie Visual Studio](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

Pobrany migawki zawiera wszystkie pliki symboli, które zostały odnalezione na serwerze aplikacji sieci web. Te pliki symboli są wymagane, aby skojarzyć dane migawki z kodu źródłowego. Dla aplikacji usługi aplikacji upewnij się, że włączyć symbol wdrożenia po opublikowaniu aplikacji sieci web.

## <a name="how-snapshots-work"></a>Jak działają migawki

Podczas uruchamiania aplikacji, tworzony jest proces przesyłania oddzielne migawki, który monitoruje aplikację żądań migawki. Zleconą migawkę kopii w tle uruchomiony proces jest przeprowadzane w milisekundach około 10 do 20. Następnie są analizowane przez proces w tle, a migawka jest tworzona podczas procesu głównego kontynuuje działanie i obsługiwać ruch do użytkowników. Migawki są następnie przekazywane do usługi Application Insights oraz pliki odpowiednich symboli (.pdb), które są potrzebne, aby wyświetlić migawki.

## <a name="current-limitations"></a>Bieżące ograniczenia

### <a name="publish-symbols"></a>Publikuj symbole
Debuger migawki wymaga plików symboli na serwerze produkcyjnym w celu zdekodowania zmienne i zapewnia środowisko debugowania w programie Visual Studio. 15.2 wydanie programu Visual Studio 2017 publikuje symboli dla kompilacji wydania domyślnie po opublikowaniu go do usługi App Service. W poprzednich wersjach, należy dodać następujący wiersz do swój profil publikowania `.pubxml` plików symboli są publikowane w trybie wersji:

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

Dla rozwiązań usługi obliczenia Azure i innych typów, upewnij się, że pliki symboli są w tym samym folderze DLL aplikacji głównej (zazwyczaj `wwwroot/bin`) lub są dostępne w bieżącej ścieżce.

### <a name="optimized-builds"></a>Zoptymalizowane kompilacji
W niektórych przypadkach zmiennych lokalnych nie można wyświetlić w kompilacjach wydania z powodu optymalizacji, które są stosowane podczas procesu kompilacji.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Te wskazówki ułatwiają rozwiązywanie problemów z debugera migawki.

### <a name="verify-the-instrumentation-key"></a>Sprawdź klucza Instrumentacji

Upewnij się, że używasz klucza Instrumentacji poprawne w opublikowanej aplikacji. Zazwyczaj usługi Application Insights odczytuje klucza Instrumentacji z pliku ApplicationInsights.config. Sprawdź, czy wartość jest taka sama jak klucza instrumentacji dla zasobu usługi Application Insights, który zostanie wyświetlony w portalu.

### <a name="check-the-uploader-logs"></a>Sprawdź dzienniki przesyłania

Po utworzeniu migawki, plik minizrzutu (dmp) jest tworzony na dysku. Proces przesyłania oddzielne przyjmuje ten plik minizrzutu i przekazuje, oraz wszystkie skojarzone pliki PDB do magazynu Application Insights migawki debugera. Po pomyślnym przekazaniu minizrzut są usuwane z dysku. Pliki dzienników w celu przesyłania minizrzutu są przechowywane na dysku. W środowisku usługi aplikacji można znaleźć te dzienniki w `D:\Home\LogFiles\Uploader_*.log`. Użyj witryny zarządzania Kudu dla aplikacji usługi, aby znaleźć te pliki dziennika.

1. Otwórz aplikację usługi aplikacji w portalu Azure.

2. Wybierz **zaawansowane narzędzia** bloku lub Wyszukaj **Kudu**.
3. Kliknij przycisk **Przejdź**.
4. W **konsoli debugowania** listy rozwijanej wybierz pozycję **CMD**.
5. Kliknij przycisk **LogFiles**.

Powinny pojawić się co najmniej jeden plik o nazwie, który rozpoczyna się od `Uploader_` i `.log` rozszerzenia. Kliknij odpowiednią ikonę, aby pobrać wszystkie pliki dziennika lub je otworzyć w przeglądarce.
Nazwa pliku zawiera nazwę komputera. Jeśli wystąpienie usługi aplikacji znajduje się na więcej niż jednej maszynie, istnieją oddzielnych plików dziennika dla każdego komputera. Gdy przekazujący wykryje nowy plik minizrzutu, jest rejestrowany w pliku dziennika. Oto przykład pomyślne ukończenie przekazywania:

```
MinidumpUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:08.0349846Z
MinidumpUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 329.12 MB
    DateTime=2017-05-25T14:25:16.0145444Z
MinidumpUploader.exe Information: 0 : Upload successful.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2017-05-25T14:25:44.2310982Z
MinidumpUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2017-05-25T14:25:44.5435948Z
MinidumpUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:44.6095821Z
```

W poprzednim przykładzie, klucz Instrumentacji jest `c12a605e73c44346a984e00000000000`. Ta wartość powinna być zgodna klucza instrumentacji aplikacji.
Minizrzut jest skojarzony z migawki z Identyfikatorem `139e411a23934dc0b9ea08a626db16c5`. Ten identyfikator można użyć do zlokalizowania telemetrii skojarzony wyjątek w Application Insights Analytics później.

Przekazujący skanowania pod kątem nowych baz danych PDB o co 15 minut. Oto przykład:

```
MinidumpUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\home\site\wwwroot\ for local PDBs.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\local\Temporary ASP.NET Files\root\a6554c94\e3ad6f22\assembly\dl3\81d5008b\00b93cc8_dec5d201 for local PDBs.
    DateTime=2017-05-25T15:11:38.8160276Z
MinidumpUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2017-05-25T15:11:38.8316450Z
MinidumpUploader.exe Information: 0 : Deleted PDB scan marker D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\.pdbscan.
    DateTime=2017-05-25T15:11:38.8316450Z
```

W przypadku aplikacji, które są _nie_ hostowanych w usłudze App Service, dzienniki przesyłania znajdują się w tym samym folderze co minizrzutów: `%TEMP%\Dumps\<ikey>` (gdzie `<ikey>` jest klucz Instrumentacji).

### <a name="use-application-insights-search-to-find-exceptions-with-snapshots"></a>Użyj usługi Application Insights Wyszukaj, aby znaleźć wyjątki z migawkami

Po utworzeniu migawki przerzucane wyjątek zostanie oznaczony przy użyciu identyfikatora migawki. Gdy dane telemetryczne wyjątku jest zgłaszane do usługi Application Insights, czy identyfikator migawki jest uwzględniona jako właściwości niestandardowej. Za pomocą bloku wyszukiwania w usłudze Application Insights, można znaleźć wszystkie dane telemetryczne z `ai.snapshot.id` właściwości niestandardowej.

1. Przejdź do zasobu usługi Application Insights w portalu Azure.
2. Kliknij przycisk **wyszukiwania**.
3. Typ `ai.snapshot.id` w polu tekstowym wyszukiwania i naciśnij klawisz Enter.

![Wyszukaj dane telemetryczne z Identyfikatorem migawki w portalu](./media/app-insights-snapshot-debugger/search-snapshot-portal.png)

Jeśli wyszukiwanie nie zwróciło żadnych wyników, migawek nie zostały zgłoszone w do usługi Application Insights dla aplikacji w wybranym zakresie czasu.

Aby wyszukać Identyfikatora określoną migawkę z dzienników przesyłania, należy wpisać ten identyfikator w polu wyszukiwania. Jeśli nie możesz znaleźć dane telemetryczne dla migawki, który został przekazany, wykonaj następujące kroki:

1. Sprawdź, czy przeglądania prawo zasobu usługi Application Insights, upewniając klucza instrumentacji.

2. Przy użyciu sygnatury czasowej w dzienniku przekazujący, Dostosuj zakres czasu filtr wyszukiwania, aby pokrywał się tego zakresu czasu.

Jeśli nadal nie widać Wystąpił wyjątek o takim identyfikatorze migawki, dane telemetryczne wyjątku nie zgłosił do usługi Application Insights. Taka sytuacja może się zdarzyć, jeśli awaria aplikacji po zajęło migawki, ale przed zgłosiła ona dane telemetryczne wyjątku. W takim wypadku zapoznaj się z dziennikami usługi aplikacji w obszarze `Diagnose and solve problems` aby zobaczyć, czy wystąpiły nieoczekiwane ponowne uruchomienia lub nieobsługiwane wyjątki.

## <a name="next-steps"></a>Następne kroki

* [Ustaw snappoints w kodzie](https://docs.microsoft.com/en-us/visualstudio/debugger/debug-live-azure-applications) można pobrać migawek bez oczekiwania na wyjątek.
* [Diagnozowanie wyjątków w aplikacjach sieci web](app-insights-asp-net-exceptions.md) wyjaśniono, jak wyświetlić więcej wyjątków do usługi Application Insights. 
* [Inteligentne wykrywania](app-insights-proactive-diagnostics.md) automatycznie odnajduje anomalii wydajności.
