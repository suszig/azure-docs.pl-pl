---
title: "Usługa Azure Active Directory B2C: Dodawanie konta Microsoft (MSA) jako dostawca tożsamości za pomocą zasad niestandardowych"
description: "Przykładowe za pomocą programu Microsoft jako dostawca tożsamości za pomocą protokołu OpenID Connect (OIDC)"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: yoelh
ms.openlocfilehash: cdc77d093358fa15bb1acbc9ba6b1867bae062f8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-add-microsoft-account-msa-as-an-identity-provider-using-custom-policies"></a>Usługa Azure Active Directory B2C: Dodawanie konta Microsoft (MSA) jako dostawca tożsamości za pomocą zasad niestandardowych

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

W tym artykule przedstawiono sposób włączenia logowania użytkowników z konta Microsoft (MSA) za pośrednictwem [niestandardowych zasad](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Wymagania wstępne
Wykonaj kroki [wprowadzenie do zasad niestandardowych](active-directory-b2c-get-started-custom.md) artykułu.

Kroki te obejmują:

1.  Tworzenie aplikacji konta Microsoft.
2.  Dodawanie klucza aplikacji konta Microsoft do usługi Azure AD B2C
3.  Dodawanie dostawcy oświadczeń dla zasad
4.  Rejestrowanie dostawcy oświadczeń Account Microsoft do podróży użytkownika
5.  Przesyłanie zasad do usługi Azure AD B2C dzierżawy i przetestować go

## <a name="create-a-microsoft-account-application"></a>Tworzenie aplikacji konta Microsoft
Do używania konta Microsoft jako dostawca tożsamości w usłudze Azure Active Directory (Azure AD) B2C, należy utworzyć aplikację konta Microsoft i dostarczyć prawo parametrów. Musisz mieć konto Microsoft. Jeśli nie masz, odwiedź stronę [https://www.live.com/](https://www.live.com/).

1.  Przejdź do [portalu rejestracji aplikacji Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) i zaloguj się przy użyciu poświadczeń konta Microsoft.
2.  Kliknij przycisk **Dodaj aplikację**.

    ![Microsoft konta — Dodawanie aplikacji](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-new-app.png)

3.  Podaj **nazwa** dla aplikacji, **adres e-mail kontaktu**, usuń zaznaczenie pola wyboru **nas pomocy Rozpoczynanie pracy** i kliknij przycisk **Utwórz**.

    ![Konto Microsoft - rejestrowania aplikacji](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-name.png)

4.  Skopiuj wartość **identyfikator aplikacji**. Należy go skonfigurować konto Microsoft jako dostawca tożsamości w dzierżawie.

    ![Konto Microsoft - kopiowania wartości identyfikatora aplikacji](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-id.png)

5.  Polecenie **Dodaj platformy**

    ![Microsoft konta — Dodawanie platformy](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-platform.png)

6.  Na liście platform wybierz **Web**.

    ![Konto Microsoft — z listy platform wybierz sieci Web](media/active-directory-b2c-custom-setup-ms-account-idp/msa-web.png)

7.  Wprowadź `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` w **identyfikator URI przekierowania** pola. Zastąp **{dzierżawa}** nazwą Twojej dzierżawy (na przykład contosob2c.onmicrosoft.com).

    ![Konto Microsoft — zestaw adresów URL przekierowania.](media/active-directory-b2c-custom-setup-ms-account-idp/msa-redirect-url.png)

8.  Polecenie **wygenerować nowe hasło** w obszarze **klucze tajne aplikacji** sekcji. Skopiuj nowe hasło, które są wyświetlane na ekranie. Należy go skonfigurować konto Microsoft jako dostawca tożsamości w dzierżawie. To hasło jest ważne poświadczenie zabezpieczeń.

    ![Microsoft konta — wygenerować nowe hasło](media/active-directory-b2c-custom-setup-ms-account-idp/msa-generate-new-password.png)

    ![Konto Microsoft - kopiowania nowe hasło](media/active-directory-b2c-custom-setup-ms-account-idp/msa-new-password.png)

9.  Sprawdź pole **Obsługa zestaw Live SDK** w obszarze **zaawansowane opcje** sekcji. Kliknij pozycję **Zapisz**.

    ![Konto Microsoft — Obsługa zestaw Live SDK](media/active-directory-b2c-custom-setup-ms-account-idp/msa-live-sdk-support.png)

## <a name="add-the-microsoft-account-application-key-to-azure-ad-b2c"></a>Dodaj klucz aplikacji konta Microsoft do usługi Azure AD B2C
Federacja z kontami Microsoft wymaga klucz tajny klienta konta Microsoft w celu zaufania usługi Azure AD B2C w imieniu aplikacji. Musisz zapisać Twoje secert aplikacji konta Microsoft w dzierżawie usługi Azure AD B2C:   

1.  Przejdź do dzierżawy usługi Azure AD B2C i wybierz **ustawieniami B2C** > **Framework obsługi tożsamości**
2.  Wybierz **zasad kluczy** Aby wyświetlić dostępne w Twojej dzierżawie kluczy.
3.  Kliknij przycisk **+ Dodaj**.
4.  Aby uzyskać **opcje**, użyj **ręcznego**.
5.  Aby uzyskać **nazwa**, użyj `MSASecret`.  
    Prefiks `B2C_1A_` mogą być dodawane automatycznie.
6.  W **klucz tajny** wprowadź klucz tajny aplikacji firmy Microsoft z https://apps.dev.microsoft.com
7.  Aby uzyskać **użycie klucza**, użyj **podpisu**.
8.  Kliknij przycisk **Utwórz**
9.  Upewnij się, że utworzono klucz `B2C_1A_MSASecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Dodawanie dostawcy oświadczeń w zasadach rozszerzenia
Użytkownikom na logowanie się przy użyciu Account firmy Microsoft, należy zdefiniować Account Microsoft jako dostawcy oświadczeń. Innymi słowy należy określić punkt końcowy, który komunikuje się usługi Azure AD B2C. Punkt końcowy zawiera zestaw oświadczeń, które są używane przez usługę Azure AD B2C, aby sprawdzić, czy określony użytkownik jest uwierzytelniony.

Zdefiniuj Account Microsoft jako dostawcy oświadczeń, dodając `<ClaimsProvider>` węzeł rozszerzenia pliku zasad:

1.  Otwórz plik zasad rozszerzenia (TrustFrameworkExtensions.xml) z katalogu roboczego. Jeśli potrzebujesz edytora XML [spróbuj Visual Studio Code](https://code.visualstudio.com/download), lekkie edytora i platform.
2.  Znajdź `<ClaimsProviders>` sekcji
3.  Dodaj następujący fragment kodu XML w obszarze `ClaimsProviders` elementu:

    ```xml
<ClaimsProvider>
    <Domain>live.com</Domain>
    <DisplayName>Microsoft Account</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="MSA-OIDC">
        <DisplayName>Microsoft Account</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <Metadata>
        <Item Key="ProviderName">https://login.live.com</Item>
        <Item Key="METADATA">https://login.live.com/.well-known/openid-configuration</Item>
        <Item Key="response_types">code</Item>
        <Item Key="response_mode">form_post</Item>
        <Item Key="scope">openid profile email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Microsoft application client id</Item>
        </Metadata>
    <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
    </CryptographicKeys>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="email" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

4.  Zastąp `client_id` wartości identyfikatora klienta aplikacji Account firmy Microsoft

5.  Zapisz plik.

## <a name="register-the-microsoft-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Rejestr Account Microsoft oświadczenia dostawcy do logowania się lub logowanie przebieg użytkownika

W tym momencie Konfigurowanie dostawcy tożsamości, ale nie jest dostępna w żadnym ekrany konta-konta/logowania. Teraz należy dodać do użytkownika dostawcy tożsamości Account Microsoft `SignUpOrSignIn` podróży użytkownika. Aby było to możliwe, utworzymy duplikatem istniejącej przebieg użytkownika szablonu.  Następnie dodaj dostawcy tożsamości Account Microsoft:

> [!NOTE]
>
>Jeśli został wcześniej skopiowany `<UserJourneys>` elementu z pliku podstawowego zasad do pliku rozszerzenie `TrustFrameworkExtensions.xml`, możesz przejść do tej sekcji.

1.  Otwórz plik bazowy tej zasady (na przykład TrustFrameworkBase.xml).
2.  Znajdź `<UserJourneys>` element i skopiuj cała zawartość `<UserJourneys>` węzła.
3.  Otwórz plik rozszerzenia (na przykład TrustFrameworkExtensions.xml) i Znajdź `<UserJourneys>` elementu. Jeśli element nie istnieje, dodaj je.
4.  Wklej całą zawartość `<UserJournesy>` węzła, który został skopiowany jako element podrzędny `<UserJourneys>` elementu.

### <a name="display-the-button"></a>Wyświetlany przycisk
`<ClaimsProviderSelections>` Element definiuje listę opcje wyboru dostawcy oświadczeń i ich kolejność.  `<ClaimsProviderSelection>`element jest odpowiednikiem przycisku dostawcy tożsamości na stronie konta-konta/logowania. Jeśli dodasz `<ClaimsProviderSelection>` elementu dla konta Microsoft, nowy przycisk zostaną wyświetlone po wyładowuje użytkownika na stronie. Aby dodać ten element:

1.  Znajdź `<UserJourney>` węzła, który zawiera `Id="SignUpOrSignIn"` w podróży użytkownika, które zostały skopiowane.
2.  Zlokalizuj `<OrchestrationStep>` węzła, który zawiera`Order="1"`
3.  Dodaj następujący fragment kodu XML w obszarze `<ClaimsProviderSelections>` węzła:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a>Połącz przycisku akcji
Teraz, gdy masz przycisku w miejscu, należy połączyć je z akcją. Akcja, w tym przypadku jest dla usługi Azure AD B2C do komunikowania się z Microsoft Account otrzymujących token. Połączyć przycisku akcji przez łączenie techniczne profilu dla dostawcy oświadczeń Account Microsoft:

1.  Znajdź `<OrchestrationStep>` zawierającą `Order="2"` w `<UserJourney>` węzła.
2.  Dodaj następujący fragment kodu XML w obszarze `<ClaimsExchanges>` węzła:

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

> [!NOTE]
>
>   * Upewnij się, `Id` ma taką samą wartość jak `TargetClaimsExchangeId` w poprzedniej sekcji
>   * Upewnij się, `TechnicalProfileReferenceId` ustawiony identyfikator techniczne profilu utworzonego wcześniej (MSA-OIDC).

## <a name="upload-the-policy-to-your-tenant"></a>Przekaż zasady dla Twojej dzierżawy
1.  W [portalu Azure](https://portal.azure.com), przejdź do [kontekstu dzierżawy usługi Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md), a następnie otwórz **usługi Azure AD B2C** bloku.
2.  Wybierz **Framework obsługi tożsamości**.
3.  Otwórz **wszystkie zasady** bloku.
4.  Wybierz **przekazywać zasady**.
5.  Sprawdź **zastąpić zasady, jeśli istnieje** pole.
6.  **Przekaż** TrustFrameworkExtensions.xml i upewnij się, że nie wystąpi niepowodzenie weryfikacji

## <a name="test-the-custom-policy-by-using-run-now"></a>Testowanie zasad niestandardowych przy użyciu Uruchom teraz

1.  Otwórz **ustawienia usługi Azure AD B2C** i przejdź do **Framework obsługi tożsamości**.
> [!NOTE]
>
>**Uruchom teraz** wymaga co najmniej jednej aplikacji można preregistered dla dzierżawcy. Aby dowiedzieć się, jak zarejestrować aplikacji, zapoznaj się z usługi Azure AD B2C [wprowadzenie](active-directory-b2c-get-started.md) artykułu lub [Rejestracja aplikacji](active-directory-b2c-app-registration.md) artykułu.
2.  Otwórz **B2C_1A_signup_signin**, jednostki uzależnionej zasady niestandardowe strony (RP), który został przekazany. Wybierz **Uruchom teraz**.
3.  Można będzie zalogować się przy użyciu konta Microsoft.

## <a name="optional-register-the-microsoft-account-claims-provider-to-profile-edit-user-journey"></a>[Opcjonalnie] Zarejestruj dostawcę oświadczeń Account Microsoft do edycji profilu użytkownika podróży
Można również dodać dostawcy tożsamości Account Microsoft do użytkownika `ProfileEdit` podróży użytkownika. Aby było to możliwe, możemy Powtórz ostatnie dwa kroki:

### <a name="display-the-button"></a>Wyświetlany przycisk
1.  Otwórz plik rozszerzenia zasad (na przykład TrustFrameworkExtensions.xml).
2.  Znajdź `<UserJourney>` węzła, który zawiera `Id="ProfileEdit"` w podróży użytkownika, które zostały skopiowane.
3.  Zlokalizuj `<OrchestrationStep>` węzła, który zawiera`Order="1"`
4.  Dodaj następujący fragment kodu XML w obszarze `<ClaimsProviderSelections>` węzła:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a>Połącz przycisku akcji
1.  Znajdź `<OrchestrationStep>` zawierającą `Order="2"` w `<UserJourney>` węzła.
2.  Dodaj następujący fragment kodu XML w obszarze `<ClaimsExchanges>` węzła:

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Testowanie zasad niestandardowych edycji profilu przy użyciu Uruchom teraz
1.  Otwórz **ustawienia usługi Azure AD B2C** i przejdź do **Framework obsługi tożsamości**.
2.  Otwórz **B2C_1A_ProfileEdit**, jednostki uzależnionej zasady niestandardowe strony (RP), który został przekazany. Wybierz **Uruchom teraz**.
3.  Można będzie zalogować się przy użyciu konta Microsoft.

## <a name="download-the-complete-policy-files"></a>Pobierz pliki pełną zasad
Opcjonalnie: Zaleca się tworzenie scenariusz przy użyciu własnych zasad niestandardowych, które przeprowadzenie pliki po zakończeniu wprowadzenie do zasad niestandardowych, zamiast korzystać z tych przykładowych plików.  [Przykładowe pliki zasad dla odwołania](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-msa-app)
