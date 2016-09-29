<properties
    pageTitle="Wprowadzenie do usługi Azure Mobile Engagement dla systemu iOS w języku Objective C | Microsoft Azure"
    description="Dowiedz się, jak używać usługi Azure Mobile Engagement razem z funkcją analizy i powiadomieniami wypychanymi na potrzeby aplikacji systemu iOS."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />


# Wprowadzenie do usługi Azure Mobile Engagement dla aplikacji systemu iOS w języku Objective C

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

W tym temacie pokazano, jak za pomocą usługi Azure Mobile Engagement określać użycie aplikacji i wysyłać powiadomienia wypychane do danego segmentu użytkowników aplikacji systemu iOS.
W tym samouczku zostanie utworzona pusta aplikacja systemu iOS służąca do zbierania podstawowych danych i odbierania powiadomień wypychanych przy użyciu systemu Apple Push Notification System (APNS).

Dla tego samouczka wymagane są następujące elementy:

+ Edytor XCode 8, który można zainstalować ze sklepu Mac App Store
+ Zestaw [Mobile Engagement iOS SDK]

Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usługi Mobile Engagement dla aplikacji systemu iOS.

> [AZURE.NOTE] Do wykonania kroków tego samouczka potrzebne jest aktywne konto platformy Azure. Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).

##<a id="setup-azme"></a>Konfigurowanie usługi Mobile Engagement dla aplikacji systemu iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>Łączenie aplikacji z zapleczem usługi Mobile Engagement

Ten samouczek przedstawia „podstawową integrację”, tj. minimalny zestaw wymagany do zbierania danych i wysyłania powiadomień wypychanych. Kompletna dokumentacja integracji znajduje się w sekcji [Mobile Engagement iOS SDK integration](mobile-engagement-ios-sdk-overview.md) (Integracja zestawu Mobile Engagement iOS SDK).

Aby zademonstrować integrację, utworzymy podstawową aplikację za pomocą edytora XCode.

###Tworzenie nowego projektu systemu iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###Łączenie aplikacji z zapleczem usługi Mobile Engagement

1. Pobierz zestaw [Mobile Engagement iOS SDK].
2. Wyodrębnij plik tar.gz do folderu na swoim komputerze.
3. Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Add files to** (Dodaj pliki do).

    ![][1]

4. Przejdź do folderu, w którym został wyodrębniony zestaw SDK, wybierz folder `EngagementSDK`, a następnie naciśnij przycisk **OK**.

    ![][2]

5. Otwórz kartę **Build Phases** (Fazy kompilacji) i w menu **Link Binary With Libraries** (Połącz plik binarny z bibliotekami) dodaj następujące platformy (patrz ilustracja):

    ![][3]

6. W przypadku edytora **XCode 7** — dodaj plik `libxml2.tbd` zamiast pliku `libxml2.dylib`.

7. Wróć do witryny Azure Portal na stronie **Informacje o połączeniu** w swojej aplikacji i skopiuj parametry połączenia.

    ![][4]

8. Dodaj następujący wiersz kodu w pliku **AppDelegate.m**.

        #import "EngagementAgent.h"

9. Wklej parametry połączenia w delegacie `didFinishLaunchingWithOptions`.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [...]
            //[EngagementAgent setTestLogEnabled:YES];
   
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
        }

10. `setTestLogEnabled` jest instrukcją opcjonalną, która umożliwia włączenie dzienników zestawu SDK w celu identyfikowania problemów. 

##<a id="monitor"></a>Włączanie monitorowania w czasie rzeczywistym

Aby rozpocząć wysyłanie danych i upewnić się, że użytkownicy są aktywni, konieczne jest wysłanie co najmniej jednego ekranu (Działanie) do zaplecza usługi Mobile Engagement.

1. Otwórz plik **ViewController.h** i zaimportuj element **EngagementViewController.h**:

    `# import "EngagementViewController.h"`

2. Następnie zastąp superklasę interfejsu **ViewController** elementem `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

##<a id="monitor"></a>Łączenie aplikacji z funkcją monitorowania w czasie rzeczywistym

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Włączanie powiadomień wypychanych i funkcji komunikatów w aplikacji

Usługa Mobile Engagement umożliwia wchodzenie w interakcję z użytkownikami i modułem REACH przy użyciu powiadomień wypychanych i komunikatów w aplikacji w kontekście kampanii. Ten moduł w portalu Mobile Engagement ma nazwę REACH.
W poniższych sekcjach aplikacja zostanie skonfigurowana do ich odbierania.

### Umożliwianie aplikacji otrzymywania cichych powiadomień wypychanych

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]  

### Dodawanie biblioteki Reach do projektu

1. Kliknij prawym przyciskiem myszy projekt.
2. Wybierz pozycję **Add file to** (Dodaj plik do).
3. Przejdź do folderu, w którym został wyodrębniony zestaw SDK.
4. Wybierz folder `EngagementReach`.
5. Kliknij pozycję **Dodaj**.

### Modyfikowanie delegata aplikacji

1. Wróć do pliku **AppDelegate.m** i zaimportuj moduł Reach usługi Engagement.

        #import "AEReachModule.h"

2. Wewnątrz metody `application:didFinishLaunchingWithOptions` utwórz moduł Reach i uwzględnij go w istniejącym wierszu inicjowania usługi Engagement:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

###Umożliwianie aplikacji otrzymywania powiadomień wypychanych usługi APNS

1. Dodaj następujący wiersz do metody `application:didFinishLaunchingWithOptions`:

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

2. Dodaj metodę `application:didRegisterForRemoteNotificationsWithDeviceToken` w następujący sposób:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }

3. Dodaj metodę `didFailToRegisterForRemoteNotificationsWithError` w następujący sposób:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           
           NSLog(@"Failed to get token, error: %@", error);
        }

4. Dodaj metodę `didReceiveRemoteNotification:fetchCompletionHandler` w następujący sposób:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png




<!--HONumber=Sep16_HO3-->


