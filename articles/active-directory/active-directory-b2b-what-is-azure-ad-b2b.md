---
title: "Co to jest współpraca z usługi Azure Active Directory B2B? | Microsoft Docs"
description: "Współpraca z usługą Azure Active Directory B2B obsługuje relacji między firmami, umożliwiając partnerom biznesowym selektywne uzyskiwanie dostępu do aplikacji firmowych."
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 1464387b-ee8b-4c7c-94b3-2c5567224c27
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 06/27/2017
ms.author: curtand
ms.custom: aaddev
ms.reviewer: sasubram
ms.openlocfilehash: fbc12a52555b190d43b5e953fd4d19923a25b0ed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-ad-b2b-collaboration"></a>Czym jest współpraca B2B w usłudze Azure AD?

<iframe width="560" height="315" src="https://www.youtube.com/embed/AhwrweCBdsc" frameborder="0" allowfullscreen></iframe>

Azure AD business-to-business (B2B) możliwości współpracy Włącz każdej przy użyciu usługi Azure AD do pracy bezpiecznie z użytkowników z dowolnej innej organizacji, małych i dużych organizacji. Te organizacje mogą być z usługą Azure AD lub bez, lub nawet w przypadku organizacji IT lub bez. 

Organizacje przy użyciu usługi Azure AD umożliwia dostęp do dokumentów, zasobów i aplikacji do ich partnerów, zachowując pełną kontrolę nad ich danymi firmowymi. Deweloperzy mogą używać usługi Azure AD business-to-business interfejsy API do programowania aplikacji, które sobą dwie organizacje bardziej bezpieczne. Ponadto jest dość łatwe nawigowania.

97% klientów zwracali naszą uwagę, że współpracy B2B usługi Azure AD jest bardzo ważne, aby je.

![Wykres kołowy](media/active-directory-b2b-what-is-azure-ad-b2b/97-percent-support.png)

Począwszy od początku kwietnia 2017 r. było około 3 milionów użytkowników już za pomocą funkcji współpracy B2B usługi Azure AD. I więcej niż 23% organizacji usługi Azure AD, które mają więcej niż 10 użytkownikami są już korzystających z tych funkcji.

## <a name="the-key-benefits-of-azure-ad-b2b-collaboration-to-your-organization"></a>Najważniejsze zalety współpracy B2B usługi Azure AD dla Twojej organizacji

### <a name="work-with-any-user-from-any-partner"></a>Praca z żadnym użytkownikiem od dowolnego partnera

* Partnerzy użyć własnych poświadczeń

* Nie jest wymagany dla partnerów przy użyciu usługi Azure AD

* Nie zewnętrznych katalogów lub skomplikowane wymagane

### <a name="simple-and-secure-collaboration"></a>Proste i bezpiecznej współpracy

* Dostęp do dowolnej aplikacji firmowych lub danych, podczas stosowania zaawansowane, Azure zasad zasilania AD autoryzacji

* Użytkownicy mogą łatwo

* Zabezpieczenia korporacyjnej dla aplikacji i danych

### <a name="no-management-overhead"></a>Nie nakład pracy

* Nie zewnętrznych zarządzania konto lub hasło

* Nie synchronizacji lub ręcznego zarządzania cyklem życia konta

* Nie zewnętrznych koszty administracyjne

## <a name="you-can-easily-add-b2b-collaboration-users-to-your-organization"></a>Łatwo można dodawać użytkowników współpracy B2B organizacji

Administratorzy mogą dodać użytkowników (Gość) współpracy B2B w [portalu Azure](https://portal.azure.com).

![Dodaj gości](media/active-directory-b2b-what-is-azure-ad-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-to-bring-their-own-identity"></a>Włącz Twojej współpracowników do ich własnych tożsamości

B2B współpracownicy mogą zalogować się przy użyciu tożsamości siebie. Jeśli użytkownik nie ma konta Microsoft lub konta usługi Azure AD — utworzenia dla nich bezproblemowo w czasie na realizację oferty.

![Wybór logowania tożsamości](media/active-directory-b2b-what-is-azure-ad-b2b/sign-in-identity-choice.png)

### <a name="delegate-to-application-and-group-owners"></a>Deleguj do aplikacji i właścicieli grupy 
Aplikacji i właścicieli grupy można dodać B2B użytkowników bezpośrednio do dowolnej aplikacji, która Cię interesuje, czy nie jest on aplikacji firmy Microsoft. Administratorzy mogą delegować uprawnienia do dodawania użytkowników B2B do innych niż administratorzy. Użytkownicy inni niż administratorzy mogą używać [Panel dostępu do aplikacji w usłudze Azure AD](https://myapps.microsoft.com) do dodawania użytkowników współpracy B2B do aplikacji lub grupy.

![panel dostępu](media/active-directory-b2b-what-is-azure-ad-b2b/access-panel.png)

![Dodawanie elementu członkowskiego](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a>Zasady autoryzacji ochrony zawartości w sieci firmowej

Zasady dostępu warunkowego, takich jak uwierzytelnianie wieloskładnikowe, można wymusić:
- Na poziomie dzierżawy
- Na poziomie aplikacji
- Dla określonych użytkowników dotyczące ochrony danych i aplikacji firmowych

![Dodawanie elementu członkowskiego](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="use-our-apis-and-sample-code-to-easily-build-applications-to-onboard"></a>Przy użyciu naszych interfejsów API, a przykładowy kod umożliwia łatwe tworzenie aplikacji do dołączenia
Przełącz z partnerami zewnętrznymi na statku w sposób dostosowany do potrzeb organizacji.

Przy użyciu [zaproszenia współpracy B2B interfejsów API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation), można dostosować komfort dołączania, w tym tworzenie portale Samoobsługowe dla rejestracji. Firma Microsoft udostępnia kodu do portal samoobsługowy [w serwisie Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

![portal rejestracji](media/active-directory-b2b-what-is-azure-ad-b2b/sign-up-portal.png)

Współpracy B2B usługi Azure AD możesz uzyskać pełną moc usługi Azure AD, aby chronić Twoje relacje z partnerami w taki sposób, aby znaleźć użytkownikom końcowym proste i intuicyjne. Dlatego dalej, Dołącz do tysięcy organizacji, które już używają B2B usługi Azure AD dla ich współpracy zewnętrznej!

## <a name="next-steps"></a>Następne kroki

* Administrator środowiska znajdują się w [portalu Azure](https://portal.azure.com).

* Informacje środowiska roboczego są dostępne w [panelu dostępu](https://myapps.microsoft.com).

* I jak zawsze Uzyskuj dostęp do zespołu produktu, opinii, dyskusji i sugestie za pośrednictwem naszego [społeczność techniczna Microsoft](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).

Zobacz nasze inne artykuły dotyczące współpracy B2B w usłudze Azure AD:

* [Jak Administratorzy usługi Azure Active Directory dodać użytkowników współpracy B2B?](active-directory-b2b-admin-add-users.md)
* [Jak pracownicy przetwarzający informacje dodać użytkowników współpracy B2B?](active-directory-b2b-iw-add-users.md)
* [Elementy współpracy B2B zaproszenie](active-directory-b2b-invitation-email.md)
* [Realizacji zaproszenia współpracy B2B](active-directory-b2b-redemption-experience.md)
* [Azure licencjonowania współpracy B2B usługi AD](active-directory-b2b-licensing.md)
* [Rozwiązywanie problemów z współpracy usługi Azure Active Directory B2B](active-directory-b2b-troubleshooting.md)
* [Azure współpracy B2B usługi Active Directory — często zadawane pytania (FAQ)](active-directory-b2b-faq.md)
* [Dostosowywanie i Azure Active Directory B2B współpracy interfejsu API](active-directory-b2b-api.md)
* [Usługa Multi-Factor Authentication dla użytkowników współpracy B2B](active-directory-b2b-mfa-instructions.md)
* [Dodawanie użytkowników współpracy B2B bez zaproszenia](active-directory-b2b-add-user-without-invite.md)
* [Użytkownik współpracy B2B inspekcji i raportowanie](active-directory-b2b-auditing-and-reporting.md)
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
