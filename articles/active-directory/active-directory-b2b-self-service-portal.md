---
title: "Zapisywania się do portalu usługi Azure Active Directory B2B współpracy | Dokumentacja firmy Microsoft"
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
ms.date: 05/24/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: bb63a3b23bdcaac5c94d43bb8e7294a82b0c3fa0
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Portal samoobsługowy do zapisywania do współpracy B2B usługi Azure AD

Klienci umożliwiają wykonywanie wielu z wbudowanych funkcji udostępnianych przez administratora IT [portalu Azure](https://portal.azure.com) i [panelu dostępu aplikacji](https://myapps.microsoft.com) dla użytkowników końcowych. Ale możemy również pamiętać, że firmy muszą dostosować przepływ pracy dołączania użytkowników B2B do potrzeb organizacji. Można wykonać z [zaproszenia interfejsu API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

W dyskusjach ze specjalistami z klientów ma jednego potrzebują, który zwiększa się przede wszystkim innym. Zapraszanie organizacji może nie wiedzieć, wcześniej, którzy są poszczególne zewnętrznych współpracowników którzy potrzebują dostępu do zasobów. Chcieli sposób dla użytkowników z firm partnerskich o zarejestrowanie się przy użyciu zestawu zasad, które kontroluje zaproszenia organizacji. Ten scenariusz jest możliwe za pośrednictwem interfejsów API, dlatego firma Microsoft opublikowane projektu w witrynie Github, która została właśnie tę: [przykładowy projekt Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

Ten projekt Github pokazano, jak organizacje mogą przy użyciu interfejsów API i podaj opartych na zasadach, Samoobsługowe możliwość zapisywania do zaufanych partnerów przy użyciu reguł, które określają aplikacje, które mogą uzyskiwać dostęp do. Użytkowników z firm partnerskich można uzyskać dostęp do zasobów, gdy to konieczne, bezpieczne, bez konieczności zaproszenia organizacji ręcznie dołączyć je. Możesz z łatwością wdrożyć projekt do subskrypcji platformy Azure wybranych przez użytkownika.

## <a name="as-is-code"></a>Jako — kod

Należy pamiętać, ten kod jest udostępniana jako próbka do zaprezentowania użycia zaproszenie usługi Azure Active Directory B2B interfejsu API. Powinny zostać dostosowane przez zespół deweloperów lub partnera i należy ją sprawdzić przed wdrożeniem w środowisku produkcyjnym.

## <a name="next-steps"></a>Kolejne kroki

Przeglądaj inne artykuły dotyczące współpracy B2B usługi Azure AD:
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