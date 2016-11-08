---
title: Wprowadzenie do usługi Mobile Services dla aplikacji platformy Xamarin iOS | Microsoft Docs
description: Wykonaj czynności opisane w tym samouczku, aby rozpocząć korzystanie z usług Azure Mobile Services w celu opracowywania aplikacji dla systemu Xamarin iOS.
services: mobile-services
documentationcenter: xamarin
author: conceptdev
manager: dwrede
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/21/2016
ms.author: craig.dunn@xamarin.com

---
# <a name="getting-started"> </a>Wprowadzenie do usług Mobile Services
[!INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Aby zapoznać się z wersją tego tematu dotyczącą funkcji Mobile Apps, zobacz artykuł [Tworzenie aplikacji na platformie Xamarin.iOS](../app-service-mobile/app-service-mobile-xamarin-ios-get-started.md).
> 
> 

W tym samouczku przedstawiono sposób dodawania opartej na chmurze usługi zaplecza do aplikacji platformy Xamarin.iOS przy użyciu usług Azure Mobile Services. Wykonując kroki tego samouczka, utworzysz nową usługę mobilną i prostą aplikację *To do list* (Lista zadań do wykonania), która przechowuje dane aplikacji w nowej usłudze mobilnej.

Jeśli wolisz obejrzeć film, w klipie poniżej przedstawiono te same kroki, które są opisane w samouczku.

Wideo: „Getting Started with Xamarin and Azure Mobile Services” (Wprowadzenie do platformy Xamarin i usług Azure Mobile Services) — prowadzący Craig Dunn, deweloper i propagator systemu Xamarin (czas trwania: 10:05 min)

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Getting-Started-with-Xamarin-and-Windows-Azure-Mobile-Services/player]
> 
> 

Zrzut ekranu gotowej aplikacji znajduje się poniżej:

![][0]

Wykonanie kroków tego samouczka wymaga programów XCode i Xamarin Studio dla systemu OS X lub programu Visual Studio w systemie Windows z dołączonym przez sieć komputerem Mac. Kompletne instrukcje instalacji można znaleźć w temacie [Konfigurowanie i instalowanie oprogramowania Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 

> [!IMPORTANT]
> Do ukończenia tego samouczka jest potrzebne konto platformy Azure. Jeśli nie masz takiego konta, możesz je utworzyć w wersji próbnej platformy Azure i uzyskać maksymalnie 10 bezpłatnych usług mobilnych, z których można korzystać nawet po wygaśnięciu wersji próbnej. Aby uzyskać szczegółowe informacje, zobacz temat [Bezpłatna wersja próbna systemu Azure](https://azure.microsoft.com/pricing/free-trial/).
> 
> 

## <a name="create-new-service"> </a>Tworzenie nowej usługi mobilnej
[!INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Tworzenie nowej aplikacji platformy Xamarin.iOS
Po utworzeniu usługi mobilnej można użyć funkcji szybkiego startu w klasycznym portalu Azure, aby utworzyć nową lub zmodyfikować istniejącą aplikację w celu nawiązania połączenia z usługą mobilną.

W tej sekcji utworzysz nową aplikację systemu Xamarin.iOS połączoną z usługą mobilną.

1. W [klasyczny portal Azure] kliknij pozycję **Mobile Services**, a następnie kliknij wcześniej utworzoną usługę mobilną.
2. Na karcie szybkiego startu kliknij pozycję **Xamarin.iOS** w obszarze **Wybierz platformę** i rozwiń listę **Utwórz nową aplikację platformy Xamarin.iOS**.
   
    ![][6]
   
    Zostaną wyświetlone trzy łatwe kroki umożliwiające utworzenie aplikacji platformy Xamarin.iOS połączonej z usługą mobilną.
   
    ![][7]
3. Jeśli nie zostało to jeszcze zrobione, pobierz i zainstaluj program Xcode (zalecamy najnowszą wersję Xcode 6.0 lub nowszą) i program [Xamarin Studio].
4. Kliknij pozycję **Create TodoItems table** (Utwórz tabelę zadań do wykonania), aby utworzyć tabelę do przechowywania danych aplikacji.
5. W obszarze **Download and run your app** (Pobierz i uruchom aplikację) kliknij pozycję **Download** (Pobierz).
   
    Spowoduje to pobranie projektu przykładowej aplikacji *To do list* (Lista zadań do wykonania), która jest połączona z usługą mobilną, wraz z odwołaniami do składnika usług Azure Mobile Services dla platformy Xamarin.iOS.  Zapisz skompresowany plik projektu na komputerze lokalnym i zapamiętaj, gdzie został on zapisany.

## Tworzenie nowej aplikacji platformy Xamarin.iOS
Ostatnim krokiem tego samouczka jest skompilowanie i uruchomienie nowej aplikacji.

1. Przejdź do lokalizacji, w której zapisano skompresowane pliki projektu, rozwiń pliki na komputerze, a następnie w programie Xamarin Studio lub Visual Studio otwórz plik rozwiązania **XamarinTodoQuickStart.iOS.sln**.
   
    ![][8]
   
    ![][9]
2. Kliknij przycisk **Run** (Uruchom), aby skompilować projekt i uruchomić aplikację w emulatorze iPhone, co jest ustawieniem domyślnym dla tego projektu.
3. W aplikacji wpisz łatwy do rozpoznania tekst (np. *Ukończenie samouczka*), a następnie kliknij ikonę plusa (**+**).
   
    ![][10]
   
    Spowoduje to wysłanie żądania POST do nowej usługi mobilnej, która jest hostowana na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez usługę mobilną, a dane są wyświetlane na liście.
   
   > [!NOTE]
   > Możesz zweryfikować kod uzyskujący dostęp do usługi mobilnej w celu wykonywania zapytań i wstawiania danych, który znajduje się w pliku języka C# ToDoService.cs.
   > 
   > 
4. W [klasyczny portal Azure] kliknij kartę **Dane**, a następnie kliknij tabelę **TodoItems**.
   
    ![][11]
   
    Pozwoli to przeglądać dane wstawiane przez aplikację do tabeli.
   
    ![][12]

## Następne kroki
Po zakończeniu procedury szybkiego startu dowiedz się, jak wykonać dodatkowe ważne zadania w usłudze Mobile Services:

* [Wprowadzenie do synchronizacji danych w trybie offline] Dowiedz się, jak w ramach szybkiego startu używana jest synchronizacja danych w trybie offline, aby zapewnić dynamiczne i niezawodne działanie aplikacji.
* [Wprowadzenie do uwierzytelniania] Dowiedz się, jak uwierzytelniać użytkowników aplikacji przy użyciu dostawcy tożsamości.
* [Wprowadzenie do powiadomień wypychanych] Dowiedz się, jak wysyłać do aplikacji bardzo proste powiadomienia wypychane.

[!INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Wprowadzenie do usługi Mobile Services]:#getting-started
[Tworzenie nowej usługi mobilnej]:#create-new-service
[Definiowanie wystąpienia usługi mobilnej]:#define-mobile-service-instance
[Następne kroki]:#next-steps

<!-- Images. -->
[0]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-completed-ios.png
[6]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-portal-quickstart-xamarin-ios.png
[7]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-steps-xamarin-ios.png
[8]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-xamarin-project-ios-xs.png
[9]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-xamarin-project-ios-vs.png
[10]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-data-tab.png
[12]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-data-browse.png


<!-- URLs. -->
[Wprowadzenie do synchronizacji danych w trybie offline]: mobile-services-xamarin-ios-get-started-offline-data.md
[Wprowadzenie do uwierzytelniania]: partner-xamarin-mobile-services-ios-get-started-users.md
[Wprowadzenie do powiadomień wypychanych]: partner-xamarin-mobile-services-ios-get-started-push.md

[Xamarin Studio]: http://xamarin.com/download
[Zestaw SDK usługi Mobile Services systemu iOS]: https://go.microsoft.com/fwLink/p/?LinkID=266533

[klasyczny portal Azure]: https://manage.windowsazure.com/



<!--HONumber=sep16_HO1-->


