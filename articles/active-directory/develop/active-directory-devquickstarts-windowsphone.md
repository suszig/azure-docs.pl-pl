---
title: "Wprowadzenie do usługi Azure AD Windows Phone | Dokumentacja firmy Microsoft"
description: "Sposób tworzenia aplikacji Windows Phone, która integruje się z usługą Azure AD w celu logowania się i wymaga usługi Azure AD chronione w trybie OAuth interfejsów API."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mtillman
editor: 
ms.assetid: 66f5ac20-5e1f-4b9d-bb99-9b3305e26416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 11/30/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: fc243f4ca6d323bf2b90cb9ab8fa9b77e1df9f8e
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="azure-ad-windows-phone-getting-started"></a>Azure AD Windows Phone wprowadzenie
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Projekty systemu Windows Phone 8.1 i wcześniejszych nie są obsługiwane w programie Visual Studio 2017 r.  Aby uzyskać więcej informacji, zobacz [Obsługiwane platformy i zgodność programu Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

Jeśli projektujesz aplikacji Windows Phone 8.1, usługi Azure AD umożliwia proste i bezpośrednie umożliwia uwierzytelnianie użytkowników przy użyciu ich kont usługi Active Directory.  Umożliwia również aplikacji bezpiecznego używać dowolnego składnika web API chronione przez usługę Azure AD, takie jak interfejsami API usługi Office 365 lub interfejsu API platformy Azure.

> [!NOTE]
> Ten przykładowy kod używa biblioteki ADAL w wersji 2.0.  Aby uzyskać najnowsze technologie, warto zamiast tego spróbuj naszych [samouczek aplikacji uniwersalnych systemu Windows przy użyciu biblioteki ADAL w wersji 3.0](active-directory-devquickstarts-windowsstore.md).  Jeśli tworzysz rzeczywiście aplikacji dla systemu Windows Phone 8.1, jest właściwym miejscu.  ADAL w wersji 2.0 jest nadal obsługiwane i jest zalecanym sposobem tworzenia agianst aplikacji Windows Phone 8.1 przy użyciu usługi Azure AD.
> 
> 

Dla platformy .NET native klientów, którzy potrzebują dostępu do chronionych zasobów Usługa Azure AD zapewnia biblioteki uwierzytelniania usługi Active Directory lub biblioteki ADAL.  ADAL w wyłącznie życia ma na celu ułatwić aplikację, aby uzyskać dostęp do tokenów.  Aby zademonstrować, jak łatwo jest, w tym miejscu będzie budujemy "katalog modułu wyszukującego" aplikacji systemu Windows Phone 8.1 który:

* Pobiera dostępu tokenów do wywoływania za pomocą interfejsu API usługi Azure AD Graph [protokół uwierzytelniania OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Przeszukuje katalog dla użytkowników z danym głównej nazwy użytkownika.
* Znaki użytkowników.

Aby utworzyć pełną działającą aplikację, musisz:

1. Zarejestrować aplikację w usłudze Azure AD.
2. Instalowanie i konfigurowanie biblioteki ADAL.
3. Użyj biblioteki ADAL, aby uzyskać tokenów z usługi Azure AD.

Aby rozpocząć, [pobrać szkielet projektu](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) lub [Pobieranie ukończone próbki](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Każdy jest rozwiązanie Visual Studio 2013.  Należy również dzierżawa usługi Azure AD można tworzyć użytkowników i zarejestrowanie aplikacji.  Jeśli nie masz już dzierżawę, [Dowiedz się, jak kupić](active-directory-howto-tenant.md).

## <a name="1-register-the-directory-searcher-application"></a>1. Zarejestrować aplikację poszukującego katalogu
Aby włączyć swoją aplikację w celu uzyskania tokenów, musisz najpierw zarejestrować ją w dzierżawie usługi Azure AD i udzielić jej uprawnienia dostępu do interfejsu API programu Azure AD Graph:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Na górnym pasku, kliknij na Twoim koncie i w obszarze **katalogu** wybierz dzierżawy usługi Active Directory, w którym chcesz zarejestrować aplikację.
3. Polecenie **wszystkie usługi** w nawigacji po lewej stronie i wybierz polecenie **usługi Azure Active Directory**.
4. Polecenie **rejestracji aplikacji** i wybierz polecenie **Dodaj**.
5. Postępuj zgodnie z monitami i utworzyć nową **natywną aplikację kliencką**.
  * **Nazwa** aplikacji będzie opisywać aplikację dla użytkowników końcowych
  * **Identyfikator Uri przekierowania** jest kombinacją schemat i ciąg, używanego do zwracania odpowiedzi tokenu usługi Azure AD.  Wprowadź wartość symbolu zastępczego teraz, np. `http://DirectorySearcher`.  Firma Microsoft będzie później Zastąp tę wartość.
6. Po zakończeniu rejestracji usługi AAD przypisze aplikacji Unikatowy identyfikator aplikacji.  Ta wartość jest potrzebny w kolejnych sekcjach, dlatego skopiuj go na karcie aplikacji.
7. Z **ustawienia** wybierz pozycję **wymagane uprawnienia** i wybierz polecenie **Dodaj**. Wybierz **Microsoft Graph** jako interfejsu API i Dodaj **Czytaj dane katalogu** uprawnienie w obszarze **delegowane uprawnienia**.  Umożliwi to aplikacja do zapytania interfejsu API programu Graph dla użytkowników.

## <a name="2-install--configure-adal"></a>2. Instalowanie i konfigurowanie biblioteki ADAL
Teraz, gdy masz aplikacji w usłudze Azure AD, można zainstalować biblioteki ADAL i wpisz swój kod dotyczące tożsamości.  Biblioteki ADAL można było nawiązać połączenia z usługą Azure AD, należy dostarczyć informacji o rejestracji aplikacji.

* Rozpocznij, dodając biblioteki ADAL do projektu DirectorySearcher przy użyciu konsoli Menedżera pakietów.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* W projekcie DirectorySearcher Otwórz `MainPage.xaml.cs`.  Zastąp wartości w `Config Values` region do wartości wejściowych do portalu Azure.  Kod będzie odwoływać tych wartości, przy każdym użyciu biblioteki ADAL.
  * `tenant` Jest domeny dzierżawy usługi Azure AD, np. contoso.onmicrosoft.com
  * `clientId` Jest clientId aplikacji został skopiowany z portalu.
* Teraz musisz odnaleźć identyfikator uri wywołania zwrotnego dla aplikacji Windows Phone.  Ustaw punkt przerwania w tym wierszu w `MainPage` metody:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
* Uruchom aplikację i skopiuj Odłóż wartość `redirectUri` gdy zostaje trafiony punkt przerwania.  Powinien on wyglądać podobnie jak

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

* Ponownie **Konfiguruj** kartę aplikacji w portalu zarządzania Azure, zastąp wartość **RedirectUri** o tej wartości.  

## <a name="3-use-adal-to-get-tokens-from-aad"></a>3. Aby uzyskać tokenów z usługi AAD, użyj biblioteki ADAL
Zasada podstawowa za ADAL jest, że zawsze, gdy Twoja aplikacja powinna tokenu dostępu, po prostu wywołuje `authContext.AcquireToken(…)`, a reszta biblioteki ADAL.  

* Pierwszym krokiem jest zainicjowanie aplikacji `AuthenticationContext` -ADAL obiektu klasy podstawowej.  Jest to, gdy przekazujesz ADAL współrzędne wymaga do komunikacji z usługą Azure AD i poinformuj go, jak pamięci podręcznej tokenów.

```csharp
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

* Znajdź teraz `Search(...)` metodę, która będzie wywoływana, gdy przycisk cliks użytkownika "Wyszukiwanie" w Interfejsie użytkownika aplikacji.  Ta metoda zgłasza żądanie GET interfejsu API Azure AD Graph zapytania dla użytkowników, których nazwy UPN rozpoczyna się od danego wyszukiwanego.  Jednak aby można było wykonać kwerendę interfejsu API programu Graph, należy uwzględnić ' access_token ' w `Authorization` nagłówka żądania — jest to, gdzie ADAL jest dostarczany.

```csharp
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try to get a token without triggering any user prompt.
    // ADAL will check whether the requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained the QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
* Jeśli konieczne jest uwierzytelnianie interakcyjne, ADAL użyje Książka adresowa systemu (Windows, Windows Phone w sieci Web uwierzytelniania Broker) i [modelu kontynuacji](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) do wyświetlania na stronie tworzenia konta usługi Azure AD.  Gdy użytkownik się zaloguje, Twoja aplikacja powinna przekazać ADAL wyniki interakcji Książka adresowa systemu Windows.  Jest tak proste, jak implementacja `ContinueWebAuthentication` interfejsu:

```csharp
// This method is automatically invoked when the application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass the authentication interaction results to ADAL, which will
    // conclude the token acquisition operation and invoke the callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

* Teraz nadszedł czas na użycie `AuthenticationResult` ADAL zwrócona do aplikacji.  W `QueryGraph(...)` wywołania zwrotnego, Dołącz ' access_token ' nabył na żądanie GET w nagłówku autoryzacji:

```csharp
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
* Można również użyć `AuthenticationResult` obiektu, aby wyświetlić informacje o użytkowniku w aplikacji. W `QueryGraph(...)` — metoda, za pomocą wynik można wyświetlić identyfikator użytkownika na stronie:

```csharp
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
* Na koniec służy ADAL logowania użytkownika z aplikacji, a także.  Gdy użytkownik kliknie przycisk "Wyloguj", chcemy się upewnić się, że następne wywołanie `AcquireTokenSilentAsync(...)` zakończy się niepowodzeniem.  Przy użyciu biblioteki ADAL jest tak proste, jak czyszczenie pamięci podręcznej tokenu:

```csharp
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Gratulacje! Możesz teraz działa aplikacji Windows Phone, który ma możliwość uwierzytelniania użytkowników, bezpiecznie wywoływać interfejsy API sieci Web przy użyciu protokołu OAuth 2.0 i uzyskać podstawowe informacje o użytkowniku.  Jeśli nie jest jeszcze nadszedł czas do wypełnienia dzierżawy z niektórych użytkowników.  Uruchom aplikację DirectorySearcher i zaloguj się przy użyciu jednej z tych użytkowników.  Wyszukiwać innych użytkowników, w oparciu o ich nazwy UPN.  Zamknij aplikację i uruchom je ponownie.  Zwróć uwagę, jak sesji użytkownika pozostanie niezmieniona.  Wyloguj się i zaloguj się ponownie jako inny użytkownik.

Biblioteka ADAL można łatwo zastosować wszystkie te typowe funkcje tożsamości w aplikacji.  Zapewnia obsługę pracy dirty dla Ciebie — Zarządzanie pamięci podręcznej, obsługa protokołu OAuth, przedstawiający użytkownika za pomocą interfejsu użytkownika, odświeżanie wygaśnięcia tokenów i inne nazwy logowania.  Tak naprawdę trzeba wiedzieć jednego wywołania interfejsu API, jest `authContext.AcquireToken*(…)`.

Odwołania, jest dostępne ukończone próbka (bez wartości konfiguracji) [tutaj](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Możesz teraz przejść do tożsamości dodatkowe scenariusze.  Można spróbować:

[Zabezpieczanie interfejsu API za pomocą usługi Azure AD sieci Web .NET >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

