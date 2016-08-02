<properties
    pageTitle="Azure Active Directory B2C (wersja zapoznawcza): wywoływanie interfejsu API sieci Web z aplikacji dla systemu iOS | Microsoft Azure"
    description="W tym artykule opisano, jak utworzyć aplikację z listą zadań do wykonania dla systemu iOS, która wywołuje interfejs API sieci Web w środowisku Node.js za pomocą tokenów elementu nośnego OAuth 2.0. Zarówno aplikacja dla systemu iOS, jak i interfejs API sieci Web korzystają z usługi Azure Active Directory B2C, aby zarządzać tożsamościami użytkowników i ich uwierzytelniać."
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags ms.service="active-directory-b2c" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic="hero-article"

    ms.date="05/31/2016"
    ms.author="brandwe"/>

# Azure AD B2C (wersja zapoznawcza): wywołanie interfejsu API sieci Web z aplikacji dla systemu iOS

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Za pomocą usługi Azure Active Directory (Azure AD) B2C można dodać zaawansowane funkcje samoobsługowego zarządzania tożsamościami do aplikacji dla systemu iOS i interfejsów API sieci Web w kilku krótkich krokach. W tym artykule opisano, jak utworzyć aplikację z „listą zadań” do wykonania dla systemu iOS, która wywołuje interfejs API sieci Web w środowisku Node.js za pomocą tokenów elementu nośnego OAuth 2.0. Zarówno aplikacja dla systemu iOS, jak i interfejs API sieci Web korzystają z usługi Azure AD B2C, aby zarządzać tożsamościami użytkowników i ich uwierzytelniać.

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

> [AZURE.NOTE]
    Aby można było w pełni skorzystać z tego przewodnika Szybki start, potrzebny będzie gotowy interfejs API sieci Web chroniony za pomocą usługi Azure AD B2C. Utworzyliśmy taki interfejs dla środowiska .NET i Node.js. W tym przewodniku zakłada się, że przykładowy interfejs API sieci Web w środowisku Node.js został skonfigurowany. Więcej informacji można znaleźć w temacie opisującym [przykładowy interfejs API sieci Web dla środowiska Node.js w usłudze Azure Active Directory](active-directory-b2c-devquickstarts-api-node.md)
).

> [AZURE.NOTE]
    W tym artykule nie opisano sposobu wdrażania logowania, rejestracji i zarządzania profilami za pomocą usługi Azure AD B2C. Skupiono się tu w szczególności na wywoływaniu interfejsów API sieci Web po uwierzytelnieniu użytkownika. Zalecamy, aby na początek zapoznać się z [wprowadzeniem do aplikacji sieci Web w środowisku .NET](active-directory-b2c-devquickstarts-web-dotnet.md) i poznać podstawy usługi Azure Active Directory B2C.

## Tworzenie katalogu usługi Azure AD B2C

Przed rozpoczęciem korzystania z usługi Azure AD B2C należy utworzyć katalog lub dzierżawę. Katalog jest kontenerem dla wszystkich użytkowników, aplikacji, grup i innych elementów. Jeśli jeszcze go nie masz, [utwórz katalog usługi B2C](active-directory-b2c-get-started.md), zanim przejdziesz dalej.

## Tworzenie aplikacji

Następnie musisz utworzyć aplikację w katalogu usługi B2C. Dzięki temu do usługi Azure AD będą przekazywane informacje wymagane do bezpiecznego komunikowania się z aplikacją. W takim przypadku zarówno aplikacja, jak i interfejs API sieci Web będą reprezentowane przez jeden **Identyfikator aplikacji**, ponieważ wchodzą w skład jednej aplikacji logicznej. Aby utworzyć aplikację, postępuj zgodnie z [tymi instrukcjami](active-directory-b2c-app-registration.md). Należy pamiętać o wykonaniu następujących czynności:

- Dołącz do aplikacji **aplikację sieci Web / interfejs API sieci Web**
- Wprowadź `http://localhost:3000/auth/openid/return` w polu **Adres URL odpowiedzi**. Jest to domyślny adres URL dla tej próbki kodu.
- Utwórz **klucz tajny aplikacji** i skopiuj go. Będzie potrzebny później.
- Skopiuj **Identyfikator aplikacji** przypisany do aplikacji. On również będzie później potrzebny.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Tworzenie zasad

W usłudze Azure AD B2C każde działanie użytkownika jest definiowane przy użyciu [zasad](active-directory-b2c-reference-policies.md). Ta aplikacja obejmuje trzy środowiska tożsamości: rejestracji, logowania i logowania przy użyciu konta w serwisie Facebook. Dla każdego typu należy utworzyć jeden zbiór zasad zgodnie z opisem w [artykule o zasadach](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Podczas tworzenia trzech zbiorów zasad należy koniecznie:

- Wybrać wartość **Nazwa wyświetlana** i atrybuty rejestracji w zasadach rejestracji.
- Wybrać oświadczenia aplikacji **Nazwa wyświetlana** oraz **Identyfikator obiektu** we wszystkich zasadach. Można również wybrać inne oświadczenia.
- Skopiować każdą utworzoną wartość **Nazwa** zasad. Powinny zawierać prefiks `b2c_1_`.  Te nazwy zasad będą potrzebne później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po utworzeniu trzech zbiorów zasad można rozpocząć tworzenie aplikacji.

Należy pamiętać, że w tym artykule nie opisano sposobu korzystania z nowo utworzonych zasad. Aby dowiedzieć się, jak działają zasady w usłudze Azure AD B2C, należy najpierw zapoznać się z [samouczkiem ułatwiającym rozpoczęcie pracy z aplikacją sieci Web platformy .NET](active-directory-b2c-devquickstarts-web-dotnet.md).

## Pobieranie kodu

Kod używany w tym samouczku [jest przechowywany w serwisie GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS). Aby utworzyć przykład w trakcie pracy, możesz [pobrać szkielet projektu jako plik ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS/archive/skeleton.zip). Można również sklonować szkielet:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS.git
```

> [AZURE.NOTE] **Aby zakończyć wykonywanie zadań z tego samouczka, należy pobrać szkielet.** Z powodu złożoności wdrażania w pełni funkcjonującej aplikacji w systemie iOS **szkielet** zawiera kod UX, który zostanie uruchomiony po ukończeniu samouczka. Pozwoli to deweloperom zaoszczędzić czas. Kod UX nie ma istotnego znaczenia dla tematu dotyczącego dodawania usługi B2C do aplikacji dla systemu iOS.

Utworzona aplikacja jest również [dostępna jako plik .zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS/archive/complete.zip) lub w gałęzi `complete` tego samego repozytorium.

Następnie załaduj `podfile` przy użyciu programu CocoaPods. Spowoduje to utworzenie nowego obszaru roboczego Xcode, który zostanie załadowany. Jeśli nie masz programu CocoaPods, [odwiedź witrynę, aby go skonfigurować](https://cocoapods.org).

```
$ pod install
...
$ open Microsoft Tasks for Consumers.xcworkspace
```

## Konfigurowanie aplikacji zadań dla systemu iOS

Aby skonfigurować aplikację zadań dla systemu iOS do komunikowania się z usługą Azure AD B2C, trzeba podać kilka wspólnych parametrów. W folderze `Microsoft Tasks` otwórz plik `settings.plist` w katalogu głównym projektu i zastąp wartości w sekcji `<dict>`. Wartości te będą używane w całej aplikacji.

```
<dict>
    <key>authority</key>
    <string>https://login.microsoftonline.com/<your tenant name>.onmicrosoft.com/</string>
    <key>clientId</key>
    <string><Enter the Application Id assigned to your app by the Azure portal, e.g.580e250c-8f26-49d0-bee8-1c078add1609></string>
    <key>scopes</key>
    <array>
        <string><Enter the Application Id assigned to your app by the Azure portal, e.g.580e250c-8f26-49d0-bee8-1c078add1609></string>
    </array>
    <key>additionalScopes</key>
    <array>
    </array>
    <key>redirectUri</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>taskWebAPI</key>
    <string>http://localhost/tasks:3000</string>
    <key>emailSignUpPolicyId</key>
    <string><Enter your sign up policy name, e.g.g b2c_1_sign_up></string>
    <key>faceBookSignInPolicyId</key>
    <string><your sign in policy for FB></string>
    <key>emailSignInPolicyId</key>
    <string><Enter your sign in policy name, e.g. b2c_1_sign_in></string>
    <key>fullScreen</key>
    <false/>
    <key>showClaims</key>
    <true/>
</dict>
</plist>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

## Pobieranie tokenów dostępu i wywoływanie interfejsu API zadań

W tej sekcji omówiono sposób wymiany tokenu OAuth 2.0 w aplikacji sieci Web za pomocą bibliotek i platform firmy Microsoft. Jeśli nie wiesz nic o kodach autoryzacji i tokenach dostępu, skorzystaj z [przewodnika po protokole OAuth 2.0](active-directory-b2c-reference-protocols.md).

### Tworzenie plików nagłówka przy użyciu metod

Metody będą niezbędne do pobrania tokenu z wybranymi zasadami, a następnie wywołania serwera zadań. Skonfiguruj je teraz.

Utwórz plik o nazwie `samplesWebAPIConnector.h` w katalogu `/Microsoft Tasks` w projekcie Xcode

Dodaj następujący kod, aby zdefiniować, co należy zrobić:

```
#import <Foundation/Foundation.h>
#import "samplesTaskItem.h"
#import "samplesPolicyData.h"
#import "ADALiOS/ADAuthenticationContext.h"

@interface samplesWebAPIConnector : NSObject<NSURLConnectionDataDelegate>

+(void) getTaskList:(void (^) (NSArray*, NSError* error))completionBlock
             parent:(UIViewController*) parent;

+(void) addTask:(samplesTaskItem*)task
         parent:(UIViewController*) parent
completionBlock:(void (^) (bool, NSError* error)) completionBlock;

+(void) deleteTask:(samplesTaskItem*)task
            parent:(UIViewController*) parent
   completionBlock:(void (^) (bool, NSError* error)) completionBlock;

+(void) doPolicy:(samplesPolicyData*)policy
         parent:(UIViewController*) parent
completionBlock:(void (^) (ADProfileInfo* userInfo, NSError* error)) completionBlock;

+(void) signOut;

@end
```

Są to proste operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD) w interfejsie API, a także metoda `doPolicy`. Za pomocą tej metody, można pobrać token z zasadami, które są nam potrzebne.

Należy zauważyć, że trzeba zdefiniować dwa inne pliki nagłówków. Będą one zawierać element zadania i dane zasad. Utwórz je teraz:

Utwórz plik `samplesTaskItem.h` z następującym kodem:

```
#import <Foundation/Foundation.h>

@interface samplesTaskItem : NSObject

@property NSString *itemName;
@property NSString *ownerName;
@property BOOL completed;
@property (readonly) NSDate *creationDate;

@end
```

Utwórz również plik `samplesPolicyData.h` do przechowywania danych zasad:

```
#import <Foundation/Foundation.h>

@interface samplesPolicyData : NSObject

@property (strong) NSString* policyName;
@property (strong) NSString* policyID;

+(id) getInstance;

@end
```
### Dodawanie wdrażania do zadania i elementów zasad

Pliki nagłówka są już gotowe do użycia, więc można napisać kod do przechowywania danych, który zostanie użyty w próbce.

Utwórz plik `samplesPolicyData.m` z następującym kodem:

```
#import <Foundation/Foundation.h>
#import "samplesPolicyData.h"

@implementation samplesPolicyData

+(id) getInstance
{
    static samplesPolicyData *instance = nil;
    static dispatch_once_t onceToken;

    dispatch_once(&onceToken, ^{
        instance = [[self alloc] init];
        NSDictionary *dictionary = [NSDictionary dictionaryWithContentsOfFile:[[NSBundle mainBundle] pathForResource:@"settings" ofType:@"plist"]];
        instance.policyName = [dictionary objectForKey:@"policyName"];
        instance.policyID = [dictionary objectForKey:@"policyID"];


    });

    return instance;
}


@end
```

### Pisanie kodu konfiguracji dla wywołania do biblioteki ADAL dla systemu iOS

Szybki kod do przechowywania obiektów dla interfejsu użytkownika jest teraz gotowy. Następnie należy napisać kod, aby uzyskać dostęp biblioteki ADAL dla systemu iOS przy użyciu parametrów umieszczonych w pliku `settings.plist`. Pozwoli to uzyskać token dostępu, który będzie można dostarczyć do serwera zadań.

Wszystkie zadania będą wykonywane w metodzie `samplesWebAPIConnector.m`.

Najpierw utwórz wdrożenie `doPolicy()` zapisane w pliku nagłówka `samplesWebAPIConnector.h`:

```
+(void) doPolicy:(samplesPolicyData *)policy
         parent:(UIViewController*) parent
completionBlock:(void (^) (ADProfileInfo* userInfo, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    [self getClaimsWithPolicyClearingCache:NO policy:policy params:nil parent:parent completionHandler:^(ADProfileInfo* userInfo, NSError* error) {

        if (userInfo == nil)
        {
            completionBlock(nil, error);
        }

        else {

            completionBlock(userInfo, nil);
        }
    }];

}


```

Ta metoda jest prosta. Rolę danych wejściowych pełni utworzony obiekt `samplesPolicyData`, obiekt nadrzędny `ViewController` i wywołanie zwrotne. Wywołanie zwrotne jest ciekawe i bliżej się mu przyjrzymy.

- Należy pamiętać, że typem `completionBlock` jest `ADProfileInfo`, który zostanie zwrócony przy użyciu obiektu `userInfo`. `ADProfileInfo` to typ, który przechowuje wszystkie odpowiedzi z serwera, w tym oświadczenia.
- Zwróć też uwagę na element `readApplicationSettings`. Odczytuje on dane dostarczone w pliku `settings.plist`.
- Na koniec mamy jeszcze dużą metodę `getClaimsWithPolicyClearingCache`. Jest to rzeczywiste wywołanie do biblioteki ADAL dla systemu iOS, które należy napisać. Wrócimy do tego później.

Następnie należy napisać dużą metodę `getClaimsWithPolicyClearingCache`. To na tyle skomplikowane zagadnienie, że opiszemy je w osobnej sekcji.

### Tworzenie wywołania do biblioteki ADAL dla systemu iOS

Po pobraniu szkieletu z serwisu GitHub widać, że mamy kilka tych wywołań gotowych do użycia, które mogą ułatwić obsługę przykładowej aplikacji. Wszystkie są oparte na wzorcu `get(Claims|Token)With<verb>ClearningCache`. Przy użyciu konwencji Objective C można je odczytywać podobnie do języka angielskiego. Na przykład „Get a token with extra parameters that I provide to you and clear the cache” (Pobierz token z dodatkowymi parametrami, które podam, i wyczyść pamięć podręczną) wygląda w następujący sposób: `getTokenWithExtraParamsClearingCache()`.

Napiszesz „Get claims and a token with the policy that I provide to you and don't clear the cache” (Pobierz oświadczenia i token wraz z podanymi przeze mnie zasadami i nie czyść pamięci podręcznej) lub `getClaimsWithPolicyClearingCache`. Z biblioteki ADAL zawsze uzyskujemy token, więc nie trzeba określać „oświadczeń i tokenu” w metodzie. Czasami jednak potrzebny jest tylko token bez niedogodności związanych z analizą oświadczeń, więc zapewniamy metodę bez oświadczeń o nazwie `getTokenWithPolicyClearingCache` w szkielecie.

Napisz teraz ten kod:

```
+(void) getClaimsWithPolicyClearingCache  : (BOOL) clearCache
                           policy:(samplesPolicyData *)policy
                           params:(NSDictionary*) params
                           parent:(UIViewController*) parent
                completionHandler:(void (^) (ADProfileInfo*, NSError*))completionBlock;
{
    SamplesApplicationData* data = [SamplesApplicationData getInstance];


    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    if(!data.correlationId ||
       [[data.correlationId stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]] length] == 0)
    {
        authContext.correlationId = [[NSUUID alloc] initWithUUIDString:data.correlationId];
    }

    [ADAuthenticationSettings sharedInstance].enableFullScreen = data.fullScreen;
    [authContext acquireTokenWithScopes:data.scopes
                      additionalScopes: data.additionalScopes
                              clientId:data.clientId
                           redirectUri:redirectUri
                            identifier:[ADUserIdentifier identifierWithId:data.userItem.profileInfo.username type:RequiredDisplayableId]
                            promptBehavior:AD_PROMPT_ALWAYS
                  extraQueryParameters: params.urlEncodedString
                                policy: policy.policyID
                       completionBlock:^(ADAuthenticationResult *result) {

                           if (result.status != AD_SUCCEEDED)
                           {
                               completionBlock(nil, result.error);
                           }                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.profileInfo, nil);
                              }
                          }];
}


```

Pierwsza część powinna wyglądać znajomo.

- Załaduj ustawienia podane w pliku `settings.plist` i przypisz je do elementu `data`.
- Skonfiguruj element `ADAuthenticationError`, który przejmuje wszystkie błędy pochodzące z biblioteki ADAL dla systemu iOS.
- Utwórz element `authContext`, który konfiguruje wywołanie do biblioteki ADAL. Przekazujesz do niego swoje uprawnienia w celu rozpoczęcia wykonywania zadań.
- Nadaj elementowi `authContext` odwołanie do kontrolera elementu nadrzędnego, dzięki czemu będzie można do niego wrócić.
- Przekonwertuj element `redirectURI`, który był ciągiem w pliku `settings.plist`, do typu NSURL oczekiwanego przez bibliotekę ADAL.
- Skonfiguruj element `correlationId`. Jest to identyfikator UUID, który może śledzić wywołanie w drodze od klienta do serwera i z powrotem. Może się to przydać podczas debugowania.

Następnie przechodzimy do rzeczywistego wywołania do biblioteki ADAL. Wywołanie jest teraz inne, niż można się było spodziewać w przypadku wcześniejszych zastosowań biblioteki ADAL dla systemu iOS:

```
[authContext acquireTokenWithScopes:data.scopes
                      additionalScopes: data.additionalScopes
                              clientId:data.clientId
                           redirectUri:redirectUri
                            identifier:[ADUserIdentifier identifierWithId:data.userItem.profileInfo.username type:RequiredDisplayableId]
                            promptBehavior:AD_PROMPT_ALWAYS
                  extraQueryParameters: params.urlEncodedString
                                policy: policy.policyID
                       completionBlock:^(ADAuthenticationResult *result) {

```

Widać tu, że wywołanie jest dość proste.

`scopes`: zakresy przekazywane do serwera, których chcesz żądać z serwera dla użytkownika, który się loguje. W wersji zapoznawczej usługi B2C przekazujesz `client_id`. Należy się jednak spodziewać, że w przyszłości nastąpi zmiana na zakresy odczytywania. Wtedy dokument ten zostanie zaktualizowany.
`additionalScopes`: są to dodatkowe zakresy, których można użyć w aplikacji. Należy się spodziewać, że będą używane w przyszłości.
`clientId`: identyfikator aplikacji pochodzący z portalu.
`redirectURI`: przekierowanie, do którego można spodziewać się ponownego zaksięgowania tokenu.
`identifier`: sposób identyfikacji użytkownika, pozwalający sprawdzić, czy w pamięci podręcznej znajduje się token nadający się do użycia. Dzięki temu można uniknąć ciągłego „proszenia” serwera o inny token. Ma to miejsce w typie o nazwie `ADUserIdentifier` i można podać, co ma być użyte jako identyfikator. Należy użyć elementu `username`.
`promptBehavior`: to przestarzała wartość, którą należy zastąpić wartością `AD_PROMPT_ALWAYS`.
`extraQueryParameters`: wszystko, co jest dodatkowe i ma być przekazane do serwera w formacie zakodowanym w adresie URL.
`policy`: wywoływane zasady. To najważniejszy element w tym przewodniku.

W elemencie `completionBlock` widać, że przekazywany jest wynik `ADAuthenticationResult`. Zawiera on informacje o tokenie i profilu (jeśli wywołanie zakończyło się pomyślnie).

Przy użyciu kodu powyżej można uzyskać token dla podanych zasad. Tokenu można następnie użyć do wywołania interfejsu API.

### Tworzenie wywołań zadań do serwera

Gdy masz już token, musisz przekazać go do interfejsu API w celu autoryzacji.

Należy wdrożyć trzy metody:

```
+(void) getTaskList:(void (^) (NSArray*, NSError* error))completionBlock
             parent:(UIViewController*) parent;

+(void) addTask:(samplesTaskItem*)task
         parent:(UIViewController*) parent
completionBlock:(void (^) (bool, NSError* error)) completionBlock;

+(void) deleteTask:(samplesTaskItem*)task
            parent:(UIViewController*) parent
   completionBlock:(void (^) (bool, NSError* error)) completionBlock;
```

`getTasksList` zawiera tablicę, która reprezentuje zadania na serwerze. `addTask` i `deleteTask` wykonują kolejne czynności i zwracają wartość `true` lub `false`, jeśli zostaną pomyślnie wykonane.

Zapisz najpierw `getTaskList`:

```

+(void) getTaskList:(void (^) (NSArray*, NSError*))completionBlock
             parent:(UIViewController*) parent;
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    SamplesApplicationData* data = [SamplesApplicationData getInstance];

    [self craftRequest:[self.class trimString:data.taskWebApiUrlString]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

        if (error != nil)
        {
            completionBlock(nil, error);
        }
        else
        {

            NSOperationQueue *queue = [[NSOperationQueue alloc]init];

            [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                if (error == nil && data != nil){

                    NSArray *tasks = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                    //each object is a key value pair
                    NSDictionary *keyValuePairs;
                    NSMutableArray* sampleTaskItems = [[NSMutableArray alloc]init];

                    for(int i =0; i < tasks.count; i++)
                    {
                        keyValuePairs = [tasks objectAtIndex:i];

                        samplesTaskItem *s = [[samplesTaskItem alloc]init];
                        s.itemName = [keyValuePairs valueForKey:@"task"];

                        [sampleTaskItems addObject:s];
                    }

                    completionBlock(sampleTaskItems, nil);
                }
                else
                {
                    completionBlock(nil, error);
                }

            }];
        }
    }];

}

```

Omówienie kodu zadania wykracza poza zakres tematyczny tego przewodnika, ale możemy tu zauważyć coś interesującego — metodę `craftRequest`, która pobiera adres URL zadania. Metody tej używa się do utworzenia żądania dla serwera przy użyciu otrzymanego tokenu dostępu. Zapisz ją teraz.

Dodaj następujący kod do pliku `samplesWebAPIConnector.m`:

```
+(void) craftRequest : (NSString*)webApiUrlString
               parent:(UIViewController*) parent
    completionHandler:(void (^)(NSMutableURLRequest*, NSError* error))completionBlock
{
    [self getClaimsWithPolicyClearingCache:NO parent:parent completionHandler:^(NSString* accessToken, NSError* error){

        if (accessToken == nil)
        {
            completionBlock(nil,error);
        }
        else
        {
            NSURL *webApiURL = [[NSURL alloc]initWithString:webApiUrlString];

            NSMutableURLRequest *request = [[NSMutableURLRequest alloc]initWithURL:webApiURL];

            NSString *authHeader = [NSString stringWithFormat:@"Bearer %@", accessToken];

            [request addValue:authHeader forHTTPHeaderField:@"Authorization"];

            completionBlock(request, nil);
        }
    }];
}
```

Powoduje to pobranie identyfikatora Uniform Resource Identifier (URI) sieci Web, dodanie do niego tokenu przy użyciu nagłówka `Bearer` w protokole HTTP, a następnie zwrócenie go użytkownikowi. Wywołujesz interfejs API `getTokenClearingCache`. To rozwiązanie może wydawać się nieco dziwne, ale to wywołanie służy po prostu do pobrania tokenu z pamięci podręcznej i sprawdzenia, czy jest nadal ważny. (Wywołania `getToken` wykonują te czynności przez odpytanie biblioteki ADAL). Ten kod będzie używany w każdym wywołaniu. Następnie utwórz dodatkowe metody zadań.

Zapisz `addTask`:

```
+(void) addTask:(samplesTaskItem*)task
         parent:(UIViewController*) parent
completionBlock:(void (^) (bool, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    SamplesApplicationData* data = [SamplesApplicationData getInstance];
    [self craftRequest:data.taskWebApiUrlString parent:parent completionHandler:^(NSMutableURLRequest* request, NSError* error){

        if (error != nil)
        {
            completionBlock(NO, error);
        }
        else
        {
            NSDictionary* taskInDictionaryFormat = [self convertTaskToDictionary:task];

            NSData* requestBody = [NSJSONSerialization dataWithJSONObject:taskInDictionaryFormat options:0 error:nil];

            [request setHTTPMethod:@"POST"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request setHTTPBody:requestBody];

            NSString *myString = [[NSString alloc] initWithData:requestBody encoding:NSUTF8StringEncoding];

            NSLog(@"Request was: %@", request);
            NSLog(@"Request body was: %@", myString);

            NSOperationQueue *queue = [[NSOperationQueue alloc]init];

            [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                NSString* content = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
                NSLog(@"%@", content);

                if (error == nil){

                    completionBlock(true, nil);
                }
                else
                {
                    completionBlock(false, error);
                }
            }];
        }
    }];
}
```

Jest to zgodne z tym samym wzorcem, ale również wprowadza ostatnią metodę, którą należy wdrożyć: `convertTaskToDictionary`. Powoduje to pobranie tablicy i przekształcenie jej w obiekt słownika. Ten obiekt jest łatwiej mutować do parametrów zapytania, które musisz przekazać do serwera. Kod jest prosty:

```
// Here we have some conversation helpers that allow us to parse passed items into dictionaries for URLEncoding later.

+(NSDictionary*) convertTaskToDictionary:(samplesTaskItem*)task
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

    if (task.itemName){
        [dictionary setValue:task.itemName forKey:@"task"];
    }

    return dictionary;
}

```

Następnie zapisz `deleteTask`:

```
+(void) deleteTask:(samplesTaskItem*)task
            parent:(UIViewController*) parent
   completionBlock:(void (^) (bool, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    SamplesApplicationData* data = [SamplesApplicationData getInstance];
    [self craftRequest:data.taskWebApiUrlString parent:parent completionHandler:^(NSMutableURLRequest* request, NSError* error){

        if (error != nil)
        {
            completionBlock(NO, error);
        }
        else
        {
            NSDictionary* taskInDictionaryFormat = [self convertTaskToDictionary:task];

            NSData* requestBody = [NSJSONSerialization dataWithJSONObject:taskInDictionaryFormat options:0 error:nil];

            [request setHTTPMethod:@"DELETE"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request setHTTPBody:requestBody];

            NSLog(@"%@", request);

            NSOperationQueue *queue = [[NSOperationQueue alloc]init];

            [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                NSString* content = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
                NSLog(@"%@", content);

                if (error == nil){

                    completionBlock(true, nil);
                }
                else
                {
                    completionBlock(false, error);
                }
            }];
        }
    }];
}
```

### Dodawanie funkcji wylogowania

Ostatnim etapem jest wdrożenie w aplikacji funkcji wylogowania się. To proste. W pliku `sampleWebApiConnector.m`:

```
+(void) signOut
{
    [authContext.tokenCacheStore removeAll:nil];

    NSHTTPCookie *cookie;

    NSHTTPCookieStorage *storage = [NSHTTPCookieStorage sharedHTTPCookieStorage];
    for (cookie in [storage cookies])
    {
        [storage deleteCookie:cookie];
    }
}
```

## Uruchamianie przykładowej aplikacji

Na koniec skompiluj i uruchom aplikację w środowisku Xcode. Zarejestruj się lub zaloguj w aplikacji i utwórz zadania dla zalogowanego użytkownika. Wyloguj się i zaloguj się ponownie jako inny użytkownik i utwórz zadania dla tego użytkownika.

Zwróć uwagę, że zadania są przechowywane dla poszczególnych użytkowników w interfejsie API, ponieważ wyodrębnia on tożsamość użytkownika z tokenu dostępu, który odbiera.

Cała próbka [jest dostępna w pliku .zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS/archive/complete.zip). Można ją także sklonować z serwisu GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS```

## Następne kroki

Możesz teraz przejść do bardziej zaawansowanych tematów dotyczących usługi B2C. Zobacz te tematy:

[Wywoływanie interfejsu API sieci Web w środowisku Node.js z aplikacji sieci Web dla środowiska Node.js]()

[Dostosowywanie środowiska użytkownika w aplikacji B2C]()



<!--HONumber=Jun16_HO2-->


