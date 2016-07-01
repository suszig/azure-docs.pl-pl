<properties
   pageTitle="Przewodnik dewelopera usługi Azure Active Directory | Microsoft Azure"
   description="Ten artykuł zawiera kompletny przewodnik dotyczący zasobów dla deweloperów usługi Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="04/02/2016"
   ms.author="mbaldwin"/>


# Przewodnik dewelopera usługi Azure Active Directory

## Omówienie
Jako platforma typu zarządzanie tożsamością jako usługa (IDMaaS), usługa Azure Active Directory zapewnia deweloperom efektywny sposób integrowania zarządzania tożsamością w swoich aplikacjach. Poniższe artykuły zawierają omówienie implementacji i kluczowych funkcji usługi Azure Active Directory. Zalecamy zapoznanie się z materiałami w odpowiedniej kolejności lub przejście do sekcji [Wprowadzenie](#getting-started), jeśli chcesz już rozpocząć pracę.


1. [Korzyści wynikające z integracji usługi Azure Active Directory](active-directory-how-to-integrate.md): odkryj, dlaczego integracja z usługą Azure Active Directory oferuje najlepsze rozwiązanie w zakresie bezpiecznego logowania i autoryzacji.

1. [Scenariusze uwierzytelniania usługi Active Directory](active-directory-authentication-scenarios.md): wykorzystaj uproszczone uwierzytelnianie w usłudze Azure Active Directory, aby zapewnić logowanie do aplikacji.

1. [Integrowanie aplikacji w usłudze Azure Active Directory](active-directory-integrating-applications.md): dowiedz się, jak dodawać, aktualizować i usuwać aplikacje z usługi Azure Active Directory i poznaj wytyczne związane ze znakowaniem zintegrowanych aplikacji.

1. [Interfejs API programu Graph w usłudze Azure Active Directory](active-directory-graph-api.md): użyj interfejsu API programu Graph w usłudze Azure Active Directory, aby uzyskać programowy dostęp do usługi Azure Active Directory poprzez punkty końcowe interfejsu API REST. Należy pamiętać, że interfejs API programu Graph w usłudze Azure AD jest również dostępny za pośrednictwem programu [Microsoft Graph](https://graph.microsoft.io/), standaryzowanego interfejsu API, który umożliwia uzyskiwanie dostępu do wielu interfejsów API usług w chmurze firmy Microsoft poprzez jeden punkt końcowy interfejsu API REST i przy użyciu jednego tokenu dostępu.

1. [Biblioteki uwierzytelniania usługi Azure Active Directory](active-directory-authentication-libraries.md): łatwe uwierzytelnianie użytkowników umożliwiające uzyskanie tokenów dostępu przy użyciu bibliotek uwierzytelniania usługi Azure AD dla platform .NET, JavaScript, Objective-C, Android i wielu innych.


## Wprowadzenie

Te samouczki są dostosowane do wielu platform i mogą pomóc w szybkim rozpoczęciu tworzenia przy użyciu usługi Azure Active Directory. Warunkiem wstępnym jest [uzyskanie dzierżawy usługi Azure Active Directory](active-directory-howto-tenant.md).

### Poradniki Szybki start dotyczące aplikacji mobilnych i komputerowych

|[![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)|[![Windows Universal](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:
|[iOS](active-directory-devquickstarts-ios.md)|[Android](active-directory-devquickstarts-android.md)|[.NET](active-directory-devquickstarts-dotnet.md)|[Windows Phone](active-directory-devquickstarts-windowsphone.md)|[Windows Store](active-directory-devquickstarts-windowsstore.md)|[Xamarin](active-directory-devquickstarts-xamarin.md)|[Cordova](active-directory-devquickstarts-cordova.md)

### Poradniki Szybki start dotyczące aplikacji sieci Web

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md)|[![Javascript](./media/active-directory-developers-guide/javascript.png)](active-directory-devquickstarts-angular.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md)
|:--:|:--:|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapp-dotnet.md)|[Java](active-directory-devquickstarts-webapp-java.md)|[Javascript](active-directory-devquickstarts-angular.md)|[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md)

### Przewodniki Szybki start dotyczące interfejsu API sieci Web

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapi-dotnet.md)|[Node.js](active-directory-devquickstarts-webapi-nodejs.md)

### Wysyłanie zapytań do katalogu — przewodnik Szybki start

| [![.NET](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md)|
|:--:|
|[Interfejs API programu Graph](active-directory-graph-api-quickstart.md)|

## Poradniki

Te artykuły zawierają opis wykonywania określonych zadań przy użyciu usługi Azure Active Directory:

- [Uzyskiwanie dzierżawy usługi Azure Active Directory](active-directory-howto-tenant.md)
- [Wyświetlanie listy aplikacji w galerii aplikacji usługi Azure Active Directory](active-directory-app-gallery-listing.md)
- [Informacje o manifeście aplikacji Azure Active Directory](active-directory-application-manifest.md)
- [Tworzenie aplikacji z interfejsami API usługi Office 365](https://msdn.microsoft.com/office/office365/howto/getting-started-Office-365-APIs)
- [Przesyłanie aplikacji sieci Web dla usługi Office 365 do pulpitu nawigacyjnego sprzedawcy](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- Dowiedz się, jak włączyć logowanie jednokrotne dla wielu aplikacji za pomocą biblioteki ADAL na urządzeniach z systemem [Android](active-directory-sso-android.md) i [iOS](active-directory-sso-ios.md)
- [Wersja zapoznawcza: jak tworzyć aplikacje, które logują użytkowników przy użyciu konta osobistego i służbowego](active-directory-appmodel-v2-overview.md)
- [Wersja zapoznawcza: jak tworzyć aplikacje, które rejestrują i logują klientów](../active-directory-b2c/active-directory-b2c-overview.md)


## Dokumentacja

Te artykuły zawierają dokumentację referencyjną dla interfejsów API REST i biblioteki uwierzytelniania, protokołów, błędów, przykładów kodu i punktów końcowych.  

###  Pomoc techniczna
- [Oznakowane pytania](http://stackoverflow.com/questions/tagged/azure-active-directory): znajdź rozwiązania Azure Active Directory w witrynie Stack Overflow, wyszukując tagi [azure-active-directory](http://stackoverflow.com/questions/tagged/azure-active-directory) i [adal](http://stackoverflow.com/questions/tagged/adal).

### Kod

- [Biblioteki typu open source usługi Azure Active Directory](http://github.com/AzureAD): najłatwiejszym sposobem na znalezienie źródła biblioteki jest użycie [listy bibliotek](active-directory-authentication-libraries.md).

- [Przykłady Azure Active Directory](https://github.com/azure-samples?query=active-directory): najłatwiejszym sposobem nawigowania na liście przykładów jest użycie [indeksu przykładów kodu](active-directory-code-samples.md).

- [ADAL dla platformy .NET](https://msdn.microsoft.com/library/azure/mt417579.aspx): dokumentacja dla biblioteki uwierzytelniania .NET.

### Interfejs API programu Graph

- [Dokumentacja interfejsu API programu Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx): dokumentacja referencyjna REST dla interfejsu API programu Graph usługi Azure Active Directory. [Wyświetlanie interaktywnego środowiska referencyjnego interfejsu API programu Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- [Zakresy uprawnień interfejsu API programu Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): zakresy uprawnień uwierzytelniania OAuth 2.0, których używa się do kontrolowania dostępu aplikacji do danych katalogu w dzierżawie.

### Protokoły uwierzytelniania

- [Protokoły uwierzytelniania w usłudze Azure Active Directory](active-directory-protocols.md): więcej informacji na temat różnych protokołów uwierzytelniania i autoryzacji obsługiwanych przez usługę Azure Active Directory.

- [Dane referencyjne protokołu SAML 2.0](active-directory-saml-protocol-reference.md): protokół SAML 2.0 umożliwia aplikacjom zapewnianie logowania jednokrotnego dla użytkowników.

- [Dane referencyjne protokołu OAuth 2.0](active-directory-protocols-oauth-code.md): protokół OAuth 2.0 służy do autoryzowania dostępu do aplikacji sieci Web i interfejsów API sieci Web w dzierżawie usługi Azure Active Directory.

- [Dane referencyjne protokołu OpenID Connect 1.0](active-directory-protocols-openid-connect-code.md): protokół OpenID Connect 1.0 rozszerza protokół OAuth 2.0, umożliwiając jego użycie jako protokołu uwierzytelniania.

- [Protokół WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): usługa Azure Active Directory obsługuje protokół WS-Federation 1.2 zgodnie ze specyfikacją Web Services Federation w wersji 1.2.

- [Obsługiwane typy tokenów i oświadczeń](active-directory-token-and-claims.md): ten przewodnik zawiera informacje pomagające zrozumieć i ocenić oświadczenia w tokenach SAML 2.0 i tokenach JSON sieci Web (JWT).

## Filmy wideo

### Kompilacja

Te prezentacje przedstawiające tworzenie aplikacji przy użyciu usługi Azure Active Directory są prowadzone przez osoby pracujące bezpośrednio w zespołach inżynieryjnych. Prezentacje obejmują podstawowe tematy, w tym IDMaaS, uwierzytelnianie, federację tożsamości i logowanie jednokrotne.

- [Microsoft Identity: State of the Union and Future Direction (Tożsamość firmy Microsoft: stan unii i kierunek rozwoju)](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
- [Azure Active Directory: Identity management as a service for modern applications (Azure Active Directory: zarządzanie tożsamością jako usługa dla nowoczesnych aplikacji)](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
- [Develop modern web applications with Azure Active Directory (Opracowywanie nowoczesnych aplikacji sieci Web przy użyciu usługi Azure Active Directory)](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
- [Develop modern native applications with Azure Active Directory (Opracowywanie nowoczesnych aplikacji natywnych przy użyciu usługi Azure Active Directory)](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### Piątek z Azure
[Piątek z Azure](https://azure.microsoft.com/documentation/videos/azure-friday/) to cykliczna seria nagrań wideo z wywiadami 1:1 odbywająca się w piątki. Seria skupia się na krótkich (10–15 minut) wywiadach z ekspertami omawiającymi różne tematy związane z platformą Azure.  Użyj funkcji filtrowania usług na stronie, aby zobaczyć wszystkie filmy wideo dotyczące usługi Azure Active Directory.

- [Tożsamość platformy Azure 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Tożsamość platformy Azure 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Tożsamość platformy Azure 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## Społeczności

- [Blog zespołu usługi Active Directory](http://blogs.technet.com/b/ad/): najnowsze osiągnięcia w świecie usługi Azure Active Directory.

- [Blog zespołu programu Graph usługi Azure Active Directory](http://blogs.msdn.com/b/aadgraphteam): informacje dotyczące usługi Azure Active Directory właściwe dla interfejsu API programu Graph.

- [Tożsamość w chmurze](http://www.cloudidentity.net): przemyślenia dotyczące rozwiązania „zarządzanie tożsamością jako usługa” pochodzące od głównego menedżera projektów usługi Azure Active Directory.  

- [Usługa Azure Active Directory w serwisie Twitter](https://twitter.com/azuread): ogłoszenia dotyczące usługi Azure Active Directory o długości maksymalnie 140 znaków.



<!--HONumber=Jun16_HO2-->


