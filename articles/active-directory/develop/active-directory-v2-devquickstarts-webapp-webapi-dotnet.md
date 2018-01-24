---
title: "Wywołanie wprowadzenie interfejsu API w aplikacji sieci web usługi Azure AD 2.0 .NET | Dokumentacja firmy Microsoft"
description: "Sposób tworzenia aplikacji sieci Web .NET MVC tego połączenia sieci web services za pomocą osobistego konta Microsoft i pracy lub logowania konta służbowego."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 56be906e-71de-469d-9a5c-9fc08aae4223
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: f59c9e2c523db319565c1cca13eb85f809b2bdd6
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="calling-a-web-api-from-a-net-web-app"></a>Wywoływanie interfejsu API sieci web z aplikacji sieci web .NET
Z punktem końcowym v2.0 można szybko Dodawanie uwierzytelniania do aplikacji sieci web i interfejsów API sieci web o obsługę zarówno osobistego konta Microsoft i konta służbowego.  W tym miejscu firma Microsoft będzie kompilacji aplikacji sieci web MVC, który zaloguje się użytkowników przy użyciu protokołu OpenID Connect, z niektórych pomoc od firmy Microsoft oprogramowania pośredniczącego OWIN.  Aplikacji sieci web pobieranie tokenów dostępu protokołu OAuth 2.0 dla sieci web interfejsu api zabezpieczone przez OAuth 2.0, który pozwala utworzyć, odczytać i delete na określony użytkownik "listą zadań do wykonania".

Ten samouczek koncentruje się przede wszystkim na przy użyciu MSAL nabywania i używania tokenów dostępu w aplikacji sieci web, w pełni opisane [tutaj](active-directory-v2-flows.md#web-apps).  Jako warunki wstępne, warto najpierw Dowiedz się, jak [dodać podstawowe logowanie do aplikacji sieci web](active-directory-v2-devquickstarts-dotnet-web.md) lub jak [właściwie zabezpieczyć interfejs API sieci web](active-directory-v2-devquickstarts-dotnet-api.md).

> [!NOTE]
> Nie wszystkie usługi Azure Active Directory scenariuszy i funkcji obsługiwanych przez punktu końcowego v2.0.  Aby ustalić, czy należy używać punktu końcowego v2.0, przeczytaj o [ograniczenia v2.0](active-directory-v2-limitations.md).
> 
> 

## <a name="download-sample-code"></a>Pobierz przykładowy kod
Kod używany w tym samouczku jest przechowywany [ w serwisie GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).  Aby z niego skorzystać, można [pobrać szkielet aplikacji w formie .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) lub sklonować szkielet:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Można też [Pobierz ukończonej aplikacji jako .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) lub sklonować ukończonej aplikacji:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Rejestracja aplikacji
Utwórz nową aplikację na [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), lub wykonaj następujące [szczegółowe kroki](active-directory-v2-app-registration.md).  Upewnij się, że:

* Skopiuj **identyfikator aplikacji** przypisany do aplikacji, będzie on potrzebny wkrótce.
* Utwórz **klucz tajny aplikacji** z **hasło** typu i skopiuj jej wartość na później
* Dodaj **Web** platformy aplikacji.
* Wprowadź poprawny **identyfikator URI przekierowania**. Wskazuje identyfikator uri przekierowania do usługi Azure AD, w którym powinny być kierowane odpowiedzi uwierzytelniania — wartością domyślną tego samouczka jest `https://localhost:44326/`.

## <a name="install-owin"></a>Instalowanie interfejsu OWIN
Dodawanie pakietów NuGet oprogramowanie pośredniczące OWIN do `TodoList-WebApp` projektu przy użyciu konsoli Menedżera pakietów.  Oprogramowanie pośredniczące OWIN będzie służyć do wysyłania żądań zalogowania się i wylogowania, zarządzania sesji użytkownika i uzyskać informacje o użytkowniku, między innymi.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>Zalogować użytkownika
Teraz skonfigurować oprogramowania pośredniczącego OWIN do zastosowania [protokołu uwierzytelniania OpenID Connect](active-directory-v2-protocols.md).  

* Otwórz `web.config` pliku w folderze głównym `TodoList-WebApp` projektu, a następnie wprowadź wartości konfiguracji aplikacji w `<appSettings>` sekcji.
  * `ida:ClientId` Jest **identyfikator aplikacji** przypisany do aplikacji w portalu rejestracji.
  * `ida:ClientSecret` Jest **klucz tajny aplikacji** utworzone w portalu rejestracji.
  * `ida:RedirectUri` Jest **identyfikator Uri przekierowania** wprowadzony w portalu.
* Otwórz `web.config` pliku w folderze głównym `TodoList-Service` projektu i Zastąp `ida:Audience` o takim samym **identyfikator aplikacji** jak powyżej.
* Otwórz plik `App_Start\Startup.Auth.cs` i Dodaj `using` instrukcje dla bibliotek z powyżej.
* W tym samym pliku implementacji `ConfigureAuth(...)` metody.  Parametry podane `OpenIDConnectAuthenticationOptions` będzie służyć jako współrzędnych dla aplikacji w celu komunikowania się z usługą Azure AD.

```csharp
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {

                    // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
                    // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                    // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                    ClientId = clientId,
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>Umożliwia pobieranie tokenów dostępu MSAL
W `AuthorizationCodeReceived` powiadomień, chcemy użyć [OAuth 2.0 w połączeniu z OpenID Connect](active-directory-v2-protocols.md) Aby zrealizować authorization_code dla tokenu dostępu do usługi listy zadań do wykonania.  MSAL może ułatwić ten proces można:

* Najpierw zainstaluj wersję zapoznawczą MSAL:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```

* I dodać kolejne `using` instrukcji `App_Start\Startup.Auth.cs` MSAL w pliku.
* Teraz Dodaj nową metodę `OnAuthorizationCodeReceived` obsługi zdarzeń.  Ten program obsługi będzie używać MSAL w celu pobrania tokenu dostępu do interfejsu API listy zadań do wykonania i będą przechowywane w pamięci podręcznej tokenu w MSAL token do użycia później:

```csharp
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

* W aplikacji sieci web MSAL ma rozszerzonego pamięci podręcznej tokenu służący do przechowywania tokenów.  W tym przykładzie implementuje `NaiveSessionCache` , który używa magazynu sesji http do pamięci podręcznej tokenów.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>Wywołanie interfejsu API sieci Web
Teraz nadszedł czas na rzeczywiście używane ' access_token ', które zostało zakupione w kroku 3.  Otwórz aplikację sieci web `Controllers\TodoListController.cs` pliku, co sprawia, że wszystkie żądania CRUD do interfejsu API listy zadań do wykonania.

* Można MSAL ponownie użyć w tym miejscu można pobrać z pamięci podręcznej MSAL access_tokens.  Najpierw dodaj `using` instrukcji dla MSAL do tego pliku.
  
    `using Microsoft.Identity.Client;`
* W `Index` akcji, użyj MSAL `AcquireTokenSilentAsync` metodę, aby pobrać ' access_token ', który może służyć do odczytywania danych z usługi listy zadań do wykonania:

```csharp
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

* Przykład następnie dodaje wynikowy token żądania HTTP GET jako `Authorization` nagłówek, który używa usługi listy zadań do wykonania uwierzytelnić żądania.
* Jeśli usługa listy zadań do wykonania zwraca `401 Unauthorized` odpowiedzi, access_tokens w MSAL stały się nieprawidłowy jakiegoś powodu.  W takim przypadku należy usunąć wszelkie access_tokens z pamięci podręcznej MSAL i Pokaż komunikat, który może być konieczne Zaloguj się ponownie, który zostanie uruchomiony ponownie przepływ nabycia tokenu użytkownika.

```csharp
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

* Podobnie MSAL nie może zwracać ' access_token ' jakiejkolwiek przyczyny, należy poinstruować użytkowników zalogować się ponownie.  Jest tak proste, jak przechwytywanie dowolnego `MSALException`:

```csharp
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

* Dokładnie takie same `AcquireTokenSilentAsync` wywołanie jest implementd w `Create` i `Delete` akcje.  W aplikacji sieci web ta metoda MSAL służy do pobrania access_tokens zawsze, gdy są potrzebne w Twojej aplikacji.  MSAL zajmie się pobieranie, buforowanie i odświeżanie tokenów.

Na koniec Skompiluj i uruchom aplikację!  Zaloguj się przy użyciu Account Microsoft lub konta usługi Azure AD i zwróć uwagę, jak tożsamość użytkowników jest widoczny w górnym pasku nawigacyjnym.  Dodaj i usuń niektóre elementy z listy zadań do wykonania użytkownika, aby zobaczyć OAuth 2.0 zabezpieczone wywołań interfejsu API w akcji.  Masz teraz aplikacji sieci web i interfejsu API sieci web, zarówno zabezpieczone przy użyciu standardowych protokołach branżowych, uwierzytelniających użytkowników przy użyciu ich osobistych i pracy/służbowego konta.

Próbka ukończone (bez wartości konfiguracji) [podano tutaj](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Następne kroki
Aby uzyskać dodatkowe zasoby Zobacz:

* [Przewodnik dewelopera v2.0 >>](active-directory-appmodel-v2-overview.md)
* [Tag StackOverflow "azure-active-directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Pobierz aktualizacje zabezpieczeń naszych produktów
Firma Microsoft zachęca do przekazywania powiadomień o występujących incydentach zabezpieczeń poprzez wizytę na [tej stronie](https://technet.microsoft.com/security/dd252948) i subskrybowanie Doradczych alertów zabezpieczeń.

