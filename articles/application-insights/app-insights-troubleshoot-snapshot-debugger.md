---
title: "Usługa Azure Application Insights migawki debugera przewodnik rozwiązywania problemów | Dokumentacja firmy Microsoft"
description: "Często zadawane pytania dotyczące Azure Application Insights migawki debugera."
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 
ms.service: application-insights
ms.workload: 
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: mbullwin
ms.openlocfilehash: 2b4a5f548578b563c92f8d7ff85457b50b02fbd4
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="snapshot-debugger-troubleshooting-guide"></a>Debugera migawki: Przewodnik rozwiązywania problemów

Application Insights migawki debugera pozwala automatycznie zbiera dane migawki debugowania z aplikacji sieci web. Migawki przedstawia stan kodu źródłowego i zmiennych w momencie utworzenia Wystąpił wyjątek. W tym artykule opisano sposób debuger działa i zapewnia rozwiązania typowych problemów.

## <a name="how-does-application-insights-snapshot-debugger-work"></a>Jak działa Application Insights migawki debugera

Application Insights migawki debugera jest częścią potoku telemetrii usługi Application Insights (wystąpienia ITelemetryProcessor). Moduł zbierający migawki monitoruje zarówno wyjątki zgłaszane w kodzie (AppDomain.FirstChanceException), jak i dane telemetryczne wyjątku raportowanych do usługi Application Insights za pośrednictwem `TelemetryClient.TrackException`. Po pakietu migawki moduł zbierający pomyślnie dodane do projektu i wykrył jedno zdarzenie w potoku dane telemetryczne, niestandardowe zdarzenie o nazwie "AppInsightsSnapshotCollectorLogs" i "SnapshotCollectorEnabled" w danych niestandardowych będą wysyłane. W tym samym czasie, zostanie ona rozpoczęta w procesie nazywanym "SnapshotUploader.exe" (wersja 1.2.0 lub nowszy) lub "MinidumpUploader.exe" (przed wersji 1.2.0), można przekazać zebrane migawki plików danych do usługi Application Insights.  Po uruchomieniu procesu przekazujący wysyła zdarzenie niestandardowe o nazwie "UploaderStart". Po wykonaniu tej migawki moduł zbierający wprowadzić normalne działanie monitorowania.

Gdy moduł zbierający migawki jest monitorowanie telemetria wyjątków usługi Application Insights, używa parametrów (na przykład ThresholdForSnapshotting, MaximumSnapshotsRequired, MaximumCollectionPlanSize, ProblemCounterResetInterval) zdefiniowanych w Konfiguracja do określenia, kiedy zbiera dane migawki. Gdy są spełnione wszystkie reguły, moduł zbierający będzie żądać migawkę wyjątek dalej w tym samym miejscu. Jednocześnie niestandardowych zdarzeń usługi Application Insights o nazwie "AppInsightsSnapshotCollectorLogs" i "RequestSnapshots" zostanie wysłana. Ponieważ kompilator będzie Optymalizuj kod "Release", zmienne lokalne mogą nie być widoczne w zebrane migawki. Moduł zbierający migawki zostanie podjęta próba deoptimize metodę, która zgłosiła wyjątek podczas żądania migawki. W tym czasie zostaną wysłane zdarzenia usługi Application Insights niestandardowy o nazwie "AppInsightsSnapshotCollectorLogs" i "ProductionBreakpointsDeOptimizeMethod" w danych niestandardowych.  Gdy migawki dalej wyjątków są zbierane, zmienne lokalne będą dostępne. Po zebraniu migawki, zostanie on wklejeniu kodu. 

> [!NOTE]
> Deoptimization wymaga rozszerzenia lokacji usługi Application Insights do zainstalowania.

Gdy migawka jest wymagany dla określonych wyjątków, moduł zbierający migawki rozpocznie się monitorowania wyjątków aplikacji obsługi potoku (AppDomain.FirstChanceException). Jeśli wyjątek wystąpi ponownie, moduł zbierający zostanie uruchomiony migawka (niestandardowych zdarzeń usługi Application Insights o nazwie "AppInsightsSnapshotCollectorLogs" i "SnapshotStart" w danych niestandardowych). Następnie staje się kopii w tle uruchomionego procesu (tabeli stron zostaną zduplikowane). To zwykle potrwa 10-20 w milisekundach. Po to niestandardowych zdarzeń usługi Application Insights o nazwie "AppInsightsSnapshotCollectorLogs" i "SnapshotStop" w niestandardowe dane zostaną wysłane. Po utworzeniu procesu rozwidlonych całkowitej ilości pamięci stronicowanej zostanie zwiększony o tę samą wartość jako stronicowane pamięci uruchomionej aplikacji (zestaw roboczy będzie znacznie mniejszy). Podczas procesu aplikacji działa normalnie, kopie w tle pamięci przez proces będzie utworzyć zrzutu na dysku i przekazać do usługi Application Insights. Po przekazaniu migawki zostaną wysłane niestandardowe zdarzenia usługi Application Insights o nazwie "UploadSnapshotFinish".

## <a name="is-the-snapshot-collector-working-properly"></a>Moduł zbierający migawki działa prawidłowo?

### <a name="how-to-find-snapshot-collector-logs"></a>Jak znaleźć migawki modułu zbierającego dzienniki
Migawki modułu zbierającego dzienniki są wysyłane do swojego konta szczegółowe informacje o aplikacji, jeśli [pakietu NuGet modułu zbierającego migawki](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) jest w wersji 1.1.0 lub nowszej. Upewnij się, że *ProvideAnonymousTelemetry* nie ma wartość false (wartość to true domyślnie).

* Przejdź do zasobu usługi Application Insights w portalu Azure.
* Kliknij przycisk *wyszukiwania* w sekcji Przegląd.
* W polu wyszukiwania, wprowadź następujący ciąg:
    ```
    AppInsightsSnapshotCollectorLogs OR AppInsightsSnapshotUploaderLogs OR UploadSnapshotFinish OR UploaderStart OR UploaderStop
    ```
* Uwaga: zmiana *zakres czasu* w razie potrzeby.

![Zrzut ekranu wyszukiwania migawki zbierania dzienników](./media/app-insights-troubleshoot-snapshot-debugger/001.png)


### <a name="examine-snapshot-collector-logs"></a>Przeanalizuj dzienniki modułu zbierającego migawki
Podczas wyszukiwania migawki modułu zbierającego dzienniki, w zakresie docelowego czasu powinien istnieć zdarzenia "UploadSnapshotFinish". Jeśli nadal nie widzisz przycisk "Otwórz debugowania Snapshot", aby otworzyć migawki, Wyślij wiadomość e-mail do snapshothelp@microsoft.com kluczem Instrumentacji Application Insights.

![Zrzut ekranu zbadać migawki modułu zbierającego dzienniki](./media/app-insights-troubleshoot-snapshot-debugger/005.png)

## <a name="i-cannot-find-a-snapshot-to-open"></a>Nie można znaleźć migawki na otwarte
Jeśli poniższe kroki nie pomocne w rozwiązaniu problemu, Wyślij wiadomość e-mail do snapshothelp@microsoft.com z klucza Instrumentacji z usługi Application Insights.

### <a name="step-1-make-sure-your-application-is-sending-telemetry-data-and-exception-data-to-application-insights"></a>Krok 1: Upewnij się, że aplikacja jest wysyłania danych telemetrycznych i dane wyjątku do usługi Application Insights
Przejdź do zasobu usługi Application Insights, sprawdź, czy dane przesyłane z aplikacji.

### <a name="step-2-make-sure-snapshot-collector-is-added-correctly-to-your-applications-application-insights-telemetry-pipeline"></a>Krok 2: Upewnij się, że moduł zbierający migawki poprawnie dodawany do potoku Telemetrię usługi Application Insights aplikacji
Jeśli dzienniki można znaleźć w kroku "Jak znaleźć migawki modułu zbierającego dzienniki", moduł zbierający migawki jest poprawnie dodane do projektu i możesz zignorować ten krok.

Jeśli nie ma żadnych dzienników modułu zbierającego migawki, należy sprawdzić następujące kwestie:
* Dla klasycznych aplikacji programu ASP.NET, Wyszukaj ten wiersz  *<Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">*  w *ApplicationInsights.config* pliku.

* Upewnij się, że dla aplikacji platformy ASP.NET Core *ITelemetryProcessorFactory* z *SnapshotCollectorTelemetryProcessor* jest dodawany do *IServiceCollection* usług .

* Sprawdź również, że używasz klucza Instrumentacji poprawne w opublikowanej aplikacji.

* Moduł zbierający migawki nie obsługuje wielu kluczy Instrumentacji w jednej aplikacji, wysyła migawki klucz Instrumentacji pierwszy wyjątek, których on obowiązuje.

* Jeśli ustawisz *InstrmentationKey* ręcznie w kodzie, należy zaktualizować *InstrumentationKey* element z *ApplicationInsights.config*.

### <a name="step-3-make-sure-the-minidump-uploader-is-started"></a>Krok 3: Upewnij się, że moduł przesyłający minizrzutu jest uruchomiona.
Moduł zbierający dzienniki migawki, wyszukiwanie *UploaderStart* (w polu wyszukiwania wpisz UploaderStart). Powinien być zdarzenie, gdy moduł zbierający migawki monitorowane pierwszy wyjątek. Jeśli to zdarzenie nie istnieje, sprawdź inne dzienniki, aby uzyskać szczegółowe informacje. Jednym ze sposobów możliwe rozwiązania tego problemu jest ponowne uruchomienie aplikacji.

### <a name="step-4-make-sure-snapshot-collector-expressed-its-intent-to-collect-snapshots"></a>Krok 4: Upewnij się, że moduł zbierający migawki wyrażone chęć zbierane migawki
Moduł zbierający dzienniki migawki, wyszukiwanie *RequestSnapshots* (typ ```RequestSnapshots``` w polu tekstowym wyszukiwania).  Jeśli nie ma żadnego, sprawdź konfigurację. Na przykład *ThresholdForSnapshotting*, co oznacza liczbę określony wyjątek, który może wystąpić przed rozpoczęciem zbierania migawek.

### <a name="step-5-make-sure-that-snapshot-is-not-disabled-due-to-memory-protection"></a>Krok 5: Upewnij się, czy migawki nie jest wyłączona z powodu ochrony pamięci
Aby chronić wydajności aplikacji, migawkę tylko będzie można przechwycić po bufora pamięci dobrej. Moduł zbierający dzienniki migawki wyszukiwanie "CannotSnapshotDueToMemoryUsage". W danych niestandardowych zdarzeń będzie mieć szczegółowe przyczyny. Jeśli aplikacja jest uruchomiona w aplikacji sieci Web platformy Azure, może być ścisłe ograniczenie. Ponieważ aplikacji sieci Web platformy Azure uruchomi aplikacji, gdy są spełnione pewne reguły pamięci. Możesz skalować planu usługi maszynom więcej pamięci, aby rozwiązać ten problem.

### <a name="step-6-make-sure-snapshots-were-captured"></a>Krok 6: Upewnij się, że zostały przechwycone migawki
Moduł zbierający dzienniki migawki, wyszukiwanie ```RequestSnapshots```.  Jeśli żaden nie jest obecny, sprawdź konfigurację. Na przykład *ThresholdForSnapshotting*, co oznacza liczbę określonych wyjątków przed zebraniu migawki.

### <a name="step-7-make-sure-snapshots-are-uploaded-correctly"></a>Krok 7: Upewnij się, że migawki są przekazywane poprawnie
Moduł zbierający dzienniki migawki, wyszukiwanie ```UploadSnapshotFinish```.  Jeśli to nie jest obecny, Wyślij wiadomość e-mail do snapshothelp@microsoft.com z klucza Instrumentacji z usługi Application Insights. Jeśli takie zdarzenie istnieje, otwórz je dzienniki i skopiuj wartość "SnapshotId" w danych niestandardowych. Następnie znajdź wartość. To znajdzie wyjątek odpowiadający migawki. Następnie kliknij przycisk wyjątku i otwórz debugowania migawki. (Jeśli brak odpowiednich wyjątków, dane telemetryczne wyjątku próbkować można z powodu dużej liczby. Spróbuj innej snapshotId).

![Zrzut ekranu przedstawiający SnapshotId Znajdź](./media/app-insights-troubleshoot-snapshot-debugger/002.png)

![Zrzut ekranu przedstawiający Otwórz wyjątek](./media/app-insights-troubleshoot-snapshot-debugger/004b.png)

![Zrzut ekranu Otwórz migawki debugowania](./media/app-insights-troubleshoot-snapshot-debugger/003.png)

## <a name="snapshot-view-local-variables-are-not-complete"></a>Zmienne lokalne widoku migawki nie zostały zakończone

Brakuje niektórych zmiennych lokalnych. Jeśli aplikacja jest uruchomiona wersja kodu, kompilator zoptymalizuje niektóre zmienne optymalizacji. Na przykład:

```csharp
    const int a = 1; // a will be discarded by compiler and the value 1 will be inline.
    Random rand = new Random();
    int b = rand.Next() % 300; // b will be discarded and the value will be directly put to the 'FindNthPrimeNumber' call stack.
    long primeNumber = FindNthPrimeNumber(b);
```

Jeśli Twoje przypadek jest inny, może to wskazywać usterki. Wyślij wiadomość e-mail do snapshothelp@microsoft.com z klucza Instrumentacji z usługi Application Insights wraz z fragmentu kodu.

## <a name="snapshot-view-cannot-obtain-value-of-the-local-variable-or-argument"></a>Widok miniatur: Nie można uzyskać wartości lokalnej zmiennej lub argumentu
Upewnij się, że [usługi Application Insights lokacji rozszerzenia](https://www.siteextensions.net/packages/Microsoft.ApplicationInsights.AzureWebSites/) jest zainstalowany. Jeśli problem będzie się powtarzać, Wyślij wiadomość e-mail do snapshothelp@microsoft.com z klucza Instrumentacji z usługi Application Insights.
