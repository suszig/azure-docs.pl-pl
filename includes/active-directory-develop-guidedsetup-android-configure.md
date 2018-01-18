
## <a name="register-your-application"></a>Rejestrowanie aplikacji
Aplikację można zarejestrować na dwa sposoby, zgodnie z opisem w dwóch następnych sekcjach.

### <a name="option-1-express-mode"></a>Opcja 1: Tryb ekspresowy
Można szybko zarejestrować aplikację w następujący sposób:
1. Przejdź do [portalu rejestracji aplikacji Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure).
2.  W **Nazwa aplikacji** wprowadź nazwę aplikacji.

3. Upewnij się, że **instrukcje konfiguracji** pole wyboru jest wybrany, a następnie wybierz **Utwórz**.

4. Postępuj zgodnie z instrukcjami w celu uzyskania Identyfikatora aplikacji i wklej go w kodzie.

### <a name="option-2-advanced-mode"></a>Opcja 2: Tryb zaawansowany
Aby zarejestrować aplikację i dodać swoje informacje rejestracyjne aplikacji do rozwiązania, wykonaj następujące czynności:
1. Jeśli nie zostało już zarejestrowane aplikacji, przejdź do [portalu rejestracji aplikacji Microsoft](https://apps.dev.microsoft.com/portal/register-app).
2. W **Nazwa aplikacji** wprowadź nazwę aplikacji. 

3. Upewnij się, że **instrukcje konfiguracji** pole wyboru jest wyczyszczone, a następnie wybierz **Utwórz**.

4. Wybierz **dodać platformy**, wybierz pozycję **aplikacji natywnej**, a następnie wybierz **zapisać**.

5. W obszarze **aplikacji** > **java** > **{hosta}. { przestrzeń nazw}**, otwórz `MainActivity`. 

6.  Zastąp *[aplikacji identyfikatora w tym miejscu wprowadź]* w następującym wierszu z Identyfikatorem aplikacji, który został zarejestrowany:

    ```java
    final static String CLIENT_ID = "[Enter the application Id here]";
    ```
<!-- Workaround for Docs conversion bug -->
7. W obszarze **aplikacji** > **manifesty**, otwórz *AndroidManifest.xml* pliku.

8. W `manifest\application` węzła, Dodaj następujące działania. Sposób tak rejestrów `BrowserTabActivity` działanie, które umożliwia systemu operacyjnego można wznowić aplikacji po jej ukończeniu uwierzytelniania:

    ```xml
    <!--Intent filter to capture System Browser calling back to our app after sign-in-->
    <activity
        android:name="com.microsoft.identity.client.BrowserTabActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            
            <!--Add in your scheme/host from registered redirect URI-->
            <!--By default, the scheme should be similar to 'msal[appId]' -->
            <data android:scheme="msal[Enter the application Id here]"
                android:host="auth" />
        </intent-filter>
    </activity>
    ```
<!-- Workaround for Docs conversion bug -->
9. W `BrowserTabActivity` węzła, Zastąp `[Enter the application Id here]` z identyfikatorem aplikacji.
