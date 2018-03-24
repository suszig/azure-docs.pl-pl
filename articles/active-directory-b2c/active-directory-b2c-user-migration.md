---
title: 'Usługa Azure Active Directory B2C: Podejścia migracji użytkownika'
description: Omówiono w nim podstawowe i zaawansowane pojęcia dotyczące migracji użytkownika przy użyciu interfejsu API programu Graph i opcjonalnie za pomocą niestandardowych zasad usługi Azure AD B2C.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 10/04/2017
ms.author: davidmu
ms.openlocfilehash: be80ea534be6de4fad2b072cf531669f45eda527
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-user-migration"></a>Usługi Azure Active Directory B2C: Migracja użytkowników
Podczas migracji dostawcy tożsamości do usługi Azure Active Directory B2C (Azure AD B2C), może być również konieczne migracji konta użytkownika. W tym artykule opisano sposób migracji istniejących kont użytkowników z dowolnego dostawcy tożsamości do usługi Azure AD B2C. Artykuł nie ma mieć charakter opisowy, ale raczej opisano dwa różne podejścia. Deweloper jest odpowiedzialny za przydatności każdego z podejść.

## <a name="user-migration-flows"></a>Przepływy migracji użytkownika
Z usługi Azure AD B2C można migrować użytkowników za pomocą [interfejsu API programu Graph](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet). Proces migracji użytkownik należy do dwóch przepływów:

* **Przed migracją**: ten przepływ ma zastosowanie, gdy masz albo wyczyść dostęp do poświadczeń użytkownika (nazwy użytkownika i hasła) lub poświadczenia są szyfrowane, ale można je odszyfrować. Proces migracji wstępnej obejmuje odczytywanie użytkowników ze starego dostawcy tożsamości i tworzenia nowych kont w katalogu usługi Azure AD B2C.

* **Przed migracją i hasło resetowania**: ten przepływ ma zastosowanie, gdy hasło użytkownika nie jest dostępny. Na przykład:
    * Hasło jest przechowywane w formacie wyznaczania wartości skrótu.
    * Hasło jest zapisywane w dostawcy tożsamości, których nie można uzyskać dostęp. Stary dostawcy tożsamości weryfikuje poświadczenia użytkownika poprzez wywołanie usługi sieci web.

W obu przepływów należy najpierw uruchom proces migracji wstępnej, odczytać użytkowników ze starego dostawcy tożsamości i tworzenia nowych kont w katalogu usługi Azure AD B2C. Jeśli nie masz hasła, tworzenia konta przy użyciu hasła, który jest generowany losowo. Następnie poprosić użytkownika o zmianę hasła lub gdy użytkownik loguje się po raz pierwszy, usługi Azure AD B2C z monitem o zresetowanie go pracownika.

## <a name="password-policy"></a>Zasady haseł
Zasady haseł usługi Azure AD B2C (dla kont lokalnych) jest oparta na zasadach usługi Azure AD. Usługi Azure AD B2C rejestracji i logowania i hasło Resetuj zasady, użyj siły "silnego" hasła, a nie wygasa hasła. Aby uzyskać więcej informacji, zobacz [zasady haseł usługi Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx).

Jeśli konta, które chcesz migrować używać słabszych siły hasła niż [siły silne hasło, wymuszane przez usługę Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), można wyłączyć wymaganie silne hasło. Aby zmienić domyślne zasady haseł, ustaw `passwordPolicies` właściwości `DisableStrongPassword`. Na przykład można zmodyfikować żądanie utworzenia użytkownika w następujący sposób: 

```JSON
"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"
```

## <a name="step-1-use-graph-api-to-migrate-users"></a>Krok 1: Użyj interfejsu API programu Graph do migrowania użytkowników
Tworzenia konta użytkownika usługi Azure AD B2C za pośrednictwem interfejsu API programu Graph (przy użyciu hasła lub za pomocą losowe hasło). W tej sekcji opisano proces tworzenia kont użytkowników w katalogu usługi Azure AD B2C przy użyciu interfejsu API programu Graph.

### <a name="step-11-register-your-application-in-your-tenant"></a>Krok 1.1: Zarejestrować aplikację w dzierżawie
Do komunikacji z interfejsu API programu Graph, należy najpierw musi mieć konto usługi z uprawnieniami administracyjnymi. W usłudze Azure AD należy zarejestrować aplikację i uwierzytelniania do usługi Azure AD. Poświadczenia aplikacji są **identyfikator aplikacji** i **klucz tajny aplikacji**. Aplikacja działa jako sam nie jako użytkownik, do wywołania interfejsu API programu Graph.

Najpierw zarejestrować aplikację migracji w usłudze Azure AD. Utwórz klucz aplikacji (klucz tajny aplikacji) i ustaw aplikacji z uprawnieniami do zapisu.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).

2. Wybierz usługę Azure AD **B2C** dzierżawy, wybierając konto u góry po prawej części okna.

3. W okienku po lewej stronie wybierz **usługi Azure Active Directory** (nie usługi Azure AD B2C). Aby ją znaleźć, konieczne może być wybierz **więcej usług**.

4. Wybierz **rejestracji aplikacji**.

5. Wybierz **nowej rejestracji aplikacji**.

    ![Rejestrowanie nowej aplikacji](media/active-directory-b2c-user-migration/pre-migration-app-registration.png)

6. Tworzenie nowej aplikacji, wykonując następujące czynności:
    * Dla **nazwa**, użyj **B2CUserMigration** lub inna nazwa ma.
    * Aby uzyskać **typu aplikacji**, użyj **aplikacji/interfejs API sieci Web**.
    * Aby uzyskać **adres URL logowania**, użyj **https://localhost** (ponieważ nie jest to odpowiednie dla tej aplikacji).
    * Wybierz pozycję **Utwórz**.

7. Po utworzeniu aplikacji w **aplikacji** wybierz nowo utworzony **B2CUserMigration** aplikacji.

8. Wybierz **właściwości**, kopiowania **identyfikator aplikacji**i zapisz je.

### <a name="step-12-create-the-application-secret"></a>Krok 1.2: Utwórz klucz tajny aplikacji
1. W portalu Azure **zarejestrowana aplikacja** wybierz **klucze**.

2. Dodaj nowy klucz (znanej także jako klucz tajny klienta), a następnie skopiuj klucz do późniejszego użycia.

    ![Identyfikator aplikacji i kluczy](media/active-directory-b2c-user-migration/pre-migration-app-id-and-key.png)

### <a name="step-13-grant-administrative-permission-to-your-application"></a>Krok 1.3: Udziel uprawnień administracyjnych do aplikacji
1. W portalu Azure **zarejestrowana aplikacja** wybierz **wymagane uprawnienia**.

2. Wybierz **systemu Windows Azure Active Directory**.

3. W **Włącz dostęp** okienku w obszarze **uprawnienia aplikacji**, wybierz pozycję **Odczyt i zapis danych katalogu**, a następnie wybierz **zapisać**.

4. W **wymagane uprawnienia** okienku wybierz **udzielanie uprawnień**.

    ![Uprawnienia aplikacji](media/active-directory-b2c-user-migration/pre-migration-app-registration-permissions.png)

Teraz możesz korzystać z aplikacji z uprawnieniami do tworzenia, odczytu i aktualizacji użytkowników z dzierżawy usługi Azure AD B2C.

### <a name="step-14-optional-environment-cleanup"></a>Krok 1.4: Czyszczenie środowiska (opcjonalnie)
Odczyt i zapis w katalogu danych uprawnienia *nie* mają prawo do usuwania użytkowników. Aby udzielić aplikacji możliwość usuwania użytkowników (Aby wyczyścić środowisko), należy wykonać dodatkowe czynności, które obejmuje programu PowerShell, aby ustawić uprawnienia administratora konta użytkownika. W przeciwnym razie możesz przejść do następnej sekcji.

> [!IMPORTANT]
> Należy użyć konta administratora dzierżawy B2C, która jest *lokalnego* do dzierżawy B2C. Składnia nazwy konta jest *admin@contosob2c.onmicrosoft.com*.

>[!NOTE]
> Poniższy skrypt programu PowerShell wymaga [Azure Active Directory programu PowerShell w wersji 2](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0).

Ten skrypt programu PowerShell wykonaj następujące czynności:
1. Łączenie z usługą online. Aby to zrobić, uruchom `Connect-AzureAD` polecenia cmdlet w programie Windows PowerShell, wiersz polecenia i wprowadź swoje poświadczenia. 

2. Użyj **identyfikator aplikacji** można przypisać rolę administratora konta użytkownika aplikacji. Te role mają dobrze znane identyfikatory, dlatego jest wszystko co należy zrobić, wprowadź użytkownika **identyfikator aplikacji** w skrypcie.

```PowerShell
Connect-AzureAD

$AppId = "<Your application ID>"

# Fetch Azure AD application to assign to role
$roleMember = Get-AzureADServicePrincipal -Filter "AppId eq '$AppId'"

# Fetch User Account Administrator role instance
$role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}

# If role instance does not exist, instantiate it based on the role template
if ($role -eq $null) {
    # Instantiate an instance of the role template
    $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq 'User Account Administrator'}
    Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId

    # Fetch User Account Administrator role instance again
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}
}

# Add application to role
Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId

# Fetch role membership for role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
```

Zmień `$AppId` wartości z usługi Azure AD **identyfikator aplikacji**.

## <a name="step-2-pre-migration-application-sample"></a>Krok 2: Przed migracją aplikacji przykładowych
[Pobierz i uruchom przykładowy kod](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration). Można go pobrać jako plik .zip.

### <a name="step-21-edit-the-migration-data-file"></a>Krok 2.1: Edytuj plik danych migracji
Przykładowa aplikacja korzysta z pliku JSON, który zawiera dane fikcyjny użytkownika. Po pomyślnym uruchomieniu próbki można zmienić kod służący do konsumowania danych z bazy danych. Lub wyeksportować profil użytkownika do pliku JSON, a następnie ustaw aplikację do używania tego pliku.

Aby edytować plik JSON, otwórz `AADB2C.UserMigration.sln` rozwiązania Visual Studio. W `AADB2C.UserMigration` otwarciu projektu `UsersData.json` pliku.

![Plik danych użytkownika](media/active-directory-b2c-user-migration/pre-migration-data-file.png)

Jak widać, plik zawiera listę jednostkami użytkownika. Każdy obiekt użytkownika ma następujące właściwości:
* e-mail
* Nazwa wyświetlana
* Imię
* lastName
* hasło (może być pusta)

> [!NOTE]
> W czasie kompilacji programu Visual Studio kopiuje plik do `bin` katalogu.

### <a name="step-22-configure-the-application-settings"></a>Krok 2.2: Konfigurowanie ustawień aplikacji
W obszarze `AADB2C.UserMigration` otwarciu projektu *App.config* pliku. Zastąp własne wartości następujących ustawień aplikacji:

```XML
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Key from above}" />
    <add key="MigrationFile" value="{Name of a JSON file containing the users' data; for example, UsersData.json}" />
    <add key="BlobStorageConnectionString" value="{Your connection Azure table string}" />
    
</appSettings>
```

> [!NOTE]
> * Użyj parametrów połączenia tabeli platformy Azure jest opisane w kolejnych sekcjach.
> * Nazwa dzierżawy B2C jest domeną wprowadzona podczas tworzenia dzierżawy, która jest wyświetlana w portalu Azure. Nazwa dzierżawy zazwyczaj kończy się wraz z sufiksem *. onmicrosoft.com* (na przykład *contosob2c.onmicrosoft.com*).
>

### <a name="step-23-run-the-pre-migration-process"></a>Krok 2.3: Uruchom proces migracji wstępnej
Kliknij prawym przyciskiem myszy `AADB2C.UserMigration` rozwiązania, a następnie Odbuduj próbki. W przypadku pomyślnego, po wykonaniu `UserMigration.exe` plik wykonywalny znajduje się w `AADB2C.UserMigration\bin\Debug`. Aby uruchomić proces migracji, użyj jednej z następujących parametrów wiersza polecenia:

* Aby **migracji użytkowników mających hasła**, użyj `UserMigration.exe 1` polecenia.

* Aby **migracji użytkowników z losowe hasło**, użyj `UserMigration.exe 2` polecenia. Ta operacja tworzy również jednostkę tabeli platformy Azure. Następnie należy skonfigurować zasady do wywołania interfejsu API REST usługi. Usługa używa tabeli platformy Azure do śledzenia i zarządzania procesem migracji.

![Wersja demonstracyjna dla procesu migracji](media/active-directory-b2c-user-migration/pre-migration-demo.png)

### <a name="step-24-check-the-pre-migration-process"></a>Krok 2.4: Sprawdź proces migracji wstępnej
Aby sprawdzić poprawność migracji, użyj jednej z następujących dwóch metod:

* Aby wyszukać użytkownika według nazwy wyświetlanej, użyj portalu Azure:
    
    a. Otwórz **usługi Azure AD B2C**, a następnie wybierz **użytkowników i grup**.

    b. W polu wyszukiwania wpisz nazwę wyświetlaną użytkownika, a następnie Wyświetl profilu użytkownika. 

* Aby pobrać użytkownika według adresu e-mail logowania, należy użyć tej aplikacji przykładowej:

    a. Uruchom następujące polecenie:

    ```Console
        UserMigration.exe 3 {email address}
    ```
        
    > [!TIP]
    > Można także zapisać dane wyjściowe do pliku JSON, za pomocą następującego polecenia:
    >
    >```Console
    >    UserMigration.exe 3 {email address} >> UserProfile.json
    >```

    > [!TIP]
    > Użytkownik może również pobierać według nazwy wyświetlanej za pomocą następującego polecenia: `UserMigration.exe 4 <Display name>`.

    b. Otwórz *UserProfile.json* plik w edytorze JSON. Z kodem Visual Studio, można sformatować dokument JSON, wybierając Shift + Alt + F lub wybierając **dokumentu w formacie** w menu kontekstowym.

    ![Plik UserProfile.json](media/active-directory-b2c-user-migration/pre-migration-get-by-email2.png)

### <a name="step-25-optional-environment-cleanup"></a>Krok 2.5: Oczyszczanie środowiska (opcjonalnie)
Jeśli chcesz wyczyścić Konfigurowanie dzierżawy usługi Azure AD i usuwać użytkowników z katalogu usługi Azure AD, uruchom `UserMigration.exe 5` polecenia.

> [!NOTE]
> * Aby wyczyścić dzierżawy, należy skonfigurować uprawnienia administratora konta użytkowników dla aplikacji.
> * Przykładowa aplikacja migracji czyści wszyscy użytkownicy, którzy są wymienione w pliku JSON.

### <a name="step-26-sign-in-with-migrated-users-with-password"></a>Krok 2.6: Zaloguj się przy użyciu zmigrowanych użytkownikom (hasła).
Po uruchomieniu proces migracji wstępnej z haseł użytkowników kont są gotowe do użycia, a użytkownicy mogą Zaloguj się do aplikacji przy użyciu usługi Azure AD B2C. Jeśli nie masz dostęp do hasła użytkownika, przejdź do następnej sekcji.

## <a name="step-3-help-users-reset-their-password"></a>Krok 3: Pomaganie użytkownikom resetowania hasła
W przypadku migrowania użytkowników z losowe hasło, należy resetowania hasła. Aby ułatwić im zresetować hasło, wysyłania powitalnej wiadomości e-mail z łączem do resetowania hasła.

Aby uzyskać link do zasad resetowania hasła, wykonaj następujące czynności:

1. Wybierz **ustawienia usługi Azure AD B2C**, a następnie wybierz **resetowania hasła** właściwości zasad.

2. Wybierz aplikację.

    >[!NOTE]
    >Uruchom teraz wymaga co najmniej jednej aplikacji można preregistered dla dzierżawcy. Aby dowiedzieć się, jak zarejestrować aplikacji, zapoznaj się z usługi Azure AD B2C [wprowadzenie](active-directory-b2c-get-started.md) artykułu lub [Rejestracja aplikacji](active-directory-b2c-app-registration.md) artykułu. 

3. Wybierz **Uruchom teraz**, a następnie sprawdź zasady.

4. W **Uruchom teraz punktu końcowego** polu, skopiuj adres URL, a następnie wyślij go do użytkowników.

    ![Dzienniki diagnostyczne zestawu](media/active-directory-b2c-user-migration/pre-migration-policy-uri.png)

## <a name="step-4-optional-change-your-policy-to-check-and-set-the-user-migration-status"></a>Krok 4: (Opcjonalnie) zmiany zasad można sprawdzić i ustawić migracji stanu użytkownika

> [!NOTE]
> Aby sprawdzić i zmienić migracji stanu użytkownika, należy użyć zasad niestandardowych. Aby uzyskać więcej informacji, zobacz [wprowadzenie do zasad niestandardowych](active-directory-b2c-get-started-custom.md).
>

Gdy użytkownicy próbują zalogować bez najpierw resetowania hasła, zgodnie z zasadami powinien zwrócić przyjazne komunikaty o błędach. Na przykład: 
>*Twoje hasło wygasło. Zresetowanie go pracownika, wybierz łącza resetowania hasła.* 

Ten opcjonalny krok wymagane jest użycie zasad niestandardowych usługi Azure AD B2C, zgodnie z opisem w [wprowadzenie do zasad niestandardowych](active-directory-b2c-get-started-custom.md) artykułu.

W tej sekcji możesz zmienić zasady do sprawdzania stanu migracji użytkowników na logowania. Jeśli użytkownik nie zmienił hasło, zwraca komunikat o błędzie HTTP 409, które pyta użytkownika, aby wybrać **nie pamiętasz hasła?** łącza. 

Aby śledzić zmiany hasła, należy użyć tabeli platformy Azure. Po uruchomieniu proces migracji wstępnej przy użyciu parametru wiersza polecenia `2`, Utwórz jednostkę użytkownika w tabeli platformy Azure. Usługa wykonuje następujące czynności:

* Na logowania zasad usługi Azure AD B2C wywołuje migracji do usługi RESTful, wysyłanie wiadomości e-mail jako oświadczenie wejściowe. Usługa Wyszukiwanie adresu e-mail w tabeli platformy Azure. Jeśli adres istnieje, Usługa generuje komunikat o błędzie: *musi zmienić hasło*.

* Po pomyślnie zmiany hasła przez użytkownika, Usuń z tabeli platformy Azure.

>[!NOTE]
>Aby uprościć próbki korzystamy tabeli platformy Azure. Stan migracji można przechowywać w dowolnej bazy danych lub jako właściwości niestandardowych w ramach konta usługi Azure AD B2C.

### <a name="41-update-your-application-setting"></a>4.1: aktualizowanie ustawień aplikacji
1. Aby przetestować pokaz interfejsu API RESTful, otwórz `AADB2C.UserMigration.sln` rozwiązania Visual Studio w programie Visual Studio. 

2. W `AADB2C.UserMigration.API` otwarciu projektu *App.config* pliku. Zastąp własną wartość ustawienia aplikacji:

    ```XML
    <appSettings>
        <add key="BlobStorageConnectionString" value="{The Azure Blob storage connection string"} />
    </appSettings>
    ```

### <a name="step-42-deploy-your-web-application-to-azure-app-service"></a>Krok 4.2: Wdrażanie aplikacji sieci web w usłudze Azure App Service
Opublikuj usługi interfejsu API w usłudze Azure App Service. Aby uzyskać więcej informacji, zobacz [wdrażanie aplikacji w usłudze Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy).

### <a name="step-43-add-a-technical-profile-and-technical-profile-validation-to-your-policy"></a>Krok 4.3: Dodawanie techniczne profilu i sprawdzania poprawności profilu techniczne do zasad 
1. W katalogu roboczym, otwórz *TrustFrameworkExtensions.xml* rozszerzenia pliku zasad. 

2. Wyszukaj `<ClaimsProviders>` węzeł, a następnie, w węźle, Dodaj następujący fragment kodu XML. Należy zmienić wartość `ServiceUrl` aby wskazywał adres URL punktu końcowego. 

    ```XML
    <ClaimsProvider>
        <DisplayName>REST APIs</DisplayName>
        <TechnicalProfiles>
    
        <TechnicalProfile Id="LocalAccountSignIn">
            <DisplayName>Local account just in time migration</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
            <Item Key="ServiceUrl">http://{your-app}.azurewebsites.net/api/PrePasswordReset/LoalAccountSignIn</Item>
            <Item Key="AuthenticationType">None</Item>
            <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
            <InputClaim ClaimTypeReferenceId="signInName" PartnerClaimType="email" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
    
        <TechnicalProfile Id="LocalAccountPasswordReset">
            <DisplayName>Local account just in time migration</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
            <Item Key="ServiceUrl">http://{your-app}.azurewebsites.net/api/PrePasswordReset/PasswordUpdated</Item>
            <Item Key="AuthenticationType">None</Item>
            <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
            <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        </TechnicalProfiles>
    </ClaimsProvider>
    
    <ClaimsProvider>
        <DisplayName>Local Account</DisplayName>
        <TechnicalProfiles>
    
        <!-- This technical profile uses a validation technical profile to authenticate the user. -->
        <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
            <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="LocalAccountSignIn" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    
        <TechnicalProfile Id="LocalAccountWritePasswordUsingObjectId">
            <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="LocalAccountPasswordReset" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    
        </TechnicalProfiles>
    </ClaimsProvider>
    ```

Poprzedniego techniczne profil definiuje jedno oświadczenie wejściowe: `signInName` (wysłać go pocztą e-mail). Na logowanie oświadczenia są wysyłane do punktu końcowego RESTful.

Po zdefiniowaniu techniczne profilu dla interfejsu API RESTful, poinformuj zasady usługi Azure AD B2C, aby wywołać techniczne profilu. Fragment kodu XML zastępuje `SelfAsserted-LocalAccountSignin-Email`, która jest zdefiniowana w podstawowej zasad. Fragment kodu XML również dodaje `ValidationTechnicalProfile`, z Identyfikator odwołania wskazujący technical profilu `LocalAccountUserMigration`. 

### <a name="step-44-upload-the-policy-to-your-tenant"></a>Krok 4.4: Przekaż zasady dla Twojej dzierżawy
1. W [portalu Azure](https://portal.azure.com), przełącz się do [kontekstu dzierżawy usługi Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md), a następnie wybierz **usługi Azure AD B2C**.

2. Wybierz **Framework obsługi tożsamości**.

3. Wybierz **wszystkich zasad**.

4. Wybierz **przekazywać zasady**.

5. Wybierz **zastąpić zasady, jeśli istnieje** pole wyboru.

6. Przekaż *TrustFrameworkExtensions.xml* pliku i upewnij się, że przekazaniem sprawdzania poprawności.

### <a name="step-45-test-the-custom-policy-by-using-run-now"></a>Krok 4.5: Testowanie zasad niestandardowych za pomocą Uruchom teraz
1. Wybierz **ustawienia usługi Azure AD B2C**, a następnie przejdź do **Framework obsługi tożsamości**.

2. Otwórz **B2C_1A_signup_signin**, jednostki uzależnionej strony (RP) zasad niestandardowych, które można przekazać, a następnie wybierz **Uruchom teraz**.

3. Spróbuj zalogować się przy użyciu jednego z migrowanych użytkowników poświadczenia, a następnie wybierz **logowania**. Interfejs API REST powinien zgłosić następujący komunikat o błędzie:

    ![Dzienniki diagnostyczne zestawu](media/active-directory-b2c-user-migration/pre-migration-error-message.png)

### <a name="step-46-optional-troubleshoot-your-rest-api"></a>Krok 4.6. (Opcjonalnie) Rozwiązywanie problemów z interfejsem API REST
Można wyświetlać i monitorować rejestrowanie informacji w czasie niemal rzeczywistym.

1. W menu Ustawienia aplikacji RESTful w obszarze **monitorowanie**, wybierz pozycję **dzienniki diagnostyczne**. 

2. Ustaw **rejestrowanie aplikacji (systemu plików)** do **na**. 

3. Ustaw **poziom** do **pełne**.

4. Wybierz **Zapisz**

    ![Dzienniki diagnostyczne zestawu](media/active-directory-b2c-user-migration/pre-migration-diagnostic-logs.png)

5. Na **ustawienia** menu, wybierz opcję **strumienia dziennika**.

6. Sprawdź dane wyjściowe interfejsu API RESTful.

Aby uzyskać więcej informacji, zobacz [przesyłania strumieniowego dzienników i konsoli](https://docs.microsoft.com/azure/app-service-web/web-sites-streaming-logs-and-console).

> [!IMPORTANT]
> Użyj dzienników diagnostycznych tylko w czasie projektowania i testowania. Interfejs API RESTful danych wyjściowych może zawierać informacje poufne, które nie powinny zostać ujawnione w środowisku produkcyjnym.
>

## <a name="optional-download-the-complete-policy-files"></a>(Opcjonalnie) Pobierz pliki pełną zasad
Po ukończeniu [wprowadzenie do zasad niestandardowych](active-directory-b2c-get-started-custom.md) wskazówki, zaleca się tworzenia scenariusz przy użyciu plików zasady niestandardowe. Użytkownikowi, firma Microsoft umieściła [przykładowe pliki zasad](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration). 
