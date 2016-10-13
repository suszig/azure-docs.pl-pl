<properties
    pageTitle="Azure Active Directory B2C: wywoływanie interfejsu API sieci Web z aplikacji dla systemu iOS przy użyciu bibliotek innej firmy | Microsoft Azure"
    description="W tym artykule opisano, jak utworzyć aplikację z listą zadań do wykonania dla systemu iOS, która wywołuje interfejs API sieci Web w środowisku Node.js za pomocą tokenów elementu nośnego OAuth 2.0 przy użyciu biblioteki innej firmy."
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags ms.service="active-directory-b2c" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic="hero-article"

    ms.date="07/26/2016"
    ms.author="brandwe"/>


# Azure AD B2C: wywoływanie interfejsu API sieci Web z aplikacji dla systemu iOS przy użyciu biblioteki innej firmy

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Platforma Microsoft Identity korzysta z otwartych standardów, takich jak OAuth2 i OpenID Connect. Umożliwia to deweloperom korzystanie z każdej biblioteki, którą chcą zintegrować z naszymi usługami. Aby pomóc deweloperom w używaniu naszej platformy z innymi bibliotekami, utworzyliśmy kilka przewodników podobnych do tego. Przedstawiają one sposób konfigurowania bibliotek innych firm pod kątem nawiązywania połączenia z platformą Microsoft Identity. Z platformą Microsoft Identity może łączyć się większość bibliotek implementujących [specyfikację RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749).


Jeśli dopiero rozpoczynasz korzystanie ze standardu OAuth2 lub OpenID Connect, spora część tej przykładowej konfiguracji może być dla Ciebie niezrozumiała. Zalecamy zapoznanie się z [krótkim omówieniem protokołu w tej dokumentacji](active-directory-b2c-reference-protocols.md).

> [AZURE.NOTE]
    Niektóre funkcje naszej platformy, które mają wyrażenie w tych standardach, na przykład dostęp warunkowy i zarządzanie zasadami usługi Intune, wymagają użycia naszych bibliotek usługi Microsoft Azure Identity typu open source. 
   
Nie wszystkie scenariusze i funkcje usługi Azure Active Directory są obsługiwane przez platformę B2C.  Aby ustalić, czy należy użyć platformy B2C, zapoznaj się z [jej ograniczeniami](active-directory-b2c-limitations.md).


## Tworzenie katalogu usługi Azure AD B2C

Przed rozpoczęciem korzystania z usługi Azure AD B2C należy utworzyć katalog lub dzierżawę. Katalog jest kontenerem dla wszystkich użytkowników, aplikacji, grup i innych elementów. Jeśli jeszcze go nie masz, [utwórz katalog usługi B2C](active-directory-b2c-get-started.md), zanim przejdziesz dalej.

## Tworzenie aplikacji

Następnie musisz utworzyć aplikację w katalogu usługi B2C. Dzięki temu do usługi Azure AD będą przekazywane informacje wymagane do bezpiecznego komunikowania się z aplikacją. W takim przypadku zarówno aplikacja, jak i interfejs API sieci Web będą reprezentowane przez jeden **Identyfikator aplikacji**, ponieważ wchodzą w skład jednej aplikacji logicznej. Aby utworzyć aplikację, postępuj zgodnie z [tymi instrukcjami](active-directory-b2c-app-registration.md). Należy pamiętać o wykonaniu następujących czynności:

- Uwzględnij **urządzenie przenośne** w aplikacji.
- Skopiuj **Identyfikator aplikacji** przypisany do aplikacji. On również będzie później potrzebny.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Tworzenie zasad

W usłudze Azure AD B2C każde działanie użytkownika jest definiowane przy użyciu [zasad](active-directory-b2c-reference-policies.md). Ta aplikacja zawiera jedno rozwiązanie tożsamości: rejestracja połączona z logowaniem. Te zasady musisz utworzyć dla każdego typu zgodnie z opisem w [artykule dotyczącym struktury zasad](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Podczas tworzenia zasad należy:

- Wybrać **Nazwę wyświetlaną** i atrybuty rejestracji w zasadach.
- Wybrać oświadczenia aplikacji **Nazwa wyświetlana** oraz **Identyfikator obiektu** we wszystkich zasadach. Można również wybrać inne oświadczenia.
- Skopiować każdą utworzoną wartość **Nazwa** zasad. Powinny zawierać prefiks `b2c_1_`.  Nazwa zasad będzie potrzebna później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po utworzeniu zasad będzie można rozpocząć tworzenie aplikacji.


## Pobieranie kodu

Kod używany w tym samouczku jest przechowywany [ w serwisie GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c).  Aby wykonać czynności z tego samouczka, możesz [pobrać aplikację jako plik zip](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)/archive/master.zip) lub sklonować ten plik:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Możesz też pobrać cały kod i rozpocząć od razu: 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## Pobieranie biblioteki innej firmy nxoauth2 i uruchamianie obszaru roboczego

W tym przewodniku użyjemy pliku OAuth2Client z serwisu GitHub, biblioteki OAuth2 dla systemu Mac OS X i iOS (Cocoa i Cocoa touch). Ta biblioteka jest oparta na wersji 10 specyfikacji OAuth2. Implementuje ona natywny profil aplikacji i obsługuje punkt końcowy autoryzacji użytkownika końcowego. Jest to potrzebne do integracji z platformą Microsoft Identity.

### Dodawanie biblioteki do projektu przy użyciu menedżera CocoaPods

CocoaPods to menedżer zależności dla projektów Xcode. Automatycznie zarządza on powyższymi krokami instalacji.

```
$ vi Podfile
```
Dodaj następujący kod do tego pliku podfile:

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

Następnie załaduj plik podfile przy użyciu menedżera cocoapods. Spowoduje to utworzenie nowego obszaru roboczego Xcode, który zostanie załadowany.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## Struktura projektu

Struktura szkieletu naszego projektu wygląda następująco:

* **Widok główny** z okienkiem zadań
* **Widok dodawania zadania** zawierający dane o wybranym zadaniu
* **Widok logowania** umożliwiający użytkownikowi zalogowanie się w aplikacji

Będziemy korzystać z różnych plików w projekcie w celu dodania uwierzytelniania. Inne części kodu, na przykład kod wizualny, nie mają istotnego znaczenia dla tożsamości i są dostarczone dla Twojej wygody.

## Tworzenie pliku `settings.plist` dla aplikacji

Aplikację można łatwiej skonfigurować, gdy istnieje scentralizowana lokalizacja do przechowywania wartości konfiguracji. Lokalizacja ta pomaga też zrozumieć działanie poszczególnych ustawień w aplikacji. Będziemy korzystać z *listy właściwości* jako sposobu na podanie tych wartości do aplikacji.

* Utwórz/otwórz plik `settings.plist` w obszarze roboczym aplikacji `Supporting Files`

* Wprowadź następujące wartości (poniżej są szczegółowo omówione)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

Szczegółowe omówienie wartości.


W przypadku elementów `authURL`, `loginURL`, `bhh` i `tokenURL` należy podać nazwę dzierżawy. Jest to przypisana do Ciebie nazwa dzierżawy B2C. Przykładowo może to być `kidventusb2c.onmicrosoft.com`. W przypadku korzystania z naszych bibliotek usługi Microsoft Azure Identity typu open source te dane są automatycznie pobierane przy użyciu naszego punktu końcowego metadanych. Włożyliśmy dużo wysiłku w wyodrębnianie tych wartości.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Wartość `keychain` to kontener, którego biblioteka NXOAuth2Client używa do tworzenia łańcucha kluczy do przechowywania tokenów. Aby uzyskać logowanie jednokrotne w wielu aplikacjach, możesz określić ten sam łańcuch kluczy w każdej z aplikacji oraz zażądać użycia tego łańcucha kluczy w uprawnieniach XCode. Odpowiednie informacje zawiera dokumentacja firmy Apple.

Element `<policy name>` na końcu każdego adresu URL to miejsce na utworzone wcześniej zasady. Aplikacja będzie wywoływać te zasady zależnie od przepływu.

Element `taskAPI` to punkt końcowy REST, który będzie przez nas wywoływany przy użyciu Twojego tokenu B2C w celu dodania zadań lub wykonania zapytań dotyczących istniejących zadań. Jest to konfiguracja specyficzna dla tego przykładu. Nie trzeba jej zmieniać, aby zadziałał przykładowy kod.

Pozostałe wartości są wymagane do używania biblioteki i po prostu tworzą miejsca do przekazywania wartości do kontekstu.

Po utworzeniu pliku `settings.plist` potrzebny będzie kod do jego odczytania.

## Konfigurowanie klasy AppData do odczytu ustawień

Utwórzmy prosty plik, który analizuje utworzony wcześniej plik `settngs.plist`, i udostępnijmy te ustawienia tak, aby były w przyszłości dostępne dla każdej klasy. Ponieważ nie chcemy tworzyć nowej kopii danych przy każdym pytaniu klasy o nie, użyjemy wzorca Singleton, aby było zwracane to samo wystąpienie tworzone w przypadku każdego żądania dotyczącego ustawień

* Utwórz plik `AppData.h`:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Utwórz plik `AppData.m`:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Po wykonaniu powyższych czynności będzie można w łatwy sposób pobrać dane, wywołując element `  AppData *data = [AppData getInstance];` w klasie zgodnie z poniższym opisem.



## Konfigurowanie biblioteki NXOAuth2Client w elemencie AppDelegate

Biblioteka NXOAuthClient wymaga ustawienia niektórych wartości. Następnie można użyć uzyskanego tokenu do wywołania interfejsu API REST. Ponieważ element `AppDelegate` będzie wywoływany przy każdym ładowaniu aplikacji, dobrym pomysłem jest umieszczenie wartości konfiguracji w takim pliku.
* Otwórz plik `AppDelegate.m`

* Zaimportuj pliki nagłówkowe, które zostaną użyte później.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* Dodaj metodę `setupOAuth2AccountStore` w elemencie AppDelegate

Musisz utworzyć element AccountStore i przekazać do niego dane odczytane z pliku `settings.plist`.

Należy pamiętać o niektórych kwestiach związanych z usługą B2C, dzięki którym ten kod będzie bardziej zrozumiały:


1. Usługa Azure AD B2C używa *zasad* udostępnionych przy użyciu parametrów zapytania na potrzeby obsługi żądania. Dzięki temu usługa Azure Active Directory może działać jako niezależna usługa tylko dla Twojej aplikacji. W celu zapewnienia dodatkowych parametrów zapytania do metody `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` należy przekazać niestandardowe parametry zasad. 

2. Usługa Azure AD B2C używa zakresów w taki sam sposób jak inne serwery OAuth2. Jednak używanie usługi B2C wiąże się w taki sam sposób z uwierzytelnianiem użytkownika i uzyskiwaniem dostępu do zasobów, dlatego niektóre zakresy są niezbędne do prawidłowego działania przepływu. Jest to zakres `openid`. Zestawy SDK usługi Microsoft Identity automatycznie udostępniają zakres `openid`, dlatego nie ma go w naszej konfiguracji zestawu SDK. Jednak ze względu na używanie biblioteki innej firmy trzeba określić ten zakres.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Następnie upewnij się, że ten zakres jest wywoływany w elemencie AppDelegate w metodzie `didFinishLaunchingWithOptions:`. 

```
[self setupOAuth2AccountStore];
```


## Tworzenie klasy `LoginViewController`, która będzie używana do obsługi żądań uwierzytelniania

Do logowania na koncie używany jest element webview. Dzięki temu można wyświetlić prośbę o użycie przez użytkownika dodatkowych składników, na przykład wiadomości SMS (jeśli skonfigurowano), lub wyświetlać komunikaty o błędzie dla użytkownika. Teraz musisz skonfigurować element webview, a następnie napisać kod obsługujący wywołania zwrotne z usługi Microsoft Identity, które będą występować w elemencie WebView.

* Utwórz klasę `LoginViewController.h`

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

Każda z tych metod zostanie utworzona poniżej.

> [AZURE.NOTE] 
    Należy upewnić się, że powiązano element `loginView` z elementem webview znajdującym się w scenorysie. W przeciwnym razie nie będzie istnieć wyskakujący element webview, który może zostać wyświetlony, gdy będzie konieczne uwierzytelnianie.

* Utwórz klasę `LoginViewController.m`

* Dodaj zmienne przekazujące informacje o stanie podczas uwierzytelniania

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* Przesłoń metody WebView na potrzeby obsługi uwierzytelniania

Musimy wskazać elementowi webview działanie, które będzie pożądane, gdy użytkownik zechce się zalogować (zgodnie z powyższym omówieniem). Możesz po prostu skopiować i wkleić poniższy kod.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* Napisz kod obsługujący wynik żądania OAuth2

Będziemy potrzebować kodu obsługującego element redirectURL zwracany z elementu WebView. W przypadku niepowodzenia zostanie podjęta kolejna próba. W międzyczasie biblioteka zgłosi błąd, który może zostać wyświetlony w konsoli lub obsłużony asynchronicznie. 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* Skonfiguruj fabryki powiadomień.

W tym kroku zostanie utworzona ta sama metoda, co w przypadku elementu `AppDelegate` powyżej, ale dodatkowo dodawane są elementy `NSNotification` informujące o tym, co dzieje się z naszą usługą. Skonfigurujemy obserwatora informującego o wszystkich zmianach dotyczących tokenu. Po otrzymaniu tokenu przekierujemy użytkownika z powrotem do elementu `masterView`.



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* Dodaj kod obsługujący użytkownika, gdy żądanie jest inicjowane dla elementu sign-native

Utwórzmy metodę, która będzie wywoływana w przypadku każdego żądania uwierzytelniania. Będzie to metoda tworząca element webview

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Na koniec ustawmy wywoływanie wszystkich powyżej napisanych metod przy każdym ładowaniu elementu `LoginViewController`. W tym celu dodamy te metody do metody `viewDidLoad` udostępnionej przez firmę Apple

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

Po wykonaniu powyższych czynności zostanie utworzony główny sposób interakcji z aplikacją podczas logowania. Po zalogowaniu musimy użyć otrzymanych tokenów. W tym celu utworzymy kod pomocniczy, który będzie dla nas wywoływać interfejsy API REST przy użyciu tej biblioteki.


## Tworzenie klasy `GraphAPICaller` do obsługi żądań do interfejsu API REST

Konfiguracja jest ładowana zawsze podczas ładowania aplikacji. Ładowanie aplikacji wraz z konfiguracją oraz uzyskanie tokenu umożliwia wykonanie kolejnych kroków. 

* Utwórz plik `GraphAPICaller.h`

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

Jak widać, zostaną utworzone dwie metody: jedna, która pobiera zadania z interfejsu API, oraz druga, która dodaje zadania do interfejsu API.

Po skonfigurowaniu interfejsu należy dodać rzeczywistą implementację:

* Utwórz plik `GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## Uruchamianie przykładowej aplikacji

Na koniec skompiluj i uruchom aplikację w środowisku Xcode. Zarejestruj się lub zaloguj w aplikacji i utwórz zadania dla zalogowanego użytkownika. Wyloguj się i zaloguj się ponownie jako inny użytkownik i utwórz zadania dla tego użytkownika.

Zwróć uwagę, że zadania są przechowywane dla poszczególnych użytkowników w interfejsie API, ponieważ wyodrębnia on tożsamość użytkownika z tokenu dostępu, który odbiera.


## Następne kroki

Możesz teraz przejść do bardziej zaawansowanych tematów dotyczących usługi B2C. Zobacz te tematy:

[Wywoływanie interfejsu API sieci Web w środowisku Node.js z aplikacji sieci Web dla środowiska Node.js]()

[Dostosowywanie środowiska użytkownika w aplikacji B2C]()



<!--HONumber=Oct16_HO1-->


