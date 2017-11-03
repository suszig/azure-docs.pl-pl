---
title: "Azure AD przekazywanego uwierzytelniania — Szybki Start | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak rozpocząć pracę z usługi Azure Active Directory (Azure AD) przekazywanego uwierzytelniania."
services: active-directory
keywords: "Azure AD Connect przekazywanego uwierzytelniania, instalacji usługi Active Directory, wymaganych składników dla usługi Azure AD, SSO, Single Sign-on"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: cbedb87722d1c230f3b8003cadd069947881f25d
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Usługi Azure Active Directory przekazywanego uwierzytelniania: Szybki start

## <a name="how-to-deploy-azure-ad-pass-through-authentication"></a>Wdrażanie usługi Azure AD przekazywanego uwierzytelniania

Azure Active Directory (Azure AD) przekazywanego uwierzytelniania umożliwia użytkownikom logowanie się zarówno lokalnie, jak i aplikacji działających w chmurze przy użyciu takich samych haseł w. Go loguje się użytkowników weryfikując haseł bezpośrednio z lokalnej usługi Active Directory.

>[!IMPORTANT]
>Jeśli z tej funkcji za pomocą podglądu, należy upewnić się, uaktualnienie wersji zapoznawczych agentów uwierzytelniania przy użyciu instrukcji [tutaj](./active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md).

Należy wykonać te instrukcje, aby wdrożyć przekazywanego uwierzytelniania:

## <a name="step-1-check-prerequisites"></a>Krok 1: Sprawdź wymagania wstępne

Upewnij się, że zostały spełnione następujące wymagania wstępne:

### <a name="on-the-azure-active-directory-admin-center"></a>W Centrum administracyjnym usługi Azure Active Directory

1. Utwórz konto administratora globalnego tylko w chmurze w dzierżawie usługi Azure AD. W ten sposób można zarządzać konfiguracją dzierżawy powinna zakończyć się niepowodzeniem z lokalnymi usługami lub staną się niedostępne. Dowiedz się więcej o [dodanie konta administratora globalnego tylko w chmurze](../active-directory-users-create-azure-portal.md). Wykonanie tego kroku jest szczególnie ważne, aby upewnić się, że możesz przed zablokowaniem dzierżawy.
2. Dodaj jeden lub kilka [nazwy domeny niestandardowej](../active-directory-add-domain.md) do dzierżawy usługi Azure AD. Użytkownicy logują się przy użyciu jednej z tych domen.

### <a name="in-your-on-premises-environment"></a>W środowisku lokalnym

1. Zidentyfikuj serwer z systemem Windows Server 2012 R2 lub nowszym, na którym ma być uruchamiany Azure AD Connect. Dodaj serwer do tego samego lasu usługi AD jako użytkowników, których hasła muszą być weryfikowane.
2. Zainstaluj [najnowszą wersję programu Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) na serwerze, który został zidentyfikowany w poprzednim kroku. Jeśli masz już uruchomiona usługa Azure AD Connect, upewnij się, że wersja zawiera się 1.1.644.0 lub nowszym.

    >[!NOTE]
    >Azure AD Connect wersji 1.1.557.0, 1.1.558.0, 1.1.561.0 i 1.1.614.0 ma problem związany z **synchronizacji skrótu hasła**. Jeśli użytkownik _nie_ ma być używany w połączeniu z uwierzytelniania przekazywanego, przeczytaj synchronizacji skrótu hasła [wersji Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470) Aby dowiedzieć się więcej.

3. Zidentyfikuj dodatkowy serwer z systemem Windows Server 2012 R2 lub nowszym, na której ma zostać uruchomiony autonomicznie Agent uwierzytelniania. Wersja agenta uwierzytelniania musi być 1.5.193.0 lub nowszym. Ten serwer jest potrzebne do zapewnienia wysokiej dostępności żądań logowania. Dodaj serwer do tego samego lasu usługi AD jako użytkowników, których hasła muszą być weryfikowane.
4. W przypadku zapory między serwerami a usługą Azure AD, należy skonfigurować następujące elementy:
   - Upewnij się, czy agenci uwierzytelniania można wprowadzać **wychodzących** żądań do usługi Azure AD przez następujące porty:
   
   | Numer portu | Jak jest używany |
   | --- | --- |
   | **80** | Pobieranie odwołania certyfikatu list podczas sprawdzania poprawności certyfikatu SSL |
   | **443** | Cała komunikacja wychodząca naszej usługi |
   
   Jeśli Zapora wymusza zasady odpowiednio dla użytkowników generujących, należy otworzyć te porty dla ruchu z usług systemu Windows, które działają jako usługa sieciowa.
   - Jeśli zapora lub serwer proxy zezwala na listę dozwolonych podobnej DNS, połączeń dozwolonych  **\*. msappproxy.net** i  **\*. servicebus.windows.net**. Jeśli nie, Zezwalaj na dostęp do [zakresy IP centrum danych Azure](https://www.microsoft.com/download/details.aspx?id=41653), która jest aktualizowana co tydzień.
   - Agentów uwierzytelniania wymagany jest dostęp do **login.windows.net** i **login.microsoftonline.com** do pierwszej rejestracji dla tych adresów URL również Otwórz zaporę.
   - Do weryfikacji certyfikatu odblokować następujące adresy URL: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80** i **www.microsoft.com:80**. Te adresy URL są używane do weryfikacji certyfikatu z innymi produktami firmy Microsoft, może już istnieć tych adresów URL odblokowany.

## <a name="step-2-enable-exchange-activesync-support-optional"></a>Krok 2: Włącz obsługę protokołu Exchange ActiveSync (opcjonalnie)

Wykonaj te instrukcje, aby włączyć obsługę programu Exchange ActiveSync:

1. Użyj [środowiska PowerShell usługi Exchange](https://technet.microsoft.com/library/mt587043(v=exchg.150).aspx) do uruchom następujące polecenie:
```
Get-OrganizationConfig | fl per*
```

2. Sprawdź wartość `PerTenantSwitchToESTSEnabled` ustawienie. Jeśli wartość jest **true**, dzierżawy jest prawidłowo skonfigurowana — zwykle jest to w przypadku większości klientów. Jeśli wartość jest **false**, uruchom następujące polecenie:
```
Set-OrganizationConfig -PerTenantSwitchToESTSEnabled:$true
```

3. Sprawdź, czy wartość `PerTenantSwitchToESTSEnabled` teraz mają ustawioną wartość **true**. Zaczekaj godzinę przed przejściem do następnego kroku.

Jeśli w tym kroku stają przed wszelkie problemy, sprawdź naszych [przewodnik rozwiązywania problemów](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#exchange-activesync-configuration-issues) Aby uzyskać więcej informacji.

## <a name="step-3-enable-the-feature"></a>Krok 3: Włącz tę funkcję

Uwierzytelniania przekazywanego można włączyć za pomocą [Azure AD Connect](active-directory-aadconnect.md).

>[!IMPORTANT]
>Na serwerze podstawowym lub tymczasowej Azure AD Connect można włączyć uwierzytelniania przekazywanego. Zdecydowanie zaleca się włączyć z poziomu serwera podstawowego.

Jeśli instalujesz program Azure AD Connect po raz pierwszy, wybierz [niestandardową ścieżkę](active-directory-aadconnect-get-started-custom.md). W **logowania użytkownika** wybierz pozycję **uwierzytelniania przekazywanego** jako znak dla metody. Po pomyślnym ukończeniu agenta przekazywania uwierzytelniania jest zainstalowany na tym samym serwerze co usługi Azure AD Connect. Ponadto funkcja przekazywanego uwierzytelniania jest włączona w dzierżawie.

![Azure AD Connect - logowanie użytkowników](./media/active-directory-aadconnect-sso/sso3.png)

Jeśli użytkownik zainstalował już Azure AD Connect (przy użyciu [instalacji ekspresowej](active-directory-aadconnect-get-started-express.md) lub [instalacji niestandardowej](active-directory-aadconnect-get-started-custom.md) ścieżki) wybierz pozycję **zmienić logowania użytkownika** zadania w usłudze Azure AD Połącz, a następnie kliknij pozycję **dalej**. Następnie wybierz **uwierzytelniania przekazywanego** jako znak dla metody. Po pomyślnym ukończeniu uwierzytelniania przekazywanego agent jest zainstalowany na tym samym serwerze co usługi Azure AD Connect, a ta funkcja jest włączona w dzierżawie.

![Azure AD Connect - zmiany logowania użytkownika](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>Przekazywanego uwierzytelniania jest funkcją poziomie dzierżawy. Włączenie ma wpływ na logowanie użytkowników między _wszystkie_ domen zarządzanych w Twojej dzierżawie. Aby przełączyć się z usługami AD FS do uwierzytelniania przekazywanego, firma Microsoft zaleca, aby poczekać co najmniej 12 godzin przed zamknięciem infrastruktury usług AD FS — czas oczekiwania jest, aby upewnić się, że użytkownicy mogą zachować zalogowanie się do programu Exchange ActiveSync podczas przejścia.

## <a name="step-4-test-the-feature"></a>Krok 4: Testowanie funkcji

Wykonaj te instrukcje, aby sprawdzić prawidłowo włączone przekazywanego uwierzytelniania:

1. Zaloguj się do [Centrum administracyjnego usługi Azure Active Directory](https://aad.portal.azure.com) przy użyciu poświadczeń administratora globalnego dla dzierżawy.
2. Wybierz **usługi Azure Active Directory** w nawigacji po lewej stronie.
3. Wybierz **programu Azure AD Connect**.
4. Sprawdź, czy **uwierzytelniania przekazywanego** funkcji jest pokazywana jako **włączone**.
5. Wybierz **uwierzytelniania przekazywanego**. Ten blok zawiera listę serwerów, na których zainstalowano agentów uwierzytelniania.

![Centrum administracyjne usługi Azure Active Directory — blok Azure AD Connect](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Centrum administracyjne usługi Active Directory platformy Azure — blok przekazywanego uwierzytelniania](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

Na tym etapie użytkowników ze wszystkich domen zarządzanych w Twojej dzierżawie zalogować się przy użyciu uwierzytelniania przekazywanego. Jednak użytkownicy z domen federacyjnych nadal logować się za pomocą usługi Active Directory Federation Services (AD FS) lub innego dostawcy federacyjnego, który został wcześniej skonfigurowany. Po przekonwertowaniu do domeny z federacyjnym zarządzane, wszyscy użytkownicy z tej domeny automatyczne uruchamianie, logowanie przy użyciu uwierzytelniania przekazywanego. Funkcji uwierzytelniania przekazywanego nie wpływa na użytkowników tylko w chmurze.

## <a name="step-5-ensure-high-availability"></a>Krok 5: Zapewni to wysoką dostępność

Jeśli planujesz wdrożenie uwierzytelniania przekazywanego w środowisku produkcyjnym, należy zainstalować autonomiczny Agent uwierzytelniania. Zainstaluj agenta tego drugiego uwierzytelniania na serwerze _innych_ niż jednym systemem Azure AD Connect i agentem pierwszego uwierzytelniania. Ta konfiguracja zapewnia wysoką dostępność żądań logowania. Wykonaj te instrukcje dotyczące wdrażania autonomicznej Agent uwierzytelniania:

1. **Pobierz najnowszą wersję agenta uwierzytelniania (wersje 1.5.193.0 lub nowszym)**: Zaloguj się do [Centrum administracyjnego usługi Azure Active Directory](https://aad.portal.azure.com) przy użyciu poświadczeń administratora globalnego Twojej dzierżawy.
2. Wybierz **usługi Azure Active Directory** w nawigacji po lewej stronie.
3. Wybierz **programu Azure AD Connect** , a następnie **uwierzytelniania przekazywanego**. I wybierz **pobierania agenta**.
4. Kliknij przycisk **zaakceptować & pobieranie** przycisku.
5. **Zainstaluj najnowszą wersję agenta uwierzytelniania**: Uruchom plik wykonywalny pobrany w poprzednim kroku. Podaj poświadczenia administratora globalnego Twojej dzierżawy, po wyświetleniu monitu.

![Centrum administracyjne usługi Active Directory platformy Azure — przycisk Pobierz agenta uwierzytelniania](./media/active-directory-aadconnect-pass-through-authentication/pta9.png)

![Centrum administracyjne usługi Active Directory platformy Azure — Pobierz agenta bloku](./media/active-directory-aadconnect-pass-through-authentication/pta10.png)

>[!NOTE]
>Możesz również pobrać agenta uwierzytelniania z [tutaj](https://aka.ms/getauthagent). Sprawdź, czy Przejrzyj i zaakceptuj Agent uwierzytelniania [warunki świadczenia usługi](https://aka.ms/authagenteula) _przed_ instalacji.

## <a name="next-steps"></a>Następne kroki
- [**Inteligentne blokady** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) — funkcja Konfigurowanie blokady inteligentnej na dzierżawy do ochrony kont użytkowników.
- [**Bieżące ograniczenia** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) — Dowiedz się, jakie scenariusze są obsługiwane i zostały.
- [**Nowości techniczne** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -zrozumieć sposób działania tej funkcji.
- [**Często zadawane pytania** ](active-directory-aadconnect-pass-through-authentication-faq.md) — odpowiedzi na często zadawane pytania.
- [**Rozwiązywanie problemów z** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) — Dowiedz się, jak rozwiązać typowe problemy z funkcją.
- [**Nowości zabezpieczeń** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -uzyskać dodatkowe informacje techniczne głębokość tę funkcję.
- [**Azure AD SSO bezproblemowe** ](active-directory-aadconnect-sso.md) — Dowiedz się więcej o tej funkcji uzupełniające.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) — w przypadku zgłoszenia żądania nowych funkcji.
