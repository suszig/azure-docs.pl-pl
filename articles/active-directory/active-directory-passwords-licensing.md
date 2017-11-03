---
title: 'Licencjonowanie: Azure AD SSPR | Dokumentacja firmy Microsoft'
description: "Azure AD samoobsługowego resetowania hasła wymagania licencyjne"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: db849cd1e9da634064f79fbc041098542580ad02
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Wymagania dotyczące usługi Azure AD samoobsługi hasła licencjonowania resetowania

Aby resetowania hasła programu Azure AD, funkcji możesz **musi mieć co najmniej jedną licencję przypisane w Twojej organizacji**. Firma Microsoft nie wymuszają licencjonowania na środowisku resetowania hasła użytkownika. Aby zachować zgodność z umowy licencyjnej firmy Microsoft, należy przypisać licencje do użytkowników korzystających z funkcji premium.

* **Tylko w chmurze użytkowników** -usługi Office 365 (O365) żadnego płatnej SKU lub Azure AD podstawowa
* **Chmura** i/lub **lokalnych użytkowników** -Azure AD Premium P1 lub P2 pakietu Enterprise Mobility + Security (EMS) i Secure produktywności przedsiębiorstwa (p)

## <a name="licenses-required-for-password-writeback"></a>Licencjami wymaganymi do zapisywania zwrotnego haseł

Aby użyć funkcji zapisywania zwrotnego haseł, musi mieć jedną z poniższych licencji przypisane w dzierżawie.

* Usługa Azure AD — warstwa Premium P1
* Usługa Azure AD — warstwa Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Microsoft 365 E3
* Microsoft 365 E5

> [!WARNING]
> Plany licencjonowania usługi Office 365 autonomiczny **nie obsługują funkcji zapisywania zwrotnego haseł** i muszą mieć jedną z powyższych planów ta funkcja działała.

Dodatkowe informacje o licencji, wraz z kosztami można znaleźć na następujących stronach

* [Usługi Azure site cennik usługi Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory, funkcje i możliwości](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Włącz grupy lub użytkownika na podstawie licencjonowania

Teraz usługi Azure AD obsługuje oparte na grupach licencjonowania, dzięki czemu administratorzy Aby przypisać licencje zbiorczo do grupy użytkowników, a nie przypisywanie pojedynczo. [Przypisać, sprawdź i rozwiąż problemy z licencjami](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

Pewnych usług firmy Microsoft nie są dostępne we wszystkich lokalizacjach. Zanim można przypisać licencję do użytkownika, administrator musi określić właściwość "Lokalizacji użytkowania" użytkownika. Można wykonać przypisania licencji użytkownika > Profil > w sekcji Ustawienia w portalu Azure. **Korzystając z przypisanie do grupy licencji, wszyscy użytkownicy bez użycia lokalizacji określonej dziedziczą lokalizację katalogu.**

## <a name="next-steps"></a>Następne kroki

* [Jak wykonać pomyślne wdrożenie SSPR?](active-directory-passwords-best-practices.md)
* [Resetowanie lub zmienianie hasła](active-directory-passwords-update-your-own-password.md).
* [Rejestrowanie się w celu samodzielnego resetowania hasła](active-directory-passwords-reset-register.md).
* [Jakie dane są używane przez SSPR i jakie dane powinny można wypełnić dla użytkowników?](active-directory-passwords-data.md)
* [Jakie metody uwierzytelniania są dostępne dla użytkowników?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Jakie są opcje zasad z SSPR?](active-directory-passwords-policy.md)
* [Co to jest funkcji zapisywania zwrotnego haseł i dlaczego I interesujących go?](active-directory-passwords-writeback.md)
* [Jak zgłosić w działaniu w SSPR](active-directory-passwords-reporting.md)
* [Co to są wszystkie opcje w SSPR i do czego ich znaczenie?](active-directory-passwords-how-it-works.md)
* [Myślę, że dany element jest uszkodzony. Jak rozwiązywać problemy z SSPR](active-directory-passwords-troubleshoot.md)
* [Masz pytania, na które nie objęte gdzieś else](active-directory-passwords-faq.md)
