---
title: "Delegowanie zaproszeń do skorzystania z usługi Azure Active Directory B2B współpracy | Dokumentacja firmy Microsoft"
description: "Właściwości użytkownika współpraca w usłudze Azure Active Directory B2B są konfigurowane"
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
ms.openlocfilehash: 08d06153ab64021d94cd3fdc8ac13839bb4c6908
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Delegowanie zaproszeń do skorzystania z usługi Azure Active Directory B2B współpracy

Ze współpracą między firmami (B2B) w usłudze Azure Active Directory (Azure AD) nie trzeba być administratorem globalnym, aby wysłać zaproszenia. Można użyć zasad i delegować zaproszeń do użytkowników, których role zezwolić im na wysyłanie zaproszeń do skorzystania z. Jest ważne nowy sposób, aby delegować zaproszenia użytkownika gościa za pomocą roli zapraszającej gościa.

## <a name="guest-inviter-role"></a>Rola zapraszającej gościa
Użytkownika można przypisać do roli zapraszającej gościa, aby wysłać zaproszenia. Nie trzeba być członkiem roli administratora globalnego Wyślij zaproszenia. Domyślnie normalnych użytkowników można także wywoływać interfejs API zaproszenia, chyba że administrator globalny wyłączone zaproszeń do normalnych użytkowników. Użytkownik może również wywołać interfejsu API przy użyciu portalu Azure lub programu PowerShell.

Oto przykład pokazujący sposób, aby dodać użytkownika do roli zapraszającej gościa za pomocą programu PowerShell:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a>Formant, który można zaprosić

![Kontrolowanie sposobu zaprosić](media/active-directory-b2b-delegate-invitations/control-who-to-invite.png)

Współpracy B2B usługi Azure AD administratora dzierżawy. można ustawić następujące zasady zaproszenia:

- Wyłącz zaproszenia
- Tylko administratorzy i użytkownicy w roli zapraszającej gościa można zaprosić
- Zaprosić administratorów, roli zapraszającej Gość i członków
- Wszyscy użytkownicy, w tym gości, można zaprosić

Domyślnie dzierżaw są ustawione na #4. (Wszystkich użytkowników, w tym gości, można zaprosić użytkowników B2B).

## <a name="next-steps"></a>Następne kroki

Zobacz nasze inne artykuły dotyczące współpracy B2B w usłudze Azure AD:

* [Czym jest współpraca B2B w usłudze Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Właściwości użytkownika współpracy B2B](active-directory-b2b-user-properties.md)
* [Dodawanie do roli użytkownika współpracy B2B](active-directory-b2b-add-guest-to-role.md)
* [Grupami dynamicznymi i współpracy B2B](active-directory-b2b-dynamic-groups.md)
* [Kod współpracy B2B i przykłady środowiska PowerShell](active-directory-b2b-code-samples.md)
* [Konfigurowanie aplikacji SaaS do współpracy B2B](active-directory-b2b-configure-saas-apps.md)
* [Tokeny użytkownika współpracy B2B](active-directory-b2b-user-token.md)
* [Oświadczenia użytkowników współpracy B2B mapowania](active-directory-b2b-claims-mapping.md)
* [Udostępnianie zewnętrzne w usłudze Office 365](active-directory-b2b-o365-external-user.md)
* [Bieżące ograniczenia współpracy B2B](active-directory-b2b-current-limitations.md)
