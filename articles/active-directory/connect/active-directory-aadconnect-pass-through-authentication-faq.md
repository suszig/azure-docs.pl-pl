---
title: "Azure AD Connect: Przekazywanego uwierzytelniania — często zadawane pytania | Dokumentacja firmy Microsoft"
description: "Odpowiedzi na często zadawane pytania dotyczące uwierzytelniania przekazywanego Azure Active Directory."
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
ms.openlocfilehash: e1bd58797124210f7c31e90fb20d728289a04ba2
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>Azure Active Directory przekazywanego uwierzytelniania: Często zadawane pytania

W tym artykule można rozwiązać często zadawane pytania dotyczące usługi Azure Active Directory (Azure AD) przekazywanego uwierzytelniania. Sprawdzanie wstecz dla nowej zawartości.

## <a name="which-of-the-azure-ad-sign-in-methods---pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs---should-i-choose"></a>Które z usługi Azure AD logowania metod - przekazywanego uwierzytelniania, synchronizacji skrótów haseł i Active Directory Federation Services (AD FS) — należy wybrać?

Go zależy od lokalnego środowiska i wymagań organizacyjnych. Ten artykuł, aby przejrzeć [porównanie różnych metod usługi Azure AD logowania](active-directory-aadconnect-user-signin.md).

## <a name="is-pass-through-authentication-a-free-feature"></a>Jest bezpłatne funkcji uwierzytelniania przekazywanego?

Przekazywanego uwierzytelniania jest funkcją wolnego i nie wymagają żadnych wersji płatnej usługi Azure AD z niego korzystać.

## <a name="is-pass-through-authentication-available-in-microsoft-cloud-germanyhttpwwwmicrosoftdecloud-deutschland-and-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Jest dostępna w uwierzytelniania przekazywanego [Niemcy Microsoft Cloud](http://www.microsoft.de/cloud-deutschland) i [Microsoft Azure dla instytucji rządowych Cloud](https://azure.microsoft.com/features/gov/)?

Nie, przekazywanego uwierzytelniania jest dostępna tylko w wystąpieniu na całym świecie usługi Azure AD.

## <a name="does-conditional-accessactive-directory-conditional-accessmd-work-with-pass-through-authentication"></a>Jest [dostępu warunkowego](../active-directory-conditional-access.md) pracy przy użyciu uwierzytelniania przekazywanego?

Tak, wszystkie możliwości dostępu warunkowego, w tym Azure Multi-Factor Authentication działa przy użyciu uwierzytelniania przekazywanego.

## <a name="does-pass-through-authentication-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Uwierzytelniania przekazywanego obsługuje "Alternatywnego Identyfikatora" jako nazwy użytkownika, a nie "userPrincipalName"?

Tak. Obsługuje uwierzytelniania przekazywanego `Alternate ID` jako nazwy użytkownika, gdy skonfigurowane w programie Azure AD Connect, jak pokazano [tutaj](active-directory-aadconnect-get-started-custom.md). Nie wszystkie aplikacje usługi Office 365 obsługują `Alternate ID`. Zajrzyj do dokumentacji aplikacji określonych w instrukcji obsługi.

## <a name="does-password-hash-synchronization-act-as-a-fallback-to-pass-through-authentication"></a>Synchronizacja skrótów haseł działać jako powrotu do uwierzytelniania przekazywanego?

Nie, uwierzytelniania przekazywanego _nie_ automatycznie w tryb failover synchronizacji skrótu hasła. Działa tylko jako rezerwowe dla [scenariusze uwierzytelniania przekazywanego nie obsługuje obecnie](active-directory-aadconnect-pass-through-authentication-current-limitations.md#unsupported-scenarios). Aby uniknąć błędów logowania użytkownika, należy skonfigurować uwierzytelniania przekazywanego [wysokiej dostępności](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="can-i-install-an-azure-ad-application-proxyactive-directory-application-proxy-get-startedmd-connector-on-the-same-server-as-a-pass-through-authentication-agent"></a>Można zainstalować [serwera Proxy aplikacji usługi Azure AD](../active-directory-application-proxy-get-started.md) łącznika na tym samym serwerze, ponieważ Agent uwierzytelniania przekazywanego?

Tak, ta konfiguracja jest obsługiwana w wersjach rebranded Agent uwierzytelniania przekazywanego (wersje 1.5.193.0 lub nowszej).

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Które wersje programu Azure AD Connect i Agent uwierzytelniania przekazywanego potrzebujesz?

Wymagana wersja 1.1.486.0 później w celu Azure AD Connect i 1.5.58.0 lub nowszym w przypadku Agent uwierzytelniania przekazywanego dla tej funkcji do pracy. Całe oprogramowanie należy zainstalować na serwerach z systemem Windows Server 2012 R2 lub nowszym.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-using-pass-through-authentication"></a>Co się stanie, jeśli upłynął hasło tego użytkownika i spróbuj zalogować się przy użyciu przekazywanego uwierzytelniania?

Jeśli skonfigurowano [funkcji zapisywania zwrotnego haseł](../active-directory-passwords-update-your-own-password.md) dla określonego użytkownika, a jeśli użytkownik loguje się przy użyciu przekazywanego uwierzytelniania, można zmienić lub resetowanie haseł. Hasła są zapisywane z powrotem do lokalnej usługi Active Directory zgodnie z oczekiwaniami.

Jednak jeśli nie skonfigurowano funkcję zapisywania zwrotnego haseł dla określonego użytkownika lub jeśli użytkownik nie ma prawidłową usługi Azure AD licencją, użytkownik nie może zaktualizować swoje hasło w chmurze. Nie można ich zaktualizowania hasła, nawet jeśli ich hasło wygasło. Użytkownik widzi zamiast tego komunikatu: "organizacji nie zezwala na zaktualizuj hasło w tej witrynie. Zaktualizuj go zgodnie z metodą zalecaną przez organizację lub poproś administratora w celu uzyskania pomocy." Użytkownik lub administrator ma do zresetowania swojego hasła w usłudze Active Directory lokalnymi.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Sposób uwierzytelniania przekazywanego chroni jednak przed atakami siłowymi hasło?

Odczyt [w tym artykule](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) Aby uzyskać więcej informacji.

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>Co agentów uwierzytelniania przekazywanego komunikacji za pośrednictwem portów 80 i 443?

- Agenci uwierzytelniania tworzą żądania HTTPS przez port 443 dla wszystkich operacji funkcji.
- Agenci uwierzytelniania należy żądań HTTP za pośrednictwem portu 80 do pobrania listy odwołania certyfikatów SSL.

     >[!NOTE]
     >Firma Microsoft zmniejszeniu liczby portów wymaganych przez funkcję najnowsze aktualizacje. Jeśli masz starsze wersje usługi Azure AD Connect lub Agent uwierzytelniania, nie zamykaj tych portów również: 5671, 8080, 9090, 9091, 9350, 9352 i 10100 10120.

## <a name="can-the-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Można agentów uwierzytelniania przekazywanego komunikują się za pośrednictwem serwera proxy ruchu wychodzącego sieci web?

Tak. WPAD (Web autowykrywania serwera Proxy) jest włączona w środowisku lokalnym, agenci uwierzytelniania automatycznie próbę zlokalizuj i używać serwera proxy sieci web w sieci.

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-the-same-server"></a>Na tym samym serwerze można zainstalować dwóch lub więcej agentów uwierzytelniania przekazywanego?

Nie, tylko można zainstalować jeden Agent uwierzytelniania przekazywanego na jednym serwerze. Jeśli chcesz skonfigurować uwierzytelniania przekazywanego wysokiej dostępności, postępuj zgodnie z instrukcjami to [artykułu](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability) zamiast tego.

## <a name="i-already-use-active-directory-federation-services-ad-fs-for-azure-ad-sign-in-how-do-i-switch-it-to-pass-through-authentication"></a>Już używać usług federacyjnych Active Directory (AD FS) do logowania w usłudze Azure AD. Jak przełączać jej do uwierzytelniania przekazywanego?

Jeśli usługi AD FS zostały skonfigurowane jako metodę logowania za pomocą Kreatora programu Azure AD Connect, zmień metody logowania użytkownika do uwierzytelniania przekazywanego. Ta zmiana umożliwia użycie uwierzytelniania przekazywanego dla dzierżawcy i konwertuje _wszystkie_ Sfederowanych domen w domenach zarządzanych. Wszystkie kolejne żądania rejestracji w dzierżawie są obsługiwane przez uwierzytelniania przekazywanego. Obecnie nie istnieje sposób obsługiwane w ramach usługi Azure AD Connect być zastosowanie kombinacji uwierzytelniania przekazywanego i usług AD FS w różnych domenach.

Jeśli usługi AD FS został skonfigurowany jako metodę logowania _poza_ Kreator Azure AD Connect zmienić metody logowania użytkownika do uwierzytelniania przekazywanego (z opcja "Nie należy konfigurować"). Ta zmiana umożliwia przekazywanego uwierzytelniania w ramach dzierżawy. Jednak wszystkie domeny federacyjnym w dalszym ciągu korzystać z usług AD FS dla logowania. Ręcznie konwertowanie niektórych lub wszystkich tych domen Sfederowanych domen zarządzanych za pomocą programu PowerShell. Po utworzeniu tego żądania wszystkich logowanie w domenach zarządzanych (_tylko_) są obsługiwane przez funkcję uwierzytelniania przekazywanego.

>[!IMPORTANT]
>Uwierzytelniania przekazywanego nie obsługuje logowania tylko w chmurze Azure AD users.

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-ad-environment"></a>W środowisku wielu lasów usługi AD można używać uwierzytelniania przekazywanego?

Tak. Środowiska z wieloma lasami są obsługiwane, jeśli istnieją relacje zaufania lasu między z lasów usługi AD i Jeśli routing sufiksów nazw jest poprawnie skonfigurowany.

## <a name="how-many-pass-through-authentication-agents-do-i-need-to-install"></a>Ile agentów uwierzytelniania przekazywanego należy zainstalować?

Instalowanie wielu agentów uwierzytelniania przekazywanego zapewnia [wysokiej dostępności](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability). Jednak nie zapewnia ona deterministyczna zrównoważenia obciążenia między agentami uwierzytelniania.

Należy wziąć pod uwagę maksymalne i średnie obciążenie żądań logowania, które powinny być widoczne w dzierżawie. Jako wzorca jednego agenta uwierzytelniania może obsługiwać 300 000 400 000 uwierzytelnienia na sekundę na standardowe 4 rdzenie procesora CPU, serwer 16 GB pamięci RAM. Dla większości klientów dwóch lub trzech agentów uwierzytelniania łącznie są wystarczające dla wysokiej dostępności i wydajności.

Zalecamy zainstalowanie agentów uwierzytelniania bliski kontrolerów domeny do poprawy logowania opóźnienia.

## <a name="can-i-install-the-first-pass-through-authentication-agent-on-a-server-other-than-the-one-that-runs-azure-ad-connect"></a>Pierwszy agenta uwierzytelniania przekazywanego można zainstalować na serwerze innym niż ten, który uruchamia Azure AD Connect?

Nie, ten scenariusz jest _nie_ obsługiwane.

## <a name="how-many-pass-through-authentication-agents-should-i-install"></a>Ile agentów uwierzytelniania przekazywanego należy zainstalować?

Zalecamy, aby:

- Całkowita liczba należy zainstalować dwóch lub trzech agentów uwierzytelniania. Ta konfiguracja jest odpowiednia dla większości klientów.
- Zainstaluj agentów uwierzytelniania na kontrolerach domeny (lub w pobliżu je jak to możliwe) zwiększające logowania opóźnienia.
- Upewnij się, że serwery (w których zostali zainstalowani agenci uwierzytelniania) są dodawane do tego samego lasu usługi AD jako użytkowników, których hasła muszą być weryfikowane.

>[!NOTE]
>Brak limitu systemowego 12 agentów uwierzytelniania dla każdego dzierżawcy.

## <a name="how-can-i-disable-pass-through-authentication"></a>Jak wyłączyć uwierzytelniania przekazywanego?

Uruchom ponownie Kreatora programu Azure AD Connect i zmień metodę logowania użytkownika z uwierzytelniania przekazywanego do innej metody. Ta zmiana powoduje wyłączenie uwierzytelniania przekazywanego dla dzierżawcy i odinstalowuje agenta uwierzytelniania z serwera. Należy ręcznie odinstalować agentów uwierzytelniania z innych serwerów.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>Co się stanie, gdy odinstalować agenta uwierzytelniania przekazywanego?

Dezinstalacja agenta uwierzytelniania przekazywanego z serwera powoduje zatrzymanie, akceptując żądania logowania. Upewnij się, że masz innego uwierzytelniania agenta uruchomionego przed wykonaniem tej operacji, aby uniknąć dzielenia logowanie użytkowników w dzierżawie.

## <a name="next-steps"></a>Następne kroki
- [**Bieżące ograniczenia** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) — Dowiedz się, jakie scenariusze są obsługiwane i zostały.
- [**Szybki Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) — Uzyskaj i systemem Azure AD przekazywanego uwierzytelniania.
- [**Inteligentne blokady** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) — funkcja Konfigurowanie blokady inteligentnej na dzierżawy do ochrony kont użytkowników.
- [**Nowości techniczne** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -zrozumieć sposób działania tej funkcji.
- [**Rozwiązywanie problemów z** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) — Dowiedz się, jak rozwiązać typowe problemy z funkcją.
- [**Nowości zabezpieczeń** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -uzyskać dodatkowe informacje techniczne głębokość tę funkcję.
- [**Azure AD SSO bezproblemowe** ](active-directory-aadconnect-sso.md) — Dowiedz się więcej o tej funkcji uzupełniające.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) — w przypadku zgłoszenia żądania nowych funkcji.
