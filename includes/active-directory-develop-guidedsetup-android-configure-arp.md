
## <a name="add-the-applications-registration-information-to-your-app"></a>Dodawanie informacji o rejestracji aplikacji do aplikacji

W tym kroku musisz dodać identyfikator klienta do projektu.

1.  Otwórz `MainActivity` (w obszarze `app`  >  `java`  >   *`{host}.{namespace}`* )
2.  Zastąp wiersz rozpoczynający się `final static String CLIENT_ID` z:
```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
3. Otwórz:`app` > `manifests` > `AndroidManifest.xml`
4. Dodaj na wykonywanie następujących czynności `manifest\application` węzła. Rejestr `BrowserTabActivity` umożliwia systemu operacyjnego można wznowić aplikacji po zakończeniu uwierzytelniania:

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

### <a name="what-is-next"></a>Co to jest dalej

[Testowanie i weryfikowanie](active-directory-develop-guidedsetup-android-test.md)
