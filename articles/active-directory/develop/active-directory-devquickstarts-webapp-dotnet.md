---
title: Azure AD .NET aplikacji sieci web wprowadzenie | Dokumentacja firmy Microsoft
description: "Tworzenie aplikacji sieci web .NET MVC, która integruje się z usługą Azure AD dla logowania."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mtillman
editor: 
ms.assetid: e15a41a4-dc5d-4c90-b3fe-5dc33b9a1e96
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 5025ebda6eb47c4155c098be4a5479d5c3814942
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="aspnet-web-app-sign-in-and-sign-out-with-azure-ad"></a>Aplikacja sieci web ASP.NET, logowania i wylogowywania z usługą Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Podając jednego logowania i wylogowywania tylko kilka wierszy kodu, Azure Active Directory (Azure AD) upraszcza umożliwiające zewnętrzny Zarządzanie tożsamościami aplikacji sieci web. Może się zarejestrować użytkowników i aplikacji sieci web ASP.NET przy użyciu przez firmę Microsoft implementacją interfejsu Open Web dla platformy .NET (OWIN) oprogramowania pośredniczącego. Społeczność oprogramowania pośredniczącego OWIN znajduje się w programie .NET Framework 4.5. W tym artykule przedstawiono sposób użycia OWIN do:

* Logowania użytkowników do aplikacji sieci web przy użyciu usługi Azure AD jako dostawcy tożsamości.
* Wyświetlane informacje użytkownika.
* Loguj użytkowników spoza aplikacji.

## <a name="before-you-get-started"></a>Przed rozpoczęciem
* Pobierz [szkielet aplikacji](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) lub pobrać [ukończone próbki](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).
* Należy również dzierżawa usługi Azure AD, w którym można zarejestrować aplikacji. Jeśli nie masz już dzierżawę usługi Azure AD [Dowiedz się, jak kupić](active-directory-howto-tenant.md).

Gdy wszystko będzie gotowe, wykonaj procedury opisane w obok cztery sekcje.

## <a name="step-1-register-the-new-app-with-azure-ad"></a>Krok 1: Zarejestrować nową aplikację z usługą Azure AD
Aby skonfigurować aplikację do uwierzytelniania użytkowników, najpierw zarejestrować ją w dzierżawie w następujący sposób:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Na górnym pasku kliknij nazwę konta. W obszarze **katalogu** wybierz dzierżawy usługi Active Directory, które chcesz zarejestrować aplikację.
3. Kliknij przycisk **więcej usług** w okienku po lewej stronie, a następnie wybierz **usługi Azure Active Directory**.
4. Kliknij przycisk **rejestracji aplikacji**, a następnie wybierz **Dodaj**.
5. Postępuj zgodnie z monitami, aby utworzyć nową **aplikacji sieci Web i/lub WebAPI**.
  * **Nazwa** opisuje aplikacji dla użytkowników.
  * **Adres URL logowania** jest podstawowy adres URL aplikacji. Szkielet domyślny adres URL jest https://localhost:44320 /.
6. Po zakończeniu rejestracji usługi Azure AD przypisuje aplikacji identyfikatora aplikacji Skopiuj wartość ze strony aplikacji do użycia w kolejnych sekcjach.
7. Z **ustawienia** -> **właściwości** strony dla aplikacji, zaktualizuj identyfikator URI aplikacji. **Identyfikator URI aplikacji** to unikatowy identyfikator dla aplikacji. Konwencja nazewnictwa jest `https://<tenant-domain>/<app-name>` (na przykład `https://contoso.onmicrosoft.com/my-first-aad-app`).

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>Krok 2: Konfigurowanie aplikacji do używania uwierzytelniania potoku OWIN
W tym kroku skonfigurujesz oprogramowanie pośredniczące OWIN do używania protokołu uwierzytelniania OpenID Connect. OWIN służy do wysyłania żądań zalogowania się i wylogowania, zarządzania sesjami użytkownika uzyskiwanie informacji o użytkowniku i tak dalej.

1. Aby rozpocząć, Dodaj pakiety NuGet oprogramowanie pośredniczące OWIN do projektu przy użyciu konsoli Menedżera pakietów.

     ```
     PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
     PM> Install-Package Microsoft.Owin.Security.Cookies
     PM> Install-Package Microsoft.Owin.Host.SystemWeb
     ```

2. Aby dodać klasy początkowej OWIN do projektu o nazwie `Startup.cs`, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj**, wybierz pozycję **nowy element**, a następnie wyszukaj **OWIN**. Wywołuje oprogramowanie pośredniczące OWIN **Configuration(...)**  metoda po uruchomieniu aplikacji.
3. Deklaracja klasy, aby zmienić `public partial class Startup`. Już zaimplementowano część tej klasy dla Ciebie w innym pliku. W **Configuration(...)**  utworzyć wywołanie metody **ConfgureAuth(...)**  Aby skonfigurować uwierzytelnianie dla aplikacji.  

     ```C#
     public partial class Startup
     {
         public void Configuration(IAppBuilder app)
         {
             ConfigureAuth(app);
         }
     }
     ```

4. Otwórz plik App_Start\Startup.Auth.cs, a następnie wdrożyć to rozwiązanie **ConfigureAuth(...)**  metody. Parametry podane *OpenIDConnectAuthenticationOptions* służyć jako współrzędnych dla aplikacji w celu komunikowania się z usługą Azure AD. Należy również skonfigurować uwierzytelniania plików cookie, ponieważ oprogramowanie pośredniczące OpenID Connect używa plików cookie w tle.

     ```C#
     public void ConfigureAuth(IAppBuilder app)
     {
         app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

         app.UseCookieAuthentication(new CookieAuthenticationOptions());

         app.UseOpenIdConnectAuthentication(
             new OpenIdConnectAuthenticationOptions
             {
                 ClientId = clientId,
                 Authority = authority,
                 PostLogoutRedirectUri = postLogoutRedirectUri,
                 Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = context =>
                        {
                            context.HandleResponse();
                            context.Response.Redirect("/Error?message=" + context.Exception.Message);
                            return Task.FromResult(0);
                        }
                    }
             });
     }
     ```

5. Otwórz plik web.config w katalogu głównym projektu, a następnie wprowadź wartości konfiguracji w `<appSettings>` sekcji.
  * `ida:ClientId`: Identyfikator GUID został skopiowany z portalu Azure w "krok 1: zarejestrować nową aplikację z usługą Azure AD."
  * `ida:Tenant`: Nazwa dzierżawy usługi Azure AD (np. contoso.onmicrosoft.com).
  * `ida:PostLogoutRedirectUri`Wskaźnik, który informuje usługi Azure AD, w którym użytkownik powinien być przekierowany po pomyślnym zakończeniu wylogowania żądania.

## <a name="step-3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>Krok 3: Użyj OWIN do wysyłania żądań zalogowania się i wylogowania do usługi Azure AD
Aplikacji jest teraz prawidłowo skonfigurowana do komunikowania się z usługą Azure AD przy użyciu protokołu uwierzytelniania OpenID Connect. OWIN obsłużyła wszystkie szczegóły obsługuje tworzenie komunikatów uwierzytelniania, sprawdzanie poprawności tokenów z usługi Azure AD i utrzymywanie sesji użytkownika. Wszystko, co jest pozostaje, aby zapewnić użytkownikom możliwość zalogować się i wylogowywania.

1. Można autoryzować tagów w kontrolerach, aby wymagać od użytkowników do logowania, aby dostęp do określonych stron. Aby to zrobić, otwórz Controllers\HomeController.cs, a następnie dodaj `[Authorize]` tag do akcji informacje.

     ```C#
     [Authorize]
     public ActionResult About()
     {
       ...
     ```

2. OWIN umożliwia również wystawić żądania uwierzytelniania od bezpośrednio w kodzie. Aby to zrobić, otwórz Controllers\AccountController.cs. Następnie w SignIn() i SignOut() akcje wystawiania żądań wylogowania i żądania OpenID Connect.

     ```C#
     public void SignIn()
     {
         // Send an OpenID Connect sign-in request.
         if (!Request.IsAuthenticated)
         {
             HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
         }
     }
     public void SignOut()
     {
         // Send an OpenID Connect sign-out request.
         HttpContext.GetOwinContext().Authentication.SignOut(
              OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
     }
     ```

3. Otwórz Views\Shared\_LoginPartial.cshtml wyświetlany, gdy użytkownik łączy logowania i wylogowywania aplikacji i wydrukuj nazwy użytkownika w widoku.

    ```HTML
    @if (Request.IsAuthenticated)
    {
     <text>
         <ul class="nav navbar-nav navbar-right">
             <li class="navbar-text">
                 Hello, @User.Identity.Name!
             </li>
             <li>
                 @Html.ActionLink("Sign out", "SignOut", "Account")
             </li>
         </ul>
     </text>
    }
    else
    {
     <ul class="nav navbar-nav navbar-right">
         <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
     </ul>
    }
    ```

## <a name="step-4-display-user-information"></a>Krok 4: Wyświetlane informacje o użytkowniku
Podczas uwierzytelniania użytkowników z OpenID Connect, usługi Azure AD zwraca żądaniu aplikacja, która zawiera "oświadczenia" lub potwierdzeniami dotyczącymi użytkownika. Może użyć tych oświadczeń spersonalizować aplikacji, wykonując następujące czynności:

1. Otwórz plik Controllers\HomeController.cs. Dostęp do oświadczeń użytkownika w kontrolerach za pośrednictwem `ClaimsPrincipal.Current` obiekt główny zabezpieczeń.

 ```C#
 public ActionResult About()
 {
     ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
     ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
     ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
     ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
     ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

     return View();
 }
 ```

2. Skompiluj i uruchom aplikację. Jeśli nie zostało utworzone nowy użytkownik w dzierżawie z domeny onmicrosoft.com, należy teraz Aby to zrobić. Oto kroki tej procedury:

  a. Zaloguj się przy użyciu tego użytkownika, zwracając uwagę, jak tożsamość użytkowników jest odzwierciedlone na górnym pasku.

  b. Wyloguj się, a następnie zalogować się jako inny użytkownik w dzierżawie.

  c. Jeśli czujesz szczególnie ambitne, Zarejestruj i uruchomić inne wystąpienie tej aplikacji (z jego własnej clientId), a Obejrzyj jednej operacji logowania w akcji.

## <a name="next-steps"></a>Następne kroki
Aby informacje, zobacz [ukończone próbki](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (bez wartości konfiguracji).

Możesz teraz przejść do bardziej zaawansowanych tematów. Na przykład, spróbuj [zabezpieczyć interfejs API sieci Web z usługą Azure AD](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
