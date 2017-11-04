---
title: "Usługi Azure AD v2 Windows Desktop pobierania pracę - Użyj | Dokumentacja firmy Microsoft"
description: "Jak aplikacje .NET pulpitu systemu Windows (XAML) można wywołać interfejsu API, które wymagają tokenów dostępu przez punkt końcowy w wersji 2 usługi Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 826ba0a00b26993d4f37f0a8ce587d7bb77e7eb4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Użyj biblioteki uwierzytelniania firmy Microsoft (MSAL), aby uzyskać token dla interfejsu API programu Microsoft Graph

W tej sekcji przedstawia sposób użycia MSAL w celu pobrania tokenu z interfejsu API programu Microsoft Graph.

1.  W `MainWindow.xaml.cs`, Dodaj odwołanie do biblioteki MSAL do klasy:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Zastąp <code>MainWindow</code> kod z klasy:
</li>
</ol>

```csharp
public partial class MainWindow : Window
{
    //Set the API Endpoint to Graph 'me' endpoint
    string _graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

    //Set the scope for API call to user.read
    string[] _scopes = new string[] { "user.read" };


    public MainWindow()
    {
        InitializeComponent();
    }

    /// <summary>
    /// Call AcquireTokenAsync - to acquire a token requiring user to sign-in
    /// </summary>
    private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
    {
        AuthenticationResult authResult = null;

        try
        {
            authResult = await App.PublicClientApp.AcquireTokenSilentAsync(_scopes, App.PublicClientApp.Users.FirstOrDefault());
        }
        catch (MsalUiRequiredException ex)
        {
            // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need to call AcquireTokenAsync to acquire a token
            System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

            try
            {
                authResult = await App.PublicClientApp.AcquireTokenAsync(_scopes);
            }
            catch (MsalException msalex)
            {
                ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
            }
        }
        catch (Exception ex)
        {
            ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
            return;
        }

        if (authResult != null)
        {
            ResultText.Text = await GetHttpContentWithToken(_graphAPIEndpoint, authResult.AccessToken);
            DisplayBasicTokenInfo(authResult);
            this.SignOutButton.Visibility = Visibility.Visible;
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Więcej informacji
#### <a name="getting-a-user-token-interactive"></a>Pobieranie tokenu użytkownika interakcyjnego
Wywoływanie `AcquireTokenAsync` metody wyniki w oknie monitowania użytkownika do logowania. Aplikacje wymagają zazwyczaj użytkownika do logowania interakcyjnego potrzebnych do uzyskania dostępu do chronionego zasobu po raz pierwszy lub gdy dyskretnej operacji można uzyskać tokenu kończy się niepowodzeniem (np. hasło użytkownika wygasło).

#### <a name="getting-a-user-token-silently"></a>Pobieranie tokenu użytkownika dyskretnej
`AcquireTokenSilentAsync`obsługuje przejęć tokenu i wznowienie bez interakcji użytkownika. Po `AcquireTokenAsync` jest wykonywana po raz pierwszy `AcquireTokenSilentAsync` jest zwykle metody używane do uzyskiwania tokenów umożliwiają dostęp do chronionych zasobów w kolejnych wywołaniach — jako wywołania żądania lub odnowić tokeny zostały wprowadzone w trybie dyskretnym.
Po pewnym czasie `AcquireTokenSilentAsync` zakończy się niepowodzeniem — np. użytkownik został wylogowany, lub zmieniono hasła na innym urządzeniu. Gdy MSAL wykryje, że problem można rozwiązać, gdyż akcję interaktywne, jego generowane `MsalUiRequiredException`. Aplikacja może obsłużyć tego wyjątku na dwa sposoby:

1.  Nawiązywanie połączeń z `AcquireTokenAsync` natychmiast, która powoduje monitowanie użytkownika o logowania. Ten wzorzec jest zazwyczaj używany w aplikacji online w przypadku, gdy nie żadnej zawartości w trybie offline w aplikacji dostępnej dla użytkownika. Przykładowe wygenerowane za pomocą tego Instalatora z przewodnikiem używa tego wzorca: widoczny w czasie działania pierwszy wykonać próbki: ponieważ żaden użytkownik nie jest nigdy korzystali z aplikacji, `PublicClientApp.Users.FirstOrDefault()` będzie zawierać wartości null i `MsalUiRequiredException` zostanie wygenerowany wyjątek. Następnie kod w próbce obsługuje wyjątek przez wywołanie metody `AcquireTokenAsync` co powoduje monitowanie użytkownika o logowania.

2.  Aplikacje można wprowadzić oznaczenia wizualne dla użytkownika, który interakcyjnego logowania jest wymagana, więc użytkownik może wybrać, właściwym czasie zalogować się lub aplikacji można ponowić próbę `AcquireTokenSilentAsync` w późniejszym czasie. To zwykle jest używana, gdy użytkownik może użyć innych funkcji aplikacji bez są zakłócone — na przykład Brak dostępnej zawartości w trybie offline w aplikacji. W takim przypadku użytkownik można wybrać, jeśli chcą Zaloguj się do uzyskania dostępu do zabezpieczonych zasobów lub Odśwież nieaktualne informacje lub aplikacji można zdecydować ponowić próbę `AcquireTokenSilentAsync` przywrócenie sieci po są tymczasowo niedostępne.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Wywołanie interfejsu API programu Microsoft Graph przy użyciu tokenu, który został uzyskany

1. Dodawanie nowej metody poniżej do Twojej `MainWindow.xaml.cs`. Metoda jest umożliwia `GET` żądania interfejsu API programu Graph przy użyciu nagłówka autoryzacji:

```csharp
/// <summary>
/// Perform an HTTP GET request to a URL using an HTTP Authorization header
/// </summary>
/// <param name="url">The URL</param>
/// <param name="token">The token</param>
/// <returns>String containing the results of the GET operation</returns>
public async Task<string> GetHttpContentWithToken(string url, string token)
{
    var httpClient = new System.Net.Http.HttpClient();
    System.Net.Http.HttpResponseMessage response;
    try
    {
        var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
        //Add the token in Authorization header
        request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
        response = await httpClient.SendAsync(request);
        var content = await response.Content.ReadAsStringAsync();
        return content;
    }
    catch (Exception ex)
    {
        return ex.ToString();
    }
}
```
<!--start-collapse-->
### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Więcej informacji na temat nawiązywania połączenia przed chronionego interfejsu API REST

W tej przykładowej aplikacji `GetHttpContentWithToken` używa metody w celu HTTP `GET` żądania względem chronionego zasobu, który wymaga tokenu, a następnie wróć zawartości do obiektu wywołującego. Ta metoda dodaje token nabyte w *nagłówek autoryzacji HTTP*. Dla tego przykładu zasób jest interfejsu API programu Microsoft Graph *mnie* punktu końcowego — Wyświetla informacje o profilu użytkownika.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Dodaj metodę, aby się wylogować użytkownika

1. Dodaj następującą metodę do Twojej `MainWindow.xaml.cs` się wylogować użytkownika:

```csharp
/// <summary>
/// Sign out the current user
/// </summary>
private void SignOutButton_Click(object sender, RoutedEventArgs e)
{
    if (App.PublicClientApp.Users.Any())
    {
        try
        {
            App.PublicClientApp.Remove(App.PublicClientApp.Users.FirstOrDefault());
            this.ResultText.Text = "User has signed-out";
            this.CallGraphButton.Visibility = Visibility.Visible;
            this.SignOutButton.Visibility = Visibility.Collapsed;
        }
        catch (MsalException ex)
        {
            ResultText.Text = $"Error signing-out user: {ex.Message}";
        }
    }
}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>Więcej informacji dotyczących wylogowania

`SignOutButton_Click`Usuwa użytkownika z pamięci podręcznej użytkownika MSAL — skutecznie poinformuje MSAL zapomnieć bieżącego użytkownika, więc żądanie przyszłych, aby uzyskać token powiodą się tylko wtedy, gdy staje się interaktywne.
Mimo że w tym przykładzie aplikacja obsługuje pojedynczego użytkownika, MSAL obsługuje scenariusze, w której wiele kont może być zalogowany na tym samym czasie — przykładem jest aplikacja poczty e-mail, w których użytkownik ma wiele kont.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Wyświetlanie informacji o tokenie podstawowe

1. Dodaj następującą metodę do do Twojej `MainWindow.xaml.cs` do wyświetlania podstawowych informacji o tokenie:

```csharp
/// <summary>
/// Display basic information contained in the token
/// </summary>
private void DisplayBasicTokenInfo(AuthenticationResult authResult)
{
    TokenInfoText.Text = "";
    if (authResult != null)
    {
        TokenInfoText.Text += $"Name: {authResult.User.Name}" + Environment.NewLine;
        TokenInfoText.Text += $"Username: {authResult.User.DisplayableId}" + Environment.NewLine;
        TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
        TokenInfoText.Text += $"Access Token: {authResult.AccessToken}" + Environment.NewLine;
    }
}
```
<!--start-collapse-->
### <a name="more-information"></a>Więcej informacji

Tokeny nabyte za pośrednictwem *OpenID Connect* również zawierać małego podzbioru informacji dotyczących użytkownika. `DisplayBasicTokenInfo`zawiera podstawowe informacje zawarte w tokenie: na przykład nazwa wyświetlana użytkownika i identyfikatorze, oraz datę wygaśnięcia tokenu i ciąg reprezentujący dostępu token samej siebie. Te informacje są wyświetlane dla można zobaczyć. Naciśnij klawisz *wywołać Microsoft interfejsu API programu Graph* przycisk wielokrotnie i zobacz, że ten sam token został ponownie dla kolejnych żądań. Można również sprawdzić Data wygaśnięcia jest rozszerzana podczas MSAL decyduje o jego czas odnowienia tokenu.
<!--end-collapse-->

