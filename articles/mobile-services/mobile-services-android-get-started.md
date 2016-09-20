<properties
    pageTitle="Wprowadzenie do usługi Azure Mobile Services dla aplikacji systemu Android (zaplecze JavaScript)"
    description="Wykonaj czynności opisane w tym samouczku, aby rozpocząć korzystanie z usługi Azure Mobile Services w celu opracowywania aplikacji systemu Android (zaplecze JavaScript)."
    services="mobile-services"
    documentationCenter="android"
    authors="RickSaling"
    manager="reikre"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="07/21/2016"
    ms.author="ricksal"/>

# Wprowadzenie do usługi Azure Mobile Services dla systemu Android (zaplecze JavaScript)

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Aby zapoznać się z wersją tego tematu dotyczącą usługi Mobile Apps, zobacz temat [Tworzenie aplikacji systemu Android w usłudze Azure Mobile Apps](../app-service-mobile/app-service-mobile-android-get-started.md).

W tym samouczku przedstawiono sposób dodawania usługi zaplecza opartej na chmurze do aplikacji systemu Android przy użyciu usługi Azure Mobile Services. Wykonując kroki tego samouczka, utworzysz nową usługę mobilną i prostą aplikację **Lista zadań do wykonania**, która przechowuje dane aplikacji w nowej usłudze mobilnej.

> [AZURE.VIDEO mobile-get-started-android]

Zrzut ekranu gotowej aplikacji znajduje się poniżej:

![](./media/mobile-services-android-get-started/mobile-quickstart-completed-android.png)

## Wymagania wstępne

Wykonanie kroków tego samouczka wymaga [narzędzi dla deweloperów systemu Android](https://developer.android.com/sdk/index.html), które obejmują zintegrowane środowisko projektowe Android Studio, oraz najnowszej platformy Android. Wymagany jest system Android 4.2 lub nowszy.

Pobrany projekt szybkiego startu zawiera zestaw SDK usługi Azure Mobile Services dla systemu Android.

> [AZURE.IMPORTANT] Do wykonania kroków tego samouczka potrzebne jest konto platformy Azure. Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28).


## Tworzenie nowej usługi mobilnej

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Tworzenie nowej aplikacji systemu Android

Po utworzeniu usługi mobilnej można użyć funkcji szybkiego startu w klasycznym portalu Azure, aby utworzyć nową lub zmodyfikować istniejącą aplikację w celu nawiązania połączenia z usługą mobilną.

W tej sekcji utworzysz nową aplikację systemu Android połączoną z usługą mobilną.

1.  W klasycznym portalu Azure kliknij opcję **Mobile Services**, a następnie kliknij wcześniej utworzoną usługę mobilną.

2. Na karcie szybkiego startu kliknij opcję **Android** w obszarze **Wybierz platformę** i rozwiń listę **Create a new Android app** (Utwórz nową aplikację systemu Android).

    ![](./media/mobile-services-android-get-started/mobile-portal-quickstart-android1.png)

    Zostaną wyświetlane trzy łatwe kroki umożliwiające utworzenie aplikacji systemu Android połączonej z usługą mobilną.

    ![](./media/mobile-services-android-get-started/mobile-quickstart-steps-android-AS.png)

3. Jeśli nie zostało to jeszcze zrobione, pobierz [narzędzia dla deweloperów systemu Android](https://go.microsoft.com/fwLink/p/?LinkID=280125) i zainstaluj je na komputerze lokalnym lub maszynie wirtualnej.

4. Kliknij przycisk **Create TodoItem table** (Utwórz tabelę zadań do wykonania), aby utworzyć tabelę do przechowywania danych aplikacji.


5. Następnie pobierz aplikację, naciskając przycisk **Download** (Pobierz).

## Uruchamianie aplikacji systemu Android

[AZURE.INCLUDE [mobile-services-run-your-app](../../includes/mobile-services-android-get-started.md)]


## <a name="next-steps"> </a>Następne kroki
Po zakończeniu procedury szybkiego startu dowiedz się, jak wykonać dodatkowe ważne zadania w usłudze Mobile Services:

* [Wprowadzenie do danych]
  <br/>Dowiedz się więcej o przechowywaniu danych i wykonywaniu zapytań w usłudze Mobile Services.

* [Wprowadzenie do uwierzytelniania]
  <br/>Dowiedz się, jak uwierzytelniać użytkowników aplikacji przy użyciu dostawcy tożsamości.

* [Wprowadzenie do powiadomień wypychanych]
  <br/>Dowiedz się, jak wysyłać do aplikacji bardzo proste powiadomienia wypychane.


[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]


<!-- URLs. -->
[Wprowadzenie (Eclipse)]: mobile-services-android-get-started-ec.md
[Wprowadzenie do danych]: mobile-services-android-get-started-data.md
[Wprowadzenie do uwierzytelniania]: mobile-services-android-get-started-users.md
[Wprowadzenie do powiadomień wypychanych]: mobile-services-javascript-backend-android-get-started-push.md
[Zestaw SDK usługi Mobile Services dla systemu Android]: https://go.microsoft.com/fwLink/p/?LinkID=266533




<!--HONumber=sep16_HO1-->


