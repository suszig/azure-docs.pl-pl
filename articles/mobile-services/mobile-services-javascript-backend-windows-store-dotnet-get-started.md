<properties
    pageTitle="Wprowadzenie do usługi Mobile Services dla aplikacji Sklepu Windows (C#) | Microsoft Azure"
    description="Czynności opisane w tym samouczku ułatwią rozpoczęcie tworzenia aplikacji do Sklepu Windows w języku C# przy użyciu usług Azure Mobile Services ."
    services="mobile-services"
    documentationCenter="windows"
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="get-started-article" 
    ms.date="07/21/2016"
    ms.author="glenga"/>

# <a name="getting-started"> </a>Wprowadzenie do usług Mobile Services

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Aby zapoznać się z wersją tego tematu dotyczącą funkcji Mobile Apps, zobacz artykuł [Tworzenie aplikacji dla systemu Windows](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started.md).

W tym samouczku przedstawiono sposób dodawania usługi zaplecza opartej na chmurze do uniwersalnej aplikacji systemu Windows przy użyciu usług Azure Mobile Services. Rozwiązania uniwersalnych aplikacji systemu Windows obejmują projekty aplikacji do Sklepu Windows 8.1 i Sklepu Windows Phone 8.1 oraz wspólny projekt współdzielony. Aby uzyskać więcej informacji, zobacz temat [Build universal Windows apps that target Windows and Windows Phone](http://msdn.microsoft.com/library/windows/apps/xaml/dn609832.aspx) (Tworzenie uniwersalnych aplikacji systemu Windows przeznaczonych dla systemów Windows i Windows Phone).

Wykonując kroki tego samouczka, utworzysz nową usługę mobilną i prostą aplikację *To do list* (Lista zadań do wykonania), która przechowuje dane aplikacji w nowej usłudze mobilnej. Usługa mobilna, która zostanie utworzona, będzie korzystać z języka JavaScript do obsługi logiki biznesowej po stronie serwera. Aby utworzyć usługę mobilną, która umożliwia zapisanie logiki biznesowej po stronie serwera w obsługiwanych językach .NET przy użyciu programu Visual Studio, zobacz wersję tego tematu dla zaplecza .NET.

[AZURE.INCLUDE [mobile-services-windows-universal-get-started](../../includes/mobile-services-windows-universal-get-started.md)]

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* Aktywne konto platformy Azure. Jeśli nie masz takiego konta, możesz je utworzyć w wersji próbnej platformy Azure i uzyskać maksymalnie 10 bezpłatnych usług mobilnych, z których można korzystać nawet po wygaśnięciu wersji próbnej. Aby uzyskać szczegółowe informacje, zobacz temat [Bezpłatna wersja próbna systemu Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-javascript-backend-windows-store-javascript-get-started%2F).
* [Visual Studio 2013 Express for Windows]

## Tworzenie nowej usługi mobilnej

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Tworzenie nowej uniwersalnej aplikacji systemu Windows

Po utworzeniu usługi mobilnej można użyć funkcji szybkiego startu w klasycznym portalu Azure, aby utworzyć nową uniwersalną aplikację systemu Windows (lub zmodyfikować istniejącą aplikację do Sklepu Windows lub Windows Phone) oraz połączyć ją z usługą mobilną.

W tej części utworzysz nową uniwersalną aplikację systemu Windows połączoną z usługą mobilną.

1.  W [klasyczny portal Azure] kliknij pozycję **Mobile Services**, a następnie kliknij wcześniej utworzoną usługę mobilną.


2. Na karcie szybkiego startu kliknij opcję **Windows** w obszarze **Wybierz platformę** i rozwiń listę **Create a new Windows Store app** (Utwórz nową aplikację do Sklepu Windows).

    Zostaną wyświetlone trzy proste kroki umożliwiające utworzenie aplikacji do Sklepu Windows i połączenie jej z usługą mobilną.

    ![Kroki procedury szybkiego startu dla usług Mobile Services](./media/mobile-services-javascript-backend-windows-store-dotnet-get-started/mobile-quickstart-steps.png)

3. Jeśli nie zostało to jeszcze zrobione, pobierz program [Visual Studio 2013 Express for Windows] i zainstaluj go na komputerze lokalnym lub maszynie wirtualnej.

4. Kliknij pozycję **Create TodoItem table** (Utwórz tabelę zadań do wykonania), aby utworzyć tabelę do przechowywania danych aplikacji.

5. W obszarze **Download and run your app** (Pobierz i uruchom aplikację) wybierz język aplikacji, a następnie kliknij opcję **Download** (Pobierz).

    Spowoduje to pobranie projektu przykładowej aplikacji *To do list* (Lista zadań do wykonania), która jest połączona z usługą mobilną. Zapisz skompresowany plik projektu na komputerze lokalnym i zapamiętaj, gdzie został on zapisany.

## Uruchamianie aplikacji systemu Windows

[AZURE.INCLUDE [mobile-services-javascript-backend-run-app](../../includes/mobile-services-javascript-backend-run-app.md)]

>[AZURE.NOTE]Możesz przejrzeć kod uzyskujący dostęp do usługi mobilnej w celu wykonywania zapytań i wstawiania danych, który znajduje się w pliku MainPage.xaml.cs.

## Następne kroki
Po zakończeniu procedury szybkiego startu dowiedz się, jak wykonać dodatkowe ważne zadania w usłudze Mobile Services:

* [Wprowadzenie do synchronizacji danych w trybie offline] Dowiedz się, jak używać synchronizacji danych w trybie offline, aby zapewnić dynamiczne i niezawodne działanie aplikacji.

* [Dodawanie uwierzytelniania do aplikacji usług Mobile Services ][Wprowadzenie do uwierzytelniania]  
  Dowiedz się, jak uwierzytelniać użytkowników aplikacji przy użyciu dostawcy tożsamości.

* [Dodawanie powiadomień wypychanych do aplikacji ][Wprowadzenie do powiadomień wypychanych]  
  Dowiedz się, jak wysyłać do aplikacji bardzo proste powiadomienia wypychane.

* [Jak używać biblioteki klienta .NET](mobile-services-dotnet-how-to-use-client-library.md)  
 Dowiedz się, jak wykonywać zapytania w usłudze mobilnej, pracować z danymi i uzyskiwać dostęp do niestandardowych interfejsów API.

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Wprowadzenie do usługi Mobile Services]:#getting-started
[Tworzenie nowej usługi mobilnej]:#create-new-service
[Definiowanie wystąpienia usługi mobilnej]:#define-mobile-service-instance
[Następne kroki]:#next-steps

<!-- Images. -->



<!-- URLs. -->
[Wprowadzenie do synchronizacji danych w trybie offline]: mobile-services-windows-store-dotnet-get-started-offline-data.md
[Wprowadzenie do uwierzytelniania]: mobile-services-javascript-backend-windows-universal-dotnet-get-started-users.md
[Wprowadzenie do powiadomień wypychanych]: mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md
[Visual Studio 2013 Express for Windows]: http://go.microsoft.com/fwlink/?LinkId=257546
[Zestaw SDK usług Mobile Services]: http://go.microsoft.com/fwlink/?LinkId=257545
[klasyczny portal Azure]: https://manage.windowsazure.com/
 


<!--HONumber=sep16_HO1-->


