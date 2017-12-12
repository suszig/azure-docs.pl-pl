---
title: "Jak ustalić, jakie jednokrotnego metodę | Dokumentacja firmy Microsoft"
description: "Zrozumienie jednej metody logowania jednokrotnego obsługiwane przez usługę Azure AD i wybieranie odpowiedniej opcji dla aplikacji planuje się."
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: c75eba01cc98a5ed3df4f51cb024d82be49f97f4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-determine-what-single-sign-on-method-to-use"></a>Jak ustalić, jakie jednokrotnego metoda do użycia

Ten artykuł ułatwia zrozumienie jednej metody logowania jednokrotnego obsługiwane przez usługę Azure AD i wybieranie odpowiedniej opcji dla aplikacji planuje się.

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Logowanie jednokrotne i Inicjowanie obsługi trybów obsługiwanych przez typy określonych aplikacji

W poniższej tabeli opisano różne rejestracji jednokrotnej i inicjowania obsługi administracyjnej trybów obsługiwanych przez każdą z powyższych typów aplikacji. Aby ułatwić zrozumienie aplikacji, które należy dodać do obsługi określonego celu, można użyć tej tabeli.

  ![Tabela typów region](./media/application-tables/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a>Jak wybrać tryb pojedynczy logowania jednokrotnego

Obsługiwane **logowanie jednokrotne** tryby aplikacji usługi Azure AD są wymienione poniżej.

-   **Azure AD rejestracji jednokrotnej wyłączone** — Wybieranie usługi Azure AD rejestracji jednokrotnej wyłączone **tryb rejestracji jednokrotnej** Jeśli nie jest jeszcze gotowa do integracji aplikacji z rejestracji jednokrotnej z usługą Azure AD, lub po prostu testowania go

-   **Połączone logowania jednokrotnego** — wybierz [połączonej logowania jednokrotnego](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **tryb rejestracji jednokrotnej** Jeśli masz aplikację, która jest już połączona z istniejącym pojedynczego logowania jednokrotnego rozwiązaniem lub jeśli chcesz, aby Publikowanie proste łącze dla użytkowników w ich [panelu dostępu aplikacji](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) lub [uruchamiający aplikację usługi Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Na podstawie hasła logowania jednokrotnego** — wybierz [opartego na hasłach logowania jednokrotnego](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **tryb rejestracji jednokrotnej** Jeśli aplikacja renderuje nazwy użytkownika i hasła pola HTML i chcesz przechowywać tej nazwy użytkownika i hasło, aby bezpiecznie odtworzone później w aplikacji

-   **Na podstawie SAML logowania jednokrotnego** — wybierz [na języku SAML logowania jednokrotnego](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) jednokrotnego tryb, jeśli aplikacja obsługuje protokoły SAML lub OpenID Connect lub chcesz mieć możliwość mapowanie użytkowników do ról aplikacji określonym na podstawie reguł Definiowanie w Twojej oświadczeń SAML *(**Uwaga:** ta opcja nie jest dostępna, gdy serwer proxy aplikacji jest skonfigurowana dla aplikacji) *

-   **Na podstawie nagłówka logowania jednokrotnego** — wybierz tę opcję, [na podstawie nagłówka logowania jednokrotnego](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) logowania jednokrotnego w tryb jednego Jeśli masz aplikację przy użyciu PingAccess, która obsługuje nagłówka HTTP na podstawie uwierzytelniania, który chcesz wykonać jednokrotnego do *(**Uwaga:** ta opcja jest dostępna tylko, gdy serwer proxy aplikacji i PingAccess jest skonfigurowana dla aplikacji) *

-   **Zintegrowane uwierzytelnianie systemu Windows** — wybierz [zintegrowane uwierzytelnianie systemu Windows](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) jednokrotnego tryb podczas udostępnianie chcesz wykonać jednokrotnego do aplikacji WIA lokalnymi *(* *Uwaga:** ta opcja jest dostępna tylko wtedy, gdy serwer proxy aplikacji jest skonfigurowana dla aplikacji) *

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Tryby pojedynczego logowania jednokrotnego dla aplikacji utworzonych niestandardowych

Aplikacje mają niestandardowe opracowane przez [opracowany niestandardowych aplikacji](#_Custom-Developed_Applications) środowisko obsługuje również dodatkowe pojedynczego logowania jednokrotnego tryby nie są wymienione powyżej. Należą do nich:

-   [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) logowania na podstawie

-   [OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) logowania na podstawie

-   [WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) logowania na podstawie

-   [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) logowania na podstawie

Odczyt [przewodnik dewelopera usługi Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) Aby dowiedzieć się więcej o sposobie tworzenia aplikacji utworzonych niestandardowych, obsługujący pojedynczego logowania jednokrotnego trybów.

## <a name="how-to-set-an-applications-single-sign-on-mode"></a>Jak ustawić aplikacji logowania jednokrotnego w tryb jednego

Aby ustawić aplikacji **logowanie jednokrotne** tryb, postępuj zgodnie z instrukcjami poniżej:

1.  Otwórz [ **Azure Portal** ](https://portal.azure.com/) i zaloguj się jako **administratora globalnego** lub **ko-administratora.**

2.  Otwórz **rozszerzenia usług Azure Active Directory** klikając **więcej usług** u dołu menu nawigacji głównego po lewej stronie.

3.  Wpisz w **"Azure Active Directory**" w polu wyszukiwania filtr a wybierz **usługi Azure Active Directory** elementu.

4.  Kliknij przycisk **aplikacje dla przedsiębiorstw** w menu nawigacji po lewej stronie usługi Azure Active Directory.

5.  Kliknij przycisk **wszystkie aplikacje** Aby wyświetlić listę wszystkich aplikacji.

   * Jeśli nie ma aplikacji ma tutaj będą wyświetlane, użyj **filtru** kontroli nad **listę wszystkich aplikacji** i ustaw **Pokaż** opcji w celu **wszystkich aplikacji.**

6.  Wybierz aplikację, aby skonfigurować logowanie jednokrotne

7.  Po załadowaniu aplikacji, kliknij przycisk **logowanie jednokrotne** z menu nawigacji po lewej stronie aplikacji.

## <a name="next-steps"></a>Następne kroki
[Podaj logowanie jednokrotne do aplikacji przy użyciu serwera Proxy aplikacji](active-directory-application-proxy-sso-using-kcd.md)

