
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Wywołania programu Microsoft Graph API z poziomu aplikacji JavaScript pojedynczej strony (SPA)

W tym przewodniku pokazano, jak JavaScript jednej strony aplikacji JEDNOSTRONICOWEJ mógł zalogować się w osobistym, pracy i kont służbowych uzyskać token dostępu i wywołania interfejsu API programu Graph firmy Microsoft lub innych interfejsów API, które wymagają tokenów dostępu z punktu końcowego usługi Azure Active Directory w wersji 2.

### <a name="how-this-guide-works"></a>Jak działa w tym przewodniku

![Jak działa przykładowej aplikacji wygenerowane przez ten przewodnik](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a>Więcej informacji

Przykładowa aplikacja utworzone przez ten przewodnik umożliwia SPA JavaScript wysyłać kwerendy interfejsu API programu Microsoft Graph i interfejsu API sieci Web, który akceptuje tokeny od punktu końcowego w wersji 2 usługi Azure Active Directory. W tym scenariuszu po użytkownika logowania token dostępu jest wymagane i dodać do żądań HTTP za pośrednictwem nagłówek autoryzacji. Token nabycia i odnawiania są obsługiwane przez biblioteki uwierzytelniania firmy Microsoft (MSAL).

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Biblioteki

W tym przewodniku korzysta z biblioteki następujące:

|Biblioteka|Opis|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Biblioteka uwierzytelniania firmy Microsoft dla podglądu JavaScript|

> [!NOTE]
> *msal.js* celów *punktu końcowego w wersji 2 usługi Azure Active Directory* — co pozwala kont prywatnych, szkolnego i zalogować się i uzyskać tokeny. *Punktu końcowego w wersji 2 usługi Azure Active Directory* ma [pewne ograniczenia](..\articles\active-directory\develop\active-directory-v2-limitations.md). Jeśli interesuje Cię tylko na kontach służbowych, jak i szkoły, użyj *adal.js* i *punktu końcowego V1*. Aby poznać różnice między punktami końcowymi v1 i v2 odczytu [porównania v1 v2](..\articles\active-directory\develop\active-directory-v2-compare.md).

<!--end-collapse-->
