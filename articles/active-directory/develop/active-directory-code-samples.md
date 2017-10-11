---
title: "Przykłady kodu usługi Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Indeks usługi Azure Active Directory, przykłady kodu, uporządkowane według scenariusza."
services: active-directory
documentationcenter: dev-center-name
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: mbaldwin
ms.custom: aaddev
ms.openlocfilehash: 19d71e65dc2c78663d94c74228dcff038566391b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-code-samples"></a>Przykłady kodu usługi Azure Active Directory
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Microsoft Azure Active Directory (Azure AD) umożliwia dodawanie uwierzytelniania i autoryzacji do aplikacji sieci web i interfejsów API sieci web. W tej sekcji podano łącza do przykłady, które pokazują, jak jest wykonywane i wstawki kodu, których można użyć w aplikacji. Na stronie przykładowy kod znajdują się szczegółowe odczytu-mnie tematy ułatwiające wymagania, instalacji i konfiguracji. I kod jest oznaczone jako ułatwią zrozumienie sekcje krytyczne.

Aby poznać podstawowy scenariusz dla każdego typu przykładowe, zobacz scenariusze uwierzytelniania dla usługi Azure AD.

Współtworzenie naszej próbki w witrynie GitHub: [Microsoft Azure Active Directory przykłady i dokumentacja](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-to-web-application"></a>Przeglądarki sieci Web do aplikacji sieci Web
Te przykłady pokazują, jak napisać aplikację sieci web kierujący przeglądarki użytkownika do logowania do usługi Azure AD.

| Język/Platform | Przykład | Opis |
| --- | --- | --- |
| C# / .NET |[Aplikacja sieci Web OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) |Użyj protokołu OpenID Connect (ASP.Net OpenID Connect OWIN oprogramowanie pośredniczące) do uwierzytelniania użytkowników w dzierżawie usługi Azure AD. |
| C# / .NET |[Aplikacja sieci Web wielodostępnej OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) |Wielodostępne aplikacji sieci web .NET MVC, która używa protokołu OpenID Connect (ASP.Net OpenID Connect OWIN oprogramowanie pośredniczące) do uwierzytelniania użytkowników z wieloma dzierżawcami usługi Azure AD. |
| C# / .NET |[Aplikacja sieci Web WSFederation-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) |Użyj WS-Federation (oprogramowanie pośredniczące OWIN WS-Federation ASP.Net) do uwierzytelniania użytkowników w dzierżawie usługi Azure AD. |

## <a name="single-page-application-spa"></a>Jednostronicowej aplikacji JEDNOSTRONICOWEJ
Ten przykład przedstawia sposób tworzenia aplikacji jednej strony zabezpieczonego za pomocą usługi Azure AD.  

| Język/Platform | Przykład | Opis |
| --- | --- | --- |
| JavaScript, C# / .NET |[SinglePageApp DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) |ADAL dla języka JavaScript i Azure AD umożliwia zabezpieczanie realizowana za pomocą programu ASP.NET web API zaplecza aplikacji na podstawie AngularJS jednej strony. |

## <a name="native-application-to-web-api"></a>Aplikacji natywnej do interfejsu API sieci Web
Te przykłady kodu pokazują, jak tworzyć aplikacje klientami, które wywołania interfejsów API, które są zabezpieczone przez usługę Azure AD w sieci web. Używają [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) i [OAuth 2.0 w usłudze Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Język/Platform | Przykład | Opis |
| --- | --- | --- |
| Javascript |[NativeClient-MultiTarget-Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) |Umożliwia tworzenie aplikacji oprogramowania Apache Cordova, który wywołuje interfejs API sieci web i używa usługi Azure AD do uwierzytelniania ADAL wtyczki dla oprogramowania Apache Cordova. |
| C# / .NET |[NativeClient DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) |Aplikacji programu .NET WPF, która wywołuje interfejs API sieci web, która jest chroniona za pomocą usługi Azure AD. |
| C# / .NET |[NativeClient WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) |Aplikacji Sklepu Windows, która wywołuje interfejs API sieci web, który jest zabezpieczony z usługą Azure AD. |
| C# / .NET |[NativeClient-WebAPI wielodostępnej WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) |Wywoływanie składnika web API, który jest zabezpieczony z usługą Azure AD wielodostępne aplikację ze Sklepu Windows. |
| C# / .NET |[WebAPI-OnBehalfOf-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) |Aplikacja klientami, która wywołuje interfejs API, który pobiera token działać w imieniu dla oryginalnego użytkownika, a następnie używa tokenu do wywoływania innego interfejsu API sieci web w sieci web. |
| C# / .NET |[NativeClient WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) |Aplikacją ze Sklepu Windows dla Windows Phone 8.1, która wywołuje interfejs API sieci web, która jest chroniona przez usługę Azure AD. |
| ObjC |[NativeClient iOS](https://github.com/Azure-Samples/active-directory-ios) |Aplikacja systemu iOS, która wywołuje interfejs API sieci web, która wymaga usługi Azure AD do uwierzytelniania. |
| C# / .NET |[WebAPI-ManuallyValidateJwt-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) |Aplikacja native klienta, która zawiera logikę do przetwarzania tokenu JWT w składniku web API, zamiast używać oprogramowania pośredniczącego OWIN. |
| C# / Xamarin |[NativeClient-Xamarin-Android](https://github.com/Azure-Samples/active-directory-xamarin-android) |Wiązanie Xamarin do natywnej Azure AD Authentication Library (ADAL) dla biblioteki systemu Android. |
| C# / Xamarin |[NativeClient Xamarin systemu iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) |Wiązanie Xamarin do natywnej Azure AD Authentication Library (ADAL) dla systemu iOS. |
| C# / Xamarin |[NativeClient-MultiTarget-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) |Projekt Xamarin, który jest przeznaczony dla platformy pięć i wywołuje interfejs API sieci web, która jest chroniona przez usługę Azure AD. |
| C# / .NET |[NativeClient bezobsługowe DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) |Aplikacji natywnej, który przeprowadza uwierzytelnianie nieinteraktywne i wywołuje interfejs API sieci web, która jest chroniona przez usługę Azure AD. |

## <a name="web-application-to-web-api"></a>Aplikacja sieci Web do interfejsu API sieci Web
Te przykłady kodu, jak używać Pokaż [OAuth 2.0 w usłudze Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) do tworzenia aplikacji sieci web tego wywołania interfejsów API sieci web, które są zabezpieczone przez usługę Azure AD.

| Język/Platform | Przykład | Opis |
| --- | --- | --- |
| C# / .NET |[Aplikacja sieci Web WebAPI-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) |Wywoływanie interfejsu API sieci web z uprawnieniami zalogowanego użytkownika. |
| C# / .NET |[Aplikacja sieci Web WebAPI-OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) |Wywoływanie interfejsu API sieci web z uprawnieniami aplikacji. |
| C# / .NET |[Aplikacja sieci Web WebAPI-OAuth2-tożsamości użytkownika Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) |Dodaj autoryzacji z [OAuth 2.0 w usłudze Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) do istniejącej aplikacji sieci web, może wywołać interfejs API sieci web. |
| JavaScript |[WebAPI Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) |Konfigurowanie usługi interfejsu API REST, który jest zintegrowany z usługą Azure AD dla interfejsu API ochrony. Zawiera serwer Node.js interfejs API sieci Web. |
| C# / .NET |[Aplikacja sieci Web WebAPI wielodostępnej OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |Wielodostępne MVC sieci web aplikacji, która używa protokołu OpenID Connect (ASP.Net OpenID Connect OWIN oprogramowanie pośredniczące) do uwierzytelniania użytkowników w dzierżawie usługi Azure AD. Korzysta z kodu autoryzacji do wywołania interfejsu API programu Graph. |

## <a name="server-or-daemon-application-to-web-api"></a>Serwera lub demon aplikacji interfejsu API sieci Web
Te przykłady kodu przedstawiają sposób tworzenia demon lub serwera aplikacji, która pobiera zasoby z interfejsu API sieci web przy użyciu [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) i [OAuth 2.0 w usłudze Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Język/Platform | Przykład | Opis |
| --- | --- | --- |
| C# / .NET |[Demon DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) |Aplikacja konsolowa wywołuje interfejs API sieci web. Poświadczenie klienta jest hasłem. |
| C# / .NET |[Demon CertificateCredential-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) |Aplikacja konsolowa, która wywołuje interfejs API sieci web. Poświadczenie klienta jest certyfikat. |

## <a name="calling-azure-ad-graph-api"></a>Wywołanie interfejsu API programu Graph usługi Azure AD
Te przykładowy kod pokazują, jak tworzyć aplikacje, które wywołania interfejsu API Azure AD Graph do odczytywania i zapisywania danych katalogu.

| Język/Platform | Przykład | Opis |
| --- | --- | --- |
| Java |[Aplikacja sieci Web GraphAPI-Java](https://github.com/Azure-Samples/active-directory-java-graphapi-web) |Aplikacja sieci web, która używa interfejsu API programu Graph dostępu do danych katalogu usługi Azure AD. |
| PHP |[Aplikacja sieci Web GraphAPI-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) |Aplikacja sieci web, która używa interfejsu API programu Graph dostępu do danych katalogu usługi Azure AD. |
| C# / .NET |[Aplikacja sieci Web GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) |Aplikacja sieci web, która używa interfejsu API programu Graph dostępu do danych katalogu usługi Azure AD. |
| C# / .NET |[ConsoleApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) |Ta aplikacja konsoli Pokazuje Typowe wywołania odczytu i zapisu do interfejsu API programu Graph i pokazuje, jak wykonać przypisania licencji użytkownika i zaktualizuj łącza i miniatury zdjęcia użytkownika. |
| C# / .NET |[ConsoleApp-GraphAPI-DiffQuery-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) |Aplikacja konsolowa, która używa różnicowej zapytania w interfejsu API programu Graph, aby uzyskać okresowych zmian obiektów użytkowników w dzierżawie usługi Azure AD. |
| C# / .NET |[Aplikacja sieci Web GraphAPI-DirectoryExtensions-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) |Aplikacji MVC używa interfejsu API programu Graph kwerend do generowania schemat organizacyjny proste firmy. |
| PHP |[Aplikacja sieci Web GraphAPI-DirectoryExtensions-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) |Aplikacja PHP, która wywołuje interfejsu API programu Graph, aby zarejestrować rozszerzenie i następnie odczytu, aktualizacji i usuwania wartości w atrybucie rozszerzenia. |

## <a name="authorization"></a>Autoryzacja
Te przykłady pokazują, jak przy użyciu usługi Azure AD do autoryzacji.

| Język/Platform | Przykład | Opis |
| --- | --- | --- |
| C# / .NET |[Aplikacja sieci Web GroupClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) |Wykonaj kontroli dostępu opartej na rolach (RBAC) przy użyciu usługi Azure Active Directory oświadczenia grupy w aplikacji, która jest zintegrowany z usługą Azure AD. |
| C# / .NET |[Aplikacja sieci Web RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) |Wykonaj kontroli dostępu opartej na rolach (RBAC) przy użyciu ról aplikacji usługi Azure Active Directory w aplikacji, która jest zintegrowany z usługą Azure AD. |

## <a name="legacy-walkthroughs"></a>Wskazówki dotyczące starszej wersji
Te wskazówki używać technologii nieco starsze, ale nadal mogą być przydatne.

| Język/Platform | Przykład | Opis |
| --- | --- | --- |
| C# / .NET |[Autoryzacji opartej na rolach i na podstawie listy ACL w aplikacji Microsoft Azure AD](http://go.microsoft.com/fwlink/?LinkId=331694) |Wykonaj autoryzacji opartej na rolach (RBAC) i autoryzacji na podstawie listy ACL w aplikacji, która jest zintegrowany z usługą Azure AD. |
| C# / .NET |[Uwierzytelnianie — w Sklepie Windows do usługi REST - AAL](http://go.microsoft.com/fwlink/?LinkId=330605) |Użyj [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) (dawniej AAL) dla umożliwiające dodanie funkcji uwierzytelniania użytkownika do aplikacji ze Sklepu Windows w wersji Beta Sklepu Windows. |
| C# / .NET |[Uwierzytelnianie ADAL - aplikacji natywnej do usługi REST - przy użyciu usługi AAD za pomocą okna dialogowego przeglądarki](http://go.microsoft.com/fwlink/?LinkId=259814) |Użyj [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) umożliwiające dodanie funkcji uwierzytelniania użytkownika na kliencie WPF. |
| C# / .NET |[Uwierzytelnianie ADAL - aplikacji natywnej do usługi REST - ACS za pomocą okna dialogowego przeglądarki](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |Użyj [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) i [2.0 usługi kontroli dostępu (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) umożliwiające dodanie funkcji uwierzytelniania użytkownika na kliencie WPF. |
| C# / .NET |[ADAL — uwierzytelnianie serwera do serwera](http://go.microsoft.com/fwlink/?LinkId=259816) |Użyj [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) do zabezpieczania wywołań usługi z procesem po stronie serwera usługi MVC4 sieci Web interfejsu API REST. |
| C# / .NET |[Dodawanie logowania jednokrotnego do aplikacji sieci Web przy użyciu usługi Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) |Konfigurowanie aplikacji .NET w celu wykonania sieci web rejestracji jednokrotnej względem katalogu usługi Azure AD w organizacji. |
| C# / .NET |[Tworzenie aplikacji sieci Web z wieloma dzierżawcami za pomocą usługi Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) |Użyj usługi Azure AD, aby dodać do logowania jednokrotnego i możliwości dostępu do katalogu jednej aplikacji .NET do pracy przez wiele organizacji. |
| JAVA |[Java przykładową aplikację Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkId=263969) |Użyj interfejsu API programu Graph uzyskują dostęp do katalogu danych z usługi Azure AD. |
| PHP |[PHP przykładową aplikację Azure AD Graph API](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) |Użyj interfejsu API programu Graph uzyskują dostęp do katalogu danych z usługi Azure AD. |
| C# / .NET |[Przykładowa aplikacja dla usługi Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkID=262648) |Użyj interfejsu API programu Graph uzyskują dostęp do katalogu danych z usługi Azure AD. |
| C# / .NET |[Przykładowa aplikacja dla usługi Azure AD Graph różnicowej zapytania](http://go.microsoft.com/fwlink/?LinkId=275398) |Użyj różnicowej zapytania w interfejsu API programu Graph, aby pobrać okresowych zmian obiektów użytkowników w dzierżawie usługi Azure AD. |
| C# / .NET |[Przykładowa aplikacja dla integracji aplikacji w chmurze wielodostępne dla usługi Azure AD](http://go.microsoft.com/fwlink/?LinkId=275397) |Integracja aplikacji wielodostępnych do usługi Azure AD. |
| C# / .NET |[Zabezpieczanie aplikacji Sklepu Windows i usługi sieci Web REST przy użyciu usługi Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) |Utwórz zasób prosty interfejs API sieci web i aplikacji Sklepu Windows za pomocą usługi Azure AD i [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md). |
| C# / .NET |[Zapytanie usługi Azure AD za pomocą interfejs API Graph](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) |Konfigurowanie aplikacji Microsoft .NET w celu dostępu do danych z katalogu dzierżawy usługi Azure AD za pomocą interfejsu API Azure AD Graph. |

## <a name="see-also"></a>Zobacz też
##### <a name="other-resources"></a>Inne zasoby
[Przewodnik dewelopera usługi Azure Active Directory](active-directory-developers-guide.md)

[Azure AD Graph API koncepcyjne i odwołania](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Biblioteka pomocnik interfejsu API Graph usługi Azure AD](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
