---
title: "Usługa Azure Active Directory B2C: przegląd | Microsoft Docs"
description: "Tworzenie aplikacji dla użytkowników za pomocą usługi Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: c465dbde-f800-4f2e-8814-0ff5f5dae610
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/24/2016
ms.author: swkrish
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 217ffc63e07d66de522accc42c246125d01713c8


---
# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications"></a>Usługa Azure Active Directory B2C: rejestrowanie i logowanie użytkowników w aplikacjach
Usługa Azure Active Directory B2C to kompleksowe rozwiązanie przeznaczone do zarządzania tożsamością w chmurze w aplikacjach internetowych i mobilnych użytkownika. Stanowi globalną usługę o wysokiej dostępności, która może obsłużyć setki milionów tożsamości użytkowników. Oparta na bezpiecznej platformie klasy korporacyjnej usługa Azure Active Directory B2C chroni Twoje aplikacje, procesy biznesowe i użytkowników.

W przeszłości deweloperzy aplikacji, którzy chcieli wprowadzić funkcje tworzenia kont i logowania użytkowników w swoich aplikacjach, musieli napisać własny kod. Musieli korzystać z lokalnych baz danych lub systemów do przechowywania nazw i haseł użytkowników. Usługa Azure Active Directory B2C oferuje deweloperom lepszy sposób integracji funkcji zarządzania tożsamością użytkowników z aplikacjami za pomocą bezpiecznej, spełniającej standardy platformy i bogatego zestawu rozszerzalnych zasad. W przypadku korzystania z usługi Azure Active Directory B2C użytkownicy mogą rejestrować się w aplikacjach za pomocą istniejących kont społecznościowych (Facebook, Google, Amazon, LinkedIn) lub tworząc nowe poświadczenia (adres e-mail i hasło lub nazwa użytkownika i hasło); te drugie nazywamy „kontami lokalnymi”.

## <a name="get-started"></a>Rozpoczynanie pracy
Aby utworzyć aplikację umożliwiającą tworzenie kont i logowanie użytkowników, musisz najpierw zarejestrować aplikację za pomocą dzierżawy usługi Azure Active Directory B2C. Aby utworzyć własną dzierżawę, wykonaj kroki opisane w temacie [Tworzenie dzierżawy usługi Azure AD B2C](active-directory-b2c-get-started.md).

Aplikacja korzystająca z usługi Azure Active Directory B2C może bezpośrednio wysyłać wiadomości protokołu za pomocą protokołu [OAuth 2.0 lub Open ID Connect](active-directory-b2c-reference-protocols.md) albo skorzystać z bibliotek, które wykonają tę pracę za nią. Wybierz preferowaną platformę z poniższej tabeli i rozpocznij pracę.

[!INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## <a name="whats-new"></a>Co nowego
Zaglądaj tu często, aby dowiadywać się o nadchodzących zmianach w usłudze Azure Active Directory B2C. Będziemy również tweetować o wszystkich aktualizacjach, korzystając z @AzureAD.

* Dowiedz się więcej o naszym [środowisku rozszerzalnych zasad](active-directory-b2c-reference-policies.md) i rodzajach zasad tworzonych i używanych w aplikacjach użytkownika.
* Dodaj do zakładek [blog na temat usługi](https://blogs.msdn.microsoft.com/azureadb2c/), aby otrzymywać powiadomienia dotyczące niewielkich problemów z usługą, aktualizacji, stanu i środków bezpieczeństwa. Śledź też [pulpit nawigacyjny stanu platformy Azure](https://azure.microsoft.com/status/).
* Bieżące [ograniczenia dotyczące usługi](active-directory-b2c-limitations.md).
* Oraz [przykładowy kod](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c) z użyciem usługi Azure AD B2C i platformy ASP.NET Core.

## <a name="howto-articles"></a>Instrukcje
Informacje o sposobie korzystania z określonych funkcji usługi Azure Active Directory B2C:

* Skonfiguruj swoje konta [Facebook](active-directory-b2c-setup-fb-app.md), [Google+](active-directory-b2c-setup-goog-app.md), [konto Microsoft](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md) i [LinkedIn](active-directory-b2c-setup-li-app.md) do użycia w aplikacjach użytkownika.
* [Wykorzystaj niestandardowe atrybuty do zbierania informacji o użytkownikach](active-directory-b2c-reference-custom-attr.md).
* [Włącz funkcję Multi-Factor Authentication platformy Azure w aplikacjach użytkownika](active-directory-b2c-reference-mfa.md).
* [Skonfiguruj funkcję samodzielnego resetowania hasła przez użytkowników](active-directory-b2c-reference-sspr.md).
* [Dostosuj wygląd i obsługę funkcji tworzenia kont i logowania oraz innych stron dla użytkownika](active-directory-b2c-reference-ui-customization.md) obsługiwanych przez usługę Azure Active Directory B2C.
* [Skorzystaj z interfejsu API Azure Active Directory programu Graph do programowego tworzenia, odczytywania, aktualizowania i usuwania użytkowników](active-directory-b2c-devquickstarts-graph-dotnet.md) w swojej dzierżawie usługi Azure Active Directory B2C.

## <a name="next-steps"></a>Następne kroki
Poniższe linki przydadzą się podczas szczegółowego eksplorowania usługi:

* Zobacz [Informacje o cenach usługi Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
* Uzyskaj pomoc na stronie Stack Overflow, korzystając ze znaczników [azure-active-directory](http://stackoverflow.com/questions/tagged/azure-active-directory) i [adal](http://stackoverflow.com/questions/tagged/adal).
* Przekaż nam swoje pomysły za pomocą funkcji [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/) — chcemy się o nich dowiedzieć. W tytule wpisu użyj frazy „AzureADB2C:”, co pozwoli nam na odnalezienie go.
* Zapoznaj się z [dokumentacją protokołu usługi Azure AD B2C](active-directory-b2c-reference-protocols.md).
* Zapoznaj się z [dokumentacją tokenu usługi Azure AD B2C](active-directory-b2c-reference-tokens.md).
* Przeczytaj temat [Azure Active Directory B2C — często zadawane pytania](active-directory-b2c-faqs.md).
* [Żądania pomocy technicznej dotyczące plików dla usługi Azure Active Directory B2C](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Pobierz aktualizacje zabezpieczeń naszych produktów
Firma Microsoft zachęca do przekazywania powiadomień o występujących incydentach zabezpieczeń poprzez wizytę na [tej stronie](https://technet.microsoft.com/security/dd252948) i subskrybowanie Doradczych alertów zabezpieczeń.




<!--HONumber=Nov16_HO2-->


