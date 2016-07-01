<properties
    pageTitle="Wprowadzenie do usług Azure Mobile Services dla aplikacji systemu iOS | Zaplecze JavaScript"
    description="Wykonaj czynności opisane w tym samouczku, aby rozpocząć korzystanie z usług Azure Mobile Services w celu opracowywania aplikacji systemu iOS."
    services="mobile-services"
    documentationCenter="ios"
    authors="krisragh"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="03/09/2016"
    ms.author="krisragh"/>

# <a name="getting-started"> </a>Wprowadzenie do usługi Mobile Services

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Aby zapoznać się z wersją tego tematu dotyczącą funkcji Mobile Apps, zobacz artykuł [Tworzenie aplikacji dla systemu iOS w usłudze Azure Mobile Apps](../app-service-mobile/app-service-mobile-ios-get-started.md).

W tym samouczku przedstawiono sposób dodawania usługi zaplecza opartej na chmurze do aplikacji iOS przy użyciu usług Azure Mobile Services.

Wykonując kroki tego samouczka, utworzysz nową usługę mobilną i prostą aplikację _To do list_ (Lista zadań do wykonania), która przechowuje dane aplikacji w nowej usłudze mobilnej. Usługa mobilna, która zostanie utworzona, będzie korzystać z języka JavaScript do obsługi logiki biznesowej po stronie serwera. Aby utworzyć na platformie .NET usługę mobilną z logiką biznesową po stronie serwera, zobacz sekcję [Wersja zaplecza .NET] w tym temacie.

> [AZURE.NOTE] Do ukończenia tego samouczka jest potrzebne konto platformy Azure. Jeśli nie masz konta, możesz utworzyć konto w wersji próbnej platformy Azure i uzyskać [bezpłatne usługi mobilne, z których możesz korzystać nawet po upływie ważności wersji próbnej](https://azure.microsoft.com/pricing/details/mobile-services/). Aby uzyskać szczegółowe informacje, zobacz temat [Bezpłatna wersja próbna systemu Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started-ios%2F%20).

## <a name="create-new-service"> </a>Tworzenie nowej usługi mobilnej

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Tworzenie nowej aplikacji iOS

W celu utworzenia nowej aplikacji połączonej z usługą mobilną można użyć procedury szybkiego startu w klasycznym portalu Azure:

1. W [klasyczny portal Azure] kliknij pozycję **Mobile Services**, a następnie kliknij wcześniej utworzoną usługę mobilną.

2. Na karcie szybkiego startu kliknij opcję **iOS** w obszarze **Wybierz platformę** i rozwiń listę **Create a new iOS app** (Utwórz nową aplikację iOS). Zostaną wyświetlane trzy łatwe kroki umożliwiające utworzenie aplikacji systemu iOS połączonej z usługą mobilną.

3. Kliknij pozycję **Create TodoItem table** (Utwórz tabelę zadań do wykonania), aby utworzyć tabelę do przechowywania danych aplikacji.

4. W obszarze **Download and run your app** (Pobierz i uruchom aplikację) kliknij opcję **Download** (Pobierz). Spowoduje to pobranie projektu przykładowej aplikacji _To do list_ (Lista zadań do wykonania), która jest połączona z usługą mobilną, wraz z zestawem SDK usług Mobile Services systemu iOS. Zapisz skompresowany plik projektu na komputerze lokalnym i zapamiętaj, gdzie został on zapisany.

## Uruchamianie nowej aplikacji systemu iOS

[AZURE.INCLUDE [mobile-services-ios-run-app](../../includes/mobile-services-ios-run-app.md)]

<ol start="4">
<li><p>Otwórz ponownie [klasyczny portal Azure] i kliknij kartę **DANE**, a następnie kliknij tabelę **TodoItems**. Pozwoli to przeglądać dane wstawiane przez aplikację do tabeli.<p></li></ol></p>

## <a name="next-steps"> </a>Następne kroki
Dowiedz się, jak wykonać dodatkowe ważne zadania w usługach Mobile Services:

* [Wprowadzenie do synchronizacji danych w trybie offline]
    <br/>Dowiedz się, jak korzystać z synchronizacji danych w trybie offline, aby zapewnić dynamiczne i niezawodne działanie aplikacji.

* [Dodawanie uwierzytelniania do istniejącej aplikacji]
    <br/>Dowiedz się, jak uwierzytelniać użytkowników aplikacji przy użyciu dostawcy tożsamości.

* [Dodawanie powiadomień wypychanych do istniejącej aplikacji]
    <br/>Dowiedz się, jak wysyłać do aplikacji bardzo proste powiadomienia wypychane.

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]


<!-- Anchors. -->
[Wprowadzenie do usługi Mobile Services]:#getting-started
[Tworzenie nowej usługi mobilnej]:#create-new-service
[Definiowanie wystąpienia usługi mobilnej]:#define-mobile-service-instance
[Następne kroki]:#next-steps

<!-- Images. -->
[6]: ./media/mobile-services-ios-get-started/mobile-portal-quickstart-ios.png
[7]: ./media/mobile-services-ios-get-started/mobile-quickstart-steps-ios.png
[8]: ./media/mobile-services-ios-get-started/mobile-xcode-project.png

[10]: ./media/mobile-services-ios-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/mobile-services-ios-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-ios-get-started/mobile-data-browse.png


<!-- URLs. -->
[Wprowadzenie do synchronizacji danych w trybie offline]: mobile-services-ios-get-started-offline-data.md
[Dodawanie uwierzytelniania do istniejącej aplikacji]: mobile-services-dotnet-backend-ios-get-started-users.md
[Dodawanie powiadomień wypychanych do istniejącej aplikacji]: mobile-services-dotnet-backend-ios-get-started-push.md


[Zestaw SDK usług Mobile Services systemu iOS]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[klasyczny portal Azure]: https://manage.windowsazure.com/
[XCode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Wersja zaplecza .NET]: mobile-services-dotnet-backend-ios-get-started.md



<!--HONumber=Jun16_HO2-->


