<properties
    pageTitle="Wprowadzenie do usługi Mobile Services dla aplikacji dla systemu Android na platformie Xamarin | Microsoft Azure"
    description="Czynności opisane w tym samouczku ułatwią rozpoczęcie tworzenia aplikacji dla systemu Android na platformie Xamarin przy użyciu usługi Azure Mobile Services."
    services="mobile-services"
    documentationCenter="xamarin"
    authors="lindydonna"
    manager="dwrede"
    editor="mollybos"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="01/14/2016"
    ms.author="donnam"/>

# <a name="getting-started"> </a>Wprowadzenie do usługi Mobile Services

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Aby zapoznać się z wersją tego tematu dotyczącą funkcji Mobile Apps, zobacz artykuł [Tworzenie aplikacji na platformie Xamarin.Android](../app-service-mobile/app-service-mobile-xamarin-android-get-started.md).

W tym samouczku opisano, jak dodać usługę zaplecza opartą na chmurze do aplikacji dla systemu Android tworzonej na platformie Xamarin, korzystając z usługi Mobile Services. Wykonując kroki tego samouczka, utworzysz nową usługę mobilną i prostą aplikację _To do list_ (Lista zadań do wykonania), która przechowuje dane aplikacji w nowej usłudze mobilnej. Usługa mobilna, która zostanie utworzona, korzysta z obsługiwanych języków .NET przy użyciu programu Visual Studio do obsługi logiki biznesowej po stronie serwera oraz do zarządzania usługami mobilnymi. Aby utworzyć usługę mobilną, która umożliwia zapisanie logiki biznesowej po stronie serwera w języku JavaScript, zobacz część [Wersja zaplecza JavaScript] w tym temacie.

>[AZURE.NOTE]W tym temacie przedstawiono sposób tworzenia nowego projektu usługi mobilnej przy użyciu klasycznego portalu Azure. Korzystając z programu Visual Studio 2013 Update 2, można również dodać nowy projekt usługi mobilnej do istniejącego rozwiązania Visual Studio. Aby uzyskać więcej informacji, zobacz temat [Quickstart: Add a mobile service (.NET backend)](http://msdn.microsoft.com/library/windows/apps/dn629482.aspx) (Szybki start: dodawanie usługi mobilnej (zaplecze .NET))

Zrzut ekranu gotowej aplikacji znajduje się poniżej:

![][0]

Ukończenie tego samouczka jest wymagane przed przejściem do innych samouczków usługi Mobile Services dotyczących aplikacji dla systemu Android tworzonych na platformie Xamarin.

>[AZURE.NOTE]Do ukończenia tego samouczka jest potrzebne konto platformy Azure. Jeśli nie masz takiego konta, możesz je utworzyć w wersji próbnej platformy Azure i uzyskać maksymalnie 10 bezpłatnych usług mobilnych, z których można korzystać nawet po wygaśnięciu wersji próbnej. Aby uzyskać szczegółowe informacje, zobacz temat [Bezpłatna wersja próbna systemu Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-xamarin-android-get-started).
>Ten samouczek wymaga programu [Visual Studio Professional 2013](https://go.microsoft.com/fwLink/p/?LinkID=257546). Dostępna jest bezpłatna wersja próbna.

## Tworzenie nowej usługi mobilnej

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## Tworzenie nowej aplikacji dla systemu Android na platformie Xamarin

Po utworzeniu usługi mobilnej można użyć funkcji szybkiego startu w portalu klasycznym, aby utworzyć nową aplikację (lub zmodyfikować istniejącą) oraz połączyć ją z usługą mobilną.

W tej części pobierzesz nową aplikację dla systemu Android tworzoną na platformie Xamarin oraz projekt usługi mobilnej.

1. Jeśli jeszcze tego nie zrobiono, zainstaluj program Visual Studio z platformą Xamarin. Instrukcje można znaleźć w temacie [Konfigurowanie i instalowanie oprogramowania Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). Możesz również użyć programu Xamarin Studio na komputerze z systemem Mac OS X. W tym celu zapoznaj się z tematem [Setup, install, and verifications for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (Konfiguracja, instalacja i weryfikacja dla użytkowników komputerów Mac).  

2. W [klasycznym portalu Azure] kliknij pozycję **Mobile Services**, a następnie kliknij wcześniej utworzoną usługę mobilną.

3. Na karcie szybkiego startu kliknij pozycję **Xamarin** w obszarze **Wybierz platformę** i rozwiń listę **Utwórz nową aplikację platformy Xamarin**.

    ![][6]

    Zostaną wyświetlone trzy proste kroki umożliwiające utworzenie aplikacji dla systemu Android na platformie Xamarin i połączenie jej z usługą mobilną.

    ![][7]

4. W obszarze **Pobierz i opublikuj usługę w chmurze** wybierz opcję **Android** i kliknij przycisk **Pobierz**.

    Spowoduje to pobranie rozwiązania zawierającego projekty zarówno dla usługi mobilnej, jak i przykładowej aplikacji _Lista zadań do wykonania_, która jest połączona z usługą mobilną. Zapisz skompresowany plik projektu na komputerze lokalnym i zapamiętaj, gdzie został on zapisany.

5. Pobierz profil publikowania, zapisz pobrany plik na komputerze lokalnym i zapamiętaj miejsce jego zapisania.

## Testowanie usługi mobilnej

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

## Publikowanie usługi mobilnej

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

## Uruchomienie aplikacji dla systemu Android na platformie Xamarin

Ostatnim krokiem tego samouczka jest skompilowanie i uruchomienie nowej aplikacji.

1. Przejdź do projektu klienta w ramach rozwiązania usługi mobilnej, korzystając z programu Visual Studio lub Xamarin Studio.

2. Kliknij przycisk **Uruchom**, aby skompilować projekt i uruchomić aplikację. Pojawi się monit o wybranie emulatora lub podłączonego urządzenia USB.

    > [AZURE.NOTE] Aby można było uruchomić projekt w emulatorze systemu Android, należy zdefiniować co najmniej jedno wirtualne urządzenie Android (Android Virtual Device, AVD). Do tworzenia tych urządzeń i zarządzania nimi służy Menedżer AVD.

3. W aplikacji wpisz łatwy do rozpoznania tekst (np. _Ukończenie samouczka_), a następnie kliknij ikonę plusa (**+**).

    ![][10]

    Spowoduje to wysłanie żądania POST do nowej usługi mobilnej, która jest hostowana na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez usługę mobilną, a dane są wyświetlane na liście.

    > [AZURE.NOTE] Możesz przejrzeć kod uzyskujący dostęp do usługi mobilnej w celu wykonywania zapytań i wstawiania danych. Znajduje się on w pliku C# o nazwie ToDoActivity.cs.

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
[0]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[7]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-quickstart-steps-xamarin-android.png
[8]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Wprowadzenie do synchronizacji danych w trybie offline]: mobile-services-xamarin-android-get-started-offline-data.md
[Wprowadzenie do uwierzytelniania]: mobile-services-dotnet-backend-xamarin-android-get-started-users.md
[Wprowadzenie do powiadomień wypychanych]: mobile-services-dotnet-backend-xamarin-android-get-started-push.md
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Zestaw SDK usług Mobile Services]: http://go.microsoft.com/fwlink/?LinkId=257545
[JavaScript i HTML]: mobile-services-win8-javascript/
[Klasyczny portal Azure]: https://manage.windowsazure.com/
[klasycznym portalu Azure]: https://manage.windowsazure.com/
[Wersja zaplecza JavaScript]: mobile-services-android-get-started.md
[Rozwiązywanie problemów z zapleczem usługi Mobile Services na platformie .NET]: mobile-services-dotnet-backend-how-to-troubleshoot.md


<!--HONumber=Jun16_HO2-->


