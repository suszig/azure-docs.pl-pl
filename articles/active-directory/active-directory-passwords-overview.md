---
title: "Azure AD samoobsługowego resetowania hasła omówienie | Dokumentacja firmy Microsoft"
description: "Do czego może usługi Azure AD hasła samodzielnego resetowania dla Twojej organizacji?"
services: active-directory
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
ms.openlocfilehash: f577ee95c34f9bc3065c026bc943d30622b9d658
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="azure-ad-self-service-password-reset-for-the-it-professional"></a>Azure AD samodzielnego resetowania haseł dla specjalistów IT

Azure Active Directory (Azure AD) samoobsługowego resetowania hasła (SSPR) umożliwia użytkownikom resetowania swoich haseł na ich własnych, kiedy i gdzie należy, co pozwala administratorom kontrolę nad jak pobiera zresetować swoje hasło. Użytkownicy nie muszą już do działu pomocy technicznej, po prostu do zresetowania swojego hasła.

* **Zmiana hasła samoobsługi** — użytkownik zna hasło, ale chce zmienić ją na inną.
* **Samoobsługowego resetowania hasła** — użytkownika nie może się zalogować i chce zresetować hasło przy użyciu co najmniej jeden z następujących metod uwierzytelniania zweryfikowany.
   * Wiadomość SMS na telefon komórkowy zweryfikowanych
   * Połączenie telefoniczne na zweryfikowanych telefon komórkowy lub pakietu office
   * Wyślij wiadomość e-mail do konta zweryfikowanych dodatkowej poczty e-mail
   * Odpowiedzi na pytania zabezpieczające
* **Odblokowywanie kont samoobsługi** — użytkownik nie ma Zaloguj się przy użyciu hasła została zablokowana i chcesz odblokować konto bez interwencji administratora przy użyciu ich metod uwierzytelniania.

## <a name="why-choose-azure-ad-self-service-password-reset"></a>Dlaczego Azure AD samoobsługowego resetowania hasła

* **Obniżenie kosztów** jako pomoc techniczna asystowaną resetowania haseł zwykle konta 20% wywołań obsługi organizacji IT. 
* **Poprawę środowiska użytkownika końcowego** i **zmniejszyć wolumin techniczną** , zapewniając użytkownikom końcowym zasilania problemów własne hasło jednocześnie bez wywoływania pomocy technicznej lub otwarcia żądania pomocy technicznej.
* **Dysk mobilności** jak użytkownicy mogą resetować hasła w dowolnym miejscu.
* **Obsługa kontroli** zasad zabezpieczeń. Administratorzy mogą w dalszym ciągu egzekwowanie zasad, które już dziś.

Gdy wszystko będzie gotowe, możesz rozpocząć pracę z usługi Azure AD SSPR za pomocą naszych [szybki start wskazówki](active-directory-passwords-getting-started.md) i szybko uzyskiwać Resetowanie własnych haseł użytkowników.

## <a name="azure-ad-self-service-password-reset-availability"></a>Azure AD samoobsługowego resetowania hasła dostępności

Hasła usługi Azure AD Samoobsługowe resetowanie jest dostępne w trzech warstwach w zależności od subskrypcji.

* **Azure AD wolne** — tylko w chmurze, Administratorzy mogą resetować swoje hasła.
* **Azure AD podstawowa** lub **płatnej subskrypcji programu Office 365** — tylko w chmurze, użytkownicy mogą resetować swoje hasła.
* **Azure AD Premium** — federacyjnych dowolnego użytkownika lub administratora, w tym trybie tylko do chmury, lub hasło synchronizowane użytkownicy, można resetować swoje hasła. Hasłami lokalnymi wymagają funkcji zapisywania zwrotnego haseł jest włączony.

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Azure AD cennik, umowy SLA, aktualizacji i plan

Więcej szczegółów na temat licencjonowania, ceny i przyszłych planów znajdują się w lokacji, które należy wykonać.

* [**Szczegóły cennika usługi Azure AD**](https://azure.microsoft.com/pricing/details/active-directory/)
* [**Cennik usługi Office 365**](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [**Umowy dotyczące poziomu usług platformy Azure**](https://azure.microsoft.com/support/legal/sla/)
* [**Umowa dotycząca poziomu usług Microsoft Online Services**](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [**Aktualizacje platformy Azure**](https://azure.microsoft.com/updates/)
* [**Plan platformy Azure**](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Następne kroki

* Czy chcesz teraz rozpocząć pracę z SSPR? [Konfiguracja usługi Azure AD samodzielnego resetowania hasła usługi](active-directory-passwords-getting-started.md).
* Planowanie pomyślne wdrożenie SSPR do użytkowników przy użyciu wskazówek w naszym [ **przewodnik wdrażania**](active-directory-passwords-best-practices.md).
* [Resetowanie lub zmienianie hasła](active-directory-passwords-update-your-own-password.md).
* [Rejestrowanie się w celu samodzielnego resetowania hasła](active-directory-passwords-reset-register.md).