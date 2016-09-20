<properties
    pageTitle="Wprowadzenie do usługi Mobile Services dla aplikacji platformy Xamarin iOS | Microsoft Azure"
    description="Wykonaj czynności opisane w tym samouczku, aby rozpocząć korzystanie z usług Azure Mobile Services w celu opracowywania aplikacji dla systemu Xamarin iOS."
    services="mobile-services"
    documentationCenter="xamarin"
    authors="lindydonna"
    manager="dwrede"
    editor="mollybos"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="07/21/2016"
    ms.author="donnam"/>

# <a name="getting-started"> </a>Wprowadzenie do usług Mobile Services

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Aby zapoznać się z wersją tego tematu dotyczącą funkcji Mobile Apps, zobacz artykuł [Tworzenie aplikacji na platformie Xamarin.iOS](../app-service-mobile/app-service-mobile-xamarin-ios-get-started.md).

W tym samouczku przedstawiono sposób dodawania opartej na chmurze usługi zaplecza do aplikacji platformy Xamarin.iOS przy użyciu usług Azure Mobile Services. Wykonując kroki tego samouczka, utworzysz nową usługę mobilną i prostą aplikację _To do list_ (Lista zadań do wykonania), która przechowuje dane aplikacji w nowej usłudze mobilnej. Usługa mobilna, która zostanie utworzona, korzysta z obsługiwanych języków .NET przy użyciu programu Visual Studio do obsługi logiki biznesowej po stronie serwera oraz do zarządzania usługami mobilnymi. Aby utworzyć usługę mobilną, która umożliwia zapisanie logiki biznesowej po stronie serwera w języku JavaScript, zobacz część [Wersja zaplecza JavaScript] w tym temacie.

>[AZURE.NOTE]W tym temacie przedstawiono sposób tworzenia nowego projektu usługi mobilnej przy użyciu klasycznego portalu Azure. Korzystając z programu Visual Studio 2013 Update 2, można również dodać nowy projekt usługi mobilnej do istniejącego rozwiązania Visual Studio. Aby uzyskać więcej informacji, zobacz temat [Quickstart: Add a mobile service (.NET backend)](http://msdn.microsoft.com/library/windows/apps/dn629482.aspx) (Szybki start: dodawanie usługi mobilnej (zaplecze .NET))

Zrzut ekranu gotowej aplikacji znajduje się poniżej:

![][0]


Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usług Mobile Services dotyczących aplikacji platformy Xamarin iOS.

>[AZURE.NOTE]Do ukończenia tego samouczka jest potrzebne konto platformy Azure. Jeśli nie masz takiego konta, możesz je utworzyć w wersji próbnej platformy Azure i uzyskać maksymalnie 10 bezpłatnych usług mobilnych, z których można korzystać nawet po wygaśnięciu wersji próbnej. Aby uzyskać szczegółowe informacje, zobacz temat <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-xamarin-ios-get-started" target="_blank">Bezpłatna wersja próbna systemu Azure</a>.<br />Ten samouczek wymaga programu <a href="https://go.microsoft.com/fwLink/p/?LinkID=257546" target="_blank">Visual Studio Professional 2013</a>. Dostępna jest bezpłatna wersja próbna.

## Tworzenie nowej usługi mobilnej

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## Tworzenie nowej aplikacji platformy Xamarin.iOS

Po utworzeniu usługi mobilnej można użyć funkcji szybkiego startu w klasycznym portalu Azure, aby utworzyć nową lub zmodyfikować istniejącą aplikację w celu nawiązania połączenia z usługą mobilną.

W tej części pobierzesz nową aplikację systemu Xamarin iOS oraz projekt usługi mobilnej.

1. Jeśli jeszcze tego nie zrobiono, zainstaluj program Visual Studio z platformą Xamarin. Instrukcje można znaleźć w temacie [Konfigurowanie i instalowanie oprogramowania Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). Możesz również użyć programu Xamarin Studio na komputerze z systemem Mac OS X. W tym celu zapoznaj się z tematem [Setup, install, and verifications for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (Konfiguracja, instalacja i weryfikacja dla użytkowników komputerów Mac).

2. W [klasyczny portal Azure] kliknij pozycję **Mobile Services**, a następnie kliknij wcześniej utworzoną usługę mobilną.

3. Na karcie szybkiego startu kliknij pozycję **Xamarin** w obszarze **Wybierz platformę** i rozwiń listę **Utwórz nową aplikację platformy Xamarin**.

    ![][6]

    Zostaną wyświetlone trzy łatwe kroki umożliwiające utworzenie aplikacji platformy Xamarin.iOS połączonej z usługą mobilną.

    ![][7]

4. W obszarze **Download and publish your service to the cloud** (Pobierz i opublikuj usługę w chmurze) wybierz opcję **iOS** i kliknij pozycję **Pobierz**.

    Spowoduje to pobranie rozwiązania zawierającego projekty zarówno dla usługi mobilnej, jak i przykładowej aplikacji _Lista zadań do wykonania_, która jest połączona z usługą mobilną. Zapisz skompresowany plik projektu na komputerze lokalnym i zapamiętaj, gdzie został on zapisany.

5. Pobierz profil publikowania, zapisz pobrany plik na komputerze lokalnym i zapamiętaj miejsce jego zapisania.

## Testowanie usługi mobilnej

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

## Publikowanie usługi mobilnej

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

## Uruchamianie aplikacji platformy Xamarin.iOS

Ostatnim krokiem tego samouczka jest skompilowanie i uruchomienie nowej aplikacji.

1. Przejdź do projektu klienta w ramach rozwiązania usługi mobilnej, korzystając z programu Visual Studio lub Xamarin Studio.

    ![][8]

    ![][9]

2. Kliknij przycisk **Run** (Uruchom), aby skompilować projekt klienta i uruchomić aplikację w emulatorze urządzenia iPhone.

3. W aplikacji wpisz łatwy do rozpoznania tekst (np. _Ukończenie samouczka_), a następnie kliknij ikonę plusa (**+**).

    ![][10]

    Spowoduje to wysłanie żądania POST do nowej usługi mobilnej, która jest hostowana na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez usługę mobilną, a dane są wyświetlane na liście.

>[AZURE.NOTE]Możesz zweryfikować kod uzyskujący dostęp do aplikacji mobilnej w celu wykonywania zapytań i wstawiania danych, który znajduje się w pliku języka C# QSTodoService.cs.


## Następne kroki
Po zakończeniu procedury szybkiego startu dowiedz się, jak wykonać dodatkowe ważne zadania w usłudze Mobile Services:

* [Wprowadzenie do synchronizacji danych w trybie offline]
  <br/>Dowiedz się, jak w ramach szybkiego startu używana jest synchronizacja danych w trybie offline, aby zapewnić dynamiczne i niezawodne działanie aplikacji.

* [Wprowadzenie do uwierzytelniania]
  <br/>Dowiedz się, jak uwierzytelniać użytkowników aplikacji przy użyciu dostawcy tożsamości.

* [Wprowadzenie do powiadomień wypychanych]
  <br/>Dowiedz się, jak wysyłać do aplikacji bardzo proste powiadomienia wypychane.

* [Rozwiązywanie problemów z zapleczem usługi Mobile Services na platformie .NET]
  <br/> Dowiedz się, jak diagnozować i naprawiać ewentualne problemy dotyczące zaplecza usługi Mobile Services na platformie .NET.

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Wprowadzenie do usługi Mobile Services]:#getting-started
[Tworzenie nowej usługi mobilnej]:#create-new-service
[Następne kroki]:#next-steps



<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-quickstart-completed-ios.png
[6]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-portal-quickstart-xamarin-ios.png
[7]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-quickstart-steps-xamarin-ios.png
[8]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Wprowadzenie do synchronizacji danych w trybie offline]: mobile-services-xamarin-ios-get-started-offline-data.md
[Wprowadzenie do uwierzytelniania]: mobile-services-dotnet-backend-xamarin-ios-get-started-users.md
[Wprowadzenie do powiadomień wypychanych]: mobile-services-dotnet-backend-xamarin-ios-get-started-push.md
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Zestaw SDK usług Mobile Services]: http://go.microsoft.com/fwlink/?LinkId=257545
[JavaScript i HTML]: mobile-services-win8-javascript/
[klasyczny portal Azure]: https://manage.windowsazure.com/
[Wersja zaplecza JavaScript]: mobile-services-ios-get-started.md
[Rozwiązywanie problemów z zapleczem usługi Mobile Services na platformie .NET]: mobile-services-dotnet-backend-how-to-troubleshoot.md


<!--HONumber=sep16_HO1-->


