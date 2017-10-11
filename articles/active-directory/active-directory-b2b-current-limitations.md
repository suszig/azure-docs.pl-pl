---
title: "Ograniczenia współpracy usługi Azure Active Directory B2B | Dokumentacja firmy Microsoft"
description: "Bieżących ograniczeń dotyczących współpracy usługi Azure Active Directory B2B"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
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
ms.openlocfilehash: 581e5d1fb5fb08d0dc89ed2c85edcb5f0005650b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Ograniczenia dotyczące współpracy B2B usługi Azure AD
Azure współpracy B2B usługi Active Directory (Azure AD) podlega obecnie ograniczenia opisane w tym artykule.

## <a name="possible-double-multi-factor-authentication"></a>Możliwe podwójne uwierzytelnianie wieloskładnikowe
Z B2B usługi Azure AD można wymusić uwierzytelnianie wieloskładnikowe w organizacji zasobów (zaproszenia organizacji). Przyczyny tego podejścia wyszczególnione w [dostępu warunkowego dla użytkowników współpracy B2B](active-directory-b2b-mfa-instructions.md). Jeśli partnera już uwierzytelnianie wieloskładnikowe, konfigurowanie i wymuszane, użytkowników może być konieczne przeprowadzenie uwierzytelniania raz w domu organizacji, a następnie ponownie w należy do Ciebie.

## <a name="instant-on"></a>Natychmiastowa
W przepływach współpracy B2B możemy dodać użytkowników do katalogu i dynamiczne aktualizowanie ich podczas realizacji zaproszenia, przypisanie aplikacji i tak dalej. Aktualizacje i zapisy zwykle się tak zdarzyć w przypadku jednego katalogu i musi zostać zreplikowana we wszystkich wystąpieniach. Gdy wszystkie wystąpienia są aktualizowane zakończeniem replikacji. Czasami, gdy obiekt jest zapisywane lub zaktualizowane w jedno wystąpienie, a wywołanie do pobrania tego obiektu jest do innego wystąpienia, może wystąpić opóźnienia w replikacji. Jeśli tak się stanie, Odśwież lub ponów próbę, aby pomóc. Jeśli piszesz aplikację przy użyciu naszego interfejsu API ponownych prób z niektórych wycofania jest rozwiązaniem obrony, właściwą rozwiązanie tego problemu.

## <a name="next-steps"></a>Następne kroki

Zobacz nasze inne artykuły dotyczące współpracy B2B w usłudze Azure AD:

* [Czym jest współpraca B2B w usłudze Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Właściwości użytkownika współpracy B2B](active-directory-b2b-user-properties.md)
* [Dodawanie do roli użytkownika współpracy B2B](active-directory-b2b-add-guest-to-role.md)
* [Delegowanie B2bB współpracy zaproszenia](active-directory-b2b-delegate-invitations.md)
* [Grupami dynamicznymi i współpracy B2B](active-directory-b2b-dynamic-groups.md)
* [Kod współpracy B2B i przykłady środowiska PowerShell](active-directory-b2b-code-samples.md)
* [Konfigurowanie aplikacji SaaS do współpracy B2B](active-directory-b2b-configure-saas-apps.md)
* [Tokeny użytkownika współpracy B2B](active-directory-b2b-user-token.md)
* [Oświadczenia użytkowników współpracy B2B mapowania](active-directory-b2b-claims-mapping.md)
* [Udostępnianie zewnętrzne w usłudze Office 365](active-directory-b2b-o365-external-user.md)
