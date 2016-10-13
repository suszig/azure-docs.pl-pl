<properties
    pageTitle="Rozpoczynanie pracy z usługą Mobile Apps za pomocą platformy Xamarin.Forms"
    description="Wykonaj czynności opisane w tym samouczku, aby rozpocząć używanie usługi Azure Mobile Apps do tworzenia aplikacji platformy Xamarin.Forms."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>


#Tworzenie aplikacji platformy Xamarin.Forms

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##Omówienie

W tym samouczku przedstawiono sposób dodawania usługi zaplecza opartej na chmurze do aplikacji mobilnej platformy Xamarin.Forms przy użyciu zaplecza Aplikacji mobilnej Azure. Będziesz tworzyć zaplecze nowej aplikacji mobilnej oraz prostą aplikację platformy Xamarin.Forms typu _Lista czynności do wykonania_, która przechowuje dane aplikacji na platformie Azure.

Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usługi Mobile Apps dotyczących aplikacji platformy Xamarin.Forms.

##Wymagania wstępne

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* Aktywne konto platformy Azure. Jeśli nie masz konta, możesz utworzyć konto wersji próbnej platformy Azure i uzyskać maksymalnie 10 bezpłatnych usług Mobile Apps, z których możesz korzystać nawet po zakończeniu okresu ważności wersji próbnej. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio z programem Xamarin. Instrukcje można znaleźć w temacie [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (Konfigurowanie i instalowanie dla programów Visual Studio i Xamarin). 

* Komputer Mac z zainstalowanym środowiskiem Xcode v7.0 lub nowszym i rozwiązaniem Xamarin Studio Community. Zobacz tematy [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (Konfigurowanie i instalowanie dla programów Visual Studio i Xamarin) oraz [Setup, install, and verifications for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (Konfigurowanie, instalowanie oraz weryfikacje dla użytkowników komputerów Mac) (MSDN).
 
>[AZURE.NOTE] Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Try App Service](https://tryappservice.azure.com/?appServiceName=mobile) (Wypróbuj usługę App Service), w którym wyjaśniono, jak od razu utworzyć początkową aplikację mobilną o krótkim okresie istnienia w usłudze App Service. Bez kart kredytowych i bez zobowiązań.

## Tworzenie zaplecza nowej Aplikacji mobilnej Azure

Wykonaj te kroki, aby utworzyć zaplecze nowej Aplikacji mobilnej.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]


W ten sposób zainicjowano obsługę zaplecza Aplikacji mobilnej Azure, które może być używane przez aplikacje mobilne klientów. Teraz pobierzesz projekt serwera dla prostego zaplecza typu „lista czynności do wykonania” i opublikujesz go na platformie Azure.

## Konfigurowanie projektu serwera

Wykonaj poniższe kroki, aby skonfigurować projekt serwera do użycia zaplecza Node.js lub .NET.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

##Pobieranie i uruchamianie platformy Xamarin.Forms

W tym miejscu masz kilka możliwości. Możesz pobrać rozwiązanie na komputer Mac i otworzyć je w środowisku Xamarin Studio lub pobrać rozwiązanie na komputer z systemem Windows i otworzyć je w programie Visual Studio przy użyciu podłączonego do sieci komputera Mac w celu skompilowania aplikacji dla systemu iOS. Jeśli potrzebujesz szczegółowych instrukcji dotyczących scenariuszy konfigurowania rozwiązania Xamarin, zobacz [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (Konfigurowanie i instalowanie dla programów Visual Studio i Xamarin).

Kontynuujmy:

 1. Na komputerze Mac lub na komputerze z systemem Windows otwórz [Azure Portal] w oknie przeglądarki.
 2. W bloku ustawień Aplikacji mobilnej kliknij pozycję **Wprowadzenie** (w obszarze Mobilne) > **Xamarin.Forms**. W kroku 3 kliknij pozycję **Utwórz nową aplikację**, jeśli nie została jeszcze wybrana.  Następnie kliknij przycisk **Pobierz**.

    Spowoduje to pobranie projektu zawierającego aplikację klienta połączoną z aplikacją mobilną. Zapisz skompresowany plik projektu na komputerze lokalnym i zapamiętaj, gdzie został on zapisany.

 3. Wyodrębnij pobrany projekt, a następnie otwórz go w programie Xamarin Studio lub Visual Studio.

    ![][9]

    ![][8]


##(Opcjonalnie) Uruchamianie projektu iOS

Ta sekcja dotyczy uruchamiania projektu Xamarin iOS dla urządzeń z systemem iOS. Jeśli nie pracujesz z urządzeniami z systemem iOS, możesz pominąć tę sekcję.

####W programie Xamarin Studio

1. Kliknij prawym przyciskiem myszy projekt iOS, a następnie kliknij pozycję **Ustaw jako projekt startowy**.
2. W menu **Uruchom** kliknij pozycję **Rozpocznij debugowanie** w celu skompilowania projektu i uruchomienia aplikacji w emulatorze urządzenia iPhone.

####W programie Visual Studio
1. Kliknij prawym przyciskiem myszy projekt iOS, a następnie kliknij pozycję **Ustaw jako projekt startowy**.
2. W menu **Kompilacja** kliknij pozycję **Configuration Manager**.
3. W oknie dialogowym **Configuration Manager** zaznacz pola wyboru **Kompiluj** i **Wdróż** dla projektu iOS.
4. Naciśnij klawisz **F5**, aby skompilować projekt i uruchomić aplikację w emulatorze urządzenia iPhone.

    >[AZURE.NOTE] Jeśli masz problemy z kompilacją, uruchom menedżera pakietów NuGet i przeprowadź aktualizację do najnowszej wersji pakietów obsługi programu Xamarin. Czasami aktualizacja projektów typu Szybki start do najnowszej wersji może być opóźniona.    

W aplikacji wpisz znaczący tekst, na przykład _Poznaj program Xamarin_, a następnie kliknij przycisk **+**.

![][10]

Spowoduje to wysłanie żądania POST do zaplecza nowej aplikacji mobilnej, która jest obsługiwana na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez zaplecze aplikacji mobilnej, a dane są wyświetlane na liście.

>[AZURE.NOTE]
> Kod uzyskujący dostęp do zaplecza aplikacji mobilnej można znaleźć w pliku C# TodoItemManager.cs projektu biblioteki klas przenośnych rozwiązania.

##(Opcjonalnie) Uruchamianie projektu systemu Android

Ta sekcja dotyczy uruchamiania projektu Xamarin (droid) dla urządzeń z systemem Android. Jeśli nie pracujesz z urządzeniami z systemem Android, możesz pominąć tę sekcję.

####W programie Xamarin Studio

1. Kliknij prawym przyciskiem myszy projekt Android, a następnie kliknij pozycję **Ustaw jako projekt startowy**.
2. W menu **Uruchom** kliknij pozycję **Rozpocznij debugowanie** w celu skompilowania projektu i uruchomienia aplikacji w emulatorze systemu Android.

####W programie Visual Studio
1. Kliknij prawym przyciskiem myszy projekt Android (Droid), a następnie kliknij pozycję **Ustaw jako projekt startowy**.
4. W menu **Kompilacja** kliknij pozycję **Configuration Manager**.
5. W oknie dialogowym **Configuration Manager** zaznacz pola wyboru **Kompiluj** i **Wdróż** dla projektu Android.
6. Naciśnij klawisz **F5**, aby skompilować projekt i uruchomić aplikację w emulatorze systemu Android.

    >[AZURE.NOTE] Jeśli masz problemy z kompilacją, uruchom menedżera pakietów NuGet i przeprowadź aktualizację do najnowszej wersji pakietów obsługi programu Xamarin. Czasami aktualizacja projektów typu Szybki start do najnowszej wersji może być opóźniona.    


W aplikacji wpisz znaczący tekst, na przykład _Poznaj program Xamarin_, a następnie kliknij przycisk **+**.

![][11]

Spowoduje to wysłanie żądania POST do zaplecza nowej aplikacji mobilnej, która jest obsługiwana na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez zaplecze aplikacji mobilnej, a dane są wyświetlane na liście.

> [AZURE.NOTE]
> Kod uzyskujący dostęp do zaplecza aplikacji mobilnej można znaleźć w pliku C# TodoItemManager.cs projektu biblioteki klas przenośnych rozwiązania.


##(Opcjonalnie) Uruchamianie projektu Windows


Ta sekcja dotyczy uruchamiania projektu Xamarin WinApp dla urządzeń z systemem Windows. Jeśli nie pracujesz z urządzeniami z systemem Windows, możesz pominąć tę sekcję.


####W programie Visual Studio
1. Kliknij prawym przyciskiem myszy projekt Windows, a następnie kliknij pozycję **Ustaw jako projekt startowy**.
4. W menu **Kompilacja** kliknij pozycję **Configuration Manager**.
5. W oknie dialogowym **Configuration Manager** zaznacz pola wyboru **Kompiluj** i **Wdróż** dla wybranego projektu Windows.
6. Naciśnij klawisz **F5**, aby skompilować projekt i uruchomić aplikację w emulatorze systemu Windows.

    >[AZURE.NOTE] Jeśli masz problemy z kompilacją, uruchom menedżera pakietów NuGet i przeprowadź aktualizację do najnowszej wersji pakietów obsługi programu Xamarin. Czasami aktualizacja projektów typu Szybki start do najnowszej wersji może być opóźniona.    


W aplikacji wpisz znaczący tekst, na przykład _Poznaj program Xamarin_, a następnie kliknij przycisk **+**.

Spowoduje to wysłanie żądania POST do zaplecza nowej aplikacji mobilnej, która jest obsługiwana na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez zaplecze aplikacji mobilnej, a dane są wyświetlane na liście.

![][12]

> [AZURE.NOTE]
> Kod uzyskujący dostęp do zaplecza aplikacji mobilnej można znaleźć w pliku C# TodoItemManager.cs projektu biblioteki klas przenośnych rozwiązania.

##Następne kroki

* [Dodawanie uwierzytelniania do aplikacji](app-service-mobile-xamarin-forms-get-started-users.md)  
Dowiedz się, jak uwierzytelniać użytkowników aplikacji przy użyciu dostawcy tożsamości.

* [Dodawanie powiadomień wypychanych do aplikacji](app-service-mobile-xamarin-forms-get-started-push.md)  
Dowiedz się, jak dodać obsługę powiadomień wypychanych do aplikacji i skonfigurować zaplecze Aplikacji mobilnej na potrzeby wysyłania powiadomień wypychanych przy użyciu usługi Azure Notification Hubs.

* [Włączanie synchronizacji w trybie offline dla aplikacji](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Dowiedz się, jak dodać obsługę aplikacji w trybie offline przy użyciu zaplecza Aplikacji mobilnej. Synchronizacja w trybie offline umożliwia użytkownikom końcowym interakcję z aplikacją mobilną &mdash; przeglądanie, dodawanie lub modyfikowanie danych &mdash; nawet w przypadku braku połączenia sieciowego.

* [Jak używać zarządzanego klienta usługi Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)  
Dowiedz się, jak pracować z zestawem SDK zarządzanego klienta w aplikacji platformy Xamarin. 


<!-- Anchors. -->
[Wprowadzenie do zapleczy aplikacji mobilnych]:#getting-started
[Tworzenie zaplecza nowej aplikacji mobilnej]:#create-new-service
[Następne kroki]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Zestaw SDK aplikacji mobilnych]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure Portal]: https://portal.azure.com/




<!--HONumber=Oct16_HO1-->


