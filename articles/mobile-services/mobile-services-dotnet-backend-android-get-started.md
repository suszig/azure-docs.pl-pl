
<properties
    pageTitle="Wprowadzenie do usługi Azure Mobile Services dla aplikacji systemu Android"
    description="Wykonaj czynności opisane w tym samouczku, aby rozpocząć korzystanie z usługi Azure Mobile Services w celu opracowywania aplikacji systemu Android."
    services="mobile-services"
    documentationCenter="android"
    authors="RickSaling"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="03/05/2016"
    ms.author="ricksal"/>


# <a name="getting-started"> </a>Wprowadzenie do usługi Mobile Services

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Aby zapoznać się z wersją tego tematu dotyczącą usługi Mobile Apps, zobacz temat [Tworzenie aplikacji systemu Android w usłudze Azure Mobile Apps](../app-service-mobile/app-service-mobile-android-get-started.md).

W tym samouczku przedstawiono sposób dodawania usługi zaplecza opartej na chmurze do aplikacji systemu Android przy użyciu usługi Azure Mobile Services. Wykonując kroki tego samouczka, utworzysz nową usługę mobilną i prostą aplikację _Lista zadań do wykonania_, która przechowuje dane aplikacji w nowej usłudze mobilnej. Usługa mobilna, która zostanie utworzona, korzysta z obsługiwanych języków .NET przy użyciu programu Visual Studio do obsługi logiki biznesowej po stronie serwera oraz do zarządzania usługami mobilnymi. Aby utworzyć usługę mobilną, która umożliwia zapisanie logiki biznesowej po stronie serwera w języku JavaScript, zobacz część [Wersja zaplecza JavaScript](mobile-services-android-get-started.md) w tym temacie.

Zrzut ekranu gotowej aplikacji znajduje się poniżej:

![](./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-completed-android.png)

Wykonanie kroków tego samouczka wymaga [narzędzi dla deweloperów systemu Android][Android Studio], które obejmują zintegrowane środowisko projektowe Android Studio, oraz najnowszej platformy Android. Wymagany jest system Android 4.2 lub nowszy.

Pobrany projekt szybkiego startu zawiera zestaw SDK usługi Mobile Services dla systemu Android.

> [AZURE.IMPORTANT] Do wykonania kroków tego samouczka potrzebne jest konto platformy Azure. Jeśli nie masz konta, możesz utworzyć konto w wersji próbnej platformy Azure i uzyskać maksymalnie 10 bezpłatnych usług mobilnych, z których możesz korzystać nawet po upływie ważności wersji próbnej. Aby uzyskać szczegółowe informacje, zobacz temat [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28) (Bezpłatna wersja próbna platformy Azure).


## <a name="create-new-service"> </a>Tworzenie nowej usługi mobilnej

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## Pobieranie usługi mobilnej na komputer lokalny

Po utworzeniu usługi mobilnej pobierz spersonalizowany projekt usługi mobilnej, który możesz uruchomić na komputerze lokalnym lub maszynie wirtualnej.

1. Kliknij utworzoną usługę mobilną i na karcie szybkiego startu kliknij opcję **Android** w obszarze **Wybierz platformę** i rozwiń listę **Create a new Android app** (Utwórz nową aplikację systemu Android).

    ![][1]

2. Jeśli nie zostało to jeszcze zrobione, pobierz i zainstaluj program [Visual Studio Professional 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) lub nowszą wersję.

3. W kroku 2 kliknij opcję **Pobierz** w obszarze **Download and publish your service to the cloud** (Pobierz i opublikuj usługę w chmurze).

    Spowoduje to pobranie projektu programu Visual Studio, który implementuje usługę mobilną. Zapisz skompresowany plik projektu na komputerze lokalnym i zanotuj miejsce, w którym został zapisany.

## Test usługi mobilnej

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

## Publikowanie usługi mobilnej

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

## Tworzenie nowej aplikacji systemu Android

W tej sekcji utworzysz nową aplikację systemu Android połączoną z usługą mobilną.

1. W [klasycznym portalu Azure] kliknij opcję **Mobile Services**, a następnie kliknij wcześniej utworzoną usługę mobilną.

2. Na karcie szybkiego startu kliknij opcję **Android** w obszarze **Wybierz platformę** i rozwiń listę **Create a new Android app** (Utwórz nową aplikację systemu Android).

    ![][2]

3. Jeśli nie zostało to jeszcze zrobione, pobierz [narzędzia dla deweloperów systemu Android][Zestaw SDK systemu Android] i zainstaluj je na komputerze lokalnym lub maszynie wirtualnej.

4. W obszarze **Download and run your app** (Pobierz i uruchom aplikację) kliknij opcję **Pobierz**.

    Spowoduje to pobranie projektu przykładowej aplikacji _Lista zadań do wykonania_, która jest połączona z usługą mobilną. Zapisz skompresowany plik projektu na komputerze lokalnym i zanotuj miejsce, w którym został zapisany.

## Uruchamianie aplikacji systemu Android

[AZURE.INCLUDE [mobile-services-run-your-app](../../includes/mobile-services-android-get-started.md)]

## <a name="next-steps"> </a>Następne kroki
Po zakończeniu procedury szybkiego startu dowiedz się, jak wykonać dodatkowe ważne zadania w usłudze Mobile Services:

* [Dodawanie powiadomień wypychanych do aplikacji]
  <br/>Dowiedz się, jak wysyłać do aplikacji bardzo proste powiadomienia wypychane.

* [Dodawanie uwierzytelniania do aplikacji]
  <br/>Dowiedz się, jak ograniczyć dostęp do danych zaplecza do określonych zarejestrowanych użytkowników aplikacji.

* [Rozwiązywanie problemów z zapleczem usługi Mobile Services na platformie .NET]
  <br/> Dowiedz się, jak diagnozować i naprawiać ewentualne problemy dotyczące zaplecza usługi Mobile Services na platformie .NET.

<!-- Anchors. -->
[Wprowadzenie do usługi Mobile Services]:#getting-started
[Tworzenie nowej usługi mobilnej]:#create-new-service
[Definiowanie wystąpienia usługi mobilnej]:#define-mobile-service-instance
[Następne kroki]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-completed-android.png
[1]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-steps-vs-AS.png
[2]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-steps-android-AS.png


[6]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-portal-quickstart-android.png
[7]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-steps-android.png
[8]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-eclipse-quickstart.png

[10]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-startup-android.png
[11]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-data-browse.png

[14]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-services-import-android-workspace.png
[15]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-services-import-android-project.png

<!-- URLs. -->
[Wprowadzenie (Eclipse)]: mobile-services-dotnet-backend-android-get-started-ec.md
[Dodawanie powiadomień wypychanych do aplikacji]: mobile-services-dotnet-backend-android-get-started-push.md
[Dodawanie uwierzytelniania do aplikacji]: mobile-services-dotnet-backend-android-get-started-auth.md
[Zestaw SDK systemu Android]: https://go.microsoft.com/fwLink/p/?LinkID=280125
[Android Studio]: https://developer.android.com/sdk/index.html
[Zestaw SDK usługi Mobile Services dla systemu Android]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Rozwiązywanie problemów z zapleczem usługi Mobile Services na platformie .NET]: mobile-services-dotnet-backend-how-to-troubleshoot.md

[klasycznym portalu Azure]: https://manage.windowsazure.com/



<!--HONumber=Jun16_HO2-->


