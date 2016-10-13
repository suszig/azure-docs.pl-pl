<properties
    pageTitle="Wprowadzenie do funkcji Azure App Service Mobile Apps dla aplikacji Xamarin.iOS | Microsoft Azure"
    description="Wykonaj czynności opisane w tym samouczku, aby rozpocząć używanie usługi Azure Mobile Apps do tworzenia aplikacji platformy Xamarin.iOS."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>



#Tworzenie aplikacji platformy Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##Omówienie

W tym samouczku przedstawiono sposób dodawania usługi zaplecza opartej na chmurze do aplikacji mobilnej platformy Xamarin.iOS przy użyciu zaplecza Aplikacji mobilnej Azure.  Będziesz tworzyć zaplecze nowej aplikacji mobilnej oraz prostą aplikację platformy Xamarin.iOS typu _Lista czynności do wykonania_, która przechowuje dane aplikacji na platformie Azure.

Wykonanie kroków tego samouczka jest wymagane w przypadku wszystkich innych samouczków dla platformy Xamarin.iOS dotyczących używania funkcji Mobile Apps w usłudze Azure App Service.

##Wymagania wstępne

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* Aktywne konto platformy Azure. Jeśli nie masz konta, możesz utworzyć konto wersji próbnej platformy Azure i uzyskać maksymalnie 10 bezpłatnych aplikacji mobilnych, z których możesz korzystać nawet po zakończeniu okresu ważności wersji próbnej. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio z programem Xamarin. Instrukcje można znaleźć w temacie [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (Konfigurowanie i instalowanie dla programów Visual Studio i Xamarin).

* Komputer Mac z zainstalowanym środowiskiem Xcode v7.0 lub nowszym i rozwiązaniem Xamarin Studio Community. Zobacz tematy [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (Konfigurowanie i instalowanie dla programów Visual Studio i Xamarin) oraz [Setup, install, and verifications for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (Konfigurowanie, instalowanie oraz weryfikacje dla użytkowników komputerów Mac) (MSDN).

>[AZURE.NOTE] Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do strony [Wypróbuj usługę App Service](https://tryappservice.azure.com/?appServiceName=mobile). W tej lokalizacji możesz od razu utworzyć początkową aplikację mobilną o krótkim okresie istnienia w usłudze App Service — bez karty kredytowej i bez zobowiązań.

## Tworzenie zaplecza nowej Aplikacji mobilnej Azure

Wykonaj te kroki, aby utworzyć zaplecze nowej Aplikacji mobilnej.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## Konfigurowanie projektu serwera

W ten sposób zainicjowano obsługę zaplecza Aplikacji mobilnej Azure, które może być używane przez aplikacje mobilne klientów. Teraz pobierzesz projekt serwera dla prostego zaplecza typu „lista czynności do wykonania” i opublikujesz go na platformie Azure.

Wykonaj poniższe kroki, aby skonfigurować projekt serwera do użycia zaplecza Node.js lub .NET.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## Pobieranie i uruchamianie aplikacji platformy Xamarin.iOS

1. Otwórz [Azure portal] w oknie przeglądarki.

2. W bloku ustawień Aplikacji mobilnej kliknij pozycję **Wprowadzenie** > **Xamarin.iOS**. W kroku 3 kliknij pozycję **Utwórz nową aplikację**, jeśli nie została jeszcze wybrana.  Następnie kliknij przycisk **Pobierz**.

    Spowoduje to pobranie projektu zawierającego aplikację klienta połączoną z aplikacją mobilną. Zapisz skompresowany plik projektu na komputerze lokalnym i zapamiętaj, gdzie został on zapisany.

3. Wyodrębnij pobrany projekt, a następnie otwórz go w programie Xamarin Studio (lub Visual Studio).

    ![][9]

    ![][8]

4. Naciśnij klawisz F5, aby skompilować projekt i uruchomić aplikację w emulatorze urządzenia iPhone.

5. W aplikacji wpisz znaczący tekst, na przykład _Poznaj program Xamarin_, a następnie kliknij przycisk **+**.

    ![][10]

    Spowoduje to wysłanie żądania POST do zaplecza nowej aplikacji mobilnej, która jest obsługiwana na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez zaplecze aplikacji mobilnej, a dane są wyświetlane na liście.

>[AZURE.NOTE]Możesz przejrzeć kod uzyskujący dostęp do zaplecza aplikacji mobilnej w celu wyszukiwania i wstawiania danych. Znajduje się on w pliku C# QSTodoService.cs.

##Następne kroki

* [Dodawanie uwierzytelniania do aplikacji ](app-service-mobile-xamarin-ios-get-started-users.md)
  <br/>Dowiedz się, jak uwierzytelniać użytkowników aplikacji przy użyciu dostawcy tożsamości.

* [Dodawanie powiadomień wypychanych do aplikacji](app-service-mobile-xamarin-ios-get-started-push.md)
  <br/>Dowiedz się, jak wysyłać do aplikacji bardzo proste powiadomienia wypychane.

<!-- Anchors. -->
[Wprowadzenie do zapleczy aplikacji mobilnych]:#getting-started
[Tworzenie zaplecza nowej aplikacji mobilnej]:#create-new-service
[Następne kroki]:#next-steps



<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure portal]: https://portal.azure.com/



<!--HONumber=Oct16_HO1-->


