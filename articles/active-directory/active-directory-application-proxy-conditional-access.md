---
title: "Warunkowy dostęp do aplikacji lokalnych — usługi Azure AD | Dokumentacja firmy Microsoft"
description: "Uwzględniono również sposób skonfigurować dostęp warunkowy do aplikacji opublikowanych jako dostępne zdalnie za pomocą serwera Proxy aplikacji usługi Azure AD."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2e97722b-eb4e-4078-b607-9fed210d8a0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 463946256f9e335fa6d98fc904835e5c3dc2725e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="working-with-conditional-access-in-azure-ad-application-proxy"></a>Praca z dostępu warunkowego za pomocą serwera Proxy aplikacji usługi Azure AD

>[!NOTE]
>Ten artykuł dotyczy klasycznego portalu Azure, która została wycofana. Firma Microsoft zaleca użycie [portalu Azure](https://portal.azure.com). W portalu Azure serwer Proxy aplikacji aplikacje mają te same funkcje dostępu warunkowego, jak każda inna aplikacja SaaS. Aby dowiedzieć się więcej na temat dostępu warunkowego, zobacz [wprowadzenie dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

Można skonfigurować reguły dostępu, aby przyznać dostęp warunkowy do aplikacji opublikowanych przy użyciu serwera Proxy aplikacji. Umożliwia to:

* Wymagaj uwierzytelniania wieloskładnikowego na aplikację
* Wymagaj uwierzytelniania wieloskładnikowego tylko wtedy, gdy użytkownicy nie są w miejscu pracy
* Uniemożliwić użytkownikom uzyskiwanie dostępu do aplikacji, jeśli nie są one w miejscu pracy

Te reguły można zastosować do wszystkich użytkowników i grup lub tylko do konkretnych użytkowników i grup. Domyślnie ta reguła ma zastosowanie do wszystkich użytkowników, którzy mają dostęp do aplikacji. Jednak reguły można również ograniczony do użytkowników, którzy są członkami określonych grup zabezpieczeń.  

Reguły dostępu są oceniane, gdy użytkownik uzyskuje dostęp do aplikacji federacyjnych, która używa protokołu OAuth 2.0, OpenID Connect, SAML i WS-Federation. Ponadto reguły dostępu są oceniane przy OAuth 2.0 i OpenID Connect, gdy token odświeżania jest używany do uzyskania tokenu dostępu.

## <a name="conditional-access-prerequisites"></a>Wymagania wstępne dostępu warunkowego
* Subskrypcja usługi Azure Active Directory Premium
* Federacyjnych lub zarządzanego dzierżawcy usługi Azure Active Directory
* Dzierżaw federacyjnych wymusić uwierzytelnianie wieloskładnikowe (MFA)  
    ![Konfigurowanie reguł dostępu — wymusić uwierzytelnianie wieloskładnikowe](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Konfigurowanie uwierzytelniania wieloskładnikowego dla każdej aplikacji
1. Zaloguj się jako administrator w klasycznym portalu Azure.
2. Przejdź do obszaru Usługa Active Directory i wybierz katalog, w którym chcesz włączyć serwer proxy aplikacji.
3. Kliknij przycisk **aplikacji** i przewiń w dół do **reguły dostępu** sekcji. W sekcji reguły dostępu jest wyświetlana tylko dla aplikacji publikowanych przy użyciu serwera Proxy aplikacji, które używają uwierzytelniania federacyjnego.
4. Włącz regułę, wybierając **Włącz zasady dostępu** do **na**.
5. Określ użytkowników i grup, do których stosowane zasady. Użyj **Dodaj grupę** przycisk, aby wybrać co najmniej jedną grupę, do których jest stosowana reguła dostępu. W tym oknie dialogowym można również usunąć wybrane grupy.  Po wybraniu opcji zasady stosowane do grupy, zasady dostępu są wymuszane tylko w przypadku użytkowników, którzy należą do jednej z określonych grup zabezpieczeń.  

   * Aby jawnie wykluczyć grup zabezpieczeń z reguły, sprawdź **z wyjątkiem** i określ co najmniej jedną grupę. Użytkownicy, którzy są członkami grupy na liście Except nie są wymagane do przeprowadzenia uwierzytelniania wieloskładnikowego.  
   * Jeśli użytkownik został skonfigurowany przy użyciu funkcji uwierzytelniania wieloskładnikowego na użytkownika, to ustawienie ma pierwszeństwo przed zasadami uwierzytelniania wieloskładnikowego aplikacji. Użytkownik, który został skonfigurowany dla poszczególnych użytkowników usługi Multi-Factor authentication jest wymagane do przeprowadzenia uwierzytelniania wieloskładnikowego, nawet wtedy, gdy zostały zwolnione z zasadami uwierzytelniania wieloskładnikowego aplikacji. Dowiedz się więcej o [ustawienia uwierzytelniania i dla poszczególnych użytkowników usługi Multi-Factor](../multi-factor-authentication/multi-factor-authentication.md).
6. Wybierz regułę dostępu, który chcesz ustawić:

   * **Wymagaj uwierzytelniania wieloskładnikowego**: użytkowników, do których stosowane zasady dostępu są wymagane do ukończenia uwierzytelnianie wieloskładnikowe przed uzyskaniem dostępu do aplikacji, do której ta reguła ma zastosowanie.
   * **Wymagaj uwierzytelniania wieloskładnikowego, gdy nie w pracy**: próby uzyskania dostępu do aplikacji z zaufanego adresu IP użytkownicy nie muszą przeprowadzać uwierzytelnianie wieloskładnikowe. Zaufany zakresów adresów IP można skonfigurować na stronie Ustawienia usługi Multi-Factor authentication.
   * **Blokowanie dostępu, gdy nie w pracy**: użytkownicy, próby uzyskania dostępu do aplikacji z spoza sieci firmowej, nie będą mogli uzyskać dostęp do aplikacji.

## <a name="configuring-mfa-for-federation-services"></a>Konfigurowanie uwierzytelniania MFA dla usług federacyjnych
W przypadku dzierżaw federacyjnych, uwierzytelnianie wieloskładnikowe (MFA) mogą być wykonywane przez usługę Azure Active Directory lub lokalnej serwera usług AD FS. Domyślnie uwierzytelnianie wieloskładnikowe jest przeprowadzana na każdej stronie hostowanej przez usługę Azure Active Directory. Aby skonfigurować uwierzytelnianie wieloskładnikowe lokalnymi, uruchom program Windows PowerShell i użyj właściwości SupportsMFA — można ustawić modułu Azure AD.

Poniższy przykład pokazuje, jak włączyć uwierzytelnianie wieloskładnikowe lokalnymi przy użyciu [polecenia cmdlet Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) dzierżawcy contoso.com:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

Oprócz ustawienia tej flagi, należy skonfigurować wystąpienie dzierżawy federacyjnej AD FS przeprowadzać uwierzytelnianie wieloskładnikowe. Postępuj zgodnie z instrukcjami dotyczącymi [wdrażanie Microsoft Azure Multi-Factor authentication lokalnymi](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="see-also"></a>Zobacz też
* [Praca z aplikacjami obsługującymi oświadczenia](active-directory-application-proxy-claims-aware-apps.md)
* [Publikowanie aplikacji przy użyciu serwera Proxy aplikacji](active-directory-application-proxy-publish.md)
* [Włączanie logowania jednokrotnego](active-directory-application-proxy-sso-using-kcd.md)
* [Publikowanie aplikacji przy użyciu własnej nazwy domeny](active-directory-application-proxy-custom-domains.md)

Aby zapoznać się z najnowszymi informacjami i aktualizacjami, zobacz [blog dotyczący serwera proxy aplikacji](http://blogs.technet.com/b/applicationproxyblog/)
