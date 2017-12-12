---
title: "Elementy wiadomość e-mail z zaproszeniem Azure Active Directory B2B współpracy | Dokumentacja firmy Microsoft"
description: "Azure Active Directory B2B współpracy zaproszenia szablon wiadomości e-mail"
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: ae8f96caaaeb9f3dad9f9f122b56fe264a5d7aec
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="the-elements-of-the-b2b-collaboration-invitation-email"></a>Elementy współpracy B2B zaproszenie

Wiadomości e-mail z zaproszeniem jest składnikiem krytycznym, aby przenieść do partnerów na statku jako użytkownicy współpracy B2B w usłudze Azure AD. Można używać ich, aby zwiększyć zaufanie adresata. Umożliwia dodanie legalności i doświadczenia z zaznaczenie tak społecznościowych dowód do wiadomości e-mail, aby upewnić się, że odbiorca **wprowadzenie** przycisk o zaakceptowanie zaproszenia. Zaufanie jest klucza oznacza, że zmniejszyć tarcia udostępniania. A także ma być e-mail wygląda świetnie!

![Azure wiadomość e-mail z zaproszeniem AD B2b](media/active-directory-b2b-invitation-email/invitation-email.png)

## <a name="explaining-the-email"></a>Wyjaśniający wiadomości e-mail
Oto kilka elementów wiadomości e-mail, aby wiedzieć, jak najlepiej używać ich funkcji.

### <a name="subject"></a>Temat
Temat wiadomości e-mail jest zgodny ze wzorcem następujące: Zapraszamy &lt;tenantname&gt; organizacji

### <a name="from-address"></a>Adres nadawcy
Używamy wzorzec przypominającej LinkedIn dla adres nadawcy.  Powinien być Wyczyść, który jest zapraszającej i adres e-mail od firmy oraz wyjaśnić, że wiadomość e-mail pochodzi od firmy Microsoft. Format: &lt;nazwę wyświetlaną zapraszającej&gt; z &lt;tenantname&gt; (za pośrednictwem firmy Microsoft) <invites@microsoft.com&gt;

### <a name="reply-to"></a>Odpowiedz
Odpowiedz na wiadomość e-mail ma ustawioną zapraszającej poczty e-mail, jeśli jest dostępna, tak, aby podczas odpowiadania na wiadomości e-mail wysyła wiadomość e-mail z powrotem do zapraszającej.

### <a name="branding"></a>Znakowania.
Wiadomości e-mail zaproszenia użyciu dzierżawy firmy znakowania, które można zdefiniować dla dzierżawy. Jeśli chcesz skorzystać z zalet tej możliwości [tutaj](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) znajdują się szczegółowe informacje na temat konfigurowania go. Baner logo jest wyświetlany w wiadomości e-mail. Postępuj zgodnie z rozmiarem obrazu i instrukcje jakości [tutaj](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) dla uzyskania najlepszych wyników. Ponadto nazwa firmy również zostaną wyświetlone w wywołaniu akcji.

### <a name="call-to-action"></a>Wywołanie akcji
Wywołanie akcji składa się z dwóch części: wyjaśniający, dlaczego odbiorca odebrał wiadomość i co odbiorca jest pytany, rozwiązanie.
- W sekcji "Dlaczego" może zostać zlikwidowane przy użyciu następującego wzorca: zaproszonych do uzyskania dostępu do aplikacji w &lt;tenantname&gt; organizacji

- I "co użytkownik jest pytany, zrobić" sekcja jest określane przez obecności **wprowadzenie** przycisku. Po dodaniu odbiorca bez konieczności zaproszeń do skorzystania z tego przycisku nie pojawiają się.

### <a name="inviters-information"></a>Informacje w zapraszającej
Nazwa wyświetlana zapraszającej jest dołączany do wiadomości e-mail. I dodatkowo, jeśli ustawiono obraz profilu konta usługi Azure AD, zaproszenia wiadomości e-mail zawierają również obraz. Zarówno mają na celu zwiększenie zaufania z odbiorców w wiadomości e-mail.

Jeśli jeszcze nie skonfigurowano obraz profilu, jest wyświetlana ikona inicjały zapraszającej zamiast obrazu:

  ![Wyświetlanie inicjały zapraszającej](media/active-directory-b2b-invitation-email/inviters-initials.png)

### <a name="body"></a>Treść
Treść zawiera komunikat, który zapraszającej Redaguj lub jest przekazywana zaproszenia interfejsu API. Jest obszaru tekstu, więc nie przetwarza tagów HTML ze względów bezpieczeństwa.

### <a name="footer-section"></a>Stopki
Stopki zawiera marki firmy Microsoft i umożliwia tym adresata, jeśli wiadomość e-mail została wysłana z niemonitorowanego alias. Szczególnych przypadkach:

- Zapraszającej nie ma adresu e-mail w zaproszenia dzierżawy

  ![Obraz zapraszającej nie ma adresu e-mail w dzierżawy zaproszenia](media/active-directory-b2b-invitation-email/inviter-no-email.png)


- Odbiorca nie wymaga zrealizować zaproszenia

  ![Kiedy adresat nie trzeba zrealizować zaproszenia](media/active-directory-b2b-invitation-email/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a>Następne kroki

Zobacz nasze inne artykuły dotyczące współpracy B2B w usłudze Azure AD:

* [Co to jest współpraca B2B usługi Azure AD](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak Administratorzy usługi Azure Active Directory dodać użytkowników współpracy B2B?](active-directory-b2b-admin-add-users.md)
* [Jak pracownicy przetwarzający informacje dodać użytkowników współpracy B2B?](active-directory-b2b-iw-add-users.md)
* [Realizacji zaproszenia współpracy B2B](active-directory-b2b-redemption-experience.md)
* [Azure licencjonowania współpracy B2B usługi AD](active-directory-b2b-licensing.md)
* [Rozwiązywanie problemów z współpracy usługi Azure Active Directory B2B](active-directory-b2b-troubleshooting.md)
* [Azure współpracy B2B usługi Active Directory — często zadawane pytania (FAQ)](active-directory-b2b-faq.md)
* [Dostosowywanie i Azure Active Directory B2B współpracy interfejsu API](active-directory-b2b-api.md)
* [Usługa Multi-Factor Authentication dla użytkowników współpracy B2B](active-directory-b2b-mfa-instructions.md)
* [Dodawanie użytkowników współpracy B2B bez zaproszenia](active-directory-b2b-add-user-without-invite.md)
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
