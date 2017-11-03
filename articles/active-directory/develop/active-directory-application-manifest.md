---
title: "Opis manifestu aplikacji usługi Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Szczegółowe pokrycia manifest aplikacji usługi Azure Active Directory, który reprezentuje konfiguracji tożsamości aplikacji w dzierżawie usługi Azure AD i jest używana do przyspieszania OAuth autoryzacji, środowisko zgody i inne."
services: active-directory
documentationcenter: 
author: sureshja
manager: mbaldwin
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
ms.openlocfilehash: d5e18f41d6eb69ccb7eafaa4de2646c4c38df5e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="understanding-the-azure-active-directory-application-manifest"></a>Opis manifestu aplikacji usługi Azure Active Directory
Aplikacje, które integrują się z usługi Azure Active Directory (AD) musi być zarejestrowana w dzierżawie usługi Azure AD, zapewniając konfiguracji trwałe tożsamości dla aplikacji. Ta konfiguracja jest konsultacje w czasie wykonywania, włączanie scenariusze, które umożliwiają aplikacjom zewnętrzny i broker uwierzytelniania/autoryzacji za pomocą usługi Azure AD. Aby uzyskać więcej informacji na temat modelu aplikacji usługi Azure AD, zobacz [Dodawanie, aktualizowanie i usuwanie aplikacji] [ ADD-UPD-RMV-APP] artykułu.

## <a name="updating-an-applications-identity-configuration"></a>Trwa aktualizowanie konfiguracji tożsamości aplikacji
Dostępne są faktycznie wiele opcji aktualizowania właściwości w konfiguracji tożsamości aplikacji, które różnią się w funkcji i stopni problemy, takie jak następujące:

* **[Portalu Azure] [ AZURE-PORTAL] interfejs użytkownika sieci Web** umożliwia zaktualizowanie typowych właściwości aplikacji. Jest to sposób podatne najszybszym i co najmniej błąd aktualizowanie właściwości aplikacji, ale nie daje pełny dostęp do wszystkich właściwości, takie jak kolejnych dwóch metod.
* Dla bardziej zaawansowanych scenariuszy, w których należy zaktualizować właściwości, które nie są widoczne w klasycznym portalu Azure, można zmodyfikować **manifest aplikacji**. To jest fokus w tym artykule i omówiono bardziej szczegółowo w następnej sekcji systemie.
* Istnieje również możliwość **napisać aplikację, która używa [interfejsu API programu Graph] [ GRAPH-API]**  można zaktualizować aplikacji, co wymaga najbardziej wysiłku. Może to być atrakcyjną opcję Chociaż, pisania oprogramowanie do zarządzania, czy należy zaktualizować właściwości aplikacji regularnie w zautomatyzowany sposób.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Aby zaktualizować konfigurację tożsamości aplikacji przy użyciu manifest aplikacji
Za pomocą [portalu Azure][AZURE-PORTAL], konfiguracja tożsamości aplikacji można zarządzać, aktualizując manifest aplikacji za pomocą wbudowanego edytora manifestu. Można również pobrać i przekaż manifest aplikacji w formacie JSON. Nie rzeczywisty plik znajduje się w katalogu. Manifest aplikacji jest jedynie HTTP GET operacji na obiekcie aplikacji interfejsu API Azure AD Graph i przekazywania jest operacji HTTP PATCH jednostki aplikacji.

W związku z tym, aby zrozumieć, format i właściwości manifest aplikacji, konieczne będzie odwoływać interfejsu API programu Graph [jednostek aplikacji] [ APPLICATION-ENTITY] dokumentacji. Przykłady aktualizacje, które mogą być wykonywane, chociaż przekazywania manifest aplikacji:

* **Deklarowanie zakresy uprawnień (oauth2Permissions)** udostępnianych przez interfejs API sieci web. Zobacz temat "Udostępnianie aplikacji sieci Web API na inne" w [integracji aplikacji z usługą Azure Active Directory] [ INTEGRATING-APPLICATIONS-AAD] informacje dotyczące implementacji personifikacji użytkownika za pomocą oauth2Permissions delegowane uprawnienia zakresu. Jak wspomniano wcześniej, właściwości jednostki aplikacji są udokumentowane w interfejsu API programu Graph [jednostki i typ złożony] [ APPLICATION-ENTITY] odwołanie do artykułu, w tym właściwości oauth2Permissions, który jest kolekcją z Typ [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].
* **Deklarowanie ról aplikacji (appRoles) udostępniany przez aplikację**. Właściwość appRoles jednostek aplikacji jest kolekcją typu [roli aplikacji][APPLICATION-ENTITY-APP-ROLE]. Zobacz [oparta na rolach kontrola dostępu w aplikacji w chmurze przy użyciu usługi Azure AD] [ RBAC-CLOUD-APPS-AZUREAD] artykule przykład implementacji.
* **Deklarowanie znane klienta aplikacji (knownClientApplications)**, umożliwiają logicznie powiązanie zgody określonego klienta aplikacji do zasobu/sieci web interfejsu API.
* **Żądanie usługi Azure AD można wystawić oświadczenie członkostwa grupy** dla zalogowanego użytkownika (groupMembershipClaims).  Może to również skonfigurowane do wystawiania oświadczenia dotyczące użytkownika katalogu ról członkostwa. Zobacz [autoryzacji w aplikacji w chmurze za pomocą grup usługi AD] [ AAD-GROUPS-FOR-AUTHORIZATION] artykule przykład implementacji.
* **Zezwalaj aplikacji do obsługi protokołu OAuth 2.0 niejawne Przyznaj** przepływów (oauth2AllowImplicitFlow). Ten typ przepływu grant jest używany osadzony stron sieci web JavaScript lub jednej strony aplikacji JEDNOSTRONICOWEJ. Aby uzyskać więcej informacji na udzielenie autoryzacji niejawne, zobacz [opis OAuth2 niejawne Przyznaj przepływu w usłudze Azure Active Directory][IMPLICIT-GRANT].
* **Zezwalaj na używanie X509 certyfikatów jako klucz tajny** (keyCredentials). Zobacz [tworzenie aplikacji usługi i demon w usłudze Office 365] [ O365-SERVICE-DAEMON-APPS] i [przewodnik dewelopera do uwierzytelniania z interfejsu API usługi Azure Resource Manager] [ DEV-GUIDE-TO-AUTH-WITH-ARM] artykułów Aby uzyskać przykłady implementacji.
* **Dodaj nowy identyfikator URI aplikacji** aplikacji (identifierURIs[]). Identyfikatory URI Identyfikatora aplikacji są używane do unikatowego identyfikowania aplikacji w ramach swojej dzierżawy usługi Azure AD (lub między wiele dzierżaw usługi Azure AD, dla wielodostępnych scenariusze, w których kwalifikowana za pomocą zweryfikowanej domeny niestandardowej). Są one używane podczas żądania uprawnień do aplikacji zasobów lub uzyskiwanie tokenu dostępu do zasobów aplikacji. Należy zaktualizować ten element, tę samą aktualizację się do odpowiedniego główną usługi servicePrincipalNames [] kolekcji, który znajduje się w dzierżawie macierzystego aplikacji.

Manifest aplikacji zawiera również dobrym sposobem na śledzenie stanu rejestracji aplikacji. Ponieważ jest ona dostępna w formacie JSON, można sprawdzić reprezentacja plików do kontroli źródła, wraz z kodu źródłowego aplikacji.

## <a name="step-by-step-example"></a>Krok przykładzie kroku
Umożliwia teraz wykonać kroki wymagane do zaktualizowania konfiguracji tożsamości aplikacji za pomocą manifest aplikacji. Wyróżnione jedną z powyższych przykładach przedstawiająca sposób zadeklarować nowego zakresu uprawnień w aplikacji zasobów:

1. Zaloguj się w witrynie [Azure Portal][AZURE-PORTAL].
2. Po uwierzytelniono, wybierz dzierżawy usługi Azure AD, wybierając ją z prawym górnym rogu strony.
3. Wybierz **usługi Azure Active Directory** rozszerzenia w panelu nawigacji po lewej stronie, kliknij polecenie **rejestracji aplikacji**.
4. Znajdź aplikację, którą chcesz zaktualizować na liście i kliknij go.
5. Na stronie aplikacji kliknij **manifestu** aby otworzyć Edytor manifestu w tekście. 
6. Można edytować bezpośrednio manifest przy użyciu tego edytora. Należy pamiętać, że manifest postępuje schematu [jednostek aplikacji] [ APPLICATION-ENTITY] jak wspomniano wcześniej: na przykład przy założeniu chcemy wdrożenie/Uwidacznianie nowe uprawnienie o nazwie "Employees.Read.All" na naszych zasób aplikacji (API), należy po prostu dodać element nowe na sekundę do kolekcji oauth2Permissions ie:
   
        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],
   
    Wpis musi być unikatowa, i dlatego należy wygenerować nowy globalnie unikatowego Identyfikatora (GUID) dla `"id"` właściwości. W takim przypadku ponieważ firma Microsoft określony `"type": "User"`, to uprawnienie można zgodę dla dowolnego konta uwierzytelniony przez usługę Azure AD dzierżawy rejestracji aplikacji zasobów/interfejsu API. Spowoduje to przydzielenie klienta aplikacji uprawnień dostępu do niego w imieniu konta. Ciągi Nazwa opis i wyświetlanie są używane podczas zgody i wyświetlane w portalu Azure.
6. Po zakończeniu aktualizowania manifest kliknij **zapisać** można zapisać manifestu.  
   
Teraz, gdy jest zapisywany plik manifestu, można nadać zarejestrowanego klienta aplikacji dostęp do nowego uprawnienia dodaliśmy powyżej. Teraz można użyć interfejsu użytkownika sieci Web portalu Azure zamiast edycji manifest aplikacji klienta:  

1. Przejdź najpierw do **ustawienia** bloku aplikacji klienckiej, do której chcesz dodać nowy interfejs API, kliknij przycisk **wymagane uprawnienia** i wybierz polecenie **wybierz interfejs API**.
2. Następnie użytkownik zostanie wyświetlone na liście zarejestrowanych zasobów aplikacji (API) w dzierżawie. Kliknij aplikację zasobów, aby go zaznaczyć, lub wpisz nazwę aplikacji, w polu wyszukiwania. Po znalezieniu aplikacji, kliknij przycisk **wybierz**.  
3. Spowoduje to przejście do **wybierz uprawnienia** strony, która wyświetli listę aplikacji uprawnienia delegowane uprawnienia dostępne dla aplikacji zasobów. Wybierz nowe uprawnienia, aby dodać go do klienta listę żądanych uprawnień. To nowe uprawnienie będą przechowywane w konfiguracji tożsamości aplikacji klienckiej, we właściwości kolekcji "requiredResourceAccess".


To już wszystko. Teraz aplikacji będzie uruchamiana za pomocą ich nowej konfiguracji tożsamości.

## <a name="next-steps"></a>Następne kroki
* Więcej szczegółów na relacje między obiektów aplikacji i nazwę główną usługi dla aplikacji, zobacz [aplikacji i usług obiektów principal w usłudze Azure AD][AAD-APP-OBJECTS].
* Zobacz [słownik dewelopera usługi Azure AD] [ AAD-DEVELOPER-GLOSSARY] definicje niektóre podstawowe koncepcje dewelopera usługi Azure Active Directory (AD).

Użyj sekcji komentarze poniżej, aby wyrazić swoją opinię i pomóc nam dostosować i kształtu zawartość.

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

