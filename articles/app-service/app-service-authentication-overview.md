---
title: "Uwierzytelnianie i autoryzację w usłudze Azure App Service | Dokumentacja firmy Microsoft"
description: "Odwołanie koncepcyjne i omówienie uwierzytelniania / autoryzacji funkcji Azure App Service"
services: app-service
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/29/2016
ms.author: mahender
ms.openlocfilehash: f0d2644903181cd2e20166feae4f90ddd4037fa8
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="authentication-and-authorization-in-azure-app-service"></a>Uwierzytelnianie i autoryzacja w usłudze Azure App Service
## <a name="what-is-app-service-authentication--authorization"></a>Co to jest aplikacja usługi uwierzytelniania / autoryzacji?
Aplikacja usługi uwierzytelniania / autoryzacji jest funkcją, która umożliwia aplikacji do logowania użytkowników, dzięki czemu nie trzeba zmienić kodu w zapleczu aplikacji. Zapewnia prosty sposób ochrona aplikacji i pracować z danymi użytkownika.

Tożsamości federacyjnych, w którym dostawca tożsamości innych firm przechowuje konta i uwierzytelnia użytkowników korzysta z usługi aplikacji. Aplikacja zależy od informacji o dostawcy tożsamości, aby aplikacja nie ma do przechowywania informacji o tej samej. App Service obsługuje pięć dostawców tożsamości fabrycznej: Azure Active Directory, Facebook, Google, Microsoft Account i Twitter. Aplikację można używać dowolnej liczby tych dostawców tożsamości zapewnić użytkownikom z opcjami sposób logowania się w. Aby rozszerzyć wbudowanych funkcji, można je zintegrować innego dostawcy tożsamości lub [rozwiązania tożsamość niestandardowa][custom-auth].

Jeśli chcesz rozpocząć od razu, zobacz jedną z następujących samouczków [Dodawanie uwierzytelniania do aplikacji systemu iOS] [ iOS] (lub [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [platformy Xamarin.Forms], lub [Cordova]).

## <a name="how-authentication-works-in-app-service"></a>Jak działa uwierzytelniania w usłudze App Service
W celu uwierzytelniania za pomocą jednego z dostawców tożsamości, należy najpierw skonfigurować dostawcy tożsamości, aby dowiedzieć się o aplikacji. Dostawca tożsamości będzie dostarczać identyfikatory i hasła, które świadczą usługi aplikacji. Na tym kończy relacji zaufania, aby sprawdzić poprawność potwierdzenia użytkownika, takich jak tokeny uwierzytelniania przez dostawcę tożsamości usługi aplikacji.

Aby zalogować użytkownika przy użyciu jednej z tych dostawców, użytkownik musi zostać przekierowany do punktu końcowego, który loguje się użytkowników dla tego dostawcy. Jeśli klienci używają przeglądarki sieci web, może mieć usługi aplikacji — automatyczne kierowanie nieuwierzytelnionym użytkownikom punktu końcowego, który loguje się użytkownicy. W przeciwnym razie musisz przekierować klientów do `{your App Service base URL}/.auth/login/<provider>`, gdzie `<provider>` jest jednym z następujących wartości: aad, facebook, google, microsoft lub twitter. Scenariusze mobilnych i interfejsu API opisano szczegółowo w sekcji w dalszej części tego artykułu.

Użytkownicy, którzy korzystają z Twojej aplikacji za pośrednictwem przeglądarki sieci web ma plik cookie skonfigurować tak, aby może pozostawać uwierzytelniony jako przeglądania aplikacji. Dla innych typów klientów, takie jak telefon komórkowy, tokenu web JSON (JWT), który będzie wyświetlany w `X-ZUMO-AUTH` nagłówka, będą wystawiane do klienta. Klient Mobile Apps SDK obsłuży to dla Ciebie. Alternatywnie token tożsamości usługi Azure Active Directory lub token dostępu może być bezpośrednio zawarta w `Authorization` nagłówek jako [tokenu elementu nośnego](https://tools.ietf.org/html/rfc6750).

Usługi aplikacji — zostanie przeprowadzona Weryfikacja dowolnego pliku cookie lub token, który wystawia aplikacji do uwierzytelniania użytkowników. Aby ograniczyć, kto ma dostęp do aplikacji, zobacz [autoryzacji](#authorization) sekcji w dalszej części tego artykułu.

### <a name="mobile-authentication-with-a-provider-sdk"></a>Przenośne uwierzytelniania za pomocą dostawcy zestawu SDK
Po wszystko, co jest skonfigurowany do wewnętrznej bazy danych, można zmodyfikować klientów mobilnych, aby zalogować się przy użyciu usługi aplikacji. Istnieją dwie metody w tym miejscu:

* Korzystanie z zestawu SDK, publikującej dostawcy tożsamości danego do ustalenia tożsamości i uzyskać dostęp do usługi App Service.
* Użyj pojedynczy wiersz kodu tak, aby zalogować się użytkowników klienta Mobile Apps SDK.

> [!TIP]
> Większość aplikacji należy używać dostawcy zestawu SDK, aby uzyskać bardziej spójne środowisko podczas logowania, z obsługą odświeżania, a także aby uzyskać inne korzyści, które określa dostawcę.
> 
> 

Gdy używasz dostawcy SDK, można logowania użytkownika do bardziej ściśle zintegrowana z systemem operacyjnym, że aplikacja jest uruchomiona na środowisko. Zapewnia to również token dostawcy i niektóre informacje użytkownika na komputerze klienckim, co pozwala na znacznie łatwiejsze do wykorzystania wykres interfejsów API i dostosowanie środowiska użytkownika. Od czasu do czasu na blogi i fora, wyświetlona zostanie to określane jako "przebieg klienta" lub "przepływ skierowane do klienta", ponieważ kod na kliencie logowania użytkowników i kod klienta ma dostęp do tokenu dostawcy.

Po uzyskaniu tokenu dostawcy musi wysłane do aplikacji usługi do sprawdzania poprawności. Po usługi aplikacji sprawdza poprawność tokenu, usługa aplikacji tworzy nowy token usługi aplikacji, która jest zwracana do klienta. Klient Mobile Apps SDK ma metody pomocnicze do zarządzania tym exchange i automatycznie dołączyć token do wszystkich żądań do zaplecza aplikacji. Deweloperzy mogą również zachować odwołania do tokenu dostawcy, jeśli więc wybrać.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Przenośne uwierzytelniania bez dostawcy zestawu SDK
Jeśli nie chcesz skonfigurować dostawcy SDK, można zezwolić funkcji Mobile Apps usługi aplikacji Azure do logowania dla Ciebie. Klient Mobile Apps SDK Otwórz widok sieci web dostawcy wybrane i zalogować użytkownika. Od czasu do czasu na blogi i fora, zobaczysz to określane jako "serwera przepływ" lub "przepływ skierowane do serwera", ponieważ serwer zarządza procesem, który loguje się użytkowników i zestawu SDK klienta nigdy nie otrzyma tokenu dostawcy.

Kod, aby uruchomić ten przepływ znajduje się w samouczku uwierzytelniania dla każdej platformy. Na końcu przepływu klienta SDK ma token usługi aplikacji, a token są automatycznie dołączane do wszystkich żądań do wewnętrznej bazy danych aplikacji.

### <a name="service-to-service-authentication"></a>Uwierzytelnianie między usługami
Mimo że można udzielać użytkownikom dostępu do aplikacji, można również ufać inną aplikację do wywołania własnego interfejsu API. Na przykład można mieć jednej aplikacji sieci web wywołania interfejsu API w innej aplikacji sieci web. W tym scenariuszu można używać poświadczeń dla konta usługi zamiast poświadczeń użytkownika do pobrania tokenu. Konto usługi jest także znana jako *nazwy głównej usługi* w terminologii usługi Azure Active Directory i uwierzytelniania przy użyciu tego konta jest także znana jako scenariusza service-to-service.

> [!IMPORTANT]
> Ponieważ aplikacje mobilne są uruchamiane na urządzeniach klienckich, czy aplikacje mobilne *nie* są liczone jako zaufane aplikacje i nie należy używać przepływu główną usługi. Zamiast tego powinny używać przepływu użytkownika, który został wcześniej szczegółowe.
> 
> 

W przypadku scenariuszy usług usługi aplikacji może chronić aplikacji za pomocą usługi Azure Active Directory. Aplikacja wywołująca musi tylko podać uzyskany przez identyfikator i klienta w usłudze Azure Active Directory, który dostarcza klientowi token autoryzacji podmiotu zabezpieczeń usługi Azure Active Directory. Przykładem tego scenariusza, który korzysta z aplikacji interfejsu API platformy ASP.NET tłumaczy samouczek [uwierzytelnianie główny dla usługi dla aplikacji interfejsu API] [apia usługa].

Jeśli chcesz użyć uwierzytelniania usługi aplikacji do obsługi scenariusza service-to-service, można użyć uwierzytelniania podstawowego lub certyfikatów klienta. Aby uzyskać informacje o certyfikatach klientów na platformie Azure, zobacz [jak do konfigurowania protokołu TLS wzajemnego uwierzytelniania dla aplikacji sieci Web](app-service-web-configure-tls-mutual-auth.md). Aby dowiedzieć się, uwierzytelnianie podstawowe w programie ASP.NET, zobacz [filtry uwierzytelniania w ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Uwierzytelnianie konta usługi z usługi aplikacji logiki aplikacji w aplikacji interfejsu API to specjalny przypadek została szczegółowo opisana w [przy użyciu niestandardowego interfejsu API hostowanych w usłudze aplikacji z usługą Logic apps](../logic-apps/logic-apps-custom-hosted-api.md).

## <a name="authorization"></a>Jak działa autoryzacji w usłudze App Service
Masz pełną kontrolę nad żądań, które można uzyskać dostępu do aplikacji. Aplikacja usługi uwierzytelniania / autoryzacji można skonfigurować za pomocą dowolnej z następujących problemów:

* Zezwalaj tylko na uwierzytelniony żądania do aplikacji.
  
    Jeśli przeglądarka wysyła żądania od użytkowników anonimowych, usługi aplikacji przekieruje do strony wybranego przez użytkownika, dzięki czemu użytkownicy mogą rejestrować dostawcy tożsamości. Jeśli żądanie pochodzi z urządzenia przenośnego, zwrócona odpowiedź jest HTTP *401 nieautoryzowane* odpowiedzi.
  
    Dzięki tej opcji nie trzeba w ogóle pisania kodu uwierzytelniania w aplikacji. Aby uzyskać bardziej precyzyjną autoryzacji, informacje o użytkowniku jest dostępna w kodzie.
* Zezwalaj na wszystkie żądania kierowane do osiągnięcia aplikacji, ale poprawności żądań uwierzytelnionych i przekazują informacje o uwierzytelnianiu w nagłówkach HTTP.
  
    Opcja ta różni się decyzji dotyczących autoryzacji w kodzie aplikacji. Większa elastyczność w obsłudze żądań anonimowych, ale trzeba napisać kod.
* Zezwalaj na wszystkie żądania do aplikacji, a nie podejmuj żadnej akcji na informacje dotyczące uwierzytelniania w żądaniach.
  
    W tym przypadku uwierzytelnianie / autoryzacja funkcja jest wyłączona. Zadania uwierzytelniania i autoryzacji są całkowicie zależy kod aplikacji.

Poprzednie zachowania są kontrolowane przez **działania należy podjąć w przypadku nieuwierzytelnionego żądania** opcji w portalu Azure. Jeśli wybierzesz ** Zaloguj się przy użyciu *Nazwa dostawcy* **, wszystkie żądania muszą zostać uwierzytelnione. **Zezwalaj na żądanie (żadnej akcji)** różni się decyzję dotyczącą autoryzacji w kodzie, ale nadal zawiera informacje dotyczące uwierzytelniania. Jeśli chcesz, aby obsłużyć wszystkie elementy kodu, możesz wyłączyć uwierzytelnianie / autoryzacja funkcji.

## <a name="working-with-user-identities-in-your-application"></a>Praca z tożsamościami użytkowników w aplikacji
Usługi aplikacji przekazuje niektóre informacje użytkownika do aplikacji za pomocą specjalnych nagłówków. Zewnętrzne żądań Zabroń używania tych nagłówków i zostanie tylko jeśli obecne ustawiona przez aplikację usługi uwierzytelniania / autoryzacji. Niektóre nagłówki przykład obejmują:

* X-MS-KLIENTA-— NAZWA GŁÓWNA
* X-MS-CLIENT-PRINCIPAL-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Kod napisany w dowolnego języka lub platformy można uzyskać informacji wymaganych z tych nagłówków. Dla aplikacji platformy ASP.NET 4.6 **ClaimsPrincipal** jest ustawiany automatycznie z odpowiednimi wartościami.

Aplikację można również uzyskać szczegóły użytkownika dodatkowe za pośrednictwem HTTP GET na `/.auth/me` punkt końcowy aplikacji. Nieprawidłowy token, który wchodzi w skład żądania zwróci ładunek JSON z szczegółowe informacje o dostawcy, który jest używany, podstawowego tokenu dostawcy i inne informacje użytkownika. Serwer Mobile Apps SDK zapewnia metody pomocnicze do pracy z tych danych. Aby uzyskać więcej informacji, zobacz [sposobu korzystania z zestawem Azure Mobile Apps Node.js SDK](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity), i [pracować z serwera wewnętrznej bazy danych .NET SDK usługi Azure Mobile Apps](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Dokumentacja i dodatkowe zasoby
### <a name="identity-providers"></a>Dostawców tożsamości
Następujące samouczki przedstawiają sposób konfigurowania aplikacji usługi do używania uwierzytelniania różnych dostawców:

* [Jak skonfigurować aplikację do używania nazwy logowania w usłudze Azure Active Directory][AAD]
* [Jak skonfigurować aplikację do używania logowania usługi Facebook][Facebook]
* [Jak skonfigurować aplikację do używania logowania Google][Google]
* [Jak skonfigurować aplikację do używania Account Microsoft logowania][MSA]
* [Jak skonfigurować aplikację do używania logowania usługi Twitter][Twitter]

Jeśli chcesz użyć innych niż system obsługi tożsamości, wprowadzone w tym miejscu można również użyć [obsługę niestandardowych uwierzytelniania na serwerze Mobile Apps .NET SDK w wersji preview][custom-auth], której można użyć w aplikacji sieci web, mobilnych aplikacje, lub aplikacje interfejsu API.

### <a name="mobile-applications"></a>Aplikacje mobilne
Następujące samouczki przedstawiają sposób dodawania uwierzytelniania dla klientów mobilnych przy użyciu przepływu skierowane do serwera:

* [Dodawanie uwierzytelniania do aplikacji systemu iOS][iOS]
* [Dodawanie uwierzytelniania do aplikacji systemu Android][Android]
* [Dodawanie uwierzytelniania do aplikacji systemu Windows][Windows]
* [Dodawanie uwierzytelniania do aplikacji platformy Xamarin.iOS][Xamarin.iOS]
* [Dodawanie uwierzytelniania do aplikacji platformy Xamarin.Android][Xamarin.Android]
* [Dodawanie uwierzytelniania do aplikacji platformy Xamarin.Forms][platformy Xamarin.Forms]
* [Dodawanie uwierzytelniania do aplikacji platformy Cordova][Cordova]

Jeśli chcesz użyć przepływu skierowane do klienta usługi Azure Active Directory, użyj następujących zasobów:

* [Użyj biblioteki uwierzytelniania usługi Active Directory dla systemu iOS][ADAL-iOS]
* [Korzystanie z biblioteki uwierzytelniania usługi Active Directory dla systemu Android][ADAL-Android]
* [Użyj biblioteki uwierzytelniania usługi Active Directory systemu Windows i Xamarin][ADAL-dotnet]

Jeśli chcesz używać przepływu skierowane do klienta dla usługi Facebook, użyj następujących zasobów:

* [Używanie zestawu SDK usługi Facebook dla systemu iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Jeśli chcesz używać przepływu skierowane do klienta dla usługi Twitter, użyj następujących zasobów:

* [Użyj usługi Twitter sieci szkieletowej dla systemu iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Jeśli chcesz użyć dla usług Google przepływu skierowane do klienta, należy użyć następujących zasobów:

* [Korzystanie z usługi Google logowania zestawu SDK dla systemu iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

<!-- ### API applications
The following tutorials show how to protect your API apps:

* [User authentication for API Apps in Azure App Service][apia-user]
* [Service principal authentication for API Apps in Azure App Service][apia-service] -->

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[platformy Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: app-service-mobile-how-to-configure-google-authentication.md
[MSA]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
