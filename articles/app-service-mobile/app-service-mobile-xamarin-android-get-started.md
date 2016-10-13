<properties
    pageTitle="Wprowadzenie do usługi Azure Mobile Apps dla aplikacji platformy Xamarin.Android"
    description="Wykonaj czynności opisane w tym samouczku, aby rozpocząć używanie usługi Azure Mobile Apps do tworzenia aplikacji platformy Xamarin Android."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha" />


#Tworzenie aplikacji platformy Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##Omówienie

W tym samouczku przedstawiono sposób dodawania usługi zaplecza opartej na chmurze do aplikacji platformy Xamarin.Android. Aby uzyskać więcej informacji, zobacz artykuł [Co to jest usługa Mobile Apps](app-service-mobile-value-prop.md).

Zrzut ekranu gotowej aplikacji znajduje się poniżej:

![][0]

Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usługi Mobile Apps dotyczących aplikacji platformy Xamarin.Android.

##Wymagania wstępne

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* Aktywne konto platformy Azure. Jeśli nie masz konta, możesz utworzyć konto wersji próbnej platformy Azure i uzyskać maksymalnie 10 bezpłatnych usług Mobile Apps, z których możesz korzystać nawet po zakończeniu okresu ważności wersji próbnej. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio z programem Xamarin. Instrukcje można znaleźć w temacie [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (Konfigurowanie i instalowanie dla programów Visual Studio i Xamarin).  
 
>[AZURE.NOTE] Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Try App Service](https://tryappservice.azure.com/?appServiceName=mobile) (Wypróbuj usługę App Service), w którym wyjaśniono, jak od razu utworzyć początkową aplikację mobilną o krótkim okresie istnienia w usłudze App Service. Bez kart kredytowych i bez zobowiązań.

## Tworzenie zaplecza nowej Aplikacji mobilnej Azure

Wykonaj te kroki, aby utworzyć zaplecze nowej Aplikacji mobilnej.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

W ten sposób zainicjowano obsługę zaplecza Aplikacji mobilnej Azure, które może być używane przez aplikacje mobilne klientów. Teraz pobierzesz projekt serwera dla prostego zaplecza typu „lista czynności do wykonania” i opublikujesz go na platformie Azure.

## Konfigurowanie projektu serwera

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## Pobieranie i uruchamianie aplikacji platformy Xamarin.Android

1. W obszarze **Pobierz i uruchom projekt platformy Xamarin.Android** kliknij przycisk **Pobierz**.

    Spowoduje to pobranie projektu zawierającego aplikację klienta połączoną z aplikacją mobilną. Zapisz skompresowany plik projektu na komputerze lokalnym i zapamiętaj, gdzie został on zapisany.

2. Naciśnij klawisz **F5**, aby skompilować projekt i uruchomić aplikację.

3. W aplikacji wpisz znaczący tekst, na przykład _Ukończ samouczek_, a następnie kliknij przycisk **Dodaj**.

    ![][10]

    Spowoduje to wysłanie żądania POST do zaplecza nowej aplikacji mobilnej, która jest obsługiwana na platformie Azure. Dane z żądania zostaną wstawione do tabeli czynności do wykonania. Elementy przechowywane w tabeli są zwracane przez zaplecze aplikacji mobilnej, a dane są wyświetlane na liście.

    > [AZURE.NOTE] Możesz przejrzeć kod uzyskujący dostęp do zaplecza aplikacji mobilnej w celu wyszukiwania i wstawiania danych. Znajduje się on w pliku C# ToDoActivity.cs.

##Następne kroki

* [Dodawanie uwierzytelniania do aplikacji ](app-service-mobile-xamarin-android-get-started-users.md)  
Dowiedz się, jak uwierzytelniać użytkowników aplikacji przy użyciu dostawcy tożsamości.
* [Dodawanie powiadomień wypychanych do aplikacji platformy Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md)  
Dowiedz się, jak dodawać powiadomienia wypychane do aplikacji.
* [Jak używać zarządzanego klienta usługi Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)  
Dowiedz się, jak pracować z zestawem SDK zarządzanego klienta w aplikacji platformy Xamarin. 


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203



<!--HONumber=Oct16_HO1-->


