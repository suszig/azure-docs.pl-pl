<properties
    pageTitle="Wprowadzenie do usługi Mobile Services dla aplikacji JavaScript do Sklepu Windows | Azure Mobile Services"
    description="Wykonaj czynności opisane w tym samouczku, aby rozpocząć korzystanie z usług Azure Mobile Services w celu opracowywania aplikacji w języku JavaScript do Sklepu Windows."
    services="mobile-services"
    documentationCenter="windows"
    authors="ggailey777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="javascript"
    ms.topic="get-started-article"
    ms.date="07/21/2016"
    ms.author="glenga"/>


# Wprowadzenie do usług Mobile Services

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Aby zapoznać się z wersją tego tematu dotyczącą funkcji Mobile Apps, zobacz artykuł [Tworzenie aplikacji dla systemu Windows w usłudze Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started.md).  

W tym samouczku przedstawiono sposób dodawania usługi zaplecza opartej na chmurze do aplikacji JavaScript do Sklepu Windows przy użyciu usług Azure Mobile Services. Wykonując kroki tego samouczka, utworzysz nową usługę mobilną i prostą aplikację *To do list* (Lista zadań do wykonania), która przechowuje dane aplikacji w nowej usłudze mobilnej. Usługa mobilna, która zostanie utworzona, będzie korzystać z języka JavaScript do obsługi logiki biznesowej po stronie serwera. 

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* Aktywne konto platformy Azure. Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz temat [Bezpłatna wersja próbna systemu Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdocumentation%2Farticles%2Fmobile-services-javascript-backend-windows-store-javascript-get-started%2F).
* [Visual Studio 2013 Express for Windows]

## Tworzenie nowej usługi mobilnej

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Tworzenie nowej aplikacji do Sklepu Windows

Po utworzeniu usługi mobilnej można skorzystać z funkcji szybkiego startu w klasycznym portalu Azure w celu utworzenia nowej aplikacji JavaScript do Sklepu Windows 8.1, która będzie połączona z usługą mobilną.

1.  W [klasyczny portal Azure] kliknij pozycję **Mobile Services**, a następnie kliknij wcześniej utworzoną usługę mobilną.


2. Na karcie szybkiego startu kliknij opcję **Windows** w obszarze **Wybierz platformę** i rozwiń listę **Create a new Windows Store app** (Utwórz nową aplikację do Sklepu Windows).

3. Jeśli nie zostało to jeszcze zrobione, pobierz program [Visual Studio 2013][Visual Studio 2013 Express for Windows] i zainstaluj go na komputerze lokalnym lub maszynie wirtualnej.

4. Kliknij pozycję **Create TodoItem table** (Utwórz tabelę zadań do wykonania), aby utworzyć tabelę do przechowywania danych aplikacji.

5. W obszarze **Download and run your app** (Pobierz i uruchom aplikację) wybierz język aplikacji, a następnie kliknij opcję **Download** (Pobierz).

    Spowoduje to pobranie projektu przykładowej aplikacji *To do list* (Lista zadań do wykonania), która jest połączona z usługą mobilną. Zapisz skompresowany plik projektu na komputerze lokalnym i zapamiętaj, gdzie został on zapisany.

## Uruchamianie aplikacji systemu Windows

Ostatnim krokiem tego samouczka jest skompilowanie i uruchomienie nowej aplikacji.

1. Przejdź do lokalizacji, w której zapisano skompresowane pliki projektu, rozwiń pliki na komputerze, a następnie w programie Visual Studio otwórz plik rozwiązania.

2. Naciśnij klawisz **F5**, aby ponownie skompilować projekt i uruchomić aplikację.

3. W aplikacji wpisz łatwy do rozpoznania tekst (np. *Ukończenie samouczka*) w polu tekstowym **Insert a TodoItem** (Wstaw zadanie do wykonania), a następnie kliknij pozycję **Save** (Zapisz).

    Spowoduje to wysłanie żądania POST do nowej usługi mobilnej, która jest hostowana na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez usługę mobilną, a dane są wyświetlane w drugiej kolumnie aplikacji.

4. (Opcjonalnie) Ponownie uruchom aplikację i sprawdź, czy dane zapisane w poprzednim kroku są ładowane z usługi mobilnej po uruchomieniu aplikacji.
 
4. W [klasyczny portal Azure] kliknij kartę **Dane**, a następnie kliknij tabelę **TodoItems**.

    Pozwoli to przeglądać dane wstawiane przez aplikację do tabeli.

>[AZURE.NOTE] Możesz zweryfikować kod uzyskujący dostęp do usługi mobilnej w celu wykonywania zapytań i wstawiania danych, który znajduje się w pliku default.js kod.

## Następne kroki
Po ukończeniu procedury szybkiego startu dowiedz się, jak pracować z [klientem usług Mobile Services dla języków HTML i JavaScript](mobile-services-html-how-to-use-client-library.md). 

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Wprowadzenie do usługi Mobile Services]:#getting-started
[Tworzenie nowej usługi mobilnej]:#create-new-service
[Definiowanie wystąpienia usługi mobilnej]:#define-mobile-service-instance
[Następne kroki]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Visual Studio 2013 Express for Windows]: http://go.microsoft.com/fwlink/?LinkId=257546
[Zestaw SDK usług Mobile Services]: http://go.microsoft.com/fwlink/?LinkId=257545
[klasyczny portal Azure]: https://manage.windowsazure.com/



<!--HONumber=Sep16_HO3-->


