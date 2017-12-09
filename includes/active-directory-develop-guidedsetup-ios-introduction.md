
# <a name="call-the-microsoft-graph-api-from-an-ios-application"></a>Wywołanie interfejsu API programu Microsoft Graph z aplikacji systemu iOS

Ten przewodnik przedstawia, jak aplikacji natywnej dla systemu iOS (Swift) mogą wywoływać interfejsy API, które wymagają tokenów dostępu z punktem końcowym v2.0 Microsoft Azure Active Directory (Azure AD). Przewodnika opisano sposób uzyskiwania tokenów dostępu i używać ich w wywołaniach interfejsu API programu Graph firmy Microsoft i innych interfejsów API.

Po ukończeniu ćwiczeń opisanych w tym przewodniku, aplikacja może wywoływać chronionego interfejsu API z firmy lub organizacji, która ma usługę Azure AD. Aplikacja wprowadzić chronionych wywołań interfejsu API przy użyciu kont osobistych, takich jak outlook.com, live.com i innych użytkowników, a także konta firmowego lub szkolnego.

## <a name="prerequisites"></a>Wymagania wstępne
- XCode w wersji 8.x jest wymagany dla przykładu, który jest tworzony w tym przewodniku. Możesz pobrać XCode z [witryny sieci Web programu iTunes](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "adresu URL pobierania w środowisku XCode").
- [Carthage](https://github.com/Carthage/Carthage) Menedżer zależności jest wymagany dla pakietu administracyjnego.

## <a name="how-this-guide-works"></a>Jak działa w tym przewodniku

![Jak działa w tym przewodniku](media/active-directory-develop-guidedsetup-ios-introduction/iosintro.png)

W tym przewodniku przykładowej aplikacji umożliwia aplikacji systemu iOS zapytania interfejsu API programu Microsoft Graph lub składnika web API, który akceptuje tokeny od punktu końcowego v2.0 usługi Azure AD. W tym scenariuszu token jest dodawany do żądań HTTP przy użyciu **autoryzacji** nagłówka. Token nabycia i odnawiania są obsługiwane przez biblioteki uwierzytelniania firmy Microsoft (MSAL).


### <a name="handle-token-acquisition-for-access-to-protected-web-apis"></a>Dojście nabycia tokenu dostępu do chronionego interfejsów API sieci web

Po użytkownik jest uwierzytelniany, przykładowa aplikacja odbiera token. Token służy do interfejsu API programu Microsoft Graph lub sieci web interfejsu API, który jest chroniony przez punktu końcowego v2.0 usługi Azure AD.

Interfejsy API, takich jak Microsoft Graph wymagają tokenu dostępu, aby zezwolić na dostęp do określonych zasobów. Tokeny są wymagane do odczytu profilu użytkownika, dostęp do kalendarza użytkownika, Wyślij wiadomość e-mail i tak dalej. Aplikacja może wysłać żądanie tokenu dostępu przy użyciu MSAL i określając zakresy interfejsu API. Token dostępu jest dodawany do HTTP **autoryzacji** nagłówek dla każdego wywołania utworzona przed chronionego zasobu.

MSAL zarządza buforowanie i odświeżanie tokenów dostępu, więc nie trzeba aplikacji.


## <a name="libraries"></a>Biblioteki

W tym przewodniku korzysta z biblioteki następujące:

|Biblioteka|Opis|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|Podgląd biblioteki uwierzytelniania firmy Microsoft dla systemu iOS|

