---
title: "Konfigurowanie funkcji Azure AD Join dla użytkowników| Microsoft Docs"
description: "Wyjaśnia, jak administratorzy mogą skonfigurować funkcję Azure AD Join dla katalogu lokalnego i rejestracji urządzeń."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: bfc5d415-c918-4d8b-afee-b3f41cc28469
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/16/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 0480c4f89b6036897bc8d7eff81b56262ea8806c
ms.openlocfilehash: e4e0fa77552c4df2ea5bb9ddae916e7c661824d1
ms.contentlocale: pl-pl
ms.lasthandoff: 02/23/2017

---
# <a name="setting-up-azure-ad-join-in-your-organization"></a>Konfigurowanie funkcji Azure AD Join w organizacji
Przed skonfigurowaniem usługi Azure Active Directory Join (Azure AD Join), należy albo zsynchronizować katalog lokalny użytkowników z chmurą, albo ręcznie utworzyć konta zarządzane w usłudze Azure AD.

Aby uzyskać szczegółowe instrukcje dotyczące synchronizacji lokalnych użytkowników z usługą Azure AD, zobacz [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).

Aby ręcznie tworzyć użytkowników w usłudze Azure AD i zarządzać nimi, zobacz [Zarządzanie użytkownikami w usłudze Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## <a name="set-up-device-registration"></a>Konfigurowanie rejestracji urządzeń
1. Zaloguj się w witrynie Azure Portal jako administrator.
2. W lewym okienku wybierz pozycję **Active Directory**.
3. Na karcie **Katalog** wybierz swój katalog.
4. Wybierz kartę **Konfigurowanie**.
5. Przejdź do sekcji **Urządzenia**.
6. Na karcie **Urządzenia** ustaw wartości poniższych elementów:  
   * **MAKSYMALNA LICZBA URZĄDZEŃ PRZYPADAJĄCYCH NA JEDNEGO UŻYTKOWNIKA**: wybierz maksymalną liczbę urządzeń, jaką użytkownik może mieć w usłudze Azure AD.  Jeśli użytkownik osiągnie ten limit przydziału, nie będzie mógł dodać dodatkowych urządzeń dopóki co najmniej jedno z istniejących urządzeń nie zostanie usunięte.
   * **WYMAGAJ UWIERZYTELNIANIA WIELOSKŁADNIKOWEGO PODCZAS PRZYŁĄCZANIA URZĄDZEŃ**: ustal, czy użytkownicy muszą zapewnić drugi składnik uwierzytelniania w celu przyłączenia urządzenia do usługi Azure AD. Aby uzyskać więcej informacji o usłudze Azure Multi-Factor Authentication, zobacz [Wprowadzenie do usługi Azure Multi-Factor Authentication w chmurze](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).
   * **UŻYTKOWNICY MOGĄ PRZYŁĄCZAĆ URZĄDZENIA DO USŁUGI AZURE AD**: wybierz użytkowników i grupy, które mogą przyłączać urządzenia do usługi Azure AD.
   * **DODATKOWI ADMINISTRATORZY URZĄDZEŃ PRZYŁĄCZONYCH DO USŁUGI AZURE AD**: korzystając z usługi Azure AD Premium lub pakietu Enterprise Mobility Suite (EMS), możesz wybrać, którzy użytkownicy mają przyznane uprawnienia administratora lokalnego do urządzenia. Administratorom globalnym i właścicielom urządzeń uprawnienia administratora lokalnego są przyznawane domyślnie.

<center>![Konfigurowanie rejestracji urządzeń](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png) </center>

Skonfigurowanie funkcji Azure AD Join dla użytkowników pozwala im łączyć się z usługą Azure AD za pomocą ich urządzeń firmowych lub osobistych.

Poniżej przedstawiono trzy możliwe scenariusze postępowania w celu umożliwienia użytkownikom skonfigurowania funkcji Azure AD Join:

* Użytkownicy dołączają urządzenie należące do firmy bezpośrednio do usługi Azure AD.
* Użytkownicy przyłączają do domeny urządzenia należącego do firmy do lokalnej usługi Active Directory, a następnie rozszerzają urządzenie o usługę Azure AD.
* Użytkownicy dodają konta służbowe do systemu Windows na urządzeniu osobistym.

## <a name="additional-information"></a>Dodatkowe informacje
* [System Windows 10 dla przedsiębiorstw: sposoby używania urządzenia do pracy](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozszerzanie możliwości chmury dla urządzeń z systemem Windows 10 za pomocą usługi Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Dowiedz się więcej na temat scenariuszy użycia funkcji Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Łączenie urządzeń przyłączonych do domeny z usługą Azure AD w celu korzystania z możliwości systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurowanie funkcji Azure AD Join](active-directory-azureadjoin-setup.md)


