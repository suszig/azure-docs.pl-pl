
## <a name="create-an-application-express"></a>Tworzenie aplikacji (Express)
Teraz musisz zarejestrować aplikację w *portalu rejestracji aplikacji Microsoft*:
1. Zarejestrować aplikację za pośrednictwem [portalu rejestracji aplikacji firmy Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)
2.  Wprowadź nazwę aplikacji i poczty e-mail
3.  Upewnij się, że zaznaczono opcję instrukcje konfiguracji
4.  Postępuj zgodnie z instrukcjami, aby uzyskać identyfikator aplikacji i wklej go w kodzie

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Dodaj swoje informacje rejestracyjne aplikacji do rozwiązania (zaawansowane)
Teraz musisz zarejestrować aplikację w *portalu rejestracji aplikacji Microsoft*:
1. Przejdź do [portalu rejestracji aplikacji Microsoft](https://apps.dev.microsoft.com/portal/register-app) do rejestrowania aplikacji
2. Wprowadź nazwę aplikacji i poczty e-mail 
3. Upewnij się, że jest zaznaczona opcja instrukcje konfiguracji
4. Kliknij przycisk `Add Platform`, a następnie wybierz pozycję `Native Application` i kliknij przycisk Zapisz
5.  Otwórz `MainActivity` (w obszarze `app`  >  `java`  >   *`{host}.{namespace}`* )
6.  Zastąp *[aplikacji identyfikatora w tym miejscu wprowadź]* w wiersz rozpoczynający się `final static String CLIENT_ID` wraz z Identyfikatorem aplikacji został zarejestrowany:

```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Otwórz `AndroidManifest.xml` (w obszarze `app`  >  `manifests`) dodaj następujące działanie `manifest\application` węzła. Rejestruje to `BrowserTabActivity` umożliwia systemu operacyjnego można wznowić aplikacji po zakończeniu uwierzytelniania:
</li>
</ol>

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
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
<ol start="8">
<li>
W `BrowserTabActivity`, Zastąp `[Enter the application Id here]` z identyfikatorem aplikacji.
</li>
</ol>
