<properties
    pageTitle="Rozpoczynanie pracy z usługą Azure Notification Hubs przy użyciu usługi Baidu | Microsoft Azure"
    description="Korzystając z tego samouczka, dowiesz się, jak za pomocą usługi Azure Notification Hubs wysyłać powiadomienia wypychane do urządzeń z systemem Android przy użyciu usługi Baidu."
    services="notification-hubs"
    documentationCenter="android"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="mobile-baidu"
    ms.workload="mobile"
    ms.date="08/19/2016"
    ms.author="wesmc"/>

# Rozpoczynanie pracy z usługą Azure Notification Hubs przy użyciu usługi Baidu

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##Omówienie

Powiadomienia wypychane w chmurze Baidu to chińska usługa w chmurze służąca do wysyłania powiadomień wypychanych do urządzeń przenośnych. Ta usługa jest szczególnie przydatna w Chinach, gdzie dostarczanie powiadomień wypychanych do urządzeń z systemem Android jest złożone z powodu istnienia różnych sklepów z aplikacjami i usług powiadomień wypychanych oraz dostępności urządzeń z systemem Android, które zwykle nie są podłączone do usługi GCM (Google Cloud Messaging).

##Wymagania wstępne

Dla tego samouczka wymagane są następujące elementy:

+ Zestaw SDK systemu Android (założono, że używany jest program Eclipse), który można pobrać z <a href="http://go.microsoft.com/fwlink/?LinkId=389797">witryny systemu Android</a>
+ [Zestaw SDK usługi Mobile Services dla systemu Android]
+ [zestaw SDK systemu Android dla powiadomień wypychanych w usłudze Baidu]

>[AZURE.NOTE] Do wykonania kroków tego samouczka potrzebne jest aktywne konto platformy Azure. Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).


##Tworzenia konta usługi Baidu

Aby korzystać z usługi Baidu, musisz mieć konto Baidu. Jeśli masz już konto, zaloguj się do [portalu Baidu] i przejdź do następnego kroku. W przeciwnym razie zapoznaj się z poniższymi instrukcjami dotyczącymi sposobu tworzenia konta usługi Baidu.  

1. Przejdź do [portalu Baidu] i kliknij link **登录** (**Zaloguj się**). Kliknij pozycję **立即注册**, aby rozpocząć proces rejestracji konta.

    ![][1]

2. Wprowadź wymagane szczegóły (telefon lub adres e-mail, hasło i kod weryfikacyjny), a następnie kliknij przycisk **Zarejestruj się**.

    ![][2]

3. Na podany adres e-mail zostanie wysłana wiadomość e-mail zawierająca link umożliwiający aktywację konta usługi Baidu.

    ![][3]

4. Zaloguj się do konta e-mail, otwórz aktywacyjną wiadomość e-mail usługi Baidu, a następnie kliknij link aktywacyjny, aby aktywować konto usługi Baidu.

    ![][4]

Po aktywowaniu konta usługi Baidu zaloguj się do [portalu Baidu].

##Rejestrowanie się jako deweloper usługi Baidu

1. Po zalogowaniu się do [portalu Baidu] kliknij pozycję **更多>>** (**więcej**).

    ![][5]

2. Przewiń w dół w sekcji **站长与开发者服务** (Usługi dla webmasterów i deweloperów) i kliknij pozycję **百度开放云平台** (**Otwarta platforma chmury Baidu**).

    ![][6]

3. Na następnej stronie kliknij pozycję **开发者服务** (**Usługi dla deweloperów**) w prawym górnym rogu.

    ![][7]

4. Na następnej stronie kliknij pozycję **注册开发者** (**Zarejestrowani deweloperzy**) w menu w prawym górnym rogu.

    ![][8]

5. Wprowadź imię i nazwisko, opis oraz numer telefonu komórkowego do odebrania weryfikacyjnej wiadomości tekstowej, a następnie kliknij pozycję **送验证码** (**Wyślij kod weryfikacyjny**). Pamiętaj, że w przypadku międzynarodowych numerów telefonów należy dołączyć kod kraju w nawiasie. Na przykład numer dla Stanów Zjednoczonych będzie mieć postać **(1)1234567890**.

    ![][9]

6. Otrzymasz wiadomość tekstową zawierającą numer weryfikacyjny, jak przedstawiono w poniższym przykładzie:

    ![][10]

7. Wprowadź numer weryfikacyjny z wiadomości w polu **验证码** (**Kod potwierdzenia**).

8. Następnie ukończ rejestrację jako deweloper, akceptując umowę usługi Baidu i klikając przycisk **提交** (**Prześlij**). Po pomyślnym ukończeniu rejestracji zostanie wyświetlona następująca strona:

    ![][11]

##Tworzenie projektu powiadomień wypychanych w chmurze Baidu

Podczas tworzenia projektu powiadomień wypychanych w chmurze Baidu otrzymasz identyfikator aplikacji, klucz interfejsu API i klucz tajny.

1. Po zalogowaniu się do [portalu Baidu] kliknij pozycję **更多>>** (**więcej**).

    ![][5]

2. Przewiń w dół w sekcji **站长与开发者服务** (**Usługi dla webmasterów i deweloperów**) i kliknij pozycję **百度开放云平台** (**Otwarta platforma chmury Baidu**).

    ![][6]

3. Na następnej stronie kliknij pozycję **开发者服务** (**Usługi dla deweloperów**) w prawym górnym rogu.

    ![][7]

4. Na następnej stronie kliknij pozycję **云推送** (**Powiadomienia wypychane w chmurze**) w sekcji **云服务** (**Usługi w chmurze**).

    ![][12]

5. Po zarejestrowaniu jako deweloper w menu u góry zostanie wyświetlona pozycja **管理控制台** (**Konsola zarządzania**). Kliknij pozycję **开发者服务管理** (**Zarządzanie usługami dla deweloperów**).

    ![][13]

6. Na następnej stronie kliknij pozycję **创建工程** (**Utwórz projekt**).

    ![][14]

7. Wprowadź nazwę aplikacji, a następnie kliknij pozycję **创建** (**Utwórz**).

    ![][15]

8. Po pomyślnym utworzeniu projektu powiadomień wypychanych w chmurze Baidu zostanie wyświetlona strona zawierająca następujące dane: **AppID** (Identyfikator aplikacji), **API Key** (Klucz interfejsu API) i **Secret Key** (Klucz tajny). Zanotuj klucz interfejsu API i klucz tajny. Użyjemy ich później.

    ![][16]

9. Skonfiguruj projekt dla powiadomień wypychanych, klikając pozycję **云推送** (**Powiadomienia wypychane w chmurze**) w okienku po lewej stronie.

    ![][31]

10. Na następnej stronie kliknij przycisk **推送设置** (**Ustawienia powiadomień wypychanych**).

    ![][32]  

11. Na stronie konfiguracji dodaj nazwę pakietu, której będziesz używać w projekcie dla systemu Android w polu **应用包名** (**Pakiet aplikacji**), a następnie kliknij przycisk **保存设置** (**Zapisz**).  

    ![][33]

Zostanie wyświetlony komunikat **保存成功！** (**Zapisano pomyślnie!**).

##Konfigurowanie centrum powiadomień

1. Zaloguj się do [klasycznego portalu Azure], a następnie kliknij przycisk **+NOWY** u dołu ekranu.

2. Kliknij pozycję **App Services**, pozycję **Service Bus** i pozycję **Centrum powiadomień**, a następnie kliknij pozycję **Szybkie tworzenie**.

3. Podaj nazwę w polu **Centrum powiadomień**, wybierz odpowiednie opcje w polach **Region** i **Przestrzeń nazw**, aby określić, gdzie zostanie utworzone centrum powiadomień, a następnie kliknij pozycję **Utwórz nowe centrum powiadomień**.  

    ![][17]

4. Kliknij przestrzeń nazw, w której utworzono centrum powiadomień, a następnie kliknij pozycję **Centra powiadomień** u góry.

    ![][18]

5. Wybierz utworzone centrum powiadomień, a następnie kliknij pozycję **Konfiguruj** w menu u góry.

    ![][19]

6. Przewiń w dół do sekcji **Ustawienia powiadomień Baidu** i wprowadź klucz interfejsu API oraz klucz tajny uzyskany wcześniej w konsoli Baidu dla projektu powiadomień wypychanych w chmurze Baidu. Kliknij pozycję **Zapisz**.

    ![][20]

7. Kliknij kartę **Pulpit nawigacyjny** u góry dla centrum powiadomień, a następnie kliknij pozycję **Wyświetl parametry połączenia**.

    ![][21]

8. Zanotuj wartości **DefaultListenSharedAccessSignature** i **DefaultFullSharedAccessSignature** w oknie **Dostęp do informacji o połączeniu**.

    ![][22]

##Łączenie aplikacji z centrum powiadomień

1. W środowisku Eclipse ADT utwórz nowy projekt dla systemu Android: **File** > **New** > **Android Application Project** (Plik — Nowy — Projekt aplikacji dla systemu Android).

    ![][23]

2. Wprowadź wartość w polu **Application Name** (Nazwa aplikacji) i upewnij się, że w polu **Minimum Required SDK** (Minimalna wymagana wersja zestawu SDK) jest ustawiona wersja **API 16: Android 4.1**.

    ![][24]

3. Kliknij przycisk **Next** (Dalej) i postępuj zgodnie z instrukcjami w kreatorze do momentu wyświetlenia okna **Create Activity** (Tworzenie działania). Upewnij się, że wybrano pozycję **Blank Activity** (Puste działanie), a następnie wybierz przycisk **Finish** (Zakończ), aby utworzyć nową aplikację dla systemu Android.

    ![][25]

4. Upewnij się, że w polu **Project Build Target** (Docelowa kompilacja projektu) określono prawidłową wartość.

    ![][26]

5. Pobierz plik notification-hubs-0.4.jar dostępny na karcie **Files** (Pliki) w projekcie [Notification-Hubs-Android-SDK w serwisie Bintray](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4). Dodaj plik do folderu **libs** w projekcie Eclipse i odśwież folder *libs*.

6. Pobierz i rozpakuj [zestaw SDK systemu Android dla powiadomień wypychanych w usłudze Baidu], otwórz folder **libs**, a następnie skopiuj plik jar **pushservice-x.y.z** oraz foldery **armeabi** & **mips** do folderu **libs** aplikacji dla systemu Android.

7. Otwórz plik **AndroidManifest.xml** projektu dla systemu Android i dodaj uprawnienia wymagane przez zestaw SDK usługi Baidu.

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />

8. Dodaj właściwość **android:name** do elementu **application** w pliku **AndroidManifest.xml**, zastępując ciąg *yourprojectname* odpowiednią wartością (np. **com.przyklad.BaiduTest**). Upewnij się, że ta nazwa projektu jest zgodna z nazwą skonfigurowaną w konsoli Baidu.

        <application android:name="yourprojectname.DemoApplication"

9. Dodaj następującą konfigurację w elemencie aplikacji po elemencie działania **.MainActivity**, zastępując ciąg *yourprojectname* odpowiednią wartością (np. **com.przyklad.BaiduTest**):

        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

9. Dodaj nową klasę o nazwie **ConfigurationSettings.java** do projektu.

    ![][28]

    ![][29]

10. Dodaj do niej następujący kod:

        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }

    Dla elementu **API_KEY** ustaw wartość uzyskaną wcześniej z projektu w chmurze Baidu, ustaw nazwę centrum powiadomień z portalu klasycznego Azure dla elementu **NotificationHubName** oraz ustaw wartość DefaultListenSharedAccessSignature z portalu klasycznego Azure dla elementu **NotificationHubConnectionString**.

11. Dodaj nową klasę o nazwie **DemoApplication.java** i dodaj do niej następujący kod:

        import com.baidu.frontia.FrontiaApplication;

        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }

12. Dodaj nową klasę o nazwie **MyPushMessageReceiver.java** i dodaj do niej poniższy kod. To jest klasa, która obsługuje powiadomienia wypychane odbierane z serwera powiadomień wypychanych Baidu.

        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG to Log */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();

            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;

                try {
                 if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }

                registerWithNotificationHubs();
            }

            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }

            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }

            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }

            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }

13. Otwórz plik **MainActivity.java** i dodaj następujący kod do metody **onCreate**:

            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);

14. Otwórz następujące instrukcje import u góry:

            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

##Wysyłanie powiadomień do aplikacji


Odbieranie powiadomień w aplikacji możesz szybko przetestować, wysyłając powiadomienia w witrynie [Azure Portal](https://portal.azure.com/) za pomocą przycisku **Testuj wysyłanie** w centrum powiadomień, jak pokazano na poniższym zrzucie ekranu.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Powiadomienia wypychane są zwykle wysyłane za pośrednictwem usługi zaplecza, takiej jak Mobile Services czy ASP.NET, z użyciem zgodnej biblioteki. Powiadomienia można również wysyłać bezpośrednio za pomocą interfejsu API REST, jeśli biblioteka nie jest dostępna w danym zapleczu.

W tym samouczku dla uproszczenia przedstawiono testowanie aplikacji klienckiej przez wysyłanie powiadomień za pomocą zestawu SDK .NET dla usługi Notification Hubs w aplikacji konsoli, a nie za pomocą usługi zaplecza. Zalecanym następnym krokiem jest zapoznanie się z samouczkiem [Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) dotyczącym wysyłania powiadomień przy użyciu zaplecza ASP.NET. Można używać następujących metod wysyłania powiadomień:

* **Interfejs REST**: powiadomienia mogą być obsługiwane na dowolnej platformie zaplecza za pomocą [interfejsu REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Zestaw SDK .NET dla usługi Microsoft Azure Notification Hubs**: w menedżerze pakietów NuGet dla programu Visual Studio uruchom polecenie [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js**: [Jak używać usługi Notification Hubs z poziomu środowiska Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

* **Azure Mobile Services**: aby zapoznać się z przykładem wysyłania powiadomień z poziomu zaplecza usługi Azure Mobile Services zintegrowanego z usługą Notification Hubs, zobacz [Add push notifications to your Mobile Services app](../mobile-services/mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md) (Dodawanie powiadomień wypychanych do aplikacji Mobile Services).

* **Java / PHP**: aby zapoznać się z przykładem wysyłania powiadomień przy użyciu interfejsów API REST, zobacz „How to use Notification Hubs from Java/PHP” (Jak używać usługi Notification Hubs za pomocą języka Java/PHP) — [Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md).

##(Opcjonalnie) Wysyłanie powiadomień z poziomu aplikacji konsolowej .NET

W tej sekcji przedstawiono sposób wysyłania powiadomienia za pomocą aplikacji konsolowej .NET.

1. Utwórz nową aplikację konsoli języka Visual C#:

    ![][30]

2. W oknie Konsola menedżera pakietów ustaw nowy projekt aplikacji konsoli jako **Projekt domyślny**, a następnie w oknie konsoli uruchom następujące polecenie:

        Install-Package Microsoft.Azure.NotificationHubs

    Spowoduje to dodanie odwołania do zestawu SDK usługi Azure Notification Hubs z użyciem <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">pakietu NuGet Microsoft.Azure.Notification Hubs</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

3. Otwórz plik **Program.cs** i dodaj następującą instrukcję using:

        using Microsoft.Azure.NotificationHubs;

4. W Twojej klasie `Program` dodaj następującą metodę i zastąp elementy *DefaultFullSharedAccessSignatureSASConnectionString* i *NotificationHubName* uzyskanymi wartościami.

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }

5. Dodaj następujące wiersze do metody **Main**:

         SendNotificationAsync();
         Console.ReadLine();

##Testowanie aplikacji

Aby przetestować tę aplikację przy użyciu rzeczywistego telefonu, podłącz telefon do komputera za pomocą kabla USB. Spowoduje to załadowanie aplikacji do podłączonego telefonu.

Aby przetestować tę aplikację przy użyciu emulatora, na górnym pasku narzędzi Eclipse kliknij pozycję **Run** (Uruchom), a następnie wybierz aplikację. Spowoduje to uruchomienie emulatora, a następnie załadowanie i uruchomienie aplikacji.

Aplikacja pobiera wartości parametrów „userId” i „channelId” z usługi powiadomień wypychanych Baidu i wykonuje rejestrację w centrum powiadomień.

Testowe powiadomienie można wysłać przy użyciu karty debugowania klasycznego portalu Azure. Jeśli utworzono aplikację konsolową .NET dla programu Visual Studio, naciśnij klawisz F5 w programie Visual Studio, aby uruchomić aplikację. Aplikacja wyśle powiadomienie, które zostanie wyświetlone w górnym obszarze powiadomień urządzenia lub emulatora.


<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Zestaw SDK usługi Mobile Services dla systemu Android]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Zestaw SDK systemu Android dla powiadomień wypychanych w usłudze Baidu]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[klasycznego portalu Azure]: https://manage.windowsazure.com/
[portalu Baidu]: http://www.baidu.com/



<!--HONumber=sep16_HO1-->


