---
title: "Azure Active Directory Identity Protection — sposób odblokowania użytkowników | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak odblokować użytkowników, które zostały zablokowane przez zasady usługi Azure Active Directory Identity Protection."
services: active-directory
keywords: "ochronę tożsamości usługi Azure active directory, Odblokuj użytkownika"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: a953d425-a3ef-41f8-a55d-0202c3f250a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 0bef2fa17474298f36f360efa49922f5f85b21a1
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2017
---
# <a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory Identity Protection — sposób odblokowania użytkowników
Z usługi Azure Active Directory Identity Protection można skonfigurować zasady blokowania użytkowników skonfigurowanych warunków. Zazwyczaj zablokowany użytkownik kontaktów pomocy technicznej, aby stać się odblokowany. Tematy to omówiono kroki można wykonać, aby odblokować zablokowanego użytkownika.

## <a name="determine-the-reason-for-blocking"></a>Określić przyczynę zablokowania
Pierwszym krokiem do odblokowanie użytkownika należy określić typ zasad, który został zablokowany użytkownik, ponieważ Twoje następne kroki są od niego zależne.
Azure Active Directory Identity Protection użytkownik może zostać albo zablokowana przez zasady logowania ryzyko lub zasad ryzyka dla użytkownika.

Możesz uzyskać typ zasad, który zablokował użytkownika z nagłówkiem w oknie dialogowym, który został przedstawiony użytkownikowi podczas próby logowania:

| Zasady | Okno dialogowe użytkownika |
| --- | --- |
| Ryzyko logowania |![Zablokowane logowania](./media/active-directory-identityprotection-unblock-howto/02.png) |
| Ryzyko użytkownika |![Konto zablokowane](./media/active-directory-identityprotection-unblock-howto/104.png) |

Użytkownik, który jest zablokowany przez:

* Zasady logowania ryzyko jest także znana jako podejrzane logowania
* Zasad ryzyka dla użytkownika jest także znana jako konto na ryzyko

## <a name="unblocking-suspicious-sign-ins"></a>Odblokowywanie podejrzane logowania
Aby odblokować podejrzane logowanie, masz następujące opcje:

1. **Logowania z lokalizacji znanych lub urządzenia** -typową przyczyną zablokowanych podejrzane logowania są próby logowania z nieznanych lokalizacji lub urządzeń. Użytkownicy może szybko określić, czy jest to blokowania powód próba logowania z lokalizacji znanych lub urządzenia.
2. **Wyklucz z zasad** — Jeśli uważasz, że bieżąca konfiguracja zasad logowania powoduje problemy dotyczące konkretnych użytkowników, można wykluczyć użytkowników z niego. Zobacz [Azure Active Directory Identity Protection](active-directory-identityprotection.md) więcej szczegółów.
3. **Wyłącz zasady** — Jeśli uważasz, że konfigurację zasad powoduje problemy dotyczące wszystkich użytkowników, możesz wyłączyć zasady. Zobacz [Azure Active Directory Identity Protection](active-directory-identityprotection.md) więcej szczegółów.

## <a name="unblocking-accounts-at-risk"></a>Odblokowywanie kont na ryzyko
Aby odblokować konto zagrożone, masz następujące opcje:

1. **Zresetuj hasło** — można zresetować hasło użytkownika. Zobacz [resetowania ręcznego bezpiecznego hasła](active-directory-identityprotection.md#manual-secure-password-reset) więcej szczegółów.
2. **Odrzuć wszystkie zdarzenia o podwyższonym ryzyku** — bloki zasad ryzyka użytkownika użytkownika, jeśli użytkownik skonfigurowany ryzyka poziom blokuje dostęp został osiągnięty. Użytkownik może zmniejszyć jego poziom ryzyka przez ręczne zamknięcie zgłoszone zdarzenia ryzyka. Aby uzyskać więcej informacji, zobacz [zamknięcie zdarzenia o podwyższonym ryzyku ręcznie](active-directory-identityprotection.md#closing-risk-events-manually).
3. **Wyklucz z zasad** — Jeśli uważasz, że bieżąca konfiguracja zasad logowania powoduje problemy dotyczące konkretnych użytkowników, można wykluczyć użytkowników z niego. Zobacz [Azure Active Directory Identity Protection](active-directory-identityprotection.md) więcej szczegółów.
4. **Wyłącz zasady** — Jeśli uważasz, że konfigurację zasad powoduje problemy dotyczące wszystkich użytkowników, możesz wyłączyć zasady. Zobacz [Azure Active Directory Identity Protection](active-directory-identityprotection.md) więcej szczegółów.

## <a name="next-steps"></a>Następne kroki
 Czy chcesz dowiedzieć się więcej o usłudze Azure AD Identity Protection? Zapoznaj się z [Azure Active Directory Identity Protection](active-directory-identityprotection.md).
