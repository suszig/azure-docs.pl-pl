---
title: "Usługa Azure Active Directory B2C: przegląd | Microsoft Docs"
description: "Tworzenie aplikacji dla użytkowników za pomocą usługi Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: saeeda
manager: krassk
editor: parja
ms.assetid: c465dbde-f800-4f2e-8814-0ff5f5dae610
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: saeeda
ms.translationtype: Human Translation
ms.sourcegitcommit: 532ff423ff53567b6ce40c0ea7ec09a689cee1e7
ms.openlocfilehash: f3c2760ec66c0292ebeb53d0acb5f9ee1df388ae
ms.contentlocale: pl-pl
ms.lasthandoff: 06/05/2017


---
# <a name="azure-ad-b2c-focus-on-your-app-let-us-worry-about-sign-up-and-sign-in"></a>Azure AD B2C: Skup się na swojej aplikacji — pozwól nam zająć się rejestrowaniem i logowaniem

Usługa Azure AD B2C to chmurowe rozwiązanie do zarządzania tożsamościami dla Twoich aplikacji sieci Web i aplikacji mobilnych. Stanowi globalną usługę o wysokiej dostępności, która może obsłużyć setki milionów tożsamości. Oparta na bezpiecznej platformie klasy korporacyjnej usługa Azure AD B2C chroni Twoje aplikacje, procesy biznesowe i użytkowników.

Wymagając minimalnej konfiguracji, usługa Azure AD B2C umożliwia Twojej aplikacji uwierzytelnianie następujących obiektów:

* **Konta społecznościowe** (np. Facebook, Google i LinkedIn)
* **Konta przedsiębiorstwa** (za pomocą standardowych protokołów uwierzytelniania OpenID Connect lub SAML)
* **Konta lokalne** (adres e-mail i hasło lub nazwa użytkownika i hasło)

## <a name="get-started"></a>Rozpoczęcie pracy

W pierwszej kolejności utwórz własną dzierżawę, wykonując kroki opisane w temacie [Tworzenie dzierżawy usługi Azure AD B2C](active-directory-b2c-get-started.md).

Następnie wybierz scenariusz tworzenia aplikacji:

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Aplikacje mobilne i klasyczne](../active-directory/develop/media/active-directory-developers-guide/NativeApp_Icon.png)<br />Aplikacje mobilne i klasyczne</center> | [Omówienie](active-directory-b2c-reference-oauth-code.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br /><br />[iOS](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal)<br /><br />[Android](https://github.com/Azure-Samples/active-directory-b2c-android-native-msal) | [.NET](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop)<br /><br />[Xamarin](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) |  |
| <center>![Aplikacje sieci Web](../active-directory/develop/media/active-directory-developers-guide/Web_app.png)<br />Aplikacje sieci Web</center> | [Omówienie](active-directory-b2c-reference-oidc.md)<br /><br />[ASP.NET](active-directory-b2c-devquickstarts-web-dotnet-susi.md)<br /><br />[ASP.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapp) | [Node.js](active-directory-b2c-devquickstarts-web-node.md) |  |
| <center>![Aplikacje jednostronicowe](../active-directory/develop/media/active-directory-developers-guide/SPA.png)<br />Aplikacje jednostronicowe</center> | [Omówienie](active-directory-b2c-reference-spa.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)<br /><br /> |  |  |
| <center>![Interfejsy API sieci Web](../active-directory/develop/media/active-directory-developers-guide/Web_API.png)<br />Interfejsy API sieci Web</center> | [ASP.NET](active-directory-b2c-devquickstarts-api-dotnet.md)<br /><br /> [ASP.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)<br /><br /> [Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) | [Wywoływanie interfejsu API sieci Web platformy .NET](active-directory-b2c-devquickstarts-web-api-dotnet.md) |

## <a name="whats-new"></a>Co nowego

Zaglądaj tu często, aby dowiadywać się o nadchodzących zmianach w usłudze Azure Active Directory B2C. Tweetujemy również o wszystkich aktualizacjach, używając konta @AzureAD.

* Oprócz „zasad wbudowanych” (ogólna dostępność) w publicznej wersji zapoznawczej jest teraz dostępna funkcja [„zasad niestandardowych”](active-directory-b2c-overview-custom.md).  Zasady niestandardowe są przeznaczone dla specjalistów ds. tożsamości, którzy potrzebują kontroli nad strukturą środowiska obsługi tożsamości.
* Funkcja [tokenu dostępu](https://azure.microsoft.com/en-us/blog/azure-ad-b2c-access-tokens-now-in-public-preview) jest teraz dostępna w publicznej wersji zapoznawczej.
* Ogłoszono [ogólną dostępność europejskich katalogów usługi Azure AD B2C](https://azure.microsoft.com/en-us/blog/azuread-b2c-ga-eu/).
* Zapoznaj się z naszą rosnącą biblioteką [przykładów kodu w usłudze GitHub](https://github.com/Azure-Samples?q=b2c)!

## <a name="how-to-articles"></a>Instrukcje

Informacje o sposobie korzystania z określonych funkcji usługi Azure Active Directory B2C:

* Skonfiguruj swoje konta [Facebook](active-directory-b2c-setup-fb-app.md), [Google+](active-directory-b2c-setup-goog-app.md), [konto Microsoft](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md) i [LinkedIn](active-directory-b2c-setup-li-app.md) do użycia w aplikacjach użytkownika.
* [Wykorzystaj niestandardowe atrybuty do zbierania informacji o użytkownikach](active-directory-b2c-reference-custom-attr.md).
* [Włącz funkcję Multi-Factor Authentication platformy Azure w aplikacjach użytkownika](active-directory-b2c-reference-mfa.md).
* [Skonfiguruj funkcję samodzielnego resetowania hasła przez użytkowników](active-directory-b2c-reference-sspr.md).
* [Dostosuj wygląd i obsługę funkcji rejestrowania i logowania oraz innych stron dla użytkownika](active-directory-b2c-reference-ui-customization.md) obsługiwanych przez usługę Azure Active Directory B2C.
* [Skorzystaj z interfejsu API Azure Active Directory programu Graph do programowego tworzenia, odczytywania, aktualizowania i usuwania użytkowników](active-directory-b2c-devquickstarts-graph-dotnet.md) w swojej dzierżawie usługi Azure Active Directory B2C.

## <a name="next-steps"></a>Następne kroki

Poniższe linki pozwalają szczegółowo eksplorować usługę:

* Zobacz [Informacje o cenach usługi Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
* Przejrzyj nasze [przykłady kodu](https://azure.microsoft.com/en-us/resources/samples/?service=active-directory&term=b2c) dla usługi Azure Active Directory B2C. 
* Uzyskaj pomoc na stronie Stack Overflow, korzystając z tagu [azure-ad-b2c](http://stackoverflow.com/questions/tagged/azure-ad-b2c).
* Przekazuj nam swoje przemyślenia za pomocą funkcji [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) — chcemy je poznać.
* Zapoznaj się z [dokumentacją protokołu usługi Azure AD B2C](active-directory-b2c-reference-protocols.md).
* Zapoznaj się z [dokumentacją tokenu usługi Azure AD B2C](active-directory-b2c-reference-tokens.md).
* Przeczytaj temat [Azure Active Directory B2C — często zadawane pytania](active-directory-b2c-faqs.md).
* [Żądania pomocy technicznej dotyczące plików dla usługi Azure Active Directory B2C](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Pobierz aktualizacje zabezpieczeń naszych produktów

Firma Microsoft zachęca do przekazywania powiadomień o występujących incydentach zabezpieczeń poprzez wizytę na [tej stronie](https://technet.microsoft.com/security/dd252948) i subskrybowanie Doradczych alertów zabezpieczeń.


