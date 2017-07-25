
1. Kliknij pozycję **App Services**, wybierz zaplecze aplikacji mobilnej, kliknij pozycję **Szybki start** > platforma klienta (iOS, Android, Xamarin, Cordova).

![Witryna Azure Portal z wyróżnioną pozycją Mobile Apps Quickstart][quickstart]

2. Jeśli nie skonfigurowano połączenia z bazą danych, należy utworzyć jedno połączenie danych.

![Witryna Azure Portal z opcją połączenia aplikacji mobilnej z bazą danych][connect]

  * Utwórz nową bazę danych SQL i serwer.

  ![Witryna Azure Portal z opcją tworzenia nowej bazy danych i serwera aplikacji mobilnej][server]

  * Poczekaj na pomyślne utworzenie połączenia danych.

  ![Witryna Azure Portal z powiadomieniem o utworzeniu połączenia danych aplikacji mobilnej][notification]

  * Połączenie danych musi się powieść.

  ![Witryna Azure Portal z powiadomieniem o utworzeniu połączenia danych aplikacji mobilnej][already-connection]

3. W obszarze **2. Utwórz tabelę interfejsu API** wybierz dla języka Node.js opcję **Język zaplecza**. Zaakceptuj potwierdzenie i kliknij opcję **Utwórz tabelę TodoItem**. Spowoduje to utworzenie nowej tabeli *TodoItem* w bazie danych. Pamiętaj, że przełączenie istniejącej wewnętrznej bazy danych na Node.js spowoduje zastąpienie całej zawartości! Aby zamiast tego utworzyć zaplecze platformy .NET, [wykonaj następujące instrukcje][instructions].

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
