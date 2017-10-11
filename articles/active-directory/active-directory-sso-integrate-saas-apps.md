---
title: "Integrowanie usługi Azure Active Directory logowania jednokrotnego dla aplikacji SaaS | Dokumentacja firmy Microsoft"
description: "Włączanie uwierzytelniania rejestracji jednokrotnej i inicjowania obsługi administracyjnej scentralizowanego zarządzania dostępu aplikacji SaaS w usłudze Azure Active Directory użytkownika. Przegląd sposobu integracji Azure Active Directory do aplikacji SaaS."
services: active-directory
keywords: "Integrowanie usługi Azure AD dla aplikacji SaaS"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 53b9d341-d1fc-4bbb-ac7c-3f4c68fcf00a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: aaronsm
ms.openlocfilehash: fc0d297598c334ca8f6f8a2bd3ae948c87956342
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Integrowanie usługi Azure Active Directory logowania jednokrotnego dla aplikacji SaaS
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](active-directory-enterprise-apps-manage-sso.md)
> * [Klasyczna witryna Azure Portal](active-directory-sso-integrate-saas-apps.md)
>
>

[!INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

Aby rozpocząć konfigurowanie rejestracji jednokrotnej dla aplikacji, które w przypadku wprowadzenia w Twojej organizacji, używany będzie istniejący katalog w usłudze Azure Active Directory (Azure AD). Możesz użyć katalog usługi Azure AD, którą można uzyskać za pośrednictwem Microsoft Azure, Office 365 lub usługi Windows Intune. Jeśli masz dwie lub więcej z nich, zobacz [administrowanie katalogiem usługi Azure AD](active-directory-administer.md) ustalenie, który z nich do użycia.

> [!IMPORTANT]
> Firma Microsoft zaleca zarządzanie usługą Azure AD przy użyciu [centrum administracyjnego usługi Azure AD](https://aad.portal.azure.com) w witrynie Azure Portal zamiast korzystania z klasycznej witryny Azure Portal przywołanej w niniejszym artykule. Do przypisywania ról administratora w Centrum administracyjnym usługi Azure AD, zobacz [przypisywanie ról administratorów w usłudze Azure Active Directory](active-directory-enterprise-apps-manage-sso.md).

## <a name="authentication"></a>Authentication
W przypadku aplikacji, które obsługują usługi WS-Federation, SAML 2.0 lub protokoły OpenID Connect, używa usługi Azure Active Directory podpisywania certyfikatów do ustanawiania relacji zaufania. Aby uzyskać więcej informacji na ten temat, zobacz [zarządzanie certyfikatami dla federacyjne logowanie jednokrotne](active-directory-sso-certs.md).

W przypadku aplikacji, które obsługują tylko HTML oparte na formularzach logowania, usługi Azure Active Directory używa "archiwizowanie haseł" do ustanawiania relacji zaufania. Dzięki temu użytkownicy w organizacji mogą być zostanie automatycznie zalogowany do aplikacji SaaS przez usługę Azure AD przy użyciu informacji o koncie użytkownika z aplikacji SaaS. Usługi Azure AD zbiera i bezpiecznie przechowywane są informacje o koncie użytkownika i hasło powiązane. Aby uzyskać więcej informacji, zobacz [opartego na hasłach rejestracji jednokrotnej](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Autoryzacja
Konto elastycznie umożliwia użytkownikowi mieć autoryzację do korzystania z aplikacji po uwierzytelnienia za pośrednictwem rejestracji jednokrotnej. Inicjowanie obsługi użytkowników można ręcznie lub w niektórych przypadkach można dodawać i usuwać informacje o użytkowniku z aplikacji SaaS, na podstawie zmian wprowadzonych w usłudze Azure Active Directory. Aby uzyskać więcej informacji na temat używania istniejące łączniki usługi Azure AD dla automatycznego inicjowania obsługi administracyjnej, zobacz [automatycznego użytkownika aprowizację i anulowanie obsługi dla aplikacji SaaS](active-directory-saas-app-provisioning.md).

W przeciwnym razie ręcznie dodać informacje użytkownika do aplikacji, lub użycie innych rozwiązań inicjowania obsługi, które są dostępne w witrynie marketplace.

## <a name="access"></a>Dostęp
Usługa Azure AD zapewnia kilka metod można dostosować do wdrożenia aplikacji dla użytkowników końcowych w organizacji. Nie są zablokowane do żadnego konkretnego wdrożenia rozwiązania dostępu. Można użyć [rozwiązanie, które najlepiej odpowiada Twoim potrzebom](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Dodatkowe zagadnienia dotyczące aplikacji już w użyciu.
Konfigurowanie rejestracji jednokrotnej na dla aplikacji, która używa Twoja organizacja już jest inny proces z procesu tworzenia nowych kont dla nowej aplikacji. Istnieje kilka kroków wstępne w tym: Mapowanie tożsamości użytkowników w aplikacji tożsamości usługi Azure AD i zrozumienie, jak użytkownicy mogą mieć logowanie do aplikacji po jest zintegrowany.

> [!NOTE]
> Aby skonfigurować logowanie Jednokrotne dla istniejącej aplikacji, musisz mieć uprawnienia administratora globalnego w obu usługi Azure AD i aplikacji SaaS.
>
>

### <a name="mapping-user-accounts"></a>Mapowania konta użytkowników
Zazwyczaj tożsamość użytkownika ma unikatowy identyfikator, który może być adres e-mail lub główna nazwa użytkownika (UPN). Konieczne będzie łącza (map) każdego użytkownika tożsamości aplikacji do ich odpowiednich tożsamości usługi Azure AD. Istnieje kilka sposobów, aby to zrobić, w zależności od tego, jak wymaganie uwierzytelniania aplikacji.

Aby uzyskać więcej informacji o mapowaniu tożsamości aplikacji z tożsamości usługi Azure AD, zobacz [Dostosowywanie oświadczeń wydanych w tokenie SAML](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) i [Dostosowywanie mapowań atrybutów do inicjowania obsługi](active-directory-saas-customizing-attribute-mappings.md).

### <a name="understanding-the-users-log-in-experience"></a>Opis użytkownika logowania
Po zintegrowaniu logowania jednokrotnego dla aplikacji, która jest już w użyciu jest zdawać sobie sprawę, że środowisko użytkownika będzie dotyczył. Dla wszystkich aplikacji użytkowników zostanie uruchomiony przy użyciu swoich poświadczeń usługi Azure AD do logowania. Możliwe również, że powinien użyć innego portalu dostępu do aplikacji.

Usługa rejestracji Jednokrotnej dla niektórych aplikacji może odbywać się na rejestracji aplikacji interfejsu, ale dla innych aplikacji, użytkownik będzie musiał przejść za pośrednictwem portalu centralnej, takich jak ([Moje aplikacje](http://myapps.microsoft.com) lub [usługi Office 365](http://portal.office.com/myapps)) do logowania. Dowiedz się więcej o różnych typach logowania jednokrotnego i ich możliwości użytkowników w [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

Jest inny zasób cenne *zgody użytkownika z pominięciem* w [deweloperzy Guiding](active-directory-applications-guiding-developers-for-lob-applications.md) artykułu.

## <a name="next-steps"></a>Następne kroki
Dla aplikacji SaaS, które możesz znaleźć w galerii aplikacji usługi Azure AD zapewnia szereg [samouczków dotyczących sposobów integracji aplikacji SaaS](active-directory-saas-tutorial-list.md).

Jeśli aplikacja nie jest w galerii aplikacji, możesz [dodać go do galerii aplikacji Azure AD jako aplikację niestandardową](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

Jest znacznie więcej szczegółów na wszystkich tych problemów w witrynie Azure.com biblioteki, począwszy od [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md).

Ponadto nie zostały pominięte [indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md).
