---
title: Azure AD uniwersalnych platformy Windows (UWP/XAML) wprowadzenie | Dokumentacja firmy Microsoft
description: "Aplikacje ze Sklepu Windows kompilacji integracji z usługą Azure AD, logowania, które wywołanie usługi Azure AD chronione interfejsów API w trybie OAuth."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mtillman
editor: 
ms.assetid: 3b96a6d1-270b-4ac1-b9b5-58070c896a68
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 11/30/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 8bc8c3a897363da2a8ebe7ac6bd8798c8e22ba04
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-ad-windows-universal-platform-uwpxaml-getting-started"></a>Azure AD uniwersalnych platformy Windows (UWP/XAML) wprowadzenie
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Wcześniejsze niż wersja projektów i Sklepu Windows 8.1 nie są obsługiwane w programie Visual Studio 2017 r.  Aby uzyskać więcej informacji, zobacz [Obsługiwane platformy i zgodność programu Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

Jeśli projektujesz aplikacji w Sklepie Windows Azure Active Directory (Azure AD) umożliwia proste i bezpośrednie do uwierzytelniania użytkowników z ich kont usługi Active Directory. Dzięki integracji z usługą Azure AD, aplikację bezpiecznie może używać dowolnego interfejsu API sieci web chronionej przez usługę Azure AD, takie jak interfejsami API usługi Office 365 lub interfejsu API Azure.

Dla Sklepu Windows aplikacji klasycznych wymagające dostępu do chronionych zasobów Usługa Azure AD zapewnia Active Directory Authentication Library (ADAL). Wyłącznie do celów ADAL jest umożliwia łatwe uzyskanie tokenów dostępu do aplikacji. Aby zademonstrować, jak łatwo, w tym artykule przedstawiono sposób tworzenia aplikacji ze Sklepu Windows DirectorySearcher który:

* Pobiera dostępu tokenów do wywoływania za pomocą interfejsu API programu Azure AD Graph [protokół uwierzytelniania OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Przeszukuje katalog dla użytkowników z główną nazwę użytkownika (UPN).
* Znaki użytkowników.

## <a name="before-you-get-started"></a>Przed rozpoczęciem
* Pobierz [szkielet projektu](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), lub pobrać [ukończone próbki](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip). Każdego pobrania to rozwiązanie Visual Studio 2015.
* Należy również dzierżawa usługi Azure AD, w którym do tworzenia użytkowników i rejestrowania aplikacji. Jeśli nie masz już dzierżawę, [Dowiedz się, jak kupić](active-directory-howto-tenant.md).

Gdy wszystko będzie gotowe, wykonaj procedury opisane w trzech kolejnych sekcjach.

## <a name="step-1-register-the-directorysearcher-app"></a>Krok 1: Rejestrowanie aplikacji DirectorySearcher
Aby umożliwić aplikacji można uzyskać tokeny, należy najpierw zarejestrować ją w dzierżawie usługi Azure AD i udzielić jej uprawnienia dostępu do interfejsu API programu Azure AD Graph. Oto kroki tej procedury:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Na górnym pasku kliknij konto. Następnie w obszarze **katalogu** wybierz dzierżawy usługi Active Directory, które chcesz zarejestrować aplikację.
3. Kliknij przycisk **więcej usług** w okienku po lewej stronie, a następnie wybierz **usługi Azure Active Directory**.
4. Kliknij przycisk **rejestracji aplikacji**, a następnie wybierz **Dodaj**.
5. Postępuj zgodnie z monitami, aby utworzyć **natywną aplikację kliencką**.
  * **Nazwa** opisuje aplikacji dla użytkowników.
  * **Identyfikator URI przekierowania** jest kombinacją schemat i ciąg, korzystającą z usługi Azure AD w celu zwracać odpowiedzi tokenu. Wprowadź wartość symbolu zastępczego teraz (na przykład **http://DirectorySearcher**). Później będzie Zamień wartość.
6. Po zakończeniu rejestracji usługi Azure AD przypisuje aplikacji identyfikatora aplikacji Skopiuj wartość na **aplikacji** karcie, ponieważ będzie on potrzebny później.
7. Na **ustawienia** wybierz pozycję **wymagane uprawnienia**, a następnie wybierz **Dodaj**.
8. Dla **usługi Azure Active Directory** aplikacji, wybierz opcję **Microsoft Graph** jako interfejsu API.
9. W obszarze **delegowane uprawnienia**, Dodaj **dostępu do katalogu jako zalogowany użytkownik** uprawnienia. W ten sposób umożliwia aplikacji zapytania interfejsu API programu Graph dla użytkowników.

## <a name="step-2-install-and-configure-adal"></a>Krok 2: Instalowanie i konfigurowanie biblioteki ADAL
Teraz, gdy masz aplikację w usłudze Azure AD, można zainstalować biblioteki ADAL i wpisz swój kod dotyczące tożsamości. Aby włączyć biblioteki ADAL do komunikowania się z usługą Azure AD, nadaj mu pewnych informacji o rejestracji aplikacji.

1. Dodawanie biblioteki ADAL do projektu DirectorySearcher przy użyciu konsoli Menedżera pakietów.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

2. W projekcie DirectorySearcher Otwórz MainPage.xaml.cs.
3. Zastąp wartości w **wartości konfiguracji** region z wartościami, które zostały wprowadzone w portalu Azure. Kod odwołuje się do tych wartości, przy każdym użyciu biblioteki ADAL.
  * *Dzierżawy* jest domeny dzierżawy usługi Azure AD (np. contoso.onmicrosoft.com).
  * *ClientId* jest identyfikator klienta aplikacji, który został skopiowany z portalu.
4. Teraz musisz odnaleźć identyfikator URI wywołania zwrotnego dla aplikacji ze Sklepu Windows. Ustaw punkt przerwania w tym wierszu w `MainPage` metody:
    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```
5. Skompiluj rozwiązanie, upewniając się, że wszystkie odwołania do pakietu zostaną przywrócone. Jeśli brakuje żadnych pakietów, otwórz Menedżera pakietów NuGet i przywrócić je.
6. Uruchom aplikację i skopiuj wartość `redirectUri` gdy zostaje trafiony punkt przerwania. Wartość powinna wyglądać jak poniżej:

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

7. Ponownie **ustawienia** kartę aplikacji w portalu Azure, należy dodać **RedirectUri** z poprzedniej wartości.  

## <a name="step-3-use-adal-to-get-tokens-from-azure-ad"></a>Krok 3: Użyj biblioteki ADAL w celu uzyskania tokenów z usługi Azure AD
Zasada podstawowa za ADAL jest, że zawsze, gdy aplikacja potrzebuje tokenu dostępu, po prostu wywołuje `authContext.AcquireToken(…)`, a reszta biblioteki ADAL.  

1. Inicjowanie aplikacji `AuthenticationContext`, która jest klasy podstawowej biblioteki adal. Ta akcja przekazuje ADAL współrzędne wymaga do komunikacji z usługą Azure AD i poinformuj go, jak pamięci podręcznej tokenów.

    ```csharp
    public MainPage()
    {
        ...

        authContext = new AuthenticationContext(authority);
    }
    ```

2. Zlokalizuj `Search(...)` metodę, która jest wywoływana, gdy użytkownik kliknie **wyszukiwania** przycisk w Interfejsie użytkownika aplikacji. Ta metoda zgłasza żądanie get interfejsu API Azure AD Graph zapytania dla użytkowników, których nazwy UPN rozpoczyna się od danego wyszukiwanego. Aby odpytać interfejsu API programu Graph, zawierać token dostępu w żądaniu **autoryzacji** nagłówka. Jest to, gdzie ADAL jest dostarczany.

    ```csharp
    private async void Search(object sender, RoutedEventArgs e)
    {
        ...
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
        }
        catch (AdalException ex)
        {
            if (ex.ErrorCode != "authentication_canceled")
            {
                ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
            }
            return;
        }
        ...
    }
    ```
    Gdy aplikacja żąda token przez wywołanie metody `AcquireTokenAsync(...)`, ADAL próbuje zwrócić token bez monitowania użytkownika o poświadczenia. Jeśli ADAL Określa, czy użytkownik powinien logować się do pobrania tokenu, zostanie wyświetlone okno dialogowe logowania, zbiera poświadczenia użytkownika i zwraca token po pomyślnym uwierzytelnieniu. Jeśli nie można zwrócić token z jakiejkolwiek przyczyny, ADAL *AuthenticationResult* stanu błędu.
3. Teraz nadszedł czas na korzystanie z tokenu dostępu, które zostało zakupione. Również w `Search(...)` metody, Dołącz token na żądanie get interfejsu API programu Graph w **autoryzacji** nagłówka:

    ```csharp
    // Add the access token to the Authorization header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

    ```
4. Można użyć `AuthenticationResult` obiektu, aby wyświetlić informacje o użytkowniku w aplikacji, takich jak identyfikator użytkownika:

    ```csharp
    // Update the page UI to represent the signed-in user
    ActiveUser.Text = result.UserInfo.DisplayableId;
    ```
5. Umożliwia także biblioteki ADAL do podpisania użytkowników poza aplikacją. Po kliknięciu przez użytkownika **Wyloguj** przycisku, upewnij się, że następne wywołanie `AcquireTokenAsync(...)` przedstawia widok logowania. Przy użyciu biblioteki ADAL ta akcja jest tak proste, jak czyszczenie pamięci podręcznej tokenu:

    ```csharp
    private void SignOut()
    {
        // Clear session state from the token cache.
        authContext.TokenCache.Clear();

        ...
    }
    ```

## <a name="whats-next"></a>Co dalej
Masz teraz pracy aplikacji do Sklepu Windows, który może uwierzytelniać użytkowników, bezpiecznie wywołać przy użyciu protokołu OAuth 2.0 interfejsów API sieci web i uzyskać podstawowe informacje o użytkowniku.

Jeśli nie zostało już wypełnione dzierżawy z użytkownikami, należy teraz Aby to zrobić.
1. Uruchom aplikację DirectorySearcher, a następnie zaloguj się przy użyciu jednego z użytkowników.
2. Wyszukiwać innych użytkowników, w oparciu o ich nazwy UPN.
3. Zamknij aplikację i ponownie ją uruchomić. Zwróć uwagę, jak sesji użytkownika pozostanie niezmieniona.
4. Wylogować klikając prawym przyciskiem myszy, aby wyświetlić dolnym pasku, a następnie zalogować się jako inny użytkownik.

ADAL można łatwo zastosować te wspólne funkcje tożsamości w aplikacji. Go odpowiedzialna za całą pracę dirty, takich jak zarządzanie pamięci podręcznej, obsługa protokołu OAuth, przedstawiający użytkownika za pomocą interfejsu użytkownika, nazwy logowania i odświeżanie wygasła tokenów. Należy znać tylko jednego wywołania interfejsu API, `authContext.AcquireToken*(…)`.

Odwołania, Pobierz [ukończone próbki](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (bez wartości konfiguracji).

Możesz teraz przejść do tożsamości dodatkowe scenariusze. Na przykład, spróbuj [zabezpieczyć interfejs API sieci Web .NET z usługą Azure AD](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
