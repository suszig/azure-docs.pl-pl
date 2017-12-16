---
title: "Opis manifestu aplikacji usługi Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Szczegółowe pokrycia manifest aplikacji usługi Azure Active Directory, który reprezentuje konfiguracji tożsamości aplikacji w dzierżawie usługi Azure AD i jest używana do przyspieszania OAuth autoryzacji, środowisko zgody i inne."
services: active-directory
documentationcenter: 
author: sureshja
manager: mtillman
editor: 
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: sureshja
ms.custom: aaddev
ms.reviewer: elisol
ms.openlocfilehash: f3284d4cbb15f21522549c678410815b54344744
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/16/2017
---
# <a name="azure-active-directory-application-manifest"></a>Manifest aplikacji w usłudze Azure Active Directory
Aplikacje do zintegrowania z usługą Azure AD musi być zarejestrowana w dzierżawie usługi Azure AD. Tę aplikację można skonfigurować przy użyciu manifest aplikacji (w ramach bloku usługi Azure AD) w [portalu Azure](https://portal.azure.com).

## <a name="manifest-reference"></a>Odwołanie do manifestu

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]
|Klucz  |Typ wartości |Przykładowa wartość  |Opis  |
|---------|---------|---------|---------|
|Identyfikator aplikacji     |  Ciąg identyfikatora       |""|  Unikatowy identyfikator dla aplikacji, która jest przypisany do aplikacji przez usługę Azure AD.|
|appRoles     |    Typ tablicy     |[{<br>&emsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"Użytkownik"<br>&emsp;],<br>&emsp;"opis": "Odczytu tylko dostęp do informacji o urządzeniu",<br>&emsp;"Nazwa wyświetlana": "Tylko do odczytu",<br>&emsp;"id": identyfikator guid<br>&emsp;"isEnabled": ma wartość true,<br>&emsp;"wartość": "ReadOnly"<br>}]|Kolekcja ról, które mogą zadeklarować aplikacji. Te role można przypisać do użytkowników, grupy lub podmiotów zabezpieczeń usługi.|
|AvailableToOtherTenants|Wartość logiczna|prawda|Jeśli ta wartość jest równa true, aplikacja jest dostępna na innych dzierżawców.  Jeśli ma wartość false, aplikacja jest tylko są one dostępne dla dzierżawcy jest zarejestrowany w.  Aby uzyskać więcej informacji, zobacz: [jak zarejestrować każdy użytkownik usługi Azure Active Directory (AD) przy użyciu wzorca wielodostępnych aplikacji](active-directory-devhowto-multi-tenant-overview.md). |
|Nazwa wyświetlana     |ciąg         |MyRegisteredApp         |Nazwa wyświetlana dla aplikacji. |
|errorURL     |ciąg         |http:<i></i>//MyRegisteredAppError         |Adres URL błędów występujących w aplikacji. |
|GroupMembershipClaims     |    ciąg     |    1     |   Maska bitowa konfigurującego wystawione oświadczenie "grupy" użytkownika lub tokenu dostępu protokołu OAuth 2.0, który oczekuje aplikacji. Wartości maski to: 0: Brak 1: grup zabezpieczeń i role usługi Azure AD, 2: zarezerwowane i 4: zastrzeżone. Ustawienie maski bitów 7 otrzyma wszystkich grup zabezpieczeń, grup dystrybucyjnych i ról katalogu usługi Azure AD, które zalogowany użytkownik jest członkiem.      |
|optionalClaims     |  ciąg       |     null    |    Opcjonalne oświadczenia zwrócony w tokenie przez usługę tokenu zabezpieczającego dla określonej aplikacji.     |
|acceptMappedClaims    |      Wartość logiczna   | prawda        |    Jeśli ta wartość jest ustawiana na wartość true pozwala aplikacji na używanie oświadczeń mapowania bez określania niestandardowych klucza podpisywania.|
|Strona główna     |  ciąg       |http:<i></i>//MyRegistererdApp         |    Adres URL do strony głównej aplikacji.     |
|identifierUris     |  Tablica ciągów       | http:<i></i>//MyRegistererdApp        |   URI(s) zdefiniowane przez użytkownika, które jednoznacznie zidentyfikować aplikacji sieci Web w swojej dzierżawy usługi Azure AD lub w zweryfikowanej domeny niestandardowej, gdy aplikacja jest wielodostępnej.      |
|keyCredentials     |   Typ tablicy      |   [{<br>&nbsp;"customKeyIdentifier": wartość null,<br>"endDate": "2018-09-13T00:00:00Z",<br>"Identyfikator klucza": "\<guid >",<br>"data_rozpoczęcia": "2017-09-12T00:00:00Z",<br>"type": "AsymmetricX509Cert"<br>"użycia": "Zweryfikuj"<br>"wartość": wartość null<br>}]      |   Ta właściwość zawiera odwołania do poświadczeń przypisane aplikacji, na podstawie ciągu wspólne klucze tajne i certyfikaty X.509.  Te poświadczenia wchodzić w grę podczas żądania tokenów dostępu (Jeśli aplikacja działa jako klient raczej który jako zasób).     |
|knownClientApplications     |     Typ tablicy    |    [identyfikator guid]     |     Wartość służy do zgody wiązanie, jeśli rozwiązanie, który zawiera dwie części, aplikacji klienckiej i niestandardowych aplikacji interfejsu API sieci web. Po wprowadzeniu identyfikator aplikacji klienta w tej wartości, użytkownik będzie miał tylko do zgody raz do aplikacji klienckiej. Usługi Azure AD będzie wiedzieć, że zgodę klienta oznacza zgodę niejawnie interfejsu API sieci web i będzie automatycznie obsługiwał nazwy główne usług zarówno dla klienta, jak i interfejs API sieci web w tym samym czasie, (zarówno klient, jak i aplikacji interfejsu API sieci web musi być zarejestrowana w tym samym dzierżawy).|
|logoutUrl     |   ciąg      |     http:<i></i>//MyRegisteredAppLogout    |   Adres URL wylogowania w aplikacji.      |
|oauth2AllowImplicitFlow     |   Wartość logiczna      |  fałsz       |       Określa, czy ta aplikacja sieci web może żądać OAuth2.0 przepływu niejawnego tokenów. Wartość domyślna to false. Służy to do aplikacji opartych na przeglądarce, takich jak aplikacje jednej strony Javascript. |
|oauth2AllowUrlPathMatching     |   Wartość logiczna      |  fałsz       |   Określa, czy w ramach żądania tokenu OAuth 2.0, usługi Azure AD będzie zezwalał na ścieżkę dopasowywania identyfikatora URI przekierowania dla aplikacji replyUrls. Wartość domyślna to false.      |
|oauth2Permissions     | Typ tablicy         |      [{<br>"adminConsentDescription": "Zezwalaj aplikacji na dostęp do zasobów w imieniu zalogowanego użytkownika",<br>"adminConsentDisplayName": "Zasób1 dostępu",<br>"id": "\<guid >",<br>"isEnabled": ma wartość true,<br>"type": "Użytkownika"<br>"userConsentDescription": "Zezwalaj aplikacji zasób1 w Twoim imieniu dostęp do",<br>"userConsentDisplayName": "Dostęp do zasobów"<br>"wartość": "user_impersonation"<br>}]   |  Kolekcja zakresy uprawnień OAuth 2.0, które ujawnia aplikacji interfejsu API (zasobów) w sieci web dla aplikacji klienckich. Te zakresy uprawnień można udzielić aplikacji klienckich podczas ich zgodę. |
|oauth2RequiredPostResponse     | Wartość logiczna        |    fałsz     |      Określa, czy w ramach żądania tokenu OAuth 2.0, usługi Azure AD będzie zezwalał na żądania POST, w przeciwieństwie do żądania GET. Wartość domyślna to false, która określa, czy mogą być tylko żądania GET.   
|Identyfikator obiektu     | Ciąg identyfikatora        |     ""    |    Unikatowy identyfikator dla aplikacji w katalogu.  To nie jest identyfikator używany do identyfikowania aplikacji w każdej transakcji protokołu.  Odwołanie do obiektu w katalogu zapytania jest użytkownika.|
|passwordCredentials     | Typ tablicy        |   [{<br>"customKeyIdentifier": wartość null,<br>"endDate": "2018-10-19T17:59:59.6521653Z",<br>"Identyfikator klucza": "\<guid >",<br>"data_rozpoczęcia": "2016-10-19T17:59:59.6521653Z",<br>"wartość": wartość null<br>}]      |    Zobacz opis właściwości keyCredentials.     |
|PublicClient     |  Wartość logiczna       |      fałsz   | Określa, czy aplikacja ma publicznego klienta (na przykład zainstalowanych aplikacji uruchomionej na urządzeniu przenośnym). Domyślna to false.        |
|supportsConvergence     |  Wartość logiczna       |   fałsz      | Tej właściwości nie powinny być edytowane.  Zaakceptuj wartość domyślną.        |
|replyUrls     |  Tablica ciągów       |   http:<i></i>//localhost     |  Ta właściwość wielowartościowe przechowuje listę wartości zarejestrowanych redirect_uri, akceptujących usługi Azure AD jako miejsca docelowe podczas returining tokenów. |
|RequiredResourceAccess     |     Typ tablicy    |    [{<br>"resourceAppId": "00000002-0000-0000-c000-000000000000"<br>"resourceAccess": [{<br>&nbsp;&nbsp;&nbsp;&nbsp;"id": "311a71cc-e848-46a1-bdf8-97ff7156d8e6"<br>&nbsp;&nbsp;&nbsp;&nbsp;"type": 'Scope'<br>&nbsp;&nbsp;}]<br>}]     |   Określa zasoby, które ta aplikacja wymaga dostępu do i z zestawem zakresy uprawnień uwierzytelniania OAuth i role aplikacji, które należy w każdej z tych zasobów. Tej wstępnej konfiguracji dostępu do wymaganych zasobów stacji środowisko zgody.|
|resourceAppId     |    Ciąg identyfikatora     |  ""      |   Unikatowy identyfikator dla aplikacji wymaga dostępu do zasobu. Ta wartość powinna być równa appId zadeklarowana w aplikacji docelowej zasobów.     |
|resourceAccess     |  Typ tablicy       | Zobacz przykład wartość właściwości requiredResourceAccess.        |   Lista OAuth2.0 zakresy uprawnień i ról aplikacji, które wymaga aplikacji z określonego zasobu (zawiera identyfikator i typ wartości określonych zasobów)        |
|samlMetadataUrl|ciąg|http:<i></i>//MyRegisteredAppSAMLMetadata|Adres URL metadanych SAML dla aplikacji.| 

## <a name="next-steps"></a>Następne kroki
* Aby uzyskać więcej informacji na relacje między obiektów aplikacji i nazwę główną usługi dla aplikacji, zobacz [aplikacji i usług obiektów principal w usłudze Azure AD][AAD-APP-OBJECTS].
* Zobacz [słownik dewelopera usługi Azure AD] [ AAD-DEVELOPER-GLOSSARY] definicje niektóre podstawowe koncepcje dewelopera usługi Azure Active Directory (AD).

Następująca sekcja komentarze służy do przekazywania opinii, ułatwiająca uściślić i kształtu zawartość.

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

