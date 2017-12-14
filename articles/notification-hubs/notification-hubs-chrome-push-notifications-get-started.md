---
title: "Wysyłanie powiadomień wypychanych do aplikacji programu Chrome przy użyciu usługi Azure Notification Hubs | Microsoft Docs"
description: "Dowiedz się, jak wysyłać powiadomienia wypychane do aplikacji dla programu Chrome przy użyciu usługi Azure Notification Hubs."
services: notification-hubs
keywords: "powiadomienia wypychane na urządzeniach przenośnych, powiadomienia wypychane, powiadomienie wypychane, powiadomienia wypychane w przeglądarce chrome"
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 75d4ff59-d04a-455f-bd44-0130a68e641f
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-chrome
ms.devlang: JavaScript
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 107c001f1b02874adfdc53856f18e6bfcbcb0cf4
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2017
---
# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Wysyłanie powiadomień wypychanych do aplikacji dla programu Chrome przy użyciu usługi Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

W tym temacie pokazano, jak za pomocą usługi Azure Notification Hubs wysyłać powiadomienia wypychane, które będą wyświetlane w kontekście przeglądarki Google Chrome. W tym samouczku utworzymy aplikację dla programu Chrome, która odbiera powiadomienia wypychane za pomocą usługi [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/). 

> [!NOTE]
> Do wykonania kroków tego samouczka potrzebne jest aktywne konto platformy Azure. Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).
> 
> 

Ten samouczek zawiera opis podstawowych kroków umożliwiających włączenie powiadomień wypychanych:

* [Włączanie usługi Google Cloud Messaging](#register)
* [Konfigurowanie centrum powiadomień](#configure-hub)
* [Łączenie aplikacji dla programu Chrome z centrum powiadomień](#connect-app)
* [Wysyłanie powiadomienia wypychanego do aplikacji dla programu Chrome](#send)
* [Dodatkowe funkcje i możliwości](#next-steps)

> [!NOTE]
> Powiadomienia wypychane aplikacji dla programu Chrome nie są standardowymi powiadomieniami w przeglądarce — są to specjalne powiadomienia modelu rozszerzeń przeglądarki. Szczegółowe informacje znajdują się w temacie [Chrome Apps Overview] (Omówienie aplikacji dla programu Chrome). Aplikacje dla programu Chrome mogą działać w przeglądarce dla komputerów oraz na urządzeniach przenośnych (Android i iOS) przy użyciu platformy Apache Cordova. Aby dowiedzieć się więcej, zobacz [Chrome Apps on Mobile] (Aplikacja dla programu Chrome na urządzeniach przenośnych).
> 
> 

Sposób konfigurowania usługi GCM i Azure Notification Hubs jest taki sam jak w przypadku konfigurowania dla systemu Android, ponieważ [usługa Google Cloud Messaging dla programu Chrome] jest przestarzała, a ta sama usługa GCM obsługuje teraz urządzenia z systemem Android i wystąpienia programu Chrome.

## <a id="register"></a>Włączanie usługi Google Cloud Messaging
1. Przejdź do witryny sieci Web [Google Cloud Console], zaloguj się przy użyciu poświadczeń konta Google, a następnie kliknij przycisk **Create project** (Utwórz projekt). Wprowadź odpowiednią nazwę w polu **Project name** (Nazwa projektu), a następnie kliknij przycisk **Create** (Utwórz).
   
       ![Google Cloud Console - Create Project][1]
2. Zanotuj wartość w polu **Project Number** (Numer projektu) na stronie **Projects** (Projekty) dla utworzonego projektu. Zostanie ona użyta w polu **GCM Sender ID** (Identyfikator nadawcy usługi GCM) w aplikacji dla programu Chrome w celu rejestracji w usłudze GCM.
   
       ![Google Cloud Console - Project Number][2]
3. W okienku po lewej stronie kliknij pozycję **APIs & auth** (Interfejsy API i uwierzytelnianie), a następnie przewiń w dół i kliknij, aby włączyć opcję **Google Cloud Messaging for Android** (Usługa Google Cloud Messaging dla systemu Android). Nie trzeba włączać opcji **usługa Google Cloud Messaging dla programu Chrome** (Usługa Google Cloud Messaging dla programu Chrome).
   
       ![Google Cloud Console - Server Key][3]
4. W okienku po lewej stronie kliknij pozycję **Credentials** > **Create New Key** >  **Server Key** >  **Create** (Poświadczenia — Utwórz nowy klucz — Klucz serwera — Utwórz).
   
       ![Google Cloud Console - Credentials][4]
5. Zanotuj wartość w polu **API Key** (Klucz interfejsu API) serwera. Zostanie ona następnie użyta podczas konfigurowania centrum powiadomień do wysyłania powiadomień wypychanych do usługi GCM.
   
       ![Google Cloud Console - API Key][5]

## <a id="configure-hub"></a>Konfigurowanie centrum powiadomień
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6.   W bloku **Ustawienia** wybierz pozycję **Usługi powiadomień**, a następnie wybierz pozycję **Google (GCM)**. Wprowadź klucz interfejsu API i zapisz.

&emsp;&emsp;![Azure Notification Hubs — Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

## <a id="connect-app"></a>Łączenie aplikacji dla programu Chrome z centrum powiadomień
Twoje centrum powiadomień jest teraz skonfigurowane do pracy z usługą GCM i uzyskano parametry połączenia do rejestrowania aplikacji w celu odbierania i wysyłania powiadomień wypychanych. LK

### <a name="create-a-new-chrome-app"></a>Tworzenie nowej aplikacji dla programu Chrome
Poniższy przykład jest oparty na [przykładzie usługi GCM dla aplikacji dla programu Chrome] i przedstawiono w nim zalecany sposób tworzenia aplikacji dla programu Chrome. Kroki dotyczące usługi Azure Notification Hubs zostały wyróżnione. 

> [!NOTE]
> Zalecane jest pobranie źródła tej aplikacji dla programu Chrome z [przykładu centrum powiadomień aplikacji dla programu Chrome].
> 
> 

Aplikacja dla programu Chrome jest tworzona przy użyciu języka JavaScript i można ją utworzyć za pomocą dowolnego edytora tekstu. Poniżej przedstawiono wygląd tworzonej aplikacji dla programu Chrome.

![Aplikacja dla programu Google Chrome][15]

1. Utwórz folder i nadaj mu nazwę `ChromePushApp`. Oczywiście możesz użyć dowolnej nazwy. Jeśli użyjesz innej nazwy, musisz zmienić ścieżkę w wymaganych segmentach kodu.
2. Pobierz [bibliotekę crypto-js] do folderu utworzonego w drugim kroku. Ten folder biblioteki będzie zawierać dwa podfoldery: `components` i `rollups`.
3. Utwórz plik `manifest.json`. Dla wszystkich aplikacji dla programu Chrome jest tworzony plik manifestu zawierający metadane aplikacji oraz, co najistotniejsze, wszystkie uprawnienia przyznane aplikacji podczas instalacji przez użytkownika.
   
        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }
   
    Zwróć uwagę na element `permissions`, który określa, że ta aplikacja dla programu Chrome będzie mogła odbierać powiadomienia wypychane z usługi GCM. Musi on również określać identyfikator URI usługi Azure Notification Hubs, względem której aplikacja dla programu Chrome będzie wykonywać wywołanie interfejsu REST w celu zarejestrowania.
    Nasza przykładowa aplikacja używa również pliku ikony, `gcm_128.png`, który można znaleźć w źródle pochodzącym z oryginalnego przykładu usługi GCM. Można zastąpić go dowolnym obrazem spełniającym [kryteria ikony](https://developer.chrome.com/apps/manifest/icons).
4. Utwórz plik o nazwie `background.js` zawierający następujący kod:
   
        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }
   
        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.
   
          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);
   
          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }
   
        var registerWindowCreated = false;
   
        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {
   
            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }
   
        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);
   
        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);
   
    To jest plik, który powoduje wyświetlenie strony HTML (**register.html**) w oknie aplikacji dla programu Chrome, a także określa procedurę obsługi **messageReceived** służącą do obsługi przychodzących powiadomień wypychanych.
5. Utwórz plik o nazwie `register.html`, który określa interfejs użytkownika aplikacji dla programu Chrome. 
   
   > [!NOTE]
   > W tym przykładzie użyto biblioteki **CryptoJS w wersji 3.1.2**. Jeśli pobrano inną wersję biblioteki, upewnij się, że prawidłowo zastąpiono wersję w ścieżce `src`.
   > 
   > 
   
        <html>
   
        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>
   
        <body>
   
        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>
   
        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>
   
        <br/>
   
        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>
   
        <br/>
        <br/>
   
        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>
   
        </body>
   
        </html>
6. Utwórz plik o nazwie `register.js` zawierający poniższy kod. Ten plik zawiera skrypt użyty w pliku `register.html`. Wykonywanie śródwierszowe w aplikacjach dla programu Chrome jest niedozwolone, dlatego należy utworzyć oddzielny skrypt pomocniczy dla interfejsu użytkownika.
   
        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";
   
        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }
   
        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }
   
          document.getElementById("console").innerHTML = currentStatus  + status;
        }
   
        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);
   
          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }
   
        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;
   
          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }
   
          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;
   
          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }
   
        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }
   
        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";
   
          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });
   
          originalUri = endpoint + hubName;
        }
   
        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration
   
          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;
   
          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);
   
          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }
   
        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";
   
          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);
   
          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();
   
          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };
   
          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }
   
          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");
   
          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }
   
    Powyższy skrypt zawiera następujące najważniejsze parametry:
   
   * **window.onload** określa zdarzenia dotyczące kliknięcia przycisków dla dwóch przycisków w interfejsie użytkownika. Jeden służy do rejestracji w usłudze GCM, a drugi używa identyfikatora rejestracji zwróconego po zarejestrowaniu w usłudze GCM do rejestracji w usłudze Azure Notification Hubs.
   * **updateLog** jest funkcją, która umożliwia obsługę prostych funkcji logowania.
   * **registerWithGCM** jest procedurą obsługi kliknięcia pierwszego przycisku, który wykonuje wywołanie `chrome.gcm.register` do usługi GCM w celu zarejestrowania bieżącego wystąpienia aplikacji dla programu Chrome.
   * **registerCallback** jest funkcją wywołania zwrotnego, która jest wywoływana po zwróceniu danych przez wywołanie rejestracji w usłudze GCM.
   * **registerWithNH** jest programem obsługi kliknięcia drugiego przycisku, który dokonuje rejestracji w usłudze Notification Hubs. Pobiera on właściwości `hubName` oraz `connectionString` (określone przez użytkownika) i tworzy wywołanie rejestracji w usłudze Notification Hubs za pomocą interfejsu API REST.
   * **splitConnectionString** i **generateSaSToken** są pomocnikami reprezentującymi implementację procesu tworzenia tokenu SaS w języku JavaScript, która musi być używana we wszystkich wywołaniach interfejsu API REST. Aby uzyskać więcej informacji, zobacz temat [Common Concepts](http://msdn.microsoft.com/library/dn495627.aspx) (Wspólne pojęcia).
   * **sendNHRegistrationRequest** jest funkcją wykonującą wywołanie interfejsu REST protokołu HTTP do usługi Azure Notification Hubs.
   * **registrationPayload** definiuje ładunek XML rejestracji. Aby uzyskać więcej informacji, zobacz temat [Create Registration NH REST API] (Interfejs API REST rejestracji w usłudze Notification Hubs). Aktualizujemy w nim identyfikator rejestracji otrzymany z usługi GCM.
   * **client** jest wystąpieniem obiektu **XMLHttpRequest** używanym do tworzenia żądań POST protokołu HTTP. Zauważ, że aktualizujemy nagłówek `Authorization` przy użyciu elementu `sasToken`. Pomyślne wykonanie tego wywołania spowoduje zarejestrowanie tego wystąpienia aplikacji dla programu Chrome w usłudze Azure Notification Hubs.

Ogólna struktura folderów dla tego projektu powinna wyglądać następująco: ![Aplikacja dla programu Google Chrome — struktura folderów][21]

### <a name="set-up-and-test-your-chrome-app"></a>Instalowanie i testowanie aplikacji dla programu Chrome
1. Otwórz przeglądarkę Chrome. Otwórz **stronę Rozszerzenia programu Chrome** i włącz **Tryb programisty**.
   
       ![Google Chrome - Enable Developer Mode][16]
2. Kliknij przycisk **Wczytaj rozszerzenie bez pakietu** i przejdź do folderu, w którym utworzono pliki. Możesz również użyć narzędzia **Chrome Apps & Extensions Developer Tool**. To narzędzie samo w sobie jest aplikacją dla programu Chrome (instalowaną ze sklepu Chrome Web Store) i zapewnia zaawansowane możliwości debugowania podczas tworzenia aplikacji dla programu Chrome.
   
       ![Google Chrome - Load Unpacked Extension][17]
3. Jeśli aplikacja dla programu Chrome została utworzona bez żadnych błędów, zostanie wyświetlona.
   
       ![Google Chrome - Chrome App Display][18]
4. Wprowadź wartość w polu **Project Number** (Numer projektu), którą uzyskano wcześniej w witrynie **Google Cloud Console**, jako identyfikator nadawcy i kliknij przycisk **Register with GCM** (Zarejestruj w usłudze GCM). Musi zostać wyświetlony komunikat **Registration with GCM succeeded** (Pomyślnie zarejestrowano w usłudze GCM).
   
       ![Google Chrome - Chrome App Customization][19]
5. Wprowadź wartość w polu **Notification Hub Name** (Nazwa centrum powiadomień) oraz wartość **DefaultListenSharedAccessSignature** uzyskaną wcześniej w portalu i kliknij przycisk pozycję **Register with Azure Notification Hub** (Zarejestruj w centrum powiadomień Azure). Musi zostać wyświetlony komunikat **Notification Hub Registration successful!** (Pomyślnie zarejestrowano w centrum powiadomień!) oraz szczegóły odpowiedzi dotyczącej rejestracji, które zawierają identyfikator rejestracji w usłudze Azure Notification Hubs.
   
       ![Google Chrome - Specify Notification Hub Details][20]  

## <a name="send"></a>Wysyłanie powiadomienia do aplikacji dla programu Chrome
W celach testowych wyślemy powiadomienia wypychane programu Chrome przy użyciu aplikacji konsolowej programu .NET. 

> [!NOTE]
> Powiadomienia wypychane można wysyłać przy użyciu usługi Notification Hubs z poziomu dowolnego zaplecza za pośrednictwem naszego publicznego <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">interfejsu REST</a>. Odwiedź nasz [portal dokumentacji](https://azure.microsoft.com/documentation/services/notification-hubs/), aby uzyskać więcej przykładów dla różnych platform.
> 
> 

1. W programie Visual Studio z menu **Plik** wybierz pozycję **Nowy**, a następnie wybierz pozycję **Projekt**. W obszarze **Visual C#** kliknij pozycję **Windows**, kliknij pozycję **Aplikacja konsolowa**, a następnie kliknij przycisk **OK**.  Spowoduje to utworzenie nowego projektu aplikacji konsolowej.
2. W menu **Narzędzia** kliknij pozycję **Menedżer pakietów biblioteki**, a następnie kliknij pozycję **Konsola menedżera pakietów**. Spowoduje to wyświetlenie konsoli menedżera pakietów.
3. W oknie konsoli wykonaj następujące polecenie:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
       This adds a reference to the Azure Service Bus SDK with the <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet package</a>.
4. Otwórz plik `Program.cs` i dodaj następującą instrukcję `using`:
   
        using Microsoft.Azure.NotificationHubs;
5. W klasie `Program` dodaj następującą metodę:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }
   
       Make sure to replace the `<hub name>` placeholder with the name of the notification hub that appears in the [portal](https://portal.azure.com) in your Notification Hub blade. Also, replace the connection string placeholder with the connection string called `DefaultFullSharedAccessSignature` that you obtained in the notification hub configuration section.
   
   > [!NOTE]
   > Upewnij się, że użyto parametrów połączenia z uprawnieniami dostępu **Pełne**, a nie **Nasłuchiwanie**. Parametry połączenia z uprawnieniami dostępu **Nasłuchiwanie** nie przyznają uprawnień do wysyłania powiadomień wypychanych.
   > 
   > 
6. Dodaj następujące wywołania do metody `Main`:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Upewnij się, że program Chrome jest uruchomiony, i uruchom aplikację konsolową.
8. Na pulpicie powinno zostać wyświetlone następujące powiadomienie.
   
       ![Google Chrome - Notification][13]
9. Wszystkie powiadomienia można również wyświetlić przy użyciu okna powiadomień programu Chrome na pasku zadań (w systemie Windows), gdy program Chrome jest uruchomiony.
   
       ![Google Chrome - Notifications List][14]

> [!NOTE]
> Aplikacja dla programu Chrome nie musi być uruchomiona lub otwarta w przeglądarce (chociaż przeglądarka Chrome musi być uruchomiona). Okno powiadomień programu Chrome umożliwia również wyświetlenie skonsolidowanego widoku wszystkich powiadomień.
> 
> 

## <a name="next-steps"> </a>Następne kroki
Dowiedz się więcej o usłudze Notification Hubs w temacie [Notification Hubs Overview] (Notification Hubs — omówienie).

Aby wyświetlać powiadomienia dla określonych użytkowników, skorzystaj z samouczka [Azure Notification Hubs Notify Users] (Powiadamianie użytkowników przy użyciu usługi Azure Notification Hubs). 

Jeśli chcesz podzielić użytkowników na grupy zainteresowań, zobacz [Azure Notification Hubs breaking news] (Powiadamianie o najważniejszych wiadomościach przy użyciu usługi Azure Notification Hubs).

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[przykładu centrum powiadomień aplikacji dla programu Chrome]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Google Cloud Console]: http://cloud.google.com/console
[Notification Hubs Overview]: notification-hubs-push-notification-overview.md (Notification Hubs — omówienie)
[Chrome Apps Overview]: https://developer.chrome.com/apps/about_apps (Omówienie aplikacji dla programu Chrome)
[przykładzie usługi GCM dla aplikacji dla programu Chrome]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome Apps on Mobile]: https://developer.chrome.com/apps/chrome_apps_on_mobile (Aplikacja dla programu Chrome na urządzeniach przenośnych)
[Create Registration NH REST API]: http://msdn.microsoft.com/library/azure/dn223265.aspx (Interfejs API REST tworzenia rejestracji w usłudze Notification Hubs)
[bibliotekę crypto-js]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[usługa Google Cloud Messaging dla programu Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure Notification Hubs Notify Users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md (Powiadamianie użytkowników przy użyciu usługi Azure Notification Hubs)
[Azure Notification Hubs breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md (Powiadamianie o najważniejszych wiadomościach przy użyciu usługi Azure Notification Hubs)
