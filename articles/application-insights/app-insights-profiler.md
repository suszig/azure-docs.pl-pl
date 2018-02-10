---
title: "Profil aplikacji sieci web na platformie Azure za pomocą profilera Insights aplikacji | Dokumentacja firmy Microsoft"
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
ms.date: 02/08/2018
ms.author: mbullwin
ms.openlocfilehash: 80792a82adbb93e80c94b4829b704b70d2a8ed23
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Profil aplikacji sieci web platformy Azure na żywo za pomocą usługi Application Insights

*Ta funkcja usługi Application Insights jest ogólnie dostępna w usłudze Azure App Service i w zasoby obliczeniowe systemu Azure w wersji zapoznawczej.*

Sprawdzić, ile jest czas w każdej metodzie aplikacji sieci web na żywo przy użyciu [usługi Application Insights](app-insights-overview.md). Narzędzia profilowania w usłudze Application Insights zamieszczono szczegółowe profilów na żywo żądań, które zostały obsłużone przez aplikację i zaznacza *aktywnej ścieżki* używającą najwięcej czasu. Żądania z czasy odpowiedzi różnych są profilowane na podstawie próbki. Koszty związane z aplikacji jest zminimalizowany, za pomocą różnych technik.

Profiler jest obecnie obsługiwane dla aplikacji sieci web ASP.NET i ASP.NET Core, systemem w usłudze Azure App Service. **Podstawowe** warstwę usługi lub nowszego jest wymagany do użycia profilera.

## <a id="installation"></a>Włącz profilera dla aplikacji sieci Web usługi aplikacji
Jeśli już aplikacja opublikowana w usługach aplikacji, ale nie wykonano żadnych czynności w kodzie źródłowym, aby móc używać usługi Application Insights, przejdź do okienka usługi aplikacji w portalu Azure, przejdź do **monitorowanie | Usługa Application Insights**, postępuj zgodnie z instrukcjami w okienku, aby utworzyć nowy zasób, lub wybierz istniejący zasób usługi Application Insights do monitorowania aplikacji sieci Web.

![Włącz Insights aplikacji w portalu usługi aplikacji][appinsights-in-appservices]

Jeśli masz dostęp do kodu źródłowego projektu, [instalacji usługi Application Insights](app-insights-asp-net.md). Jeśli jest już zainstalowany, upewnij się, że masz najnowszą wersję. Aby wyszukać najnowszą wersję, w Eksploratorze rozwiązań kliknij projekt prawym przyciskiem myszy, a następnie wybierz **pakiety zarządzania pakietami NuGet** > **aktualizacje** > **Aktualizuj wszystkie pakiety**. Następnie można wdrożyć aplikację.

Aplikacje platformy ASP.NET Core wymagają zainstalowania 2.1.0-beta6 pakietu Microsoft.ApplicationInsights.AspNetCore NuGet lub nowszej do pracy z profilera. Począwszy od 27 czerwca 2017 wcześniejszych wersji nie są obsługiwane.

W [portalu Azure](https://portal.azure.com), otwórz zasobu usługi Application Insights dla aplikacji sieci web. Wybierz **wydajności** > **włączyć Application Insights profilera**.

![Zaznacz opcję Włącz transparent profilera][enable-profiler-banner]

Alternatywnie można wybrać **profilera** konfigurację, aby wyświetlić stan i włączanie lub wyłączanie profilera.

![W obszarze wydajność wybierz konfigurację profilera][performance-blade]

Aplikacje sieci Web, które są skonfigurowane przy użyciu usługi Application Insights są wymienione w **profilera** okienko konfiguracji. Jeśli kroki powyżej zostały wykonane, powinien już zainstalowany agent profilera. Wybierz **włączyć profilera** w **profilera** okienko konfiguracji.

Wykonaj instrukcje, aby zainstalować agenta profilera, w razie potrzeby. Jeśli żadne aplikacje sieci web zostały skonfigurowane przy użyciu usługi Application Insights, wybierz **Dodaj aplikacje połączone**.

![Skonfiguruj opcje okienka][linked app services]

W przeciwieństwie do aplikacji sieci web, które znajdują się za pośrednictwem planów usługi App Service aplikacje, które znajdują się w zasoby obliczeniowe systemu Azure (na przykład, maszynach wirtualnych platformy Azure, zestawy skalowania maszyny wirtualnej, sieć szkieletowa usług Azure lub usługi w chmurze Azure) nie są bezpośrednio zarządzane przez usługę Azure. W takim przypadku jest żadnej aplikacji sieci web, aby połączyć. Zamiast łącze do aplikacji, wybierz **włączyć profilera** przycisku.

### <a name="enable-the-profiler-for-azure-compute-resources-preview"></a>Włącz profilera dla zasobów obliczeniowych Azure (wersja zapoznawcza)

Uzyskiwanie informacji [wersji zapoznawczej profilera dla zasobów obliczeniowych Azure](https://go.microsoft.com/fwlink/?linkid=848155).

## <a name="view-profiler-data"></a>Wyświetlanie danych profilera

**Upewnij się, że aplikacja odbiera ruch.** Jeśli przeprowadzasz eksperyment, można wygenerować żądania przy użyciu aplikacji sieci Web [test wydajności, Application Insights](https://docs.microsoft.com/en-us/vsts/load-test/app-service-web-app-performance-test). Włączenie nowo profilera można uruchomić testu obciążenia krótkich przez 15 minut, które powinien wygenerować ślady profilera. Gdyby profilera już włączone przez pewien czas uruchamia keep pamiętać, że Profiler losowo uruchamia dwa razy co godzinę i czas trwania dwie minuty każdej aktualizacji. Zalecamy najpierw uruchamiania testu obciążenia przez jedną godzinę do upewnij się, że możesz uzyskać przykładowe ślady profilera.

Gdy aplikacja odbiera część ruchu, przejdź do **wydajności** bloku > **podjąć akcje** Aby wyświetlić profilera śladów. Wybierz **ślady profilera** przycisku.

![Ślady profilera aplikacji wydajności wgląd w okienku podglądu][performance-blade-v2-examples]

Wybierz przykład do pokazywania podział poziomie kodu czas wykonania żądania.

![Eksplorator śledzenia usługi Application Insights][trace-explorer]

W Eksploratorze śledzenia zawiera następujące informacje:

* **Pokaż ścieżkę aktywną**: Otwiera pośrednictwem powiązanych punktów sprzedaży liść węzła lub co najmniej coś zamknąć. W większości przypadków ten węzeł jest sąsiadujące wąskie gardło.
* **Etykieta**: nazwę funkcji lub zdarzeń. Drzewo zawiera zarówno kod i zdarzenia (takie jak zdarzenia SQL i HTTP). Górny zdarzeń reprezentuje całkowity czas trwania żądania.
* **Upłynął**: interwał czasu rozpoczęcia operacji i koniec operacji.
* **Gdy**: po funkcji lub zdarzeń była uruchomiona w odniesieniu do innych funkcji.

## <a name="how-to-read-performance-data"></a>Jak można odczytać danych wydajności

Profilera usługi firmy Microsoft używa kombinacji metod próbkowania i instrumentacji do analizowania wydajności aplikacji. Szczegółowe kolekcji jest w toku, profilera usługi przykłady wskaźnik instrukcji poszczególnych procesorów na komputerze co milisekund. Każda próbka przechwytuje stosu wywołań zakończenie wątku, który jest aktualnie wykonywany. Udostępnia szczegółowe informacje na temat tego wątku został czynności, zarówno na wysokim poziomie, jak i na niskim poziomie abstrakcji. Usługa profiler zbiera również inne zdarzenia, aby śledzić korelacji działania i przyczynowości, łącznie z kontekstem przełączania zdarzenia, zdarzenia zadania biblioteki równoległych (TPL) oraz zdarzenia puli wątków.

Stos wywołań, który jest wyświetlany w widoku osi czasu jest wynikiem próbkowania i instrumentacji. Ponieważ każdy przykład przechwytuje stosu wywołań zakończenie wątku, zawiera kod z programu Microsoft .NET Framework i innych platform, które możesz odwoływać się do.

### <a id="jitnewobj"></a>Obiekt alokacji (clr! JIT\_nowy lub clr! JIT\_Newarr1)
**CLR! JIT\_nowy** i **clr! JIT\_Newarr1** są funkcje pomocnicze w programie .NET Framework, które przydzielić pamięci ze sterty zarządzanej. **CLR! JIT\_nowy** jest wywoływane, gdy obiekt jest przydzielony. **CLR! JIT\_Newarr1** jest wywoływane, gdy przydzielone jest Tablica obiektów. Te dwie funkcje są zwykle szybkie i podjąć względnie niewielkich ilości czasu. Jeśli widzisz **clr! JIT\_nowy** lub **clr! JIT\_Newarr1** trwać dość czasu na osi czasu, to wskazanie, czy kod może być Alokacja wiele obiektów i wykorzystywanie znacznych ilości pamięci.

### <a id="theprestub"></a>Kod ładujący (clr! ThePreStub)
**CLR! ThePreStub** jest funkcją pomocnika w programie .NET Framework, który przygotowuje kod do wykonania po raz pierwszy. To zwykle obejmuje, ale nie jest ograniczona do kompilacji just-in-time (JIT). W przypadku każdego C# metody **clr! ThePreStub** powinna być wywoływana co najwyżej raz przez cały okres istnienia procesu.

Jeśli **clr! ThePreStub** trwa dość czasu dla żądania, oznacza to, że żądanie jest pierwszą, która wykonuje tej metody. Czas dla środowiska uruchomieniowego .NET Framework załadować pierwsza metoda jest znacząca. Należy rozważyć przy użyciu procesu rozgrzewania, który wykonuje część kod przed użytkowników do niego dostęp, lub rozważ użycie polecenia Generator obrazu natywnego (ngen.exe) z zestawów.

### <a id="lockcontention"></a>Zablokuj rywalizacji (clr! JITutil\_MonContention lub clr! JITutil\_MonEnterWorker)
**CLR! JITutil\_MonContention** lub **clr! JITutil\_MonEnterWorker** wskazuje, że bieżący wątek oczekuje na zwolnienie blokady. To zwykle zostaną wyświetlone podczas wykonywania C# **blokady** instrukcji, podczas wywoływania **Monitor.Enter** metody, lub podczas wywoływania metody z **MethodImplOptions.Synchronized**atrybutu. Rywalizacji blokad zwykle występuje, gdy wątek _A_ uzyskuje blokady i wątku _B_ próbuje uzyskać blokady tego samego przed wątku _A_ zwolnieniem.

### <a id="ngencold"></a>Kod ładujący ([chłodni])
Jeśli nazwa metody zawiera **[ZIMNYCH]**, takich jak **mscorlib.ni! [COLD]system.Reflection.CustomAttribute.IsDefined**, środowiska uruchomieniowego .NET Framework jest wykonywanie kodu po raz pierwszy, który nie jest zoptymalizowana przez <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">Optymalizacja sterowana profilem</a>. Dla każdej metody go powinny być widoczne co najwyżej raz przez cały okres istnienia procesu.

Jeśli kod ładowania przyjmuje rozległe czas na żądanie, oznacza to, że żądanie jest pierwsza z nich można wykonać zoptymalizowanego część metody. Należy rozważyć użycie procesem rozgrzewania część kodu jest wykonywana przed użytkowników do niego dostęp.

### <a id="httpclientsend"></a>Wysyłanie żądania HTTP
Metod, takich jak **HttpClient.Send** wskazują, że kod czeka na żądania HTTP.

### <a id="sqlcommand"></a>Operacja bazy danych
Metod, takich jak **SqlCommand.Execute** wskazują, że kod czeka na zakończenie operacji bazy danych.

### <a id="await"></a>Oczekiwanie (AWAIT\_czasu)
**AWAIT\_czasu** wskazuje, że kod czeka na zakończenie innego zadania. Dzieje się to zwykle z języka C# **AWAIT** instrukcji. Jeśli kod jest C# **AWAIT**, wątek cofa i zwraca sterowania do puli wątków i nie nie wątku, który jest zablokowana i czeka na **AWAIT** aby zakończyć. Jednak logicznie, wątek który zostało **AWAIT** "zablokowaniu" i oczekuje na zakończenie operacji. **AWAIT\_czasu** instrukcji wskazuje zablokowanych czasu oczekiwania na zakończenie zadania.

### <a id="block"></a>Czas blokowania
**BLOCKED_TIME** wskazuje, czy kod jest oczekiwanie na inne zasoby, które mają być dostępne. Na przykład może być oczekiwanie obiektu synchronizacji, wątek był dostępny lub do zakończenia żądania.

### <a id="cpu"></a>Czas procesora CPU
Procesor był zajęty wykonywaniem instrukcji.

### <a id="disk"></a>Czas dysku
Aplikacja wykonuje operacje dysku.

### <a id="network"></a>Czas sieci
Aplikacja jest przeprowadzanie operacji sieciowych.

### <a id="when"></a>Gdy kolumny
**Podczas** kolumna jest wizualizację jak włącznie przykłady zbieranych dla węzła w zależności od wraz z upływem czasu. Łączny zakres żądania jest podzielona na 32 przedziałów czasu. Wraz z wartościami granicznymi przykłady dla tego węzła jest zebranych w tych zasobników 32. Zasobnik każdy jest reprezentowany jako pasek. Wysokość paska reprezentuje skalowaną wartość. Dla węzłów oznaczone **CPU_TIME** lub **BLOCKED_TIME**, lub w przypadku relacji oczywiste korzystanie z zasobów (Procesora, dysku, wątek), pasek reprezentuje korzystanie z jednego z tych zasobów w czasie Czas ten zasobnika. Dla tych metryk jest można uzyskać wartość większa niż 100% przez korzystanie z wielu zasobów. Korzystając z średnio dwa procesory przedział czasu, na przykład get 200%.

## <a name="limitations"></a>Ograniczenia

Przechowywanie danych domyślny jest pięć dni. Maksymalna danych pozyskanych na dzień wynosi 10 GB.

Nie ma żadnych opłat za korzystanie z usługi profilera. Aby korzystać z usługi profilera, aplikacji sieci web musi być obsługiwana w warstwie podstawowa usługi Azure App Service.

## <a name="overhead-and-sampling-algorithm"></a>Koszty i algorytm pobierania próbek

Profiler losowo uruchamia dwie minuty co godzinę na każdej maszynie wirtualnej hostującego aplikację, która ma włączone do przechwytywania danych śledzenia profilera. Uruchamiając profilera, dodaje między 5 – 15% obciążenie Procesora na serwerze.
Więcej serwerów, które są dostępne do hostowania aplikacji, mniej wpływ profilera ma na ogólną wydajnością. Jest to spowodowane powoduje algorytm próbkowania profilera systemem jedynie 5% serwerów w dowolnym momencie. Więcej serwerów dostępnych do obsługi żądań sieci web do przesunięcia spowodowane przez uruchomione profilera obciążenie serwera.

## <a name="disable-the-profiler"></a>Wyłączanie profilera
Zatrzymaj lub ponownego uruchomienia profilera dla poszczególnych wystąpień usługi aplikacji, w obszarze **zadania Web Job**, przejdź do zasobu usługi aplikacji. Aby usunąć profilera, przejdź do **rozszerzenia**.

![Wyłącz program profilujący do zadania sieci web][disable-profiler-webjob]

Zaleca się, że profiler włączona na wszystkich aplikacji sieci web, aby odnaleźć wszystkie problemy z wydajnością tak szybko jak to możliwe.

Pakiet WebDeploy można używać do wdrażania zmian do aplikacji sieci web, upewnij się, Wyklucz w folderze App_Data przed usunięciem podczas wdrażania. W przeciwnym razie rozszerzenia profilera pliki zostaną usunięte przy następnym możesz wdrożyć aplikację sieci web na platformie Azure.


## <a id="troubleshooting"></a>Rozwiązywanie problemów

### <a name="too-many-active-profiling-sessions"></a>Zbyt wiele aktywnych sesji profilowania

Obecnie można włączyć profilera na maksymalnie cztery aplikacji sieci web platformy Azure i miejsc wdrożenia, które działają w tym samym planie usługi. Jeśli zadanie sieci web profilera zgłoszenie zbyt wiele aktywnych sesji profilowania, Przenieś niektórych aplikacji sieci web do planu innej usługi.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Jak ustalić, czy Application Insights Profiler jest uruchomiona?

Profiler jest uruchamiany jako zadanie web ciągły w aplikacji sieci web. Można otworzyć zasobu aplikacji sieci web w [portalu Azure](https://portal.azure.com). W **Webjob** okienka, sprawdź stan **ApplicationInsightsProfiler**. Jeśli nie jest uruchomiony, otwórz **dzienniki** Aby uzyskać więcej informacji.

### <a name="why-cant-i-find-any-stack-examples-even-though-the-profiler-is-running"></a>Dlaczego nie można odnaleźć przykładami stosu, mimo że profiler jest uruchomiona?

Oto kilka rzeczy, które można sprawdzić:

* Upewnij się, że planu usługi aplikacji sieci web jest warstwy podstawowa lub nowszej.
* Upewnij się, że aplikacja sieci web ma Application Insights SDK 2.2 Beta lub później włączone.
* Upewnij się, że aplikacja sieci web ma **APPINSIGHTS_INSTRUMENTATIONKEY** ustawieniem skonfigurowanym mających taki sam klucz instrumentacji, która jest używana przez zestaw SDK usługi Application Insights.
* Upewnij się, że aplikacja sieci web jest uruchomiona na .NET Framework 4.6.
* Sprawdź, czy aplikacja platformy ASP.NET Core aplikacji sieci web, [wymaganych zależności](#aspnetcore).

Po rozpoczęciu profiler jest okres rozgrzewania krótkie, podczas którego profiler zbiera aktywnie kilka śledzenia wydajności. Po wykonaniu tej profilera zbiera dane śledzenia wydajności dla dwóch minut co godzinę.

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Został przy użyciu profilera usługi Azure. Co się stało go?

Po włączeniu Application Insights profilera agenta profilera usługi Azure jest wyłączona.

### <a id="double-counting"></a>Zliczanie w równoległych wątków o podwójnej precyzji

W niektórych przypadkach w podglądzie stosu Metryka całkowity czas jest większy niż czas trwania żądania.

Taka sytuacja może wystąpić, gdy istnieją dwa lub więcej wątków skojarzone z żądaniem i działają równolegle. W takim przypadku wątku całkowity czas jest większy niż czas, który upłynął. Jeden wątek może być oczekiwanie na drugi, należy wykonać. Podgląd próbuje wykryć i pomija postrzegać czas oczekiwania, ale jego errs boku przedstawiający zbyt dużo zamiast pominięcie, co może być ważnych informacji.

Po wyświetleniu równoległych wątków w dane śledzenia należy ustalić, które wątków oczekujących, można ustalić ścieżkę krytyczną dla żądania. W większości przypadków szybko przechodzi w stan oczekiwania wątku jest po prostu oczekiwanie na inne wątki. Skoncentrować się na inne wątki i Ignoruj, czas w wątków oczekujących.

### <a id="issue-loading-trace-in-viewer"></a>Nie danych profilowania

Oto kilka rzeczy, które można sprawdzić:

* Jeśli dane, które próbujesz wyświetlić, jest starsza niż kilka tygodni, spróbuj ograniczyć czas filtru i spróbuj ponownie.
* Sprawdź, czy serwery proxy lub zapora nie zablokowane dostęp do https://gateway.azureserviceprofiler.net.
* Sprawdź, czy klucz Instrumentacji usługi Application Insights, używanym w aplikacji jest taka sama jak zasobu usługi Application Insights, który został użyty do profilowania włączone. Klucz jest zwykle ApplicationInsights.config, ale może znajdować się w plikach web.config lub app.config.

### <a name="error-report-in-the-profiling-viewer"></a>Raport o błędach w podglądzie profilowania

Przedstawia biletu pomocy technicznej w portalu. Należy uwzględnić identyfikator korelacji z komunikat o błędzie.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Błąd wdrażania: katalog nie jest pusty "D:\\macierzystego\\lokacji\\wwwroot\\App_Data\\zadań

Jeśli są ponownego wdrażania aplikacji sieci web do zasobu usługi App Service przy użyciu profilera włączone, można napotkać komunikat, który wygląda następująco:

Katalog nie jest pusty "D:\\macierzystego\\lokacji\\wwwroot\\App_Data\\zadań

Ten błąd występuje podczas uruchamiania narzędzia Web Deploy ze skryptów lub z potoku wdrożenia usług Team w usłudze Visual Studio. Rozwiązanie to dodaj następujące parametry dodatkowe wdrożenia do zadań narzędzia Web Deploy:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Te parametry Usuń folder, który jest używany przez Profiler Insights aplikacji i odblokować proces ponownego wdrażania. Nie wpływają one na wystąpienie profilera, które jest aktualnie uruchomione.


## <a name="manual-installation"></a>Instalacja ręczna

Po skonfigurowaniu profilera uaktualnienia odnoszą się do ustawień aplikacji sieci web. Należy ręcznie zastosować aktualizacje, jeśli dane środowisko wymaga go. Na przykład, jeśli aplikacja działa w środowisku usługi aplikacji dla rozwiązania PowerApps.

1. W Panelu sterowania aplikacji sieci web, należy otworzyć **ustawienia**.
2. Ustaw **.Net Framework w wersji** do **4.6**.
3. Ustaw **zawsze włączone** do **na**.
4. Dodaj __APPINSIGHTS_INSTRUMENTATIONKEY__ aplikacji, ustawienia i ustaw wartość do tego samego klucza instrumentacji, która jest używana przez zestaw SDK.
5. Otwórz **zaawansowane narzędzia**.
6. Wybierz **Przejdź** otworzyć Kudu witryny sieci Web.
7. W witrynie sieci Web Kudu, wybierz **lokacji rozszerzenia**.
8. Zainstaluj __usługi Application Insights__ z galerii aplikacji sieci Web platformy Azure.
9. Uruchom ponownie aplikację sieci web.

## <a id="profileondemand"></a>Ręcznie uruchomić profilera
Podczas opracowywania profiler, dodane interfejsu wiersza polecenia, aby firma Microsoft można testować profilera na usługi aplikacji. Przy użyciu tych tych samych użytkowników interface można również dostosować sposób uruchamiania profilera. Na wysokim poziomie profilera używa systemu Kudu usługi aplikacji do zarządzania profilowania w tle. Po zainstalowaniu rozszerzenie usługi Application Insights, utworzymy zadania ciągłego sieci web, które obsługuje profilera. Używamy tej samej technologii, aby utworzyć nowe zadanie sieci web, którą można dostosować do własnych potrzeb.

W tej sekcji opisano sposób:

1. Utwórz zadanie sieci web, które można uruchomić profilera dwie minuty o naciśnięcie przycisku.
2. Utwórz zadanie sieci web, które można zaplanować profiler do uruchomienia.
3. Ustaw argumentów profilera.


### <a name="set-up"></a>Konfigurowanie
Pierwszy umożliwia zapoznanie się z pulpitu nawigacyjnego zadanie sieci web. W obszarze Ustawienia kliknij na karcie zadań Webjob.

![Blok zadań webjob](./media/app-insights-profiler/webjobs-blade.png)

Jak widać, że ten pulpit nawigacyjny pokazuje wszystkie zadania sieci web, które są aktualnie zainstalowane w witrynie. Widać ApplicationInsightsProfiler2 zadanie sieci web, który ma uruchomione zadanie profilera. Jest to, gdzie możemy przechodzili tworzenie naszego nowego zadania web Job ręczne i zaplanowane profilowania.

Pierwszy Załóż potrzebujemy plików binarnych.

1.  Przejdź do witryny Kudu. Na karcie Narzędzia Programowanie kliknij kartę "Zaawansowane narzędzia" Kudu logo. Kliknij przycisk "Przejdź". Przejście do nowej lokacji i automatycznie loguje.
2.  Następnie należy pobierania plików binarnych profilera. Przejdź do Eksploratora plików za pomocą konsoli Debuguj -> CMD znajdujący się w górnej części strony.
3.  Kliknij w lokacji -> wwwroot -> App_Data -> zadania -> ciągłe. Folder "ApplicationInsightsProfiler2" powinna zostać wyświetlona. Kliknij ikonę pobierania z lewej strony folderu. This downloads "ApplicationInsightsProfiler2.zip" file.
4.  Spowoduje to pobranie wszystkich plików, które są potrzebne. Najlepiej tworzenie wyczyścić katalogu można przenieść tego archiwum zip do zmiany przed kontynuowaniem.

### <a name="setting-up-the-web-job-archive"></a>Konfigurowanie zadania archiwum sieci web
Podczas zasadniczo dodać nowe zadanie sieci web do witryny sieci Web platformy azure, możesz utworzyć archiwum zip z run.cmd wewnątrz. Run.cmd informuje system zadanie sieci web, co należy zrobić, gdy zostanie uruchomione zadanie sieci web.

1.  Aby uruchomić Utwórz nowy folder, w naszym przykładzie nosi nazwę "RunProfiler2Minutes".
2.  Skopiuj pliki z folderu ApplicationInsightProfiler2 wyodrębnione do tego nowego folderu.
3.  Utwórz nowy plik run.cmd. (Można otworzyć ten folder roboczy w kodzie VS przed rozpoczęciem jako udogodnienie.)
4.  Dodaj polecenie `ApplicationInsightsProfiler.exe start --engine-mode immediate --single --immediate-profiling-duration 120`i Zapisz plik.
a.  `start` Polecenie informuje profiler do uruchomienia.
b.  `--engine-mode immediate`profilera informuje, że chcemy natychmiast uruchomić profilowania.
c.  `--single`oznacza, że aby uruchomić i zatrzymać następnie automatycznie d.  `--immediate-profiling-duration 120`oznacza, że ma profilera uruchomione przez 120 sekund lub 2 minuty.
5.  Zapisz ten plik.
6.  Archiwum tego folderu, można kliknij folder prawym przyciskiem myszy i wybierz polecenie Wyślij do -> folderu skompresowane. Spowoduje to utworzenie pliku .zip przy użyciu nazwy folderu.

![Uruchom polecenia profilera](./media/app-insights-profiler/start-profiler-command.png)

Mamy teraz .zip zadanie sieci web, które możemy użyć, aby skonfigurować zadania sieci web w witrynie.

### <a name="add-a-new-web-job"></a>Dodaj nowe zadanie sieci web
Następnie możemy dodać nowe zadanie sieci web w witrynie. Ten przykład przedstawia sposób dodawania zadanie ręcznie wyzwalanych sieci web. Po można to zrobić proces jest prawie dokładnie takie same w harmonogramie.

1.  Przejdź do pulpitu nawigacyjnego zadania sieci web.
2.  Kliknij polecenie Dodaj z paska narzędzi.
3.  Nadaj nazwę zadania. Dla jasności może pomóc odpowiadać nazwie archiwum i otwarcie go dla różnych wersji systemu run.cmd.
4.  W pliku przekazywanie części formularza, kliknij ikonę otwartego pliku i Znajdź plik zip utworzonych powyżej.
5.  W przypadku tego typu wybierz Triggered.
6.  Wyzwalacze wybrać ręczny.
7.  Kliknij przycisk OK, aby zapisać.

![Uruchom polecenia profilera](./media/app-insights-profiler/create-webjob.png)

### <a name="run-the-profiler"></a>Uruchom profilera

Teraz, gdy mamy nowe zadanie sieci web, który można ręcznie wyzwalana spróbujemy można go uruchomić.

1. Zgodnie z projektem może mieć tylko jeden proces ApplicationInsightsProfiler.exe uruchomione na komputerze w danym momencie. Tak, aby rozpocząć, pamiętaj wyłączyć zadania ciągłego sieci web z tego pulpitu nawigacyjnego. Kliknij wiersz, a następnie naciśnij klawisz STOP (Zatrzymaj)". Wybierz odświeżania na pasku narzędzi i upewnij się, że stan wskazuje, że zadanie zostało zatrzymane.
2. Kliknij wiersz z nowego zadania sieci web dodanych i naciśnij przycisk Uruchom.
3. Kliknąć nadal wybranego wiersza polecenia dzienniki na pasku narzędzi spada poniżej należy do sieci web pulpitu nawigacyjnego zadań dla zadania sieci web, które zostały uruchomione. Wyświetla listę najnowszych uruchamia i ich wyników.
4. Polecenie wystąpienie wykonywania tylko uruchomienia.
5. Jeśli wszystkie poszło dobrze, powinny pojawić się niektórych dzienników diagnostycznych pochodzące z profilera potwierdzenie, że zostały uruchomione, profilowania.

### <a name="things-to-consider"></a>Co należy wziąć pod uwagę

Chociaż ta metoda jest stosunkowo prosta, jest kilka rzeczy, należy wziąć pod uwagę.

- Ponieważ to nie jest zarządzana przez naszych usług, firma Microsoft nie ma możliwości aktualizowanie plików binarnych agenta dla zadania sieci web. Firma Microsoft aktualnie nie masz strony pobierania stabilna dla naszych danych binarnych więc jedynym sposobem uzyskania najnowszej aktualizacji rozszerzenia i dane z folderu ciągłego, tak jak opisano w poprzednich krokach.

- Ponieważ to jest przy użyciu argumentów wiersza polecenia, które pierwotnie zostały zaprojektowane dla deweloperów użyć zamiast używana przez użytkownika końcowego, argumenty może zmiany w przyszłości, więc po prostu należy pamiętać o który podczas uaktualniania. Nie należy go znacznie problem, ponieważ użytkownik może dodać zadanie sieci web, uruchamianie i test, który działa. Po pewnym czasie zostaną budujemy interfejsu użytkownika do obsługi to bez ręczny proces.

- Funkcja zadania sieci Web dla usług aplikacji jest unikatowa, że po uruchomieniu zadania sieci web gwarantuje, że proces ma tego samego zmienne środowiskowe i ustawienia aplikacji, które witryny sieci web zakończą się o. Oznacza to, że nie należy do przekazania klucza Instrumentacji z wiersza polecenia profilera. Należy go po prostu wybierz klucza instrumentacji ze środowiska. Jednak jeśli chcesz uruchomić profilera z pola deweloperów lub na komputerze poza usługi aplikacji należy podać klucz instrumentacji. Można to zrobić przez przekazywanie w argumencie `--ikey <instrumentation-key>`. Ta wartość musi być zgodna klucza instrumentacji, przez aplikację. W danych wyjściowych dziennika z profilera informuje o tym, które ikey wprowadzenie profilera i jeśli wykryliśmy działania z tego klucza Instrumentacji podczas możemy są profilowania.

- Zadania ręczne wyzwalanych sieci web mogą być wyzwalane faktycznie za pośrednictwem sieci Web punktu zaczepienia. Ten adres url można uzyskać przez kliknięcie prawym przyciskiem myszy zadanie sieci web z poziomu pulpitu nawigacyjnego i wyświetlania właściwości. Lub wybierz polecenie Właściwości na pasku narzędzi po wybraniu zadanie sieci web z tabeli. Spowoduje to otwarcie nieskończone możliwości, takie jak wyzwalania profilera z planowaną CI/CD (np. programu VSTS) lub przypominać Flow firmy Microsoft (https://flow.microsoft.com/en-us/). Ostatecznie zależy od sposobu złożonych, która ma być Twojej run.cmd (która może być również run.ps1), ale istnieje już elastyczność.

## <a name="next-steps"></a>Kolejne kroki

* [Praca z usługi Application Insights w programie Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
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
