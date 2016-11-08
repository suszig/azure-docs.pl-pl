---
title: Wprowadzenie do usług Azure Mobile Services dla aplikacji PhoneGap/Cordova | Microsoft Docs
description: Czynności opisane w tym samouczku ułatwią rozpoczęcie programowania rozwiązań PhoneGap dla systemów iOS, Android i Windows Phone za pomocą usług Azure Mobile Services .
services: mobile-services
documentationcenter: ''
author: ggailey777
manager: dwrede
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-phonegap
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 07/21/2016
ms.author: ggailey777

---
# Wprowadzenie do usług Mobile Services
[!INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[!INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

W tym samouczku przedstawiono sposób dodawania usługi zaplecza do aplikacji opartej na chmurze przy użyciu usług Azure Mobile Services. Wykonując kroki tego samouczka, utworzysz nową usługę mobilną i prostą aplikację *To do list* (Lista zadań do wykonania), która przechowuje dane aplikacji w nowej usłudze mobilnej.

Zrzut ekranu gotowej aplikacji znajduje się poniżej:

![][3]

### Wymagania dodatkowe
Do skorzystania z tego samouczka wymagane są następujące elementy:

* Narzędzia PhoneGap (w przypadku projektów Windows Phone 8 wymagana jest wersja 3.2 lub nowsza).
* Aktywne konto platformy Microsoft Azure.
* PhoneGap umożliwia programowanie dla wielu platform. Oprócz narzędzi PhoneGap trzeba zainstalować narzędzia odpowiednie do wszystkich platform docelowych:
  
  * System Windows Phone: zainstaluj program [Visual Studio 2012 Express for Windows Phone](https://go.microsoft.com/fwLink/p/?LinkID=268374)
  * System iOS: zainstaluj program [Xcode](wymagana wersja 4.4 lub nowsza)
  * System Android: zainstaluj [narzędzia dla deweloperów systemu Android][Android SDK]
      <br/>(Zestaw SDK usług Mobile Services dla systemu Android obsługuje aplikacje dla systemu Android w wersji 2.2 lub nowszej. Do uruchomienia aplikacji szybki start wymagany jest system Android 4.2 lub nowszy).

## Tworzenie nowej usługi mobilnej
[!INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Tworzenie nowej aplikacji PhoneGap
W tej części utworzysz nową aplikację PhoneGap połączoną z usługą mobilną.

1. W [klasyczny portal Azure] kliknij pozycję **Mobile Services**, a następnie kliknij wcześniej utworzoną usługę mobilną.
2. Na karcie szybkiego startu kliknij opcję **PhoneGap** w obszarze **Wybierz platformę** i rozwiń listę **Create a new PhoneGap app** (Utwórz nową aplikację systemu PhoneGap).
   
    ![][0]
   
    Zostaną wyświetlone trzy proste kroki umożliwiające utworzenie aplikacji systemu PhoneGap i połączenie jej z usługą mobilną.
   
    ![][1]
3. Jeśli jeszcze tego nie zrobiono, pobierz i zainstaluj program PhoneGap i co najmniej jedno z narzędzi programistycznych platformy (Windows Phone, iOS lub Android).
4. Kliknij pozycję **Create TodoItem table** (Utwórz tabelę zadań do wykonania), aby utworzyć tabelę do przechowywania danych aplikacji.
5. W obszarze **Download and run your app** (Pobierz i uruchom aplikację) kliknij opcję **Download** (Pobierz).
   
    Spowoduje to pobranie projektu przykładowej aplikacji *To do list* (Lista zadań do wykonania), która jest połączona z usługą mobilną, wraz z zestawem SDK JavaScript usług Mobile Services. Zapisz skompresowany plik projektu na komputerze lokalnym i zapamiętaj, gdzie został on zapisany.

## Uruchamianie nowej aplikacji PhoneGap
Ostatnim krokiem tego samouczka jest skompilowanie i uruchomienie nowej aplikacji.

1. Przejdź do lokalizacji, w której zapisano skompresowane pliki projektu, i wyodrębnij je na komputerze.
2. Otwórz i uruchom projekt zgodnie z poniższymi instrukcjami dotyczącymi poszczególnych platform.
   
   * **Windows Phone 8**
     
     1. Windows Phone 8: otwórz plik .sln w katalogu **platforms\wp8** w programie Visual Studio 2012 Express for Windows Phone.
     2. Naciśnij klawisz **F5**, aby ponownie skompilować projekt i uruchomić aplikację.
        
        ![][2]
   * **iOS**
     
     1. Otwórz projekt w katalogu **platforms/ios** w programie Xcode.
     2. Kliknij przycisk **Run** (Uruchom), aby skompilować projekt i uruchomić aplikację w emulatorze iPhone, co jest ustawieniem domyślnym dla tego projektu.
        
        ![][3]
   * **Android**
     
     1. W programie Eclipse kliknij przycisk **File** (Plik), następnie **Import** (Importuj), rozwiń węzeł **Android**, kliknij przycisk **Existing Android Code into Workspace** (Istniejący kod Android do obszaru roboczego), a następnie kliknij przycisk **Next** (Dalej).
     2. Kliknij przycisk **Browse** (Przeglądaj), przejdź do lokalizacji wyodrębnionych plików projektu, kliknij przycisk **OK**, upewnij się, że projekt TodoActivity jest zaznaczony, a następnie kliknij przycisk **Finish** (Zakończ). <p>Pliki projektu zostaną zaimportowane do bieżącego obszaru roboczego.</p>
     3. W menu **Run** (Uruchom) kliknij pozycję **Run** (Uruchom), aby uruchomić projekt w emulatorze systemu Android.
        
         ![][4]
        
        > [!NOTE]
        > Aby można było uruchomić projekt w emulatorze systemu Android, należy zdefiniować co najmniej jedno wirtualne urządzenie Android (Android Virtual Device, AVD). Do tworzenia tych urządzeń i zarządzania nimi służy Menedżer AVD.
        > 
        > 
3. Po uruchomieniu aplikacji w jednym z powyższych emulatorów urządzeń mobilnych, wpisz dowolny tekst w polu tekstowym, a następnie kliknij przycisk **Add** (Dodaj).
   
    Spowoduje to wysłanie żądania POST do nowej usługi mobilnej, która jest hostowana na platformie Azure. Dane z żądania zostaną wstawione do tabeli **TodoItem**. Elementy przechowywane w tabeli są zwracane przez usługę mobilną, a dane są wyświetlane na liście.
   
   > [!IMPORTANT]
   > Zmiany w tym projekcie platformy zostaną nadpisane, jeśli główny projekt zostanie skompilowany ponownie za pomocą narzędzi PhoneGap. Zamiast tego należy wprowadzić zmiany w głównym katalogu www projektu, zgodnie z opisem w poniższej części.
   > 
   > 
4. W [klasyczny portal Azure] kliknij kartę **Dane**, a następnie kliknij tabelę **TodoItem**.
   
    ![](./media/mobile-services-javascript-backend-phonegap-get-started/mobile-data-tab.png)
   
    Pozwoli to przeglądać dane wstawiane przez aplikację do tabeli.
   
    ![](./media/mobile-services-javascript-backend-phonegap-get-started/mobile-data-browse.png)

## Aktualizowanie aplikacji i ponowna kompilacja projektów dla poszczególnych platform
1. Wprowadź zmiany do plików kodu w katalogu „www” (w tym przypadku jest to katalog „todolist/www”).
2. Sprawdź, czy wszystkie narzędzia platformy docelowej są dostępne w ścieżce systemowej.
3. Otwórz wiersz polecenia w katalogu głównym projektu, a następnie uruchom następujące polecenia dotyczące danej platformy:
   
   * **Windows Phone**
     
       Uruchom następujące polecenie z wiersza polecenia programu Visual Studio Developer:
     
           phonegap local build wp8
   * **iOS**
     
       Otwórz terminal i uruchom następujące polecenie:
     
           phonegap local build ios
   * **Android**
     
       Otwórz wiersz polecenia lub okno terminalu i uruchom następujące polecenie.
     
           phonegap local build android
4. Otwórz każdy projekt w odpowiednim środowisku programistycznym, zgodnie z opisem w poprzedniej sekcji.

> [!NOTE]
> Możesz przejrzeć znajdujący się w pliku js/index.js kod uzyskujący dostęp do usługi mobilnej w celu wykonywania zapytań i wstawiania danych.
> 
> 

## Następne kroki
Po zakończeniu procedury szybkiego startu dowiedz się, jak wykonać dodatkowe ważne zadania w usłudze Mobile Services:

* **[Dodawanie uwierzytelniania do aplikacji]**  
  Dowiedz się, jak uwierzytelniać użytkowników aplikacji przy użyciu dostawcy tożsamości.  
* **[Dodawanie powiadomień wypychanych do aplikacji](https://msdn.microsoft.com/magazine/dn879353.aspx)**  
  Dowiedz się, jak przeprowadzać rejestrację i wysyłanie powiadomień wypychanych do aplikacji.
* **[Przewodnik koncepcyjny HTML/JavaScript dotyczący usługi Mobile Services](mobile-services-html-how-to-use-client-library.md)**  
  Dowiedz się więcej o sposobie używania biblioteki klienckiej JavaScript w celu dostępu do danych, wywoływania niestandardowych interfejsów API i uwierzytelniania.

[!INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Images. -->
[0]: ./media/mobile-services-javascript-backend-phonegap-get-started/portal-screenshot1.png
[1]: ./media/mobile-services-javascript-backend-phonegap-get-started/portal-screenshot2.png
[2]: ./media/mobile-services-javascript-backend-phonegap-get-started/mobile-portal-quickstart-wp8.png
[3]: ./media/mobile-services-javascript-backend-phonegap-get-started/mobile-portal-quickstart-ios.png
[4]: ./media/mobile-services-javascript-backend-phonegap-get-started/mobile-portal-quickstart-android.png

<!-- URLs. -->
[Dodawanie uwierzytelniania do aplikacji]: mobile-services-html-get-started-users.md
[Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=280125
[klasyczny portal Azure]: https://manage.windowsazure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio 2012 Express for Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374



<!--HONumber=sep16_HO1-->


