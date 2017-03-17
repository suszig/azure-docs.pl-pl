---
title: "Usługa Azure AD B2C | Microsoft Docs"
description: "Jak utworzyć interfejs API sieci Web programu .NET przy użyciu usługi Azure Active Directory B2C zabezpieczonej przy użyciu tokenów dostępu protokołu OAuth 2.0 na potrzeby uwierzytelniania."
services: active-directory-b2c
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 7146ed7f-2eb5-49e9-8d8b-ea1a895e1966
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 1062af9abfd167dd251621a43943dea399aed027
ms.lasthandoff: 12/28/2016


---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: tworzenie interfejsu API sieci Web platformy .NET
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Usługa Azure Active Directory (Azure AD) B2C umożliwia zabezpieczanie interfejsu API sieci Web za pomocą tokenów dostępu protokołu OAuth 2.0. Tokeny te służą aplikacjom klienckim używającym usługi Azure AD B2C do uwierzytelniania w obrębie interfejsu API. W tym artykule pokazano, jak utworzyć interfejs API „lista zadań do wykonania” kontrolera MVC (Model-View-Controler) platformy .NET umożliwiający użytkownikom wykonywanie zadań CRUD. Interfejs API sieci Web jest zabezpieczony za pomocą usługi Azure AD B2C i zezwala na zarządzanie listami zadań do wykonania tylko uwierzytelnionym użytkownikom.

## <a name="create-an-azure-ad-b2c-directory"></a>Tworzenie katalogu usługi Azure AD B2C
Przed rozpoczęciem korzystania z usługi Azure AD B2C należy utworzyć katalog lub dzierżawę. Katalog jest kontenerem dla wszystkich użytkowników, aplikacji, grup i innych elementów. Jeśli jeszcze go nie masz, [utwórz katalog usługi B2C](active-directory-b2c-get-started.md), zanim przejdziesz dalej.

## <a name="create-an-application"></a>Tworzenie aplikacji
Następnie musisz utworzyć aplikację w katalogu usługi B2C. Dzięki temu informacje wymagane do bezpiecznego komunikowania się z aplikacją będą przekazywane do usługi Azure AD. Aby utworzyć aplikację, postępuj zgodnie z [tymi instrukcjami](active-directory-b2c-app-registration.md). Należy pamiętać o wykonaniu następujących czynności:

* Uwzględnij **aplikację sieci Web** lub **interfejs API sieci Web** w aplikacji.
* Użyj **identyfikatora URI przekierowania** `https://localhost:44316/` w aplikacji sieci Web. Jest to domyślna lokalizacja klienta aplikacji sieci Web dla tego przykładu kodu.
* Skopiuj **identyfikator aplikacji** przypisany do aplikacji. Będzie on potrzebny później.

  [!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Tworzenie zasad
W usłudze Azure AD B2C każde działanie użytkownika jest definiowane przy użyciu [zasad](active-directory-b2c-reference-policies.md). W tym przykładzie kodu klient obejmuje trzy funkcje tożsamości: rejestrację, logowanie się i edytowanie profilu. Dla każdego typu musisz utworzyć środowisko zgodnie z opisem w [artykule informacyjnym na temat zasad](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Podczas tworzenia trzech zbiorów zasad należy koniecznie:

* W bloku dostawców tożsamości wybrać opcję **tworzenia konta przy użyciu identyfikatora użytkownika** lub **tworzenia konta przy użyciu adresu e-mail**.
* Wybrać wartość **Nazwa wyświetlana** i inne atrybuty tworzenia konta w zasadach tworzenia konta.
* Wybrać oświadczenia **Nazwa wyświetlana** i **Identyfikator obiektu** jako oświadczenia aplikacji dla wszystkich zasad. Można również wybrać inne oświadczenia.
* Skopiować każdą utworzoną wartość **Nazwa** zasad. Te nazwy zasad będą potrzebne później.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po pomyślnym utworzeniu trzech zasad możesz rozpocząć tworzenie aplikacji.

## <a name="download-the-code"></a>Pobieranie kodu
Kod używany w tym samouczku [jest przechowywany w serwisie GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet). Aby samodzielnie tworzyć przykładowy kod w trakcie pracy, możesz [pobrać plik ZIP ze szkieletem projektu](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). Można również sklonować szkielet:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

Ukończona aplikacja jest również [dostępna jako plik ZIP](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) lub w gałęzi `complete` tego samego repozytorium.

Po pobraniu przykładu kodu otwórz plik SLN programu Visual Studio, aby rozpocząć. Plik rozwiązania zawiera dwa projekty: `TaskWebApp` i `TaskService`. `TaskWebApp` to aplikacja sieci Web z architekturą MVC, z której korzysta użytkownik. `TaskService` to interfejs API zaplecza aplikacji, który przechowuje listy zadań do wykonania poszczególnych użytkowników.

## <a name="configure-the-task-web-app"></a>Konfigurowanie aplikacji sieci Web do obsługi zadań
Gdy użytkownik korzysta z aplikacji `TaskWebApp`, klient wysyła żądania do usługi Azure AD i otrzymuje w odpowiedzi tokeny, których można użyć do wywołania interfejsu API sieci Web aplikacji `TaskService`. Aby zalogować użytkownika i uzyskać tokeny, projektowi `TaskWebApp` należy podać niektóre informacje o aplikacji. W projekcie `TaskWebApp` otwórz plik `web.config` w katalogu głównym i zastąp wartości w sekcji `<appSettings>`.  Wartości `AadInstance`, `RedirectUri` i `TaskServiceUrl` możesz zostawić bez zmian.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

W tym artykule nie omówiono tworzenia klienta `TaskWebApp`.  Aby dowiedzieć się, jak utworzyć aplikację sieci Web, używając usługi Azure AD B2C, zobacz [nasz samouczek dotyczący aplikacji sieci Web platformy .NET](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="secure-the-api"></a>Zabezpieczanie interfejsu API
Jeśli klient wywołuje interfejs API w imieniu użytkowników, należy zabezpieczyć projekt `TaskService` za pomocą tokenów elementu nośnego OAuth 2.0. Interfejs API może akceptować i weryfikować tokeny przy użyciu biblioteki interfejsu OWIN (Open Web Interface for .NET) firmy Microsoft.

### <a name="install-owin"></a>Instalowanie interfejsu OWIN
Rozpocznij od zainstalowania potoku uwierzytelniania OAuth interfejsu OWIN:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>Wprowadzanie szczegółów B2C
Otwórz plik `web.config` w katalogu głównym projektu `TaskService` i zastąp wartości w sekcji `<appSettings>`. Te wartości będą używane w bibliotece interfejsu API i OWIN.  Wartość `AadInstance` możesz zostawić bez zmian.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>Dodawanie klasy początkowej OWIN
Dodaj klasę początkową OWIN do projektu `TaskService` o nazwie `Startup.cs`.  Kliknij projekt prawym przyciskiem myszy, wybierz kolejno pozycje **Dodaj** i **Nowy element**, a następnie wyszukaj interfejs OWIN.

```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>Konfigurowanie uwierzytelniania OAuth 2.0
Otwórz plik `App_Start\Startup.Auth.cs` i zaimplementuj metodę `ConfigureAuth(...)`:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>Zabezpieczanie kontrolera zadań
Po skonfigurowaniu aplikacji do używania uwierzytelniania OAuth 2.0 możesz zabezpieczyć interfejs API sieci Web, dodając tag `[Authorize]` do kontrolera zadań. Jest to kontroler, w obrębie którego odbywają się wszystkie operacje edytowania listy zadań do wykonania, dlatego należy zabezpieczyć cały kontroler na poziomie klasy. Można również dodać tag `[Authorize]` do poszczególnych czynności w celu uzyskania ściślejszej kontroli.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Uzyskiwanie informacji o użytkowniku z tokenu
`TasksController` przechowuje zadania w bazie danych. Z każdym zadaniem jest skojarzony użytkownik będący jego „właścicielem”. Właściciel jest identyfikowany za pomocą **identyfikatora obiektu** użytkownika. (Dlatego trzeba było dodać identyfikator obiektu jako oświadczenie aplikacji we wszystkich zasadach).

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>Uruchamianie przykładowej aplikacji
Na koniec skompiluj i uruchom oba projekty: `TaskWebApp` i `TaskService`. Utwórz konto aplikacji, używając adresu e-mail lub nazwy użytkownika. Utwórz kilka zadań na liście użytkownika i zwróć uwagę na to, że zostają one zachowane w interfejsie API nawet po zatrzymaniu i ponownym uruchomieniu klienta.

## <a name="edit-your-policies"></a>Edytowanie zasad
Po zabezpieczeniu interfejsu API za pomocą usługi Azure AD B2C możesz eksperymentować z zasadami aplikacji i sprawdzać skutki (lub ich brak) powiązane z interfejsem API. Możesz manipulować oświadczeniami aplikacji w zasadach i zmieniać informacje o użytkowniku dostępne w interfejsie API sieci Web. Wszystkie dodane oświadczenia będą dostępne dla interfejsu API sieci Web w architekturze MVC programu .NET w obiekcie `ClaimsPrincipal`, zgodnie z opisem we wcześniejszej części tego artykułu.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->

