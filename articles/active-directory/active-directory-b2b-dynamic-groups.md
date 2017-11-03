---
title: "Grupami dynamicznymi i współpracy usługi Azure Active Directory B2B | Dokumentacja firmy Microsoft"
description: "Azure współpracy B2B usługi Active Directory mogą być używane z grupami dynamicznymi w usłudze Azure AD"
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
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: sasubram
ms.openlocfilehash: 5818c41610c8c5df89abcb0dcd058bcbe9579ce7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a>Grupami dynamicznymi i współpracy usługi Azure Active Directory B2B

## <a name="what-are-dynamic-groups"></a>Co to są grupami dynamicznymi?
Dynamicznej konfiguracji przynależności do grupy zabezpieczeń usługi Azure Active Directory (Azure AD) jest dostępna w [portalu Azure](https://portal.azure.com). Administratorzy mogą ustawić reguły do wypełniania grupy, które są tworzone w usłudze Azure Active Directory na podstawie atrybutów użytkownika (na przykład userType, działu lub kraju). Automatycznie dodawane do elementów członkowskich lub usunięte z grupy zabezpieczeń na podstawie ich atrybutów. Te grupy mogą umożliwiać dostęp do aplikacji lub zasobów w chmurze (witryny programu SharePoint, dokumenty) i przypisywanie licencji do elementów członkowskich. Przeczytaj więcej na temat grup dynamicznych w [grup w usłudze Azure Active Directory w wersji dedykowanej](active-directory-accessmanagement-dedicated-groups.md).

Odpowiednie [licencjonowania usługi Azure AD Premium P1 lub P2](https://azure.microsoft.com/pricing/details/active-directory/) jest wymagany do tworzenia i używania grup dynamicznych. Dowiedz się więcej informacji, zobacz artykuł [tworzenia reguł opartych na atrybutach dynamiczne członkostwo w grupie w usłudze Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).

## <a name="what-are-the-built-in-dynamic-groups"></a>Co to są wbudowane grupy dynamiczne?
**Wszyscy użytkownicy** Dynamiczna grupa umożliwia administratorom dzierżawy utworzenie grupy zawierającej wszystkich użytkowników w dzierżawie za pomocą jednego kliknięcia. Domyślnie **wszyscy użytkownicy** grupy należą wszyscy użytkownicy w katalogu, w tym elementów członkowskich i gości.
W ramach nowego portalu administracyjnego usługi Azure Active Directory, możesz włączyć **wszyscy użytkownicy** w widoku ustawienia grupy.

![grupy wbudowane](media/active-directory-b2b-dynamic-groups/built-in-groups.png)

## <a name="hardening-the-all-users-dynamic-group"></a>Ograniczenie funkcjonalności wszystkich Dynamiczna grupa użytkowników
Domyślnie **wszyscy użytkownicy** grupa zawiera również użytkowników (Gość) współpracy B2B. Można dodatkowo zabezpieczyć Twoje **wszyscy użytkownicy** grupy przy użyciu reguły, aby usunąć gości. Na poniższej ilustracji pokazano **wszyscy użytkownicy** grupy zmodyfikowane w celu wykluczenia gości.

![Włącz wszystkie grupy użytkowników](media/active-directory-b2b-dynamic-groups/enable-all-users-group.png)

Użytkownik może być również bardzo przydatne do tworzenia nowej grupy dynamicznej, zawierający tylko gości, dzięki czemu można zastosować zasad (np. zasady dostępu warunkowego AD Azure) do nich.
Jakie takiej grupy może wyglądać tak:

![Wyklucz gości](media/active-directory-b2b-dynamic-groups/exclude-guest-users.png)

## <a name="next-steps"></a>Następne kroki

Zobacz nasze inne artykuły dotyczące współpracy B2B w usłudze Azure AD:

* [Czym jest współpraca B2B w usłudze Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Właściwości użytkownika współpracy B2B](active-directory-b2b-user-properties.md)
* [Dodawanie do roli użytkownika współpracy B2B](active-directory-b2b-add-guest-to-role.md)
* [Delegowanie zaproszenia współpracy B2B](active-directory-b2b-delegate-invitations.md)
* [Kod współpracy B2B i przykłady środowiska PowerShell](active-directory-b2b-code-samples.md)
* [Konfigurowanie aplikacji SaaS do współpracy B2B](active-directory-b2b-configure-saas-apps.md)
* [Tokeny użytkownika współpracy B2B](active-directory-b2b-user-token.md)
* [Oświadczenia użytkowników współpracy B2B mapowania](active-directory-b2b-claims-mapping.md)
* [Udostępnianie zewnętrzne w usłudze Office 365](active-directory-b2b-o365-external-user.md)
* [Bieżące ograniczenia współpracy B2B](active-directory-b2b-current-limitations.md)
