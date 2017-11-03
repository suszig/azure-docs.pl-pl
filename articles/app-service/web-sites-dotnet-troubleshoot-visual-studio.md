---
title: "Rozwiązywanie problemów z aplikacji sieci web w usłudze Azure App Service przy użyciu programu Visual Studio"
description: "Dowiedz się, jak rozwiązywanie problemów z aplikacją sieci web platformy Azure przy użyciu zdalnego debugowania i śledzenia i narzędzia rejestrowania, które są wbudowane w programie Visual Studio 2013."
services: app-service
documentationcenter: .net
author: tdykstra
manager: erikre
editor: 
ms.assetid: def8e481-7803-4371-aa55-64025d116c97
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/29/2016
ms.author: rachelap
ms.openlocfilehash: e42ff64fdd2be87fc19be267d4e2a29e38f67ef5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-a-web-app-in-azure-app-service-using-visual-studio"></a>Rozwiązywanie problemów z aplikacji sieci web w usłudze Azure App Service przy użyciu programu Visual Studio
## <a name="overview"></a>Omówienie
Ten samouczek pokazuje, jak za pomocą narzędzi Visual Studio, pomagających w debugowaniu aplikacji sieci web w [usługi aplikacji](http://go.microsoft.com/fwlink/?LinkId=529714), uruchamiając w [tryb debugowania](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) zdalnie lub poprzez wyświetlenie Dzienniki aplikacji i dzienniki serwera sieci web.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

Dowiesz się:

* Funkcji zarządzania aplikacjami sieci web platformy Azure są dostępne w programie Visual Studio.
* Jak szybko wprowadź zmiany w aplikacji sieci web do zdalnego za pomocą widoku zdalnego programu Visual Studio.
* Jak uruchomić tryb debugowania zdalnego podczas projektu jest uruchomiona na platformie Azure, zarówno w przypadku aplikacji sieci web, jak i zadanie WebJob.
* Tworzenie aplikacji dzienników śledzenia i przeglądać podczas aplikacji jest ich tworzenia.
* Jak wyświetlać dzienniki serwera sieci web, w tym szczegółowe komunikaty o błędach i śledzenie nieudanych żądań.
* Sposób wysyłania dzienników diagnostycznych do magazynu Azure konta i wyświetlić je istnieje.

Jeśli masz program Visual Studio Ultimate, można również użyć [IntelliTrace](http://msdn.microsoft.com/library/vstudio/dd264915.aspx) do debugowania. W tym samouczku nie pasuje do funkcji IntelliTrace.

## <a name="prerequisites"></a>Wymagania wstępne
W tym samouczku współpracuje z Środowisko deweloperskie, projekt sieci web i aplikacji sieci web platformy Azure, który został skonfigurowany w [wprowadzenie do platformy Azure i ASP.NET][GetStarted]. Dla sekcji zadań Webjob, będziesz potrzebować aplikacji, które są tworzone w [wprowadzenie do zestawu SDK zadań Webjob Azure][GetStartedWJ].

Przykłady kodu przedstawiono w tym samouczku są przeznaczone dla aplikacji sieci web MVC C#, ale procedur rozwiązywania problemów są takie same dla aplikacji Visual Basic i formularzy sieci Web.

Samouczka przyjęto założenie, że używasz programu Visual Studio 2015 lub 2013. Jeśli używasz programu Visual Studio 2013, funkcje zadań Webjob wymagają [Update 4](http://go.microsoft.com/fwlink/?LinkID=510314) lub nowszym.

Dzienniki przesyłania strumieniowego funkcja działa tylko dla aplikacji przeznaczonych dla platformy .NET Framework 4 lub nowszy.

## <a name="sitemanagement"></a>Konfiguracja aplikacji sieci Web i zarządzanie
Visual Studio zapewnia dostęp do podzbioru funkcje zarządzania aplikacjami sieci web i ustawienia konfiguracji dostępne w [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715). W tej sekcji zobaczysz, co jest dostępne przy użyciu **Eksploratora serwera**. Aby wyświetlić najnowsze funkcje integracji platformy Azure, wypróbowanie **Eksplorator chmury** również. Możesz otworzyć oba okna z **widoku** menu.

1. Jeśli jeszcze nie jest obecnie zalogowany na platformie Azure w programie Visual Studio, kliknij przycisk **Connect Azure** przycisku na **Eksploratora serwera**.

    Alternatywą jest zainstalowany certyfikat zarządzania, który umożliwia dostęp do Twojego konta. Jeśli wybierzesz zainstalować certyfikat, kliknij prawym przyciskiem myszy **Azure** w węźle **Eksploratora serwera**, a następnie kliknij przycisk **zarządzanie i subskrypcje filtru** w menu kontekstowym. W **Zarządzaj subskrypcjami Azure** okno dialogowe, kliknij przycisk **certyfikaty** , a następnie kliknij pozycję **importu**. Postępuj zgodnie z instrukcjami, aby pobierać i importować plik subskrypcji (nazywane również *.publishsettings* pliku) dla konta platformy Azure.

   > [!NOTE]
   > Jeśli pobierzesz plik subskrypcji, zapisz go w folderze poza katalogów kodu źródłowego (na przykład w folderze pobrane), a następnie usuń go, po zakończeniu importowania. Złośliwy użytkownik, który uzyskuje dostęp do pliku subskrypcji można edytować, tworzenia i usuwania usług Azure.
   >
   >

    Aby uzyskać więcej informacji na temat nawiązywania połączenia z zasobami Azure z programu Visual Studio, zobacz [Zarządzanie kontami, subskrypcje i ról administracyjnych](http://go.microsoft.com/fwlink/?LinkId=324796#BKMK_AccountVCert).
2. W **Eksploratora serwera**, rozwiń węzeł **Azure** i rozwiń **usługi aplikacji**.
3. Rozwiń grupę zasobów zawierającą aplikację sieci web, który został utworzony w [wprowadzenie do platformy Azure i ASP.NET][GetStarted], a następnie kliknij prawym przyciskiem myszy węzeł aplikacji sieci web i kliknij przycisk **ustawienia widoku**.

    ![Ustawienia widoku w Eksploratorze serwera](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewsettings.png)

    **Aplikacji sieci Web Azure** zostanie wyświetlona karta i widać tam sieci web aplikacji zarządzania i konfiguracji zadań, które są dostępne w programie Visual Studio.

    ![Okno aplikacji sieci Web platformy Azure](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configtab.png)

    W tym samouczku należy używać rejestrowania i śledzenia, listach rozwijanych. Będzie również używany zdalnego debugowania, ale będzie użyć innej metody, aby je włączyć.

    Informacji o polach ustawień aplikacji i parametry połączenia w tym oknie znajduje się w temacie [Azure Web Apps: jak ciągi aplikacji i pracy ciągów połączenia](http://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

    Jeśli chcesz wykonać zadania zarządzania aplikacji sieci web, które nie może zostać wykonane w tym oknie kliknij **Otwórz w portalu zarządzania** do Otwórz okno przeglądarki w portalu Azure.

## <a name="remoteview"></a>Dostęp do plików aplikacji sieci web w Eksploratorze serwera
Zwykle wdrażanie projektu sieci web z `customErrors` w pliku Web.config, Ustaw flagę `On` lub `RemoteOnly`, co oznacza, że użytkownik nie pojawia się komunikat przydatne, gdy coś nieprawidłowość. Wiele błędów, możesz uzyskać będzie strony, jak jeden z następujących komputerów.

**Błąd serwera w "/" aplikacji:**

![Strona błędu która nie jest pomocna](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror.png)

**Wystąpił błąd:**

![Strona błędu która nie jest pomocna](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror1.png)

**Witryna sieci Web nie można wyświetlić strony**

![Strona błędu która nie jest pomocna](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror2.png)

Często najłatwiejszym sposobem na znalezienie przyczyny tego błędu jest umożliwienie szczegółowe komunikaty o błędach, które pierwszy poprzednich zrzutach ekranu wyjaśniono sposób wykonywania. Wymagające zmiany wdrożonym pliku Web.config. Można edytować *Web.config* pliku w projekcie i ponownie wdrożyć projekt lub Utwórz [przekształcenie pliku Web.config](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) i wdrożyć kompilację debugowania, ale szybciej: w **Eksploratora rozwiązań** bezpośrednio możesz wyświetlić i edytować pliki w aplikacji sieci web do zdalnego przy użyciu *zdalnego widoku* funkcji.

1. W **Eksploratora serwera**, rozwiń węzeł **Azure**, rozwiń węzeł **usługi aplikacji**, rozwiń grupę zasobów, która aplikacja sieci web znajdują się w temacie, a następnie rozwiń węzeł dla aplikacji sieci web.

    Możesz sprawdzić węzły, które zapewniają dostęp do plików zawartości i pliki dziennika aplikacji sieci web.
2. Rozwiń węzeł **pliki** węzeł, a następnie kliknij dwukrotnie plik *Web.config* pliku.

    ![Otwórz plik Web.config](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfig.png)

    Visual Studio otworzy plik Web.config z aplikacji sieci web do zdalnego i przedstawia [zdalnego] obok nazwy pliku na pasku tytułu.
3. Dodaj następujący wiersz do `system.web` elementu:

    `<customErrors mode="Off"></customErrors>`

    ![Edytowanie pliku Web.config](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfigedit.png)
4. Odśwież przeglądarkę, która jest wyświetlana, która nie jest pomocna komunikat, a teraz uzyskać szczegółowy komunikat o błędzie, takie jak na poniższym przykładzie:

    ![Szczegółowy komunikat o błędzie](./media/web-sites-dotnet-troubleshoot-visual-studio/detailederror.png)

    (Błąd pokazano został utworzony przez dodanie wiersza zaznaczone na czerwono na *Views\Home\Index.cshtml*.)

Edytowania pliku Web.config jest tylko jeden przykład scenariusze, w których możliwość odczytania i zmodyfikowania plików w aplikacji sieci web platformy Azure ułatwienia rozwiązania problemu.

## <a name="remotedebug"></a>Zdalne debugowanie aplikacji sieci web
Jeśli szczegółowy komunikat o błędzie nie zawiera informacji wystarczających i nie można ponownie utworzyć błąd lokalnie, rozwiązywanie problemów z innym sposobem jest zdalne uruchamianie w trybie debugowania. Można ustawić punktów przerwania, manipulowanie nimi pamięci bezpośrednio, krokowo kodu oraz nawet zmienić ścieżkę kodu.

Debugowanie zdalne nie działa w wersji Express programu Visual Studio.

W tej sekcji pokazano, jak można debugować zdalnie przy użyciu projektu, należy utworzyć w [wprowadzenie do platformy Azure i ASP.NET][GetStarted].

1. Otwórz projekt sieci web, który został utworzony w [wprowadzenie do platformy Azure i ASP.NET][GetStarted].
2. Otwórz *Controllers\HomeController.cs*.
3. Usuń `About()` — metoda i wstaw poniższy kod w jego miejscu.

        public ActionResult About()
        {
            string currentTime = DateTime.Now.ToLongTimeString();
            ViewBag.Message = "The current time is " + currentTime;
            return View();
        }
4. [Ustaw punkt przerwania](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) na `ViewBag.Message` wiersza.
5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i kliknij przycisk **publikowania**.
6. W **profilu** listy rozwijanej wybierz pozycję takie same profilu tego można użyć w [wprowadzenie do platformy Azure i ASP.NET][GetStarted].
7. Kliknij przycisk **ustawienia** , a następnie zmień **konfiguracji** do **debugowania**, a następnie kliknij przycisk **publikowania**.

    ![Publikowanie w trybie debugowania](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-publishdebug.png)
8. Po wdrożeniu zakończenie i przeglądarce zostanie otwarty adres URL aplikacji sieci web platformy Azure, zamknij przeglądarkę.
9. W **Eksploratora serwera**, kliknij prawym przyciskiem myszy aplikację sieci web, a następnie kliknij przycisk **dołączyć debuger**.

    ![Dołączanie debugera](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-attachdebugger.png)

    Przeglądarka automatycznie otwiera do strony głównej działające na platformie Azure. Może być konieczne Zaczekaj 20 sekund lub to Azure konfiguruje serwer do debugowania. To opóźnienie odbywa się tylko przy pierwszym uruchomieniu w trybie debugowania aplikacji sieci web. Kolejne razy w ciągu 48 godzin po rozpoczęciu debugowania ponownie będzie wyświetlony z pewnym opóźnieniem.

    **Uwaga:** Jeśli masz problemy z uruchamianie debugera, spróbuj to zrobić za pomocą **Eksplorator chmury** zamiast **Eksploratora serwera**.
10. Kliknij przycisk **o** w menu.

     Visual Studio przestaje punkt przerwania i kod działa na platformie Azure, nie na komputerze lokalnym.
11. Umieść kursor nad `currentTime` zmiennej, aby wyświetlić wartość czasu.

     ![Zmienna widoku w trybie debugowania, które działają na platformie Azure](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugviewinwa.png)

     Czas, który zostanie wyświetlony jest czas na serwerach Azure, które może być w innej strefie czasowej niż komputera lokalnego.
12. Wprowadź nową wartość dla `currentTime` zmiennych, takich jak "Teraz działają na platformie Azure".
13. Naciśnij klawisz F5, aby kontynuować działanie.

     Strona informacje działające na platformie Azure Wyświetla nową wartość wprowadzona w zmiennej bieżącagodzina.

     ![Strona z nową wartością — informacje](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugchangeinwa.png)

## <a name="remotedebugwj"></a>Zdalne debugowanie zadań Webjob
W tej sekcji pokazano, jak można debugować zdalnie za pomocą aplikacji sieci web i projektu, możesz utworzyć w [wprowadzenie do zestawu SDK zadań Webjob Azure](https://github.com/Azure/azure-webjobs-sdk/wiki).

Funkcje w tej sekcji są dostępne tylko w programie Visual Studio 2013 z aktualizacją Update 4 lub nowszym.

Debugowanie zdalne działa tylko ciągłe zadania Webjob. Zaplanowanych, jak i na żądanie zadania Webjob nie obsługuje debugowania.

1. Otwórz projekt sieci web, który został utworzony w [wprowadzenie do zestawu SDK zadań Webjob Azure][GetStartedWJ].
2. W projekcie ContosoAdsWebJob Otwórz *Functions.cs*.
3. [Ustaw punkt przerwania](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) na pierwszą instrukcją w `GnerateThumbnail` metody.

    ![Ustaw punkt przerwania](./media/web-sites-dotnet-troubleshoot-visual-studio/wjbreakpoint.png)
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt sieci web (nie projekt zadania WebJob) i kliknij przycisk **publikowania**.
5. W **profilu** listy rozwijanej wybierz pozycję takie same profilu tego można użyć w [wprowadzenie do zestawu SDK zadań Webjob Azure](https://github.com/Azure/azure-webjobs-sdk/wiki).
6. Kliknij przycisk **ustawienia** , a następnie zmień **konfiguracji** do **debugowania**, a następnie kliknij przycisk **publikowania**.

    Program Visual Studio wdroży sieci web i zadania WebJob projektów i przeglądarce zostanie otwarty adres URL aplikacji sieci web platformy Azure.
7. W **Eksploratora serwera** rozwiń **Azure > usługi aplikacji > grupy zasobów > aplikacji sieci web > zadań Webjob > ciągłe**i kliknij prawym przyciskiem myszy **ContosoAdsWebJob**.
8. Kliknij przycisk **dołączyć debuger**.

    ![Dołączanie debugera](./media/web-sites-dotnet-troubleshoot-visual-studio/wjattach.png)

    Przeglądarka automatycznie otwiera do strony głównej działające na platformie Azure. Może być konieczne Zaczekaj 20 sekund lub to Azure konfiguruje serwer do debugowania. To opóźnienie odbywa się tylko przy pierwszym uruchomieniu w trybie debugowania aplikacji sieci web. Przy następnym dołączyć debuger będzie opóźnienia, jeśli chcesz w ciągu 48 godzin.
9. W przeglądarce sieci web, która jest otwarta do strony głównej aplikacji Contoso Ads Utwórz nowe usługi ad.

    Tworzenie ad powoduje, że komunikatu w kolejce do utworzenia, które będą pobierane przez zadania WebJob i przetwarzane. Gdy zestaw SDK zadań Webjob wywołuje funkcję do przetwarzania komunikatu w kolejce, kod będzie trafiony punkt przerwania.
10. Gdy debuger dzieli się na punkt przerwania, można sprawdzić i zmienić wartości zmiennych, gdy program jest uruchomiony w chmurze. Na poniższej ilustracji debugera zawiera zawartość obiektu blobInfo, który został przekazany do metody GenerateThumbnail.

     ![Obiekt blobInfo w debugerze](./media/web-sites-dotnet-troubleshoot-visual-studio/blobinfo.png)
11. Naciśnij klawisz F5, aby kontynuować działanie.

     Metoda GenerateThumbnail zakończy, tworzenie miniatur.
12. W przeglądarce Odśwież stronę indeksu i zobaczyć miniaturę.
13. W programie Visual Studio naciśnij klawisz SHIFT + F5, aby zatrzymać debugowanie.
14. W **Eksploratora serwera**, kliknij prawym przyciskiem myszy węzeł ContosoAdsWebJob i kliknij przycisk **widoku pulpitu nawigacyjnego**.
15. Zaloguj się przy użyciu poświadczeń platformy Azure, a następnie kliknij przycisk Nazwa zadania WebJob, aby przejść do strony dla WebJob.

     ![Kliknij przycisk ContosoAdsWebJob](./media/web-sites-dotnet-troubleshoot-visual-studio/clickcaw.png)

     Pulpit nawigacyjny wskazuje, że funkcja GenerateThumbnail ostatnio wykonane.

     (Przy następnym kliknięciu **widoku pulpitu nawigacyjnego**, nie trzeba zarejestrować i przeglądarki przejście bezpośrednio do strony dla WebJob.)
16. Kliknij nazwę funkcji, aby zobaczyć szczegółowe informacje dotyczące wykonywania funkcji.

     ![Szczegóły funkcji](./media/web-sites-dotnet-troubleshoot-visual-studio/funcdetails.png)

Jeśli funkcja [zapisano dzienniki](https://github.com/Azure/azure-webjobs-sdk/wiki), można kliknąć przycisk **ToggleOutput** je wyświetlić.

## <a name="notes-about-remote-debugging"></a>Uwagi dotyczące zdalnego debugowania
* Nie zaleca się uruchamiania w trybie debugowania w środowisku produkcyjnym. Jeśli do wielu wystąpień serwera nie jest skalowanie aplikacji sieci web w środowisku produkcyjnym, debugowanie uniemożliwi serwera sieci web odpowiada na żądania innych. Jeśli masz wiele wystąpień serwerów sieci web po dołączeniu do debugera, zostanie wyświetlony losowe wystąpienia, a nie ma możliwości aby upewnić się, że przeglądarka kolejne żądania zostanie wysłany do tego wystąpienia. Ponadto zwykle nie można wdrożyć kompilację debugowania w środowisku produkcyjnym i optymalizacje kompilatora dla wersji kompilacji może uniemożliwić Pokaż, co dzieje się linii w kodzie źródłowym. Podczas rozwiązywania problemów w środowisku produkcyjnym, najlepiej zasobu to aplikacja sieci web i śledzenie dzienniki serwera.
* Uniknąć długich zatrzymane na punktów przerwania podczas zdalnego debugowania. Azure traktuje procesu, który zostało zatrzymane przez czas dłuższy niż kilka minut jako proces nie odpowiada, a następnie zamyka go.
* Podczas debugowania kodu, serwer wysyła dane do programu Visual Studio, które mogą mieć wpływ na koszty przepustowości. Informacje o szybkości przepustowości, zobacz [cennik usługi Azure](https://azure.microsoft.com/pricing/calculator/).
* Upewnij się, że `debug` atrybutu `compilation` element *Web.config* plik jest ustawiony na wartość true. Ma ustawioną wartość true, domyślnie po opublikowaniu konfiguracji kompilacji debugowania.

        <system.web>
          <compilation debug="true" targetFramework="4.5" />
          <httpRuntime targetFramework="4.5" />
        </system.web>
* Jeśli okaże się, że debuger nie wkroczyć do kodu, który chcesz debugować, trzeba będzie zmienić ustawienie tylko mój kod.  Aby uzyskać więcej informacji, zobacz [Ogranicz wykonywanie krok po kroku, aby tylko mój kod](http://msdn.microsoft.com/library/vstudio/y740d9d3.aspx#BKMK_Restrict_stepping_to_Just_My_Code).
* Po włączeniu funkcji debugowania zdalnego, oraz po 48 godzin funkcji jest automatycznie wyłączane czasomierz rozpoczyna się na serwerze. Ze względów bezpieczeństwa i wydajności odbywa się to ograniczenie 48 godzin. Można łatwo włączyć funkcję ponownie dowolną liczbę razy. Zaleca się pozostawienie ją wyłączoną, gdy nie są aktywnie debugowania.
* Można ręcznie dołączyć debuger do dowolnego procesu, nie tylko procesu sieci web app (w3wp.exe). Aby uzyskać więcej informacji o sposobie używania tryb debugowania w programie Visual Studio, zobacz [debugowania w programie Visual Studio](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx).

## <a name="logsoverview"></a>Przegląd dzienników diagnostycznych
Aplikację ASP.NET, która działa w aplikacji sieci web platformy Azure można utworzyć następujące rodzaje dzienników:

* **Dzienniki śledzenia aplikacji**<br/>
  Te dzienniki powoduje utworzenie aplikacji przez wywołanie metody [System.Diagnostics.Trace](http://msdn.microsoft.com/library/system.diagnostics.trace.aspx) klasy.
* **Dzienniki serwera sieci Web**<br/>
  Serwer sieci web tworzy wpisu dziennika dla każdego żądania HTTP do aplikacji sieci web.
* **Szczegółowe informacje o błędzie dzienników komunikatów**<br/>
  Serwer sieci web tworzy strony HTML z kilku dodatkowych informacji nieudanych żądań HTTP (te, które powoduje kod stanu 400 lub nowszej).
* **Nie powiodło się żądanie dzienniki śledzenia**<br/>
  Serwer sieci web tworzy plik XML z informacjami o szczegółowe śledzenie niepomyślnych żądań HTTP. Serwer sieci web udostępnia również pliku XSL do formatu XML w przeglądarce.

Rejestrowanie ma wpływ na wydajność aplikacji sieci web, więc Azure daje możliwość włączyć lub wyłączyć każdego typu dziennika zgodnie z potrzebami. Dla Dzienniki aplikacji można określić, że można zapisywać tylko dzienniki powyżej pewnego poziomu ważności. Podczas tworzenia nowej aplikacji sieci web, domyślnie całego rejestrowania jest wyłączona.

Dzienniki są zapisywane w plikach w *LogFiles* folder w systemie plików, aplikacji sieci web i są dostępne za pośrednictwem protokołu FTP. Dzienniki aplikacji i dzienniki serwera sieci Web można również będą zapisywane do konta usługi Azure Storage. Można zachować większa ilość dzienniki na koncie magazynu nie jest możliwe w systemie plików. Jest ograniczone do maksymalnie 100 megabajtów dzienników przy użyciu systemu plików. (Dzienniki systemu plików są tylko do przechowywania krótkoterminowego. Azure usuwa stare pliki dziennika, aby zwolnić miejsce dla nowych, po osiągnięciu limitu).  

## <a name="apptracelogs"></a>Tworzenie i sprawdź dzienniki śledzenia aplikacji
W tej sekcji należy wykonać następujące zadania:

* Dodawanie instrukcji śledzenia do projektu sieci web, który został utworzony w [wprowadzenie do platformy Azure i ASP.NET][GetStarted].
* Sprawdź dzienniki, gdy lokalne uruchamianie projektu.
* Wyświetl dzienniki wygenerowane aplikacji działających na platformie Azure.

Rejestruje informacje o sposobie tworzenia aplikacji w zadań Webjob, można znaleźć [sposób pracy z magazynem kolejek Azure przy użyciu zestawu SDK WebJobs — jak zapisywanie dzienników](https://github.com/Azure/azure-webjobs-sdk/wiki). Poniższe instrukcje dotyczące wyświetlania dziennika i określania, jak są przechowywane na platformie Azure dotyczą także dzienniki aplikacji utworzonych przez zadań Webjob.

### <a name="add-tracing-statements-to-the-application"></a>Dodawanie instrukcji śledzenia do aplikacji
1. Otwórz *Controllers\HomeController.cs*i Zastąp `Index`, `About`, i `Contact` metody z następującym kodem, aby można było dodać `Trace` instrukcje i `using` instrukcji dla `System.Diagnostics`:

        public ActionResult Index()
        {
            Trace.WriteLine("Entering Index method");
            ViewBag.Message = "Modify this template to jump-start your ASP.NET MVC application.";
            Trace.TraceInformation("Displaying the Index page at " + DateTime.Now.ToLongTimeString());
            Trace.WriteLine("Leaving Index method");
            return View();
        }

        public ActionResult About()
        {
            Trace.WriteLine("Entering About method");
            ViewBag.Message = "Your app description page.";
            Trace.TraceWarning("Transient error on the About page at " + DateTime.Now.ToShortTimeString());
            Trace.WriteLine("Leaving About method");
            return View();
        }

        public ActionResult Contact()
        {
            Trace.WriteLine("Entering Contact method");
            ViewBag.Message = "Your contact page.";
            Trace.TraceError("Fatal error on the Contact page at " + DateTime.Now.ToLongTimeString());
            Trace.WriteLine("Leaving Contact method");
            return View();
        }        
2. Dodaj `using System.Diagnostics;` instrukcji na początku pliku.

### <a name="view-the-tracing-output-locally"></a>Wyświetl dane wyjściowe śledzenia lokalnie
1. Naciśnij klawisz F5, aby uruchomić aplikację w trybie debugowania.

    Odbiornik śledzenia domyślnego zapisuje wszystkie dane wyjściowe śledzenia do **dane wyjściowe** okna, oraz innych danych wyjściowych debugowania. Na poniższej ilustracji przedstawiono danych wyjściowych instrukcji śledzenia, które zostały dodane do `Index` metody.

    ![Śledzenie w oknie debugowania](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugtracing.png)

    Poniższe kroki przedstawiają sposób wyświetlania danych wyjściowych śledzenia na stronie sieci web bez kompilowania kodu w trybie debugowania.
2. Otwórz plik Web.config (znajdujący się w folderze projektu) i Dodaj `<system.diagnostics>` element na końcu pliku, bezpośrednio przed zamknięciem `</configuration>` elementu:

          <system.diagnostics>
            <trace>
              <listeners>
                <add name="WebPageTraceListener"
                    type="System.Web.WebPageTraceListener,
                    System.Web,
                    Version=4.0.0.0,
                    Culture=neutral,
                    PublicKeyToken=b03f5f7f11d50a3a" />
              </listeners>
            </trace>
          </system.diagnostics>

    `WebPageTraceListener` Umożliwiają wyświetlanie dane wyjściowe śledzenia, przechodząc do `/trace.axd`.
3. Dodaj <a href="http://msdn.microsoft.com/library/vstudio/6915t83k(v=vs.100).aspx">element śledzenia</a> w obszarze `<system.web>` w pliku Web.config, takie jak na poniższym przykładzie:

        <trace enabled="true" writeToDiagnosticsTrace="true" mostRecent="true" pageOutput="false" />
4. Naciśnij klawisze CTRL+F5, aby uruchomić aplikację.
5. Na pasku adresu w oknie przeglądarki Dodaj *trace.axd* do adresu URL, a następnie naciśnij klawisz Enter (adres URL będzie podobny do http://localhost:53370/trace.axd).
6. Na **śledzenia aplikacji** kliknij przycisk **Wyświetl szczegóły** w pierwszym wierszu (nie linia BrowserLink).

    ![trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd1.png)

    **Szczegółów żądania** zostanie wyświetlona strona, a następnie w **informacje o śledzeniu** sekcji Zobacz danych wyjściowych instrukcji śledzenia, które zostały dodane do `Index` — metoda.

    ![trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd2.png)

    Domyślnie `trace.axd` jest dostępna tylko lokalnie. Jeśli chcesz udostępnić go z aplikacji sieci web do zdalnego, można dodać `localOnly="false"` do `trace` element *Web.config* plików, jak pokazano w poniższym przykładzie:

        <trace enabled="true" writeToDiagnosticsTrace="true" localOnly="false" mostRecent="true" pageOutput="false" />

    Jednak włączenie `trace.axd` w sieci produkcyjnej aplikacji sieci web zazwyczaj nie zaleca się ze względu na bezpieczeństwo i w poniższych sekcjach zobaczysz łatwiejszy sposób odczytać dzienniki śledzenia w aplikacji sieci web platformy Azure.

### <a name="view-the-tracing-output-in-azure"></a>Wyświetl dane wyjściowe śledzenia w systemie Azure
1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt sieci web i kliknij przycisk **publikowania**.
2. W **publikowanie w sieci Web** okno dialogowe, kliknij przycisk **publikowania**.

    Po Visual Studio publikuje aktualizację, zostanie otwarte okno przeglądarki do strony głównej (zakładając, że nie można wyczyścić **docelowy adres URL** na **połączenia** kartę).
3. W **Eksploratora serwera**, kliknij prawym przyciskiem myszy aplikację sieci web i wybierz **Wyświetl dzienniki przesyłania strumieniowego**.

    ![Wyświetl dzienniki przesyłania strumieniowego w menu kontekstowym](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewlogsmenu.png)

    **Dane wyjściowe** oknie wyświetlane są połączone z usługą przesyłania strumieniowego dzienników i dodaje wiersz powiadomienia co minutę, który jest przesyłany bez dziennika do wyświetlenia.

    ![Wyświetl dzienniki przesyłania strumieniowego w menu kontekstowym](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-nologsyet.png)
4. W oknie przeglądarki, które zawiera strony głównej aplikacji, kliknij przycisk **skontaktuj się z**.

    W ciągu kilku sekund dane wyjściowe z poziomu Błąd śledzenia, należy dodać do `Contact` metoda pojawia się w **dane wyjściowe** okna.

    ![Błąd podczas śledzenia w oknie danych wyjściowych](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-errortrace.png)

    Visual Studio jest tylko wyświetlane śladów poziom błędu, ponieważ jest ustawieniem domyślnym, po włączeniu dziennika usługi monitorowania. Podczas tworzenia nowej aplikacji sieci web platformy Azure, wszystkie rejestrowanie jest domyślnie wyłączona, jak przedstawiono po otwarciu strony ustawień wcześniej:

    ![Rejestrowanie aplikacji](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-apploggingoff.png)

    Jednakże, gdy wybrano **Wyświetl dzienniki przesyłania strumieniowego**, Visual Studio automatycznie zmieniane **Logging(File System) aplikacji** do **błąd**, co oznacza, że poziom błędów dzienniki przekazywane. Aby wyświetlić wszystkie dzienniki śledzenia, można zmienić to ustawienie, aby **pełne**. Po wybraniu niższa niż błąd poziom ważności, również są zgłaszane wszystkie dzienniki dla wyższego poziomu ważności. Więc po wybraniu pełne, możesz również sprawdzić informacje, ostrzeżenie i dzienników błędów.  

1. W **Eksploratora serwera**, kliknij prawym przyciskiem myszy aplikację sieci web, a następnie kliknij przycisk **ustawienia widoku** tak samo jak wcześniej.
2. Zmień **rejestrowania aplikacji (w systemie plików)** do **pełne**, a następnie kliknij przycisk **zapisać**.

    ![Ustawienie poziomu śledzenia na pełne](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-applogverbose.png)
3. W oknie przeglądarki, która jest teraz wyświetlany z **skontaktuj się z** kliknij przycisk **Home**, następnie kliknij przycisk **o**, a następnie kliknij przycisk **skontaktuj się z**.

    W ciągu kilku sekund **dane wyjściowe** okna są wyświetlane wszystkie dane wyjściowe śledzenia.

    ![Dane wyjściowe śledzenia Verbose](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-verbosetraces.png)

    W tej sekcji włączyć i wyłączyć rejestrowanie za pomocą ustawień aplikacji sieci web platformy Azure. Można także włączyć i wyłączyć obiekty nasłuchujące śledzenia przez zmodyfikowanie pliku Web.config. Jednak zmodyfikowanie pliku Web.config powoduje, że domena aplikacji do odtworzenia, podczas włączania rejestrowania za pomocą konfiguracji aplikacji sieci web nie to zrobić. Jeśli ten problem zajmuje dużo czasu do odtworzenia, lub jest przerywana, odtwarzania domen aplikacji może być "Napraw" i wymusza poczekaj, aż zdarza się ponownie. Włączanie diagnostyki na platformie Azure nie to zrobić, można już zacząć od razu przechwytywanie informacji o błędzie.

### <a name="output-window-features"></a>Funkcje okna danych wyjściowych
**Dzienników Azure** karcie **dane wyjściowe** okno ma kilka przycisków i pola tekstowego:

![Dzienniki karcie przycisków](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-icons.png)

Te należy wykonać następujące funkcje:

* Wyczyść **dane wyjściowe** okna.
* Włącz lub Wyłącz zawijanie wierszy.
* Aby uruchomić lub zatrzymać monitorowanie dzienników.
* Określ, które dzienniki mają być monitorowane.
* Pobierz dzienniki.
* Przefiltruj dzienniki na podstawie wyszukiwany ciąg lub wyrażenie regularne.
* Zamknij **dane wyjściowe** okna.

Jeśli wprowadzisz ciąg wyszukiwania lub wyrażenie regularne, Visual Studio filtruje informacje o rejestrowaniu po stronie klienta. Oznacza to, że kryteria można wprowadzić po dzienniki są wyświetlane w **dane wyjściowe** a następnie zmienić kryteria filtrowania bez konieczności ponownego generowania dzienniki.

## <a name="webserverlogs"></a>Wyświetl dzienniki serwera sieci web
Dzienniki serwera sieci Web rejestrować wszystkie aktywności protokołu HTTP dla aplikacji sieci web. Aby zobaczyć je w **dane wyjściowe** okna, należy włączyć je do aplikacji sieci web i przekaż programowi Visual Studio, chcesz monitorować je.

1. W **konfiguracji aplikacji sieci Web Azure** kartę, który został otwarty z **Eksploratora serwera**, zmień rejestrowanie serwera sieci Web w celu **na**, a następnie kliknij przycisk **zapisać**.

    ![Włącz rejestrowanie pracy serwera sieci web](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-webserverloggingon.png)
2. W **dane wyjściowe** okna, kliknij przycisk **Określ, które dzienniki platformy Azure, aby monitorować** przycisku.

    ![Określ, które dzienniki platformy Azure do monitorowania](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-specifylogs.png)
3. W **opcje rejestrowania Azure** okno dialogowe, wybierz opcję **dzienniki serwera w sieci Web**, a następnie kliknij przycisk **OK**.

    ![Monitoruj dzienniki serwera sieci web](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorwslogson.png)
4. W oknie przeglądarki, które zawiera aplikacji sieci web, kliknij przycisk **Home**, następnie kliknij przycisk **o**, a następnie kliknij przycisk **skontaktuj się z**.

    Dzienniki aplikacji zwykle występować jako pierwszy, a następnie dzienniki serwera sieci web. Może być konieczne poczekaj trochę czasu zanim dzienniki będą widoczne.

    ![Dzienniki serwera sieci Web w oknie danych wyjściowych](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-wslogs.png)

Domyślnie po pierwszym włączeniu dzienniki serwera sieci web przy użyciu programu Visual Studio, Azure zapisuje dzienniki w systemie plików. Alternatywnie można użyć portalu Azure do określania tego serwera sieci web, które mają być zapisywane dzienniki kontenera obiektów blob na koncie magazynu.

Jeśli na serwerze sieci web logowania do konta magazynu platformy Azure za pomocą portalu, a następnie wyłącz rejestrowanie w programie Visual Studio po ponownym włączeniu rejestrowania w programie Visual Studio ustawienia konta magazynu są przywracane.

## <a name="detailederrorlogs"></a>Wyświetlanie dzienników komunikatów szczegółowe informacje o błędzie
Szczegółowe dzienniki błędów podaj dodatkowe informacje o żądaniach HTTP, które powoduje błąd kody odpowiedzi (400 lub nowszy). Aby zobaczyć je w **dane wyjściowe** okna, należy włączyć je do aplikacji sieci web i przekaż programowi Visual Studio, chcesz monitorować je.

1. W **konfiguracji aplikacji sieci Web Azure** kartę, który został otwarty z **Eksploratora serwera**, zmień **szczegółowe komunikaty o błędach** do **na**, a następnie kliknij przycisk **zapisać**.

    ![Włącz szczegółowe komunikaty o błędach](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailedlogson.png)
2. W **dane wyjściowe** okna, kliknij przycisk **Określ, które dzienniki platformy Azure, aby monitorować** przycisku.
3. W **opcje rejestrowania Azure** okno dialogowe, kliknij przycisk **wszystkie dzienniki**, a następnie kliknij przycisk **OK**.

    ![Monitorowanie wszystkich dzienników](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorall.png)
4. Na pasku adresu w oknie przeglądarki Dodaj nadmiarowe znaki do adresu URL, aby spowodować błąd 404 (na przykład `http://localhost:53370/Home/Contactx`), i naciśnij klawisz Enter.

    Po kilku sekundach dziennika szczegółowe informacje o błędzie pojawia się w programie Visual Studio **dane wyjściowe** okna.

    ![Szczegółowy dziennik błędów w oknie danych wyjściowych](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorlog.png)

    Control + kliknij łącze, aby wyświetlić dane wyjściowe dziennika sformatowany w przeglądarce:

    ![Szczegółowy dziennik błędów w oknie przeglądarki](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorloginbrowser.png)

## <a name="downloadlogs"></a>Pobierz dzienniki systemu plików
Żadnych dzienników, które można monitorować w **dane wyjściowe** okna można również pobrać jako *.zip* pliku.

1. W **dane wyjściowe** okna, kliknij przycisk **Pobierz dzienniki przesyłania strumieniowego**.

    ![Dzienniki karcie przycisków](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadicon.png)

    Zostanie otwarty Eksplorator plików z *pobiera* folder o pobrany plik zaznaczony.

    ![Pobrany plik](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadedfile.png)
2. Wyodrębnij *.zip* pliku i zostanie wyświetlony następującą strukturę folderów:

    ![Pobrany plik](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilefolders.png)

   * Dzienniki śledzenia aplikacji znajdują się w *.txt* plików *LogFiles\Application* folderu.
   * Dzienniki serwera sieci Web znajdują się w *log* plików *LogFiles\http\RawLogs* folderu. Można użyć narzędzia, takie jak [Log Parser](http://www.microsoft.com/download/details.aspx?displaylang=en&id=24659) do wyświetlania i modyfikowania tych plików.
   * Szczegółowe informacje o błędzie komunikat Dzienniki znajdują się w *.html* plików *LogFiles\DetailedErrors* folderu.

     ( *Wdrożeń* folder jest tworzonych przez publikowanie; kontroli źródła nie ma żadnych czynności związanych z publikowaniem Visual Studio. *Git* folder jest dający zapisy związane z kontroli źródła przesyłania strumieniowego usługa plików Publikowanie i dziennika.)  

## <a name="storagelogs"></a>Wyświetl dzienniki magazynu
Dzienniki śledzenia aplikacji mogą być również wysyłane do konta magazynu platformy Azure i można je wyświetlić w programie Visual Studio. Czy utworzysz konto magazynu, włączyć dzienniki magazynu w klasycznym portalu i wyświetlić je w **dzienniki** karcie **aplikacji sieci Web Azure** okna.

Możesz wysłać dzienniki do dowolnego lub wszystkich trzech miejsc docelowych:

* System plików.
* Tabele konta magazynu.
* Obiekty BLOB z konta magazynu.

Można określić poziom ważności różnych dla każdej lokalizacji docelowej.

Tabele ułatwiają wyświetlić szczegóły dzienniki w trybie online i obsługują przesyłanie strumieniowe; można zbadać dzienniki w tabelach i Zobacz nowe dzienniki, ponieważ są one tworzone. Obiekty BLOB ułatwiają Aby pobrać dzienniki w plikach i analizować je za pomocą usługi HDInsight, ponieważ HDInsight wie, jak pracować z magazynu obiektów blob. Aby uzyskać więcej informacji, zobacz **Hadoop i MapReduce** w [opcje magazynu danych (kompilowanie praktyczne aplikacje w chmurze platformy Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options).

Obecnie masz dzienniki systemu plików, ustawić poziom szczegółowości; w poniższych krokach objaśniono przy konfigurowaniu dzienniki poziomu informacji, aby przejść do tabel konta magazynu. Poziom informacji oznacza wszystkie dzienniki utworzona przez wywołanie metody `Trace.TraceInformation`, `Trace.TraceWarning`, i `Trace.TraceError` będą wyświetlane, ale nie dzienniki utworzona przez wywołanie metody `Trace.WriteLine`.

Konta usługi Storage oferują więcej pamięci, jak i długotrwałe przechowywania dzienników w porównaniu do systemu plików. Inną zaletą wysyłania dzienników śledzenia aplikacji do magazynu jest, aby uzyskać dodatkowe informacje o każdym dzienniku, której nie można korzystać z dzienników systemu plików.

1. Kliknij prawym przyciskiem myszy **magazynu** w obszarze węzła Azure, a następnie kliknij przycisk **Utwórz konto magazynu**.

![Tworzenie konta magazynu](./media/web-sites-dotnet-troubleshoot-visual-studio/createstor.png)

1. W **Utwórz konto magazynu** okna dialogowego, wprowadź nazwę konta magazynu.

    Nazwa musi być muszą być unikatowe (inne konto magazynu platformy Azure mogą mieć takiej samej nazwy). Jeśli wprowadzona nazwa jest już używana uzyskasz możliwość go zmienić.

    Adres URL, aby uzyskać dostęp do konta magazynu będzie *{nazwa}*. core.windows.net.
2. Ustaw **Region lub grupę koligacji** listy rozwijanej do najbliższego regionu.

    To ustawienie określa, w którym Centrum danych Azure będą obsługiwać Twoje konto magazynu. W tym samouczku wybór nie należy to znaczącej różnicy, ale dla aplikacji sieci web w środowisku produkcyjnym trzeba serwera sieci web i konta magazynu w tym samym regionie, aby zminimalizować opóźnienie i danych opłaty za wyjście. Aplikacja sieci web (do którego należy utworzyć później) należy uruchomić w regionie, w jak najbardziej zbliżone do przeglądarki, aby zminimalizować czas oczekiwania na dostęp do aplikacji sieci web.
3. Z listy rozwijanej **Replikacja** wybierz wartość **Lokalnie nadmiarowy**.
   
    Jeśli na koncie magazynu włączono replikację geograficzną, przechowywana zawartość jest replikowana do pomocniczego centrum danych. Pozwala to na przejście do trybu failover w tej lokalizacji w przypadku poważnej awarii w lokalizacji głównej. Replikacja geograficzna może pociągnąć za sobą dodatkowe koszty. W przypadku kont testowych i projektowych przeważnie nie chcesz płacić za replikację geograficzną. Aby uzyskać więcej informacji, zobacz temat dotyczący [tworzenia i usuwania konta magazynu oraz zarządzania nim](../storage/common/storage-create-storage-account.md).
4. Kliknij przycisk **Utwórz**.

    ![Nowe konto usługi Storage](./media/web-sites-dotnet-troubleshoot-visual-studio/newstorage.png)    
5. W programie Visual Studio **aplikacji sieci Web Azure** okna, kliknij przycisk **dzienniki** , a następnie kliknij pozycję **Konfiguruj rejestrowanie w portalu zarządzania**.

    <!-- todo:screenshot of new portal if the VS page link goes to new portal -->
    ![Konfigurowanie rejestrowania](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configlogging.png)

    Spowoduje to otwarcie **Konfiguruj** kartę w klasycznym portalu dla aplikacji sieci web.
6. W portalu klasycznym **Konfiguruj** karcie, przewiń w dół do sekcji diagnostyki aplikacji, a następnie zmień **rejestrowania aplikacji (Table Storage)** do **na**.
7. Zmień **poziom rejestrowania** do **informacji**.
8. Kliknij przycisk **zarządzać magazynem tabel**.

    ![Kliknij przycisk Zarządzaj TableStorage](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-stgsettingsmgmtportal.png)

    W **zarządzania magazynem tabeli z programem application Diagnostics** pole, można wybrać konta magazynu Jeśli masz więcej niż jeden. Możesz utworzyć nową tabelę lub użyć istniejącego.

    ![Zarządzanie magazynem tabeli](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-choosestorageacct.png)
9. W **zarządzania magazynem tabeli z programem application Diagnostics** pole kliknij znacznik wyboru, aby zamknąć okno.
10. W portalu klasycznym **Konfiguruj** , kliknij pozycję **zapisać**.
11. W oknie przeglądarki, która wyświetla aplikacji sieci web aplikacji, kliknij przycisk **Home**, następnie kliknij przycisk **o**, a następnie kliknij przycisk **skontaktuj się z**.

     Rejestrowanie informacji utworzonej przez przeglądanie te strony sieci web będą zapisywane na koncie magazynu.
12. W **dzienniki** karcie **aplikacji sieci Web Azure** okna w programie Visual Studio, kliknij przycisk **Odśwież** w obszarze **diagnostycznych Podsumowanie**.

     ![Kliknij przycisk Odśwież](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-refreshstorage.png)

     **Diagnostycznych Podsumowanie** sekcji przedstawiono dzienniki dla ostatnich 15 minut domyślnie. Można zmienić okres, aby zobaczyć więcej dzienników.

     (Jeśli zostanie wyświetlony błąd "nie można odnaleźć tabeli", sprawdź, czy przeglądanie do stron, które wykonują śledzenie można włączyć **rejestrowania aplikacji (magazyn)** i po kliknięciu przycisku **zapisać**.)

     ![Dzienniki magazynu](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-storagelogs.png)

     Zobacz powiadomienie, że w tym widoku, który **identyfikator procesu** i **identyfikator wątku** dla każdego dziennika, który nie możesz uzyskać w dzienniku systemu plików. Dodatkowe pola można wyświetlić, korzystając bezpośrednio w tabeli magazynu systemu Azure.
13. Kliknij przycisk **Wyświetl wszystkie dzienniki aplikacji**.

     W tabeli dziennika śledzenia występuje w podglądzie tabeli magazynu systemu Azure.

     (Jeśli zostanie wyświetlony komunikat o błędzie "Sekwencja nie zawiera elementów", otwórz **Eksploratora serwera**, rozwiń węzeł dla konta magazynu w obszarze **Azure** węzeł, a następnie kliknij prawym przyciskiem myszy **tabel** i kliknij przycisk **Odśwież**.)

     ![Dzienniki magazynu w widoku tabeli](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracelogtableview.png)

     Ten widok przedstawia dodatkowe pola nie jest widoczny w innych widokach. Ten widok umożliwia można filtrować przy użyciu specjalnego interfejsu użytkownika konstruktora zapytań tworzenia kwerendy do dzienników. Aby uzyskać więcej informacji, zobacz temat pracy z zasobami tabeli — filtrowania jednostek w [przeglądanie zasobami magazynu za pomocą Eksploratora serwera](http://msdn.microsoft.com/library/ff683677.aspx).
14. Do przeglądania informacji w pojedynczym wierszu, kliknij dwukrotnie jeden z wierszy.

     ![Tabela śledzenia w Eksploratorze serwera](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracetablerow.png)

## <a name="failedrequestlogs"></a>Wyświetl dzienniki śledzenia nieudanych żądań
Dzienniki śledzenia nieudanych żądań są przydatne, gdy trzeba poznać szczegóły jak obsługuje żądania HTTP, w scenariuszach, takich jak problemy uwierzytelniania lub ponowne zapisywanie adresów URL usług IIS.

Aplikacje sieci web platformy Azure wykorzystują te same funkcje śledzenia nieudanych żądań, które było dostępne w usługach IIS 7.0 i nowszych. Nie masz dostępu do skonfigurowanych błędy, które mają być rejestrowane, ale ustawienia usług IIS. Po włączeniu śledzenia nieudanych żądań, wszystkie błędy są przechwytywane.

Śledzenie nieudanych żądań można włączyć za pomocą programu Visual Studio, ale nie można wyświetlić je w programie Visual Studio. Te dzienniki są plikami XML. Usługa przesyłania strumieniowego dzienników monitoruje tylko pliki, które zostaną uznane za czytelnych w trybie zwykłego tekstu: *.txt*, *.html*, i *log* plików.

Dzienniki śledzenia nieudanych żądań można wyświetlić w przeglądarce bezpośrednio przy użyciu usługi FTP, lub po lokalnie za pomocą narzędzia FTP, aby je pobrać do komputera lokalnego. W tej sekcji można będzie je wyświetlić w przeglądarce bezpośrednio.

1. W **konfiguracji** na karcie **aplikacji sieci Web Azure** okna, który został otwarty z **Eksploratora serwera**, zmień **śledzenia nieudanych żądań** do **na**, a następnie kliknij przycisk **zapisać**.

    ![Włącz śledzenie nieudanych żądań](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequeston.png)
2. Na pasku adresu w przeglądarce, pokazujący aplikacji sieci web Dodaj nadmiarowe znaki do adresu URL i naciśnij klawisz Enter, aby spowodować błąd 404.

    Powoduje to, że dziennik śledzenia nieudanych żądań, należy utworzyć, a w następujących krokach przedstawiono sposób wyświetlić lub pobrać dziennika.
3. W programie Visual Studio w **konfiguracji** karcie **aplikacji sieci Web Azure** okna, kliknij przycisk **Otwórz w portalu zarządzania**.
4. W [Azure Portal](https://portal.azure.com) **ustawienia** bloku aplikacji sieci web, kliknij przycisk **poświadczenia wdrażania**, a następnie wprowadź nową nazwę użytkownika i hasło.

    ![Nowa FTP nazwa użytkownika i hasło](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-enterftpcredentials.png)

    ** Podczas logowania, należy użyć pełnej nazwy z prefiksem do niego nazwa aplikacji sieci web. Na przykład jeśli witryna jest "mój_przykład" Wprowadź "myid" jako nazwy użytkownika, logujesz się jako "myexample\myid".
5. W nowym oknie przeglądarki, przejdź do adresu URL, który jest wyświetlany w obszarze **nazwa hosta FTP** lub **FTPS hostname** w **aplikacji sieci Web** bloku aplikacji sieci web.
6. Zaloguj się przy użyciu poświadczeń FTP, które zostały utworzone wcześniej (w tym prefiks nazwy aplikacji sieci web dla nazwy użytkownika).

    Przeglądarka zawiera folder główny aplikacji sieci web.
7. Otwórz *LogFiles* folderu.

    ![Otwórz LogFiles folder](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilesfolder.png)
8. Otwórz folder o nazwie W3SVC plus wartość liczbową.

    ![Otwórz W3SVC folder](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfolder.png)

    Folder zawiera pliki XML błędów, które zostały zarejestrowane po włączeniu śledzenia nieudanych żądań i pliku XSL używanego przez przeglądarkę do formatu XML.

    ![W3SVC folder](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfoldercontents.png)
9. Kliknij plik XML dla nieudanych żądań, które mają być wyświetlane informacje śledzenia dla.

    Na poniższej ilustracji przedstawiono część informacji śledzenia dla błędu próbki.

    ![Śledzenie nieudanych żądań w przeglądarce](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequestinbrowser.png)

## <a name="nextsteps"></a>Następne kroki
Przedstawiono sposób Visual Studio ułatwia przeglądanie dzienników utworzone przez aplikację sieci web platformy Azure. Poniższe sekcje zawierają łącza do większej ilości zasobów na tematy pokrewne:

* Rozwiązywanie problemów aplikacji sieci web platformy Azure
* Debugowanie w programie Visual Studio
* Zdalne debugowanie na platformie Azure
* Śledzenie w aplikacjach ASP.NET
* Analizowanie dzienników serwera sieci web
* Analizowanie dzienników śledzenia nieudanych żądań
* Debugowanie usług w chmurze

### <a name="azure-web-app-troubleshooting"></a>Rozwiązywanie problemów aplikacji sieci web platformy Azure
Aby uzyskać więcej informacji na temat rozwiązywania problemów z aplikacjami sieci web w usłudze Azure App Service zobacz następujące zasoby:

* [Jak monitorować aplikacje sieci web](/manage/services/web-sites/how-to-monitor-websites/)
* [Badanie przecieki pamięci w aplikacji sieci Web platformy Azure za pomocą programu Visual Studio 2013](http://blogs.msdn.com/b/visualstudioalm/archive/2013/12/20/investigating-memory-leaks-in-azure-web-sites-with-visual-studio-2013.aspx). Microsoft ALM wpis w blogu o funkcje programu Visual Studio do analizowania problemów pamięci zarządzanej.
* [Narzędzia online aplikacji sieci web platformy Azure należy wiedzieć o](https://azure.microsoft.com/blog/2014/03/28/windows-azure-websites-online-tools-you-should-know-about-2/). Wpis w blogu przez firmę Apple Amitowi.

Aby uzyskać pomoc dotyczącą konkretne pytanie dotyczące rozwiązywania problemów należy uruchomić w jednym z następujących fora wątku:

* [Azure forum w witrynie platformy ASP.NET](http://forums.asp.net/1247.aspx/1?Azure+and+ASP+NET).
* [Azure forum w witrynie MSDN](http://social.msdn.microsoft.com/Forums/windowsazure/).
* [StackOverflow.com](http://www.stackoverflow.com).

### <a name="debugging-in-visual-studio"></a>Debugowanie w programie Visual Studio
Aby uzyskać więcej informacji o sposobie używania tryb debugowania w programie Visual Studio, zobacz [debugowania w programie Visual Studio](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx) MSDN, tematu i [debugowania etykietki z programu Visual Studio 2010](http://weblogs.asp.net/scottgu/archive/2010/08/18/debugging-tips-with-visual-studio-2010.aspx).

### <a name="remote-debugging-in-azure"></a>Zdalne debugowanie na platformie Azure
Aby uzyskać więcej informacji na temat zdalnego debugowania dla aplikacji sieci web platformy Azure i zadań Webjob zobacz następujące zasoby:

* [Wprowadzenie do zdalne debugowanie aplikacji sieci Web usługi aplikacji Azure](https://azure.microsoft.com/blog/2014/05/06/introduction-to-remote-debugging-on-azure-web-sites/).
* [Wprowadzenie do zdalnego debugowania aplikacji usługi aplikacje sieci Web Azure część 2 - wewnątrz debugowanie zdalne](https://azure.microsoft.com/blog/2014/05/07/introduction-to-remote-debugging-azure-web-sites-part-2-inside-remote-debugging/)
* [Wprowadzenie do zdalnego debugowania na aplikacje sieci Web usługi aplikacji Azure część 3 - w środowisku wielu wystąpień i GIT](https://azure.microsoft.com/blog/2014/05/08/introduction-to-remote-debugging-on-azure-web-sites-part-3-multi-instance-environment-and-git/)
* [Zadania Webjob debugowania (klip wideo)](https://www.youtube.com/watch?v=ncQm9q5ZFZs&list=UU_SjTh-ZltPmTYzAybypB-g&index=1)

Jeśli aplikacja sieci web używa interfejsu API sieci Web Azure lub usługi Mobile Services zaplecza i musisz debugować, zobacz [debugowania zaplecza .NET w programie Visual Studio](http://blogs.msdn.com/b/azuremobile/archive/2014/03/14/debugging-net-backend-in-visual-studio.aspx).

### <a name="tracing-in-aspnet-applications"></a>Śledzenie w aplikacjach ASP.NET
Nie dostępnych nie szczegółowe i aktualne wprowadzeń do śledzenia ASP.NET w Internecie. Najlepiej, które może wykonywać jest wprowadzenie starego materiałów wprowadzające napisane specjalnie dla formularzy sieci Web ponieważ MVC i nie zostały jeszcze istnieje, które uzupełniają z nowszej blogu ogłoszenia skupiających się na konkretnych problemów. Niektóre dobre miejsca, aby uruchomić są następujące zasoby:

* [Monitorowanie i dane telemetryczne (tworzenia rzeczywistych aplikacji w chmurze platformy Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).<br>
  Książka elektroniczna rozdział z zaleceniami dotyczącymi śledzenie w aplikacjach w chmurze Azure.
* [Śledzenie na platformie ASP.NET](http://msdn.microsoft.com/library/ms972204.aspx)<br/>
  Stary, ale nadal dobrej zasobu wstęp do tematu.
* [Obiekty nasłuchujące śledzenia](http://msdn.microsoft.com/library/4y5y10s7.aspx)<br/>
  Informacje o obiektów nasłuchujących śledzenia, ale nie wspomina [WebPageTraceListener](http://msdn.microsoft.com/library/system.web.webpagetracelistener.aspx).
* [Wskazówki: Integrowanie śledzenie na platformie ASP.NET z System.Diagnostics śledzenia](http://msdn.microsoft.com/library/b0ectfxd.aspx)<br/>
  To zbyt jest stara, ale zawiera dodatkowe informacje, który nie obejmuje wprowadzające artykułu.
* [Śledzenie w widoki ASP.NET MVC Razor](http://blogs.msdn.com/b/webdev/archive/2013/07/16/tracing-in-asp-net-mvc-razor-views.aspx)<br/>
  Oprócz śledzenia w widokach Razor, post wyjaśniono również, jak utworzyć filtr błędu w celu rejestrowania wszystkich nieobsługiwanych wyjątków w aplikacji MVC. Aby uzyskać informacje na temat rejestrowania wszystkich nieobsługiwanych wyjątków w aplikacji formularzy sieci Web, zobacz przykład Global.asax w [pełny przykład dla programów obsługi błędu](http://msdn.microsoft.com/library/bb397417.aspx) w witrynie MSDN. W MVC i formularzy sieci Web Jeśli chcesz rejestrować pewne wyjątki, ale pozwól framework domyślne obsługi zaczęły obowiązywać, można przechwycić i rethrow jak w poniższym przykładzie:

        try
        {
           // Your code that might cause an exception to be thrown.
        }
        catch (Exception ex)
        {
            Trace.TraceError("Exception: " + ex.ToString());
            throw;
        }
* [Przesyłania strumieniowego śledzenia diagnostyki logowania z wiersza polecenia platformy Azure (oraz Glimpse!)](http://www.hanselman.com/blog/StreamingDiagnosticsTraceLoggingFromTheAzureCommandLinePlusGlimpse.aspx)<br/>
  Jak za pomocą wiersza polecenia wykonywać jakie w tym samouczku przedstawiono sposób wykonywania w programie Visual Studio. [Glimpse](http://www.hanselman.com/blog/IfYoureNotUsingGlimpseWithASPNETForDebuggingAndProfilingYoureMissingOut.aspx) to narzędzie do debugowania aplikacji ASP.NET.
* [Przy użyciu aplikacji sieci Web, rejestrowania i diagnostyki — z Ebbo Dominik](/documentation/videos/azure-web-site-logging-and-diagnostics/) i [przesyłania strumieniowego dzienników z aplikacji sieci Web — za pomocą Ebbo Dominik](/documentation/videos/log-streaming-with-azure-web-sites/)<br>
  Filmy wideo Scott Hanselman i Ebbo Dominika.

Do rejestrowania błędów zamiast zapisywania kod śledzenia jest używać struktury rejestrowania open source, takich jak [ELMAH](http://nuget.org/packages/elmah/). Aby uzyskać więcej informacji, zobacz [wpisy blogu Scott Hanselman o ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx).

Należy również zauważyć, że nie trzeba używać ASP.NET lub System.Diagnostics śledzenie, aby uzyskać podgląd dzienników przesyłanych strumieniowo z platformy Azure. Przesyłania strumieniowego usługi rejestrowania w dzienniku aplikacji sieci web platformy Azure będzie strumienia żadnego *.txt*, *.html*, lub *log* pliku znalezionego w *LogFiles* folderu. W związku z tym można utworzyć własne systemu rejestrowania, który zapisuje do systemu plików, aplikacji sieci web, a plik zostanie automatycznie przesyłane strumieniowo i pobrane. Musisz wykonać zapisu kodu aplikacji, która tworzy pliki w jest *d:\home\logfiles* folderu.

### <a name="analyzing-web-server-logs"></a>Analizowanie dzienników serwera sieci web
Aby uzyskać więcej informacji na temat analizowania dzienniki serwera sieci web zobacz następujące zasoby:

* [LogParser](http://www.microsoft.com/download/details.aspx?id=24659)<br/>
  Narzędzia do wyświetlania danych w dzienniki serwera sieci web (*log* plików).
* [Rozwiązywanie problemów z wydajnością usług IIS lub błędy aplikacji przy użyciu LogParser](http://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser)<br/>
  Wprowadzenie do narzędzia Log Parser, która służy do analizowania dzienniki serwera sieci web.
* [Blog ogłoszenia przy Roberta Mcmurraya przy użyciu LogParser](http://blogs.msdn.com/b/robert_mcmurray/archive/tags/logparser/)<br/>
* [Kod stanu HTTP w usługach IIS 7.0, IIS 7.5 i IIS 8.0](http://support.microsoft.com/kb/943891)

### <a name="analyzing-failed-request-tracing-logs"></a>Analizowanie dzienników śledzenia nieudanych żądań
Zawiera witrynę sieci Web Microsoft TechNet [za pomocą śledzenia nieudanych żądań](http://www.iis.net/learn/troubleshoot/using-failed-request-tracing) sekcji, które mogą być przydatne dla zrozumienia sposobu korzystania z nich. Jednak ta dokumentacja skupiono się głównie na temat konfigurowania śledzenia nieudanych żądań w usługach IIS, które nie są możliwe w aplikacjach sieci Web platformy Azure.

[GetStarted]: app-service-web-get-started-dotnet.md
[GetStartedWJ]: https://github.com/Azure/azure-webjobs-sdk/wiki
