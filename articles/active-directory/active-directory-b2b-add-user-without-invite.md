---
title: "Dodawanie użytkowników współpracy B2B do usługi Azure Active Directory bez zaproszenia | Dokumentacja firmy Microsoft"
description: "Możesz pozwolić, aby dodać inne gości do usługi Azure AD bez realizowanie zaproszenie w usłudze Azure Active Directory B2B współpracy użytkownika gościa."
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
ms.date: 03/15/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 68d828b8fb23f7074be5fe691a4250d0fa653ab7
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a>Dodaj gości współpracy B2B bez zaproszenia

Można zezwolić użytkownikowi, takich jak przedstawiciela partnera, aby dodać użytkowników od partnera do organizacji bez konieczności zaproszenia do zostać zrealizowane. Wszystko, co należy wykonać to nadać temu użytkownikowi wyliczenie uprawnienia w katalogu używanego org. partnera 

Udziel tych uprawnień, gdy:

1. Użytkownik w organizacji hosta (na przykład WoodGrove) zaprasza jeden użytkownik z organizacji partnerskiej (na przykład Sam@litware.com) jako Gość.
2. Administrator w organizacji hosta ustawienie zasad, które umożliwiają Sam do identyfikowania i dodać inni użytkownicy z organizacji partnerskiej (Litware).
3. Teraz Sam można dodawać innych użytkowników z Litware do katalogu WoodGrove, grupy lub aplikacji bez konieczności zaproszenia do zostać zrealizowane. Jeśli Sam ma wyliczenie odpowiednie uprawnienia w Litware, odbywa się automatycznie.

### <a name="next-steps"></a>Kolejne kroki

Zobacz nasze inne artykuły dotyczące współpracy B2B w usłudze Azure AD:

* [Czym jest współpraca B2B w usłudze Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak Administratorzy usługi Azure Active Directory dodać użytkowników współpracy B2B?](active-directory-b2b-admin-add-users.md)
* [Jak pracownicy przetwarzający informacje dodać użytkowników współpracy B2B?](active-directory-b2b-iw-add-users.md)
* [Elementy współpracy B2B zaproszenie](active-directory-b2b-invitation-email.md)
* [Realizacji zaproszenia współpracy B2B](active-directory-b2b-redemption-experience.md)
* [Azure licencjonowania współpracy B2B usługi AD](active-directory-b2b-licensing.md)
* [Rozwiązywanie problemów z współpracy usługi Azure Active Directory B2B](active-directory-b2b-troubleshooting.md)
* [Azure współpracy B2B usługi Active Directory — często zadawane pytania (FAQ)](active-directory-b2b-faq.md)
* [Dostosowywanie i Azure Active Directory B2B współpracy interfejsu API](active-directory-b2b-api.md)
* [Usługa Multi-Factor Authentication dla użytkowników współpracy B2B](active-directory-b2b-mfa-instructions.md)
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)