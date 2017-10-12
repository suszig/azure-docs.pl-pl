
### <a name="update-manifest-file-to-enable-notifications"></a>Aktualizacja pliku manifestu w celu włączenia powiadomień
Skopiuj poniższe zasoby obsługi komunikatów w aplikacji do pliku Manifest.xml między tagami `<application>` i `</application>`.

        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
        </activity>
        <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
            </intent-filter>
        </receiver>
        <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
            <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
            </intent-filter>
        </receiver>

### <a name="specify-an-icon-for-notifications"></a>Określanie ikony dla powiadomień
Wklej następujący fragment kodu XML do pliku Manifest.xml między tagami `<application>` i `</application>`.

        <meta-data android:name="engagement:reach:notification:icon" android:value="engagement_close"/>

Spowoduje to określenie ikony, która jest wyświetlana zarówno w powiadomieniach systemowych, jak i w powiadomieniach wewnętrznych aplikacji. Ta czynność jest opcjonalna w przypadku powiadomień wewnętrznych aplikacji, lecz obowiązkowa w przypadku powiadomień systemowych. System Android będzie odrzucać powiadomienia systemowe z nieprawidłowymi ikonami.

Pamiętaj, aby użyć ikony znajdującej się w jednym z folderów zawierających **obiekty drawable** (np. ``engagement_close.png``). Folder **mipmap** nie jest obsługiwany.

> [!NOTE]
> Nie należy używać ikony **uruchamiania**. Ma ona inną rozdzielczość i zazwyczaj znajduje się w folderach mipmap, które nie są obsługiwane.
> 
> 

W przypadku aplikacji rzeczywistych można użyć ikony odpowiedniej dla powiadomień zgodnie z [zaleceniami dotyczącymi projektowania w systemie Android](http://developer.android.com/design/patterns/notifications.html).

> [!TIP]
> Aby mieć pewność, że użyta rozdzielczość ikony jest poprawna, zapoznaj się z [tymi przykładami](https://www.google.com/design/icons).
> Przewiń w dół do sekcji **Notification** (Powiadomienie), kliknij ikonę, a następnie kliknij opcję `PNGS`, aby pobrać zestaw obiektów drawable dla ikon. Możesz sprawdzić, które foldery obiektów drawable i jakie rozdzielczości mogą być użyte w przypadku poszczególnych wersji ikony.
> 
> 

### <a name="enable-your-app-to-receive-gcm-push-notifications"></a>Włączanie funkcji odbierania powiadomień wypychanych usługi GCM w aplikacji
1. Wklej poniższą zawartość do pliku Manifest.xml między tagami `<application>` i `</application>` po zastąpieniu **identyfikatora nadawcy** uzyskanego z konsoli projektu Firebase. Tag \n jest zamierzony, dlatego upewnij się, że został umieszczony na końcu numeru projektu.
   
        <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
2. Wklej poniższy kod do pliku Manifest.xml między tagami `<application>` i `</application>`. Zastąp nazwę pakietu <Your package name>.
   
        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
        android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<Your package name>" />
            </intent-filter>
        </receiver>
3. Dodaj ostatni zestaw uprawnień, które są wyróżnione przed tagiem `<application>`. Zastąp ciąg `<Your package name>` rzeczywistą nazwą pakietu aplikacji.
   
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="<Your package name>.permission.C2D_MESSAGE" />
        <permission android:name="<Your package name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

