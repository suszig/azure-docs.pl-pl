---
title: Azure AD systemu Android wprowadzenie | Dokumentacja firmy Microsoft
description: "Sposób tworzenia aplikacji systemu Android, która integruje się z usługą Azure AD, logowania i wywołania usługi Azure AD chronione przy użyciu OAuth2.0 interfejsów API."
services: active-directory
documentationcenter: android
author: danieldobalian
manager: mtillman
editor: 
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 11/30/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 6c949c07c95c5d965b31725bce4ab2deac646bca
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="azure-ad-android-getting-started"></a>Azure AD systemu Android wprowadzenie
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Jeśli projektujesz klasycznej aplikacji usługi Azure Active Directory (Azure AD) umożliwia proste i bezpośrednie umożliwia uwierzytelnianie użytkowników przy użyciu ich lokalnych kont usługi Active Directory. Umożliwia również aplikacji bezpiecznego używać dowolnego składnika web API chronione przez usługę Azure AD, takie jak interfejsami API usługi Office 365 lub interfejsu API platformy Azure.

W przypadku systemu Android klientów, którzy potrzebują dostępu do chronionych zasobów usługi Azure AD zapewnia Active Directory Authentication Library (ADAL). Wyłącznie do celów ADAL jest umożliwia łatwe uzyskanie tokenów dostępu do aplikacji. Aby zademonstrować, jak łatwo, firma Microsoft będzie kompilacji aplikacji systemu Android listy zadań do wykonania który:

* Pobiera dostępu tokeny na potrzeby wywoływania API listy zadań do wykonania przy użyciu [protokół uwierzytelniania OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Pobiera listę zadań do wykonania przez użytkownika.
* Wylogowuje użytkowników.

Aby rozpocząć pracę, należy dzierżawa usługi Azure AD można tworzyć użytkowników i zarejestrowanie aplikacji. Jeśli nie masz już dzierżawę, [Dowiedz się, jak kupić](active-directory-howto-tenant.md).

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Krok 1: Pobierz i uruchom serwer próbki TODO interfejsu API REST Node.js
Przykładowe TODO interfejsu API REST Node.js są zapisywane w szczególności pracy projektowej naszej próbki istniejących tworzenia pojedynczej dzierżawy interfejsu API REST zadań do wykonania dla usługi Azure AD. Jest to wymagane w przypadku Szybki Start.

Aby uzyskać informacje na temat tej konfiguracji, zobacz nasze przykłady istniejących w [Microsoft usługi Azure Active Directory próbki REST API for Node.js](active-directory-devquickstarts-webapi-nodejs.md).


## <a name="step-2-register-your-web-api-with-your-azure-ad-tenant"></a>Krok 2: Zarejestruj interfejsu API sieci web z dzierżawy usługi Azure AD
Usługa Active Directory obsługuje dodawanie dwa typy aplikacji:

- Interfejsy API, które oferują usługi dla użytkowników sieci Web
- Aplikacje (z systemem w sieci web lub na urządzeniu), które uzyskują dostęp do tych interfejsów API sieci web

W tym kroku jest zarejestrowanie sieci web interfejsu API, której używasz lokalnie do testowania w tym przykładzie. Zazwyczaj interfejs API sieci web to usługa REST, która oferuje funkcje, które mają dostęp do aplikacji. Usługi Azure AD może pomóc chronić dowolnego punktu końcowego.

Zakłada się, że w przypadku rejestrowania interfejsu API REST TODO odwołuje się do wcześniej. Ale działa to w przypadku dowolnej sieci web interfejsu API, który ma usługi Azure Active Directory w celu ochrony.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Na górnym pasku kliknij konto. W **katalogu** wybierz dzierżawy usługi Azure AD, które chcesz zarejestrować aplikację.
3. Kliknij przycisk **wszystkie usługi** w okienku po lewej stronie, a następnie wybierz **usługi Azure Active Directory**.
4. Kliknij przycisk **rejestracji aplikacji**, a następnie wybierz **Dodaj**.
5. Wprowadź przyjazną nazwę dla aplikacji (na przykład **TodoListService**), wybierz pozycję **aplikacji sieci Web i/lub interfejs API sieci Web**i kliknij przycisk **dalej**.
6. Zaloguj się na adres URL wprowadź podstawowy adres URL przykładowej. Domyślnie jest to `https://localhost:8080`.
7. Kliknij przycisk **OK** do ukończenia rejestracji.
8. Nadal w portalu Azure, przejdź do strony aplikacji, Znajdź wartość Identyfikatora aplikacji i skopiuj go. Należy to później podczas konfigurowania aplikacji.
9. Z **ustawienia** -> **właściwości** strony, aktualizacja identyfikator URI — wprowadź `https://<your_tenant_name>/TodoListService`. Zastąp `<your_tenant_name>` o nazwie dzierżawy usługi Azure AD.

## <a name="step-3-register-the-sample-android-native-client-application"></a>Krok 3: Zarejestruj przykładowej aplikacji systemu Android Native Client
W tym przykładzie należy zarejestrować aplikacji sieci web. Dzięki temu aplikacja do komunikacji z zarejestrowana tylko interfejs API sieci web. Usługi Azure AD odmówi nawet Zezwalaj aplikacji na poproś dla logowania, chyba że jest on zarejestrowany. Który jest częścią modelu zabezpieczeń.

Zakłada się, że w przypadku rejestrowania przykładowej aplikacji odwołuje się do wcześniej. Jednak ta procedura działa w przypadku dowolnej aplikacji, która projektujesz.

> [!NOTE]
> Może zastanawiasz się, dlaczego chcesz umieścić zarówno aplikacji, jak i interfejs API sieci web w jednej dzierżawy. Ponieważ użytkownik może mieć odgadnąć, można utworzyć aplikację, która uzyskuje dostęp do zewnętrznego interfejsu API, który jest zarejestrowany w usłudze Azure AD z innego dzierżawcę. Jeśli to zrobisz, który, klienci wyświetli monit o zgodę na korzystanie z interfejsu API w aplikacji. Biblioteki uwierzytelniania usługi Active Directory dla systemu iOS zapewnia obsługę tej zgody. Jak możemy zapoznać się z bardziej zaawansowane funkcje, zobaczysz, że jest ważnym elementem pracy wymagane do uzyskiwania dostępu do pakietu Microsoft APIs z Azure i pakietu Office, a także innych dostawcy usług. Teraz ponieważ w zarejestrowany zarówno interfejs API sieci web i aplikacji w ramach tej samej dzierżawy, nie zobaczysz żadnych monitów o zgodę. To jest zwykle w przypadku tworzenia aplikacji, po prostu przez firmę do użycia.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Na górnym pasku kliknij konto. W **katalogu** wybierz dzierżawy usługi Azure AD, które chcesz zarejestrować aplikację.
3. Kliknij przycisk **wszystkie usługi** w okienku po lewej stronie, a następnie wybierz **usługi Azure Active Directory**.
4. Kliknij przycisk **rejestracji aplikacji**, a następnie wybierz **Dodaj**.
5. Wprowadź przyjazną nazwę dla aplikacji (na przykład **TodoListClient Android**), wybierz pozycję **natywną aplikację kliencką**i kliknij przycisk **dalej**.
6. Identyfikator URI przekierowania, można wprowadzić `http://TodoListClient`. Kliknij przycisk **Zakończ**.
7. Na stronie aplikacji Znajdź wartość Identyfikatora aplikacji i skopiuj go. Należy to później podczas konfigurowania aplikacji.
8. Z **ustawienia** wybierz pozycję **wymagane uprawnienia** i wybierz **Dodaj**.  Zlokalizuj i wybierz TodoListService, dodać **TodoListService dostępu** uprawnienie w obszarze **delegowane uprawnienia**i kliknij przycisk **gotowe**.

Tworzenia za pomocą narzędzia Maven, można użyć pom.xml na najwyższym poziomie:

1. Klonuj to repozytorium w wybranym katalogu:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  
2. Postępuj zgodnie z instrukcjami [wymagania wstępne dotyczące konfigurowania środowiska Maven dla systemu Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).
3. Konfigurowanie emulatora z 19 zestawu SDK.
4. Przejdź do folderu głównego, w którym sklonować repozytorium.
5. Uruchom następujące polecenie: `mvn clean install`
6. Zmień katalog na przykład Szybki Start: `cd samples\hello`
7. Uruchom następujące polecenie: `mvn android:deploy android:run`

   Powinna zostać wyświetlona uruchamiania aplikacji.
8. Wprowadź poświadczenia użytkownika testowego próby.

Pakiety JAR zostaną przesłane obok pakietu AAR.

## <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Krok 4: Pobierz ADAL dla systemu Android i dodaj go do swojego obszaru roboczego programu Eclipse
Wprowadziliśmy go łatwo jest wiele sposobów używania biblioteki ADAL w projekcie systemu Android:

* Kod źródłowy służy do importowania tej biblioteki do Eclipse i łącza do aplikacji.
* Jeśli używasz programu Android Studio, można użyć formatu pakietu AAR i odwoływać pliki binarne.

### <a name="option-1-source-zip"></a>Opcja 1: Źródło Zip
Aby pobrać kopię kodu źródłowego, kliknij przycisk **Pobierz ZIP** w prawej części strony. Można także [pobrać z witryny GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

### <a name="option-2-source-via-git"></a>Opcja 2: Źródło za pomocą narzędzia Git
Aby uzyskać kod źródłowy zestawu SDK przez Git, wpisz:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

### <a name="option-3-binaries-via-gradle"></a>Opcja 3: Pliki binarne, za pomocą narzędzia Gradle
Pliki binarne można uzyskać z centralnym repozytorium Maven. Pakiet AAR mogą zostać włączone w następujący sposób w projekcie w programie Android Studio:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

### <a name="option-4-aar-via-maven"></a>Opcja 4: AAR za pomocą narzędzia Maven
Jeśli używasz wtyczek M2Eclipse zależności można określić w pliku pom.xml:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


### <a name="option-5-jar-package-inside-the-libs-folder"></a>Opcja 5: Pakiet JAR w folderze libs
Możesz pobrać plik JAR z repozytorium Maven i upuść ją na **biblioteki** folder w projekcie. Musisz skopiować wymagane zasoby do projektu również, ponieważ pakiety JAR nie zawierają je.

## <a name="step-5-add-references-to-android-adal-to-your-project"></a>Krok 5: Dodaj odwołania do ADAL dla systemu Android do projektu
1. Dodaj odwołanie do projektu i określ go jako biblioteka systemu Android. Jeśli masz pewności, jak to zrobić, więcej informacji można znaleźć [lokacji Android Studio](http://developer.android.com/tools/projects/projects-eclipse.html).
2. Dodaj zależności projektu do debugowania do ustawień projektu.
3. Aktualizowanie pliku AndroidManifest.xml projektu, aby uwzględnić:

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
            ....
        <application/>

4. Utwórz wystąpienie kontekst AuthenticationContext na Twoje działania głównego. Szczegóły tego wywołania wykraczają poza zakres tego tematu, ale można uzyskać dobry początek, analizując [próbki Android Native Client](https://github.com/AzureADSamples/NativeClient-Android). W poniższym przykładzie SharedPreferences jest domyślne pamięci podręcznej, a Urząd jest w formie `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:

    `mContext = new AuthenticationContext(MainActivity.this, authority, true); // mContext is a field in your activity`

5. Skopiuj ten blok kodu do obsługi koniec AuthenticationActivity po użytkownik wprowadza poświadczenia i odbiera kod autoryzacji:

        @Override
         protected void onActivityResult(int requestCode, int resultCode, Intent data) {
             super.onActivityResult(requestCode, resultCode, data);
             if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
             }
         }

6. Aby uzyskać token, zdefiniować wywołanie zwrotne:

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };

7. Na koniec uzyskać tokenu przy użyciu wywołanie zwrotne:

    `mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                   callback);`

Oto objaśnienie parametrów:

* *zasób* jest wymagany i próbujesz uzyskać dostęp do zasobu.
* *ClientID* jest wymagany i pochodzi z usługi Azure AD.
* *RedirectUri* nie jest wymagana dla wywołania acquireToken. Można skonfigurować go jako nazwę pakietu.
* *PromptBehavior* pozwala pytanie o poświadczenia pominąć pamięci podręcznej i plik cookie.
* *Wywołanie zwrotne* jest wywoływana po kod autoryzacji są wymieniane dla tokenu. Ma ona obiektu AuthenticationResult, który ma token dostępu, Data wygasła i identyfikator informacji o tokenie.
* *acquireTokenSilent* jest opcjonalna. Można go wywołać do obsługi pamięci podręcznej i token odświeżania. Umożliwia także wersję usługi synchronizacji. Akceptuje *userId* jako parametr.

        mContext.acquireTokenSilent(resource, clientid, userId, callback );

Przy użyciu tego przewodnika, powinien mieć, co jest potrzebne do pomyślnie integracji z usługą Azure Active Directory. Więcej przykładów dotyczących tej pracy, odwiedź stronę AzureADSamples / repozytorium w witrynie GitHub.

## <a name="important-information"></a>Ważne informacje
### <a name="customization"></a>Dostosowywanie
Zasoby aplikacji można zastąpić zasoby biblioteki projektu. Dzieje się tak, gdy aplikacja została skompilowana. Z tego powodu można dostosować sposób, który ma układ działania uwierzytelniania. Pamiętaj zachować identyfikator kontrolki używany ADAL (WebView).

### <a name="broker"></a>Broker
Aplikacja Portal firmy usługi Microsoft Intune zawiera składnik brokera. Konto jest tworzone w elementu AccountManager. Typ konta jest "com.microsoft.workaccount." Elementu AccountManager umożliwia tylko jedno konto logowania jednokrotnego. Tworzy plik cookie logowania jednokrotnego dla użytkownika po zakończeniu wyzwanie urządzenia dla jednej z aplikacji.

Konta brokera, jeśli jedno konto użytkownika jest tworzona w to wystawca uwierzytelnienia i można zrezygnować z Pomiń je używa biblioteki ADAL. Można pominąć brokera użytkownikowi:

   `AuthenticationSettings.Instance.setSkipBroker(true);`

Należy zarejestrować specjalne RedirectUri użycia brokera. RedirectUri jest w formacie `msauth://packagename/Base64UrlencodedSignature`. Możesz też uzyskać Twojej RedirectUri aplikacji przy użyciu skryptu brokerRedirectPrint.ps1 lub mContext.getBrokerRedirectUri wywołania interfejsu API. Podpis jest powiązany z podpisywania certyfikatów.

Bieżący model brokera jest jeden użytkownik. Element AuthenticationContext udostępnia metodę interfejsu API można pobrać użytkownika brokera.

   `String brokerAccount =  mContext.getBrokerUser(); //Broker user is returned if account is valid.`

Manifest aplikacji powinien mieć następujące uprawnienia do korzystania z kont elementu AccountManager. Aby uzyskać więcej informacji, zobacz [elementu AccountManager informacji na temat witryny systemu Android](http://developer.android.com/reference/android/accounts/AccountManager.html).

* GET_ACCOUNTS
* USE_CREDENTIALS
* MANAGE_ACCOUNTS

### <a name="authority-url-and-ad-fs"></a>Adres URL urzędu i usługami AD FS
Active Directory Federation Services (AD FS) nie został rozpoznany jako produkcji STS, więc należy włączyć odnajdywania instancji i przekazać wartość false w Konstruktorze kontekst AuthenticationContext.

Adres URL urzędu wymaga wystąpienia usługi STS i [nazwa dzierżawcy](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).

### <a name="querying-cache-items"></a>Wykonywanie zapytania o elementy pamięci podręcznej
Biblioteka ADAL udostępnia pamięć podręczną domyślne w SharedPreferences z niektórych prostych pamięci podręcznej funkcje zapytań. Możesz uzyskać bieżącej pamięci podręcznej z kontekst AuthenticationContext przy użyciu:

    ITokenCacheStore cache = mContext.getCache();

Można też podać implementacji pamięci podręcznej, jeśli chcesz dostosować.

    mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);

### <a name="prompt-behavior"></a>Zachowanie monitu
ADAL udostępnia opcję, aby określić zachowanie monitu. PromptBehavior.Auto wyświetli interfejsu użytkownika, jeśli token odświeżania jest nieprawidłowy i wymagane są poświadczenia użytkownika. PromptBehavior.Always pominie użycia pamięci podręcznej i zawsze pokazuj interfejsu użytkownika.

### <a name="silent-token-request-from-cache-and-refresh"></a>Dyskretnej żądania tokenu z pamięci podręcznej i Odśwież
Dyskretnej żądania tokenu nie używa wyskakujących interfejsu użytkownika i nie wymaga działania. Zwraca tokenu z pamięci podręcznej Jeśli jest dostępna. Jeśli token utracił ważność, ta metoda próbuje go odświeżyć. Jeśli token odświeżania wygasła lub nie powiodło się, zwraca authenticationexception —.

    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );

Można również przeprowadzać synchronizacji wywołać za pomocą tej metody. Można ustawić wartości null do wywołania zwrotnego lub użyć acquireTokenSilentSync.

### <a name="diagnostics"></a>Diagnostyka
Są to podstawowego źródła informacji dotyczących diagnozowania problemów:

* Wyjątki
* Dzienniki
* Śledzenie sieci

Należy zauważyć, że identyfikatorów korelacji Centrum diagnostyki w bibliotece. Można ustawić sieci korelacji identyfikatorów na podstawie danego żądania, jeśli chcesz skorelować ADAL żądania z innymi operacjami w kodzie. Jeśli identyfikator korelacji nie jest ustawiona, biblioteka ADAL wygeneruje losowy. Wszystkie komunikaty dziennika i wywołania sieci zostanie następnie opatrzone identyfikatora korelacji. Automatycznie wygenerowany identyfikator zmian na każdym żądaniu.

#### <a name="exceptions"></a>Wyjątki
Wyjątki są dane diagnostyczne pierwszy. Spróbujemy Podaj przydatne komunikaty. Jeśli znajdziesz taki, który nie jest pomocna, zgłoś problem i Daj nam znać. Zawierają informacje urządzenia, takie jak modelu oraz liczbę zestawu SDK.

#### <a name="logs"></a>Dzienniki
Można skonfigurować biblioteki do generowania komunikatów dziennika, które można użyć do diagnozowania problemów. Rejestrowanie można skonfigurować za pomocą następujących wywołania, aby skonfigurować wywołanie zwrotne, które ADAL użyje do strony poza wszystkich komunikatach dziennika, ponieważ jest ona generowana.

    Logger.getInstance().setExternalLogger(new ILogger() {
        @Override
        public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
        ...
        // You can write this to log file depending on level or error code.
        writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
        }
    }

Komunikaty można pisać do niestandardowego pliku dziennika, jak pokazano w poniższym kodzie. Niestety nie istnieje standardowy sposób pobierania dzienniki z urządzenia. Brak niektórych usług, które mogą pomóc Ci z tym. Możesz można również zapasów własny, takie jak wysyłanie pliku na serwerze.

    private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
    }

Są to poziomów rejestrowania:
* Błąd (wyjątków)
* Warn (ostrzeżenie)
* Informacje o (celów informacyjnych)
* Pełne (więcej szczegółów)

Możesz ustawić poziom dziennika następująco:

    Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

 Wszystkie komunikaty dziennika są wysyłane do logcat oprócz wszystkie wywołania zwrotne dziennika niestandardowego.
Możesz pobrać dziennik do pliku ze logcat w następujący sposób:

    adb logcat > "C:\logmsg\logfile.txt"

 Aby uzyskać więcej informacji o poleceniach adb, zobacz [logcat informacji na temat witryny systemu Android](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).

#### <a name="network-traces"></a>Śledzenie sieci
Można użyć różnych narzędzi do przechwytywania ruchu HTTP, które generuje biblioteki ADAL.  Jest to najbardziej przydatne, jeśli znasz protokołu OAuth lub należy podać informacje diagnostyczne do firmy Microsoft lub innych kanałów pomocy technicznej.

Fiddler jest najprostszym narzędzie śledzenia HTTP. Skorzystaj z poniższych linków, aby ustawić ją do poprawnie rekordów ADAL ruchu sieciowego. Dla narzędzia śledzenia, takiego jak Fiddler lub Charlesa powinna być użyteczna musisz skonfigurować je do rejestrowania nieszyfrowanego ruchu protokołu SSL.  

> [!NOTE]
> Ślady generowane w ten sposób mogą zawierać wysoko uprzywilejowane informacje, takie jak tokenów dostępu, nazwy użytkowników i hasła. Jeśli używasz konta produkcji, nie są udostępniane te operacje śledzenia stron trzecich. Jeśli musisz podać śledzenia do osoby, aby uzyskać pomoc techniczną, Odtwórz problem przy użyciu tymczasowego konta użytkowników i hasła, które nie stanowi udostępniania.

* Ze strony firmy Telerik witryny sieci Web: [ustawienia zapasowej Fiddler dla systemu Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
* Z witryny GitHub: [skonfigurować reguły Fiddler dotyczące biblioteki ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)

### <a name="dialog-mode"></a>Tryb okna dialogowego
Metoda acquireToken bez działania obsługuje wiersza okna dialogowego.

### <a name="encryption"></a>Szyfrowanie
Biblioteka ADAL szyfruje tokeny i magazynu w SharedPreferences domyślnie. Można przyjrzeć się klasy StorageHelper, aby wyświetlić szczegóły. Android wprowadzonej Android Keystore 4.3 (interfejs API 18) bezpieczny magazyn kluczy prywatnych. Biblioteka ADAL użyty dla interfejsu API 18 lub nowszym. Jeśli chcesz używać biblioteki ADAL dla niższych wersjach zestawu SDK, należy podać klucz tajny w AuthenticationSettings.INSTANCE.setSecretKey.

### <a name="oauth2-bearer-challenge"></a>Żądania elementu nośnego OAuth2
Klasa AuthenticationParameters udostępnia funkcję można pobrać authorization_uri z żądania elementu nośnego OAuth2.

### <a name="session-cookies-in-webview"></a>Pliki cookie z sesji w widoku sieci Web
Android WebView nie wyczyść pliki cookie z sesji, po zamknięciu aplikacji. Który można obsługiwać przy użyciu ten przykładowy kod:

    CookieSyncManager.createInstance(getApplicationContext());
    CookieManager cookieManager = CookieManager.getInstance();
    cookieManager.removeSessionCookie();
    CookieSyncManager.getInstance().sync();

Aby uzyskać więcej informacji o plikach cookie, zobacz [CookieSyncManager informacji na temat witryny systemu Android](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).

### <a name="resource-overrides"></a>Zastąpienia zasobów
Biblioteka ADAL zawiera ciągi angielskiej wersji językowej ProgressDialog wiadomości. Aplikacji należy je zastąpić, jeśli chcesz zlokalizowanych ciągów.

     <string name="app_loading">Loading...</string>
     <string name="broker_processing">Broker is processing</string>
     <string name="http_auth_dialog_username">Username</string>
     <string name="http_auth_dialog_password">Password</string>
     <string name="http_auth_dialog_title">Sign In</string>
     <string name="http_auth_dialog_login">Login</string>
     <string name="http_auth_dialog_cancel">Cancel</string>

### <a name="ntlm-dialog-box"></a>Okno dialogowe NTLM
ADAL w wersji 1.1.0 obsługuje okno dialogowe NTLM, jest przetwarzany przez zdarzenie onReceivedHttpAuthRequest z WebViewClient. Można dostosować układ i ciągi dla okna dialogowego.

### <a name="cross-app-sso"></a>Usługa rejestracji Jednokrotnej w wielu aplikacji
Dowiedz się [jak włączyć logowanie Jednokrotne wielu aplikacji w systemie Android przy użyciu biblioteki ADAL](active-directory-sso-android.md).  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
