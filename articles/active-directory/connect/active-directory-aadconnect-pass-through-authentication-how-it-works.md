---
title: "Azure AD Connect: Uwierzytelniania przekazywanego — jak to działa? | Microsoft Docs"
description: "W tym artykule opisano, jak działa uwierzytelniania przekazywanego Azure Active Directory."
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
ms.openlocfilehash: c7863e38671349b6424ee08330da8aaa49cb2a70
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Azure przekazywanego uwierzytelnianie usługi Active Directory: Techniczne nowości
Następujący artykuł jest omówienie działania usługi Azure AD przekazywanego uwierzytelniania.  Bezpośrednie technicznych i informacji o zabezpieczeniach dla [ **nowości zabezpieczeń** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) artykułu.

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Jak działa uwierzytelniania przekazywanego Azure Active Directory?

Gdy użytkownik próbuje zalogować się do aplikacji zabezpieczonej przez usługi Azure Active Directory (Azure AD) i przekazywanego uwierzytelniania jest włączona w ramach dzierżawy, zostaną wykonane następujące kroki:

1. Użytkownik próbuje uzyskać dostęp do aplikacji (na przykład Outlook Web App - https://outlook.office365.com/owa/).
2. Jeśli użytkownik nie jest już zalogowany, użytkownik jest przekierowany do strony logowania usługi Azure AD.
3. Użytkownik wprowadza nazwę użytkownika i hasło do strony logowania usługi Azure AD i kliknie przycisk "Zaloguj".
4. Azure AD, po otrzymaniu żądania logowania umieszcza nazwy użytkownika i hasła (zaszyfrowane przy użyciu klucza publicznego) w kolejce.
5. Agent uwierzytelniania lokalnego pobiera nazwę użytkownika i hasło zaszyfrowanych z kolejki.
6. Agent odszyfrowuje hasła, używając swojego klucza prywatnego.
7. Agent następnie weryfikuje nazwy użytkownika i hasła w usłudze Active Directory przy użyciu standardowych interfejsów API systemu Windows (podobny mechanizm co to jest używany przez usługi federacyjne Active Directory). Nazwa użytkownika może być albo lokalnymi domyślna nazwa użytkownika (zwykle `userPrincipalName`) lub inny atrybut skonfigurowane w programie Azure AD Connect (nazywane `Alternate ID`).
8. Lokalnymi Active Directory domeny kontrolera (kontroler domeny), następnie ocenia żądanie i zwraca właściwą odpowiedź (Powodzenie, Niepowodzenie, hasło wygasło lub użytkownika zablokowane) do agenta.
9. Agent uwierzytelniania z kolei, zwraca ten odpowiedź z powrotem do usługi Azure AD.
10. Usługi Azure AD ocenia odpowiedzi i odpowiada użytkowników zależnie od potrzeb — na przykład jego loguje się użytkownik natychmiast albo żądań uwierzytelniania wieloskładnikowego (MFA).
11. Jeśli logowanie użytkowników zostanie nawiązane, użytkownik jest w stanie uzyskać dostęp do aplikacji.

Na poniższym diagramie przedstawiono wszystkie składniki i kroki do wykonania.

![Uwierzytelnianie przekazywane](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Następne kroki
- [**Bieżące ograniczenia** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) — Dowiedz się, jakie scenariusze są obsługiwane i zostały.
- [**Szybki Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) — Uzyskaj i systemem Azure AD przekazywanego uwierzytelniania.
- [**Inteligentne blokady** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) — funkcja Konfigurowanie blokady inteligentnej na dzierżawy do ochrony kont użytkowników.
- [**Często zadawane pytania** ](active-directory-aadconnect-pass-through-authentication-faq.md) — odpowiedzi na często zadawane pytania.
- [**Rozwiązywanie problemów z** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) — Dowiedz się, jak rozwiązać typowe problemy z funkcją.
- [**Nowości zabezpieczeń** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -uzyskać dodatkowe informacje techniczne głębokość tę funkcję.
- [**Azure AD SSO bezproblemowe** ](active-directory-aadconnect-sso.md) — Dowiedz się więcej o tej funkcji uzupełniające.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) — w przypadku zgłoszenia żądania nowych funkcji.
