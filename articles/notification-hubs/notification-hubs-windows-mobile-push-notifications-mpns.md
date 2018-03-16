---
title: "Rozpoczynanie pracy z usługą Azure Notification Hubs dla aplikacji systemu Windows Phone | Microsoft Docs"
description: "Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji platformy Silverlight dla systemu Windows Phone 8 lub Windows Phone 8.1 przy użyciu usługi Azure Notification Hubs."
services: notification-hubs
documentationcenter: windows
keywords: powiadomienie wypychane, powiadomienia wypychane, wypychanie w systemie windows phone
author: jwhitedev
manager: kpiteira
editor: 
ms.assetid: d872d8dc-4658-4d65-9e71-fa8e34fae96e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/06/2018
ms.author: jawh
ms.openlocfilehash: 38d60001293a3bae6eb9f90179abb8af7815fbe7
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="get-started-with-azure-notification-hubs-for-windows-phone-apps"></a>Rozpoczynanie pracy z usługą Azure Notification Hubs dla aplikacji systemu Windows Phone
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Omówienie
> [!NOTE]
> Do wykonania kroków tego samouczka potrzebne jest aktywne konto platformy Azure. Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).
> 
> 

Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji platformy Silverlight dla systemu Windows Phone 8 lub Windows Phone 8.1 przy użyciu usługi Azure Notification Hubs. Jeśli aplikacja ma być przeznaczona dla systemu Windows Phone 8.1 (bez użycia platformy Silverlight), dodatkowe informacje zawiera wersja dotycząca [aplikacji uniwersalnych systemu Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
Korzystając z tego samouczka, utworzysz pustą aplikację dla systemu Windows Phone 8, która odbiera powiadomienia wypychane przy użyciu usługi powiadomień wypychanych firmy Microsoft (MPNS, Microsoft Push Notification Service). Po zakończeniu będzie można za pomocą centrum powiadomień wysyłać powiadomienia wypychane do wszystkich urządzeń z tą aplikacją.

> [!NOTE]
> Zestaw SDK usługi Notification Hubs dla systemu Windows Phone nie obsługuje używania usługi WNS (Windows Push Notification Service) z aplikacjami platformy Silverlight dla systemu Windows Phone 8.1. Aby używać usługi WNS (zamiast usługi MPNS) z aplikacjami platformy Silverlight dla systemu Windows Phone 8.1, postępuj zgodnie z instrukcjami w [samouczku dotyczącym usługi Notification Hubs dla aplikacji platformy Silverlight dla systemu Windows Phone], w którym zastosowano interfejsy API REST.
> 
> 

W tym samouczku został omówiony prosty scenariusz wysyłania przy użyciu usługi Notification Hubs.

## <a name="prerequisites"></a>Wymagania wstępne
Dla tego samouczka wymagane są następujące elementy:

* Program [Visual Studio 2012 Express for Windows Phone] lub nowsza wersja.

Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usługi Notification Hubs dotyczących aplikacji dla systemu Windows Phone 8.

## <a name="create-your-notification-hub"></a>Tworzenie centrum powiadomień
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>W obszarze <b>Usługi powiadomień</b> wybierz pozycję <b>Windows Phone (MPNS)</b>, a następnie kliknij pole wyboru <b>Włącz nieuwierzytelnione wypychanie</b>.</p>
</li>
</ol>

&emsp;&emsp;![Witryna Azure Portal — włączanie nieuwierzytelnionych powiadomień push](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

Centrum zostało utworzone i jest skonfigurowane do wysyłania nieuwierzytelnionych powiadomień do urządzeń z systemem Windows Phone.

> [!NOTE]
> W tym samouczku używana jest usługa MPNS w trybie nieuwierzytelnionym. W trybie nieuwierzytelnionym usługi MPNS występują ograniczenia dotyczące powiadomień, które można wysyłać do poszczególnych kanałów. Usługa Notification Hubs obsługuje [tryb uwierzytelniony usługi MPNS](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx), umożliwiając przekazanie certyfikatu.
> 
> 

## <a name="connecting-your-app-to-the-notification-hub"></a>Łączenie aplikacji z centrum powiadomień
1. W programie Visual Studio utwórz nową aplikację dla systemu Windows Phone 8.
   
    ![Visual Studio — Nowy projekt — Aplikacja dla systemu Windows Phone][13]
   
    W programie Visual Studio 2013 Update 2 lub nowszym zamiast tego utwórz aplikację platformy Silverlight dla systemu Windows Phone.
   
    ![Visual Studio — Nowy projekt — Pusta aplikacja — Windows Phone Silverlight][11]
2. W programie Visual Studio kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij pozycję **Zarządzaj pakietami NuGet**.
   
    Zostanie wyświetlone okno dialogowe **Zarządzanie pakietami NuGet**.
3. Wyszukaj pakiet `WindowsAzure.Messaging.Managed` i kliknij pozycję **Zainstaluj**, a następnie zaakceptuj warunki użytkowania.
   
    ![Visual Studio — menedżer pakietów NuGet][20]
   
    Spowoduje to pobranie i zainstalowanie biblioteki Azure Messaging dla systemu Windows oraz dodanie odwołania do niej przy użyciu <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">pakietu NuGet WindowsAzure.Messaging.Managed</a>.
4. Otwórz plik App.xaml.cs i dodaj następujące instrukcje `using`:
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
5. Dodaj następujący kod w górnej części metody **Application_Launching** w pliku App.xaml.cs:
   
        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }
   
        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());
   
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });
   
   > [!NOTE]
   > Wartość **MyPushChannel** jest indeksem służącym do wyszukiwania istniejącego kanału w kolekcji [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx). Jeśli nie istnieje, utwórz nowy wpis o tej nazwie.
   > 
   > 
   
    Wstaw nazwę centrum i parametry połączenia o nazwie **DefaultListenSharedAccessSignature** uzyskane w poprzedniej sekcji.
    Ten kod pobiera identyfikator URI kanału dla aplikacji z usługi MPNS, a następnie rejestruje ten identyfikator URI kanału w centrum powiadomień. Gwarantuje również to, że identyfikator URI kanału jest rejestrowany w centrum powiadomień przy każdym uruchomieniu aplikacji.
   
   > [!NOTE]
   > W tym samouczku do urządzenia wysyłane jest wyskakujące powiadomienie. W przypadku wysyłania powiadomienia na kafelku należy zamiast tego wywołać metodę **BindToShellTile** na kanale. W celu obsługi wyskakujących powiadomień oraz powiadomień na kafelku należy wywołać metodę **BindToShellTile** i **BindToShellToast**.
   > 
   > 
6. W Eksploratorze rozwiązań rozwiń węzeł **Właściwości**, otwórz plik `WMAppManifest.xml`, kliknij kartę **Możliwości** i upewnij się, że możliwość **ID_CAP_PUSH_NOTIFICATION** jest zaznaczona.
   
    ![Visual Studio — możliwości aplikacji dla systemu Windows Phone][14]
   
    Dzięki temu aplikacja może odbierać powiadomienia wypychane. Bez tego wszystkie próby wysłania powiadomienia wypychanego do aplikacji zakończą się niepowodzeniem.
7. Naciśnij klawisz `F5`, aby uruchomić aplikację.
   
    W aplikacji zostanie wyświetlony komunikat dotyczący rejestracji.
8. Zamknij aplikację.  
   
   > [!NOTE]
   > Aby otrzymywać wyskakujące powiadomienia wypychane, aplikacja nie może być uruchomiona na pierwszym planie.
   >

## <a name="next-steps"></a>Następne kroki
W tym prostym przykładzie wysłano powiadomienia wypychane do wszystkich urządzeń z systemem Windows Phone 8. 

Aby skierować je do określonych użytkowników, zapoznaj się z samouczkiem [Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs]. 

Jeśli chcesz podzielić użytkowników na grupy zainteresowań, zobacz [Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]. 

Dowiedz się więcej o sposobie użycia usługi Notification Hubs w temacie [Wskazówki dotyczące usługi Notification Hubs].

<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Visual Studio 2012 Express for Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Wskazówki dotyczące usługi Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[toast catalog]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[tile catalog]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[samouczku dotyczącym usługi Notification Hubs dla aplikacji platformy Silverlight dla systemu Windows Phone]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

