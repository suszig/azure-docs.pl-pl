---
title: "Zapisywania się do portalu usługi Azure Active Directory B2B współpracy | Dokumentacja firmy Microsoft"
description: "Współpraca B2B usługi Azure Active Directory wspiera relacje między firmami, umożliwiając partnerom biznesowym selektywne uzyskiwanie dostępu do Twoich aplikacji firmowych"
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
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: 307373c75bbb87cec683f7a3097f8f159c9d5e61
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Portal samoobsługowy do zapisywania do współpracy B2B usługi Azure AD

Klienci umożliwiają wykonywanie wielu z wbudowanych funkcji, które są dostępne za pośrednictwem naszego administrator IT [portalu Azure](https://portal.azure.com) i naszych [panelu dostępu aplikacji](https://myapps.microsoft.com) dla użytkowników końcowych. Ale możemy również pamiętać, że firmy muszą dostosować przepływ pracy dołączania użytkowników B2B do potrzeb organizacji. Można wykonać z [interfejsach API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

W dyskusjach ze specjalistami z naszych klientów widzimy się, że jeden wspólny wjazd należy przede wszystkim innym. Zapraszanie organizacji może nie wiedzieć, wcześniej, którzy są poszczególne zewnętrznych współpracowników którzy potrzebują dostępu do zasobów. Chcieli sposób dla użytkowników z firm partnerskich o zarejestrowanie się przy użyciu zestawu zasad, które kontroluje zaproszenia organizacji. Ten scenariusz jest możliwe za pośrednictwem naszych interfejsów API, dlatego firma Microsoft opublikowane projektu w witrynie Github, która została właśnie tę: [przykładowy projekt Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

Naszych projektu Github pokazano, jak organizacji można użyć naszych interfejsów API i zapewnienia opartych na zasadach, Samoobsługowe możliwość zapisywania do swoich zaufanych partnerów, z zasadami, które określają aplikacje, które mogą uzyskiwać dostęp do. Użytkowników z firm partnerskich można uzyskać dostęp do zasobów, gdy to konieczne, bezpieczne, bez konieczności zaproszenia organizacji ręcznie dołączyć je. Możesz z łatwością wdrożyć projekt do subskrypcji platformy Azure wybranych przez użytkownika.

## <a name="as-is-code"></a>Jako — kod

Należy pamiętać, ten kod jest udostępniana jako próbka do zaprezentowania użycia zaproszenie usługi Azure Active Directory B2B interfejsu API. Powinny zostać dostosowane przez zespół deweloperów lub partnera i należy ją sprawdzić przed wdrożeniem w środowisku produkcyjnym.

## <a name="next-steps"></a>Następne kroki

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