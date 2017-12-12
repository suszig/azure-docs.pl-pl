---
title: "Dostosowywanie oświadczeń wydanych w tokenie SAML dla aplikacji przedsiębiorstwa w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dostosowywanie oświadczeń wydanych w tokenie SAML dla aplikacji przedsiębiorstwa w usłudze Azure Active Directory"
services: active-directory
documentationcenter: 
author: jeevansd
manager: mtillman
editor: 
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: 7394857f55493b072e6ea549c8eeec54a808f5e5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="customizing-claims-issued-in-the-saml-token-for-enterprise-applications-in-azure-active-directory"></a>Dostosowywanie oświadczeń wydanych w tokenie SAML dla aplikacji przedsiębiorstwa w usłudze Azure Active Directory
Dzisiaj usługi Azure Active Directory obsługuje rejestracji jednokrotnej w większości aplikacji przedsiębiorstwa, w tym obie aplikacje wstępnie zintegrowane w galerii aplikacji Azure AD, a także niestandardowych aplikacji. Podczas uwierzytelniania użytkownika do aplikacji za pomocą usługi Azure AD przy użyciu protokołu SAML 2.0, usługi Azure AD wysyła token do aplikacji (za pośrednictwem protokołu HTTP POST). A następnie aplikacja sprawdza poprawność tokenu i używa do logowania użytkownika zamiast monitowania o nazwę użytkownika i hasło. Te tokeny SAML zawierają informacje o użytkowniku, nazywany "oświadczeń".

W tożsamości mowy, "roszczenie" informacje stwierdzający przez dostawcę tożsamości użytkownika w tokenie wydawać się one dla tego użytkownika. W [tokenu SAML](http://en.wikipedia.org/wiki/SAML_2.0), dane te zwykle znajduje się w instrukcji atrybutu SAML. Unikatowy identyfikator użytkownika jest zazwyczaj reprezentowany w podmiocie SAML, nazywany również jako nazwy identyfikatora.

Domyślnie program Azure Active Directory wystawia SAML token do aplikacji, która zawiera oświadczenie NameIdentifier o wartości nazwy użytkownika (AKA główna nazwa użytkownika) w usłudze Azure AD. Ta wartość może jednoznacznie zidentyfikować użytkownika. SAML token zawiera również dodatkowe roszczenia, zawierające adres e-mail użytkownika, imię i nazwisko.

Aby wyświetlić lub edytować oświadczeń wydanych w tokenie SAML do aplikacji, należy otworzyć aplikację w portalu Azure. Następnie wybierz **widoku i edytować wszystkie atrybuty użytkowników** checkbox w **atrybuty użytkownika** części aplikacji.

![Użytkownik atrybutów sekcji][1]

Istnieją dwie możliwe przyczyny, dlaczego konieczne może być Edycja oświadczeń wydanych w tokenie SAML:
* Aplikacja została zapisana wymagają innego zestawu oświadczeń identyfikatorów URI lub wartości oświadczeń.
* Aplikacja została wdrożona w taki sposób, który wymaga oświadczeń NameIdentifier inny niż nazwa użytkownika (nazwy głównej użytkownika AKA) przechowywanych w usłudze Azure Active Directory.

Możesz edytować dowolne domyślnych wartości oświadczenia. Zaznacz wiersz oświadczeń w tabeli atrybutów tokenu SAML. Spowoduje to otwarcie **atrybutu Edytuj** sekcji, a następnie możesz edytować nazwę oświadczenia, wartość oraz skojarzony z oświadczeniem przestrzeni nazw.

![Edytuj atrybut użytkownika][2]

Można również usunąć oświadczeń (inne niż NameIdentifier), za pomocą menu kontekstowego, który zostanie otwarty, klikając **...**  ikony.  Możesz także dodać nowe oświadczeń przy użyciu **Dodaj atrybut** przycisku.

![Edytuj atrybut użytkownika][3]

## <a name="editing-the-nameidentifier-claim"></a>Edycja oświadczeń NameIdentifier
Aby rozwiązać ten problem, na którym aplikacja została wdrożona przy użyciu innej nazwy użytkownika, kliknij **identyfikator użytkownika** listy rozwijanej **atrybuty użytkownika** sekcji. Ta akcja zawiera okno dialogowe z kilku różnych opcji:

![Edytuj atrybut użytkownika][4]

Z listy rozwijanej wybierz **user.mail** można ustawić NameIdentifier oświadczenie będzie adres e-mail użytkownika w katalogu. Lub wybierz **user.onpremisessamaccountname** ustawioną użytkownika jego nazwa konta SAM, które zostały zsynchronizowane z lokalnej usługi Azure AD.

Można również użyć specjalną **ExtractMailPrefix()** funkcji, aby usunąć sufiks domeny adresu e-mail, nazwa konta SAM lub główna nazwa użytkownika. Spowoduje to wyodrębnienie tylko pierwsza część nazwy użytkownika, są przekazywane (na przykład "joe_smith" zamiast joe_smith@contoso.com).

![Edytuj atrybut użytkownika][5]

Mamy teraz również został dodany **join()** funkcji w celu przyłączenia do domeny zweryfikowane z wartością identyfikatora użytkownika. Po wybraniu funkcji join() w **identyfikator użytkownika** najpierw wybierz identyfikator użytkownika podobne wiadomości e-mail adres lub główna nazwa użytkownika, a następnie w drugiej listy rozwijanej wybierz swoją domenę zweryfikowaną. Jeśli wybierz adres e-mail z zweryfikowanej domeny, a następnie usługi Azure AD wyodrębnia nazwy użytkownika z pierwszego joe_smith wartości z joe_smith@contoso.com i dołącza go z contoso.onmicrosoft.com. Zobacz poniższy przykład:

![Edytuj atrybut użytkownika][6]

## <a name="adding-claims"></a>Dodawanie oświadczeń
Podczas dodawania oświadczenia, można określić nazwy atrybutu (co ściśle nie należy wykonywać wzorzec URI zgodnie z harmonogramem specyfikacji SAML). Ustaw wartość na dowolny atrybut użytkownika, który jest przechowywany w katalogu.

![Dodaj atrybut użytkownika][7]

Na przykład musisz wysłać działu, której należy użytkownik w organizacji jako oświadczeń (na przykład, sprzedaż). Wprowadź nazwę oświadczenia, zgodnie z oczekiwaniami przez aplikację, a następnie wybierz **user.department** jako wartość.

> [!NOTE]
> Jeśli dla danego użytkownika nie ma żadnej wartości przechowywanych dla wybranego atrybutu, następnie roszczenie nie został opublikowany w tokenie.

> [!TIP]
> **User.onpremisesecurityidentifier** i **user.onpremisesamaccountname** są obsługiwane tylko podczas synchronizowania danych użytkownika z lokalnej usługi Active Directory przy użyciu [usługi Azure AD Połączenia narzędzia](../active-directory-aadconnect.md).

## <a name="restricted-claims"></a>Ograniczone oświadczeń

Brak niektórych ograniczeniami oświadczeń w SAML. Jeśli dodasz te oświadczenia, następnie usługi Azure AD nie będzie wysyłać te oświadczenia. SAML ograniczony zestaw oświadczeń są następujące:

    | Typ oświadczenia (URI) |
    | ------------------- |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/Expiration |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/EXPIRED |
    | http://schemas.microsoft.com/Identity/Claims/accesstoken |
    | http://schemas.microsoft.com/Identity/Claims/openid2_id |
    | http://schemas.microsoft.com/Identity/Claims/identityprovider |
    | http://schemas.microsoft.com/Identity/Claims/objectidentifier |
    | http://schemas.microsoft.com/Identity/Claims/PUID |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/NameIdentifier [MR1] |
    | http://schemas.microsoft.com/Identity/Claims/tenantid |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/AuthenticationInstant |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/AuthenticationMethod |
    | http://schemas.microsoft.com/accesscontrolservice/2010/07/Claims/identityprovider |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/groups |
    | http://schemas.microsoft.com/Claims/groups.Link |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/role |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/wids |
    | http://schemas.microsoft.com/2014/09/devicecontext/Claims/iscompliant |
    | http://schemas.microsoft.com/2014/02/devicecontext/Claims/isknown |
    | http://schemas.microsoft.com/2012/01/devicecontext/Claims/ismanaged |
    | http://schemas.microsoft.com/2014/03/psso |
    | http://schemas.microsoft.com/Claims/authnmethodsreferences |
    | http://schemas.xmlsoap.org/ws/2009/09/Identity/Claims/Actor |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/samlissuername |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/confirmationkey |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/windowsaccountname |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/primarygroupsid |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/primarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/authorizationdecision |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/Authentication |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/SID |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/denyonlyprimarygroupsid |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/denyonlyprimarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/denyonlysid |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/denyonlywindowsdevicegroup |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/windowsdeviceclaim |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/windowsdevicegroup |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/windowsfqbnversion |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/windowssubauthority |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/windowsuserclaim |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/x500distinguishedname |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/UPN |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/GroupSID |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/SPN |
    | http://schemas.microsoft.com/WS/2008/06/Identity/Claims/ispersistent |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/privatepersonalidentifier |
    | http://schemas.microsoft.com/Identity/Claims/SCOPE |

## <a name="next-steps"></a>Następne kroki
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](../active-directory-apps-index.md)
* [Konfigurowanie logowania jednokrotnego do aplikacji, które nie znajdują się w galerii aplikacji Azure Active Directory](../application-config-sso-how-to-configure-federated-sso-non-gallery.md)
* [Rozwiązywanie problemów z systemem SAML logowania jednokrotnego](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/user-attribute-section.png
[2]: ./media/active-directory-saml-claims-customization/edit-claim-name-value.png
[3]: ./media/active-directory-saml-claims-customization/delete-claim.png
[4]: ./media/active-directory-saml-claims-customization/user-identifier.png
[5]: ./media/active-directory-saml-claims-customization/extractemailprefix-function.png
[6]: ./media/active-directory-saml-claims-customization/join-function.png
[7]: ./media/active-directory-saml-claims-customization/add-attribute.png
