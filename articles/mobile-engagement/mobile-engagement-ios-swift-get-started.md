<properties
    pageTitle="Wprowadzenie do usługi Azure Mobile Engagement dla aplikacji systemu iOS w języku Swift"
    description="Dowiedz się, jak używać usługi Azure Mobile Engagement z funkcją analizy i powiadomieniami wypychanymi na potrzeby aplikacji systemu iOS."
    services="mobile-engagement"
    documentationCenter="ios"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="swift"
    ms.topic="hero-article"
    ms.date="05/03/2016"
    ms.author="piyushjo" />

# Wprowadzenie do usługi Azure Mobile Engagement dla aplikacji systemu iOS w języku Swift

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

W tym temacie pokazano, jak za pomocą usługi Azure Mobile Engagement określać użycie aplikacji i wysyłać powiadomienia wypychane do danego segmentu użytkowników aplikacji systemu iOS.
W tym samouczku zostanie utworzona pusta aplikacja systemu iOS służąca do zbierania podstawowych danych i odbierania powiadomień wypychanych przy użyciu systemu Apple Push Notification System (APNS).

Dla tego samouczka wymagane są następujące elementy:

+ Edytor XCode 6 lub XCode 7, który można zainstalować ze sklepu Mac App Store
+ Zestaw [Mobile Engagement iOS SDK]
+ Certyfikat powiadomień wypychanych (p12), który można uzyskać z Centrum deweloperów firmy Apple

> [AZURE.NOTE] W tym samouczku jest używany język Swift w wersji 2.0. 

Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usługi Mobile Engagement dla aplikacji systemu iOS.

> [AZURE.NOTE] Do wykonania kroków tego samouczka potrzebne jest aktywne konto platformy Azure. Jeśli nie masz konta, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).

##<a id="setup-azme"></a>Konfigurowanie usługi Mobile Engagement dla aplikacji systemu iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>Łączenie aplikacji z zapleczem usługi Mobile Engagement

Ten samouczek przedstawia „podstawową integrację”, tj. minimalny zestaw wymagany do zbierania danych i wysyłania powiadomień wypychanych. Kompletna dokumentacja integracji znajduje się w sekcji [Mobile Engagement iOS SDK integration](mobile-engagement-ios-sdk-overview.md) (Integracja zestawu Mobile Engagement iOS SDK).

Aby zademonstrować integrację, zostanie utworzona podstawowa aplikacja za pomocą edytora XCode:

###Tworzenie nowego projektu systemu iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###Łączenie aplikacji z zapleczem usługi Mobile Engagement

1. Pobierz zestaw [Mobile Engagement iOS SDK].
2. Wypakuj plik tar.gz do folderu na swoim komputerze.
3. Kliknij prawym przyciskiem myszy projekt i wybierz pozycję „Add files to” (Dodaj pliki do).

    ![][1]

4. Przejdź do folderu, w którym został wypakowany zestaw SDK, wybierz folder `EngagementSDK`, a następnie naciśnij przycisk OK.

    ![][2]

5. Otwórz kartę `Build Phases` i w menu `Link Binary With Libraries` dodaj struktury w sposób pokazany poniżej. **UWAGA** Należy dołączyć elementy `CoreLocation, CFNetwork, CoreTelephony, and SystemConfiguration`:

    ![][3]

6. W przypadku edytora **XCode 7** — dodaj plik `libxml2.tbd` zamiast pliku `libxml2.dylib`.

7. Utwórz nagłówek mostkowania, aby można było używać interfejsów API języka Objective C zestawu SDK, wybierając pozycję File (Plik) > New (Nowy) > File (Plik) > iOS > Source (Źródło) > Header File (Plik nagłówka).

    ![][4]

8. Przeprowadź edycję pliku nagłówka mostkowania w celu udostępnienia kodu usługi Mobile Engagement w języku Objective C dla kodu w języku Swift, dodając następujące instrukcje importu:

        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEIdfaProvider.h"

9. W ustawieniach kompilacji upewnij się, że ustawienie kompilacji nagłówków mostkowania języka Objective-C w obszarze Swift Compiler (Kompilator języka Swift) — Code Generation (Generowanie kodu) zawiera ścieżkę do tego nagłówka. Oto przykład ścieżki: **$(SRCROOT)/MojaAplikacja/Nagłówek-Mostkowania-MojaAplikacja.h (zależne od ścieżki)**

    ![][6]

10. Wróć do portalu Azure na stronie *Informacje o połączeniu* swojej aplikacji i skopiuj parametry połączenia.

    ![][5]

11. Wklej parametry połączenia w delegacie `didFinishLaunchingWithOptions`.

        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool
        {
            [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
            [...]
        }

##<a id="monitor"></a>Włączanie monitorowania w czasie rzeczywistym

Aby rozpocząć wysyłanie danych i upewnić się, że użytkownicy są aktywni, konieczne jest wysłanie co najmniej jednego ekranu (Działanie) do zaplecza usługi Mobile Engagement.

1. Otwórz plik **ViewController.swift** i zmień klasę podstawową klasy **ViewController** na **EngagementViewController**:

    `class ViewController : EngagementViewController {`

##<a id="monitor"></a>Łączenie aplikacji z funkcją monitorowania w czasie rzeczywistym

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Włączanie powiadomień wypychanych i funkcji komunikatów w aplikacji

Usługa Mobile Engagement umożliwia interakcję z użytkownikami przy użyciu powiadomień wypychanych i komunikatów w aplikacji w kontekście kampanii. Ten moduł w portalu Mobile Engagement ma nazwę REACH.
Poniższe sekcje umożliwią skonfigurowanie aplikacji do ich odbierania.

### Umożliwianie aplikacji otrzymywania cichych powiadomień wypychanych

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### Dodawanie biblioteki Reach do projektu

1. Kliknij prawym przyciskiem projekt.
2. Wybierz pozycję `Add file to ...`
3. Przejdź do folderu, w którym został wypakowany zestaw SDK.
4. Wybierz folder `EngagementReach`. 
5. Kliknij pozycję Dodaj.
6. Przeprowadź edycję pliku nagłówka mostkowania w celu udostępnienia nagłówków Reach usługi Mobile Engagement w języku Objective C, dodając następujące instrukcje importu:

        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### Modyfikowanie delegata aplikacji

1. Wewnątrz elementu `didFinishLaunchingWithOptions` utwórz moduł Reach i przekaż go do istniejącego wiersza inicjowania usługi Engagement:

        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
            let reach = AEReachModule.moduleWithNotificationIcon(UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

###Umożliwianie aplikacji otrzymywania powiadomień wypychanych usługi APNS
1. Dodaj następujący wiersz do metody `didFinishLaunchingWithOptions`:

        /* Ask user to receive push notifications */
        if #available(iOS 8.0, *)
        {
           let settings = UIUserNotificationSettings(forTypes: [UIUserNotificationType.Alert, UIUserNotificationType.Badge, UIUserNotificationType.Sound], categories: nil)
           application.registerUserNotificationSettings(settings)
           application.registerForRemoteNotifications()
        }
        else
        {
           application.registerForRemoteNotificationTypes([UIRemoteNotificationType.Alert, UIRemoteNotificationType.Badge, UIRemoteNotificationType.Sound])
        }

2. Dodaj metodę `didRegisterForRemoteNotificationsWithDeviceToken` w następujący sposób:

        func application(application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: NSData)
        {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }

3. Dodaj metodę `didReceiveRemoteNotification:fetchCompletionHandler:` w następujący sposób:

        func application(application: UIApplication, didReceiveRemoteNotification userInfo: [NSObject : AnyObject], fetchCompletionHandler completionHandler: (UIBackgroundFetchResult) -> Void)
        {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png



<!--HONumber=Jun16_HO2-->


