<properties
    pageTitle="Wprowadzenie do usługi Mobile Services dla platformy Xamarin.Android | Microsoft Azure"
    writer="craigd"
    description="Dowiedz się, jak używać usługi Azure Mobile Services w aplikacji platformy Xamarin.Android."
    documentationCenter="xamarin"
    authors="lindydonna"
    manager="dwrede"
    editor=""
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="07/21/2016"
    ms.author="donnam"/>

# <a name="getting-started"></a>Wprowadzenie do usług Mobile Services

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Aby zapoznać się z wersją tego tematu dotyczącą funkcji Mobile Apps, zobacz artykuł [Tworzenie aplikacji na platformie Xamarin.Android](../app-service-mobile/app-service-mobile-xamarin-android-get-started.md).

W tym samouczku przedstawiono sposób dodawania usługi zaplecza opartej na chmurze do aplikacji platformy Xamarin.Android przy użyciu usług Azure Mobile Services. Wykonując kroki tego samouczka, utworzysz nową usługę mobilną i prostą aplikację *To do list* (Lista zadań do wykonania), która przechowuje dane aplikacji w nowej usłudze mobilnej.

Jeśli wolisz obejrzeć wersję wideo, w poniższym klipie opisano te same kroki, które są zawarte w samouczku.

Wideo: „Getting Started with Xamarin and Azure Mobile Services” (Wprowadzenie do platformy Xamarin i usług Azure Mobile Services) — prowadzący Craig Dunn, deweloper i propagator systemu Xamarin (czas trwania: 10:05 min)

> [AZURE.VIDEO getting-started-with-xamarin-and-mobile-services]

Zrzut ekranu gotowej aplikacji znajduje się poniżej:

![][0]

Wykonanie kroków tego samouczka wymaga programów XCode i Xamarin Studio dla systemu OS X lub programu Visual Studio w systemie Windows z dołączonym przez sieć komputerem Mac. Kompletne instrukcje instalacji można znaleźć w temacie [Konfigurowanie i instalowanie oprogramowania Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 

Pobrany projekt szybkiego startu zawiera składnik usług Azure Mobile Services dla platformy Xamarin.Android. Ten projekt jest przeznaczony dla systemu Android w wersji 4.2 lub nowszej, natomiast zestaw SDK usług Mobile Services wymaga systemu Android w wersji 2.2 lub nowszej.

> [AZURE.IMPORTANT] Do ukończenia tego samouczka jest potrzebne konto platformy Azure. Jeśli nie masz takiego konta, możesz je utworzyć w wersji próbnej platformy Azure i uzyskać maksymalnie 10 bezpłatnych usług mobilnych, z których można korzystać nawet po wygaśnięciu wersji próbnej. Aby uzyskać szczegółowe informacje, zobacz temat [Bezpłatna wersja próbna systemu Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5).

## <a name="create-new-service"> </a>Tworzenie nowej usługi mobilnej

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Tworzenie nowej aplikacji platformy Xamarin.Android

Po utworzeniu usługi mobilnej można użyć funkcji szybkiego startu w klasycznym portalu Azure, aby utworzyć nową lub zmodyfikować istniejącą aplikację w celu nawiązania połączenia z usługą mobilną.

W tej sekcji utworzysz nową aplikację platformy Xamarin.Android połączoną z usługą mobilną.

1.  W [klasyczny portal Azure] kliknij pozycję **Mobile Services**, a następnie kliknij wcześniej utworzoną usługę mobilną.

2. Na karcie szybkiego startu kliknij pozycję **Xamarin.Android** w obszarze **Wybierz platformę** i rozwiń listę **Create a new Android app** (Utwórz nową aplikację systemu Android).

    ![][6]

    Zostaną wyświetlone trzy proste kroki umożliwiające utworzenie aplikacji platformy Xamarin.Android i połączenie jej z usługą mobilną.

    ![][7]

3. Kliknij pozycję **Create TodoItem table** (Utwórz tabelę zadań do wykonania), aby utworzyć tabelę do przechowywania danych aplikacji.

4. W obszarze **Download and run your app** (Pobierz i uruchom aplikację) kliknij pozycję **Download** (Pobierz).

    Spowoduje to pobranie projektu przykładowej aplikacji _To do list_ (Lista zadań do wykonania), która jest połączona z usługą mobilną. Zapisz skompresowany plik projektu na komputerze lokalnym i zapamiętaj, gdzie został on zapisany.

## Uruchamianie aplikacji systemu Android

Ostatnim krokiem tego samouczka jest skompilowanie i uruchomienie nowej aplikacji.

1. Przejdź do lokalizacji, w której zapisano skompresowane pliki projektu, i wyodrębnij je na komputerze.

2. W programie Xamarin Studio lub Visual Studio kliknij kolejno pozycje **Plik** i **Otwórz**, przejdź do nieskompresowanych przykładowych plików i otwórz plik **XamarinTodoQuickStart.Android.sln**.

3. Kliknij przycisk **Uruchom**, aby skompilować projekt i uruchomić aplikację. Pojawi się monit o wybranie emulatora lub podłączonego urządzenia USB.

    > [AZURE.NOTE] Aby można było uruchomić projekt w emulatorze systemu Android, musisz zdefiniować co najmniej jedno urządzenie wirtualne systemu Android (AVD, Android Virtual Device). Do tworzenia tych urządzeń i zarządzania nimi służy Menedżer AVD.

4. W aplikacji wpisz łatwy do rozpoznania tekst, na przykład _Ukończ samouczek_, a następnie kliknij pozycję **Dodaj**.

    ![][10]

    Spowoduje to wysłanie żądania POST do nowej usługi mobilnej, która jest hostowana na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez usługę mobilną, a dane są wyświetlane na liście.

    > [AZURE.NOTE]
    > Możesz przejrzeć kod uzyskujący dostęp do usługi mobilnej w celu wykonywania zapytań i wstawiania danych. Znajduje się on w pliku C# o nazwie ToDoActivity.cs.

6. W [klasyczny portal Azure] kliknij kartę **Dane**, a następnie kliknij tabelę **TodoItems**.

    ![][11]

    Pozwoli to przeglądać dane wstawiane przez aplikację do tabeli.

    ![][12]

## <a name="next-steps"> </a>Następne kroki
Po zakończeniu procedury szybkiego startu dowiedz się, jak wykonać dodatkowe ważne zadania w usłudze Mobile Services:

* [Wprowadzenie do synchronizacji danych w trybie offline] Dowiedz się, jak w ramach szybkiego startu używana jest synchronizacja danych w trybie offline, aby zapewnić dynamiczne i niezawodne działanie aplikacji.

* [Wprowadzenie do uwierzytelniania] Dowiedz się, jak uwierzytelniać użytkowników aplikacji przy użyciu dostawcy tożsamości.

* [Wprowadzenie do powiadomień wypychanych] Dowiedz się, jak wysyłać do aplikacji bardzo proste powiadomienia wypychane.



[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Wprowadzenie do usługi Mobile Services]:#getting-started
[Tworzenie nowej usługi mobilnej]:#create-new-service
[Definiowanie wystąpienia usługi mobilnej]:#define-mobile-service-instance
[Następne kroki]:#next-steps

<!-- Images. -->
[0]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-quickstart-completed-android.png
[2]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-create.png
[3]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-create-page1.png
[4]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-create-page2.png
[5]: ./media/partner-xamarin-mobile-services-android-get-started/obile-services-selection.png
[6]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-portal-quickstart-xamarin-android.png
[7]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-quickstart-steps-xamarin-android.png
[8]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-xamarin-project-android-xs.png
[9]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-xamarin-project-android-vs.png
[10]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-quickstart-startup-android.png
[11]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-data-tab.png
[12]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-data-browse.png
[13]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-services-diagram.png


<!-- URLs. -->
[Wprowadzenie do danych]: /develop/mobile/tutorials/get-started-with-data-xamarin-android
[Wprowadzenie do synchronizacji danych w trybie offline]: mobile-services-xamarin-android-get-started-offline-data.md
[Wprowadzenie do uwierzytelniania]: /develop/mobile/tutorials/get-started-with-users-xamarin-android
[Wprowadzenie do powiadomień wypychanych]: /develop/mobile/tutorials/get-started-with-push-xamarin-android
[Zestaw SDK usług Mobile Services dla systemu Android]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Azure]: http://azure.microsoft.com/
[klasyczny portal Azure]: https://manage.windowsazure.com/




<!--HONumber=sep16_HO1-->


