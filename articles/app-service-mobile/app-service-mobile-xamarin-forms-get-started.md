---
title: "Rozpoczynanie pracy z funkcją Mobile Apps za pomocą platformy Xamarin.Forms"
description: "Wykonaj czynności opisane w tym samouczku, aby rozpocząć używanie funkcji Mobile Apps do tworzenia aplikacji platformy Xamarin.Forms"
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: ee12caaad4095cff6dae3282f747ae804f93db81
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-xamarinforms-app"></a>Tworzenie aplikacji platformy Xamarin.Forms
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Omówienie
W tym samouczku przedstawiono sposób dodawania usługi zaplecza opartej na chmurze do aplikacji mobilnej platformy Xamarin.Forms przy użyciu funkcji Mobile Apps usługi Azure App Service jako zaplecza. Tworzysz nowe zaplecze funkcji Mobile Apps oraz prostą aplikację platformy Xamarin.Forms typu Lista czynności do wykonania, która przechowuje dane aplikacji na platformie Azure.

Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usługi Mobile Apps dotyczących aplikacji platformy Xamarin.Forms.

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* Aktywne konto platformy Azure. Jeśli nie masz konta, możesz utworzyć konto wersji próbnej platformy Azure i uzyskać maksymalnie 10 bezpłatnych aplikacji mobilnych, z których możesz korzystać nawet po zakończeniu okresu ważności wersji próbnej. Aby uzyskać więcej informacji, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio z programem Xamarin. Aby uzyskać informacje, zobacz stronę [Set up and install for Visual Studio and Xamarin (Konfigurowanie i instalowanie dla programów Visual Studio i Xamarin)](https://msdn.microsoft.com/library/mt613162.aspx).

* Komputer Mac z zainstalowanym środowiskiem Xcode v7.0 lub nowszym i rozwiązaniem Xamarin Studio Community. Aby uzyskać informacje, zobacz tematy [Set up and install for Visual Studio and Xamarin (Konfigurowanie i instalowanie dla programów Visual Studio i Xamarin)](https://msdn.microsoft.com/library/mt613162.aspx) oraz [Set up, install, and verify for Mac users (Konfigurowanie, instalowanie i weryfikowanie dla użytkowników komputerów Mac)](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

## <a name="create-a-new-mobile-apps-back-end"></a>Tworzenie nowego zaplecza funkcji Mobile Apps

Aby utworzyć nowe zaplecze funkcji Mobile Apps, wykonaj następujące czynności:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Masz teraz zdefiniowane zaplecze funkcji Mobile Apps, którego mogą używać mobilne aplikacje klienckie. Teraz pobierasz projekt serwera dla prostego zaplecza typu „lista czynności do wykonania” i publikujesz go na platformie Azure.

## <a name="configure-the-server-project"></a>Konfigurowanie projektu serwera

Aby skonfigurować projekt serwera do użycia zaplecza Node.js lub .NET, wykonaj następujące czynności:

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinforms-solution"></a>Pobieranie i uruchamianie platformy Xamarin.Forms

Możesz pobrać rozwiązanie na dwa sposoby. Pobierz rozwiązanie na komputer Mac i otwórz je w środowisku Xamarin Studio lub pobierz rozwiązanie na komputer z systemem Windows i otwórz je w programie Visual Studio przy użyciu podłączonego do sieci komputera Mac w celu skompilowania aplikacji dla systemu iOS. Aby uzyskać więcej informacji, zobacz stronę [Set up and install Visual Studio and Xamarin (Konfigurowanie i instalowanie programów Visual Studio i Xamarin)](https://msdn.microsoft.com/library/mt613162.aspx).

Na komputerze Mac lub w systemie Windows wykonaj następujące czynności:

1. Przejdź do witryny [Azure Portal].

2. W bloku **Ustawienia** aplikacji mobilnej w obszarze **Mobilne** wybierz pozycję **Wprowadzenie** > **Xamarin.Forms**. W **kroku 3** wybierz pozycję **Utwórz nową aplikację**, a następnie wybierz pozycję **Pobierz**.

   Ta akcja spowoduje pobranie projektu zawierającego aplikację klienta połączoną z aplikacją mobilną. Zapisz skompresowany plik projektu na komputerze lokalnym i zapamiętaj, gdzie został on zapisany.

3. Wyodrębnij pobrany projekt, a następnie otwórz go w programie Xamarin Studio (Mac) lub Visual Studio (Windows).

   ![Projekt wyodrębniony w programie Xamarin Studio][9]

   ![Projekt wyodrębniony w programie Visual Studio][8]

## <a name="optional-run-the-ios-project"></a>(Opcjonalnie) Uruchamianie projektu iOS
W tej sekcji uruchamiasz projekt Xamarin iOS dla urządzeń z systemem iOS. Jeśli nie pracujesz z urządzeniami z systemem iOS, możesz pominąć tę sekcję.

#### <a name="in-xamarin-studio"></a>W programie Xamarin Studio
1. Kliknij prawym przyciskiem myszy projekt iOS, a następnie wybierz pozycję **Ustaw jako projekt startowy**.

2. W menu **Uruchom** wybierz pozycję **Rozpocznij debugowanie** w celu skompilowania projektu i uruchomienia aplikacji w emulatorze urządzenia iPhone.

#### <a name="in-visual-studio"></a>W programie Visual Studio
1. Kliknij prawym przyciskiem myszy projekt iOS, a następnie wybierz pozycję **Ustaw jako projekt startowy**.

2. W menu **Kompilacja** wybierz pozycję **Configuration Manager**.

3. W oknie dialogowym **Configuration Manager** zaznacz pola wyboru **Kompiluj** i **Wdróż** obok projektu iOS.

4. Aby skompilować projekt i uruchomić aplikację w emulatorze urządzenia iPhone, naciśnij klawisz **F5**.

   > [!NOTE]
   > Jeśli masz problemy z kompilowaniem projektu, uruchom menedżera pakietów NuGet i przeprowadź aktualizację do najnowszej wersji pakietów obsługi programu Xamarin. Aktualizowanie projektów typu Szybki start do najnowszej wersji może odbywać się wolno.    
   >
   >

5. W aplikacji wpisz znaczący tekst, na przykład *Poznaj program Xamarin*, a wybierz znak plusa (**+**).

    ![][10]

    Ta akcja spowoduje wysłanie żądania POST do nowego zaplecza funkcji Mobile Apps, które jest hostowane na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez zaplecze funkcji Mobile Apps, a dane są wyświetlane na liście.

    > [!NOTE]
    > Kod uzyskujący dostęp do zaplecza funkcji Mobile Apps można znaleźć w pliku C# TodoItemManager.cs projektu biblioteki klas przenośnych rozwiązania.
    >
    >

## <a name="optional-run-the-android-project"></a>(Opcjonalnie) Uruchamianie projektu systemu Android
W tej sekcji uruchamiasz projekt Xamarin (droid) dla systemu Android. Jeśli nie pracujesz z urządzeniami z systemem Android, możesz pominąć tę sekcję.

#### <a name="in-xamarin-studio"></a>W programie Xamarin Studio

1. Kliknij prawym przyciskiem myszy projekt Android, a następnie wybierz pozycję **Ustaw jako projekt startowy**.

2. W celu skompilowania projektu i uruchomienia aplikacji w emulatorze systemu Android w menu **Uruchom** wybierz pozycję **Rozpocznij debugowanie**.

#### <a name="in-visual-studio"></a>W programie Visual Studio

1. Kliknij prawym przyciskiem myszy projekt Android (Droid), a następnie wybierz pozycję **Ustaw jako projekt startowy**.

2. W menu **Kompilacja** wybierz pozycję **Configuration Manager**.

3. W oknie dialogowym **Configuration Manager** zaznacz pola wyboru **Kompiluj** i **Wdróż** obok projektu Android.

4. Aby skompilować projekt i uruchomić aplikację w emulatorze systemu Android, naciśnij klawisz **F5**.

   > [!NOTE]
   > Jeśli masz problemy z kompilowaniem projektu, uruchom menedżera pakietów NuGet i przeprowadź aktualizację do najnowszej wersji pakietów obsługi programu Xamarin. Aktualizowanie projektów typu Szybki start do najnowszej wersji może odbywać się wolno.    
   >
   >

5. W aplikacji wpisz znaczący tekst, na przykład *Poznaj program Xamarin*, a wybierz znak plusa (**+**).

    ![][11]
    
    Ta akcja spowoduje wysłanie żądania POST do nowego zaplecza funkcji Mobile Apps, które jest hostowane na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez zaplecze funkcji Mobile Apps, a dane są wyświetlane na liście.
    
    > [!NOTE]
    > Kod uzyskujący dostęp do zaplecza funkcji Mobile Apps można znaleźć w pliku C# TodoItemManager.cs projektu biblioteki klas przenośnych rozwiązania.
    >
    >

## <a name="optional-run-the-windows-project"></a>(Opcjonalnie) Uruchamianie projektu Windows

W tej sekcji uruchamiasz projekt Xamarin WinApp dla urządzeń z systemem Windows. Jeśli nie pracujesz z urządzeniami z systemem Windows, możesz pominąć tę sekcję.

#### <a name="in-visual-studio"></a>W programie Visual Studio

1. Kliknij prawym przyciskiem myszy dowolny projekt Windows, a następnie wybierz pozycję **Ustaw jako projekt startowy**.

2. W menu **Kompilacja** wybierz pozycję **Configuration Manager**.

3. W oknie dialogowym **Configuration Manager** zaznacz pola wyboru **Kompiluj** i **Wdróż** obok wybranego projektu Windows.

4. Aby skompilować projekt i uruchomić aplikację w emulatorze systemu Windows, naciśnij klawisz **F5**.

   > [!NOTE]
   > Jeśli masz problemy z kompilowaniem projektu, uruchom menedżera pakietów NuGet i przeprowadź aktualizację do najnowszej wersji pakietów obsługi programu Xamarin. Aktualizowanie projektów typu Szybki start do najnowszej wersji może odbywać się wolno.    
   >
   >

5. W aplikacji wpisz znaczący tekst, na przykład *Poznaj program Xamarin*, a wybierz znak plusa (**+**).

    Ta akcja spowoduje wysłanie żądania POST do nowego zaplecza funkcji Mobile Apps, które jest hostowane na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez zaplecze funkcji Mobile Apps, a dane są wyświetlane na liście.
    
    ![][12]
    
    > [!NOTE]
    > Kod uzyskujący dostęp do zaplecza funkcji Mobile Apps można znaleźć w pliku C# TodoItemManager.cs projektu biblioteki klas przenośnych rozwiązania.
    >
    >

## <a name="next-steps"></a>Następne kroki

* [Dodawanie uwierzytelniania do aplikacji](app-service-mobile-xamarin-forms-get-started-users.md)  
  Dowiedz się, jak uwierzytelniać użytkowników aplikacji przy użyciu dostawcy tożsamości.

* [Dodawanie powiadomień wypychanych do aplikacji](app-service-mobile-xamarin-forms-get-started-push.md)  
  Dowiedz się, jak dodać obsługę powiadomień wypychanych do aplikacji i skonfigurować zaplecze funkcji Mobile Apps na potrzeby wysyłania powiadomień wypychanych przy użyciu usługi Azure Notification Hubs.

* [Włączanie synchronizacji w trybie offline dla aplikacji](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Dowiedz się, jak dodać obsługę aplikacji w trybie offline przy użyciu zaplecza funkcji Mobile Apps. Synchronizacja w trybie offline umożliwia wyświetlanie, dodawanie lub modyfikowanie danych aplikacji mobilnej, nawet w przypadku braku połączenia sieciowego.

* [Używanie zarządzanego klienta funkcji Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)  
  Dowiedz się, jak pracować z zestawem SDK zarządzanego klienta w aplikacji platformy Xamarin.

<!-- Anchors. -->
[Get started with Mobile Apps back ends]:#getting-started
[Create a new Mobile Apps back end]:#create-new-service
[Next steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure Portal]: https://portal.azure.com/
