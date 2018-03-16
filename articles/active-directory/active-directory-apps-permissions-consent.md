---
title: "Aplikacje, uprawnienia i zgody w usłudze Azure Active Directory. | Microsoft Docs"
description: "Program Azure AD Connect umożliwia integrowanie katalogów lokalnych z usługą Azure Active Directory. Dzięki temu można posługiwać się wspólną tożsamością dla usługi Office 365, platformy Azure i aplikacji SaaS zintegrowanych z usługą Azure AD."
keywords: "wprowadzenie do usługi Azure AD, aplikacje, co to jest program Azure AD Connect, instalowanie usługi Active Directory"
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 25897cc4-7687-49b6-b0d5-71f51302b6b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/03/2018
ms.author: billmath
ms.reviewer: jesakowi
ms.custom: oldportal;it-pro;
ms.openlocfilehash: d3e14c18d5e4cd77f4c68d8a5d9d0b915e896695
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/14/2018
---
# <a name="apps-permissions-and-consent-in-azure-active-directory"></a>Aplikacje, uprawnienia i zgody w usłudze Azure Active Directory
W ramach usługi Azure Active Directory można dodać aplikacje do katalogu.  Aplikacje mogą się różnić w zależności od typu.  Aby wyświetlić aplikacje w portalu, wybierz katalog, a następnie wybierz aplikacje.

> [!IMPORTANT]
> Firma Microsoft zaleca zarządzanie usługą Azure AD przy użyciu [centrum administracyjnego usługi Azure AD](https://aad.portal.azure.com) w witrynie Azure Portal zamiast korzystania z witryny Azure Portal przywołanej w niniejszym artykule.

## <a name="types-of-apps"></a>Typy aplikacji

1. **Aplikacje z jedną dzierżawą** </br>
    - **Aplikacje z jedną dzierżawą** — często są nazywane aplikacjami biznesowymi. Mamy z nimi do czynienia, jeśli ktoś w organizacji opracuje własną aplikację i chce, aby użytkownicy w organizacji mogli się w niej logować.
    ![](media/active-directory-apps-permissions-consent/apps2.png)
    - **Aplikacje usługi Serwer proxy aplikacji** — jeśli udostępnisz aplikację lokalną przy użyciu usługi Serwer proxy aplikacji usługi Azure AD, aplikacja jednej dzierżawy zostanie zarejestrowana w Twojej dzierżawie (oprócz usługi Serwer proxy aplikacji). Ta aplikacja reprezentuje Twoją aplikację lokalną dla wszystkich interakcji w chmurze (na przykład na potrzeby uwierzytelniania). (Usługa Serwer proxy aplikacji wymaga usługi Azure AD w wersji Podstawowa lub wyższej).


2. **Aplikacje z wieloma dzierżawami**
    - **Aplikacje z wieloma dzierżawami, na które mogą się zgadzać inne osoby** — przypominają „aplikacje z jedną dzierżawą opracowywane w danej organizacji”. Główna różnica (oprócz logiki aplikacji) polega na tym, że użytkownicy z innych dzierżaw również mogą wyrazić zgodę na tę aplikację i zalogować się w niej.</br>
    ![](media/active-directory-apps-permissions-consent/apps4.png)
    - **Opracowane przez strony trzecie aplikacje z wieloma dzierżawami, na które może się zgodzić firma Contoso**. (W skrócie: „aplikacje, na które wyrażono zgodę”). Jest to odwrotność „aplikacji z wieloma dzierżawami opracowanych przez daną organizację”. Jeśli inna organizacja opracowała aplikację z wieloma dzierżawami, użytkownicy w Twojej organizacji mogą wyrazić na nią zgodę i zalogować się w niej.
    - **Aplikacje firmy Microsoft** — aplikacje, które reprezentują usługi firmy Microsoft. Zgoda wynika z faktu zarejestrowania się w usłudze. Niektóre aplikacje naszej firmy mają czasami specjalne środowisko użytkownika i logikę używane podczas ustanawiania zasad dostępu do aplikacji.</br>
    ![](media/active-directory-apps-permissions-consent/apps3.png)
    - **Wstępnie zintegrowane aplikacje** — aplikacje dostępne w galerii aplikacji usługi Azure AD, które można dodać do swojego katalogu w celu zapewnienia logowania jednokrotnego (a w niektórych przypadkach także aprowizacji) w popularnych aplikacjach typu oprogramowanie jako usługa.
    - **Logowanie jednokrotne w usłudze Azure AD**: „rzeczywiste” logowanie jednokrotne w aplikacjach, które można zintegrować z usługą Azure AD przy użyciu obsługiwanego protokołu logowania, takiego jak SAML 2.0 lub OpenID Connect. Kreator pomoże w odpowiednim skonfigurowaniu tej funkcji.
    - **Logowanie jednokrotne przy użyciu hasła**: usługa Azure AD bezpiecznie przechowuje poświadczenia użytkownika dotyczące aplikacji. Poświadczenia są wstawiane do formularza logowania przy użyciu rozszerzenia przeglądarki Dostęp do aplikacji dostępnego w usłudze Azure AD. To działanie jest znane również jako „magazynowanie haseł”.

## <a name="permissions"></a>Uprawnienia

Podczas rejestrowania aplikacji użytkownik przeprowadzający rejestrację aplikacji (czyli deweloper) definiuje uprawnienia i zasoby, do których aplikacja wymaga dostępu. (Zasoby są zdefiniowane jako inne aplikacje). Na przykład osoba tworząca aplikację do odczytu poczty e-mail może zdecydować, że aplikacja wymaga uprawnienia „Dostęp do skrzynek pocztowych jako zalogowany użytkownik” względem zasobu „Office 365 Exchange Online”:
    
![](media/active-directory-apps-permissions-consent/apps6.png)

Aby jedna aplikacja (klient) mogła żądać określonych uprawnień od innej aplikacji (zasobu), deweloper aplikacji zasobu definiuje istniejące uprawnienia. W tym przykładzie firma Microsoft, czyli właściciel aplikacji zasobu „Office 365 Exchange Online”, zdefiniowała uprawnienie „Dostęp do skrzynek pocztowych jako zalogowany użytkownik”.

Podczas definiowania uprawnień deweloper aplikacji musi zdefiniować, czy można wyrazić zgodę na uprawnienia, czy też wymagają one zgody administratora. Dzięki temu deweloperzy mogą umożliwić użytkownikom samodzielne wyrażenie zgody na aplikacje żądające uprawnień o niskim poziomie poufności, ale w przypadku bardziej rygorystycznych uprawnień mogą wymagać zgody administratorów. Na przykład aplikacja zasobu „Azure Active Directory” została tak zdefiniowana, że użytkownicy mogą wyrazić zgodę na aplikacje żądające ograniczonych uprawnień tylko do odczytu.  Jednak w przypadku pełnych uprawnień do odczytu i wszystkich uprawnień do zapisu jest wymagana zgoda administratora.

Ponieważ klienci natywni nie są uwierzytelniani, aplikacja zdefiniowana jako klient natywny może żądać jedynie uprawnień delegowanych. To znaczy, że w uzyskiwanie tokenu zawsze musi być zaangażowany rzeczywisty użytkownik. Podczas uzyskiwania tokenu dostępu aplikacje sieci Web i interfejsy API sieci Web (klienci poufni) zawsze muszą dokonywać uwierzytelniania za pomocą usługi Azure AD. Przekłada się to na możliwość zażądania uprawnień dotyczących tylko aplikacji. Dotyczy to na przykład sytuacji, w której jedna usługa zaplecza wymaga uwierzytelnienia w innej usłudze zaplecza. Aplikacje żądające uprawnień dotyczących tylko aplikacji zawsze wymagają zgody administratora.

Podsumowując:



- Aplikacja (klient) oświadcza, jakich uprawnień potrzebuje względem innych aplikacji (zasobów).
- Aplikacja (zasób) oświadcza, jakie uprawnienia są udostępniane dla innych aplikacji (klientów).
- Uprawnienia mogą dotyczyć tylko aplikacji lub być uprawnieniami delegowanymi.
- Uprawnienia delegowane mogą być oznaczona jako wymagające zgody użytkownika lub wymagające zgody administratora.
- Aplikacje mogą zachowywać się jak klient (deklarując, że wymagają uprawnień do zasobu), jak zasób (deklarując udostępniane uprawnienia) lub jak oba te typy.

## <a name="controls"></a>Kontrolki

Poniżej przedstawiono listę różnych kontrolek, za pomocą których administrator steruje wszystkimi tymi działaniami.

W witrynie Azure Portal są dostępne w obszarze **Zarządzanie**, **Ustawienia użytkownika**.

![](media/active-directory-apps-permissions-consent/apps11.png)



- Można zdecydować, czy użytkownicy mogą wyrazić zgodę na aplikacje:

W witrynie Azure Portal wybierz pozycję **Użytkownicy mogą zezwalać aplikacjom na dostęp do swoich danych**.
![](media/active-directory-apps-permissions-consent/apps12.png)



- Możesz kontrolować, czy użytkownicy mogą rejestrować własne aplikacje LOB pojedynczej dzierżawy:

W portalu Azure wybierz opcję **Użytkownicy mogą rejestrować aplikacje**.
![](media/active-directory-apps-permissions-consent/apps13.png)

>[!NOTE]
>Nawet jeśli zezwolisz użytkownikom na rejestrowanie aplikacji biznesowych z jedną dzierżawą, istnieją określone ograniczenia dotyczące rejestrowania.  
>Dotyczy to na przykład deweloperów, którzy nie są administratorami katalogów.
>
>- Z aplikacji z jedną dzierżawą użytkownicy nie mogą zrobić aplikacji z wieloma dzierżawami.
>- Podczas rejestrowania aplikacji biznesowych z jedną dzierżawą użytkownicy nie mogą zażądać uprawnień dotyczących tylko aplikacji względem innych aplikacji.
>- Podczas rejestrowania aplikacji biznesowych z jedną dzierżawą użytkownicy nie mogą zażądać uprawnień delegowanych względem innych aplikacji, jeśli uprawnienia te wymagają zgody administratora.
>- Użytkownicy mogą wprowadzać zmiany tylko w tych aplikacjach, których są właścicielami.


## <a name="example"></a>Przykład

Jako przykład weźmy aplikację „FabrikamMail for Office 365”, do której — jak zauważasz — logują się użytkownicy w Twojej dzierżawie. Aplikacja „FabrikamMail” to czytnik poczty e-mail dla systemu Android opublikowany przez firmę „Fabrikam, Inc.”. Można ją zaliczyć do kategorii „opracowane przez strony trzecie aplikacje z wieloma dzierżawami, na które może się zgodzić firma Contoso”.

Jeśli użytkownicy mogą wyrazić zgodę, podczas pierwszego logowania zobaczą monit o wyrażenie zgody: ![](media/active-directory-apps-permissions-consent/apps14.png)

„Dostęp do skrzynek pocztowych” to widoczna dla użytkowników pozycja dotycząca zgody, która odpowiada uprawnieniu „Dostęp do skrzynek pocztowych jako zalogowany użytkownik” udostępnionemu przez aplikację „Office 365 Exchange Online” (czyli aplikację Exchange).

Uprawnienia można zobaczyć, wyszukując obiekt ServicePrincipal dla aplikacji Exchange (zasób), który został dodany podczas tworzenia konta w usłudze Office 365. Obiekt ServicePrincipal można potraktować jako „wystąpienie” aplikacji w swojej dzierżawie służące do rejestrowania różnych opcji i konfiguracji.  Można to zaobserwować, używając polecenia `Get-AzureADServicePrincipal` w programie PowerShell.

    PS C:\> Get-AzureADServicePrincipal -ObjectId 383f7b97-6754-4d3d-9474-3908ebcba1c6 | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : Office 365 Exchange Online
    AppId                     : 00000002-0000-0ff1-ce00-000000000000
    AppOwnerTenantId          : 
    AppRoleAssignmentRequired : False
    AppRoles                  : {...}
    DisplayName               : Microsoft.Exchange
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {...
                                , class OAuth2Permission {
                                  AdminConsentDescription : Allows the app to have the same access to mailboxes as the signed-in user via Exchange Web Services.
                                  AdminConsentDisplayName : Access mailboxes as the signed-in user via Exchange Web Services
                                  Id                      : 3b5f3d61-589b-4a3c-a359-5dd4b5ee5bd5
                                  IsEnabled               : True
                                  Type                    : User
                                  UserConsentDescription  : Allows the app full access to your mailboxes on your behalf.
                                  UserConsentDisplayName  : Access your mailboxes
                                  Value                   : full_access_as_user
                                },
                                ...}
    PasswordCredentials       : {}
    PublisherName             : 
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {00000002-0000-0ff1-ce00-000000000000/outlook.office365.com, 00000002-0000-0ff1-ce00-000000000000/mail.office365.com, 00000002-0000-0ff1-ce00-000000000000/outlook.com, 
                                00000002-0000-0ff1-ce00-000000000000/*.outlook.com...}
    Tags                      : {}

Zgoda jest inicjowana, kiedy użytkownik kliknie przycisk „Akceptuj”. Najpierw w dzierżawie jest tworzony obiekt ServicePrincipal dla aplikacji „FabrikamMail for Office 365”. Obiekt ServicePrincipal wygląda mniej więcej tak:

    PS C:\> Get-AzureADServicePrincipal -SearchString "FabrikamMail for Office 365" | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : a8b16333-851d-42e8-acd2-eac155849b37
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : FabrikamMail for Office 365
    AppId                     : aba7c072-2267-4031-8960-e7a2db6e0590
    AppOwnerTenantId          : 4a4076e0-a70f-41c6-b819-6f9c4a86df89
    AppRoleAssignmentRequired : False
    AppRoles                  : {}
    DisplayName               : FabrikamMail for Office 365
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {}
    PasswordCredentials       : {}
    PublisherName             : Fabrikam, Inc.
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {aba7c072-2267-4031-8960-e7a2db6e0590}
    Tags                      : {WindowsAzureActiveDirectoryIntegratedApp}

Wyrażenie zgody na aplikację powoduje utworzenie linku Oauth2PermissionGrant między następującymi elementami:
  
- obiekt użytkownika,
- element ServicePrincipalName (SPN) aplikacji klientów,
- element ServicePrincipalName (SPN) aplikacji zasobów,
- uprawniania w aplikacji zasobów.  

W przypadku aplikacji FabrikamMail wygląda to następująco:

    PS C:\> Get-AzureADUserOAuth2PermissionGrant -ObjectId ddiggle@aadpremiumlab.onmicrosoft.com | fl *
    
    ClientId    : a8b16333-851d-42e8-acd2-eac155849b37
    ConsentType : Principal
    ExpiryTime  : 05/15/2017 07:02:39 AM
    ObjectId    : M2OxqB2F6EKs0urBVYSbN5d7PzhUZz1NlH25COvLocbJjoxkUFfRQauryBKwBWet
    PrincipalId : 648c8ec9-5750-41d1-abab-c812b00567ad
    ResourceId  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    Scope       : full_access_as_user
    StartTime   : 01/01/0001 12:00:00 AM

(Element **ClientId** to identyfikator obiektu nazwy głównej usługi aplikacji FabrikamMail, który właśnie został utworzony, element **PrincipalId** to identyfikator obiektu użytkownika, który wyraził zgodę, element **ResourceId** to identyfikator obiektu nazwy głównej usługi aplikacji Exchange, a element Scope to uprawnienia w aplikacji Exchange, na które wyrażono zgodę).

Jeśli użytkownicy nie mogą wyrazić zgody, zostanie wyświetlony ekran informujący, że uprawnienia są wymagane.

