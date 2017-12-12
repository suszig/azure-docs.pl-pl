# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Wywoływanie Microsoft Graph API z pulpitu aplikacji systemu Windows

W tym przewodniku pokazano, jak natywnych aplikacji .NET pulpitu systemu Windows (XAML) można uzyskać token dostępu i wywołania interfejsu API programu Graph firmy Microsoft lub innych interfejsów API, które wymagają tokenów dostępu z punktu końcowego w wersji 2 usługi Azure Active Directory.

Na końcu tego przewodnika aplikacja będzie mogła wywołać interfejs API chronionych przy użyciu konta osobiste (w tym outlook.com, live.com i inne) oraz pracy i służbowych z firmy lub organizacji, która ma usługę Azure Active Directory.  

> W tym przewodniku wymaga programu Visual Studio 2015 Update 3 lub Visual Studio 2017 r.  Nie masz? [Bezpłatnie pobrać Visual Studio 2017 r.](https://www.visualstudio.com/downloads/)

### <a name="how-this-guide-works"></a>Jak działa w tym przewodniku

![Jak działa w tym przewodniku](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.png)

Przykładowa aplikacja utworzone przez ten przewodnik umożliwia aplikacji pulpitu systemu Windows do zapytania interfejsu API programu Microsoft Graph i interfejsu API sieci Web, który akceptuje tokeny od punktu końcowego w wersji 2 usługi Azure Active Directory. W tym scenariuszu token zostanie dodany do żądań HTTP za pośrednictwem nagłówek autoryzacji. Token nabycia i odnawiania jest obsługiwany przez bibliotekę uwierzytelniania firmy Microsoft (MSAL).


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Obsługa tokenów nabycia do uzyskiwania dostępu do chronionego interfejsów API sieci Web

Po użytkownik jest uwierzytelniany, przykładowa aplikacja odbiera token, który może służyć do badania interfejsu API programu Microsoft Graph i interfejsu API sieci Web zabezpieczonych przez usługi Microsoft Azure Active Directory w wersji 2.

Interfejsy API, takich jak Microsoft Graph wymagają tokenu dostępu, aby umożliwić uzyskiwanie dostępu do określonych zasobów — na przykład do odczytu profilu użytkownika, kalendarz dostęp użytkownika lub Wyślij wiadomość e-mail. Aplikacja może żądać tokenu dostępu przy użyciu MSAL dostępu do tych zasobów, określając zakresy interfejsu API. Ten token dostępu jest dodawane do nagłówka HTTP autoryzacji dla każdego wywołania wykonane w stosunku do chronionego zasobu. 

MSAL zarządza buforowanie i odświeżanie tokenów dostępu, więc nie trzeba aplikacji.


### <a name="nuget-packages"></a>Pakiety NuGet

W tym przewodniku używa następujących pakietów NuGet:

|Biblioteka|Opis|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Biblioteka uwierzytelniania firmy Microsoft (MSAL)|

