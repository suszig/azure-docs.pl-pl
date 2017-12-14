---
title: "Usługa Azure Active Directory dla deweloperów | Microsoft Docs"
description: "Ten artykuł zawiera omówienie logowania się na konta służbowe Microsoft przy użyciu usługi Azure Active Directory."
services: active-directory
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: c7375b91790cc7a089560bd823e99850e3b34fec
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-for-developers"></a>Usługa Azure Active Directory dla deweloperów
Azure Active Directory (Azure AD) to usługa zarządzania tożsamościami w chmurze, która umożliwia deweloperom bezpieczne logowanie użytkowników mających konta służbowe Microsoft. W tej dokumentacji pokazano, jak dodać obsługę usługi Azure AD do własnej aplikacji, używając standardowych w branży protokołów: OAuth i OpenID Connect.

| | |
| --- | --- |
|[Informacje podstawowe o uwierzytelnianiu](active-directory-authentication-scenarios.md) | Wprowadzenie do uwierzytelniania za pomocą usługi Azure AD. |
|[Typy aplikacji](active-directory-authentication-scenarios.md#application-types-and-scenarios) | Przegląd scenariuszy uwierzytelniania obsługiwanych przez usługę Azure AD. |                                
                                                                              
## <a name="get-started"></a>Rozpoczynanie pracy
Przedstawieni poniżej instalatorzy z przewodnikiem prowadzą użytkownika krok po kroku po korzystaniu z bibliotek uwierzytelniania firmy Microsoft w celu logowania użytkowników usługi Azure AD.

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Aplikacje mobilne i klasyczne](./media/active-directory-developers-guide/NativeApp_Icon.png)<br />Aplikacje mobilne i klasyczne</center> | [Omówienie](active-directory-authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](active-directory-devquickstarts-ios.md)<br /><br />[Android](active-directory-devquickstarts-android.md) | [.NET](active-directory-devquickstarts-dotnet.md)<br /><br />[Windows](active-directory-devquickstarts-windowsstore.md)<br /><br />[Xamarin](active-directory-devquickstarts-xamarin.md) | [Cordova](active-directory-devquickstarts-cordova.md)<br /><br />[OAuth 2.0](active-directory-protocols-oauth-code.md) |
| <center>![Aplikacje internetowe](./media/active-directory-developers-guide/Web_app.png)<br />Aplikacje internetowe</center> | [Omówienie](active-directory-authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](active-directory-devquickstarts-webapp-dotnet.md)<br /><br />[Java](active-directory-devquickstarts-webapp-java.md) | [Node.js](active-directory-devquickstarts-openidconnect-nodejs.md)<br /><br />[OpenID Connect 1.0](active-directory-protocols-openid-connect-code.md) |  |
| <center>![Aplikacje jednostronicowe](./media/active-directory-developers-guide/SPA.png)<br />Aplikacje jednostronicowe</center> | [Omówienie](active-directory-authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |  |
| <center>![Interfejsy API sieci Web](./media/active-directory-developers-guide/Web_API.png)<br />Interfejsy API sieci Web</center> | [Omówienie](active-directory-authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[Node.js](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Usługa-usługa](./media/active-directory-developers-guide/Service_App.png)<br />Usługa-usługa</center> | [Omówienie](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#server-or-daemon-application-to-web-api)<br /><br />[Poświadczenia klienta OAuth 2.0](active-directory-protocols-oauth-service-to-service.md) |  |

## <a name="how-to-guides"></a>Przewodniki z instrukcjami
W poniższych przewodnikach znajdziesz informacje o tym, jak wykonywać typowe zadania w usłudze Azure AD.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[Rejestracja aplikacji](active-directory-integrating-applications.md)           | Jak zarejestrować aplikację w usłudze Azure AD. |
|[Aplikacje wielodostępne](active-directory-devhowto-multi-tenant-overview.md)    | Jak zalogować dowolne konto służbowe Microsoft. |
|[Protokoły OAuth i OpenID Connect](active-directory-protocols-openid-connect-code.md)| Jak logować użytkowników i wywoływać interfejsy API sieci Web za pomocą protokołów uwierzytelniania firmy Microsoft. |
|[Dodatkowe przewodniki](active-directory-developers-guide-index.md#guides)        |  Lista dostępnych przewodników dotyczących usługi Azure AD.   |

## <a name="reference-topics"></a>Tematy informacyjne
Następujące artykuły zawierają szczegółowe informacje o interfejsach API, komunikatach protokołów i terminach używanych w usłudze Azure AD.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Biblioteki uwierzytelniania (ADAL)](active-directory-authentication-libraries.md)   | Omówienie bibliotek i zestawów SDK dostarczanych przez usługę Azure AD. |
| [Przykłady kodu](active-directory-code-samples.md)                                  | Lista wszystkich przykładów kodu usługi Azure AD. |
| [Słownik](active-directory-dev-glossary.md)                                      | Terminologia i definicje słów używanych w tej dokumentacji. |
| [Dodatkowe tematy informacyjne](active-directory-developers-guide-index.md#reference)| Lista dostępnych tematów informacyjnych usługi Azure AD.   |


> [!NOTE]
> Jeśli musisz logować konta osobiste Microsoft, warto rozważyć skorzystanie z [punktu końcowego usługi Azure AD w wersji 2.0](active-directory-appmodel-v2-overview.md). Punkt końcowy usługi Azure AD w wersji 2.0 stanowi ujednolicenie kont osobistych i kont służbowych Microsoft (z usługi Azure AD) do postaci jednego systemu uwierzytelniania.


[!INCLUDE  [Help and support](../../../includes/active-directory-develop-help-support-include.md)]