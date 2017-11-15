---
title: "Azure AD Connect: Przekazywanego uwierzytelniania - bieżące ograniczenia | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano bieżące ograniczenia uwierzytelniania przekazywanego usługi Azure Active Directory (Azure AD)."
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
ms.openlocfilehash: 4a33df43ca218545d6c684103a64f2cd1460913b
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2017
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Usługi Azure Active Directory przekazywanego uwierzytelniania: Bieżące ograniczenia

>[!IMPORTANT]
>Azure AD przekazywanego uwierzytelniania jest funkcją wolne, i nie wymagają żadnych wersji płatnej usługi Azure AD z niego korzystać. Przekazywanego uwierzytelniania jest dostępna w wystąpieniu na całym świecie usługi Azure AD, a nie na tylko [Niemcy Microsoft Cloud](http://www.microsoft.de/cloud-deutschland) i [Microsoft Azure dla instytucji rządowych Cloud](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Obsługiwane scenariusze

Pełni są obsługiwane następujące scenariusze:

- Użytkownik logowania do wszystkich aplikacji opartej na przeglądarce sieci web.
- Logowania użytkownika do aplikacji klienta usługi Office 365, które obsługują [nowoczesnego uwierzytelniania](https://aka.ms/modernauthga) -pakiet Office 2016 i Office 2013 _z_ nowoczesnego uwierzytelniania.
- Azure AD Join dla urządzeń z systemem Windows 10.
- Obsługa programu Exchange ActiveSync.

## <a name="unsupported-scenarios"></a>Nieobsługiwane scenariusze

Poniższe scenariusze są _nie_ obsługiwane:

- Logowania użytkowników w starszych aplikacjach klienckich pakietu Office — pakiet Office 2010 i Office 2013 _bez_ nowoczesnego uwierzytelniania). Organizacje zaleca się przełączyć do nowoczesnego uwierzytelniania, jeśli to możliwe. Umożliwia obsługę uwierzytelniania przekazywanego nowoczesnego uwierzytelniania, ale również pomaga zabezpieczyć użytkownika kont przy użyciu narzędzia [dostępu warunkowego](../active-directory-conditional-access-azure-portal.md) funkcje takie jak uwierzytelnianie wieloskładnikowe (MFA).
- Użytkownik logowania do usługi Skype dla firm klienta aplikacji, w tym usługi Skype dla firm 2016.
- Użytkownik logowania do programu PowerShell w wersji 1.0. Zalecane jest, użyj programu PowerShell w wersji 2.0.
- Usługi domenowe Azure AD.
- Hasła aplikacji dla usługi MFA.
- Wykrywanie użytkownikom [ujawnione poświadczenia](../active-directory-reporting-risk-events.md#leaked-credentials).

>[!IMPORTANT]
>Jako obejścia nieobsługiwane scenariusze _tylko_, włączanie synchronizacji skrótów haseł na [funkcje opcjonalne](active-directory-aadconnect-get-started-custom.md#optional-features) strony kreatora programu Azure AD Connect. Włączanie synchronizacji skrótów haseł umożliwia także opcję Uwierzytelnianie trybu failover Jeśli infrastruktury lokalnej całkowicie jest zakłócona. Ten tryb failover z uwierzytelniania przekazywanego synchronizacji skrótu hasła nie jest automatyczne, ale ma wykonać za pomocą programu Microsoft Support.

## <a name="next-steps"></a>Następne kroki
- [**Szybki Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) — Uzyskaj i systemem Azure AD przekazywanego uwierzytelniania.
- [**Inteligentne blokady** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) — funkcja Konfigurowanie blokady inteligentnej na dzierżawy do ochrony kont użytkowników.
- [**Nowości techniczne** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -zrozumieć sposób działania tej funkcji.
- [**Często zadawane pytania** ](active-directory-aadconnect-pass-through-authentication-faq.md) — odpowiedzi na często zadawane pytania.
- [**Rozwiązywanie problemów z** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) — Dowiedz się, jak rozwiązać typowe problemy z funkcją.
- [**Nowości zabezpieczeń** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -uzyskać dodatkowe informacje techniczne głębokość tę funkcję.
- [**Azure AD SSO bezproblemowe** ](active-directory-aadconnect-sso.md) — Dowiedz się więcej o tej funkcji uzupełniające.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) — w przypadku zgłoszenia żądania nowych funkcji.
