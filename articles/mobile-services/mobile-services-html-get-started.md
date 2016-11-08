---
title: Wprowadzenie do usług Azure Mobile Services dla aplikacji HTML/JavaScript | Microsoft Docs
description: Wykonaj czynności opisane w tym samouczku, aby rozpocząć korzystanie z usług Azure Mobile Services w celu opracowywania aplikacji HTML.
services: mobile-services
documentationcenter: ''
author: ggailey777
manager: dwrede
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-html5
ms.devlang: javascript
ms.topic: get-started-article
ms.date: 07/21/2016
ms.author: glenga

---
# <a name="getting-started"> </a>Wprowadzenie do usług Mobile Services
[!INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[!INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

## Omówienie
W tym samouczku przedstawiono sposób dodawania usługi zaplecza opartej na chmurze do aplikacji HTML przy użyciu usług Azure Mobile Services. Wykonując kroki tego samouczka, utworzysz nową usługę mobilną i prostą aplikację *To do list* (Lista zadań do wykonania), która przechowuje dane aplikacji w nowej usłudze mobilnej. Można obejrzeć wersję wideo tego samouczka poniżej. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Getting-Started-with-the-Mobile-Services-HTML-Client/player]
> 
> 

Zrzut ekranu gotowej aplikacji znajduje się poniżej:

![][0]

Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usług Mobile Services dotyczących aplikacji HTML. W przypadku aplikacji PhoneGap/Cordova zobacz [wersję dotyczącą aplikacji PhoneGap/Cordova](mobile-services-javascript-backend-phonegap-get-started.md) tego samouczka.

## Wymagania wstępne
Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* Na komputerze lokalnym musi być uruchomiony jeden z następujących serwerów sieci Web:
  
  * **System Windows**: IIS Express. Serwer IIS Express jest instalowany przez [Instalatora platformy Microsoft Web].
  * **System MacOS X**: środowisko Python, które powinno być już zainstalowane.
  * **System Linux**: środowisko Python. Należy zainstalować [najnowszą wersję środowiska Python].
    
    Do hostowania aplikacji można użyć dowolnego serwera sieci Web, ale wymienione serwery sieci Web są obsługiwane przez pobierane skrypty.  
* Przeglądarka sieci Web, która obsługuje protokół HTML5.
* Konto platformy Azure. Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz temat [Bezpłatna wersja próbna systemu Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started-html%2F"%20target="_blank). 

## <a name="create-new-service"> </a>Tworzenie nowej usługi mobilnej
[!INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Tworzenie nowej aplikacji HTML
Po utworzeniu usługi mobilnej można użyć funkcji szybkiego startu w klasycznym portalu Azure, aby utworzyć nową lub zmodyfikować istniejącą aplikację w celu nawiązania połączenia z usługą mobilną.

W tej sekcji utworzysz nową aplikację HTML połączoną z usługą mobilną.

1. W [klasyczny portal Azure] kliknij pozycję **Mobile Services**, a następnie kliknij wcześniej utworzoną usługę mobilną.
2. Na karcie szybkiego startu kliknij opcję **Windows** w obszarze **Wybierz platformę** i rozwiń listę **Create a new HTML app** (Utwórz nową aplikację HTML).
   
    ![][6]
   
    Zostaną wyświetlane trzy łatwe kroki umożliwiające utworzenie i hostowanie aplikacji HTML połączonej z usługą mobilną.
   
    ![][7]
3. Kliknij pozycję **Create TodoItems table** (Utwórz tabelę zadań do wykonania), aby utworzyć tabelę do przechowywania danych aplikacji.
4. W obszarze **Download and run your app** (Pobierz i uruchom aplikację) kliknij opcję **Download** (Pobierz).
   
    Spowoduje to pobranie plików witryny sieci Web przykładowej aplikacji *Lista zadań do wykonania*, która jest połączona z usługą mobilną. Zapisz skompresowany plik na komputerze lokalnym i zanotuj miejsce zapisu.
5. Na karcie **Konfiguracja** sprawdź, czy nazwa `localhost` znajduje się już na liście **Zezwalaj na żądania z nazw hostów** w obszarze **Współużytkowanie zasobów między źródłami (CORS)**. Jeśli nie, wpisz ciąg `localhost` w polu **Nazwa hosta**, a następnie kliknij pozycję **Zapisz**.
   
    ![][9]
   
   > [!IMPORTANT]
   > Jeśli aplikacja szybkiego startu jest wdrażana na serwerze sieci Web innym niż localhost, należy dodać nazwę hosta serwera sieci Web do listy **Allow requests from host names** (Zezwalaj na żądania z nazw hostów). Aby uzyskać więcej informacji, zobacz temat [Współużytkowanie zasobów między źródłami](http://msdn.microsoft.com/library/windowsazure/dn155871.aspx).
   > 
   > 

## Hostowanie i uruchamianie aplikacji HTML
Ostatnim krokiem tego samouczka jest hostowanie i uruchomienie nowej aplikacji na komputerze lokalnym.

1. Przejdź do lokalizacji, w której zapisano skompresowane pliki projektu, rozwiń pliki na swoim komputerze, a następnie uruchom jeden z następujących plików poleceń w podfolderze **server**.
   
   * **launch-windows** (komputery z systemem Windows)
   * **launch-mac.command** (komputery z systemem Mac OS X)
   * **launch-linux.sh** (komputery z systemem Linux)
     
     > [!NOTE]
     > Na komputerze z systemem Windows wpisz polecenie `R`, gdy pojawi się monit o potwierdzenie uruchomienia skryptu programu PowerShell. Przeglądarka sieci Web może wyświetlić ostrzeżenie przed uruchomieniem skryptu, ponieważ został on pobrany z Internetu. W takim przypadku należy kontynuować ładowanie skryptu przez przeglądarkę.
     > 
     > 
     
     Spowoduje to uruchomienie serwera sieci Web na komputerze lokalnym w celu hostowania nowej aplikacji.
2. W przeglądarce sieci Web otwórz adres URL <a href="http://localhost:8000/" target="_blank">http://localhost:8000/</a>, aby uruchomić aplikację.
3. W aplikacji wpisz znaczący tekst w polu **Enter new text** (Wprowadź nowy tekst), na przykład *Ukończ samouczek*, a następnie kliknij przycisk **Dodaj**.
   
    ![][10]
   
    Spowoduje to wysłanie żądania POST do nowej usługi mobilnej, która jest hostowana na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez usługę mobilną, a dane są wyświetlane w drugiej kolumnie aplikacji.
   
   > [!NOTE]
   > Możesz zweryfikować znajdujący się w pliku page.js kod uzyskujący dostęp do usługi mobilnej w celu wykonywania zapytań i wstawiania danych.
   > 
   > 
4. W [klasyczny portal Azure] kliknij kartę **Dane**, a następnie kliknij tabelę **TodoItems**.
   
    ![][11]
   
    Pozwoli to przeglądać dane wstawiane przez aplikację do tabeli.
   
    ![][12]

## <a name="next-steps"> </a>Następne kroki
Po zakończeniu procedury szybkiego startu dowiedz się, jak wykonać dodatkowe ważne zadania w usłudze Mobile Services:

* **[Dodawanie uwierzytelniania do aplikacji]**  
  Dowiedz się, jak uwierzytelniać użytkowników aplikacji przy użyciu dostawcy tożsamości.
* **[Przewodnik koncepcyjny HTML/JavaScript dotyczący usługi Mobile Services]**  
  Dowiedz się więcej o sposobach korzystania z kodu HTML/JavaScript w usłudze Mobile Services

[!INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Wprowadzenie do usługi Mobile Services]:#getting-started
[Tworzenie nowej usługi mobilnej]:#create-new-service
[Definiowanie wystąpienia usługi mobilnej]:#define-mobile-service-instance
[Następne kroki]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-html-get-started/mobile-quickstart-completed-html.png

[6]: ./media/mobile-services-html-get-started/mobile-portal-quickstart-html.png
[7]: ./media/mobile-services-html-get-started/mobile-quickstart-steps-html.png

[9]: ./media/mobile-services-html-get-started/mobile-services-set-cors-localhost.png
[10]: ./media/mobile-services-html-get-started/mobile-quickstart-startup-html.png
[11]: ./media/mobile-services-html-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-html-get-started/mobile-data-browse.png


<!-- URLs. -->
[Dodawanie uwierzytelniania do aplikacji]: mobile-services-html-get-started-users.md

[klasyczny portal Azure]: https://manage.windowsazure.com/
[Instalatora platformy Microsoft Web]:  http://go.microsoft.com/fwlink/p/?LinkId=286333
[najnowszą wersję środowiska Python]: http://go.microsoft.com/fwlink/p/?LinkId=286342
[Przewodnik koncepcyjny HTML/JavaScript dotyczący usługi Mobile Services]: mobile-services-html-how-to-use-client-library.md
[Współużytkowanie zasobów między źródłami]: http://msdn.microsoft.com/library/azure/dn155871.aspx




<!--HONumber=sep16_HO1-->


