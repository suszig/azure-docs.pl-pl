
## <a name="use-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Użyj MSAL, aby uzyskać token dla interfejsu API programu Microsoft Graph

W tej sekcji umożliwiają MSAL uzyskać token dla interfejsu API programu Microsoft Graph.

1.  W *MainWindow.xaml.cs* plików, Dodaj odwołanie do MSAL do klasy:

    ```csharp
    using Microsoft.Identity.Client;
    ```
<!-- Workaround for Docs conversion bug -->

2. Zastąp `MainWindow` klasy kod następującym kodem:

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
#### <a name="get-a-user-token-interactively"></a>Pobierz token użytkownika interakcyjnego
Wywoływanie `AcquireTokenAsync` metoda powoduje okna, które monituje użytkowników do logowania. Aplikacje zazwyczaj wymagają użytkownikom na logowanie interakcyjne po raz pierwszy potrzebnych do uzyskania dostępu do chronionego zasobu. Może być muszą się zalogować, gdy dyskretnej operacji można uzyskać tokenu nie powiodło się (na przykład po wygaśnięciu hasła użytkownika).

#### <a name="get-a-user-token-silently"></a>Pobierz token użytkownika dyskretnej
`AcquireTokenSilentAsync` Metoda obsługuje nabycia tokenu i odnowienia bez interakcji użytkownika. Po `AcquireTokenAsync` jest wykonywana po raz pierwszy `AcquireTokenSilentAsync` jest zwykle metodę można uzyskać tokeny, które uzyskują dostęp do chronionych zasobów w kolejnych wywołaniach, ponieważ wywołania żądania lub odnowić tokeny zostały wprowadzone w trybie dyskretnym.

Po pewnym czasie `AcquireTokenSilentAsync` metoda zakończy się niepowodzeniem. Przyczyny niepowodzenia może być, czy użytkownik został wylogowany lub zmienić swoje hasło na innym urządzeniu. Gdy MSAL wykryje, że problem można rozwiązać, gdyż akcję interaktywne, jego generowane `MsalUiRequiredException` wyjątku. Aplikacja może obsłużyć tego wyjątku na dwa sposoby:

* Można wykonać połączenie przed `AcquireTokenAsync` natychmiast. To wywołanie powoduje monitowanie użytkownika do logowania. Ten wzorzec jest zazwyczaj używany w aplikacji online w przypadku, gdy brak dostępnej zawartości w trybie offline dla użytkownika. Przykładowe wygenerowane za pomocą tego Instalatora z przewodnikiem następuje tego wzorca, który można wyświetlić w czasie działania pierwszy wykonać próbki. 
    * Ponieważ żaden użytkownik nie ma korzystali z aplikacji `PublicClientApp.Users.FirstOrDefault()` zawiera wartość null i `MsalUiRequiredException` wyjątku. 
    * Następnie kod w próbce obsługuje wyjątek przez wywołanie metody `AcquireTokenAsync`, która powoduje monitowanie użytkownika do logowania.

* Ona zamiast tego prezentować oznaczenia wizualne dla użytkowników, którzy interakcyjne logowanie jest wymagane, dzięki czemu użytkownik może wybrać pozycję odpowiednich momentach do logowania. Lub aplikacji można ponowić próbę `AcquireTokenSilentAsync` później. Ten wzorzec jest często używany, gdy użytkownicy mogą używać innych funkcji aplikacji bez przerw w działaniu — na przykład, jeśli w trybie offline zawartość jest dostępna w aplikacji. W takim przypadku użytkownicy mogą wybrać, jeśli chcą, aby zalogować się do dostępu do chronionego zasobu albo Odśwież nieaktualne informacje. Alternatywnie aplikacji można zdecydować ponowić próbę `AcquireTokenSilentAsync` przywrócenie sieci po jest tymczasowo niedostępny.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Wywołanie interfejsu API programu Microsoft Graph przy użyciu tokenu, który został uzyskany

Dodaj następującą metodę nowe do Twojej `MainWindow.xaml.cs`. Metoda jest umożliwia `GET` żądania dotyczącego interfejsu API programu Graph przy użyciu nagłówka autoryzacji:

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
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Więcej informacji na temat nawiązywania połączenia przed chronionego interfejsu API REST

W tej przykładowej aplikacji, użyj `GetHttpContentWithToken` metody, aby HTTP `GET` żądania względem chronionego zasobu, który wymaga tokenu, a następnie wróć zawartości do obiektu wywołującego. Ta metoda dodaje nabytych przez niego tokenu w nagłówku autoryzacji HTTP. Dla tego przykładu zasób jest interfejsu API programu Microsoft Graph *mnie* punktu końcowego, który wyświetla informacje o profilu użytkownika.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-a-user"></a>Dodaj metodę, aby się wylogować użytkownika

Aby wylogować się z użytkownikiem, dodaj następującą metodę do Twojej `MainWindow.xaml.cs` pliku:

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
### <a name="more-information-about-user-sign-out"></a>Więcej informacji na temat wylogowania użytkownika

`SignOutButton_Click` Metoda usuwa użytkowników w buforze użytkownika MSAL kod w praktyce określa MSAL zapomnieć bieżącego użytkownika, dzięki czemu przyszłych żądań można uzyskać tokenu powiedzie się tylko wtedy, gdy jest on interaktywne.

Mimo że aplikacja, w tym przykładzie obsługuje pojedynczy użytkowników, MSAL obsługuje scenariusze, w których może być podpisana wiele kont w tym samym czasie. Przykładem jest aplikacja poczty e-mail, gdy użytkownik ma wiele kont.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Wyświetlanie informacji o tokenie podstawowe

Aby wyświetlić podstawowe informacje o tokenie, dodaj następującą metodę do Twojej *MainWindow.xaml.cs* pliku:

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

Oprócz tokenu dostępu, który jest używany do wywołania interfejsu API Graph firmy Microsoft, po loguje się użytkownik MSAL uzyskuje identyfikator tokenu. Token ten zawiera małego podzbioru informacje, które są odpowiednie dla użytkowników. `DisplayBasicTokenInfo` Metoda Wyświetla podstawowe informacje zawarte w tokenie. Na przykład wyświetla, nazwa wyświetlana użytkownika i identyfikator, a także data wygaśnięcia tokenu i ciąg reprezentujący sam token dostępu. Możesz wybrać *wywołać Microsoft interfejsu API programu Graph* przycisk wielokrotnie i zobacz, że ten sam token został ponownie dla kolejnych żądań. Można również sprawdzić Data wygaśnięcia jest rozszerzana podczas MSAL decyduje o jego czas odnowienia tokenu.
<!--end-collapse-->

