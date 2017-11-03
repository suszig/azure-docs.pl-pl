---
title: "Najlepsze rozwiązania i w podręczniku rozwiązywania problemów z węzła aplikacje aplikacji sieci Web Azure"
description: "Dowiedz się, najlepszych rozwiązań i kroki rozwiązywania problemów z węzła aplikacje na platformie Azure aplikacje sieci Web."
services: app-service\web
documentationcenter: nodejs
author: ranjithr
manager: wadeh
editor: 
ms.assetid: 387ea217-7910-4468-8987-9a1022a99bef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 06/06/2016
ms.author: ranjithr
ms.openlocfilehash: 8f39b5e6faf5f9121ec2abe347f50653c5c3e4f9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Najlepsze rozwiązania i w podręczniku rozwiązywania problemów z węzła aplikacje aplikacji sieci Web Azure
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

W tym artykule dowiesz się najlepszych rozwiązań i kroki rozwiązywania problemów z [węzła aplikacje](app-service-web-get-started-nodejs.md) systemem Azure Webapps (z [iisnode](https://github.com/azure/iisnode)).

> [!WARNING]
> Przy użyciu kroków w swojej witrynie produkcji, należy zachować ostrożność. Zalecanie służy do rozwiązywania problemów z aplikacją w Instalator nieprodukcyjnych na przykład Twoje miejsce przemieszczania, a jeśli problem został rozwiązany, zamienić Twoje miejsce wystawiania z Twojego miejsca produkcji.
> 
> 

## <a name="iisnode-configuration"></a>Konfiguracja programu IISNODE
To [pliku schematu](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) zawiera wszystkie ustawienia, które można skonfigurować dla programu iisnode. Niektóre ustawienia, które będą przydatne w przypadku aplikacji, to:

* nodeProcessCountPerApplication
  
    To ustawienie określa liczbę procesów węzła, które będą uruchamiane na aplikacji usług IIS. Wartość domyślna to 1. Można uruchomić dowolną liczbę node.exe jako liczba rdzeni sieci maszyn wirtualnych przez ustawienie to 0. Zalecana wartość to 0 dla większości aplikacji, można korzystać ze wszystkich rdzeni na tym komputerze. Node.exe jest pojedynczym wątku, maksymalnie 1 rdzeń i uzyskanie maksymalnej wydajności poza węzeł aplikacji chcesz wykorzystać wszystkie rdzenie będą korzystać z jednego node.exe.
* nodeProcessCommandLine
  
    To ustawienie określa ścieżkę do node.exe. Można ustawić tej wartości, aby wskazywał wersji node.exe.
* maxConcurrentRequestsPerProcess
  
    To ustawienie określa maksymalną liczbę jednoczesnych żądań wysłanych przez program iisnode do każdego node.exe. Na używanie usługi azure wartość domyślna to jest bez ograniczeń czasowych. Nie musisz martwić się o tym ustawieniu. Poza azure webapps wartość domyślna to 1024. Można to skonfigurować, w zależności od liczby żądań, pobiera aplikacji i tempa aplikacji przetwarzania każdego żądania.
* maxNamedPipeConnectionRetry
  
    To ustawienie określa maksymalną liczbę razy iisnode ponowi próbę połączenia wprowadzania na nazwanym potoku do wysłania żądania do node.exe. To ustawienie w połączeniu z namedPipeConnectionRetryDelay określa całkowity limit czasu każdego żądania w ramach programu iisnode. Wartość domyślna to 200 na używanie usługi Azure. Całkowity limit czasu w sekundach = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
* namedPipeConnectionRetryDelay
  
    To ustawienie określa ilość czasu (w ms) iisnode będzie oczekiwał na między kolejnymi próbami wysłać żądania do node.exe za pomocą potoku nazwanego. Wartość domyślna to 250ms.
    Całkowity limit czasu w sekundach = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
  
    Domyślnie łącznego limitu czasu w iisnode na używanie azure to 200 \* 250ms = 50 sekund.
* logDirectory
  
    To ustawienie określa, gdzie iisnode będzie rejestrować stdout/stderr katalogu. Wartość domyślna to iisnode, która jest względem katalogu głównego skryptu (gdzie występuje server.js głównego katalogu)
* debuggerExtensionDll
  
    To ustawienie określa, jakie wersji narzędzia node-inspector iisnode będzie używany podczas debugowania aplikacji węzła. Obecnie iisnode inspektora 0.7.3.dll i iisnode inspector.dll są tylko 2 prawidłowe wartości dla tego ustawienia. Wartość domyślna to iisnode inspektora 0.7.3.dll. wersja programu iisnode inspektora 0.7.3.dll używa 0.7.3-node-inspector i używa protokołu websocket, więc musisz włączyć protokół websockets na azure aplikacji sieci Web do użycia tej wersji. Zobacz <http://www.ranjithr.com/?p=98> Aby uzyskać więcej informacji na temat konfigurowania programu iisnode, aby użyć nowego narzędzia node-inspector.
* flushResponse
  
    Domyślne zachowanie usług IIS jest buforuje dane odpowiedzi na górę 4 MB przed opróżnianie lub aż do zakończenia odpowiedzi, zależnie od zostanie osiągnięty jako pierwszy. iisnode oferuje ustawienie konfiguracji, aby zmienić to zachowanie: Aby opróżnić fragment treści jednostki odpowiedzi, jak program iisnode odbiera on z node.exe, należy ustawić iisnode/@flushResponse atrybutu w pliku web.config na wartość "true":
  
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```
  
    Włączanie opróżnianie każdego fragmentu odpowiedzi treści jednostki zwiększa wydajność ograniczającą przepływności systemu % ~ 5 (począwszy od v0.1.13), dlatego najlepiej zakres to ustawienie tylko dla punktów końcowych, które wymagają odpowiedzi przesyłania strumieniowego (np. przy użyciu <location> elementu w pliku web.config)
  
    Oprócz tego do przesyłania strumieniowego aplikacji, należy również ustawić responseBufferLimit Twojego programu iisnode obsługi na 0.
  
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```
* watchedFiles
  
    Jest to rozdzielana średnikami lista plików, które będą monitorowane zmian. Zmiany w pliku powoduje, że aplikacja do odtworzenia. Każdy wpis zawiera nazwę katalogu opcjonalne oraz nazwę wymaganego pliku, które są względem katalogu, w którym znajduje się punkt wejścia aplikacji głównej. Symbole wieloznaczne są dozwolone w tylko części nazwy pliku. Wartość domyślna to "\*. js;web.config"
* recycleSignalEnabled
  
    Wartość domyślna to false. Włączenie aplikacji węzła można nawiązać połączenia nazwanego potoku (zmienną środowiskową IISNODE\_kontroli\_POTOKU) i wysłać wiadomość "odtworzenia". Spowoduje to w3wp do odtworzenia bezpiecznie zamknąć.
* idlePageOutTimePeriod
  
    Wartość domyślna to 0, co oznacza, że ta funkcja jest wyłączona. Gdy ustawiona niektórych wartość większą niż 0, iisnode będzie strony się jego procesy podrzędne co milisekund "idlePageOutTimePeriod". Aby zrozumieć, co strony limit oznacza, zapoznaj się to [dokumentacji](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). To ustawienie będzie przydatna dla aplikacji, które zużywają dużej ilości pamięci i chcesz pageout pamięci na dysk od czasu do czasu zwolnienia niektórych pamięci RAM.

> [!WARNING]
> Należy zachować ostrożność podczas włączania następujące ustawienia konfiguracji w przypadku aplikacji produkcyjnej. Zaleca się nie je włączyć w przypadku aplikacji produkcyjnej na żywo.
> 
> 

* debugHeaderEnabled
  
    Wartość domyślna to false. Jeśli ustawiono wartość true, iisnode doda HTTP odpowiedzi nagłówka iisnode debugowania do każdej odpowiedzi HTTP wysyła się, że wartość nagłówka debugowania programu iisnode jest adresem URL. Pojedynczych informacji diagnostycznych może zgromadzone analizując fragmentu adresu URL, ale jest to osiągane przez otwierania adresu URL w przeglądarce dużo lepsze wizualizacje.
* loggingEnabled
  
    To ustawienie określa rejestrowanie stdout i stderr przez program iisnode. Iisnode będzie przechwytywać stdout/stderr z procesów węzła, który jest uruchamiany i zapis w katalogu określonym w ustawieniu "logDirectory". Gdy jest włączone, aplikacja będzie można zapisywania dzienników do systemu plików i w zależności od ilości rejestrowania wykonywane przez aplikację, może mieć wpływ na wydajność.
* devErrorsEnabled
  
    Wartość domyślna to false. Gdy ustawiona na wartość true, iisnode wyświetli kod stanu HTTP i kod błędu Win32 w przeglądarce. Kod win32 będzie pomocnych w debugowaniu niektóre rodzaje problemów.
* debuggingEnabled (nie należy włączać w witrynie produkcyjnym)
  
    To ustawienie określa funkcja debugowania. Program Iisnode jest zintegrowany z narzędzia node-inspector. Po włączeniu tego ustawienia, należy włączyć debugowanie aplikacji węzła. Po włączeniu tego ustawienia program iisnode będzie układu niezbędne narzędzia node-inspector plików w katalogu "debuggerVirtualDir" na pierwsze żądanie debugowania do węzła aplikacji. Można załadować narzędzia node-inspector, wysyłając żądanie do http://yoursite/server.js/debug. Segment adresu URL debugowania można kontrolować z ustawieniem "debuggerPathSegment". Domyślne debuggerPathSegment = "debugowanie". Możesz ustawić na identyfikator GUID, na przykład, aby trudniej mają zostać wykryte przez innych użytkowników.
  
    Sprawdź [łącze](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) uzyskać więcej informacji dotyczących debugowania.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Scenariusze i zalecenia/Rozwiązywanie problemów
### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Moja aplikacja węzła jest wprowadzenie zbyt wiele wywołań wychodzących.
Wiele aplikacji warto, aby nawiązywać połączenia wychodzące w ramach ich regularnych operacji. Na przykład gdy nadejdzie żądanie, węzeł aplikacji warto, aby skontaktować się z interfejsu API REST w innym miejscu i niektóre informacje do przetwarzania żądania. Czy chcesz użyć agenta keep alive wywołania protokołu http lub https. Na przykład można użyć modułu programu agentkeepalive jako keep alive agenta, podczas wprowadzania tych wywołań wychodzących. Dzięki temu, że w sieci azure aplikacji sieci Web maszyny Wirtualnej nie są ponownie gniazdami i zmniejsza obciążenie związane z tworzenia nowych gniazd dla każdego żądania wychodzącego. Ponadto dzięki temu się, że używasz mniejszą liczbę gniazd, aby wprowadzić wiele żądań wychodzących i dlatego nie może przekraczać ustawienie maxSockets, które są przydzielone dla maszyny Wirtualnej. Zalecenie dotyczące Azure Webapps będzie można ustawić wartości ustawienie maxSockets agentKeepAlive w sumie 160 gniazda dla maszyny Wirtualnej. Oznacza to, jeśli masz 4 node.exe uruchomione na maszynie Wirtualnej, czy chcesz ustawioną ustawienie maxSockets agentKeepAlive 40 za node.exe czyli 160 całkowita dla maszyny Wirtualnej.

Przykład [agentKeepALive](https://www.npmjs.com/package/agentkeepalive) konfiguracji:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

W tym przykładzie przyjęto założenie, że masz 4 node.exe uruchomione na maszynie Wirtualnej. Jeśli masz inną liczbę node.exe uruchomione na maszynie Wirtualnej, należy zmodyfikować ustawienie maxSockets, w związku z tym ustawieniem.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Moja aplikacja węzła zużywa zbyt dużej ilości Procesora.
Zalecenia w Azure Webapps prawdopodobnie zostanie wyświetlony w portalu sieci o wysokie użycie procesora cpu. Można również skonfigurować monitory do monitorowania określonych [metryki](web-sites-monitor.md). Podczas sprawdzania użycia procesora CPU na [pulpitu nawigacyjnego portalu Azure](../application-insights/app-insights-web-monitor-performance.md), sprawdź, czy wartości MAX procesora CPU, nie zostały pominięte wartości szczytowe.
W przypadkach, gdy uważasz, że aplikacja zużywa zbyt dużej ilości procesora CPU i nie można wyjaśnić, dlaczego należy do profilu aplikacji węzła.

### 
#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>Profilowanie aplikacji węzła na używanie azure z V8 profilera
Na przykład umożliwia powiedz aplikacji world hello, który ma zostać profilu, jak pokazano poniżej:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Przejdź do Twojej scm https://yoursite.scm.azurewebsites.net/DebugConsole lokacji

Zobaczysz wiersz polecenia, jak pokazano poniżej. Przejdź do katalogu witryny/wwwroot

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Uruchom polecenie "npm install v8 profilera"

To należy instalować programu profilującego v8 w węźle\_modułów katalogu i wszystkich jego zależności.
Teraz Edytuj server.js sieci do profilu aplikacji.

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Powyżej zmian będzie profilu funkcji WriteConsoleLog, a następnie wpisz profilu dane wyjściowe do pliku "profile.cpuprofile" w obszarze wwwroot z lokacji. Wyślij żądanie do aplikacji. Zostanie wyświetlony plik 'profile.cpuprofile' utworzone na podstawie wwwroot z lokacji.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Pobierz ten plik i musisz otworzyć ten plik z narzędziami F12 Chrome. Trafienia F12 na chrome, a następnie na karcie"profilów". Kliknij przycisk "Obciążenia". Wybierz plik profile.cpuprofile, który został pobrany. Kliknij żądany profil załadowane przed chwilą.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Zobaczysz, że 95% czasu został wykorzystany przez funkcję WriteConsoleLog, jak pokazano poniżej. To również wyświetlanie numerów wierszy dokładne i plików źródłowych, które mogą powodować ten problem.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Moja aplikacja węzła zużywa zbyt dużej ilości pamięci.
Zalecenia w Azure używanie prawdopodobnie zostanie wyświetlony w portalu sieci o duże użycie pamięci. Można również skonfigurować monitory do monitorowania określonych [metryki](web-sites-monitor.md). Podczas sprawdzania użycia pamięci na [pulpitu nawigacyjnego portalu Azure](../application-insights/app-insights-web-monitor-performance.md), sprawdź wartości maksymalna wartość pamięci, dzięki czemu nie traci wartości szczytowe.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Wykrywanie przecieków i porównywania sterty dla środowiska node.js
Można użyć [memwatch węzła](https://github.com/lloyd/node-memwatch) pomoże zidentyfikować pamięci przecieków.
Można zainstalować memwatch podobnie jak profilera v8 i edytować kod do przechwytywania i różnicowy stosów do identyfikowania pamięć przecieków w aplikacji.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Moje node.exe są pobierania losowo skasowane.
Istnieje kilka przyczyn, dlaczego może to występować:

1. Aplikacji wywołującej nieprzechwyconych wyjątków — d: wyboru Sprawdź\\macierzystego\\LogFiles\\aplikacji\\errors.txt rejestrowania w pliku na wyjątek. Ten plik zawiera ślad stosu, więc można naprawić na podstawie tych aplikacji.
2. Aplikacja zużywa zbyt dużej ilości pamięci, co ma wpływ na inne procesy z wprowadzenie. Jeśli łączna ilość pamięci maszyny Wirtualnej jest bliski 100%, Twoje node.exe można kasować przez Menedżera procesu, aby umożliwić inne procesy, które umożliwiają wykonania dodatkowych czynności. Aby rozwiązać ten problem, upewnij się, że aplikacja nie jest przeciek pamięci lub jeśli naprawdę aplikacji użytkownik musi używać dużej ilości pamięci, proszę skalowanie w górę do większych maszyn wirtualnych ze znacznie większą ilość pamięci RAM.

### <a name="my-node-application-does-not-start"></a>Moja aplikacja węzła nie uruchamia.
Jeśli aplikacja zwraca 500 błędów podczas uruchamiania, może istnieć kilka przyczyn:

1. Node.exe nie jest obecny w poprawnej lokalizacji. Sprawdź ustawienie nodeProcessCommandLine.
2. Plik skryptu głównego nie ma w poprawnej lokalizacji. Sprawdź plik web.config i upewnij się, że nazwa pliku głównego skryptu w sekcji obsługi pasuje do pliku głównego skryptu.
3. Plik Web.config konfiguracji jest niepoprawna — sprawdzanie nazw/wartości ustawień.
4. Zimny Start — aplikacja działa zbyt długo do uruchomienia. Jeśli aplikacja będzie trwało dłużej niż (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1 000 sekund iisnode zwróci błąd 500. Zwiększenie wartości tych ustawień, aby dopasować godziną początkową aplikacji, aby zapobiec iisnode limit czasu i zwróceniem komunikatu o błędzie 500.

### <a name="my-node-application-crashed"></a>Moja aplikacja węzła awaria
Aplikacji wywołującej nieprzechwyconych wyjątków — d: wyboru Sprawdź\\macierzystego\\LogFiles\\aplikacji\\errors.txt rejestrowania w pliku na wyjątek. Ten plik zawiera ślad stosu, więc można naprawić na podstawie tych aplikacji.

### <a name="my-node-application-takes-too-much-time-to-startup-cold-start"></a>Moja aplikacja węzeł ma zbyt dużo czasu do uruchomienia (zimny Start)
Najczęstszą przyczyną tego jest, że aplikacja ma wiele plików w węźle\_moduły i aplikacja próbuje załadować większość tych plików podczas uruchamiania. Domyślnie ponieważ pliki znajdują się w udziale sieciowym na używanie Azure ładowanie tak wielu plików może zająć trochę czasu.
Są to przyspieszyć rozwiązania:

1. Upewnij się, że masz struktury płaskiej zależności i ma zduplikowane zależności za pomocą npm3 własnych modułów.
2. Próba opóźnionego ładowania węzeł\_moduły i nie wszystkie moduły podczas uruchamiania obciążenia. Oznacza to, że wywołanie require('module') ma się odbywać w razie konieczności faktycznie wewnątrz funkcji próbować użyć modułu.
3. Używanie Azure oferuje funkcję o nazwie lokalnej pamięci podręcznej. Ta funkcja kopiuje zawartość z udziału sieciowego na lokalny dysk na maszynie Wirtualnej. Ponieważ pliki znajdują się lokalnie, czas ładowania węzła\_modułów jest znacznie szybsze.

## <a name="iisnode-http-status-and-substatus"></a>Stan http IISNODE i podstanu
To [plik źródłowy](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) zawiera wszystkie możliwe stan/podstanu iisnode kombinacja może zwrócić w przypadku błędu.

Włącz FREB dla aplikacji, aby zobaczyć kod błędu win32 (Upewnij się, możesz włączyć FREB tylko w lokacjach nieprodukcyjnych ze względu na wydajność).

| Stan HTTP | Podstan protokołu HTTP | Możliwa przyczyna? |
| --- | --- | --- |
| 500 |1000 |Znaleziono niektórych problem podczas wysyłania żądania do programu IISNODE — Sprawdź, czy node.exe został uruchomiony w. Node.exe może ulec awarii podczas uruchamiania. Sprawdź konfigurację web.config błędów. |
| 500 |1001 |-Win32Error 0x2 — aplikacja nie odpowiada na adres URL. Sprawdź reguły ponownego zapisywania adresów URL lub jeśli aplikacja express ma poprawne trasy zdefiniowane. -Win32Error 0x6d — nazwany potok jest zajęty — Node.exe nie akceptuje żądania potoku jest zajęty. Sprawdź wysokiego użycia procesora cpu. -Inne błędy — Sprawdź, czy awaria node.exe. |
| 500 |1002 |Awaria node.exe — sprawdź d:\\macierzystego\\LogFiles\\rejestrowania errors.txt dla ślad stosu. |
| 500 |1003 |Potok konfiguracji problem — nigdy nie powinno to zobaczyć, ale jeśli to zrobisz, konfiguracja nazwany potok jest niepoprawna. |
| 500 |1004-1018 |Wystąpił błąd podczas wysyłania żądania lub odpowiedzi z node.exe przetwarzania. Sprawdź, czy awaria node.exe. Sprawdź d:\\macierzystego\\LogFiles\\rejestrowania errors.txt dla ślad stosu. |
| 503 |1000 |Za mało pamięci, aby przydzielić więcej połączeń nazwanych potoków. Sprawdź, dlaczego aplikacja zużywa dużej ilości pamięci. Sprawdź wartość ustawienia maxConcurrentRequestsPerProcess. Jeśli nie jest on nieskończone i wiele żądań, zwiększyć tę wartość, aby uniknąć tego błędu. |
| 503 |1001 |Nie można wysłać żądania do node.exe, ponieważ aplikacja jest odtwarzania. Po aplikacji po odtworzeniu, żądania powinien być obsługiwany normalnie. |
| 503 |1002 |Kod błędu win32 wyboru przyczyny rzeczywiste — żądanie nie mógł zostać wysłany do node.exe. |
| 503 |1003 |Nazwany potok jest zbyt zajęty — Sprawdź, czy węzeł zużywa dużo zasobów procesora |

Ustawienie w NODE.exe węzeł\_OCZEKUJE\_POTOKU\_WYSTĄPIEŃ. Domyślnie poza azure webapps ta wartość to 4. Oznacza to, że node.exe w czasie na nazwanym potoku może akceptować tylko 4 żądania. Na używanie usługi Azure ta wartość jest równa 5000 i ta wartość powinna być wystarczająca dla większości aplikacji węzła działających na używanie usługi azure. Użytkownik nie powinien być widoczny 503.1003 na używanie azure ponieważ mamy wysokiej wartości dla węzła\_OCZEKUJE\_POTOKU\_WYSTĄPIEŃ.  |

## <a name="more-resources"></a>Więcej zasobów
Skorzystaj z poniższych linków, aby dowiedzieć się więcej na temat aplikacji node.js w usłudze Azure App Service.

* [Rozpoczynanie pracy z aplikacjami sieci web Node.js w usłudze Azure App Service](app-service-web-get-started-nodejs.md)
* [How to debug a Node.js web app in Azure App Service (Jak debugować aplikację sieci Web Node.js w usłudze Azure App Service)](app-service-web-tutorial-nodejs-mongodb-app.md)
* [Using Node.js Modules with Azure applications (Używanie modułów Node.js z aplikacjami platformy Azure)](../nodejs-use-node-modules-azure-apps.md)
* [Azure App Service Web Apps: Node.js (Aplikacje sieci Web w usłudze Azure App Service: Node.js)](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Centrum deweloperów środowiska Node.js](../nodejs-use-node-modules-azure-apps.md)
* [Exploring the Super Secret Kudu Debug Console (Szczegółowe informacje o ściśle tajnej konsoli debugowania aparatu Kudu)](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)

