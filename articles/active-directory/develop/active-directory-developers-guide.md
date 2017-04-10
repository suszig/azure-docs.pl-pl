---
title: "Przewodnik dewelopera usługi Azure Active Directory | Microsoft Docs"
description: "Ten artykuł zawiera kompletny przewodnik dotyczący zasobów dla deweloperów usługi Azure Active Directory."
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/09/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 45ff2ff08cb36f9a4bf49ff04de149accf157e0e
ms.lasthandoff: 03/31/2017


---
# <a name="azure-active-directory-developers-guide"></a>Przewodnik dewelopera usługi Azure Active Directory
## <a name="overview"></a>Omówienie
Jako platforma typu zarządzanie tożsamością jako usługa (IDMaaS), usługa Azure Active Directory (AD) zapewnia deweloperom efektywny sposób integrowania zarządzania tożsamością w swoich aplikacjach. Poniższe artykuły zawierają omówienie implementacji i kluczowych funkcji usługi Azure AD. Zalecamy zapoznanie się z materiałami w odpowiedniej kolejności lub przejście do sekcji [Wprowadzenie](#getting-started), jeśli chcesz już rozpocząć pracę.

1. [Korzyści wynikające z integracji usługi Azure AD](active-directory-how-to-integrate.md): odkryj, dlaczego integracja z usługą Azure AD oferuje najlepsze rozwiązanie w zakresie bezpiecznego logowania i autoryzacji.
2. [Scenariusze uwierzytelniania usługi Azure AD](active-directory-authentication-scenarios.md): wykorzystaj uproszczone uwierzytelnianie w usłudze Azure AD, aby zapewnić logowanie do aplikacji.
3. [Integrowanie aplikacji w usłudze Azure AD](active-directory-integrating-applications.md): dowiedz się, jak dodawać, aktualizować i usuwać aplikacje z usługi Azure AD i poznaj wytyczne związane ze znakowaniem zintegrowanych aplikacji.
4. [Program Microsoft Graph](https://graph.microsoft.io/) i [interfejs API funkcji Azure AD Graph](active-directory-graph-api.md): uzyskiwanie programowego dostępu do usługi Azure AD za pośrednictwem punktów końcowych interfejsu API REST. **Zdecydowanie zalecamy uzyskiwanie dostępu do zasobów usługi Azure Active Directory za pomocą programu Microsoft Graph zamiast interfejsu API funkcji Azure AD Graph.** Obecnie koncentrujemy nasze działania deweloperskie na programie Microsoft Graph i nie planujemy żadnych dodatkowych rozszerzeń dla interfejsu API funkcji Azure AD Graph. Istnieje bardzo ograniczona liczba scenariuszy, w których można używać interfejsu API funkcji Azure AD Graph. Więcej informacji można znaleźć we wpisie w blogu [Microsoft Graph or the Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) w Centrum deweloperów pakietu Office.
5. [Biblioteki uwierzytelniania usługi Azure AD](active-directory-authentication-libraries.md): łatwe uwierzytelnianie użytkowników umożliwiające uzyskanie tokenów dostępu przy użyciu bibliotek uwierzytelniania usługi Azure AD dla platform .NET, JavaScript, Objective-C, Android i wielu innych.

## <a name="getting-started"></a>Wprowadzenie
Te samouczki są dostosowane do wielu platform i mogą pomóc w szybkim rozpoczęciu tworzenia przy użyciu usługi Azure Active Directory. Warunkiem wstępnym jest [uzyskanie dzierżawy usługi Azure Active Directory](active-directory-howto-tenant.md).

### <a name="mobile-and-pc-application-quick-start-guides"></a>Poradniki Szybki start dotyczące aplikacji mobilnych i komputerowych
| [![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md) | [![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md) | [![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md) | [![Aplikacje uniwersalne systemu Windows](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md) | 
|:---:|:---:|:---:|:---:|:---:|
| [iOS](active-directory-devquickstarts-ios.md) |[Android](active-directory-devquickstarts-android.md) |[.NET](active-directory-devquickstarts-dotnet.md) |[Aplikacje uniwersalne</br>systemu Windows](active-directory-devquickstarts-windowsstore.md) |

|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md) | [![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md) | [![OAuth 2.0](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md) |
|:---:|:---:|:---:|
|[Xamarin](active-directory-devquickstarts-xamarin.md) |[Cordova](active-directory-devquickstarts-cordova.md) |[Integracja bezpośrednia</br>z uwierzytelnianiem OAuth 2.0](active-directory-protocols-oauth-code.md) |

### <a name="web-application-quick-start-guides"></a>Poradniki Szybki start dotyczące aplikacji sieci Web
| [![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md) | [![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md) | [![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md) |
|:---:|:---:|:---:|
| [.NET](active-directory-devquickstarts-webapp-dotnet.md) |[Java](active-directory-devquickstarts-webapp-java.md) |[AngularJS](active-directory-devquickstarts-angular.md) |[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md) |[Integracja bezpośrednia</br> z protokołem OpenID Connect](active-directory-protocols-openid-connect-code.md) |

| [![JavaScript](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | [![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![OpenID Connect](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md) |
|:---:|:---:|:---:|
|[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md) |[Integracja bezpośrednia</br> z protokołem OpenID Connect](active-directory-protocols-openid-connect-code.md) |

### <a name="web-api-quick-start-guides"></a>Przewodniki Szybki start dotyczące interfejsu API sieci Web
| [![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md) | [![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md) |
|:---:|:---:|
| [.NET](active-directory-devquickstarts-webapi-dotnet.md) |[Node.js](active-directory-devquickstarts-webapi-nodejs.md) |

### <a name="microsoft-graph-and-azure-ad-graph-api-quick-start-guides"></a>Przewodniki Szybki start dotyczące programu Microsoft Graph i interfejsu API funkcji Azure AD Graph
| [![Microsoft Graph](./media/active-directory-developers-guide/msgraph.png)](https://developer.microsoft.com/graph/quick-start) | [![Interfejs API funkcji Azure AD Graph](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md) |
|:---:|:---:|
| [Microsoft Graph](https://developer.microsoft.com/graph/quick-start) | [Interfejs API funkcji Azure AD Graph](active-directory-graph-api-quickstart.md) |

## <a name="how-tos"></a>Poradniki
Te artykuły zawierają opis wykonywania określonych zadań przy użyciu usługi Azure Active Directory:

* [Uzyskiwanie dzierżawy usługi Azure AD](active-directory-howto-tenant.md)
* [Logowanie się dowolnego użytkownika usługi Azure AD za pomocą wzorca aplikacji wielodostępnych](active-directory-devhowto-multi-tenant-overview.md)
* [Używanie certyfikatu zamiast wpisu tajnego do uwierzytelniania tożsamości aplikacji](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/)
* Włączanie logowania jednokrotnego dla wielu aplikacji za pomocą biblioteki ADAL na urządzeniach z systemem [Android](active-directory-sso-android.md) i [iOS](active-directory-sso-ios.md)
* [Przyznawanie aplikacji certyfikatu AppSource dla usługi Azure AD](active-directory-devhowto-appsource-certified.md)
* [Wyświetlanie listy aplikacji w galerii aplikacji usługi Azure AD](active-directory-app-gallery-listing.md)
* [Przesyłanie aplikacji sieci Web dla usługi Office 365 do pulpitu nawigacyjnego sprzedawcy](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
* [Rejestrowanie aplikacji w usłudze Azure Active Directory przy użyciu witryny Azure Portal](../active-directory-app-registration.md)
* [Informacje o manifeście aplikacji Azure Active Directory](active-directory-application-manifest.md)
* [Zrozumienie wytycznych dotyczących znakowania przycisków logowania i pozyskiwania aplikacji w aplikacji klienckiej](active-directory-branding-guidelines.md)
* [Wersja zapoznawcza: jak tworzyć aplikacje, które logują użytkowników przy użyciu konta osobistego i służbowego lub szkolnego](active-directory-appmodel-v2-overview.md)
* [Wersja zapoznawcza: jak tworzyć aplikacje, które rejestrują i logują klientów](../../active-directory-b2c/active-directory-b2c-overview.md)
* [Wersja zapoznawcza: konfigurowanie okresów istnienia tokenu w usłudze Azure AD](../active-directory-configurable-token-lifetimes.md) przy użyciu programu PowerShell. Zobacz [operacje zasad](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) i [jednostka zasad](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity), aby poznać szczegółowe informacje na temat konfigurowania za pośrednictwem interfejsu API programu Azure AD Graph.

## <a name="reference"></a>Dokumentacja
Te artykuły zawierają dokumentację referencyjną dla interfejsów API REST i biblioteki uwierzytelniania, protokołów, błędów, przykładów kodu i punktów końcowych.  

### <a name="support"></a>Pomoc techniczna
* [Oznakowane pytania](http://stackoverflow.com/questions/tagged/azure-active-directory): znajdź rozwiązania Azure Active Directory w witrynie Stack Overflow, wyszukując tagi [azure-active-directory](http://stackoverflow.com/questions/tagged/azure-active-directory) i [adal](http://stackoverflow.com/questions/tagged/adal).
* Aby uzyskać definicje niektórych powszechnie używanych terminów dotyczących tworzenia i integrowania aplikacji, zobacz [Słownik deweloperów usługi Azure AD](active-directory-dev-glossary.md).

### <a name="code"></a>Kod
* [Biblioteki typu open source usługi Azure Active Directory](http://github.com/AzureAD): najłatwiejszym sposobem na znalezienie źródła biblioteki jest użycie [listy bibliotek](active-directory-authentication-libraries.md).
* [Przykłady Azure Active Directory](https://github.com/azure-samples?query=active-directory): najłatwiejszym sposobem nawigowania na liście przykładów jest użycie [indeksu przykładów kodu](active-directory-code-samples.md).
* [Biblioteka ADAL (Azure Active Directory Authentication Library) dla środowiska .NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) — dokumentacja referencyjna jest dostępna zarówno dla [najnowszej wersji głównej](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory), jak i [wcześniejszej wersji głównej](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory).

### <a name="microsoft-graph-and-azure-ad-graph-api"></a>Program Microsoft Graph i interfejs API funkcji Azure AD Graph
> [!IMPORTANT]
> Zdecydowanie zalecamy uzyskiwanie dostępu do zasobów usługi Azure Active Directory za pomocą programu [Microsoft Graph](https://graph.microsoft.io/) zamiast interfejsu API funkcji Azure AD Graph. Obecnie koncentrujemy nasze działania deweloperskie na programie Microsoft Graph i nie planujemy żadnych dodatkowych rozszerzeń dla interfejsu API funkcji Azure AD Graph. Istnieje bardzo ograniczona liczba scenariuszy, w których można używać interfejsu API funkcji Azure AD Graph. Więcej informacji można znaleźć we wpisie w blogu [Microsoft Graph or the Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) w Centrum deweloperów pakietu Office.
> 
* [Microsoft Graph](https://graph.microsoft.io/): dokumentacja, informacje, przykłady i zestawy SDK dotyczące programu Microsoft Graph. 
* [Dokumentacja interfejsu API funkcji Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog): dokumentacja referencyjna REST dla interfejsu API funkcji Azure Active Directory Graph. 
* [Zakresy uprawnień interfejsu API funkcji Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): zakresy uprawnień uwierzytelniania OAuth 2.0 używane do kontrolowania dostępu aplikacji do danych katalogu w dzierżawie.

### <a name="authentication-and-authorization-protocols"></a>Protokoły uwierzytelniania i autoryzacji
* [Przerzucanie klucza podpisywania w usłudze Azure AD](active-directory-signing-key-rollover.md): dowiedz się więcej o kadencji przerzucania klucza podpisywania usługi Azure AD i sposobie aktualizowania klucza w najbardziej typowych scenariuszach aplikacji.
* [Protokół OAuth 2.0: korzystanie z funkcji przyznawania kodu autoryzacji](active-directory-protocols-oauth-code.md): przyznawanie kodu autoryzacji protokołu OAuth 2.0 umożliwia autoryzowanie dostępu do aplikacji sieci Web i interfejsów API sieci Web w ramach dzierżawy usługi Azure Active Directory.
* [Protokół OAuth 2.0: zrozumienie funkcji przyznawania niejawnego](active-directory-dev-understanding-oauth2-implicit-grant.md): dowiedz się więcej o niejawnym przyznawaniu autoryzacji i o tym, czy jest ono odpowiednie dla Twojej aplikacji.
* [Protokół OAuth 2.0: wywołania typu „service to service” przy użyciu poświadczeń klienta](active-directory-protocols-oauth-service-to-service.md): poświadczenia klienta protokołu OAuth 2.0 zezwalają usłudze sieci Web (klientowi poufnemu) na używanie jej własnych poświadczeń w celu uwierzytelniania podczas wywoływania innej usługi sieci Web, zamiast personifikacji użytkownika. W tym scenariuszu klient jest zazwyczaj usługą sieci Web warstwy środkowej, usługą demona lub witryną sieci Web.
* [Protokół OpenID Connect 1.0: logowanie się i uwierzytelnianie](active-directory-protocols-openid-connect-code.md): protokół OpenID Connect 1.0 rozszerza protokół OAuth 2.0, umożliwiając jego użycie jako protokołu uwierzytelniania. Aplikacja kliencka może odbierać element id_token w celu zarządzania procesem logowania lub rozszerzyć przepływ kodu autoryzacji w celu odbierania zarówno elementu id_token, jak i kodu autoryzacji.
* [Dane referencyjne protokołu SAML 2.0](active-directory-saml-protocol-reference.md): protokół SAML 2.0 umożliwia aplikacjom zapewnianie logowania jednokrotnego dla użytkowników.
* [Protokół WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): usługa Azure Active Directory obsługuje protokół WS-Federation 1.2 zgodnie ze specyfikacją Web Services Federation w wersji 1.2. Aby uzyskać więcej informacji na temat dokumentu metadanych federacji, zobacz [Metadane federacji](active-directory-federation-metadata.md).
* [Obsługiwane typy tokenów i oświadczeń](active-directory-token-and-claims.md): ten przewodnik zawiera informacje pomagające zrozumieć i ocenić oświadczenia w tokenach SAML 2.0 i tokenach JSON sieci Web (JWT).

## <a name="videos"></a>Filmy wideo
### <a name="build"></a>Kompilacja
Te prezentacje przedstawiające tworzenie aplikacji przy użyciu usługi Azure Active Directory są prowadzone przez osoby pracujące bezpośrednio w zespołach inżynieryjnych. Prezentacje obejmują podstawowe tematy, w tym IDMaaS, uwierzytelnianie, federację tożsamości i logowanie jednokrotne.

* [Microsoft Identity: State of the Union and Future Direction](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/) (Rozwiązania firmy Microsoft dotyczące tożsamości: stan unifikacji i kierunek rozwoju)
* [Azure Active Directory: Identity management as a service for modern applications](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/) (Azure Active Directory: zarządzanie tożsamością jako usługa dla nowoczesnych aplikacji)
* [Develop modern web applications with Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/) (Opracowywanie nowoczesnych aplikacji sieci Web przy użyciu usługi Azure Active Directory)
* [Develop modern native applications with Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/) (Opracowywanie nowoczesnych aplikacji natywnych przy użyciu usługi Azure Active Directory)

### <a name="azure-friday"></a>Piątek z Azure
[Piątek z Azure](https://azure.microsoft.com/documentation/videos/azure-friday/) to cykliczna seria nagrań wideo z wywiadami 1:1 odbywająca się w piątki. Seria skupia się na krótkich (10–15 minut) wywiadach z ekspertami omawiającymi różne tematy związane z platformą Azure.  Użyj funkcji filtrowania usług na stronie, aby zobaczyć wszystkie filmy wideo dotyczące usługi Azure Active Directory.

* [Azure Identity 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/) (Podstawy zarządzania tożsamościami na platformie Azure — 101)
* [Azure Identity 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/) (Podstawy zarządzania tożsamościami na platformie Azure — 102)
* [Azure Identity 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/) (Podstawy zarządzania tożsamościami na platformie Azure — 103)

## <a name="social"></a>Społeczności
* [Blog zespołu usługi Active Directory](http://blogs.technet.com/b/ad/): najnowsze osiągnięcia w świecie usługi Azure Active Directory.
* [Blog zespołu programu Graph usługi Azure Active Directory](https://blogs.msdn.microsoft.com/aadgraphteam): informacje dotyczące usługi Azure Active Directory właściwe dla interfejsu API programu Graph.
* [Tożsamość w chmurze](http://www.cloudidentity.net): przemyślenia dotyczące rozwiązania „zarządzanie tożsamością jako usługa” pochodzące od głównego menedżera projektów usługi Azure Active Directory.  
* [Usługa Azure Active Directory w serwisie Twitter](https://twitter.com/azuread): ogłoszenia dotyczące usługi Azure Active Directory o długości maksymalnie 140 znaków.

## <a name="windows-server-on-premises-development"></a>Lokalne programowanie systemu Windows Server
Aby uzyskać wskazówki dotyczące używania funkcji programowania systemu Windows Server i usług Active Directory Federation Services (AD FS), zobacz:

* [Scenariusze usług AD FS dla deweloperów](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-scenarios-for-developers): zawiera omówienie składników usług AD FS i sposobu ich działania, ze szczegółami dotyczącymi obsługiwanych scenariuszy uwierzytelniania/autoryzacji.
* [Przewodniki dotyczące usług AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/ad-fs-development): lista przewodników, które zawierają instrukcje krok po kroku dotyczące implementowania powiązanych przepływów uwierzytelniania/autoryzacji.

