---
title: "Dostosowywanie i Azure Active Directory B2B współpracy interfejsu API | Dokumentacja firmy Microsoft"
description: "Współpraca B2B usługi Azure Active Directory wspiera relacje między firmami, umożliwiając partnerom biznesowym selektywne uzyskiwanie dostępu do Twoich aplikacji firmowych"
services: active-directory
documentationcenter: 
author: twooley
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/11/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: b40dc42c1dfc8910f9be9242fee3beeade92d193
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a>Dostosowywanie i Azure Active Directory B2B współpracy interfejsu API

Firma Microsoft zakończonej wielu klientów, powiedz nam, że chcą dostosowania procesu zaproszenia w taki sposób, który najlepiej pasuje do organizacji. O interfejsie API możesz to zrobić tylko. [https://Developer.microsoft.com/Graph/docs/API-Reference/V1.0/Resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-the-invitation-api"></a>Możliwości zaproszenia do interfejsu API
Interfejs API oferuje następujące możliwości:

1. Zaproś użytkownika zewnętrznego z *żadnych* adres e-mail.

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. Dostosowywanie miejscu użytkownikom przejście po zaakceptowaniu zaproszenia ich.

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. Wysyłanie poczty standardowe zaproszenia przez nas

    ```
    "sendInvitationMessage": true
    ```

  komunikat do adresata, który można dostosować

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. I wybierz opcję DW: osób, które mają być zachowane w pętli o tym współpracownika zaproszenie.

5. Lub całkowicie dostosować, wybierając pozycję nie wysłać powiadomienia za pomocą usługi Azure AD Twojego zaproszenia i przepływ pracy dołączania.

    ```
    "sendInvitationMessage": false
    ```

  W takim przypadku można odzyskać realizacji adres URL z interfejsu API, który można osadzić w szablonu wiadomości e-mail, wiadomości Błyskawicznych lub innej metody dystrybucji wybranych przez użytkownika.

6. Ponadto jeśli jesteś administratorem, można zaprosić użytkowników jako elementu członkowskiego.

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a>Modelu autoryzacji
Interfejs API mogą być uruchamiane w następujących trybach autoryzacji:

### <a name="app--user-mode"></a>Aplikacja + trybu użytkownika
W tym trybie, kto korzysta na potrzeby interfejsu API uprawnienia można utworzyć B2B zaproszeń do skorzystania z.

### <a name="app-only-mode"></a>Tryb tylko do aplikacji
W kontekście tylko aplikacji Aplikacja musi zakres User.Invite.All zaproszenia powiodło się.

Aby uzyskać więcej informacji, zapoznaj się: https://graph.microsoft.io/docs/authorization/permission_scopes


## <a name="powershell"></a>PowerShell
Obecnie istnieje możliwość dodania i łatwo zaprosić użytkowników zewnętrznych do organizacji przy użyciu programu PowerShell. Tworzenie zaproszenia, za pomocą polecenia cmdlet:

```
New-AzureADMSInvitation
```

Można użyć następujących opcji:

* -InvitedUserDisplayName
* -InvitedUserEmailAddress
* -SendInvitationMessage
* -InvitedUserMessageInfo

Można również wyewidencjonować odwołania zaproszenia interfejsu API w [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="next-steps"></a>Kolejne kroki

Zobacz nasze inne artykuły dotyczące współpracy B2B w usłudze Azure AD:

* [Czym jest współpraca B2B w usłudze Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak Administratorzy usługi Azure Active Directory dodać użytkowników współpracy B2B?](active-directory-b2b-admin-add-users.md)
* [Jak pracownicy przetwarzający informacje dodać użytkowników współpracy B2B?](active-directory-b2b-iw-add-users.md)
* [Elementy współpracy B2B zaproszenie](active-directory-b2b-invitation-email.md)
* [Realizacji zaproszenia współpracy B2B](active-directory-b2b-redemption-experience.md)
* [Azure licencjonowania współpracy B2B usługi AD](active-directory-b2b-licensing.md)
* [Rozwiązywanie problemów z współpracy usługi Azure Active Directory B2B](active-directory-b2b-troubleshooting.md)
* [Azure współpracy B2B usługi Active Directory — często zadawane pytania (FAQ)](active-directory-b2b-faq.md)
* [Usługa Multi-Factor Authentication dla użytkowników współpracy B2B](active-directory-b2b-mfa-instructions.md)
* [Dodawanie użytkowników współpracy B2B bez zaproszenia](active-directory-b2b-add-user-without-invite.md)
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
