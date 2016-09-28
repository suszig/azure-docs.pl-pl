<properties
    pageTitle="Azure Notification Hubs"
    description="Dowiedz się, jak używać powiadomień wypychanych na platformie Azure. Przykłady kodu napisane w języku C# z użyciem interfejsu API programu .NET."
    authors="wesmc7777"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="hero-article"
    ms.date="08/25/2016"
    ms.author="wesmc"/>



#Azure Notification Hubs

##Omówienie

Usługa Azure Notification Hubs zapewnia łatwą w użyciu, wieloplatformową infrastrukturę skalowaną w poziomie, która umożliwia wysyłanie powiadomień wysyłanych do urządzeń przenośnych z dowolnego zaplecza (lokalnego lub w chmurze) na dowolną platformę mobilną.

Usługa Notification Hubs umożliwia łatwe wysyłanie spersonalizowanych powiadomień wypychanych na różne platformy, zapewniając abstrakcyjność szczegółów różnych systemów powiadomień platformy (PNS, platform notification system). Przy użyciu jednego wywołania interfejsu API można wysyłać powiadomienia do poszczególnych użytkowników lub całych segmentów odbiorców zawierających miliony użytkowników na ich wszystkie urządzenia.

Usługi Notification Hubs można używać w scenariuszach przeznaczonych dla firm i konsumentów. Na przykład:

- Wysyłaj powiadomienia o najważniejszych wiadomościach do milionów osób z niskim opóźnieniem (usługa Notification Hubs działa w aplikacjach Bing wstępnie zainstalowanych na wszystkich urządzeniach z systemem Windows i Windows Phone).
- Wysyłaj kupony oparte na lokalizacji do segmentów użytkowników.
- Wysyłaj powiadomienia o zdarzeniach do użytkowników lub grup dla aplikacji sportowych i finansowych oraz gier.
- Powiadamiaj użytkowników o zdarzeniach dotyczących przedsiębiorstwa, takich jak nowe wiadomości lub wiadomości e-mail oraz potencjalnych klientach.
- Wysyłaj jednorazowe hasła wymagane do uwierzytelniania wieloskładnikowego.



##Co to są powiadomienia wypychane?

Smartfony i tablety mogą „powiadamiać” użytkowników o wystąpieniu zdarzenia. Te powiadomienia mogą mieć wiele form.

W aplikacjach ze Sklepu Windows i aplikacjach dla systemu Windows Phone te powiadomienia mogą mieć formę _powiadomienia wyskakującego_: wyświetlane jest niemodalne okno dialogowe i odtwarzany jest dźwięk sygnalizujący nowe powiadomienie. Obsługiwane są inne typy powiadomień, w tym powiadomienia _na kafelkach_, _nieprzetworzone_ i _na wskaźnikach_. Aby uzyskać więcej informacji o typach powiadomień obsługiwanych na urządzeniach z systemem Windows, zobacz [Tiles, Badges, and Notifications](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx) (Kafelki, wskaźniki i powiadomienia).

Na urządzeniach z systemem Apple iOS powiadomienie wypychane podobnie powiadamia użytkownika przy użyciu okna dialogowego z monitem o wyświetlenie lub zamknięcie powiadomienia. Kliknięcie pozycji **View** (Wyświetl) powoduje otwarcie aplikacji, która odebrała wiadomość. Aby uzyskać więcej informacji o powiadomieniach w systemie iOS, zobacz temat [iOS Notifications](http://go.microsoft.com/fwlink/?LinkId=615245) (Powiadomienia w systemie iOS).

Powiadomienia wypychane ułatwiają urządzeniom przenośnym wyświetlanie nowych informacji przy jednoczesnym oszczędzaniu energii. Powiadomienia mogą być wysyłane przez systemy zaplecza do urządzeń przenośnych nawet wtedy, gdy odpowiednie aplikacje na urządzeniach nie są aktywne. Powiadomienia wypychane są istotnym składnikiem aplikacji dla konsumentów, gdzie służą do zwiększania zaangażowania i użycia aplikacji. Powiadomienia są także przydatne dla przedsiębiorstw, ponieważ aktualne informacje zwiększają szybkość reagowania pracowników na wydarzenia w firmie.

Oto kilka przykładów marketingu na urządzeniach przenośnych:

1.  Aktualizowanie kafelka w systemie Windows 8 lub Windows Phone przy użyciu bieżących informacji finansowych.
2.  Powiadomienie użytkownika przy użyciu powiadomienia wyskakującego o przypisaniu elementu roboczego do tego użytkownika w aplikacji dla przedsiębiorstw opartej na przepływie pracy.
3.  Wyświetlenie wskaźnika z liczbą bieżących potencjalnych klientów w aplikacji CRM (takiej jak Microsoft Dynamics CRM).

##Sposób działania powiadomień wypychanych

Powiadomienia wypychane są dostarczane przy użyciu infrastruktur dla poszczególnych platform nazywanych _systemami powiadomień platformy_ (PNS, Platform Notification System). System powiadomień platformy zapewnia podstawowe funkcje (tj. nie obsługuje emisji i personalizacji) i nie ma wspólnego interfejsu. Na przykład aby wysłać powiadomienie do aplikacji ze Sklepu Windows, deweloper musi skontaktować się z usługą WNS (Windows Notification Service). Aby wysłać powiadomienie do urządzenia z systemem iOS, ten sam deweloper musi skontaktować się z usługą APNS (Apple Push Notification Service), a następnie wysłać wiadomość po raz drugi. Usługa Azure Notification Hubs pomaga, zapewniając wspólny interfejs oraz inne funkcje do obsługi powiadomień wypychanych na każdej platformie.

Jednak na wysokim poziomie wszystkie systemy powiadomień platformy działają według tego samego wzorca:

1.  Aplikacja kliencka kontaktuje się z systemem powiadomień platformy w celu pobrania jego _dojścia_. Typ dojścia zależy od systemu. W przypadku usługi WNS jest to identyfikator URI lub „kanał powiadomień”. W przypadku usługi APNs jest to token.
2.  Aplikacja kliencka przechowuje to dojście w _zapleczu_ aplikacji do późniejszego wykorzystania. W przypadku usługi WNS zapleczem jest zwykle usługa w chmurze. W przypadku usługi firmy Apple system jest nazywany _dostawcą_.
3.  W celu wysłania powiadomienia wypychanego zaplecze kontaktuje się z systemem powiadomień platformy przy użyciu dojścia do określonego wystąpienia aplikacji klienckiej.
4.  System powiadomień platformy przekazuje następnie powiadomienie do urządzenia określonego przez dojście.

![][0]

##Wyzwania związane z powiadomieniami wypychanymi

Te systemy są bardzo zaawansowane, ale deweloper aplikacji nadal musi wykonać dużo pracy, aby zaimplementować nawet typowe scenariusze dotyczące powiadomień wypychanych, takie jak emitowanie lub wysyłanie powiadomień wypychanych do segmentu użytkowników.

Powiadomienia wypychane są jedną z najbardziej pożądanych funkcji usług w chmurze dla aplikacji mobilnych. Jest to spowodowane tym, że infrastruktura wymagana do ich działania jest dosyć złożona i w większości niezwiązana z główną logiką biznesową aplikacji. Oto niektóre wyzwania związane z tworzeniem infrastruktury powiadomień wypychanych na żądanie:

- **Zależności dotyczące platformy.** Wysyłanie powiadomień do urządzeń na różnych platformach wymaga utworzenia kodu wielu interfejsów zaplecza. Różnice dotyczą nie tylko szczegółów niskiego poziomu, ale również prezentacji powiadomienia (kafelka, powiadomienia wyskakującego lub wskaźnika), która jest zależna od platformy. Te różnice mogą prowadzić do utworzenia złożonego i trudnego w obsłudze kodu.

- **Skalowanie.** Skalowanie tej infrastruktury ma następujące dwa aspekty:
    + Zgodnie z zaleceniami systemu powiadomień platformy tokeny urządzeń muszą być odświeżane przy każdym uruchomieniu aplikacji. Prowadzi to do dużego ruchu (i wynikającej z niego dużej liczby operacji uzyskiwania dostępu do bazy danych) w celu zachowania aktualności tokenów urządzeń. W przypadku zwiększenia liczby urządzeń (potencjalnie do milionów sztuk) koszt tworzenia i obsługi tej infrastruktury jest znaczący.

    + Większość systemów powiadomień platformy nie obsługuje emisji do wielu urządzeń. Z tego względu emisja do milionów urządzeń powoduje miliony wywołań do systemu powiadomień platformy. Zapewnienie możliwości skalowania tych żądań nie jest proste, ponieważ zwykle deweloperzy aplikacji chcą utrzymywać niskie opóźnienie całkowite. Na przykład ostatnie urządzenie odbierające wiadomość nie powinno otrzymać powiadomienia 30 minut po wysłaniu powiadomień, ponieważ w wielu przypadkach mija się to z celem powiadomień wypychanych.
- **Routing.** Systemy powiadomień platformy umożliwiają wysyłanie komunikatów do urządzeń. Jednak w przypadku większości aplikacji powiadomienia są przeznaczone dla użytkowników i/lub grup zainteresowań (na przykład wszystkich pracowników przypisanych do określonego konta klienta). W związku z tym, aby można było kierować powiadomienia do odpowiednich urządzeń, zaplecze aplikacji musi zapewniać rejestr zawierający skojarzenia grup zainteresowań z tokenami urządzeń. Ten narzut zwiększa całkowity czas wprowadzenia na rynek oraz koszty obsługi aplikacji.

##Dlaczego warto używać usługi Notification Hubs?

Usługa Notification Hubs eliminuje złożoność: nie trzeba zarządzać wyzwaniami związanymi z powiadomieniami wypychanymi. Zamiast tego można użyć centrum powiadomień. Usługa Notification Hubs korzysta z pełnej, wieloplatformowej infrastruktury powiadomień wypychanych skalowanej w poziomie i znacznie zmniejsza ilość kodu dotyczącego powiadomień wypychanych uruchamianego w zapleczu aplikacji. Usługa Notification Hubs implementuje całą funkcjonalność infrastruktury powiadomień wypychanych. Urządzenia są odpowiedzialne wyłącznie za rejestrację dojść systemu powiadomień platformy, a zaplecze odpowiada za wysyłanie komunikatów niezależnych od platformy do użytkowników lub grup zainteresowań, jak przedstawiono na poniższej ilustracji:

![][1]


Usługa Notification Hubs zapewnia gotową do użycia infrastrukturę powiadomień wypychanych oferującą następujące korzyści:

- **Wiele platform.**
    +  Obsługa wszystkich głównych platform urządzeń przenośnych. Usługa Notification Hubs umożliwia wysyłanie powiadomień wypychanych do aplikacji ze Sklepu Windows oraz aplikacji dla systemu iOS, Android i Windows Phone.

    +  Usługa Notification Hubs zapewnia wspólny interfejs do wysyłania powiadomień do wszystkich obsługiwanych platform. Protokoły dla poszczególnych platform nie są wymagane. Zaplecze aplikacji może wysyłać powiadomienia w formatach przeznaczonych dla określonych platform lub w formacie niezależnym od platformy. Aplikacja komunikuje się wyłącznie z usługą Notification Hubs.

    +  Zarządzanie dojściami urządzeń. Usługa Notification Hubs przechowuje rejestr dojść i informacji zwrotnych z systemów powiadomień platformy.

- **Działa z każdym zapleczem**: w środowisku w chmurze lub lokalnym, .NET, PHP, Java, Node itd.

- **Skalowanie.** Usługa Notification Hubs umożliwia skalowanie do milionów urządzeń bez konieczności ponownego projektowania lub fragmentacji.


- **Bogaty zestaw wzorców dostarczania**:

    - *Emisja*: możliwość niemal jednoczesnej emisji do milionów urządzeń przy użyciu jednego wywołania interfejsu API.

    - *Emisja pojedyncza lub multiemisja*: wypychanie do tagów reprezentujących poszczególnych użytkowników, w tym wszystkie ich urządzenia, lub do szerszej grupy, na przykład oddzielnych typów urządzeń (tablet lub telefon).

    - *Segmentacja*: wypychanie do złożonego segmentu zdefiniowanego przy użyciu wyrażeń tagów (na przykład urządzenia w Warszawie obserwujące wydarzenia dotyczące Legii).

    Każde urządzenie podczas wysyłania dojścia do centrum powiadomień może określić jeden lub więcej _tagów_. Więcej informacji na temat [tagów]. Tagów nie trzeba wstępnie tworzyć ani usuwać. Tagi umożliwiają proste wysyłanie powiadomień do użytkowników lub grup zainteresowań. Tagi mogą zawierać dowolny identyfikator specyficzny dla aplikacji (na przykład identyfikator użytkownika lub grupy), dlatego ich użycie zwalnia zaplecze aplikacji z konieczności przechowywania dojść urządzeń i zarządzania nimi.

- **Personalizacja**: poszczególne urządzenia mogą mieć jeden lub więcej szablonów w celu uzyskania możliwości lokalizacji i personalizacji bez wpływu na kod zaplecza.

- **Zabezpieczenia**: uwierzytelnianie przy użyciu klucza tajnego dostępu współdzielonego (SAS, Shared Access Secret) lub uwierzytelnianie federacyjne.

- **Rozbudowane informacje telemetryczne**: dostępne w portalu i programowo.


##Integracja z usługą App Service Mobile Apps

W celu umożliwienia bezproblemowej i jednorodnej obsługi we wszystkich usługach Azure funkcja [App Service Mobile Apps] ma wbudowaną obsługę powiadomień wypychanych przy użyciu usługi Notification Hubs. Funkcja [App Service Mobile Apps] oferuje wysoce skalowalną, globalnie dostępną platformę tworzenia aplikacji mobilnych dla deweloperów w przedsiębiorstwach i integratorów systemów. Platforma ta oferuje bogaty zestaw funkcji dla deweloperów aplikacji mobilnych.

Deweloperzy aplikacji mobilnych mogą korzystać z usługi Notification Hubs przy użyciu następującego przepływu pracy:

1. Pobranie dojścia do urządzenia w systemie powiadomień platformy
2. Rejestracja urządzenia i [szablonów] w usłudze Notification Hubs przy użyciu wygodnego interfejsu API rejestracji w ramach zestawu SDK klienta usługi Mobile Apps
    + Zauważ, że usługa Mobile Apps usuwa wszystkie tagi rejestracji ze względów bezpieczeństwa. Pracuj z usługą Notification Hubs bezpośrednio z zaplecza, aby skojarzyć tagi z urządzeniami.
3. Wysyłanie powiadomień z zaplecza aplikacji przy użyciu usługi Notification Hubs

Oto niektóre udogodnienia dla deweloperów wynikające z tej integracji:

- **Zestawy SDK klienta usługi Mobile Apps** Te wieloplatformowe zestawy SDK zapewniają proste interfejsy API do rejestracji i komunikacji z centrum powiadomień automatycznie skojarzone z aplikacją mobilną. Deweloperzy nie muszą odnajdywać poświadczeń usługi Notification Hubs i pracować przy użyciu dodatkowych usług.
    + Zestawy SDK automatycznie dodają tagi dla danego urządzenia przy użyciu identyfikatora uwierzytelnionego użytkownika usługi Mobile Apps w celu umożliwienia scenariusza wypychania do użytkownika.
    + Zestawy SDK automatycznie używają identyfikatora instalacji usługi Mobile Apps jako identyfikatora GUID do rejestracji w usłudze Notification Hubs, oszczędzając deweloperom pracy związanej z obsługą identyfikatorów GUID wielu usług.
    
- **Model instalacji.** Usługa Mobile Apps współpracuje z najnowszym modelem wypychania usługi Notification Hubs w celu reprezentowania wszystkich właściwości wypychania skojarzonych z urządzeniem w instalacji JSON, które są zgodne z usługami powiadomień wypychanych i łatwe w użyciu.

- **Elastyczność.** Deweloperzy mogą zawsze pracować bezpośrednio za pomocą usługi Notification Hubs nawet po integracji.

- **Zintegrowane środowisko w witrynie [Azure Portal].** Wypychanie jako możliwość ma wizualną reprezentację w usłudze Mobile Apps, a deweloperzy mogą z łatwością pracować przy użyciu skojarzonego centrum powiadomień za pomocą usługi Mobile Apps.



##Następne kroki

Więcej informacji na temat usługi Notification Hubs można znaleźć w następujących tematach:

+ **[W jaki sposób klienci używają usługi Notification Hubs]**

+ **[Przewodniki i samouczki dotyczące usługi Notification Hubs]**

+ **Samouczki dotyczące rozpoczynania pracy z usługą Notification Hubs** ([iOS], [Android], [aplikacje uniwersalne systemu Windows], [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android])

Dokumentacja zarządzanego interfejsu API programu .NET dla powiadomień wypychanych jest dostępna tutaj:

+ [Microsoft.WindowsAzure.Messaging.NotificationHub]
+ [Microsoft.ServiceBus.Notifications]


  [0]: ./media/notification-hubs-overview/registration-diagram.png
  [1]: ./media/notification-hubs-overview/notification-hub-diagram.png
  [W jaki sposób klienci używają usługi Notification Hubs]: http://azure.microsoft.com/services/notification-hubs
  [Przewodniki i samouczki dotyczące usługi Notification Hubs]: http://azure.microsoft.com/documentation/services/notification-hubs
  [iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
  [Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
  [aplikacje uniwersalne systemu Windows]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
  [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
  [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
  [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
  [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
  [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
  [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
  [App Service Mobile Apps]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
  [szablonów]: notification-hubs-templates-cross-platform-push-messages.md
  [Azure Portal]: https://portal.azure.com
  [tagów]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)



<!--HONumber=Sep16_HO3-->


