---
title: "Azure współpracy B2B usługi Active Directory dla organizacji hybrydowe | Dokumentacja firmy Microsoft"
description: "Nadaj partnerom dostęp do lokalnego i zasobów w chmurze przy współpracy B2B usługi Azure AD"
services: active-directory
documentationcenter: 
author: twooley
manager: mtillman
editor: 
tags: 
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.date: 12/15/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 2e690eeea6a9f7e1cc10830a913774daa3c66689
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/16/2017
---
# <a name="azure-active-directory-b2b-collaboration-for-hybrid-organizations"></a>Azure współpracy B2B usługi Active Directory dla organizacji hybrydowego

W hybrydowego organizacji, w którym znajduje się zarówno lokalnie i w chmurze zasobów, możesz chcieć zapewnia partnerami zewnętrznymi dostęp do lokalnego i zasobów w chmurze. Aby uzyskać dostęp do zasobów lokalnych można zarządzać partnera kont lokalnie w lokalnym środowisku usługi Active Directory. Partnerzy Zaloguj się przy użyciu swoich poświadczeń konta partnera dostęp do zasobów organizacji. Aby udostępnić partnerów zasobów w chmurze przy użyciu tych samych poświadczeń, teraz umożliwia łączenie usługi Azure Active Directory (Azure AD) zsynchronizować konta partnera w chmurze jako użytkowników B2B usługi Azure AD (to znaczy użytkownikom UserType = gościa).

## <a name="identify-unique-attributes-for-usertype"></a>Identyfikowanie unikatowych atrybutach UserType

Przed włączeniem synchronizacji atrybut UserType, najpierw należy zdecydować, jak pochodzi ten atrybut UserType z lokalnej usługi Active Directory. Innymi słowy jakich parametrów w danym środowisku lokalnych są unikatowe dla sieci zewnętrznych współpracowników? Określ parametr, który odróżnia tych zewnętrznych współpracowników od członków organizacji.

Typowe są dwa podejścia dla tego celu:

- Należy określić atrybut usługi Active Directory nieużywane lokalnymi (na przykład extensionAttribute1) do użycia jako atrybutu źródłowego. 
- Można również pochodzić atrybut UserType od innych właściwości. Na przykład chcesz synchronizować wszystkich użytkowników jako Gość, jeśli ich lokalne atrybut UserPrincipalName usługi Active Directory kończy się z domeną  *@partners.fabrikam123.org* .
 
Atrybut szczegółowe wymagania można znaleźć [Włącz synchronizację UserType](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype). 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Konfigurowanie usługi Azure AD Connect do synchronizacji użytkowników w chmurze

Po zidentyfikowaniu unikatowy atrybut można skonfigurować usługi Azure AD Connect do synchronizowania tych użytkowników zdalnych do chmury jako użytkowników B2B usługi Azure AD (to znaczy użytkownikom UserType = gościa). Z punktu widzenia autoryzacji użytkownicy są nierozróżnialne od użytkowników B2B została utworzona za pośrednictwem procesu zaproszenia współpracy B2B usługi Azure AD.

Aby uzyskać instrukcje dotyczące wdrażania, zobacz [Włącz synchronizację UserType](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype).

## <a name="next-steps"></a>Następne kroki

- Przegląd współpracy B2B usługi Azure AD, zobacz [co to jest współpraca B2B usługi Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
- Omówienie programu Azure AD Connect zobacz [integrację katalogów lokalnych z usługą Azure Active Directory](connect/active-directory-aadconnect.md).

