---
title: Sposób śledzenia zachowania użytkownika za pomocą zdarzeń w usłudze Application Insights z usługi Azure AD B2C | Dokumentacja firmy Microsoft
description: Przewodnik krok po kroku, aby włączyć dzienniki zdarzeń w usłudze Application Insights z usługi Azure AD B2C podróże użytkownika za pomocą niestandardowych zasad - PREVIEW
services: active-directory-b2c
documentationcenter: dev-center-name
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.topic: article
ms.workload: identity
ms.date: 3/15/2018
ms.author: davidmu
ms.openlocfilehash: 1e8c4a53266072db71952ee35b15e5de54fabb95
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="tracking-user-behavior-inside-azure-ad-b2c-journeys-using-application-insights"></a>Śledzenie zachowania użytkowników w usłudze Azure AD B2C podróże przy użyciu usługi Application Insights

Usługa Azure Active Directory B2C dobrze działa z usługi Azure Application Insights.  Zapewniają szczegółowe i dostosowane dzienniki zdarzeń z podróży niestandardowego utworzonego użytkownika.  W tym przewodniku pokazano, jak rozpocząć pracę, możesz: 
* Uzyskaj informacje na zachowanie użytkownika
* Rozwiązywanie problemów dotyczących własnych zasad do rozwoju lub w środowisku produkcyjnym
* miara wydajności
* Utwórz powiadomienia z usługi Application Insights

## <a name="how-it-works"></a>Jak to działa
Dodano nowego dostawcę dla struktury obsługi tożsamości usługi Azure AD B2C: `Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0`.  Wysyła dane zdarzenia bezpośrednio do usługi Application Insights przy użyciu klucza Instrumentacji dostarczone do usługi Azure AD B2C.  Techniczne profilu używa tego dostawcy, aby zdefiniować zdarzenie z B2C.  Profil określa nazwę zdarzenia, oświadczenia, które mają być rejestrowane i klucz instrumentacji.  Aby przesłać zdarzenia, techniczne profilu jest dodawane jako am `orchestration step` lub jako `validation technical profile` w podróży użytkownika niestandardowego.  Zdarzenia można unified przez usługę Application Insights, aby zarejestrować sesję użytkownika za pomocą Identyfikatora korelacji.  Usługi Application Insights ułatwia zdarzeń i sesji w ciągu kilku sekund i przedstawia wiele wizualizacji, eksportowania i narzędzi analitycznych.



## <a name="prerequisites"></a>Wymagania wstępne
Wykonaj kroki [wprowadzenie do zasad niestandardowych](active-directory-b2c-get-started-custom.md).  W tym przewodniku przyjęto założenie, jest używany pakiet początkowy zasady niestandardowe.  Jednak nie jest wymagane.



## <a name="step-1-create-an-application-insights-resource-and-get-the-instrumentation-key"></a>Krok 1. Tworzenie zasobu usługi Application Insights i Uzyskaj klucz Instrumentacji

Usługa Application Insights jest zaawansowanym narzędziem. Podczas korzystania z usługi Azure AD B2C, jedynym wymaganiem jest tworzenie zasobu i uzyskanie klucza instrumentacji.  Usługa Application Insights, należy utworzyć w [portalu Azure.](https://portal.azure.com)

[Pełną dokumentację dla usługi Application Insights](https://docs.microsoft.com/azure/application-insights/)

1. Polecenie `+ Create a resource` w portalu Azure w ramach subskrypcji dzierżawy.  Tej dzierżawy nie jest dzierżawy usługi Azure AD B2C.  
2. Wyszukaj i wybierz `Application Insights`  
3. Utwórz zasób z `Application Type` `ASP.NET web application` w ramach subskrypcji swoich preferencji.
4. Po utworzeniu otworzyć zasobu usługi Application Insights i Uwaga  `Instrumentation Key` 

![Omówienie Insights aplikacji i klucza Instrumentacji](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-key.png)

## <a name="step-2-add-new-claimtype-definitions-to-your-trust-framework-extension-file"></a>Krok 2. Dodaj nowe definicje typu oświadczenia do pliku rozszerzenie Framework zaufania

### <a name="open-the-extension-file-from-the-starter-pack-and-add-the-following-elements-to-the-buildingblocks-node--the-extensions-filename-is-typically-yourtenantonmicrosoftcom-b2c1atrustframeworkextensionsxml"></a>Otwórz plik rozszerzenia z pakietu starter i dodaj następujące elementy do `<BuildingBlocks>` węzła.  Rozszerzenia nazwy pliku jest zwykle `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

```xml

    <ClaimsSchema>
      <ClaimType Id="EventType">
        <DisplayName>EventType</DisplayName>
        <DataType>string</DataType>
        <AdminHelpText />
        <UserHelpText />
      </ClaimType>
      <ClaimType Id="PolicyId">
        <DisplayName>PolicyId</DisplayName>
        <DataType>string</DataType>
        <AdminHelpText />
        <UserHelpText />
      </ClaimType>
      <ClaimType Id="Culture">
        <DisplayName>Culture</DisplayName>
        <DataType>string</DataType>
        <AdminHelpText />
        <UserHelpText />
      </ClaimType>
      <ClaimType Id="CorrelationId">
        <DisplayName>CorrelationId</DisplayName>
        <DataType>string</DataType>
        <AdminHelpText />
        <UserHelpText />
      </ClaimType>
      <!--Additional claims used for passing claims to Application Insights Provider -->
      <ClaimType Id="federatedUser">
        <DisplayName>federatedUser</DisplayName>
        <DataType>boolean</DataType>
        <UserHelpText />
      </ClaimType>
      <ClaimType Id="parsedDomain">
        <DisplayName>Parsed Domain</DisplayName>
        <DataType>string</DataType>
        <UserHelpText>The domain portion of the email address.</UserHelpText>
      </ClaimType>
      <ClaimType Id="userInLocalDirectory">
        <DisplayName>userInLocalDirectory</DisplayName>
        <DataType>boolean</DataType>
        <UserHelpText />
      </ClaimType>
    </ClaimsSchema>

```

## <a name="step-3-add-new-technical-profiles-that-use-the-application-insights-provider"></a>Krok 3. Dodaj nowy techniczne profilów, które korzystają z dostawcy usługi Application Insights

Profile techniczne można uznać funkcje w ramach obsługi tożsamości usługi Azure AD B2C.  Pięć profilów techniczne są zdefiniowane w tym przykładzie otwórz sesję programu do publikowania zdarzeń.

| Profil techniczne       | Zadanie |
| ----------------------------- |:---------------------------------------------|
| AzureInsights-Common | Wspólny zestaw parametrów, które mają zostać uwzględnione we wszystkich profilach techniczne Azure Insights     | 
| JourneyContextForInsights   | Otwiera sesji w usłudze App Insights i wysyła identyfikator korelacji |
| AzureInsights-SignInRequest     | Tworzy zdarzenie "Logowanie" z zestawu oświadczeń, gdy otrzymano żądanie logowania      | 
| AzureInsights-UserSignup | Tworzy zdarzenie o nazwie "UserSignup", gdy opcja signup zostało wyzwolone przez użytkownika w podróży signup/signin     | 
| AzureInsights-SignInComplete | rejestruje pomyślne zakończenie uwierzytelniania, gdy token zostało wysłane do jednostki uzależnionej strony aplikacji     | 

Dodaj profile do pliku rozszerzenie z pakietu starter przez dodanie tych elementów do `<ClaimsProviders>` węzła.  Rozszerzenia nazwy pliku jest zwykle `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

> [!IMPORTANT]
> Zmień `Instrumentation Key` w `ApplicationInsights-Common` techniczne profilu podany przez zasób usługi Application Insights identyfikator GUID.

```xml
<ClaimsProvider>
      <DisplayName>Application Insights</DisplayName>
      <TechnicalProfiles>

        <TechnicalProfile Id="JourneyContextForInsights">
          <DisplayName>Application Insights</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="CorrelationId" />
          </OutputClaims>
        </TechnicalProfile>

        <TechnicalProfile Id="AzureInsights-SignInRequest">
          <InputClaims>
            <!-- 
                            An input claim with a PartnerClaimType="eventName" is required. This is used by the AzureApplicationInsightsProvider
                            to create an event with the specified value.
                        -->
            <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInRequest" />
          </InputClaims>
          <IncludeTechnicalProfile ReferenceId="AzureInsights-Common" />
        </TechnicalProfile>

        <TechnicalProfile Id="AzureInsights-SignInComplete">
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInComplete" />
            <InputClaim ClaimTypeReferenceId="federatedUser" PartnerClaimType="{property:FederatedUser}" DefaultValue="false" />
            <InputClaim ClaimTypeReferenceId="parsedDomain" PartnerClaimType="{property:FederationPartner}" DefaultValue="Not Applicable" />
          </InputClaims>
          <IncludeTechnicalProfile ReferenceId="AzureInsights-Common" />
        </TechnicalProfile>

        <TechnicalProfile Id="AzureInsights-UserSignup">
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="UserSignup" />
          </InputClaims>
          <IncludeTechnicalProfile ReferenceId="AzureInsights-Common" />
        </TechnicalProfile>

        <TechnicalProfile Id="AzureInsights-Common">
          <DisplayName>Alternate Email</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <!-- The ApplicationInsights instrumentation key which will be used for logging the events -->
            <Item Key="InstrumentationKey">xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</Item>
            <!-- 
                            A boolean indicating whether delevoper mode is enabled. This controls how events are buffered. In a development environment
                            with minimal event volume, enabling developer mode results in events being sent immediately to ApplicationInsights.
                        -->
            <Item Key="DeveloperMode">false</Item>
            <!-- 
                            A boolean indicating whether telemetry should be enabled or not.
                        -->
            <Item Key="DisableTelemetry ">false</Item>
          </Metadata>
          <InputClaims>
            <!--
                            Properties of an event are added using the syntax {property:NAME} where NAME is the name of the property being added
                            to the event. DefaultValue can be either a static value or one resolved by one of the supported DefaultClaimResolvers.
                        -->
            <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
            <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:JourneyId}" />
            <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
          </InputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>

```

## <a name="step-4-add-the-technical-profiles-for-application-insights-as-orchestration-steps-in-an-existing-user-journey"></a>Krok 4. Dodaj profile techniczne dla usługi Application Insights, jak aranżacji czynnościach w ramach istniejącego przebieg użytkownika.

Wywołanie `JournyeContextForInsights` jako aranżacji krok 1

```xml
<!-- Initialize a session with Application Insights -->
<OrchestrationStep Order="1" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="JourneyContextForInsights" TechnicalProfileReferenceId="JourneyContextForInsights" />
          </ClaimsExchanges>
        </OrchestrationStep>
```

Wywołanie `Azure-Insights-SignInRequest` jako aranżacji kroku 2, aby śledzić Otrzymano żądanie logowania — w/tworzenia konta.

```xml
<!-- Track that we have received a sign in request -->
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AzureInsights-SignInRequest" />
          </ClaimsExchanges>
        </OrchestrationStep>
```

Natychmiast **przed** `SendClaims` aranżacji kroku, należy dodać nowy krok, który wywołuje `Azure-Insights-UserSignup`. Jest on wyzwalane, gdy kliknięto przycisk rejestracji w podróży signup/signin.

```xml
        <!-- Handles the user clicking the sign up link in the local account sign in page -->
        <OrchestrationStep Order="9" Type="ClaimsExchange">
          <Preconditions>
            <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
              <Value>newUser</Value>
              <Action>SkipThisOrchestrationStep</Action>
            </Precondition>
            <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
              <Value>newUser</Value>
              <Value>false</Value>
              <Action>SkipThisOrchestrationStep</Action>
            </Precondition>
          </Preconditions>
          <ClaimsExchanges>
            <ClaimsExchange Id="TrackUserSignUp" TechnicalProfileReferenceId="AzureInsights-UserSignup" />
          </ClaimsExchanges>
```

Natychmiast po `SendClaims` krok aranżacji, wywołaj `Azure-Insights-SignInComplete`.   W tym kroku zostaną one zastosowane pomyślnie zakończono podróży.

```xml
        <!-- Track that we have successfully sent a token -->
        <OrchestrationStep Order="11" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AzureInsights-SignInComplete" />
          </ClaimsExchanges>
        </OrchestrationStep>

```

> [!IMPORTANT]
> Po dodaniu nowego kroki aranżacji, ponowne numerowanie czynności sekwencyjne bez pominięcia dowolnej liczby całkowite z zakresu od 1 do N


## <a name="step-5-upload-your-modified-extensions-file-run-the-policy-and-view-events-in-application-insights"></a>Krok 5. Przesłać plik rozszerzenia zmodyfikowane, uruchamianie zasad i wyświetlać zdarzenia w usłudze Application Insights

Zapisz i Przekaż nowy plik rozszerzenia framework zaufania.  Następnie wywołaj metodę zasady zaufania jednostek uzależnionych od aplikacji lub użyj `Run Now` w interfejsie usługi Azure AD B2C.  W ciągu kilku sekund zdarzeń będzie dostępna w usłudze Application Insights.

1. Otwórz zasobu usługi Application Insights w dzierżawie usługi Azure Active Directory.
2. Polecenie `Events` w `USAGE` podmenu.
3. Ustaw `During` do `Last hour` i `By` do `3 minutes`.  Może zajść potrzeba kliknięcia przycisku `Refresh` Aby wyświetlić wyniki

![Application Insights USAGE-Events Blase](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-graphic.png)





##  <a name="next-steps"></a>NASTĘPNE KROKI

Dodaj dodatkowe oświadczenia i zdarzenia do użytkownika podróży odpowiednio do potrzeb.  Poniżej przedstawiono listę możliwych oświadczeń przy użyciu oświadczeń dodatkowe programy rozpoznawania nazw.

### <a name="culture-specific-claims"></a>Oświadczenia specyficzne dla kultury

```xml
Culture-specific Claims
            Referenced using {Culture:One of the property names below}
 
        /// An enumeration of the claims supported by the <see cref="JourneyCultureDefaultClaimProcessor"/>
        public enum SupportedClaim
        {
             /// The two letter ISO code for the language i.e. en
            LanguageName
             
            /// The two letter ISO code for the region i.e. US
            RegionName,
 
            /// The RFC5646 language code i.e. en-US
            RFC5646,

            /// The LCID of language code i.e. 1033
            LCID
        }

```

### <a name="policy-specific-claims"></a>Zasady dotyczące oświadczeń

```xml
Policy-specific Claims
Referenced using {Policy:One of the property names below}
 
        /// An enumeration of the claims supported by the <see cref="PolicyDefaultClaimProcessor"/> 
        public enum SupportedClaim
        {
            /// The trustframework tenant id
            TrustFrameworkTenantId,
  
            /// The tenant id of the relying party
            RelyingPartyTenantId,
 
            /// The policy id of the policy
            PolicyId,
 
            /// The tenant object id of the policy
            TenantObjectId
}

```

### <a name="openidconnect-specific-claims"></a>Oświadczenia właściwe OpenIDConnect

```xml
OpenIDConnect Specific Claims
Referenced using {OIDC:One of the property names below}
 
/// 
        /// An enumeration of the claims supported by the <see cref="OpenIdConnectDefaultClaimProcessor"/>

        public enum SupportedClaim
        {
            /// The OpenIdConnect prompt parameter
            Prompt,
 
            /// The OpenIdConnect login_hint parameter
            LoginHint,

            /// The OpenIdConnect domain_hint parameter
            DomainHint,
 
             /// The OpenIdConnect max_age parameter
            MaxAge,
 
            /// The OpenIdConnect client_id parameter
            ClientId,
 
            /// The OpenIdConnect username parameter
            Username,

            /// The OpenIdConnect password parameter
            Password,
 
            /// The OpenIdConnect resource type parameter
            Resource,
 
            /// The OpendIdConext acr_values parameter
             AuthenticationContextReferences
        }
 
```

### <a name="non-protocol-parameters-included-with-oidc--oauth2-requests"></a>Parametry non-protocol dołączone do żądania OIDC & OAuth2

```xml
Referenced using { OAUTH-KV:Querystring parameter name }
```

Nazwa parametru uwzględniane jako część żądania OIDC lub OAuth2 mogą być mapowane na oświadczenia w podróży użytkownika.  Następnie go mogą być rejestrowane zdarzenia. Na przykład żądanie od aplikacji może zawierać parametr ciągu zapytania o nazwie `app_session`, `loyalty_number` lub `any_string`.

Przykładowe żądanie od aplikacji:
```
https://login.microsoftonline.com/sampletenant.onmicrosoft.com/oauth2/v2.0/authorize?p=B2C_1A_signup_signin&client_id=e1d2612f-c2bc-4599-8e7b-d874eaca1ae1&nonce=defaultNonce&redirect_uri=https%3A%2F%2Fjwt.ms&scope=openid&response_type=id_token&prompt=login&app_session=0a2b45c&loyalty_number=1234567

```
Oświadczenia może być dodane przez dodanie elementu Input oświadczeń przy użyciu zdarzeń usługi Application Insights:
```
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="app_session" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="loyalty_number" DefaultValue="{OAUTH-KV:loyalty_number}" />
```

### <a name="other-system-claims"></a>Pozostałe roszczenia systemu

Niektóre oświadczenia system musi można było dodać do zbioru oświadczenia są dostępne do rejestrowania jako zdarzenia. Profil techniczne `SimpleUJContext` musi zostać wywołana jako etap aranżacji lub sprawdzania poprawności profilu techniczne przed oświadczenia te są dostępne.

```xml
<ClaimsProvider>
    <DisplayName>User Journey Context Provider</DisplayName>
        <TechnicalProfiles>
            <TechnicalProfile Id="SimpleUJContext">
                <DisplayName>User Journey Context Provide</DisplayName>
                <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
                <OutputClaims>
                    <OutputClaim ClaimTypeReferenceId="IP-Address" />
                    <OutputClaim ClaimTypeReferenceId="CorrelationId" />
                    <OutputClaim ClaimTypeReferenceId="DateTimeInUtc" />
                    <OutputClaim ClaimTypeReferenceId="Build" />
                 </OutputClaims>
            </TechnicalProfile>
        </TechnicalProfiles>
</ClaimsProvider>



