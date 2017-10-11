1. W Twojej **aplikacji** projekt, otwórz plik `AndroidManifest.xml`. W kodzie następne dwa kroki, Zastąp  *`**my_app_package**`*  z nazwą pakietu aplikacji dla projektu. Jest to wartość `package` atrybutu `manifest` znacznika.
2. Dodaj następujące nowe uprawnienia po istniejącej `uses-permission` elementu:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
3. Dodaj następujący kod po `application` tagu początkowego:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                         android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>
4. Otwórz plik *ToDoActivity.java*i dodaj następującą instrukcję import:

        import com.microsoft.windowsazure.notifications.NotificationsManager;
5. Dodaj następujące prywatne zmienną do klasy. Zastąp  *`<PROJECT_NUMBER>`*  numer projektu przypisany przez firmę Google do aplikacji w poprzedniej procedurze.

        public static final String SENDER_ID = "<PROJECT_NUMBER>";
6. Zmień definicję *MobileServiceClient* z **prywatnej** do **publiczne statyczne**, więc teraz wygląda następująco:

        public static MobileServiceClient mClient;
7. Dodaj nową klasę do obsługi powiadomień. Otwórz w Eksploratorze projektu **src** > **głównego** > **java** węzłów, a następnie kliknij prawym przyciskiem myszy węzeł nazwę pakietu. Kliknij przycisk **nowy**, a następnie kliknij przycisk **Klasa Java**.
8. W **nazwa**, typ `MyHandler`, a następnie kliknij przycisk **OK**.

    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)

9. W pliku MyHandler Zastąp deklaracji klasy z:

        public class MyHandler extends NotificationsHandler {
10. Dodaj następujące instrukcje importu dla `MyHandler` klasy:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
11. Następnie dodać ten element członkowski do `MyHandler` klasy:

        public static final int NOTIFICATION_ID = 1;
12. W `MyHandler` klasy, Dodaj następujący kod, aby zastąpić **onRegistered** metodę, która rejestruje urządzenie w Centrum powiadomień usługi mobilnej.

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
           super.onRegistered(context, gcmRegistrationId);

           new AsyncTask<Void, Void, Void>() {

               protected Void doInBackground(Void... params) {
                   try {
                       ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                       return null;
                   }
                   catch(Exception e) {
                       // handle error                
                   }
                   return null;              
               }
           }.execute();
       }
13. W `MyHandler` klasy, Dodaj następujący kod, aby zastąpić **zdarzenia onReceive** metodę, która powoduje, że powiadomienie, aby wyświetlić po odebraniu.

        @Override
        public void onReceive(Context context, Bundle bundle) {
               String msg = bundle.getString("message");

               PendingIntent contentIntent = PendingIntent.getActivity(context,
                       0, // requestCode
                       new Intent(context, ToDoActivity.class),
                       0); // flags

               Notification notification = new NotificationCompat.Builder(context)
                       .setSmallIcon(R.drawable.ic_launcher)
                       .setContentTitle("Notification Hub Demo")
                       .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                       .setContentText(msg)
                       .setContentIntent(contentIntent)
                       .build();

               NotificationManager notificationManager = (NotificationManager)
                       context.getSystemService(Context.NOTIFICATION_SERVICE);
               notificationManager.notify(NOTIFICATION_ID, notification);
       }
14. W pliku TodoActivity.java aktualizacji **onCreate** metody *ToDoActivity* klasy można zarejestrować klasy obsługi powiadomień. Upewnij się, że Dodaj ten kod po *MobileServiceClient* zostanie uruchomiony.

        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Aplikacja jest teraz zaktualizowana do obsługi powiadomień wypychanych.
