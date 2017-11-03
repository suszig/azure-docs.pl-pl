---
title: "Profilowanie aplikacji sieci web na platformie Azure za pomocą usługi Application Insights | Dokumentacja firmy Microsoft"
description: "Określ ścieżkę aktywną w kodzie serwera sieci web z profilera niskiego poziomu."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: mbullwin
ms.openlocfilehash: f6669d90878398dcd4592df97180dcd59b146350
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="profiling-live-azure-web-apps-with-application-insights"></a>Profilowanie aplikacji sieci web platformy Azure na żywo za pomocą usługi Application Insights

*Ta funkcja usługi Application Insights jest GA dla usług aplikacji i w wersji zapoznawczej obliczania.*

Sprawdzić, ile jest czas w każdej metodzie w aplikacji sieci web na żywo za pomocą narzędzia profilowania z [Azure Application Insights](app-insights-overview.md). Pokazuje szczegółowe profile bieżących żądań, które zostały obsłużone przez aplikację, a wyróżnia "ścieżkę aktywną", który używa najwięcej czasu. Powoduje automatyczne wybranie przykłady, które mają czasy odpowiedzi na inny. Profiler używa różnych technik, aby zminimalizować obciążenie.

Profiler jest obecnie obsługiwane platformy ASP.NET sieci web aplikacji działających na usługi aplikacji Azure, co najmniej Basic warstwy cenowej.

<a id="installation"></a>
## <a name="enable-the-profiler"></a>Włącz profilera

[Zainstaluj usługi Application Insights](app-insights-asp-net.md) w kodzie. Jeśli jest już zainstalowany, upewnij się, że masz najnowszą wersję. (Aby to zrobić, kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz pakiety zarządzania pakietami NuGet. Wybierz aktualizacje i wszystkich pakietów aktualizacji). Wdróż ponownie aplikację.

*Przy użyciu platformy ASP.NET Core? [Sprawdź tutaj](#aspnetcore).*

W [https://portal.azure.com](https://portal.azure.com), otwórz zasobu usługi Application Insights dla aplikacji sieci web. Otwórz **wydajności** i kliknij przycisk **włączyć profilera Insights aplikacji...** .

![Kliknij pozycję Włącz transparent profilera][enable-profiler-banner]

Alternatywnie możesz zawsze kliknąć **Konfiguruj** Aby wyświetlić stan, włączanie lub wyłączanie profilera.

![W bloku wydajności kliknij przycisk Konfiguruj][performance-blade]

Aplikacje sieci Web, które są skonfigurowane przy użyciu usługi Application Insights są wyświetlane w bloku Konfigurowanie. Postępuj zgodnie z instrukcjami, aby zainstalować agentów Profiler, jeśli to konieczne. Jeśli żadnej aplikacji sieci web jest skonfigurowana jeszcze z usługą Application Insights, kliknij przycisk *Dodaj aplikacje połączone*.

Użyj *włączyć profilera* lub *wyłączanie profilera* przyciski w bloku konfigurowanie do kontrolowania profilera na wszystkie aplikacje sieci web połączonej.

![Skonfiguruj bloku][linked app services]

W przeciwieństwie do aplikacji sieci web hostowanej przez plany usługi App Service, aplikacje hostowane w *rozwiązań usługi obliczenia Azure* zasobów (np.: maszyny wirtualnej, zestawu skalowania maszyn wirtualnych, sieci szkieletowej usług, usługi w chmurze) nie są zarządzane bezpośrednio przez platformę Azure. W takim przypadku istnieje żadnej aplikacji sieci web, aby utworzyć łącze do tej lokalizacji i musisz tylko kliknąć, aby włączyć profilera na ekranie.

## <a name="disable-the-profiler"></a>Wyłączanie profilera
Zatrzymaj lub ponownego uruchomienia profilera dla poszczególnych wystąpień usługi aplikacji, znajdziesz ją **w zasobie usługi aplikacji**w **zadania Web Job**. Aby usunąć je, sprawdź w obszarze **rozszerzenia**.

![Wyłącz program profilujący do zadania sieci web][disable-profiler-webjob]

Zaleca się, że Profiler włączona na wszystkich aplikacji sieci web, aby jak najszybciej odnajdź wszystkie problemy z wydajnością.

Jeśli używasz WebDeploy wdrażania zmian do aplikacji sieci web, upewnij się, można wykluczyć **App_Data** folderu przed usunięciem podczas wdrażania. W przeciwnym razie rozszerzenia profilera pliki zostaną usunięte podczas obok wdrażania aplikacji sieci web na platformie Azure.

### <a name="using-profiler-with-azure-vms-and-compute-resources-preview"></a>Przy użyciu profilera z maszynami wirtualnymi Azure i zasobów obliczeniowych (wersja zapoznawcza)

Gdy użytkownik [Włącz usługi Application Insights dla usługi Azure aplikacji w czasie wykonywania](app-insights-azure-web-apps.md#run-time-instrumentation-with-application-insights), Profiler jest automatycznie dostępny. (Jeśli jeszcze włączone usługi Application Insights dla zasobu, może być konieczna aktualizacja do najnowszej wersji za pomocą **Konfiguruj** kreatora.)

Brak [wersji zapoznawczej profilera dla rozwiązań usługi obliczenia Azure zasobów](https://go.microsoft.com/fwlink/?linkid=848155).


## <a name="limitations"></a>Ograniczenia

Przechowywanie danych domyślna to 5 dni. Maksymalna pozyskanych dziennie 10 GB.

Jest bezpłatna dla usługi profilera. Aplikacja sieci web musi być obsługiwana w warstwie podstawowej usług aplikacji.

## <a name="overhead-and-sampling-algorithm"></a>Koszty i algorytm pobierania próbek

Profiler losowo uruchamia 2 minuty co godzinę na każdej maszynie wirtualnej, która obsługuje aplikację z profilera włączone do przechwytywania danych śledzenia. Profiler jest uruchomiona, dodaje między 5 – 15% obciążenie Procesora na serwerze.
Więcej serwerów dostępnych do hostowania aplikacji mniej wpływu, jaki ma profilera na ogólną wydajnością. To jest ponieważ algorytm próbkowania powoduje profilera systemem uruchomić tylko na % 5 serwerów w danym momencie, a więcej serwerów będą dostępne do obsługi żądań sieci web do przesunięcia serwerami, które są koszty z profilera.


## <a name="viewing-profiler-data"></a>Wyświetlanie danych profilera

Otwórz blok wydajności i przewiń w dół listy operacji.




![Blok Insights wydajności aplikacji przykłady kolumny][performance-blade-examples]

Kolumn w tabeli są:

* **Liczba** — liczba tych żądań w zakresie czasu bloku.
* **Mediana** - typowy czas życia aplikacji, odpowiadanie na żądanie. Połowa wszystkich odpowiedzi zostały szybciej niż to.
* **95. percentyl** 95% odpowiedzi zostały szybciej niż to. Jeśli tego rysunku jest bardzo różnią się od medianę, może to być tymczasowy problem z aplikacją. (Lub może być wyjaśnione przy użyciu funkcji projektu, takie jak buforowanie.)
* **Ślady profilera** — ikona wskazuje, że profiler zostały przechwycone dane śledzenia stosu dla tej operacji.

Kliknij przycisk Widok, aby otworzyć Eksploratora śledzenia. Eksplorator przedstawiono kilka przykładów czy profilera zostały przechwycone, sklasyfikowane przez czas odpowiedzi.

Jeśli używasz bloku wydajności podglądu, przejdź do **podjąć akcje** sekcji w prawym dolnym rogu, aby wyświetlić ślady profilera. Kliknij przycisk ślady profilera.

![Application Insights wydajności bloku Podgląd ślady profilera][performance-blade-v2-examples]

Wybierz przykład do pokazywania podział poziomie kodu czas wykonania żądania.

![Eksplorator śledzenia usługi Application Insights][trace-explorer]

**Pokaż ścieżkę aktywną** otwiera pośrednictwem powiązanych punktów sprzedaży liść węzła lub co najmniej coś zamknąć. W większości przypadków będzie sąsiadujące wąskie gardło tego węzła.



* **Etykieta**: nazwę funkcji lub zdarzeń. Drzewo zawiera zarówno kod i zdarzenia (takie jak zdarzenia SQL i http). Górny zdarzeń reprezentuje całkowity czas trwania żądania.
* **Upłynął**: interwału czasu rozpoczęcia operacji i zakończenia.
* **Gdy**: Pokazuje po funkcji lub zdarzenia była uruchomiona w odniesieniu do innych funkcji.

## <a name="how-to-read-performance-data"></a>Jak można odczytać danych wydajności

Profiler usługi firmy Microsoft używa kombinacji metody próbkowania i instrumentacji do analizowania wydajności aplikacji.
Gdy szczegółowe kolekcji jest w toku, profilera usługi przykłady wskaźnik instrukcji każdego procesora CPU na komputerze w każdym milisekund.
Każda próbka przechwytuje stosu wywołań zakończenie wątku aktualnie wykonywane, podając szczegółowe i przydatne informacje o tym, co, że wątek wykonywanych zarówno na poziomie wysoki i niski poziom abstrakcji. Usługa profiler zbiera również inne zdarzenia, takie jak zdarzenia przełączania kontekstu, zdarzenia TPL i zdarzenia puli wątków do śledzenia korelacji działania i przyczynowości.

Stos wywołań w widoku osi czasu jest wynikiem powyższych próbkowania i instrumentacji. Ponieważ każdy przykład przechwytuje stosu wywołań zakończenie wątku, zawiera kod z programu .NET framework, a także innych platform, do którego odwołuje.

### <a id="jitnewobj"></a>Obiekt alokacji (`clr!JIT\_New or clr!JIT\_Newarr1`)
`clr!JIT\_New and clr!JIT\_Newarr1`są funkcje pomocnicze wewnątrz przydziela pamięć z sterty zarządzanej .NET framework. `clr!JIT\_New`wywoływane, gdy obiekt jest przydzielony. `clr!JIT\_Newarr1`wywoływane, gdy przydzielone jest Tablica obiektów. Te dwie funkcje są zazwyczaj bardzo szybko i powinno zająć względnie krótkim czasie. Jeśli widzisz `clr!JIT\_New` lub `clr!JIT\_Newarr1` trwać dość czasu na osi czasu, to wskazanie, że kodu mogą być Alokacja wiele obiektów i zużywa dużo pamięci.

### <a id="theprestub"></a>Podczas ładowania kodu (`clr!ThePreStub`)
`clr!ThePreStub`jest to funkcja pomocnika wewnątrz .NET framework, który przygotowuje kod do wykonania po raz pierwszy. Zwykle obejmuje to między innymi, kompilacja JIT (tylko w czasie). W przypadku każdego C# metody `clr!ThePreStub` powinna być wywoływana co najwyżej raz przez cały okres istnienia procesu.

Jeśli widzisz `clr!ThePreStub` zajmuje dużo czasu na żądanie, wskazuje, że żądanie jest pierwszą, która wykonuje tej metody i czas dla środowiska wykonawczego .NET framework załadować tej metody jest znacząca. Należy wziąć pod uwagę procesem rozgrzewania część kodu jest wykonywana przed użytkowników do niego dostęp, lub rozważ użycie polecenia NGen z zestawów.

### <a id="lockcontention"></a>Blokowanie rywalizacji (`clr!JITutil\_MonContention` lub `clr!JITutil\_MonEnterWorker`)
`clr!JITutil\_MonContention`lub `clr!JITutil\_MonEnterWorker` wskazują bieżący wątek oczekuje na zwolnienie blokady. To zwykle zostaną wyświetlone podczas wykonywania C# instrukcji "lock", wywołania metody Monitor.Enter lub wywołania metody z atrybutem MethodImplOptions.Synchronized. Rywalizacji blokad występuje zwykle, gdy wątek A uzyskuje blokadę i próbuje uzyskać blokady tego samego, przed zwolnieniem wątku A wątku B.

### <a id="ngencold"></a>Podczas ładowania kodu (`[COLD]`)
Jeśli nazwa metody zawiera `[COLD]`, takich jak `mscorlib.ni![COLD]System.Reflection.CustomAttribute.IsDefined`, oznacza to, że środowiska wykonawczego .NET framework jest wykonywany kod, który nie jest zoptymalizowana przez <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">Optymalizacja sterowana profilem</a> po raz pierwszy. Dla każdej metody go powinny być widoczne co najwyżej raz przez cały okres istnienia procesu.

Ładowania kodu zajmuje dużo czasu na żądanie, wskazuje, to żądanie jest pierwsza z nich można wykonać zoptymalizowanego część metody. Należy wziąć pod uwagę ciepłych proces, która część kodu jest wykonywana przed użytkowników do niego dostęp.

### <a id="httpclientsend"></a>Wysyłanie żądania HTTP
Metody, takie jak `HttpClient.Send` wskazuje kod oczekuje na żądania HTTP zakończyć.

### <a id="sqlcommand"></a>Operacja bazy danych
Metoda, takich jak SqlCommand.Execute oznacza, że kod czeka na zakończenie operacji bazy danych.

### <a id="await"></a>Oczekiwanie (`AWAIT\_TIME`)
`AWAIT\_TIME`Wskazuje, że kod oczekuje na zakończenie innego zadania. Zwykle dzieje się z C# "await" instrukcji. Gdy kodu nie C# "await", wątek cofa i zwraca sterowania do puli wątków i jest nie wątku, który jest zablokowana i czeka na "await" zakończyć. Jednak logicznie wątku, który został await jest "zablokowane" Oczekiwanie na ukończenie tej operacji. `AWAIT\_TIME` Wskazuje zablokowanych czasu oczekiwania na zakończenie zadania.

### <a id="block"></a>Czas blokowania
`BLOCKED_TIME`Wskazuje, że kod jest oczekiwanie na inne zasoby, które mają być dostępne, takich jak oczekiwania na obiekt synchronizacji oczekiwania wątku dostępne lub oczekiwania na żądanie zakończyć.

### <a id="cpu"></a>Czas procesora CPU
Procesor był zajęty wykonywaniem instrukcji.

### <a id="disk"></a>Czas dysku
Aplikacja wykonuje operacje dysku.

### <a id="network"></a>Czas sieci
Aplikacja jest przeprowadzanie operacji sieciowych.

### <a id="when"></a>Gdy kolumny
Jest to wizualizację jak włącznie przykłady zbieranych dla węzła w zależności od wraz z upływem czasu. Łączny zakres żądania jest podzielony na 32 przedziałów czasu i włącznie przykłady dla tego węzła jest zebranych w tych zasobników 32. Każdy zasobnika następnie jest reprezentowany jako pasek, którego wysokość reprezentuje skalowaną wartość. Dla węzłów oznaczone `CPU_TIME` lub `BLOCKED_TIME`, lub w przypadku relacji oczywiste korzystanie z zasobów (procesora, dysku, wątek), pasek reprezentuje korzystanie z jednego z tych zasobów w okresie czasu tego zasobnika. Dla tych metryk możesz uzyskać większa niż 100% przez korzystanie z wielu zasobów. Na przykład jeśli średnio Użyj dwa procesory przedziałach, możesz uzyskać 200%.


## <a id="troubleshooting"></a>Rozwiązywanie problemów

### <a name="too-many-active-profiling-sessions"></a>Zbyt wiele aktywnych sesji profilowania

Obecnie można włączyć profilera na maksymalnie 4 aplikacji sieci Web platformy Azure i uruchomiona na tym samym planu usługi miejsc wdrożenia. Jeśli widzisz profilera zadanie sieci web raportowania zbyt wiele aktywnych sesji profilowania, należy przenieść niektórych aplikacji sieci Web do planu innej usługi.

### <a name="how-can-i-know-whether-application-insights-profiler-is-running"></a>Jak sprawdzić, czy działa profilera usługi Application Insights?

Profiler jest uruchamiany jako zadanie web ciągły w aplikacji sieci Web. Można otworzyć zasobów aplikacji sieci Web w https://portal.azure.com i sprawdzić stan "ApplicationInsightsProfiler" w bloku zadań Webjob. Jeśli nie jest uruchomiony, otwórz **dzienniki** Aby dowiedzieć się więcej.

### <a name="why-cant-i-find-any-stack-examples-even-though-the-profiler-is-running"></a>Dlaczego nie można odnaleźć przykładami stosu, mimo że profiler jest uruchomiona?

Oto kilka rzeczy, które można sprawdzić.

1. Upewnij się, że sieci Web planu usługi aplikacji jest warstwy podstawowa lub nowszym.
2. Upewnij się, aplikacja sieci Web ma Application Insights SDK 2.2 Beta i powyżej włączyć.
3. Upewnij się, że aplikacja sieci Web ma ustawienie APPINSIGHTS_INSTRUMENTATIONKEY mających taki sam klucz Instrumentacji używane przez zestaw SDK usługi Application Insights.
4. Upewnij się, że aplikacja sieci Web jest uruchomiona na platformie .net Framework 4.6.
5. Jeśli jest to aplikacja platformy ASP.NET Core, Sprawdź również, czy [wymaganych zależności](#aspnetcore).

Po rozpoczęciu profilera istnieje okres rozgrzewania krótkich po profiler zbiera aktywnie kilka śledzenia wydajności. Po wykonaniu tej profilera zbieranie śladów wydajności przez dwie minuty w co godzinę.  

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Został przy użyciu profilera usługi Azure. Co się stało go?  

Po włączeniu Application Insights profilera agenta profilera usługi Azure jest wyłączona.

### <a id="double-counting"></a>Zliczanie w równoległych wątków o podwójnej precyzji

W niektórych przypadkach w podglądzie stosu Metryka całkowity czas jest większy niż rzeczywisty czas trwania żądania.

Może się to zdarzyć, gdy istnieją dwa lub więcej wątków skojarzony z żądaniem, równoległe. Czas całkowita liczba wątków następnie jest większa niż czas, który upłynął. W wielu przypadkach jeden wątek może być oczekiwanie na innych, aby zakończyć. Podgląd komunikatów o próbuje wykryć i Pomiń postrzegać czas oczekiwania, ale errs boku przedstawiający zbyt dużo zamiast pominięcie, co może być ważnych informacji.  

Po wyświetleniu równoległych wątków w dane śledzenia, należy określić, które wątków oczekujących, dzięki czemu można określić ścieżkę krytyczną dla żądania. W większości przypadków szybko przechodzi w stan oczekiwania wątku jest po prostu oczekiwanie na inne wątki. Skoncentrować się na inne i Ignoruj, czas w wątków oczekujących.

### <a id="issue-loading-trace-in-viewer"></a>Nie danych profilowania

1. Jeśli dane, które próbujesz wyświetlić, jest starsza niż kilka tygodni, spróbuj ograniczyć czas filtru i spróbuj ponownie.

2. Sprawdź, czy serwery proxy lub zapora nie zablokowane dostęp do https://gateway.azureserviceprofiler.net.

3. Sprawdź, czy klucz Instrumentacji usługi Application Insights, używanym w aplikacji jest taka sama jak zasób usługi Application Insights, do którego włączono profilowanie z. Klucz jest zazwyczaj w ApplicationInsights.config, ale można także znaleźć w pliku web.config lub app.config.

### <a name="error-report-in-the-profiling-viewer"></a>Raport o błędach w podglądzie profilowania

Plik biletu pomocy technicznej z portalu. Podaj identyfikator korelacji z komunikat o błędzie.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Błąd wdrażania katalogu nie jest pusty "D:\\macierzystego\\lokacji\\wwwroot\\App_Data\\zadań

Jeśli są ponownego wdrażania aplikacji sieci web do zasobu usługi aplikacji przy użyciu profilera włączone, może wystąpić błąd podobny do następującego: katalog nie jest pusty "D:\\macierzystego\\lokacji\\wwwroot\\App_Data\\zadania ten błąd będzie występować po uruchomieniu narzędzia Web Deploy ze skryptów lub w potoku wdrożenia usługi VSTS.
Dodaj następujące parametry dodatkowe wdrożenia do zadań narzędzia Web Deploy jest rozwiązanie tego problemu:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Spowoduje to usunięcie folder używany przez Profiler Insights aplikacji i odblokować proces ponownego wdrażania. Nie wpłynie na aktualnie uruchomione wystąpienie profilera.


## <a name="manual-installation"></a>Instalacja ręczna

Po skonfigurowaniu profilera ustawień aplikacji sieci Web składają się następujące aktualizacje. Można wykonać je samodzielnie ręcznie, jeśli dane środowisko wymaga na przykład, jeśli aplikacja działa w środowisku usługi aplikacji Azure (ASE):

1. Otwórz ustawienia w bloku kontroli aplikacja sieci web.
2. Ustaw ".Net Framework w wersji" Aby 4.6.
3. Ustaw wartość "Zawsze włączone" włączone.
4. Dodaj ustawienie aplikacji "__APPINSIGHTS_INSTRUMENTATIONKEY__" i ustaw wartość na tym samym kluczem Instrumentacji używane przez zestaw SDK.
5. Otwieranie zaawansowanych narzędzi.
6. Kliknij przycisk "Przejdź", aby otworzyć program Kudu witryny sieci Web.
7. W witrynie internetowej Kudu wybierz "Lokacji rozszerzenia".
8. Zainstaluj "__usługi Application Insights__" z galerii.
9. Uruchom ponownie aplikację sieci web.

## <a id="aspnetcore"></a>Obsługa platformy ASP.NET Core

Aplikacja ASP.NET Core, należy zainstalować 2.1.0-beta6 pakietu Microsoft.ApplicationInsights.AspNetCore Nuget lub nowszej, aby pracować z profilera. Nie obsługujemy niższych wersjach po 2017-6/27.

## <a name="next-steps"></a>Następne kroki

* [Praca z usługi Application Insights w programie Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[performance-blade-v2-examples]:./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png
