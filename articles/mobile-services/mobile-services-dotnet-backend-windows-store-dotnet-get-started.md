---
title: Wprowadzenie do usługi Mobile Services dla uniwersalnych aplikacji systemu Windows | Microsoft Docs
description: Wykonaj czynności opisane w tym samouczku, aby rozpocząć korzystanie z usługi Azure Mobile Services w celu opracowywania uniwersalnych aplikacji systemu Windows w języku C#.
services: mobile-services
documentationcenter: windows
author: ggailey777
manager: erikre
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/21/2016
ms.author: glenga

---
# <a name="getting-started"> </a>Wprowadzenie do usług Mobile Services
[!INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Aby zapoznać się z wersją tego tematu dotyczącą usługi Mobile Apps, zobacz [Tworzenie aplikacji systemu Windows w usłudze Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started.md).
> 
> 

W tym samouczku przedstawiono sposób dodawania usługi zaplecza opartej na chmurze do uniwersalnych aplikacji systemu Windows przy użyciu usługi Azure Mobile Services. Rozwiązania uniwersalnych aplikacji systemu Windows obejmują projekty aplikacji do Sklepu Windows 8.1 i Sklepu Windows Phone 8.1 oraz wspólny projekt współdzielony. Aby uzyskać więcej informacji, zobacz temat [Build universal Windows apps that target Windows and Windows Phone](http://msdn.microsoft.com/library/windows/apps/xaml/dn609832.aspx) (Tworzenie uniwersalnych aplikacji systemu Windows przeznaczonych dla systemów Windows i Windows Phone).

Wykonując kroki tego samouczka, utworzysz nową usługę mobilną i prostą aplikację *Lista zadań do wykonania*, która przechowuje dane aplikacji w nowej usłudze mobilnej. Usługa mobilna, która zostanie utworzona, korzysta z obsługiwanych języków .NET przy użyciu programu Visual Studio do obsługi logiki biznesowej po stronie serwera oraz do zarządzania usługami mobilnymi. Aby utworzyć usługę mobilną, która umożliwia zapisanie logiki biznesowej po stronie serwera w języku JavaScript, zobacz część Wersja zaplecza JavaScript w tym temacie.

> [!NOTE]
> W tym temacie przedstawiono sposób tworzenia nowego projektu usługi mobilnej i uniwersalnej aplikacji systemu Windows przy użyciu klasycznego portalu Azure. Korzystając z programu Visual Studio 2013 Update 3, można również dodać nowy projekt usługi mobilnej do istniejącego rozwiązania Visual Studio. Aby uzyskać więcej informacji, zobacz temat [Dodawanie usługi Mobile Services do istniejącej aplikacji](mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md).
> 
> Aby dodać usługę mobilną do projektu aplikacji systemu Windows Phone 8.0 lub Windows Phone Silverlight 8.1, zobacz temat [Dodawanie usługi Mobile Services do istniejącej aplikacji systemu Windows Phone](mobile-services-windows-phone-get-started-data.md).
> 
> 

[!INCLUDE [mobile-services-windows-universal-get-started](../../includes/mobile-services-windows-universal-get-started.md)]

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* Aktywne konto platformy Azure. Jeśli nie masz takiego konta, możesz je utworzyć w wersji próbnej platformy Azure i uzyskać maksymalnie 10 bezpłatnych usług mobilnych, z których można korzystać nawet po wygaśnięciu wersji próbnej. Aby uzyskać szczegółowe informacje, zobacz temat [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-windows-store-dotnet-get-started%2F) (Bezpłatna wersja próbna platformy Azure).
* [Visual Studio 2013].

## Tworzenie nowej usługi mobilnej
[!INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## Tworzenie nowej uniwersalnej aplikacji systemu Windows
Po utworzeniu usługi mobilnej można użyć funkcji szybkiego startu w klasycznym portalu Azure, aby utworzyć nową aplikację lub zmodyfikować aplikację istniejącą w celu nawiązania połączenia z usługą mobilną.

W tej sekcji utworzysz nową uniwersalną aplikację systemu Windows połączoną z usługą mobilną.

1. W [klasyczny portal Azure] kliknij pozycję **Mobile Services**, a następnie kliknij wcześniej utworzoną usługę mobilną.
2. Na karcie szybkiego startu kliknij opcję **Windows** w obszarze **Wybierz platformę** i rozwiń listę **Create a new Windows Store app** (Utwórz nową aplikację do Sklepu Windows).
   
    Zostaną wyświetlone trzy proste kroki umożliwiające utworzenie aplikacji do Sklepu Windows i połączenie jej z usługą mobilną.
   
    ![Kroki procedury szybkiego startu dla usługi Mobile Services](./media/mobile-services-dotnet-backend-windows-store-dotnet-get-started/mobile-quickstart-steps.png)
3. Jeśli nie zostało to jeszcze zrobione, pobierz program [Visual Studio 2013] i zainstaluj go na komputerze lokalnym lub maszynie wirtualnej.
4. W obszarze **Download and run your app and service locally** (Pobierz i uruchom lokalnie aplikację i usługę) wybierz język dla aplikacji do Sklepu Windows, a następnie kliknij opcję **Pobierz**.
   
    Spowoduje to pobranie rozwiązania zawierającego projekty zarówno dla usługi mobilnej, jak i przykładowej aplikacji *Lista zadań do wykonania*, która jest połączona z usługą mobilną. Zapisz skompresowany plik projektu na komputerze lokalnym i zapamiętaj, gdzie został on zapisany.

## Testowanie aplikacji z lokalną usługą mobilną
[!INCLUDE [mobile-services-dotnet-backend-test-local-service-dotnet](../../includes/mobile-services-dotnet-backend-test-local-service-dotnet.md)]

> [!NOTE]
> Możesz zweryfikować kod uzyskujący dostęp do usługi mobilnej w celu wykonywania zapytań i wstawiania danych, który znajduje się w pliku MainPage.xaml.cs.
> 
> 

## Publikowanie usługi mobilnej
[!INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

<ol start="4">
<li><p>W projekcie kodu udostępnianego otwórz plik App.xaml.cs, odszukaj kod tworzący wystąpienie <a href="http://msdn.microsoft.com/library/Windowsazure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx" target="_blank">MobileServiceClient</a> i przekształć w komentarz kod tworzący tego klienta przy użyciu wartości <em>localhost</em>. Następnie usuń komentarz kodu tworzącego klienta przy użyciu adresu URL zdalnej usługi mobilnej, który wygląda podobnie do następującego:</p>

        <pre><code>public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://todolist.azure-mobile.net/",
            "XXXX-APPLICATION-KEY-XXXXX");</code></pre>

    <p>Klient będzie miał teraz dostęp do usługi mobilnej opublikowanej na platformie Azure.</p></li>
</ol>

## Testowanie aplikacji z usługą mobilną obsługiwaną na platformie Azure
Po opublikowaniu usługi mobilnej i połączeniu klienta ze zdalną usługą mobilną obsługiwaną na platformie Azure możemy uruchomić aplikację, używając platformy Azure do przechowywania elementów.

[!INCLUDE [mobile-services-windows-universal-test-app](../../includes/mobile-services-windows-universal-test-app.md)]

## Następne kroki
Po zakończeniu procedury szybkiego startu dowiedz się, jak wykonać dodatkowe ważne zadania w usłudze Mobile Services:

* [Dodawanie usługi Mobile Services do istniejącej aplikacji][Wprowadzenie do danych]
  <br/>Dowiedz się więcej o przechowywaniu danych i wykonywaniu zapytań w usłudze Mobile Services.
* [Wprowadzenie do synchronizacji danych w trybie offline]
  <br/>Dowiedz się, jak korzystać z synchronizacji danych w trybie offline, aby zapewnić dynamiczne i niezawodne działanie aplikacji.
* [Dodawanie uwierzytelniania do aplikacji usługi Mobile Services ][Wprowadzenie do uwierzytelniania]
  <br/>Dowiedz się, jak uwierzytelniać użytkowników aplikacji przy użyciu dostawcy tożsamości.
* [Dodawanie powiadomień wypychanych do aplikacji ][Wprowadzenie do powiadomień wypychanych]
  <br/>Dowiedz się, jak wysyłać do aplikacji bardzo proste powiadomienia wypychane.
* [Rozwiązywanie problemów z zapleczem usługi Mobile Services na platformie .NET]
  <br/> Dowiedz się, jak diagnozować i naprawiać ewentualne problemy dotyczące zaplecza usługi Mobile Services na platformie .NET.

Aby uzyskać więcej informacji na temat uniwersalnych aplikacji systemu Windows, zobacz temat [Obsługa wielu platform urządzeń przy użyciu jednej usługi mobilnej](mobile-services-how-to-use-multiple-clients-single-service.md#shared-vs).

[!INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->

<!-- Images. -->



<!-- URLs. -->
[Visual Studio 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Wprowadzenie do danych]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md
[Wprowadzenie do synchronizacji danych w trybie offline]: mobile-services-windows-store-dotnet-get-started-offline-data.md
[Wprowadzenie do uwierzytelniania]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-users.md
[Wprowadzenie do powiadomień wypychanych]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Zestaw SDK usług Mobile Services]: http://go.microsoft.com/fwlink/?LinkId=257545
[JavaScript i HTML]: mobile-services-win8-javascript/
[klasyczny portal Azure]: https://manage.windowsazure.com/
[Rozwiązywanie problemów z zapleczem usługi Mobile Services na platformie .NET]: mobile-services-dotnet-backend-how-to-troubleshoot.md



<!--HONumber=sep16_HO1-->


