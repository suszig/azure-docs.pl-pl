---
title: "Problem podczas instalowania aplikacji dostępu panelu rozszerzenia przeglądarki | Dokumentacja firmy Microsoft"
description: "Jak rozwiązywać typowe błędy podczas instalowania rozszerzenia przeglądarki panelu dostępu"
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
ms.date: 01/15/2018
ms.author: asteen
ms.reviewer: japere
ms.openlocfilehash: 26dc5d5ffce84206450123132c0633c2aa323e9f
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/06/2018
---
# <a name="problem-installing-the-application-access-panel-browser-extension"></a>Problem podczas instalowania aplikacji dostępu panelu rozszerzenia przeglądarki

Panel dostępu jest widok, a następnie uruchom chmurowych aplikacji, które administrator usługi Azure AD udzielił im dostępu do portalu sieci web, dzięki czemu użytkownik, który ma konto służbowe w usłudze Azure Active Directory (Azure AD). Użytkownik, który ma wersje usługi Azure AD umożliwia także grupami samoobsługi i funkcje zarządzania aplikacjami za pomocą panelu dostępu. Panel dostępu jest oddzielony od portalu Azure i nie wymaga użytkownikom posiadania subskrypcji platformy Azure.

Aby użyć opartego na hasłach rejestracji jednokrotnej (SSO) w panelu dostępu, rozszerzenia Panelu dostępu musi być zainstalowany w przeglądarce. To rozszerzenie jest pobierany automatycznie, gdy użytkownik wybierze aplikacji, która jest skonfigurowana do opartego na hasłach logowania jednokrotnego.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Spełniające wymagania przeglądarki do panelu dostępu

Panel dostępu wymaga przeglądarki obsługującej JavaScript i włączył CSS. Aby użyć opartego na hasłach rejestracji jednokrotnej (SSO) w panelu dostępu, rozszerzenia Panelu dostępu musi być zainstalowany w przeglądarce. To rozszerzenie jest pobierany automatycznie, gdy użytkownik wybierze aplikacji, która jest skonfigurowana do opartego na hasłach logowania jednokrotnego.

Logowanie Jednokrotne opartego na hasłach można przeglądarki przez użytkownika końcowego:

-   Krawędź w systemie Windows 10 Anniversary Edition lub nowszy 

-   Chrome — W systemie Windows 7 lub nowszy oraz System MacOS x lub nowszych

-   Firefox 26.0 lub później — w systemie Windows XP z dodatkiem SP2 lub nowszy oraz w systemie Mac OS X 10,6 lub nowszy

-   Internet Explorer 8, 9, 10, 11 — w systemie Windows 7 lub nowszy (obsługa ograniczona)
## <a name="how-to-install-the-access-panel-browser-extension"></a>Jak zainstalować rozszerzenie przeglądarki panelu dostępu

Aby zainstalować rozszerzenie przeglądarki panelu dostępu, wykonaj następujące czynności:

1.  Otwórz [panelu dostępu](https://myapps.microsoft.com) w jednym z obsługiwanych przeglądarkach i zaloguj się jako **użytkownika** w usługi Azure AD.

2.  Kliknij przycisk **logowania jednokrotnego hasła aplikacji** w panelu dostępu.

3.  W oknie komunikatu z pytaniem do zainstalowania oprogramowania, zaznaczyć **Zainstaluj teraz**.

4.  Oparte na przeglądarce być kierowane do łącza pobierania. **Dodaj** rozszerzenia do przeglądarki.

5.  Jeśli przeglądarka pytanie, wybierz albo **włączyć** lub **Zezwalaj** rozszerzenia.

6.  Wcześniej zainstalowano **ponowne uruchomienie** sesji przeglądarki.

7.  Zaloguj się do panelu dostępu i zobacz, czy można **uruchamianie** logowania jednokrotnego hasła aplikacji

Może również pobrać rozszerzenia dla programu Chrome i krawędzi z bezpośrednich łączy poniżej:

-   [Rozszerzenie panelu dostępu Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Rozszerzenie panelu dostępu krawędzi](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

## <a name="how-do-i-use-the-my-apps-secure-sign-in-extension"></a>Jak używać Moje aplikacje bezpiecznego logowania rozszerzenie?
Zmiana domyślnego adresu URL aplikacji Moje rozszerzenia

Jeśli używasz różnych Moje aplikacje adresu URL niż https://myapps.microsoft.com następnie należy skonfigurować adres URL domyślnej jednak następujące czynności:
1. Nie jest zalogowany do rozszerzenia, **kliknij prawym przyciskiem myszy** ikona rozszerzenia.
2. Polecenie **wybierz adres URL Moje aplikacje** z menu.
3. **Wybierz** Twojego domyślny adres URL.
4. Kliknij ikonę rozszerzenia.
5. Zaloguj się do rozszerzenia, wybierając **Zaloguj się rozpocząć**.

Zaloguj się bezpośrednio do aplikacji z przeglądarki
1. Po zainstalowaniu rozszerzenia, zaloguj się do rozszerzenia, wybierając **Zaloguj się rozpocząć**.
2. Przejdź do **adres URL logowania na** aplikacji, które chcesz logować się do, zazwyczaj jest to adres URL aplikacji, która wyświetla formularz logowania.
3. Rozszerzenia należy zmiany stanu i umożliwiają znać hasło jest dostępna, kliknij pozycję **ikona rozszerzenia** do logowania.

Uruchom aplikację z rozszerzenia
1. Po zainstalowaniu rozszerzenia, zaloguj się do rozszerzenia, wybierając **Zaloguj się rozpocząć**.
2. Kliknij ikonę rozszerzenia, aby otworzyć jego **menu**.
3. **Wyszukiwanie** dla aplikacji, które są dostępne w portalu Moje aplikacje.
4. Kliknij aplikację z **wyniki wyszukiwania** można go uruchomić.
5. Ostatnie trzy aplikacje uruchomione będą również widoczne w **ostatnio używane** listy skrótów

> [!NOTE]
> Te opcje są dostępne tylko na krawędzi, Chrome, Firefox.

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Konfigurowanie zasad grupy dla programu Internet Explorer

Możesz skonfigurować zasady grupy, które umożliwiają zdalnie zainstalować rozszerzenie Panel dostępu dla programu Internet Explorer na komputerach użytkowników.

Wymagania wstępne należą:

-   Po skonfigurowaniu [usług domenowych w usłudze Active Directory](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), i dołączeniu komputerów użytkowników do domeny.

-   Musi mieć uprawnienie "Edytuj ustawienia" do edycji obiektu zasad grupy (GPO). Domyślnie to uprawnienie mają członków z następujących grup zabezpieczeń: Administratorzy domeny, Administratorzy przedsiębiorstwa oraz Twórcy-właściciele zasad grupy. [Dowiedz się więcej](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Czynności opisane w samouczku [wdrażanie rozszerzenia Panel dostępu dla programu Internet Explorer przy użyciu zasad grupy](active-directory-saas-ie-group-policy.md) instrukcje krok po kroku dotyczące sposobu konfigurowania zasad grupy oraz wdrażanie dla użytkowników.

## <a name="troubleshoot-the-access-panel-extension-in-internet-explorer"></a>Rozwiązywanie problemów z rozszerzeniem panelu dostępu w programie Internet Explorer

Postępuj zgodnie z [Rozwiązywanie problemów z rozszerzeniem Panel dostępu dla programu Internet Explorer](active-directory-saas-ie-troubleshooting.md) przewodnik dostępu narzędzia diagnostycznego i instrukcje krok po kroku dotyczące konfigurowania rozszerzenia dla programu Internet Explorer.

> [!NOTE]
> Programu Internet Explorer znajduje się na ograniczony pomocy technicznej i nie będzie już otrzymywać nowe aktualizacje oprogramowania. Krawędź jest zalecane przeglądarki.

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a>Jeśli te kroki rozwiązywania problemów nie rozwiąże problemu

Otwórz bilet pomocy technicznej następujące informacje, jeśli są dostępne:

-   Identyfikator błędu korelacji

-   Nazwa UPN (adres e-mail użytkownika)

-   Dla identyfikatora dzierżawcy

-   Typ przeglądarki

-   Strefa czasowa i przedziału czasu/czasu podczas błąd występuje

-   Ślady fiddler

## <a name="next-steps"></a>Kolejne kroki
[Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)
